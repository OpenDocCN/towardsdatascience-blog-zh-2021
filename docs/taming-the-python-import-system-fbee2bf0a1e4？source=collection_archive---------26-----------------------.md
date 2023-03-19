# 驯服 Python 导入系统

> 原文：<https://towardsdatascience.com/taming-the-python-import-system-fbee2bf0a1e4?source=collection_archive---------26----------------------->

## 把它做错，这样你就能把它做对

![](img/fe3d4c85f5dc3000bec6fbb4f59ef6c9.png)

照片由[格伦·凯莉](https://unsplash.com/@glencarrie?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这些年来，我遇到了相当多的导入问题，只是在反复试验的基础上让它们工作。导入是困难的，因为有太多的因素相互作用，使事情在不应该的时候工作，并使事情在不应该的时候失败/得到警告。我终于厌倦了这一点，这篇文章(主要是织机视频)旨在通过一系列实验向您展示影响导入成功和失败的因素，这些实验演示了您可能会遇到的许多情况。

它可能没有涵盖所有情况，但是列出了一些可能的交互，希望让您有足够的知识来知道要注意什么，以便您可以在不可预见的情况下进行调试。这篇文章是对视频的总结，也是为了帮助你决定是否观看视频，新手从视频开始会做得更好。

## 视频

[https://www.loom.com/share/efea0a982d1d4f73a4d8cd5837b484c7](https://www.loom.com/share/efea0a982d1d4f73a4d8cd5837b484c7)

## 你将学到什么

1.  运行 python 文件的 2 种模式
    a .作为模块`python -m main`
    b .作为脚本`python main.py`
2.  绝对进口与相对进口
3.  如何编写导入语句决定了代码的成败
4.  运行终端的当前目录，以及打开 VSCode 的文件夹，决定了代码的成功/失败
5.  什么是 __init__。py 代表什么？

## 2 种运行模式

用-m 作为模块运行将当前目录放在`sys.path`的第一个索引中，作为脚本运行将脚本的直接父目录放在`sys.path`的第一个索引中。
当项目层次结构简单且没有嵌套时，或者当您从模块(python 文件)正上方的目录运行程序时，这两个路径可能是相同的，所以这里没有什么可担心的。然而，当这两个路径开始不同时，你可以打印`sys.path`来调试。

## 绝对进口与相对进口

绝对导入使用项目根中模块的完整路径，相对导入使用点符号。Absolute 更明确地说明了模块在项目层次结构中的位置，如果您不想键入长时间的导入，并且不需要知道模块相对于它正在导入的模块的位置，可以使用 relative。

当进行相对导入时，作为脚本运行将导致`__package__`为 None，因此没有相对性概念的参考点，所以它将失败。如果模块位于顶层，作为模块运行可能仍然会失败，这会导致`__package__`为`''`空字符串。对于`__package__`正确包含包名(例如文件夹)的其他情况，相对导入工作正常。

绝对导入失败可以通过像`sys.path.insert(0,path_to_package)`这样的快速破解(通常在将模块导入 jupyter 笔记本时)来修复，但这无助于依赖于`__package__`具有正确值的相对导入失败。

熊猫用绝对，Sklearn 用相对。
**熊猫**:[https://github . com/Pandas-dev/Pandas/blob/master/Pandas/core/group by/group by . py](https://github.com/pandas-dev/pandas/blob/master/pandas/core/groupby/groupby.py)
**sk learn**:[https://github . com/scikit-learn/scikit-learn/blob/main/sk learn/metrics/pairwise . py](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/metrics/pairwise.py)

## 如何编写导入决定了成功/失败

`from folder.utils import add` vs `from utils import add`是两种不同的编写导入的方式。前者搜索`folder`进行导入，而后者搜索`utils`。当出现问题时，查看`sys.path`,看看您导入的内容是否能在任何路径中找到。

## 运行终端的当前目录以及打开 VSCode 的文件夹决定了成功/失败

作为模块或脚本运行的 terminal 中的当前目录会影响哪个路径被添加到`sys.path`的前面，从而影响代码中的导入(也取决于您如何编写它们)是否能被找到。

您打开 VSCode 所在的文件夹会影响红色下划线警告“xxx 无法解析”。它们很碍眼，可以通过在正确的目录中打开 VSCode 来修复。(假设您处于正确的环境中，并且安装了导入的软件包)

## 什么是 __init__。巴拉圭

帮助有用的模块/功能从项目层次结构的深处上升到顶层，因此用户或其他模块可以用更简单的`from folder import add`而不是更深的`from folder.utils import add`来调用它们。
从我的另一篇文章中可以看到类似的概念([https://towards data science . com/whats-the-difference-between-PD-merge-and-df-merge-ab 387 BC 20 a2e](/whats-the-difference-between-pd-merge-and-df-merge-ab387bc20a2e))，其中`merge`函数通过该文件中的导入从`pandas.core.reshape.merge`模块冒泡到`pandas.core.reshape.api`名称空间，然后通过位于 [https://github 的 pandas 包中的顶层`__init__.py`进一步冒泡到顶层`pandas.merge`py#L129-L143](https://github.com/pandas-dev/pandas/blob/v0.25.1/pandas/__init__.py#L129-L143) 。

我欢迎在评论中对更正或不清楚的解释的任何反馈，我将用它们来更新这篇文章。

参考文献:
1。[https://stack overflow . com/questions/16981921/relative-imports-in-python-3](https://stackoverflow.com/questions/16981921/relative-imports-in-python-3)
2 .[https://stack overflow . com/questions/14132789/relative-imports-for-the-billion-time](https://stackoverflow.com/questions/14132789/relative-imports-for-the-billionth-time)
3 .[https://stack overflow . com/questions/22241420/execution-of-python-code-with-m-option-or-not](https://stackoverflow.com/questions/22241420/execution-of-python-code-with-m-option-or-not)
4 .[https://stack overflow . com/questions/21233229/what-the-purpose-of-package-attribute-in-python/](https://stackoverflow.com/questions/21233229/whats-the-purpose-of-the-package-attribute-in-python/)
5 .[https://stack overflow . com/questions/448271/what-is-init-py-for](https://stackoverflow.com/questions/448271/what-is-init-py-for)