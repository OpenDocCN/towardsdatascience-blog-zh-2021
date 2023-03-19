# 如何获得熊猫数据帧的行数

> 原文：<https://towardsdatascience.com/how-to-get-the-row-count-of-a-pandas-dataframe-be67232ad5de?source=collection_archive---------3----------------------->

## 探讨如何更有效地获取熊猫数据帧的行数

![](img/db46be3ac49aa8dcf451d07310312d4c.png)

照片由[斯蒂夫·约翰森](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/count?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

在今天的简短指南中，我们将讨论几种计算熊猫数据帧行数的方法。此外，我们将展示如何在导出计数时忽略空值。最后，我们将观察本文中介绍的每种方法的性能，并确定计算行数的最有效方法。

首先，让我们创建一个示例数据框架，我们将在本指南中引用它来演示一些概念。

```
import pandas as pddf = pd.DataFrame({
    'colA':[1, 2, None, 4, 5], 
    'colB': [None, 'b', 'c', 'd', 'e'],
    'colC': [True, False, False, True, None],
})print(df)
   *colA  colB   colC
0   1.0  None   True
1   2.0     b  False
2   NaN     c  False
3   4.0     d   True
4   5.0     e   None*
```

## 使用 len()

计算数据帧行数最简单明了的方法是使用`len()`内置方法:

```
>>> **len(df)**
5
```

请注意，您甚至可以通过`df.index`来稍微提高性能(在本文的最后一节中有更多相关内容):

```
>>> **len(df.index)**
5
```

## 使用形状

或者，您甚至可以使用`[pandas.DataFrame.shape](https://pandas.pydata.org/pandas-docs/version/0.24.2/reference/api/pandas.DataFrame.shape.html)`，它返回一个表示数据帧维度的元组。元组的第一个元素对应于行数，而第二个元素表示列数。

```
>>> **df.shape[0]**
5
```

您也可以解包`df.shape`的结果并推断行数，如下所示:

```
>>> **n_rows, _ = df.shape**
>>> n_rows
5
```

## 使用 count()

在计算 pandas 中的行数时，第三个选项是`[pandas.DataFrame.count()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.count.html)`方法，即**返回非 NA 条目的计数**。

假设我们想要计算某一列下没有空值的所有行。以下内容应该对我们有用:

```
>>> **df[df.columns[1]].count()**
4
```

只有当您希望忽略空值时，才应该使用此方法。如果不是这种情况，那么您应该使用`len()`或`shape`。

## 表演

现在我们已经知道了一些计算数据帧中行数的不同方法，讨论它们对性能的影响会很有意思。为此，我们将创建一个比我们在本指南中使用的数据帧更大的数据帧。

```
import numpy as np
import pandas as pd df = pd.DataFrame(
    np.random.randint(0, 100, size=(10000, 4)),
    columns=list('ABCD')
)print(df)
       *A   B   C   D
0     61  38   2  39
1     96  65  20  47
2     35  56  97   8
3     71  31  80  25
4     20  63  99  34
   ..  ..  ..  ..
9995  96  81   6  43
9996  43  22  83  47
9997  62  92  42  26
9998  11  48  91  85
9999  55  47  77  66
[10000 rows x 4 columns]*
```

为了评估性能，我们将使用`[timeit](https://docs.python.org/3/library/timeit.html)`,这在对少量 Python 代码计时时很有用。

```
>>> **%timeit len(df)**
**548 ns** ± 24.6 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)>>> **%timeit len(df.index)**
**358 ns** ± 10.3 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)>>> **%timeit df.shape[0]
904 ns** ± 48 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)>>> %timeit df[df.columns[1]].count()
**81.9 µs** ± 4.91 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)
```

从上面的结果我们可以看出**熊猫中最高效的行数计算方法是** `**len()**` **方法。通过提供索引(** `**len(df.index)**` **)甚至更快。**

**效率最低的方法是** `**count()**`，因此，只有在需要从计数中排除空条目时，才应该使用这种方法。

## 最后的想法

在今天的文章中，我们讨论了如何使用

*   `len()`
*   `shape`
*   和`count()`

方法。注意`count()`应该只在你想忽略某些列的空行时使用。最后，我们讨论了这些方法在性能方面的差异。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</loc-vs-iloc-in-pandas-92fc125ed8eb>  <https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39> 