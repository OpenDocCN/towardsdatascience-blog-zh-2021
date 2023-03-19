# 如何用 Ray v1 更快的训练时间序列预测？第二部分，共三部分。

> 原文：<https://towardsdatascience.com/how-to-train-time-series-forecasting-faster-using-ray-part-2-of-2-aacba89ca49a?source=collection_archive---------13----------------------->

## 时间序列预测使用谷歌的时间融合变压器 LSTM 版的 RNN 与 PyTorch 预测和火炬闪电

![](img/309ddb02417f3423b52a04327a9b364d.png)

显示[纽约市黄色出租车](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)的乘坐量，以及 1 周每小时的预测。根据验证数据集，蓝色=观察到的，橙色=预测到的。由 Pytorch forecasting 实现的使用 Google 的时间融合转换器算法生成的预测，并由 [Ray](https://www.ray.io/) 并行化以实现更快的运行时间，无论是在笔记本电脑上还是在任何云上。图片作者。

在第 1 部分中，我的[前一篇博客](/scaling-time-series-forecasting-with-ray-arima-and-prophet-e6c856e605ee)解释了当每个模型都被独立训练时，如何应用[令人尴尬的并行](https://en.wikipedia.org/wiki/Embarrassingly_parallel#:~:text=In%20parallel%20computing%2C%20an%20embarrassingly,a%20number%20of%20parallel%20tasks.)模式来加速预测，例如使用传统的预测算法 ARIMA、预言者和神经预言者。数据、训练和推理由 Ray 引擎分布在本地笔记本电脑内核上。这个概念类似于多处理池，除了 Ray 可以处理分布更复杂的类和函数。与多处理不同，完全相同的代码也可以在任何云中的任何集群上并行运行。

这篇文章将解释当训练一个大型全球模型以预测许多目标时间序列时，如何使用 Ray 来加速深度学习预测。为什么要这么做？嗯，通常情况下，一个公司想要预测的东西是相互关联的，比如体育用品、相同品牌和颜色的洗衣机和烘干机、经常一起购买的超市物品等等。然后，每个目标时间序列都被用作同一个模型的输入，每个目标时间序列都得到不同的输出。

为全局深度学习模型的分布式运行时并行化代码需要分布式数据并行和模型并行。这需要分布式计算工作者之间的协作来分割数据，在每个具有其自己的数据片的工作者之间共享梯度，并将梯度组合到单个全局模型中。Ray 处理数据和模型并行性，同时为开发人员保留一个简单的 API。此外，Ray 可以并行训练和推断深度学习模型，分布在单个笔记本电脑的内核或任何云中的计算节点上。

本博客分为以下几个主题:

*   **介绍用于预测的深度学习人工智能算法**
*   **在 Pytorch 预测中使用 Google 的时态融合转换器(使用 py torch Lightning API)**
*   **如何使用 Ray 加速模型训练和推理**
*   **如何使用 Anyscale 加速任何云中的模型训练和推理**

# 介绍用于预测的深度学习人工智能算法

![](img/ed8f99dc83d654fac0c7d0850da9023b.png)

向左。RNN 的高级视图和展开的时间步骤。图片[来源](https://www.researchgate.net/figure/The-standard-RNN-and-unfolded-RNN_fig1_318332317)。中间。典型的 ANN 构造块，由 1 个输入层、2 个隐藏的密集层和 1 个输出层组成。图片[来源](https://www.researchgate.net/figure/An-Artificial-Neural-Network-ANN-with-two-hidden-layers-and-six-nodes-in-each-hidden_fig1_335855384)。没错。示例 GRN 由 2 个致密层组成，具有 2 个激活函数(ELU 指数线性单位和 GLU 门控线性单位)，以及脱落。图片来自时间融合转换器[论文](https://arxiv.org/pdf/1912.09363.pdf)。

**递归神经网络** ( **RNN** )是一种常用于时间序列的神经网络，因为它按顺序处理数据。RNN 由一系列 ANN(人工神经网络)组成，通常每个时间步一个 ANN。RNN 架构允许顺序输入和顺序输出。每个 ANN 构件本身是一组神经元，分为输入层、隐藏层和输出层，其中每个神经元与其他神经元相连，每个连接具有可训练的权重。rnn 首先用于文本翻译。

以下是一些相关的概念术语。

**LSTM(长短期记忆)**是一种递归神经网络架构，旨在克服消失梯度问题(过去的事情可能接近 0 值权重)。LSTM 有 3 个记忆门，它们一起允许一个网络记住和忘记。

**GRN** 或门控剩余网络可以代替基本的 ANN 构建模块。具体包括:2 个致密层和 2 个激活函数(ELU 指数线性单元和谷氨酸门控线性单元)。这使得网络能够理解哪些输入转换是简单的，哪些需要更复杂的建模，以及哪些需要完全跳过。

**编码器-解码器模型**是一种 RNN，其中数据(训练数据)的输入序列可以与输出序列(验证或测试数据，也称为预测范围)具有不同的长度。位置编码被添加到输入嵌入中，以指示输入相对于整个时间序列的位置。

**自我注意机制**是为解决 LSTMs 的远程依赖问题(由于 LSTM 的遗忘门，重要信息可能丢失)而开发的一种进化。通过增加一个变压器，某些输入可以得到更多的“关注”，通过 RNN 网络进行前馈。在每个时间步，可学习的权重被计算为 **Q** 查询(具有特定的输入向量 w.r.t .其他输入)、 **K** ey(也包含该查询的输入嵌入)和 **V** 值(通常通过 Q，K 学习的权重的点积计算的输出向量)的函数。输出通过 RNN 网络进行前馈。由于 Q、K 都是从相同的输入中计算出来的，而这些输入又被应用于相同的输入，这个过程被称为“自我注意”。

**多头注意力**在每个时间步使用多个 Q，K 变换。纯自我关注使用每个时间步的所有历史数据。例如，如果 h=4 个注意头，则输入数据被分成 4 个块，然后使用 Q，K 矩阵将自我注意应用于每个块，以获得 4 个不同的 V 得分向量。这意味着单个输入被投射到 4 个不同的“表示子空间”上，并且通过 RNN 网络进行前馈。结果是更加细致入微的自我关注。从分布式计算的角度来看，这是理想的，因为来自多头注意力的每个块 h 可以在单独的处理器或工作器上异步运行。

**回测。**训练和验证数据被分割成滑动窗口的批次(每一批次是在未来移动 1 个值的前一批次)。这种技术被称为“回溯测试”，因为你不能像往常一样随机抽取 80/20 的训练/测试样本。顺序数据顺序必须保持不变。时间序列通常采用 context_length 大小的数据窗口进行训练，然后采用不同的 prediction_length 大小的窗口进行验证。

# 在 Pytorch 预测中使用 Google 的时态融合转换器实现的示例

本教程中使用的数据集是 8 个月的历史[纽约市黄色出租车](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)乘坐量。

我们的数据编码对象将是一个使用回溯测试技术重复折叠顺序数据的生成器。为了处理去趋势化，我们将使用 [PyTorch Forecasting 的](https://pytorch-forecasting.readthedocs.io/en/latest/api/pytorch_forecasting.data.encoders.GroupNormalizer.html#pytorch_forecasting.data.encoders.GroupNormalizer)组规格化器，或每个 item_id 的批量规格化。每批分为 63 小时的训练输入和 168 小时或 1 周的预测目标。也就是说，使用 63/168 窗口长度对数据进行训练/有效采样，以保持数据的顺序完整。

网络设计将是 LSTM 版的 RNN，具有 GRN 构建块、编码器-解码器和多头注意力。我们将使用 PyTorch Forecasting 对谷歌的[时间融合转换器](https://github.com/google-research/google-research/tree/master/tft)的实现。PyTorch Forecasting 是为 [PyTorch Lightning](https://www.pytorchlightning.ai/) 开发的一套方便的 API。PyTorch Lightning 又是一套基于 [PyTorch](https://pytorch.org/) 之上的便利 API。这与 Keras 是 TensorFlow 之上的一组便利 API 的概念类似。

演示的代码是 github 上的[。](https://github.com/christy/AnyscaleDemos/blob/main/forecasting_demos/pytorch_forecasting_ray_local.ipynb)

# 示例如何使用 Ray 加速模型训练和推理

![](img/9a7a34d40f04f81b42b0aa58e21ebf56.png)

[Ray 是加州大学伯克利分校 RISELab 开发的开源库](https://www.ray.io/)，该校还开发了 Apache Spark。Ray 使得并行化和分发 Python 代码变得很容易。代码可以在任何类型的内核上运行:a)你自己的笔记本电脑内核，AWS、GCP 或任何公共云中的集群。

这篇文章的其余部分假设你已经定义了一个 PyTorch 闪电模型，无论是通过普通的 PyTorch 闪电还是通过 PyTorch 预测。**下面用粗体显示了您需要修改的代码部分，以使它能够在 Ray 上运行。**

**第一步。安装并导入**[**Ray**](https://docs.ray.io/en/latest/installation.html)**[**Ray 插件**](https://github.com/ray-project/ray_lightning) **为 PyTorch Lightning，以及**[**any scale**](https://docs.anyscale.com/get-started)**。确保您的 PyTorch Lightning 版本为 1.4。****

```
# Install these libraries in your conda environment  
conda install pytorch  
**pip install pytorch_lightning==1.4**   #required version for ray  
**pip install git+https://github.com/jdb78/pytorch-forecasting@maintenance/pip-install**  #used at time of writing this blog, check for updates  
**pip install ray  
pip install anyscale**  
**pip install ray_lightning**# Import these libraries in your .py or .ipynb code     
import torch   
import pytorch_lightning as pl    
import pytorch_forecasting as ptf   
**import ray**  
**from ray_lightning import RayPlugin**# PyTorch visualization uses Tensorboard
import tensorflow as tf #Tensorflow
import tensorboard as tb  #Tensorboard
#compatibility for PyTorch
tf.io.gfile = tb.compat.tensorflow_stub.io.gfile
```

****第二步。根据笔记本电脑上的内核数量初始化 Ray** (这是默认行为)。我的有 8 个内核。**

```
# initialize ray, detects and uses all available CPU by default
**ray.init()**
```

****第三步。初始化 Ray Lightning 插件**，也是为了你笔记本电脑上的内核数量。**

```
# initialize the Ray Lightning plugin **plugin = \      
   RayPlugin(           
      num_workers=8,  #fixed num CPU        
      num_cpus_per_worker=1,           
      use_gpu=False,  #True or False    
      find_unused_parameters=False, #skip warnings    
   )**
```

****步骤四。像往常一样，将您的数据转换为 PyTorch 张量，并定义 PyTorch 预测数据加载器。PyTorch 预测数据加载器 API 方便地将张量自动折叠到训练/测试回溯测试窗口中。****

**接下来，修改 PyTorch 闪电训练器来使用射线插件。在下面添加`**plugins=[ray_plugin]**`参数。**

**注意:示例数据与代码位于同一个 github repo 中。数据已经汇总到纽约每个地点的每小时出租车乘坐次数中。**

```
# read data into pandas dataframe
filename = "data/clean_taxi_hourly.parquet"
df = pd.read_parquet(filename)# keep only certain columns
df = df[["time_idx", "pulocationid", "day_hour",
         "trip_quantity", "mean_item_loc_weekday",
         "binned_max_item"]].copy()# convert data to PyTorch tensors and PyTorch Forecasting loaders 
# PyTorch Forecasting folds tensors into backtest windows
train_dataset, train_loader, val_loader = \
     convert_pandas_pytorch_timeseriesdata(df)# define the pytorch lightning trainer
trainer = pl.Trainer(      
     max_epochs=EPOCHS,      
     gpus=NUM_GPU,      
     gradient_clip_val=0.1,        
     limit_train_batches=30,       
     callbacks=[lr_logger, 
                early_stop_callback],      
     # how often to log, default=50      
     logger=logger,      
     # To go back to regular python - just comment out below      
     # Plugin allows Ray engine to distribute objects     
     plugins=[**ray_plugin**] 
     )
```

**最后，像往常一样定义一个 PyTorch 闪电(或预测)模型。并像往常一样拟合模型。**

```
# define a pytorch forecasting model
model = ptf.models.TemporalFusionTransformer.from_dataset(   
             train_dataset,      
             learning_rate=LR,      
             hidden_size=HIDDEN_SIZE,      
             attention_head_size=ATTENTION_HEAD_SIZE,      
             dropout=DROPOUT,    
             hidden_continuous_size=HIDDEN_CONTINUOUS_SIZE,   
             loss=ptf.metrics.QuantileLoss(),      
             log_interval=10,      
             reduce_on_plateau_patience=4, 
             )# fit the model on training data
trainer.fit(
     model,      
     train_dataloaders=train_loader,         
     val_dataloaders=val_loader, 
)# get best model from the trainer
best_model_path = trainer.checkpoint_callback.best_model_path
best_model = \
ptf.models.TemporalFusionTransformer.load_from_checkpoint(
     best_model_path
)
```

**就是这样！现在您的 PyTorch Lightning 模型将分布式运行。在幕后，Ray Lightning 插件 API 和 Ray 一起自动分发数据和模型。输入数据被自动完全分片，数据分片和训练函数被放置在每个并行工作器上，在工作器之间共享梯度，产生一个全局模型，并且结果模型作为请求的类型(PyTorch Lightning 或 PyTorch Forecasting 模型类型)被返回。**

**![](img/68731d0b2b25b4e8586b423f07ac23c6.png)**

**Ray 在幕后将全局模型的学习分布到 N 个计算节点上。输入数据被完全分片，每个节点上的每个工人得到相同的训练函数，梯度在并行工人之间共享，并且产生一个全局模型。图片作者。**

**以前，我试图在我的笔记本电脑上训练这个模型，但几个小时后中断了运行，因为第一个时期还没有结束。用 Ray 分发代码后，同样的代码运行大约 1 小时。**

**这些小的调整使得在一个相当小的计算资源(我的笔记本电脑)上在大约 1 小时内训练一个非常精确的 DL 全球预测模型成为可能。**

**Ray 的另一个好处是，现在代码可以在我的笔记本电脑上并行运行，我可以使用 Anyscale 在任何云上运行相同的代码，接下来我将展示这一点。**

**![](img/8006f013730bc9727a77d54ffe47626d.png)**

**在 8 核笔记本电脑上运行的模型训练输出。iPython %% time 输出显示，训练一个非常准确的预测大约需要 1 个小时。**

**![](img/2ef460d180745efde795c1e8e8c73839.png)**

**在 8 核笔记本电脑上运行的模型训练输出。使用对 1 周延迟验证数据的预测计算精确度。**

# **如何使用 Anyscale 在任何云中加速模型训练和推理**

**接下来，为了更快地训练或进行推理，您可能希望在更大的实例或更大的集群上的云上运行相同的代码。为了在云上使用完全相同的射线代码，(AWS，GCP，…)，你需要使用[射线开源集群](https://docs.ray.io/en/latest/cluster/index.html)或 [Anyscale](https://www.anyscale.com/) 来简化任何云设置。**

**使用 Anyscale，您可以选择 a)在集群配置上进行 pip 安装和 github 克隆，或者 b)在运行时进行。更多信息见[集群或运行时环境](https://docs.anyscale.com/user-guide/configure/dependency-management/anyscale-environments)。首先使用集群配置，然后运行时配置(如果指定)将覆盖集群配置。**

**使用 Anyscale 在任何云上运行 Ray 代码的步骤如下:**

****第一步。** [**报名**](https://www.anyscale.com/signup) **参加 Anyscale 和** [**建立自己的账户**](https://docs.anyscale.com/get-started) **。****

****第二步。创建集群配置**。我这样做是为了方便，因为我有许多非典型的、较新的 ML 库要安装，并带有依赖项。打开浏览器到 [Anyscale 控制台](https://console.anyscale.com/)，在`Configurations`左侧菜单下，点击`Create new environment`按钮。见下面 Anyscale 控制台的图片。**

1.  **`Cluster environment name`。给你的环境配置起一个名字。**
2.  **选择 Python 版本。**
3.  **选择一个基本 docker 图像。我选择了`anyscale/ray-ml:1.9.0-python38-gpu`。**
4.  **在`Pip packages`下，见下图了解要安装的软件包和顺序。**
5.  **在`Post build commands`下，如果您想自动安装[该演示代码](https://github.com/christy/AnyscaleDemos/tree/main/forecasting_demos)和数据，请参见下图。**
6.  **点击`Create`按钮。**

**记下您的`cluster-config-name:version_number`。在下面的截图中，我的名字是`christy-forecast-pytorch:13`。下一步你会需要这个。**

**![](img/5a76215f7abc5a57bb71f9f303de80e4.png)**

**Anyscale 控制台，显示配置左侧菜单。主屏幕显示了一个示例配置，在基本 docker 映像上安装了额外的 pip 和 github clone。**

****第三步。用云集群的名称和集群配置初始化光线**。**

```
import anyscale # initialize ray on Anyscale to run on any cloud 
# name your cluster on the fly 
# set cluster_env parameter = your preconfigured cluster config name
ray.init(      
     **anyscale://my-cool-cluster**, #give your cluster any name   
     **cluster_env=christy-forecast-pytorch:13**, 
)
```

****第四步。用 num_workers=N 初始化 Ray Lightning 插件**，其中 N > num cpu 在你的云集群的头节点上。如果指定任意数字< = N，Anyscale 将不会向外扩展。对于任意数量的 N，Anyscale 自动缩放将自动触发，直到您的帐户中配置的限制。如果您可以访问 GPU，请设置 GPU。**

```
plugin = \
     RayPlugin(
          num_workers=**10**,              
          num_cpus_per_worker=1,              
          use_gpu=**True**,                  
     )
```

**现在，像平常一样运行 python 代码(或笔记本)。它会自动在任何云中并行运行！当您的应用程序运行时，您可以在`Clusters`下的 [Anyscale 控制台](https://console.anyscale.com/)中监控您的云集群使用情况。**

# **结论**

**这篇博客展示了为用于时间序列预测的 PyTorch Lightning 模型启用数据和模型并行性是多么容易。只需要最少的代码更改。一旦针对 Ray 进行了修改，相同的代码可以在您的笔记本电脑上并行运行，或者通过 Anyscale 在任何云上并行运行。**

**演示的完整代码在 github 上的[。](https://github.com/christy/AnyscaleDemos/blob/main/forecasting_demos/pytorch_forecasting_ray_local.ipynb)**

**感谢 Jan Beitner， [PyTorch Forecasting](https://pytorch-forecasting.readthedocs.io/en/stable/) 的作者，接受我的拉动请求并创建了一个维护版本，用于本演示。**

# **资源**

1.  **雷 doc 页数:[https://docs.ray.io/en/latest/using-ray.html](https://docs.ray.io/en/latest/using-ray.html)**
2.  **PyTorch 闪电雷外挂:[https://github.com/ray-project/ray_lightning](https://github.com/ray-project/ray_lightning)**
3.  **PyTorch 预测:[https://pytorch-forecasting.readthedocs.io/en/stable/](https://pytorch-forecasting.readthedocs.io/en/stable/)**
4.  **Anyscale doc 页数:[https://docs.anyscale.com/get-started](https://docs.anyscale.com/get-started)**
5.  **时态融合变换算法论文:[https://arxiv.org/pdf/1912.09363.pdf](https://arxiv.org/pdf/1912.09363.pdf)**
6.  **用于预测的 RNN 背景资料:[https://assets . Amazon . science/0b/93/4117 a 5014 a F5 DD 487d 7 ffd 74 ab/deep-state-space-models-for-time-series-Forecasting . pdf](https://assets.amazon.science/0b/93/4117a5014af5a5dd487d7ffd74ab/deep-state-space-models-for-time-series-forecasting.pdf)**
7.  **时间序列预测算法背景介绍:[https://towards data science . com/the-best-deep-learning-models-for-Time-Series-Forecasting-690767 bc63f 0](/the-best-deep-learning-models-for-time-series-forecasting-690767bc63f0)**
8.  **数据和模型并行性背景介绍:[https://arxiv.org/abs/1404.5997](https://arxiv.org/abs/1404.5997)**

***最初发表于*[*https://www.anyscale.com*](https://www.anyscale.com/blog/scaling-time-series-forecasting-on-pytorch-lightning-ray)*。***