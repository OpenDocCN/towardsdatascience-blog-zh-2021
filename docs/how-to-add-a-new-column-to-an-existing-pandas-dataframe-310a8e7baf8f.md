# 如何向现有的 Pandas 数据框架添加新列

> 原文：<https://towardsdatascience.com/how-to-add-a-new-column-to-an-existing-pandas-dataframe-310a8e7baf8f?source=collection_archive---------1----------------------->

## 讨论在熊猫数据框中插入新列的 4 种方法

![](img/0fce96fbc03457ffbc869f348f15ff40.png)

[猪八戒](https://unsplash.com/@suetxxuan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/insert?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 介绍

在今天的简短指南中，我们将讨论向数据帧中添加新列的四种不同方法。具体来说，我们将探索如何

*   一次插入一列或多列
*   覆盖现有列
*   通过考虑索引来添加列
*   通过忽略索引来插入列
*   添加具有重复名称的列
*   在指定位置插入列

采用简单赋值、`insert()`、`assign()`和`concat()`的方法。

首先，让我们创建一个示例 DataFrame，我们将在本指南中引用它来演示一些与向 pandas 框架添加列相关的概念。

```
import pandas as pd df = pd.DataFrame({
    'colA':[True, False, False], 
    'colB': [1, 2, 3],
})print(df) *colA  colB
0   True     1
1  False     2
2  False     3*
```

最后，假设我们需要插入一个名为`colC`的新列，该列应该包含值`'a'`、`'b'`和`'c'`，分别表示索引`0`、`1`和`3`。

```
s = pd.Series(['a', 'b', 'c'], index=[0, 1, 2])
print(s)*0    a
1    b
2    c
dtype: object*
```

## 使用简单赋值

插入新列的最简单方法是简单地将`Series`的值分配到现有帧中:

```
**df['colC'] = s.values**print(df) *colA  colB colC
0   True     1    a
1  False     2    b
2  False     3    c*
```

请注意，假设新列的索引与数据帧的索引相匹配，上述方法适用于大多数情况，否则`NaN`值将被分配给缺失的索引。举个例子，

```
df['colC'] = pd.Series(['a', 'b', 'c'], index=[1, 2, 3])
print(df) *colA  colB colC
0   True     1  NaN
1  False     2    a
2  False     3    b*
```

## 使用 assign()

当您需要在一个数据帧中**插入多个新列**时，可以使用`[pandas.DataFrame.assign()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.assign.html)`方法；当您需要忽略要添加的列的索引时，可以使用**方法；当您需要覆盖现有列的值时，可以使用**或**方法。**

该方法将返回一个新的 DataFrame 对象(副本),其中包含所有原始列和新列:

```
e = pd.Series([1.0, 3.0, 2.0], index=[0, 2, 1])
s = pd.Series(['a', 'b', 'c'], index=[0, 1, 2])**df.assign(colC=s.values, colB=e.values)** colA  colB colC
0   True   1.0    a
1  False   3.0    b
2  False   2.0    c
```

永远记住用*分配*:

*   忽略要添加的列的索引
*   所有重新分配的现有列都将被覆盖

## 使用 insert()

或者，也可以使用`[pandas.DataFrame.insert()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.insert.html)`。这种方法通常在需要**在特定位置或索引**插入新列时有用。

例如，要将`colC`添加到数据帧的末尾:

```
**df.insert(len(df.columns), 'colC', s.values)**print(df)
 *colA  colB colC
0   True     1    a
1  False     2    b
2  False     3    c*
```

在`colA`和`colB`之间插入`colC`:

```
df.insert(1, 'colC', s.values)print(df) *colA colC  colB
0   True    a     1
1  False    b     2
2  False    c     3*
```

另外，`insert()`甚至可以用来添加一个重复的列名。默认情况下，当 DataFrame 中已经存在一列时，会引发一个`ValueError`:

```
df.insert(1, 'colC', s.values)
df.insert(1, 'colC', s.values)**ValueError: cannot insert colC, already exists**
```

但是，如果将`allow_duplicates=True`传递给`insert()`方法，DataFrame 将有两列同名:

```
df.insert(1, 'colC', s.values)
df.insert(1, 'colC', s.values, allow_duplicates=True)print(df)
 *colA colC colC  colB
0   True    a    a     1
1  False    b    b     2
2  False    c    c     3*
```

## 使用 concat()

最后，`[pandas.concat()](https://pandas.pydata.org/docs/reference/api/pandas.concat.html)`方法还可以用于通过传递`axis=1`将新列连接到 DataFrame。这个方法返回一个新的数据帧，它是连接的结果。

```
**df = pd.concat([df, s.rename('colC')], axis=1)**print(df)
 *colA  colB colC
0   True     1    a
1  False     2    b
2  False     3    c*
```

上述操作将使用索引将序列与原始数据帧连接起来。在大多数情况下，如果要连接的对象的索引彼此匹配，您应该使用`concat()`。如果索引不匹配，则每个对象的所有索引都将出现在结果中:

```
s = pd.Series(['a', 'b', 'c'], index=[10, 20, 30])
df = pd.concat([df, s.rename('colC')], axis=1)print(df)
 *colA  colB colC
0    True   1.0  NaN
1   False   2.0  NaN
2   False   3.0  NaN
10    NaN   NaN    a
20    NaN   NaN    b
30    NaN   NaN    c*
```

## 更改要添加的列的索引

向数据帧添加新列时，最棘手的部分之一是索引。您应该小心，因为我们在本指南中讨论的每种方法都可能以不同的方式处理索引。

如果由于任何原因，要添加的新列的索引没有任何特殊意义，并且您不希望在插入时考虑它，您甚至可以指定`Series`的索引与数据帧的索引相同。

```
s = pd.Series(['a', 'b', 'c'], **index=df.index)**
```

## 最后的想法

在今天的简短指南中，我们讨论了在 pandas 数据框架中插入新列或覆盖现有列的 4 种方法。我们已经看到了如何使用简单赋值、`assign()`、`insert()`和`concat()`方法来插入或覆盖新列。

此外，我们讨论了基于您想要实现的最终目标，您应该何时使用每种方法(例如，如果您想要忽略或考虑要添加的新列的索引)。

插入新列时，您必须选择最合适的方法，因为当新列和现有框架的索引不匹配时，每个列可能会有不同的行为。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351>  </whats-the-difference-between-shallow-and-deep-copies-in-python-ceee1e061926>  </6-tips-to-help-you-stand-out-as-a-python-developer-2294d15672e9> 