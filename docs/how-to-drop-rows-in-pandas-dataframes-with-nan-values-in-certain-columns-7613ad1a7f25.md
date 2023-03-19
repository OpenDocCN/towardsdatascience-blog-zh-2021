# 如何删除 Pandas 数据帧中某些列中有 NaN 值的行

> 原文：<https://towardsdatascience.com/how-to-drop-rows-in-pandas-dataframes-with-nan-values-in-certain-columns-7613ad1a7f25?source=collection_archive---------0----------------------->

## 讨论从 pandas 数据帧中删除某些列中值为空的行的多种方法

![](img/c2759908b142651192d751e97d398751.png)

[Reed Mok](https://unsplash.com/@reeeed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/rows?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 介绍

在今天的简短指南中，我们将探索几种从 pandas 数据帧中删除某些列中有空值的行的方法。具体来说，我们将讨论如何删除带有以下内容的行:

*   至少有一列是`NaN`
*   所有列值都是`NaN`
*   具有空值的特定列
*   至少 N 列具有非空值

首先，让我们创建一个示例数据框架，我们将引用它来演示本文中的一些概念。

```
import pandas as pddf = pd.DataFrame({
    'colA':[None, False, False, True], 
    'colB': [None, 2, None, 4],
    'colC': [None, 'b', 'c', 'd'],
    'colD': [None, 2.0, 3.0, 4.0],
})print(df)
 *colA  colB  colC  colD
0   None   NaN  None   NaN
1  False   2.0     b   2.0
2  False   NaN     c   3.0
3   True   4.0     d   4.0*
```

## 删除至少有一个空值的所有行

当谈到删除熊猫数据帧中的空值时，`[pandas.DataFrame.dropna()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.dropna.html)`方法是你的朋友。当您在整个数据帧上调用`dropna()`而没有指定任何参数(即使用默认行为)时，该方法将删除至少有一个缺失值的所有行。

```
**df = df.dropna()**print(df)
 *colA  colB colC  colD
1  False   2.0    b   2.0
3   True   4.0    d   4.0*
```

## 删除只有缺失值的行

现在，如果您想删除所有列值都为空的行，那么您需要指定`how='all'`参数。

```
**df = df.dropna(how='all')**print(df)
 *colA  colB colC  colD
1  False   2.0    b   2.0
2  False   NaN    c   3.0
3   True   4.0    d   4.0*
```

## 删除特定列值为空的行

如果您想只考虑特定的列，那么您需要指定`subset`参数。

例如，让我们假设我们想要删除任何列`colA`或`colC`中缺少值的所有行:

```
**df = df.dropna(subset=['colA', 'colC'])**print(df)
 *colA  colB colC  colD
1  False   2.0    b   2.0
2  False   NaN    c   3.0
3   True   4.0    d   4.0*
```

此外，如果所有行在`colA`和`colB`中都缺少值，您甚至可以删除所有行:

```
**df = df.dropna(subset=['colA', 'colB'], how='all')**print(df)
 *colA  colB colC  colD
1  False   2.0    b   2.0
2  False   NaN    c   3.0
3   True   4.0    d   4.0*
```

## 删除至少有 N 个非缺失值的行

最后，如果您需要删除至少有 N 个非缺失值列的所有行，那么您需要指定`**thresh**` **参数，该参数指定为了不被删除，每一行应该存在的非缺失值的数量**。

例如，如果您想要删除所有具有多个空值的列，那么您需要将`thresh`指定为`len(df.columns) — 1`

```
**df = df.dropna(thresh=len(df.columns)-1)**print(df)
 *colA  colB colC  colD
1  False   2.0    b   2.0
2  False   NaN    c   3.0
3   True   4.0    d   4.0*
```

## 最后的想法

在今天的简短指南中，我们讨论了在 pandas 数据帧中删除缺少值的行的 4 种方法。

请注意，除了`pandas.DataFrame.dropna()`之外，您可以使用许多不同的方法(例如 `numpy.isnan()`方法)来删除行(和/或列)，后者是专门为 pandas 构建的，与更通用的方法相比，它具有更好的性能。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒介上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</how-to-add-a-new-column-to-an-existing-pandas-dataframe-310a8e7baf8f>  <https://medium.com/geekculture/use-performance-visualization-to-monitor-your-python-code-f6470592a1cb>  </how-to-upload-your-python-package-to-pypi-de1b363a1b3> [## 如何将 Python 包上传到 PyPI

towardsdatascience.com](/how-to-upload-your-python-package-to-pypi-de1b363a1b3)