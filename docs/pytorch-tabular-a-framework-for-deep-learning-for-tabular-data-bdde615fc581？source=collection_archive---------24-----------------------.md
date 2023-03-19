# py torch Tabular——表格数据深度学习框架

> 原文：<https://towardsdatascience.com/pytorch-tabular-a-framework-for-deep-learning-for-tabular-data-bdde615fc581?source=collection_archive---------24----------------------->

众所周知，当涉及到表格数据时，[梯度推进模型](https://deep-and-shallow.com/2020/02/02/the-gradient-boosters-i-the-math-heavy-primer-to-gradient-boosting-algorithm/)通常会击败所有其他机器学习模型。我写了大量关于梯度推进、[背后的理论](https://deep-and-shallow.com/2020/02/02/the-gradient-boosters-i-the-math-heavy-primer-to-gradient-boosting-algorithm/)的文章，并涵盖了不同的实现，如 [XGBoost](https://deep-and-shallow.com/2020/02/12/the-gradient-boosters-iii-xgboost/) 、 [LightGBM](https://deep-and-shallow.com/2020/02/21/the-gradient-boosters-iii-lightgbm/) 、 [CatBoost](https://deep-and-shallow.com/2020/02/29/the-gradient-boosters-v-catboost/) 、 [NGBoost](https://deep-and-shallow.com/2020/06/27/the-gradient-boosters-vib-ngboost/) 等。详细地说。

![](img/0d4906448bc5239b0d27feed1c023933.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

以许多其他形式显示的深度学习的不合理的有效性——如文本和图像——还没有在表格数据中得到证明。但是最近，深度学习革命已经将一点点焦点转移到了表格世界，因此，我们看到了专门为表格数据形态设计的新架构和模型。其中许多模型相当于甚至略好于调整良好的梯度推进模型。

# PyTorch 表格是什么？

![](img/c7a57ac15bff98517b9b17adb1590366.png)

PyTorch Tabular 是一个框架/包装器库，旨在使使用表格数据的深度学习变得容易，并可用于真实世界的案例和研究。图书馆设计背后的核心原则是:

而不是从零开始，框架已经搭在了 [**PyTorch**](https://pytorch.org/) (显然)和[**py torch Lightning**](https://www.pytorchlightning.ai/)这样的巨人肩上。

它还配备了最先进的深度学习模型，可以使用熊猫数据框架轻松训练。

*   高级配置驱动的 API 使得使用和迭代非常快速。您可以只使用 pandas 数据框架，所有标准化、规范化、分类特性编码和准备数据加载器的繁重工作都由库来处理。
*   `BaseModel`类提供了一个易于扩展的抽象类，用于实现定制模型，并且仍然利用库打包的其余机制。
*   实现了最先进的网络，如用于表格数据深度学习的[神经不经意决策集成](https://arxiv.org/abs/1909.06312)和 [TabNet:专注的可解释表格学习](https://arxiv.org/abs/1908.07442)。有关如何使用它们，请参见文档中的示例。
*   通过使用 PyTorch Lightning 进行培训，PyTorch 表格继承了 Pytorch Lightning 提供的灵活性和可伸缩性

# 为什么是 PyTorch 表格？

PyTorch Tabular 旨在降低行业应用和表格数据深度学习研究的准入门槛。就目前的情况来看，使用神经网络并不容易；至少不像传统的 ML 模型用 Sci-kit Learn 那么容易。

PyTorch 表格试图使使用神经网络的“软件工程”部分尽可能简单和容易，并让您专注于模型。我还希望将表格空间中的不同开发统一到一个具有 API 的单一框架中，该框架将与不同的最新模型一起工作。

目前，表格深度学习的大多数发展都分散在各个 Github repos 中。而且除了(我又爱又恨)，没有一个框架真正关注过表格数据。这也是 PyTorch 表格产生的原因。

# PyTorch 表格怎么用？

# 装置

首先，让我们看看如何安装这个库。

虽然安装包括 PyTorch，但是最好的和推荐的方法是首先从[这里](https://pytorch.org/get-started/locally/)安装 PyTorch，为你的机器选择正确的 CUDA 版本。(PyTorch 版本> 1.3)

一旦安装了 Pytorch，只需使用:

```
pip install pytorch_tabular[all]
```

使用额外的依赖项安装完整的库([权重&偏差](https://wandb.ai)用于实验跟踪)。

并且:

```
pip install pytorch_tabular
```

最基本的必需品。

pytorch_tabular 的源代码可以从`Github repo`下载。

您可以克隆公共存储库:

```
git clone git://github.com/manujosephv/pytorch_tabular
```

一旦您有了源代码的副本，您就可以使用以下软件进行安装:

```
python setup.py install
```

# 设置配置

您需要提供四个配置(其中大多数都有智能默认值)，这将驱动其余的过程。

*   **DataConfig** —定义目标列名、分类列名和数字列名、您需要做的任何转换等。
*   **型号配置** —每个型号都有特定的配置。这决定了我们要训练哪个模型，也让您定义模型的超参数
*   **TrainerConfig** —这让你可以通过设置诸如批量大小、时期、提前停止等来配置训练过程。绝大多数参数直接从 PyTorch Lightning 借用，并在训练期间传递给底层的训练器对象
*   **优化器配置** —这让您可以定义和使用不同的优化器和学习率调度器。支持标准 PyTorch 优化器和学习率调度器。对于自定义优化器，您可以使用 fit 方法中的参数来覆盖它。定制优化器应该是 PyTorch 兼容的
*   **实验配置** —这是一个可选参数。如果设置，这将定义实验跟踪。目前，只支持两个实验跟踪框架:Tensorboard 和 Weights&bias。B 实验跟踪器有更多的功能，如跟踪跨时代的梯度和逻辑。

```
data_config = DataConfig( target=['target'], continuous_cols=num_col_names, categorical_cols=cat_col_names, ) trainer_config = TrainerConfig( auto_lr_find=True, batch_size=1024, max_epochs=100, gpus=1) ptimizer_config = OptimizerConfig() model_config = CategoryEmbeddingModelConfig( task="classification", layers="1024-512-512", # Number of nodes in each layer activation="LeakyReLU", # Activation between each layers learning_rate = 1e-3 )
```

# 初始化模型和训练

现在我们已经定义了配置，我们需要使用这些配置初始化模型，并调用 *fit* 方法。

```
tabular_model = TabularModel( data_config=data_config, model_config=model_config, optimizer_config=optimizer_config, trainer_config=trainer_config, ) tabular_model.fit(train=train, validation=val)
```

就是这样。将针对指定数量的时期对模型进行训练。

## 型号列表

*   具有类别嵌入的前馈网络是一个简单的 FF 网络，但是具有用于类别列的嵌入层。这与 *fastai* 表格模型非常相似
*   [用于表格数据深度学习的神经不经意决策集成](https://arxiv.org/abs/1909.06312)是在 ICLR 2020 上提出的一个模型，据作者称，该模型在许多数据集上击败了经过良好调整的梯度推进模型。
*   [TabNet:Attention 可解释表格学习](https://arxiv.org/abs/1908.07442)是 Google Research 的另一个模型，它在决策的多个步骤中使用稀疏注意力来模拟输出。

要实现新模型，请参见[如何实现新模型教程](https://github.com/manujosephv/pytorch_tabular/blob/main/docs/04-Implementing%20New%20Architectures.ipynb)。它涵盖了基础架构和高级架构。

# 根据看不见的数据评估模型

为了在训练期间使用的相同度量/损失的新数据上评估模型，我们可以使用方法。

```
result = tabular_model.evaluate(test)-------------------------------------------------------------------------------- DATALOADER:0 TEST RESULTS 
{
'test_accuracy': tensor(0.6924, device='cuda:0'), 
'train_accuracy': tensor(0.6051, device='cuda:0'), 
'train_loss': tensor(0.6258, device='cuda:0'), 
'valid_accuracy': tensor(0.7440, device='cuda:0'), 
'valid_loss': tensor(0.5769, device='cuda:0')} 
--------------------------------------------------------------------------------
```

# 根据看不见的数据进行预测

为了获得数据帧形式的预测，我们可以使用`predict`方法。这将把预测添加到传入的同一数据帧中。对于分类问题，我们以 0.5 为阈值，得到概率和最终预测

```
pred_df = tabular_model.predict(test)
```

# 保存和加载模型

我们还可以保存一个模型，并在以后加载它进行推理。

```
tabular_model.save_model("examples/basic") loaded_model = TabularModel.load_from_checkpoint("examples/basic") result = loaded_model.evaluate(test)
```

# 代码、文档和如何贡献

该框架的代码可在 [PyTorch Tabular:为表格数据建立深度学习模型的标准框架(github.com)](https://github.com/manujosephv/pytorch_tabular)获得。

文档和教程可以在[py torch Tabular(py torch-Tabular . readthedocs . io)](https://pytorch-tabular.readthedocs.io/en/latest/)找到

我们非常欢迎投稿，关于如何投稿的详细信息也在这里[列出](https://pytorch-tabular.readthedocs.io/en/latest/contributing/)。

# 相关著作

*fastai* 是最接近 PyTorch 表格式的，都是建立在 PyTorch 之上。但是 PyTorch Tabular 与 fastai 的不同之处在于它的模块化和解耦性质，以及它对标准 PyTorch 和 PyTorch Lightning 组件的使用，这使得采用(包括新模型)和破解代码比使用 *fastai* 要容易得多。

# 参考

[1]塞尔戈·波波夫，斯坦尼斯拉夫·莫罗佐夫，阿尔滕·巴本科。 [*“用于表格数据深度学习的神经不经意决策集成”*](https://arxiv.org/abs/1909.06312) 。arXiv:1909.06312 [cs。LG] (2019)

[2]塞尔詹·奥·阿里克，托马斯·普菲斯特；。[*《TabNet:专注可解释性表格学习》*](https://arxiv.org/abs/1908.07442) 。arXiv:1908.07442 (2019)。

# 接下来呢？

我将继续撰写单独的博客文章来谈论到目前为止 PyTorch 表格中已经实现的不同模型。小心他们。

*原载于 2021 年 1 月 27 日 http://deep-and-shallow.com**的* [*。*](https://deep-and-shallow.com/2021/01/27/pytorch-tabular-a-framework-for-deep-learning-for-tabular-data/)