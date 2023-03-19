# 如何对 Pandas 中的列应用函数

> 原文：<https://towardsdatascience.com/apply-function-to-columns-in-pandas-60b01c41c485?source=collection_archive---------7----------------------->

## 讨论在对 pandas 列应用函数时何时使用 apply()或 map()，以及如何更有效地执行

![](img/e00200f6dc4ea32bcb33726d85b54fe8.png)

照片由[Meagan car science](https://unsplash.com/@mcarsience_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/transform?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

当涉及到数据转换时，对 pandas 列应用特定的函数是一种非常常见的方法。在今天的简短指南中，我们将讨论如何对 pandas 数据帧中的一列或多列应用预定义函数或 lambda 函数。

此外，我们将讨论如何实现最佳性能，以及为什么在对多个列应用一个方法时使用`apply()` 方法更好，而在只处理一个列时使用`map()`方法更好。

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示一些概念。

```
import pandas as pddf = pd.DataFrame({
    'colA': [1, 2, 3, 4, 5],
    'colB': [True, False, None, False, True],
    'colC': ['a', 'b', 'c', 'd', 'e'],
    'colD': [1.0, 2.0, 3.0, 4.0, 5.0]
})print(df)
   *colA   colB colC  colD
0     1   True    a   1.0
1     2  False    b   2.0
2     3   None    c   3.0
3     4  False    d   4.0
4     5   True    e   5.0*
```

此外，让我们假设我们想给列`colA`下的每个值加 1。这可以使用 lambda 表达式轻松完成

```
lambda x: x + 1
```

## 对单个列应用函数

如果您想只对一列应用函数，那么`map()`是最好的选择。

```
**df['colA'] = df['colA'].map(****lambda x: x + 1****)**print(df)
***colA*** *colB colC  colD
0* ***2*** *True    a   1.0
1* ***3*** *False    b   2.0
2* ***4*** *None    c   3.0
3* ***5*** *False    d   4.0
4* ***6*** *True    e   5.0*
```

您也可以使用`apply()`方法，但是在对单个列应用方法时，`**map()**` **更有效。** `[**pandas.Series.map(**](https://pandas.pydata.org/docs/reference/api/pandas.Series.map.html)**)**` **方法对** `**Series**` **(即数据帧的单列)进行操作，一次操作一个单元格**。另一方面，`pandas.DataFrame.apply()`一次对整行进行操作。因此，使用`map()`会更有效，因为您只需要访问特定列的值(而不是整行的值)。

## 将函数应用于多列

另一方面，在需要对多个列应用某个函数的场合，那么您可能应该使用`[pandas.DataFrame.apply()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html)`方法。例如，假设我们需要对列`colA`和`colD`应用 lambda 函数`lambda x: x + 1`。以下内容应该可以解决问题:

```
**df[['colA', 'colD']] = df[['colA', 'colD']].apply(****lambda x: x + 1****)**print(df)
***colA*** *colB colC* ***colD*** *0* ***2*** *True    a* ***2.0*** *1* ***3*** *False    b* ***3.0*** *2* ***4*** *None    c* ***4.0*** *3* ***5*** *False    d* ***5.0*** *4* ***6*** *True    e* ***6.0***
```

在这种情况下，`map()`方法不能使用，因为它应该只在`Series`对象上操作(即在单个列上)。`apply()`方法将对整行应用指定的 lambda 函数，在本例中，整行由列`colA`和`colD`中的值组成。

## 最后的想法

在今天的简短指南中，我们讨论了如何在 pandas 中对列应用特定的函数。此外，我们探讨了为什么在将一个方法应用于单个列时使用`map()`方法很重要，而`apply()`应该只在处理多个列时使用。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

[](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [## 加快 PySpark 和 Pandas 数据帧之间的转换

### 将大火花数据帧转换为熊猫时节省时间

towardsdatascience.com](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [](/dynamic-typing-in-python-307f7c22b24e) [## Python 中的动态类型

### 探索 Python 中对象引用的工作方式

towardsdatascience.com](/dynamic-typing-in-python-307f7c22b24e) [](/mastering-indexing-and-slicing-in-python-443e23457125) [## 掌握 Python 中的索引和切片

### 深入研究有序集合的索引和切片

towardsdatascience.com](/mastering-indexing-and-slicing-in-python-443e23457125)