# 获取 Pandas 中具有特定列值的行的索引

> 原文：<https://towardsdatascience.com/rows-index-with-certain-column-value-pandas-1b25d382f0cb?source=collection_archive---------12----------------------->

## 了解如何在 pandas 数据帧中检索其列与特定值匹配的行的索引

![](img/d0a105e4a37f90187dedeb70fd060578.png)

[davisuko](https://unsplash.com/@davisuko?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/index?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

有时，我们可能需要检索其列匹配特定值的数据帧行的索引。在今天的简短指南中，我们将探索如何在熊猫身上做到这一点。

具体来说，我们将展示如何使用

*   熊猫数据框的`index`属性
*   和 NumPy 的`where()`方法

此外，我们还将讨论如何使用检索到的索引来执行常见任务。

首先，让我们创建一个示例数据框架，我们将在本文中引用它，以便围绕感兴趣的主题演示一些概念。

```
import pandas as pd df = pd.DataFrame(
    [
        (1, 100, 10.5, True),
        (2, 500, 25.6, False), 
        (3, 150, 12.3, False),
        (4, 100, 76.1, True),
        (5, 220, 32.1, True),
        (6, 100, 10.0, True),
    ],
    columns=['colA', 'colB', 'colC', 'colD']
)print(df)
 *colA  colB  colC   colD
0     1   100  10.5   True
1     2   500  25.6  False
2     3   150  12.3  False
3     4   100  76.1   True
4     5   220  32.1   True
5     6   100  10.0   True*
```

## 使用索引属性

访问索引的第一个选项是`[pandas.DataFrame.index](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.index.html)`属性返回熊猫数据帧的索引(即行标签)。

例如，假设我们想要检索所有行的索引，这些行在`colD`中的列值是`True`。下面的方法可以解决这个问题:

```
**idx = df.index[df['colD']]**print(idx)
*Int64Index([0, 3, 4, 5], dtype='int64')*
```

现在，如果你想将索引存储到一个列表中，你可以简单地使用`tolist()`方法:

```
**indices = df.index[df['colD']].tolist()**print(indices)
*[0, 3, 4, 5]*
```

## 使用 NumPy 的 where()方法

或者，您可以使用 NumPy 的`[where()](https://numpy.org/doc/stable/reference/generated/numpy.where.html)`方法，该方法返回一个包含基于指定条件选择的元素的`ndarray`。

例如，为了获得列值在`colB`中等于`100`的所有行的索引，下面的表达式将完成这个任务:

```
import numpy as np**indices = np.where(df['colB'] == 100)**print(indices)
*(array([0, 3, 5]),)*
```

## 利用检索到的索引

现在我们知道了如何获取特定行的索引，这些行的列匹配特定的值，您可能希望以某种方式利用这些索引。

例如，为了使用检索到的索引从 pandas DataFrame 中选择行，您可以使用`loc`属性和`Index`:

```
**idx = df.index[df['colD']]** **df_colD_is_true = df.loc[idx]** print(df_colD_is_true)
 *colA  colB  colC  colD
0     1   100  10.5  True
3     4   100  76.1  True
4     5   220  32.1  True
5     6   100  10.0  True*
```

或者，如果您有一些存储在列表中的索引，并且您想要直接使用它们来获取相应的行，您可以使用`iloc`属性:

```
**indices = df.index[df['colD']].tolist()** **df_colD_is_true =** **df.iloc[indices]**print(df_colD_is_true)
 *colA  colB  colC  colD
0     1   100  10.5  True
3     4   100  76.1  True
4     5   220  32.1  True
5     6   100  10.0  True*
```

## 最后的想法

在今天的简短指南中，我们讨论了如何检索其列与指定值匹配的行的索引。具体来说，我们展示了如何使用 pandas DataFrames 的`index`属性以及 NumPy 库的`where()`方法来获取索引。

此外，我们还探索了如何使用检索到的索引从 pandas 数据帧中获取实际的行。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒体上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership>  

**你可能也会喜欢**

</whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351>  </automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb>  </mastering-indexing-and-slicing-in-python-443e23457125> 