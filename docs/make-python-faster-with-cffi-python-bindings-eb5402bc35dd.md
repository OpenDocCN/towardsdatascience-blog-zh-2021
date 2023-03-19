# 使用 CFFI Python 绑定提高 Python 速度

> 原文：<https://towardsdatascience.com/make-python-faster-with-cffi-python-bindings-eb5402bc35dd?source=collection_archive---------17----------------------->

## 从 Python 中的任何 C 库中调用函数。准备好起飞了吗？

![](img/4438cca64c00b01e6c8d68fccd4ec15a.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=67643) 的[维基图片](https://pixabay.com/users/wikiimages-1897/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=67643)

Python 是最用户友好的编程语言之一。它易于学习，免费使用，并且您可以随意扩展它的功能。

此外，Python 可以说是数据科学和机器学习领域最常用的编程语言。优秀的数字库，如 NumPy 和 SciPy，以及卓越的深度学习框架，如 PyTorch 和 TensorFlow，为每个喜欢玩数据和人工神经网络的程序员创建了一个庞大的工具库。

另一方面，Python 是一种解释型语言，它可以设置代码执行速度的上限。在 AI 和大数据的世界里，这不是一件好事。所以，我们确实有矛盾。Python 如何成为该领域使用最多的编程语言？我们能做些什么让它更快？

好吧，我们可以用另一种语言(例如，C 或 C++)编写我们要求高的函数，并利用特定的绑定从 Python 调用这些函数。这就是这些巨大的数字库或深度学习框架所做的事情。因此，如果你是一名数据科学家或机器学习工程师，想要调用 CUDA 函数，这个故事适合你。

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=cffi)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# CFFI

在上一篇文章中，我们用标准 Python 库的一部分库`ctypes`做了同样的事情。我们看到了什么是编组，以及 Python 在内存管理方面与 C 有何不同。最后，我们通过一个简单的例子看到了`ctypes`如何帮助我们调用 C 函数。

</do-you-hate-how-slow-python-is-this-is-how-you-can-make-it-run-faster-532468fe1583>  

在这个故事中，让我们和 CFFI 一起更上一层楼。

## CFFI 对 ctypes

`ctypes`可能是 Python 标准库的一部分，但是所有这些类型声明都容易出错，并且它不能扩展到大型项目/

此外，`ctypes`允许您将共享库直接加载到 Python 程序中。另一方面，使用`CFFI`，您可以构建一个新的 Python 模块，并像导入任何其他 Python 库一样导入它。

## 装置

`CFFI`不是标准库的一部分。因此，要使用它，我们需要先安装它。我们可以使用 Python 包管理器`pip`轻松做到这一点:

```
$ python3 -m pip install cffi
```

> 请注意，建议使用 Python 虚拟环境来遵循本教程，以避免全局安装 Python 包。这可能会破坏系统工具或其他项目。

## 简单的例子

在这个例子中，我们将从的`m`库中调用一个函数。我们将使用这个函数来得到一个数的平方根。

第一步是创建一个 python 文件，该文件将构建一个具有我们需要的功能的 Python 模块；提供所有好东西的图书馆。

为此，复制并粘贴以下代码:

在这个例子中，您从的`m`库中用`sqrt`函数创建了一个绑定。首先，在`set_source`方法中，传递想要构建的 Python 模块的名称(例如`_libmath`)。

您还包括来自的`m`库的`math`头文件，因为我们使用的是来自`C`标准库的库，所以我们不需要在`library_dirs`中提供库的位置。

最后，您可以像运行任何其他 Python 脚本一样运行该脚本:

```
$ python3 build_cffi.py
```

这将产生`.o`和一个`.c`文件。让我们看看如何使用这个调用的输出。

## 从 Python 调用 C

现在您已经准备好从`C`调用`sqrt`函数。这就像运行下面的代码一样简单:

首先，从您创建的库(即`_libmath`)导入 lib 模块。在内部，您可以找到对`sqrt`函数的绑定。最后，您可以用任何浮点数调用这个函数。

您也可以创建到其他函数的绑定。例如，要创建与`m`库的`sin`或`cos`函数的绑定，只需在`cdef`中声明它们:

```
ffibuilder.cdef("""
    double sqrt(double x);
    double sin(double x);
""")
```

# 结论

Python 是最用户友好的编程语言之一，也可以说是数据科学和机器学习领域中使用最多的编程语言。

另一方面，Python 是一种解释型语言，它可以设置代码执行速度的上限。但并不是所有的希望都破灭了；我们可以用另一种语言(例如，C 或 C++)编写要求苛刻的函数，并利用特定的绑定从 Python 调用这些函数。

在上一篇文章中，我们讨论了`ctypes`以及如何使用它从 Python 中调用`C`库。在这篇文章中，我们使用了一种更加自动化的方法，并看到了如何使用`CFFI`来实现这一点。

接下来:我们将看到如何利用 Cython 来加速我们的 Python 代码！

# 关于作者

我的名字是[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=cffi)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。