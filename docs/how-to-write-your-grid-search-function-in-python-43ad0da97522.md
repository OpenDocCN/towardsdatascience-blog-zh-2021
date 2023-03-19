# 如何用 Python 编写网格搜索函数

> 原文：<https://towardsdatascience.com/how-to-write-your-grid-search-function-in-python-43ad0da97522?source=collection_archive---------10----------------------->

## Itertools 是您需要的全部

![](img/8f63ff6ee5370ee798f41b58244f6fda.png)

本杰明·博斯奎特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 介绍

我回来尝试在 medium 写一些关于编码、机器学习和 Python 的东西。可能，我是唯一一个对此感到兴奋的人，但至少有人:)

由于我没有太多的时间，我决定(希望)写一个系列来解释微小的 Python 函数，这些函数可以使您的日常开发工作变得更容易。你准备好了吗，还有大约一分钟？那我们开始吧！

## 问题是

假设您想要测试一组超参数来训练您的模型。一种方法是使用网格搜索。在网格搜索中，您可以创建想要尝试的参数的每种可能的组合。对于所有这些组合，您训练您的模型并运行一些评分方法，以了解在给定参数集的情况下，训练好的模型表现如何。

当您是 Python 用户时，最明显的选择是使用 scikit-learn，更具体地说是使用名为 [GridsearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) 的类。然而，您可能不喜欢 scikit-learn(我对此表示怀疑)，或者您编写了一些不符合 scikit-learn 接口的自定义代码，或者您只想做一些完全不同的事情。对于这些情况，我有一个用纯 Python 编写的解决方案。这样，当您想要将搜索扩展到更多参数时，就不必编写许多嵌套的 for 循环，也不必更改代码。

## 解决方案

现在，不用多说，让我们直接进入解决方案

我在这里做了什么？首先，为了避免嵌套和不灵活的 for 循环，我使用了 awesome [itertools](https://docs.python.org/3/library/itertools.html) 模块中的`product`函数。它所做的是构建一个 iterable，返回你传入的所有 iterable 的笛卡尔积。听起来很疯狂。例如，如果您将四个 iterables 传递给`product`，每个 iterables 包含两个条目，那么您将得到大小为 4 的 2⁴=16 元组。也许听起来不太好，那就试试吧。

其次，为了方便起见，我没有生成包含 concert 值的普通元组，而是创建了一个字典，将输入键映射到各自的单个值。这应该会简化`grid_parameters`函数的使用。

最后，怎么用？您必须在字典中定义想要尝试的参数。键/名称应该理想地匹配您将传递给模型的`__init__`函数的参数。他们不需要这样做，但是这样会让你的生活更轻松。你的字典**的值必须**是可迭代的，所以列表或元组或类似的东西包含所有你想尝试的值。现在，您只需将该字典传递给`grid_parameters`函数，并对其进行迭代。在那之后，你想用你的网格点做什么就看你自己了。很简单，不是吗？只有 3 行代码。耶！

![](img/fe7ff51f1fe1aa0aa04fb97439055c86.png)

照片由[艾伦·赫特小](https://unsplash.com/@alanaut24?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## 结论

在这篇非常简短的文章中，我向您展示了一个紧凑且易于使用的原生 Python 实现来生成网格点，您可以使用这些网格点来执行网格搜索。使用它的主要原因是，当您构建了一个没有实现 scikit learn estimator Apis 的模型时，或者当您只想展示您的 Python 技能时:-)

感谢您关注这篇文章。一如既往，如有任何问题、意见或建议，请随时联系我，或者通过 [LinkedIn](https://www.linkedin.com/in/simon-hawe-75832057/) 。