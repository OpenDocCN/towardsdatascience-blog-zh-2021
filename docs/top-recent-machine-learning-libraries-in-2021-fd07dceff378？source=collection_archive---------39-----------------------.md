# 2021 年最新的机器学习库

> 原文：<https://towardsdatascience.com/top-recent-machine-learning-libraries-in-2021-fd07dceff378?source=collection_archive---------39----------------------->

## 包含在您的 ML 项目中的最广泛使用和最有用的 ML 库的集合

![](img/14afab8aab6505f9ea7c0085edfb21f5.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral) 拍照

库是允许我们快速实现网络和开发成功应用程序的主要使能工具之一。ML 库一直在快速变化，我认为给最好的新库一个概述是个好主意。有许多流行的库已经存在了很长时间，我不打算回顾它们，因为你可能对它们很熟悉；这些是:

*   Pytorch & Tensorflow/Keras
*   熊猫和 Matplotlib
*   数字与科学工具包学习

到目前为止，这些都是顶级的图书馆，没有必要去回顾它们。我将回顾我遇到的其他库，并看到许多其他人在 Kaggle 比赛中使用它们。

## [1。timm](https://pypi.org/project/timm/) —监督图像学习

有大量的 SOTA 监督图像模型，每次我想实现一个新的模型，你都必须研究一段时间，直到你找到一个可以在 Keras / Pytorch 中适应的简单实现。Timm 提供超过 **347 种型号**，包括从 ResNet 到 ResNext 到 EfficientNet 等等。它支持的网络数量给我留下了很深的印象，而且实现起来也很容易，你只需要输入并加载网络名称就可以了。我很喜欢图书馆的概念，它把所有的东西都“收集”在一个地方，这样你就可以省去很多研究。如果您想查看受支持型号的列表，您只需做以下事情:

```
import 
supported_models = timm.list_models()
# You will then get a list of more than 347 models (they might be more)
```

*   [**预训练模型— PyTorch**](https://github.com/Cadene/pretrained-models.pytorch)

如果您没有在 timm 中找到您正在寻找的模型，您很可能会在 Pretrainedmodels 中找到它。您甚至可以在 timm 中找到它，但不是权重，这就是 Pretrainedmodels 的用途。

## 2. [MMDetection —物体检测](/mmdetection-tutorial-an-end2end-state-of-the-art-object-detection-library-59064deeada3)

MMDetection 在某种意义上类似于 timm，它是一个“集合库”,但用于对象检测模型，而不是监督图像模型。它支持 30 多种物体检测模型，你可以点击这里查看。关于这个库最好的事情是，它对构建一个 End 2 End 对象检测管道有很大的帮助。因为对象检测比监督图像分类任务稍微复杂一些，所以这个库非常有用。

## 3.细分 _ 车型(既有[py torch](https://github.com/qubvel/segmentation_models.pytorch)&Keras

这是我将要提到的最后一个“收藏图书馆”。一个给喀拉斯，一个给 Pytorch。它基本上包括了所有的图像分割模型，并为图像分割任务提供了许多有用的实用函数。我提到所有这些“集合”库的原因是，我发现它们在 Kaggle 比赛中非常有用，当你正在寻找快速原型时。这在许多机器学习项目中是非常需要的，在这些项目中，您想要测试几个模型并比较它们的性能。

## 4. [Pytorch 闪电](https://www.pytorchlightning.ai/#footer)

Pytorch 闪电之于 Pytorch 就像 Keras 之于 Tensorflow。Pytorch 的一个重要问题是它的实现相当长(就代码行而言)。Pytorch Lightning 更像是一个高级 API，构建在 Pytorch 之上，使编写神经网络代码变得更简单、更快速(因此有了 Lightning)。它还提供了一个非常有用的特性，称为“ **Grid** ”，这是一个大型云监视器，用于监视您正在运行的模型。

## **5。** [**相册**](https://github.com/albumentations-team/albumentations)

图像增强不再是可选功能。这是迄今为止使用最广泛的库之一，也是易于实现的图像扩充的首选。它很容易使用，并且有大量的教程和资源。它还提供了对大量不同扩展的支持，您可以使用这些扩展(超过 30 种不同的扩展)。

## [6。拥抱脸](https://huggingface.co/)

Huggingface 在 NLP 方面做得非常好。该库提供了对大量 SOTA 变压器和 NLP 模型的支持。它们提供了一些函数来帮助完成传统的 NLP 任务，并帮助您用很少的几行代码构建端到端的 NLP 项目。他们拥有迄今为止最大的 SOTA 变形金刚和 NLP 模型收藏。我想他们最近正在开发一个 Auto-ML NLP 工具包，它基本上为你正在尝试做的 NLP 任务选择最好的转换器。

**最终想法**

我希望这篇文章对你有用。这些图书馆为我和许多人节省了大量的时间和精力。我认为用 Python 这样的语言编写的一个主要优势是有大量有用且易于使用的库(尤其是在机器学习方面)。如果你知道一个好的 ML 库，你认为它应该在这个列表中，请在评论中写下它。

如果你想定期收到关于人工智能和机器学习的最新论文的评论，请在这里添加你的电子邮件并订阅！

【https://artisanal-motivator-8249.ck.page/5524b8f934 号