# 使用 OpenAI 剪辑链接图像和文本

> 原文：<https://towardsdatascience.com/linking-images-and-text-with-openai-clip-abb4bdf5dbd2?source=collection_archive---------12----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 什么是剪辑以及如何使用它

![](img/ea26b1b5364fe6e83334991f7960ed19.png)

照片由来自 Unsplash 的 [Marten Newhall](https://unsplash.com/@laughayette) 拍摄。

# 介绍

尽管深度学习已经彻底改变了计算机视觉和自然语言处理，但使用当前最先进的方法仍然很困难，并且需要相当多的专业知识。

OpenAI 方法，如对比语言图像预训练(CLIP)，旨在降低这种复杂性，从而使开发人员专注于实际案例。

CLIP 是一个在大量(400M)图像和文本对上训练的神经网络。作为这种多模态训练的结果，CLIP 可以用于找到最能代表给定图像的文本片段，或者给定文本查询的最合适的图像。

这使得 CLIP 对于开箱即用的图像和文本搜索非常有用。

# 它是如何工作的？

对 CLIP 进行训练，使得给定一幅图像，它预测该图像在训练数据集中与 32768 个随机采样的文本片段中的哪一个配对。这个想法是，为了解决这个任务，模型需要从图像中学习多个概念。

这种方法与传统的图像任务有很大的不同，在传统的图像任务中，通常需要模型来从一大组类别中识别出一个类别(例如 ImageNet)。

总之，CLIP 联合训练一个图像编码器(像 ResNet50)和一个文本编码器(像 BERT)来预测一批图像和文本的正确配对。

![](img/fb7697809109cacf384118bb2c8daf38.png)

剪辑训练(1)和零镜头学习(2) (3)的原始插图可从[从自然语言监督](https://arxiv.org/abs/2103.00020)论文中学习可转移视觉模型获得。(1)在 N 维向量中处理和编码图像和文本片段。该模型通过最小化正确图像-文本对(N 个真实对)之间的余弦距离，同时最大化不正确对(N -N)之间的余弦距离来训练。(2)为了使用剪辑模型进行零镜头学习，类值被编码在文本片段中。(3)将每个类别值的文本嵌入与图像嵌入进行比较，并通过相似性进行排序。如需详细描述，请阅读回形针。

如果希望使用该模型进行分类，可以通过文本编码器嵌入类别，并与图像进行匹配。这个过程通常被称为零射击学习。

# 入门指南

以下部分解释了如何在 Google Colab 中设置 CLIP，以及如何使用 CLIP 进行图像和文本搜索。

## 装置

要使用 CLIP，我们首先需要安装一组依赖项。为了方便起见，我们将通过 Conda 安装它们。此外，谷歌 Colab 将用于使复制更容易。

1.  *打开 Google Colab*

在浏览器中打开以下网址:[https://research.google.com/colaboratory/](https://research.google.com/colaboratory/)

然后，点击屏幕下方的**新 PYTHON 3 笔记本**链接。

你可能注意到了，笔记本的界面和 Jupyter 提供的很像。有一个代码窗口，您可以在其中输入 Python 代码。

*2。检查 Colab 中的 Python*

为了安装正确的 Conda 版本，使其看起来可以与 Colab 一起工作，我们首先需要知道 Colab 使用的是哪个 Python 版本。在 colab 类型的第一个单元格中这样做

这应该会返回类似于

```
/usr/local/bin/python 
Python 3.7.10
```

*3。安装康达*

在你的浏览器中打开以下网址:
[https://repo.anaconda.com/miniconda/](https://repo.anaconda.com/miniconda/)

然后复制与上面输出中指示的主要 Python 版本相对应的 miniconda 版本名称。miniconda 版本应该类似于“miniconda 3-py { VERSION }-Linux-x86 _ 64 . sh”。

最后，在 colab 的新单元格中键入以下代码片段，确保 conda_version 变量设置正确。

再次确认 Python 主版本仍然是相同的

这应该会返回类似于

```
/usr/local/bin/python 
Python 3.7.10
```

*4。安装剪辑+依赖关系*

康达现在应该很有起色了。下一步是使用 Conda 安装剪辑模型的依赖项(pytorch、torchvision 和 cudatoolkit ),然后安装剪辑库本身。

为此，将下面的代码片段复制到 Colab 中。

这一步可能需要一段时间，因为所需的库很大。

*5。将 conda 路径附加到系统*

使用 CLIP 之前的最后一步是将 conda site-packages 路径附加到 sys。否则，在 Colab 环境中可能无法正确识别已安装的软件包。

## 文本和图像

我们的环境现在可以使用了。

1.  *导入剪辑模型*

通过导入所需的库并加载模型来使用 CLIP start。为此，将下面的代码片段复制到 Colab。

这应该显示类似下面的返回，表明模型被正确加载。

```
100%|███████████████████████████████████████| 354M/354M [00:11<00:00, 30.1MiB/s]
```

*2。提取图像嵌入*

现在让我们使用下面的示例图像来测试模型

![](img/816a322348c868e62078f1644d264c98.png)

图片来自 Pexels 的 Artem Beliaikin

为此，将下面的代码片段复制到 Colab。这段代码将首先使用 PIL 加载图像，然后使用剪辑模型对其进行预处理。

这将显示样本图像，然后是经过处理的图像张量。

```
Tensor shape: 
torch.Size([1, 3, 224, 224])
```

现在可以通过从剪辑模型中调用“encode_image”方法来提取图像特征，如下所示

这应该返回图像特征的张量大小

```
torch.Size([1, 512])
```

*3。提取文本嵌入内容*

让我们创建一组文本片段，其中不同的类值以如下方式嵌入:“一个#CLASS#的照片”。

然后我们可以运行 clip tokeniser 来预处理代码片段。

这将返回文本张量形状

```
torch.Size([3, 77])
```

现在可以通过从剪辑模型中调用“encode_text”方法来提取文本特征，如下所示

*4。比较图像嵌入和文本嵌入*

因为我们现在有了图像和文本嵌入，我们可以比较每个组合，并根据相似性对它们进行排序。

为此，我们可以简单地调用两个嵌入的模型，并计算 softmax。

这应该会返回以下输出。

```
Label probs: [[0.9824866 0.00317319 0.01434022]]
```

正如所料，我们可以观察到“一张狗的照片”文本片段与样本图像具有最高的相似性。

现在，您可以让文本查询包含更多的上下文，并查看它们之间的比较。例如，如果你添加“一张狗在草地上跑的照片”，你会想象现在的排名会是什么样子？

## 完整脚本

如需完整的脚本，请点击以下链接进入我的 github 页面:

[](https://github.com/andreRibeiro1989/medium/blob/ed800bad2c636049ea789dfd77598a8b72e3e42f/clip_getting_started.ipynb) [## 安德里贝罗 1989/中号

### clip_getting_started.ipynb。

github.com](https://github.com/andreRibeiro1989/medium/blob/ed800bad2c636049ea789dfd77598a8b72e3e42f/clip_getting_started.ipynb) 

或者通过以下链接直接访问 Google Colab 笔记本:

[](https://colab.research.google.com/github/andreRibeiro1989/medium/blob/main/clip_getting_started.ipynb) [## 谷歌联合实验室

### clip_getting_started.ipynb

colab.research.google.com](https://colab.research.google.com/github/andreRibeiro1989/medium/blob/main/clip_getting_started.ipynb) 

# 结论

CLIP 是一个非常强大的图像和文本嵌入模型，可用于查找最能代表给定图像的文本片段(如在经典分类任务中)，或给定文本查询的最合适图像(如图像搜索)。

CLIP 不仅功能强大，而且非常易于使用。该模型可以很容易地嵌入到 API 中，并通过 AWS lambda 函数等方式提供。

[1] Openai。【https://openai.com/blog/clip/#rf36】剪辑:连接文字和图像
T5

[2]亚历克·拉德福德，琼·金旭，克里斯·哈拉西等.*从自然语言监督中学习可转移的视觉模型*
[arXiv:2103.00020](https://arxiv.org/abs/2103.00020)