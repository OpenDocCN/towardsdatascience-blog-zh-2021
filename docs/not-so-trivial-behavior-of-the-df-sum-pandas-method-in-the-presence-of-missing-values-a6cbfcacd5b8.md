# 在缺少值的情况下，df.sum() Pandas 方法的行为非常重要

> 原文：<https://towardsdatascience.com/not-so-trivial-behavior-of-the-df-sum-pandas-method-in-the-presence-of-missing-values-a6cbfcacd5b8?source=collection_archive---------25----------------------->

## 一些潜在的问题以及如何解决它们

![](img/cc55a408be0da42f4e9d83a11c13737d.png)

来自 [Pixabay](https://pixabay.com/photos/abacus-beads-mathematics-numeral-3116200/)

尽管是最常用的 pandas 方法之一，`df.sum()`在数据集中有缺失值的情况下会变得很麻烦。在本文中，我们将看看它可能导致的一些潜在问题以及修复它们的方法。

让我们导入必要的库，并为我们的进一步实验创建一个带有许多缺失值的虚拟数据帧:

```
import pandas as pd
import numpy as npdf = pd.DataFrame({'A': [np.nan, 5, np.nan],
                   'B': [2, np.nan, np.nan],
                   'C': [np.nan, 6, np.nan],
                   'D': [True, False, np.nan]})
print(df)**Output:
**     A    B    C      D
0  NaN  2.0  NaN   True
1  5.0  NaN  6.0  False
2  NaN  NaN  NaN    NaN
```

现在，我们将按列(`axis=0`默认)和行(`axis=1`)计算值的总和:

```
print(f'Summing up by column: \n{df.sum()}\n\n'
      f'Summing up by row: \n{df.sum(axis=1)}')**Output:** Summing up by column:
A    5.0
B    2.0
C    6.0
D      1
dtype: object

Summing up by row:
0     3.0
1    11.0
2     0.0
dtype: float64
```

我们可以注意到两个可能的问题:

1.  在最后一列中，布尔值就像数字一样(`True` =1，`False` =0)，并被求和。
2.  在具有所有 NaN 值的最后一行中，NaN 的总和为 0.0。

让我们更详细地探讨这些观察结果。

# 1.布尔值就像数字一样，是相加的。

从“pythonic”的角度来看，布尔的这种行为没有任何问题。事实上:

```
True + False**Output:** 1
```

然而，在许多情况下，我们实际上只对实际数值求和感兴趣，而忽略了布尔值。让我们看看`numeric_only`参数是否有用:

```
print(f'Summing up by column: \n{df.sum()}\n\n'
      f'Summing up only numeric values by column: \n{df.sum(numeric_only=True)}')**Output:** Summing up by column:
A    5.0
B    2.0
C    6.0
D      1
dtype: object

Summing up only numeric values by column:
A    5.0
B    2.0
C    6.0
dtype: float64
```

我们看到，在第二种情况下，尽管在原生 Python 中有布尔的数值行为，但是`D`列被排除在计算之外。但是是因为布尔值吗？此外，这个列是布尔型的吗？

```
df.dtypes**Output:** A    float64
B    float64
C    float64
D     object
dtype: object
```

遗憾的是，要成为布尔数据类型，列必须只包含`True` / `False`值，不接受任何 nan。否则，该列将包含混合数据类型，并将自动转换为对象数据类型。事实上，在 Python 中，NaN 值实际上是一种浮点类型:

```
type(np.nan)**Output:** float
```

我们可以通过应用`convert_dtypes()`方法来解决这个问题，该方法使用支持`pd.NA`的 dtypes 将列转换为可能的最佳数据类型:

```
df1 = df.convert_dtypes()print(f'{df1}\n\n'
      f'Column data types: \n{df1.dtypes}\n\n'
      f'Summing up by column: \n{df1.sum()}\n\n'
      f'Summing up only numeric values by column: \n{df1.sum(numeric_only=True)}')**Output:
**      A     B     C      D
0  <NA>     2  <NA>   True
1     5  <NA>     6  False
2  <NA>  <NA>  <NA>   <NA>

Column data types:
A      Int64
B      Int64
C      Int64
D    boolean
dtype: object

Summing up by column:
A    5
B    2
C    6
D    1
dtype: int64

Summing up only numeric values by column:
A    5
B    2
C    6
D    1
dtype: int64
```

既然`D`列是布尔数据类型，那么`numeric_only`参数就没用了。似乎该参数在分配给`True`时，只能有助于过滤掉包含布尔值和 NaN 值的列。在包含字符串值的对象数据类型的列上，使用 vs .不使用此参数会产生以下不明显的影响:

```
df2 = pd.DataFrame({'A': [1, 1, 1, 1, 1],
                    'B': ['H', 'e', 'l', 'l', 'o'],
                    'C': ['W', 'o', 'r', 'l','d'],
                    'D': [1, 2, 3, 4, 5]})print(f'{df2}\n\n'
      f'Summing up by column: \n{df2.sum()}\n\n'
      f'Summing up only numeric values by column: \n{df2.sum(numeric_only=True)}')**Output:
**   A  B  C  D
0  1  H  W  1
1  1  e  o  2
2  1  l  r  3
3  1  l  l  4
4  1  o  d  5

Summing up by column:
A        5
B    Hello
C    World
D       15
dtype: object

Summing up only numeric values by column:
A     5
D    15
dtype: int64
```

# 2.在包含所有 nan 的 Series 对象中，值的总和为 0。

让我们回到最初的数据框架:

```
print(df)**Output:
**     A    B    C      D
0  NaN  2.0  NaN   True
1  5.0  NaN  6.0  False
2  NaN  NaN  NaN    NaN
```

对于我们在开始时概述的第二个问题:为什么最后一行中所有 NaN 值的总和为 0，而不是一个 NaN？事实上，这与 Python 中 NaN 和 NA 值的正常行为相矛盾，不仅是当它们相加在一起时，而且当我们试图向它们添加任何值时:

```
print(np.nan + np.nan)
print(np.nan + 1)
print(np.nan + True)
print(pd.NA + pd.NA)
print(pd.NA + np.nan)**Output:** nan
nan
nan
<NA>
<NA>
```

如果我们尝试手动对数据帧的所有列求和(即逐行求和)，我们将得到以下输出:

```
df['A'] + df['B'] + df['C'] + df['D']**Output:** 0    NaN
1    NaN
2    NaN
dtype: object
```

这是因为每行至少有一个 NaN 值。

然而，在之前的所有实验中，我们从未在输出中看到任何 NaN 值。显然在`df.sum()`方法中，有一个抑制 NaNs 影响的参数——`skipna`(默认为`True`):

```
print(f'Summing up by row excluding NaNs:\n{df.sum(axis=1)}\n\n'
      f'Summing up by row including NaNs:\n{df.sum(axis=1, skipna=False)}')**Output:** Summing up by row excluding NaNs:
0     3.0
1    11.0
2     0.0
dtype: float64

Summing up by row including NaNs:
0   NaN
1   NaN
2   NaN
dtype: float64
```

在第一种情况下，数据帧最后一行中的所有 nan 的总和为 0，这在现实生活中可能非常误导，并导致错误的统计数据，从而导致错误的见解。另一方面，在第二种情况下，将`skipna`参数赋给`False`并恢复值的正常“pythonic 式”行为，我们丢失了许多有效信息。事实上，在本例中，计算对一行中至少存在一个 NaN 变得非常敏感。有时候，这种方法正是我们所需要的。然而，在许多情况下，我们感兴趣的是考虑任何有效值，并且只考虑这样的值。为此，我们可以使用代表所需有效值数量的参数`min_count`来执行操作。为了考虑*任何*有效数据，我们必须将其赋值为 1:

```
print(f'Summing up by row excluding NaNs: \n{df.sum(axis=1)}\n\n'
      f'Summing up by row including NaNs: \n{df.sum(axis=1, skipna=False)}\n\n'
      f'Summing up by row considering any valid value: \n{df.sum(axis=1, min_count=1)}')**Output:** Summing up by row excluding NaNs:
0     3.0
1    11.0
2     0.0
dtype: float64

Summing up by row including NaNs:
0   NaN
1   NaN
2   NaN
dtype: float64

Summing up by row considering any valid value:
0     3.0
1    11.0
2     NaN
dtype: float64
```

最后一种方法给出了最有意义的输出。

当然，根据任务的不同，可以为这个参数指定另一个阈值，强制每行或每列都有最小数量的有效数据。

# 结论

在本文中，我们讨论了在缺少值的数据集上使用`df.sum()` pandas 方法的潜在复杂性:值的不可预测的、非 pythonic 行为的情况，列数据类型的隐式转换及其后果，有时会导致不良结果的方法的不明显的默认参数，以及如何防止它。

感谢阅读！

你会发现这些文章也很有趣:

</going-beyond-value-counts-creating-visually-engaging-frequency-tables-with-only-3-lines-of-code-3021c7756991>  </an-unconventional-yet-convenient-matplotlib-broken-barh-function-and-when-it-is-particularly-88887b76c127>  <https://levelup.gitconnected.com/when-a-python-gotcha-leads-to-wrong-results-2447f379fdfe> 