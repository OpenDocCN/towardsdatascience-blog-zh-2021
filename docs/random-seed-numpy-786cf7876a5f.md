# random.seed 在 NumPy 中做什么

> 原文：<https://towardsdatascience.com/random-seed-numpy-786cf7876a5f?source=collection_archive---------32----------------------->

## 理解在 Python 中使用 NumPy 生成伪随机结构时如何创建可再现的结果

![](img/b4fa5cb1b304bbbae76bf0af2f266b34.png)

安德鲁·西曼在 [Unsplash](https://unsplash.com/s/photos/random?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

随机性是一个基本的数学概念，通常也用在编程的上下文中。有时，当我们创建一些玩具数据时，或者当我们需要执行一些依赖于一些随机事件的特定计算时，我们可能需要引入一些随机性。

在今天的文章中，我们将首先讨论伪随机数和真随机数的概念。此外，我们将讨论 numpy 的`random.seed`以及如何使用它来创建可重复的结果。最后，我们将展示如何确保在整个代码中保持相同的种子。

## 理解伪随机性

伪随机数序列是使用确定性过程生成的序列，但是**看起来**是**统计随机的**。

即使由伪随机发生器(也称为确定性随机位发生器)产生的序列的性质近似于随机数序列的性质，但**实际上它们并不是真正随机的**。这是因为生成的序列由初始值决定，该初始值被称为**种子**。

您可以将种子视为序列的实际起点。伪随机数生成器基于对先前生成的值执行的一些处理和操作来生成每个数字。由于要生成的第一个值没有生成的可以对其执行这些操作的先前值，因此种子充当要生成的第一个数字的“先前值”。

## 使用 random.seed 创建可重复的结果

同样，NumPy 的随机数例程生成伪随机数序列。这是通过使用[位生成器](https://numpy.org/doc/stable/reference/random/bit_generators/generated/numpy.random.BitGenerator.html#numpy.random.BitGenerator)(生成随机数的对象)和[生成器](https://numpy.org/doc/stable/reference/random/generator.html#numpy.random.Generator)创建一个序列来实现的，这些生成器利用创建的序列从不同的概率分布(如正态分布、均匀分布或二项式分布)中进行采样。

现在，为了生成可再现的伪随机数序列，BitGenerator 对象接受一个用于设置初始状态的种子。这可以通过如下所示设置`[numpy.random.seed](https://numpy.org/doc/stable/reference/random/generated/numpy.random.seed.html)`来实现:

```
import numpy as np**np.random.seed(123)**
```

在不同的用例中，创建可重复的结果是一个常见的需求。例如，当测试某项功能时，您可能需要通过将种子配置为特定值来创建可重复的结果，以便可以将生成的结果与预期的结果进行比较。

此外，在更广泛的研究领域中，产生可重复的结果是常见的。例如，如果您使用一个使用随机性的模型(例如一个随机的森林)并想要发布结果(比如在一篇论文中)，那么您可能想要(并且可能已经)确保其他人和用户可以复制您所呈现的结果。

## 种子的局部效应

值得一提的是，NumPy 中的随机种子还会影响其他方法，例如`[numpy.random.permutation](https://numpy.org/doc/stable/reference/random/generated/numpy.random.permutation.html)`，它还会产生**局部效应**。

这意味着如果你只指定了`numpy.random.seed`一次，但是调用了`numpy.random.permutation`多次，你得到的结果将会不同(因为它们不依赖于同一个种子)。为了展示这个问题，让我们考虑下面的代码:

```
import numpy as npnp.random.seed(123)print(np.random.permutation(10))
***array([4, 0, 7, 5, 8, 3, 1, 6, 9, 2])***print(np.random.permutation(10))
***array([3, 5, 4, 2, 8, 7, 6, 9, 0, 1])***
```

如您所见，即使我们设置了随机种子，结果也是不可重复的。这是因为`random.seed`只有*‘局部效应’。*为了重现结果，您必须在每次调用`np.random.permutation`之前指定相同的随机种子，如下所示。

```
import numpy as npnp.random.seed(123)
print(np.random.permutation(10))
***array([4, 0, 7, 5, 8, 3, 1, 6, 9, 2])***np.random.seed(123)
print(np.random.permutation(10))
***array([4, 0, 7, 5, 8, 3, 1, 6, 9, 2])***
```

## 最后的想法

在今天的文章中，我们讨论了真或伪随机性的概念以及 NumPy 和 Python 中的`random.seed`的用途。此外，我们展示了如何在每次执行相同的代码时创建可重复的结果，即使结果依赖于一些(伪)随机性。最后，我们探讨了如何在需要时确保随机种子的效果在整个代码中得以维持。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

[](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [## 加快 PySpark 和 Pandas 数据帧之间的转换

### 将大火花数据帧转换为熊猫时节省时间

towardsdatascience.com](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [](/whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351) [## Python 中静态方法和类方法有什么区别？

### 关于 classmethod 和 staticmethod，您只需要知道

towardsdatascience.com](/whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351) [](https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39) [## 用于日常编程的 11 个 Python 一行程序

### 令人惊叹的 Python 片段不会降低可读性

better 编程. pub](https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39)