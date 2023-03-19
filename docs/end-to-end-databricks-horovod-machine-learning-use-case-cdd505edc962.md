# 端到端数据块-Horovod 用例

> 原文：<https://towardsdatascience.com/end-to-end-databricks-horovod-machine-learning-use-case-cdd505edc962?source=collection_archive---------24----------------------->

![](img/a578d3967246096c854f19aa939db38e.png)

[https://unsplash.com/photos/1lfI7wkGWZ4?utm_source=unsplash&UTM _ medium = referral&UTM _ content = creditShareLink](https://unsplash.com/photos/1lfI7wkGWZ4?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

当开始一个大的机器学习项目时，有许多平台可供选择。本文将给出在数据块上使用 Tensorflow & Horovod 训练神经网络的指南。

# 背景

在 [SureWx](https://surewx.com/solutions.php) (发音为“确定的天气”)，我们已经为航空应用开发了定制的天气预报。这符合我们为航空公司提供的从登机口到起飞的数据驱动产品的 T4 套件。在冬季，操作决策变得复杂，因为降水要求在飞机离开地面之前采取额外的清洁和安全措施。更准确的天气预报有助于确保飞机按时起飞。这就是为什么我们一直在使用来自全球传感器网络的数据结合雷达数据来训练神经网络，以预测短期天气。这篇文章包含了我们在这个过程中学到的与模型的架构和训练相关的最重要的发现。

# 1.将数据保存在 TFRecord 批中

数据从您的 S3 存储桶中读取，该存储桶安装在 spark 集群上的/dbfs 中。只要您的 EC2 实例与您的 S3 时段在同一个区域，您只需为到 S3 的 GET 请求付费(但不包括数据传输)。如果您的数据没有进行批处理，GET 请求会很快增加。

无阴影示例:

0.0004 美元/ 1000 个请求* 200，000 个文件/时期* 1000 个时期= 80 美元

每个模型 80 美元的 S3 成本是一笔巨大的意外成本，在某些情况下甚至可能超过 GPU 成本。如果您已经为定型生成了 tf.data.dataset，则可以通过将数据写入 TFRecords 来轻松地对数据进行预批处理。您可以在这里找到编写和解析 TFRecords 的指南:

<https://www.tensorflow.org/tutorials/load_data/tfrecord#writing_a_tfrecord_file>  

# **2。解析时交错**

当您进行解析时，确保使用并行化的交错来获得最佳性能。此外，为了在添加更多节点时有效地扩大培训规模，tf.data.Dataset 需要在解析文件之前应用分片。这样，每个碎片只将一小部分文件读入内存。

```
import glob
import tensorflow as tfdef parse_fn(serialized): features = \
  {
  'radar': tf.io.FixedLenFeature([], tf.string),
  'stationd_data': tf.io.FixedLenFeature([], tf.string),
  'label': tf.io.FixedLenFeature([], tf.string)
  } # Parse the serialized data so we get a dict with our data.
  parsed_example = tf.io.parse_single_example(serialized=serialized, features=features) # Get the image as raw bytes.
  radar_raw = parsed_example['radar']
  station_data_raw = parsed_example['station_data']
  label_raw = parsed_example['label'] # Decode the raw bytes so it becomes a tensor with type.
  radar = tf.io.decode_raw(radar_raw, tf.float32)
  radar = tf.reshape(radar, radar_shape) station_data = tf.io.decode_raw(station_data_raw, tf.float64)
  station_data = tf.reshape(station_data, station_data_shape) label = tf.io.decode_raw(label_raw, tf.float32)
  label = tf.reshape(label, label_shape) return (radar, station_data), labelfiles = glob.glob(f'{data_path}/*.tfrecord')files_ds = tf.data.Dataset.from_tensor_slices(files)
files_ds = files_ds.shard(self.shards, shard_index)ds = files_ds.interleave(lambda x: tf.data.TFRecordDataset(x)).map(lambda x: parse_fn(x), num_parallel_calls = tf.data.experimental.AUTOTUNE)
```

# 3.选择 S3 路径来优化 S3 的分片

批量保存数据的另一个原因是为了避免 S3 节流和网络错误。在 S3，每个前缀每秒钟最多只能有 5500 个请求。这可能看起来很多——但是如果你同时训练许多模型，你可以达到这个极限。用优化 S3 分片的文件路径保存预先批处理的 TFRecords 是一种很好的做法。请参见下面的指南。

<https://aws.amazon.com/premiumsupport/knowledge-center/s3-object-key-naming-pattern/>  

# 4.错误处理

训练可能会因为各种原因而中断:错误的数据、训练中的不稳定性、回调中的错误、点实例被取走…为了不浪费资源，您希望您的模型进行检查点检查和恢复。

```
keras.callbacks.ModelCheckpoint(f'{checkpoint_path}_{time}', save_weights_only = False, monitor='val_loss', mode='min', save_best_only=True))keras.callbacks.ModelCheckpoint(checkpoint_path, save_weights_only = False, save_best_only=False, save_freq='epoch', period=3))
```

第一次回调是保存培训课程中产生的最佳模型。第二个回调是每隔几个时期保存一次模型，如果训练中断，可以启动一个新的作业并加载模型。如果没有这两次复试，你会丢掉自上一个最佳模特以来的所有训练。两次回调很容易，何必浪费 GPU 时间？

# 5.多输入混合数据

处理混合数据(例如:图像和时间序列数据)的 ETL 可能很难设计。在我们的案例中，我们将雷达图像序列与来自传感器的时间序列数据相结合。我们使用 Spark 构建数据框架，将我们的图像与传感器数据对齐。例如，假设我们想要将图像与最近 30 分钟的传感器数据进行匹配。我们对时间序列数据应用窗口函数，将所有时间序列数据保存到单个字节数组中。

```
import pandas as pd
import numpy as np
from pyspark.sql.types import BinaryType
from pyspark.sql.functions import pandas_udfdf # sensor time series data. # Stack the sensor data columns together and serialize
[@pandas_udf](http://twitter.com/pandas_udf)(BinaryType())
def serialize_udf(s1: pd.Series, s2: pd.Series) -> bytearray:
    """Takes multiple pandas series, stacks them, and converts the resulting array to bytes"""
    data = np.dstack((s1.to_numpy(), s2.to_numpy()))
    data = data.astype(np.double)
    data = data[0,:,:]return data.tobytes()w = Window.orderBy(f.col("time").cast('long')).rangeBetween(-30*60, 0)df = df.withColumn('sensor_data', serialize_udf(*config['sensor_cols']).over(w))pdf = df.toPandas()
```

然后，在 tf.data.dataset 中读取时间序列数据之前，需要应用一个解析器。

```
sensor_data_bytes_list = list(pdf['sensor_data'].to_numpy())
sensor_data_list = list(map(lambda blob: np.frombuffer(blob, np.double), sensor_data_bytes_list))
sensor_data_array = np.concatenate(sensor_data_list)
ds = tf.data.Dataset.from_tensor_slices(sensor_data_array)
```

# 逮到你了

1.  通过 HorovodRunner 在 spark 上运行 Horovod，你需要支付 1 个不能用于计算的 GPU 实例——这是 Spark 驱动程序的开销，Horovod 不能用于训练。
2.  Databricks 上没有 Tensorboard profiler 扩展，如果您是第一次设计 tensorflow 数据管道，最好先在单台 GPU 服务器上进行优化。
3.  Databricks 上的 Horovod 没有弹性——当 spot 实例被删除时，训练就会停止。如果你打算采用现货价格，你应该预料到培训会有很多中断。
4.  获取 g4dn EC2 spot 实例相当困难(截至 2021 年)。Databricks 无法在 g4da 实例上运行，其他 EC2 GPU 实例也没有这么好的价值。

# 结论

Databricks 创建了一个很好的套件，将数据、ETL 和分布式培训都放在同一个地方。希望这些小技巧能让你的开发更轻松！祝你好运。