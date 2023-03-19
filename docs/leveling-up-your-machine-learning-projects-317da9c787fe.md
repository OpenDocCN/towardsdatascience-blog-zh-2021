# 提升你的机器学习项目

> 原文：<https://towardsdatascience.com/leveling-up-your-machine-learning-projects-317da9c787fe?source=collection_archive---------27----------------------->

## 如何构建机器学习项目的实用指南

![](img/d569257e89d8e73d7a02e8ccd6835965.png)

照片由[阿诺·弗朗西斯卡](https://unsplash.com/@clark_fransa?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

从数据科学开始，笔记本是你的朋友。它们是可视化和探索数据的多用途工具，但随着项目变得越来越复杂，它们并不是最好的。

对于这样的项目，我们希望代码更加结构化和可伸缩，以便与不同类型的架构和数据集一起工作。

在这篇博文中，我将分享一个简单通用的项目模板，可以用于你的深度学习项目。我们将看到如何为 PyTorch 项目编写良好的可重用代码，以及如何更好地构建我们的 Python 项目！

遵循下面的代码！

<https://github.com/reoneo97/pytorch-project-template>  

## **我们的兵工厂**

在进入项目的结构之前，让我们看一下我们将使用的 Python 包。

1.  由脸书开发的 PyTorch 一直是我深度学习项目的首选库。我真正喜欢 PyTorch 的是它的灵活性和模块化。使用`nn.Module`类构建模型，使用`forward`函数可以很容易地将许多不同的组件拼凑在一起。
2.  PyTorch Lightning 解决了 PyTorch 最大的缺点，那就是需要大量的样板代码。这里我们使用 **PyTorch 闪电来训练我们的香草 PyTorch 模型**。我们通过使用`pl.LightningModule`抽象掉大部分丑陋的代码来做到这一点。
3.  随着你的机器学习项目的增长，超参数的数量也会增长。大多数项目将使用 YAML 配置文件来指定所有这些细节。这就是 Pydantic 的用武之地！Pydantic 为配置文件提供了类型验证，这有助于确保项目中的一切顺利进行

# **项目结构**

在这个项目中，我们将建立一个简单的 CIFAR 10 图像分类器，但我们将像 Python 包一样构造它，而不是使用笔记本。

```
|-- project
|   |-- __init__.py
|   |-- config.py
|   |-- datasets
|   |   |-- __init__.py
|   |   `-- dataset.py
|   |-- experiment.py
|   `-- models
|       |-- __init__.py
|       `-- conv.py
|-- requirements.txt
|-- config.yml
`-- main.py
```

我们将根包命名为`project`。在这个产品包中，我们有 3 个重要组件

1.这个目录将存放用于创建 ML 模型的 PyTorch 模块。

2.`datasets/`-包含用于训练模型的数据集和数据加载器类

3.`config.py` —包含加载、解析和验证配置文件的代码

## **型号**

使用`nn.Module`类在 PyTorch 中实例化神经网络，这个目录将包含所有的类文件

对于这个项目，我们希望代码是通用的，并允许架构的动态构建。代码应该接受像层数、层大小、卷积大小等参数。，并在此基础上建立模型。让我们来看一个这样的例子。

基本卷积模型

在代码中，我们将一个维度大小列表传入模型，它将基于该大小列表生成卷积层。

以这种方式编写代码，我们只需更改模型中的参数，就可以更快地进行实验，而不需要进入代码本身。

既然模型代码已经写好了，让我们来看看如何构建它。

## **__init__** 。巴拉圭

`__init__.py`文件用于将目录实例化为一个 python 包，但我们为它添加了额外的功能，以使我们的工作流程更加顺畅。

在文件中，我们可以指定可以导入的特定类/函数，而不必访问单个 Python 文件。这通过定义一个`__all__`列表并指定要导入的不同类/函数来完成。示例[此处](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/linear_model/__init__.py)

__init__ 文件和模型字典

在我们的例子中，我们希望我们的`__init__`文件略有不同。我们正在寻找的是一种使用字符串来定义模型架构类型的方法。

这可以通过创建一个字典来存储每个 PyTorch 模块，用一个字符串作为键(`Dict[str:nn.Module]`)来实现。这允许我们使用配置文件中的字符串来索引字典，并选择我们想要使用的模型。稍后我们可以将这个字典导入到主脚本中。

> `StackModel`是另一个具有 ResNet 层的卷积网络，查看源代码了解更多信息！

## 数据集

在这个目录中，我们将放置为模型加载不同数据集所需的所有代码。对于我们的项目，我们将使用 PyTorch 的 CIFAR10 数据集，但在某些情况下，可能需要构建我们自己的数据集类。

加载数据的功能

对于我们的例子，我们编写了一个简单的函数，通过传入两个参数返回一个`DataLoader`,即数据的路径和批量大小。这个函数将在我们编写训练代码时用到

## **项目配置**

为机器学习编写好代码的一个重要目标是在不改变任何代码的情况下改变模型。这就是为什么我们需要一个配置文件，在一个地方存储重要的参数和变量，让我们可以快速修改它们。

有许多不同的方式来编写配置文件，但作为个人偏好，我喜欢使用 YAML 文件。YAML 文件本质上是将变量编码为键值对，这可以被 Python 解析并作为字典加载。

然后这个字典将被转换成 Pydantic 对象。本质上，Pydantic 帮助我们用一组特定的类型化变量来定义类。这有助于我们验证输入到模型中的超参数，确保所有需要的超参数都存在，并且是正确的类型。

Pydantic 类的示例

定义任何 Pydantic 对象都很简单，我们只需要继承`BaseModel` 类并定义模型内部的变量。对于每个被定义的变量，我们还需要提供该变量的类型，Pydantic 将帮助进行必要的转换/验证。我们也可以使用`Optional`类型来指定具有默认值的参数。

除了执行类型验证，Pydantic 还允许我们编写简单的验证测试来确保输入到模型中的超参数是有效的。一个简单的例子是确保决定架构的值是非负的。

对于这个项目，我们还希望将配置分成 3 个不同的部分:

1.  模型配置
2.  培训配置
3.  实验配置

这有助于更好地构建配置文件，并帮助我们更容易地找到超参数。

解析 config.yml 并转换为 Pydantic 对象

一旦创建了 Pydantic 基本模型，我们需要做的就是创建一个函数来加载配置文件，读取它，然后将它转换成 Pydantic 对象。

这个 Pydantic 对象稍后将被导入到我们的`main.py`文件中，参数用于开始我们的训练

## **PyTorch 闪电**

一旦所有这些组件都完成了，我们就可以开始编写 PyTorch Lightning 代码了，它将使所有的组件一起工作。

PyTorch lightning 是一个在 vanilla PyTorch 基础上有用的包，它允许在不使用大量常用样板代码的情况下构建模型。

而不是编写用于训练/加载等的样板代码。，我们编写类方法，如返回损失的`training_step`，返回数据加载器的`train_dataloader`，PyTorch lightning 完成其余工作。我们的 PyTorch lightning 模型只需要模型(PyTorch 模块)和几个参数，它将执行训练模型所需的梯度下降步骤。

如果你想要更深入的介绍，可以看看我在这方面的博文

</beginner-guide-to-variational-autoencoders-vae-with-pytorch-lightning-part-2-6b79ad697c79> [## 使用 PyTorch Lightning 的可变自动编码器(VAE)初学者指南(第 2 部分)

towardsdatascience.com](/beginner-guide-to-variational-autoencoders-vae-with-pytorch-lightning-part-2-6b79ad697c79) 

让我们来看看`training_step`功能。该函数接受一批训练数据作为输入，并返回该批数据的损失。PyTorch lightning 将在幕后进行反向传播和优化，以训练模型。

PyTorch 闪电代码

首先，我们将输入传递到模型中。如果您还记得，`model`是作为一个类变量传入的，我们将在 forward 函数中使用它。这可以通过使用`out = self.model(input)`简单地完成。接下来，我们使用该输出来计算损失函数，在这种情况下，我们使用`CrossEntropyLoss`。剩下的就是归还损失，我们就完事了！

## **最终剧本**

一旦一切都完成了，我们需要的最后一个文件就是运行一切的 python 脚本。

这个脚本唯一要做的就是从`project`包中导入类并运行训练代码。

在代码的第一部分，我们正在构建将被训练的模型。我们首先使用“models”字典，并从配置文件中索引我们想要使用的模型。然后我们基于`model_config`实例化模型。这是通过将 Pydantic 对象转换为字典并使用**关键字参数解包** `**config.dict()`来创建对象来完成的。

下一部分是训练模型。我们首先以类似于模型的方式创建`Experiment` 类和`Trainer`类。然后我们用训练器来拟合模型就这样！大部分繁重的工作已经在前面的部分完成，我们可以有一个非常清晰的`main.py`培训脚本。

这样，项目就完成了！项目模板具有训练良好模型的所有必要组件，同时还具有足够的灵活性，可以轻松地适应不同的场景。

虽然这个模板对于示例项目来说有点大材小用，但它是一块垫脚石，可以随着项目复杂性的增加而扩展。适当地组织你的代码是一个很好的实践，这将有助于提高它的可读性和可用性，尤其是在专业环境中。

# 走向

这个模板只是冰山一角，还有很多其他方法可以改进它，使它对您的工作流程有用。以下是一些建议:

**1。阅读代码**

写好代码的最好方法是阅读好代码。为此，我建议阅读 scikit-learn 的源代码。Scikit-Learn 提供了大量不同的模型、功能变压器和实用功能，但一切都被整齐地打包在一起，易于使用。

<https://github.com/scikit-learn/scikit-learn>  

**2。部署机器学习模型课程**

如果你有兴趣让你的 ML 工程技能更上一层楼，我绝对推荐这门课。它教我如何使用 Pydantic 进行配置验证，还有很多内容，比如编写单元测试以及如何在 API 中部署模型。

[https://www . udemy . com/course/deployment-of-machine-learning-models/](https://www.udemy.com/course/deployment-of-machine-learning-models/)

如果你喜欢这篇文章，请在 Medium 上关注我！
在 LinkedIn 上连接:[https://www.linkedin.com/in/reo-neo/](https://www.linkedin.com/in/reo-neo/)