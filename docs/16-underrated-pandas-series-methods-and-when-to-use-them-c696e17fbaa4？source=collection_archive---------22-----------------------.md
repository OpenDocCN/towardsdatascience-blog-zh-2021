# 16 种被低估的熊猫系列方法以及何时使用它们

> 原文：<https://towardsdatascience.com/16-underrated-pandas-series-methods-and-when-to-use-them-c696e17fbaa4?source=collection_archive---------22----------------------->

## Hasnans、pct_change、is_monotonic、repeat 以及许多其他

![](img/1e193e365e8eb3691a227144c51f35b5.png)

来自 [Pixabay](https://pixabay.com/photos/loveable-red-pandas-sichuan-1711019/)

在本文中，我们将探索一些鲜为人知但非常有用的 pandas 方法来操作系列对象。其中一些方法仅与系列相关，而其他方法则与系列和数据框架都相关，然而，当与两种结构类型一起使用时，它们具有特定的功能。

# 1.`is_unique`

顾名思义，该方法检查一个系列的所有值是否都是唯一的:

```
import pandas as pd
print(pd.Series([1, 2, 3, 4]).is_unique)
print(pd.Series([1, 2, 3, 1]).is_unique)**Output:**
True
False
```

# 2 & 3.`is_monotonic`和`is_monotonic_decreasing`

使用这两种方法，我们可以检查一个系列的值是否按升序/降序排列:

```
print(pd.Series([1, 2, 3, 8]).is_monotonic)
print(pd.Series([1, 2, 3, 1]).is_monotonic)
print(pd.Series([9, 8, 4, 0]).is_monotonic_decreasing)**Output:** True
False
True
```

这两种方法也适用于具有字符串值的序列。在这种情况下，Python 使用字典顺序，逐字符比较两个后续字符串。这不仅仅是字母排序，实际上，上面的数字数据的例子是这种排序的一个特例。正如 [Python 文档](https://docs.python.org/3/tutorial/datastructures.html#comparing-sequences-and-other-types)所说，

> *字符串的词典排序使用 Unicode 码位号来排序单个字符。*

实际上，这主要意味着还考虑了字母大小写和特殊符号:

```
print(pd.Series(['fox', 'koala', 'panda']).is_monotonic)
print(pd.Series(['FOX', 'Fox', 'fox']).is_monotonic)
print(pd.Series(['*', '&', '_']).is_monotonic)**Output:** True
True
False
```

当一个序列的所有值都相同时，会发生一个奇怪的异常。在这种情况下，两种方法都返回`True`:

```
print(pd.Series([1, 1, 1, 1, 1]).is_monotonic)
print(pd.Series(['fish', 'fish']).is_monotonic_decreasing)**Output:** True
True
```

# 4.`hasnans`

此方法检查序列是否包含 NaN 值:

```
import numpy as np
print(pd.Series([1, 2, 3, np.nan]).hasnans)
print(pd.Series([1, 2, 3, 10, 20]).hasnans)**Output:** True
False
```

# 5.`empty`

有时，我们可能想知道一个系列是否完全为空，甚至不包含 NaN 值:

```
print(pd.Series().empty)
print(pd.Series(np.nan).empty)**Output:** True
False
```

对序列进行一些操作后，序列可能会变空，例如过滤:

```
s = pd.Series([1, 2, 3])
s[s > 3].empty**Output:** True
```

# 6 & 7.`first_valid_index()`和`last_valid_index()`

这两种方法返回第一个/最后一个非 NaN 值的索引，对于具有许多 NaN 的系列对象特别有用:

```
print(pd.Series([np.nan, np.nan, 1, 2, 3, np.nan]).first_valid_index())
print(pd.Series([np.nan, np.nan, 1, 2, 3, np.nan]).last_valid_index())**Output:** 2
4
```

如果一个序列的所有值都是 NaN，两个方法都返回`None`:

```
print(pd.Series([np.nan, np.nan, np.nan]).first_valid_index())
print(pd.Series([np.nan, np.nan, np.nan]).last_valid_index())**Output:** None
None
```

# 8.`truncate()`

该方法允许截断某个索引值前后的序列。让我们截断上一节中的序列，只留下非 NaN 值:

```
s = pd.Series([np.nan, np.nan, 1, 2, 3, np.nan])
s.truncate(before=2, after=4)**Output:** 2    1.0
3    2.0
4    3.0
dtype: float64
```

该系列的原始索引被保留。我们可能希望重置它，并将截断的级数赋给一个变量:

```
s_truncated = s.truncate(before=2, after=4).reset_index(drop=True)
print(s_truncated)**Output:** 0    1.0
1    2.0
2    3.0
dtype: float64
```

# 9.`convert_dtypes()`

正如[熊猫文档](https://pandas.pydata.org/docs/reference/api/pandas.Series.convert_dtypes.html)所说，这种方法用于

> *使用支持* `*pd.NA*` *的数据类型将列转换为最佳数据类型。*

如果只考虑 Series 对象而不考虑 DataFrames，那么该方法的唯一应用是转换所有可空的整数(即小数部分等于 0 的浮点数，如 1.0、2.0 等)。)还原为“正常”整数。当原始序列同时包含整数和 NaN 值时，就会出现这种浮点数。因为 NaN 在 numpy 和 pandas 中是一个浮点型，所以它导致带有任何缺失值的整个系列也变成浮点型。

让我们看一下上一节中的例子，看看它是如何工作的:

```
print(pd.Series([np.nan, np.nan, 1, 2, 3, np.nan]))
print('\n')
print(pd.Series([np.nan, np.nan, 1, 2, 3, np.nan]).convert_dtypes())**Output:** 0    NaN
1    NaN
2    1.0
3    2.0
4    3.0
5    NaN
dtype: float640    <NA>
1    <NA>
2       1
3       2
4       3
5    <NA>
dtype: Int64
```

# 10.`clip()`

我们可以在输入阈值(`lower`和`upper`参数)处裁剪一个序列的所有值:

```
s = pd.Series(range(1, 11))
print(s)
s_clipped = s.clip(lower=2, upper=7)
print(s_clipped)**Output:** 0     1
1     2
2     3
3     4
4     5
5     6
6     7
7     8
8     9
9    10
dtype: int640    2
1    2
2    3
3    4
4    5
5    6
6    7
7    7
8    7
9    7
dtype: int64
```

# 11.`rename_axis()`

对于 Series 对象，此方法设置索引的名称:

```
s = pd.Series({'flour': '300 g', 'butter': '150 g', 'sugar': '100 g'})
print(s)
s=s.rename_axis('ingredients')
print(s)**Output:** flour     300 g
butter    150 g
sugar     100 g
dtype: objectingredients
flour     300 g
butter    150 g
sugar     100 g
dtype: object
```

# 12 & 13.`nsmallest()`和`nlargest()`

这两个方法返回一个序列的最小/最大元素。默认情况下，它们返回 5 个值，对`nsmallest()`按升序，对`nlargest()`按降序。

```
s = pd.Series([3, 2, 1, 100, 200, 300, 4, 5, 6])
s.nsmallest()**Output:** 2    1
1    2
0    3
6    4
7    5
dtype: int64
```

可以指定另一组要返回的最小/最大值。此外，我们可能希望重置索引并将结果赋给一个变量:

```
largest_3 = s.nlargest(3).reset_index(drop=True)
print(largest_3)**Output:** 0    300
1    200
2    100
dtype: int64
```

# 14.`pct_change()`

对于 Series 对象，我们可以计算当前元素和前一个元素之间的百分比变化(或者更准确地说，分数变化)。例如，当处理时间序列时，或者创建以百分比或分数表示的[瀑布图](https://medium.com/geekculture/creating-a-waterfall-chart-in-python-dc7bcddecb45?sk=3f4033acab6cbe98e0d20806ee8c46dd)时，这种方法会很有帮助。

```
s = pd.Series([20, 33, 14, 97, 19])
s.pct_change()**Output:** 0         NaN
1    0.650000
2   -0.575758
3    5.928571
4   -0.804124
dtype: float64
```

为了使结果系列更具可读性，让我们将其四舍五入:

```
s.pct_change().round(2)**Output:** 0     NaN
1    0.65
2   -0.58
3    5.93
4   -0.80
dtype: float64
```

# 15.`explode()`

该方法将一个序列(列表、元组、集、系列、ndarrays)中的每个类似列表的元素转换为一行。空名单-喜欢将在一排与南转换。为了避免结果序列中的重复索引，最好重置索引:

```
s = pd.Series([[np.nan], {1, 2}, 3, (4, 5)])
print(s)
s_exploded = s.explode().reset_index(drop=True)
print(s_exploded)**Output:** 0     [nan]
1    {1, 2}
2         3
3    (4, 5)
dtype: object0    NaN
1      1
2      2
3      3
4      4
5      5
dtype: object
```

# 16.`repeat()`

此方法用于将一个序列中的每个元素连续重复定义的次数。同样在这种情况下，重置索引是有意义的:

```
s = pd.Series([1, 2, 3])
print(s)
s_repeated = s.repeat(2).reset_index(drop=True)
print(s_repeated)**Output:** 0    1
1    2
2    3
dtype: int640    1
1    1
2    2
3    2
4    3
5    3
dtype: int64
```

如果重复次数被指定为 0，将返回一个空序列:

```
s.repeat(0)**Output:** Series([], dtype: int64)
```

# 结论

总之，我们研究了 16 种很少使用的 pandas 方法及其一些应用案例。如果你知道其他一些操纵熊猫系列的有趣方法，欢迎在评论中分享。

感谢阅读！

**你会发现这些文章也很有趣:**

[](/5-pandas-methods-youve-never-used-and-you-didn-t-lose-anything-37277fae7c55) [## 你从未用过的 5 种熊猫方法…而且你没有失去任何东西！

### 你知道他们到底什么时候能帮上忙吗？

towardsdatascience.com](/5-pandas-methods-youve-never-used-and-you-didn-t-lose-anything-37277fae7c55) [](/the-easiest-ways-to-perform-logical-operations-on-two-dictionaries-in-python-88c120fa0c8f) [## 在 Python 中对两个字典执行逻辑运算的最简单方法

towardsdatascience.com](/the-easiest-ways-to-perform-logical-operations-on-two-dictionaries-in-python-88c120fa0c8f) [](https://medium.com/geekculture/emojize-your-data-science-projects-8f19d447f03c) [## 你的数据科学项目🎭

### 如何让你的代码和讲故事更生动

medium.com](https://medium.com/geekculture/emojize-your-data-science-projects-8f19d447f03c)