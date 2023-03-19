# 熊猫中的 loc 与 iloc

> 原文：<https://towardsdatascience.com/loc-vs-iloc-in-pandas-92fc125ed8eb?source=collection_archive---------12----------------------->

## Python 和熊猫中的 loc[]和 iloc[]有什么区别

![](img/e816f12c5b63be6afc0032ef9ae5b8d7.png)

照片由 [Nery Montenegro](https://unsplash.com/@neryfabiola_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/slice?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

索引和切片熊猫数据帧和 Python 有时可能很棘手。切片时最常用的两个属性是`iloc`和`loc`。

在今天的文章中，我们将讨论这两种属性之间的区别。我们还将通过几个例子来确保您理解何时使用一个而不是另一个。

首先，让我们创建一个熊猫数据框架，作为一个例子来演示一些概念。

```
import pandas as pddf = pd.DataFrame(
 index=[4, 6, 2, 1], 
 columns=['a', 'b', 'c'], 
 data=[[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]],
)print(df)#     a   b   c
# 4   1   2   3
# 6   4   5   6
# 2   7   8   9
# 1  10  11  12
```

## 使用 loc[]切片

`loc[]`属性用于对熊猫数据帧或系列进行切片，并通过标签访问行和列**。这意味着输入标签将对应于应该返回的行的索引。**

因此，如果我们将一个整数传递给`loc[]`，它**将被解释为索引的标签，而不是位置索引**。在下面的例子中，`loc`将返回索引标签等于`1`的行。

```
>>> df.loc[1]
a    10
b    11
c    12
Name: 1, dtype: int64
```

`loc`也接受标签数组:

```
>>> df.loc[[6, 2]]
   a  b  c
6  4  5  6
2  7  8  9
```

类似地，我们也可以使用 slice 对象来检索特定范围的标签。在下面的例子中，注意切片是如何计算的；`4:2`不对应于索引，而是对应于标签。换句话说，它告诉 pandas 返回索引`4`和`2`之间的所有行。

```
>>> df.loc[4:2]
   a  b  c
4  1  2  3
6  4  5  6
2  7  8  9
```

## 使用 iloc[]切片

另一方面，`iloc`属性提供了**基于整数位置的索引**，其中位置用于检索所请求的行。

因此，每当我们将一个整数传递给`iloc`时，您应该期望检索具有相应的**位置索引的行。**在下面的例子中，`iloc[1]`将返回位置`1`的行(即第二行):

```
>>> df.iloc[1]
a    4
b    5
c    6
Name: 6, dtype: int64# Recall the difference between loc[1]
>>> df.loc[1]
a    10
b    11
c    12
Name: 1, dtype: int64
```

同样，您甚至可以传递一个位置索引数组来检索原始数据帧的子集。举个例子，

```
>>> df.iloc[[0, 2]]
   a  b  c
4  1  2  3
2  7  8  9
```

或者甚至是整数的切片对象:

```
>>> df.iloc[1:3]
   a  b  c
6  4  5  6
2  7  8  9
```

`iloc`也可以接受一个可调用函数，该函数接受一个类型为`pd.Series`或`pd.DataFrame`的参数，并返回一个对索引有效的输出。

例如，为了只检索奇数索引的行，一个简单的 lambda 函数就可以做到:

```
>>> df.iloc[lambda x: x.index % 2 != 0]
    a   b   c
1  10  11  12
```

最后，您还可以使用`iloc`来索引两个轴。例如，为了获取前两条记录并丢弃最后一列，您应该调用

```
>>> df.iloc[:2, :2]
   a  b
4  1  2
6  4  5
```

## 最后的想法

在本文中，我们讨论了如何使用两个最常见的属性，即`loc`和`iloc`，正确地索引切片熊猫数据帧(或序列)。

理解这两个属性之间的差异，并能够有效地使用它们，以便为您的特定用例创建所需的输出，这一点非常重要。`loc`用于使用**标签索引熊猫数据帧或系列。**另一方面，`iloc`可用于根据记录的**位置索引**检索记录。

## 你可能也喜欢

</dynamic-typing-in-python-307f7c22b24e>  <https://medium.com/geekculture/how-to-refine-your-google-search-and-get-better-results-c774cde9901c>  </easter-eggs-in-python-f32b284ef0c5> 