# Python Lambda 函数:三个实际例子(排序、映射和应用)

> 原文：<https://towardsdatascience.com/python-lambda-functions-three-practical-examples-sort-map-and-apply-286593792cb4?source=collection_archive---------21----------------------->

## 通过真实的例子学习 lambda 函数

![](img/cb01c7ec24d7f09d1e4529a9d948f82c.png)

照片由[托尔加·乌尔坎](https://unsplash.com/@tolga__?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 简介—λ函数

一个高级的 Python 概念是 lambda 函数，它是使用`lambda`关键字定义的匿名函数。Lambda 函数具有以下基本形式:

```
lambda params: expression
```

*   关键字`lambda`表示您正在定义一个 lambda 函数。
*   `params`指的是 lambda 函数将使用的参数。参数的数量可以是零到多个。
*   `expression`表示 lambda 函数将要运行的表达式。

就其核心而言，lambda 函数就像其他 Python 函数一样，可以使用 call 操作符(括号)来调用。下面的代码向您展示了相关的事实。

```
>>> multiply = lambda x, y: x*y
>>> type(multiply)
<class 'function'>
>>> multiply(5, 3)
15
>>> multiply(8, 5)
40
```

虽然我们把 lambda 函数赋给了变量`multiply`，这只是为了向你展示 lambda 是函数的事实。在实际项目中，强烈建议不要这样做，因为正如它的别名所知，lambdas 是匿名函数，应该用在不需要明确定义函数的地方。

在了解了如何定义 lambda 函数之后，让我们探索它的实际用例，以便更彻底地学习这种技术。

# 实际例子

## 示例 1:对自定义实例列表进行排序

通常，我们使用列表来保存同类数据——相同类型的对象。但是，这些对象可能不会按照特定需求的期望顺序排列。例如，一个列表可以包含一个论坛应用程序的一堆帖子。我们应该允许用户按照作者或者评论号来排序文章。在这些情况下，我们可以将`sort`方法与 lambda 函数一起使用。

使用 Lambda 排序

因为我们希望按照作者的姓氏对文章进行排序，所以 lambda 函数的表达式需要提取每次的姓氏。我们利用了 string 的 split 方法，它创建了一个字符串列表。因为姓氏是最后一项，所以我们使用-1 来检索它。

如你所见，这些帖子确实是按照作者的姓氏排序的。顺便提一下，sort 方法对列表中的项目进行就地排序，这意味着它改变了原始列表的顺序。

## 示例 2:绘制熊猫系列的数据(地图)

pandas 中的一个主要数据类型是`Series`类型，它表示一维数据，比如数据表中的一行或一列。当我们从一个系列开始时，我们可以使用 map 方法创建另一个系列。尽管可以使用 dictionary 对象来提供映射，但通常可以使用 lambda 函数。考虑下面的例子。

```
>>> import pandas as pd
>>> str_data = pd.Series(["95.2%", "92.7%", "93.4%"])
>>> str_data
0    95.2%
1    92.7%
2    93.4%
dtype: object
>>> float_data = str_data.map(lambda x: float(x[:-1]))
>>> float_data
0    95.2
1    92.7
2    93.4
dtype: float64
```

如上所示，我们从一个由多个百分比字符串数据组成的`Series`开始。但是，我们希望在删除百分号后提取这些字符串的数值。我们可以用一个 lambda 函数轻松完成转换:`lambda x: float(x[:-1])`。在这个函数中，`x`指的是`Series`中的每一项，我们使用`float`函数将字符串转换成相应的数值。

## 示例 3:从 Pandas 数据框架创建数据(应用)

pandas 中的另一个基本数据类型是`DataFrame`类型，它表示类似数据表的二维电子表格。带有`DataFrame`的 lambda 函数最常见的用例是`apply`方法，使用它，我们可以从现有的列创建新的数据列。让我们通过下面的例子来探索这种用法。

数据帧应用 Lambda

在示例中，我们使用 lambda 函数创建另一列`portion`。参数`x`指的是`DataFrame`中的每一行(因为我们设置了`axis=1`，这意味着操作是按行应用的)。如您所见，在操作之后，我们成功地计算出了具有相应百分比值的部分。

# 结论

在本文中，我们回顾了 lambda 函数的三个常见用例。本质上，lambda 函数应该用于执行不需要定义常规函数的小任务。

我希望你喜欢这篇文章。你可以在这里成为中级会员[来支持我，不需要额外付费。](https://medium.com/@yong.cui01/membership)