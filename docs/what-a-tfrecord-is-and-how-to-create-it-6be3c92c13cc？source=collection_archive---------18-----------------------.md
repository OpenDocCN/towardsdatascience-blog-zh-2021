# 什么是 TFRecord 以及如何创建它

> 原文：<https://towardsdatascience.com/what-a-tfrecord-is-and-how-to-create-it-6be3c92c13cc?source=collection_archive---------18----------------------->

## 如何使用 TFRecord 格式有效地训练神经网络

![](img/804967fcc41d241d166e246515b10151.png)

简·安东宁·科拉尔在 [Unsplash](https://unsplash.com/s/photos/files?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

TensorFlow 是当今最流行的深度学习框架之一。有些人相信它，有些人认为它是一个伟大但臃肿的工具，一个承载着沉重遗留代码负担的工具。

就个人而言，我更喜欢使用 PyTorch，但在我看来，每个机器学习(ML)研究人员或工程师都应该知道如何找到进入 TensorFlow 知识库的方法。该领域有很多创新，几乎一半是用 TensorFlow 表达的。

然而，在很大程度上，TensorFlow 是一个固执己见的框架。它最出名的僵硬 API 之一与数据处理和加载有关。

在这个故事中，我们转向基础，看看 TFRecords，它们是什么，如何生成它们，以及如何有效地使用它们来训练神经网络。

> [Learning Rate](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=tfrecord) 是一份时事通讯，面向那些对 AI 和 MLOps 世界感到好奇的人。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=tfrecord)！

# TFRecords:什么和为什么

TFRecord 格式是 Tensorflow 自己的二进制存储格式。它使用[协议缓冲区](https://developers.google.com/protocol-buffers/)，这是一个跨平台、跨语言的库，用于结构化数据的高效序列化。使用 TFRecord 格式有许多优点:

*   **效率**:TF record 格式的数据可以比原始数据占用更少的空间。
*   **快速 I/O** : TensorFlow 可以通过并行 I/O 操作读取 TFRecord 格式的数据。当你使用 GPU 或 TPU 设备时，这非常有用。
*   **自包含文件** : TFRecords 是自包含的，这意味着您可以在一个文件中拥有您需要的一切，数据及其元数据，并且格式允许您使用对您来说重要的任何内容。

# TFRecords:如何

我们看到使用 TFRecords 代替原始数据格式有几个优点。然而，天下没有免费的午餐。您必须完成这项工作，并将原始数据转换为 TFRecords。

为了实现这一点，您需要主要使用两个类:

*   `[tf.train.Example](https://www.tensorflow.org/api_docs/python/tf/train/Example)`或`[tf.train.SequenceExample](https://www.tensorflow.org/api_docs/python/tf/train/SequenceExample)`取决于您的数据
*   `[tf.train.Feature](https://www.tensorflow.org/api_docs/python/tf/train/Feature)`

此外，您需要以下列格式之一来表示数据集的要素:

*   `[tf.train.BytesList](https://www.tensorflow.org/api_docs/python/tf/train/BytesList)`
*   `[tf.train.FloatList](https://www.tensorflow.org/api_docs/python/tf/train/FloatList)`
*   `[tf.train.Int64List](https://www.tensorflow.org/api_docs/python/tf/train/Int64List)`

为此，让我们看看如何使用一个简单的[示例](https://keras.io/examples/keras_recipes/creating_tfrecords/)来实现这一点，由 [Dimitre Oliveira](https://www.linkedin.com/in/dimitre-oliveira-7a1a0113a/) 提供:我们将把 [COCO2017](https://cocodataset.org/#home) 数据集转换成 TFRecords。

# 简单的例子

COCO2017 数据集有两个子集:图像和注释元数据。它用于为执行对象检测、关键点检测、全景分割或解决密集任务的模型建立基线。

图像以 JPG 格式存储，注释数据存储为普通的 JSON 文件，包含以下属性:

```
id: int,
image_id: int,
category_id: int,
segmentation: RLE or [polygon], object segmentation mask
bbox: [x,y,width,height], object bounding box coordinates
area: float, area of the bounding box
iscrowd: 0 or 1, is single object or a collection
```

例如，特定图像的元数据可能是这样的:

```
{
   "area": 367.89710000000014,
   "bbox": [
      265.67,
      222.31,
      26.48,
      14.71
   ],
   "category_id": 72,
   "id": 34096,
   "image_id": 525083,
   "iscrowd": 0,
   "segmentation": [
      [
         267.51,
         222.31,
         292.15,
         222.31,
         291.05,
         237.02,
         265.67,
         237.02
      ]
   ]
}
```

我们可以看到，要转换这个数据集，我们需要使用 TFRecord API 提供的几乎所有类型。我们需要转换整数、浮点数、浮点数列表和图像。让我们把手弄脏吧。

## 将数据转换为 TFRecords

假设我们已经将数据加载到一个名为`annotations`的变量中，让我们指定每个 TFRecord 上的样本数，以及我们将创建多少个这样的文件:

现在，我们准备创建我们的助手函数，将每个特性转换成其等效的`tf.train.Feature`格式:

接下来，让我们再添加两个函数:一个使用上面的实用程序创建一个`tf.train.Example`对象，另一个解析它:

现在，剩下的就是读取数据并将它的点转换成 TFRecords:

在您的工作区中，您应该准备好了九个`.tfrec`文件。

## 训练简单的分类器

现在我们被读取来训练任何我们想要的模型，使用我们创建的`.tfrec`文件和 Keras。

首先，让我们创建一个助手函数，它将从我们之前创建的 TFrecords 生成数据集:

现在，我们准备好享受我们的工作成果，并训练一个简单的 Keras 分类器:

仅此而已！要端到端地运行整个示例，您可以使用由[迪米特里·奥利维拉](https://www.linkedin.com/in/dimitre-oliveira-7a1a0113a/)在 colab 上的这个优秀的[笔记本](https://colab.research.google.com/github/keras-team/keras-io/blob/master/examples/keras_recipes/ipynb/creating_tfrecords.ipynb)。

# 结论

TensorFlow 是当今最流行的深度学习框架之一。每个机器学习(ML)研究人员或工程师都应该知道如何在 TensorFlow 存储库中导航。这个领域有很多创新，很多都是用 TensorFlow 表达的。

在这个故事中，我们看到了 TFRecord 格式，为什么应该使用它，以及如何使用。然后，我们使用了 Dimitre Oliveira 提供的一个极好的例子来练习我们的知识。

下次使用 Keras 训练神经网络时，考虑利用 TFRecords 的能力，尤其是如果您计划使用 TPUs 的话！

# 关于作者

我叫 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=tfrecord) ，我是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。