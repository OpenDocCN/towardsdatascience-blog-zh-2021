# 关于熊猫团购要知道的八件事

> 原文：<https://towardsdatascience.com/8-things-to-know-to-get-started-with-with-pandas-groupby-3086dc91acb4?source=collection_archive---------27----------------------->

## Groupby 是如此强大，这对于初学者来说听起来可能令人畏缩，但您不必了解它的所有特性

![](img/9fadaea0684b7dee21f107a957c5ab41.png)

[哈德森辛慈](https://unsplash.com/@hudsonhintze?utm_source=medium&utm_medium=referral)在[unplash](https://unsplash.com?utm_source=medium&utm_medium=referral)上的照片

没有必要证明熊猫图书馆在数据科学领域的重要性。如果使用 Python，这个库是任何数据处理作业的必备工具。熊猫库的一个通用功能是基于`groupby`函数，它创建了一个`GroupBy`对象，支持许多可能的操作。

然而，`groupby`相关的功能是如此强大，以至于我们中的许多人很难记住它的所有功能。此外，通用性的另一个显著副作用是，初学者可能会发现自己迷失在如何使用`groupby`功能中。在本文中，我想回顾一下我们可以对`GroupBy`对象执行的 8 个最常见的操作。

## 1.创建频率表

让我们首先检索我们要使用的数据集(著名的 iris 数据集)，如下所示。

Iris 数据集

当我们拥有包含分类变量的数据记录时，我们通常希望知道按组划分的记录数。在这种情况下，我们可以使用`GroupBy`的`size()`功能创建频率表。如你所见，我们对这三个物种都有 50 个记录。

频率表

需要注意的一点是，我们创建了`GroupBy`对象(即`iris_gb`，因为接下来我们将多次使用它。

## 2.计算通用描述性统计信息(例如，平均值、最小值、最大值)

要按组计算平均值，我们只需使用`mean()`函数。

计算方式

默认情况下，`mean()`函数将对所有数值列执行计算。如果需要某些列的平均值，我们可以指定该值，如下所示。

列的平均值

以类似的方式，我们可以使用`min()`和`max()`函数计算`GroupBy`对象中各组的最小值和最大值，就像`mean()`函数一样。我们还可以计算中位数和标准差。

```
**# Calculate the min**
iris_gb.min()
**# Calculate the max**
iris_gb.max()
**# Calculate the median**
iris_gb.median()
**# Calculate the SD**
iris_gb.std()
```

## 3.寻找最大值(或最小值)的索引

当你想找出每个组的最大记录的索引，而不是定位最大记录并找到它的索引时，有一个方便的函数直接为我们做了这项工作。见下文。

最大记录索引

这一功能可能很有用。例如，假设我们想要检索最大萼片长度的记录，我们可以执行以下操作:

组合 idxmax 和 loc

## 4.Groupby 后重置索引

有时，我们希望在`groupby`处理之后进行额外的操作。更具体地说，我们希望重置分组索引，使它们成为规则的行和列。第一个选项是在创建的`DataFrame`对象上使用`reset_index()`函数。

重置索引

通过在`groupby`函数中设置`as_index`参数，第二个选项更加友好。

重置 groupby 函数中的索引

## 5.多个操作的简单聚合

从技术上来说，我们可以如上图分别执行多个操作。然而，可以通过利用`GroupBy`对象的`agg()`函数来执行这些操作。考虑下面这个微不足道的例子。

聚合

*   为了清楚起见，我们只计算两列的结果:`sepal_length`和`petal_length`。
*   在`agg`函数中，我们指定想要应用的函数的名称。在这种情况下，我们要求计算两列的最小值和平均值。

## 6.特定于列的聚合

当我们想要聚合一些计算时，我们并不总是需要对所有列进行相同的计算。在这种情况下，我们可以为不同的列请求不同的计算。请考虑以下情况:

按列的不同聚合

在上面的示例中，我们看到聚合计算了`sepal_length`的最小值和最大值以及`petal_length`的平均值和标准差。语法基本上是传递一个字典，其中列是键，计算函数是值。

## 7.命名聚合

您可能已经注意到，由于涉及到多级索引，输出文件不是太用户友好。与此相关，计算字段没有非常简单的名称。为了解决这个问题，我们可以考虑命名聚合特性，如下所示。

命名聚合

上面的代码利用了 pandas 中实现的`NamedAgg`。如您所见，四个计算列具有正确的名称。值得注意的是，`NamedAgg`是一种命名元组，因此我们可以直接传递元组作为快捷方式。等效的代码如下。

```
iris_gb.agg(
    sepal_min=("sepal_length", "min"),
    sepal_max=("sepal_length", "max"),
    petal_mean=("petal_length", "mean"),
    petal_std=("petal_length", "std")
)
```

## 8.使用自定义函数

到目前为止，我们在计算中应用的函数是通过名称传递的(例如，“min”、“mean”)。然而，我们可以直接传递一个函数对象。请考虑以下情况。

功能对象

值得注意的是，函数对象可以和函数名一起使用。下面的代码也是有效的调用。

```
iris_gb.agg(["min", pd.Series.mean])
```

此外，您可以指定自定义函数。下面的代码片段向您展示了一个简单的例子。

自定义功能

由于 lambda 函数的核心也是函数，我们可以对`GroupBy`对象使用 lambda 函数，如下所示。代码相当于上面的代码片段。

```
**# Use a lambda function**
iris_gb.agg(lambda x: x.mean())
```

## 结论

在本文中，我们回顾了在 pandas 中使用 groupby 函数对`GroupBy`对象进行的八个最常见的操作。当然，它并不是`GroupBy`对象可用特性的详尽列表。相反，它们只是一些与大多数日常数据处理需求相关的重要问题。