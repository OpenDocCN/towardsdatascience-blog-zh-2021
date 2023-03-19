# 如何重命名 Pandas 中的列

> 原文：<https://towardsdatascience.com/how-to-rename-columns-in-pandas-d35d13262c4f?source=collection_archive---------23----------------------->

## 重命名熊猫数据框架中的列

![](img/cc59e54a9631f37ad1d05c5c1eff22dc.png)

照片由[夏羽·亚伊奇](https://unsplash.com/@stefyaich?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

重命名 dataframe 列是一种常见的做法，尤其是当我们希望与其他人和团队分享一些见解时。这意味着我们可能希望使列名更有意义，以便读者更容易将它们与特定的上下文联系起来。

在这篇短文中，我们将看看在重命名熊猫数据帧的列时有哪些选择。具体来说，我们将了解如何重命名列:

*   使用`rename()`方法
*   通过更新`DataFrame.columns`属性
*   并且使用`set_axis()`的方法

首先，让我们创建一个示例数据框架，它将在本指南中引用，以展示所需的 pandas 功能。

```
import pandas as pddf = pd.DataFrame({
    'colA':[1, 2, 3], 
    'colB': ['a', 'b', 'c'],
})print(df)
#    colA colB
# 0     1    a
# 1     2    b
# 2     3    c
```

## 使用。重命名()

`[**pandas.DataFrame.rename()**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html)` **可用于改变列名或索引名**。

> 改变坐标轴标签。
> 
> 函数/字典值必须是唯一的(一对一)。未包含在`dict` / `Series`中的标签将保持原样。

为了使用`rename()`方法重命名列，我们需要提供一个映射(即字典),其中键是旧的列名，值是新的列名。此外，我们必须指定`axis=1`来表示我们希望重命名列而不是索引:

```
**df = df.rename({'colA': 'A', 'colB': 'B'}, axis=1)**print(df)
#    A  B
# 0  1  a
# 1  2  b
# 2  3  c
```

## 更新 df.columns 属性

pandas 数据帧带有`[pandas.DataFrames.columns](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.columns.html)`属性，该属性是包含数据帧的列标签的`Index` c **。**

我们可以通过重新分配该特定属性来重命名 DataFrame 列，如下所示:

```
**df.columns = ['column_A', 'column_B']**print(df)
#    column_A column_B
# 0         1        a
# 1         2        b
# 2         3        c
```

## 使用 set_axis()

`[pandas.DataFrame.set_axis()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.set_axis.html)`方法可用于为列或索引轴分配所需的索引。为了重命名列名，请确保提供如下所示的`axis=1`:

```
**df = df.set_axis(['AA', 'BB'], axis=1, inplace=False)**print(df)
#    AA BB
# 0   1  a
# 1   2  b
# 2   3  c
```

请注意，在前面讨论的所有示例中，您甚至可以使用`axis='columns'`代替`axis=1`来表示操作应该在列级别有效。例如，

```
df = df.rename({'colA': 'A', 'colB': 'B'}, axis='columns')
df = df.set_axis(['AA', 'BB'], axis='columns')
```

## 最后的想法

在今天的简短指南中，我们讨论了如何以几种不同的方式重命名熊猫数据帧的列。

您可能还有兴趣了解如何更改熊猫数据框架特定列的数据类型。

</how-to-change-column-type-in-pandas-dataframes-d2a5548888f8>  

此外，下面的文章讨论了如何根据具体情况进行适当的行选择。

</how-to-select-rows-from-pandas-dataframe-based-on-column-values-d3f5da421e93> 