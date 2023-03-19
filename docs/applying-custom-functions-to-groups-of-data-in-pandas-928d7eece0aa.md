# 对 Pandas 中的数据组应用自定义函数

> 原文：<https://towardsdatascience.com/applying-custom-functions-to-groups-of-data-in-pandas-928d7eece0aa?source=collection_archive---------0----------------------->

![](img/7cdd14ce19b0126e2fde781db9596c2e.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在这里，我将与您分享两种不同的方法来将自定义函数应用于 pandas 中的数据组。已经有许多现成的聚合和过滤功能可供我们使用，但这些功能并不总是能满足我们的所有需求。有时，当我想知道“每月第二个最近的观察结果是什么”或“按产品类型计算的两个加权平均值之间的差异是什么”时，我知道我需要定义一个自定义函数来完成这项工作。

## 样本数据(不要再说了……)

通常，我尽量不使用这个玩具数据集，因为它已经被使用了很多次，但它有一个很好的分类列，可以从中创建数据组，所以我打算使用它。[对于每一个分类，在每一列中有 4 个数字数据的特征来测量花的花瓣和萼片。](https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html)

## 聚合函数如何应用于组

通常，首轮分析可能包括检查数据集的描述性统计数据。如果您有一个正在特别查看的表，像“describe”方法这样的方法可以揭示关于列的一些关键信息，比如非空值的数量和数字特征的分布。一个稍微深一点的问题是“每组的这些描述性统计数据是什么？”。根据单个列中的相似值将数据分组在一起，并获取每个组中所有行的最大值，如下所示:

“刚毛”组的最大萼片长度(以厘米计)为 5.8。这很好，但是如果我们所关心的只是群体水平的描述性统计，那么我们也许可以给计算机编一个程序来处理所有的可能性并返回一些有趣的发现。我发现更经常出现的是需要对数据帧中的每个组应用自定义函数。接下来，我将创建一个定制函数，它不容易被 groupby 聚合函数复制。

## 定义自定义函数

这可能是任何事情。在这里，我感兴趣的是萼片长度与花瓣长度的比值。为什么？也许我有兴趣看看哪种花的萼片变异比花瓣变异大。谁知道呢。重点是，这是一个你想要回答的特定问题，通常你可以用最大值和最小值做一个分组，保存这些结果，得到比率。这是可能的，但是为什么不对组执行自定义函数呢？我们不局限于聚合，如果你愿意，你可以在数据帧的列中找到第二个值。

请注意，该函数使用括号符号显式命名数据帧中的列。您还可以创建应用于数据帧中所有列的自定义函数，并以我们稍后将看到的相同方式在组中使用它。

## 看看 Python 中的 Groupby 对象

注意，groupby 中的每个元素都是一个元组，第一个元素包含数据分组所依据的内容(可以是多列，在这种情况下是另一个元组)和分组的数据(一个 pandas dataframe)。这意味着无论在 groupby 元素上使用什么函数，都需要接受组和数据帧。

## 第一种方法

只需对 groupby 对象中的每个数据帧使用 apply 方法。这是最直接的方式，也最容易理解。请注意，该函数将数据帧作为唯一的参数，因此自定义函数中的任何代码都需要处理 pandas 数据帧。

```
data.groupby([‘target’]).apply(find_ratio)
```

也可以通过在每个数据帧上执行 lambda 函数来实现这一点:

```
data.groupby([‘target’]).apply(lambda x: find_ratio(x))
```

## 第二种方法

保存 groupby 元素，并对 groupby 对象中的每个元素执行该函数。第二个选项需要更多的工作，但可能是您在定制方面所寻找的。虽然我们之前直接将自定义函数应用于每个数据帧，但我们需要稍微重写函数，以接受 groupby 元素作为参数。但是首先，为要分组的列创建一个 groupby 对象，并为它分配一个变量名。接下来，重写函数以处理 groupby 元素中的每个 groupby。注意:groupby 对象是可迭代的(意味着 python 可以遍历它),并且包含分组级别和结果数据帧。

```
grouping = data.groupby([‘target’])
```

在循环中或使用列表理解对 groupby 元素执行函数。我选择在第一个例子中更新一个字典，在第二个例子中添加到一个列表中。每种方法都有其优点:

## 结论

这是将自定义函数应用于 pandas 中的数据组的两种方法。请，[在这里查看完整的笔记本](https://github.com/caseywhorton/custom-function-pandas-groups)。

## 参考

<https://github.com/caseywhorton/medium-blog-code>    <https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html> 