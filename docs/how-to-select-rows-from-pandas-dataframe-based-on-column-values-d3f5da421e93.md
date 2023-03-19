# 如何根据列值从 Pandas 数据框架中选择行

> 原文：<https://towardsdatascience.com/how-to-select-rows-from-pandas-dataframe-based-on-column-values-d3f5da421e93?source=collection_archive---------1----------------------->

## 探索如何根据 pandas 数据框架中的条件选择行

![](img/5304d4743c96a501520cd465318e7fdb.png)

[Teslariu Mihai](https://unsplash.com/@mihaiteslariu0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 介绍

从数据帧中选择行可能是使用 pandas 最常见的任务之一。在今天的文章中，我们将讨论如何对列值为:

*   等于标量/字符串
*   不等于标量/字符串
*   大于或小于一个标量
*   包含特定(子)字符串
*   可迭代的成员(例如列表)

此外，我们将讨论如何将多个条件组合在一起。

在开始讨论从 pandas 数据帧中选择行的不同方法之前，首先让我们创建一个示例数据帧来演示一些概念，该示例数据帧将贯穿本文。

```
import numpy as npdf = pd.DataFrame(
 [
  (73, 15, 55, 33, 'foo'),
  (63, 64, 11, 11, 'bar'),
  (56, 72, 57, 55, 'foo'),
 ],
 columns=['A', 'B', 'C', 'D', 'E'],
)print(df)#     A   B   C   D    E
# 0  73  15  55  33  foo
# 1  63  64  11  11  bar
# 2  56  72  57  55  foo
```

# 在熊猫中选择行

在下面几节中，我们将讨论并展示如何基于各种可能的条件从数据帧中选择特定的行。

## 选择列值等于标量或字符串的行

假设我们只想选择特定列中有一个特定值的行。我们可以通过简单地使用`loc[]`属性来做到这一点:

```
>>> df.loc[df['B'] == 64]
    A   B   C   D    E
1  63  64  11  11  bar
```

我们甚至可以省略`loc`并在索引熊猫数据帧时提供布尔条件，如下所示:

```
>>> df[df['B'] == 64]
    A   B   C   D    E
1  63  64  11  11  bar
```

注意，除了`df['B']`，您还可以使用`df.B`来引用列`B`。例如，下面的语句与上面的等价。

```
>>> df[df.B == 64]
```

在 Python 的上下文中，将这样的布尔条件命名为`mask`是一种常见的做法，然后我们在索引 DataFrame 时将它传递给 data frame。

```
>>> mask = df.B == 64
>>> df[mask]
    A   B   C   D    E
1  63  64  11  11  bar
```

在介绍了几种基于列值等于标量来选择行的可能方法之后，我们应该强调的是`loc[]`才是正确的选择。这仅仅是因为`**df[mask]**` **总是会分派到** `**df.loc[mask]**` **，这意味着直接使用** `**loc**` **会稍微快一点**。

## 选择列值不等于标量的行

接下来，您可能希望只选择特定列**中的值不等于标量**的行的子集。为此，我们只需遵循与之前相同的约定:

```
>>> df.loc[df['B'] != 64]
    A   B   C   D    E
0  73  15  55  33  foo
2  56  72  57  55  foo
```

## 选择行显示大于或小于标量的列值

这是一个与前一个非常相似的用例。在这种情况下，我们只需要使用所需的操作符来执行比较。举个例子，

```
>>> df.loc[df['B'] >= 64]
    A   B   C   D    E
1  63  64  11  11  bar
2  56  72  57  55  foo
```

## 选择列值包含字符串的行

最常见的一个用例是，当您需要过滤数据帧时，只保留特定列中包含特定(子)字符串的行。

和前面的用例一样，我们可以使用`loc`来实现:

```
>>> df.loc[df['E'].str.contains('oo')]
    A   B   C   D    E
0  73  15  55  33  foo
2  56  72  57  55  foo
```

## 选择列值在可迭代中的行

现在让我们假设我们只想选择那些特定列中的值包含在列表中的行。为此，我们只需使用如下所示的`isin()`。

```
>>> df.loc[df['B'].isin([64, 15])]
    A   B   C   D    E
0  73  15  55  33  foo
1  63  64  11  11  bar
```

## 选择列值不在 iterable 中的行

现在，如果你想选择值不在可迭代的行(例如，一个列表)，那么你可以简单地使用否定字符`~`:

```
>>> df.loc[~df['B'].isin([64, 15])]
    A   B   C   D    E
2  56  72  57  55  foo
```

## 多重条件

当从 pandas 数据框架中选择行时，您可能希望或必须将多个条件组合在一起。

为此，我们需要使用`&`操作符

```
>>> df.loc[(df['A'] >= 59) & (df['E'].isin(['foo', 'boo']))]
    A   B   C   D    E
0  73  15  55  33  foo
```

# 最后的想法

在今天的文章中，我们讨论了如何对 pandas 数据帧执行行选择，并展示了具体的用例及示例。

在大部分章节中，我们基本上使用了标签索引，这是通过`loc`实现的。请注意，pandas 有一个名为`iloc`的附加属性，通常用于执行位置索引。理解这两者之间的区别是很重要的，我建议你阅读下面的文章，它详细解释了每一个的作用。

[](/loc-vs-iloc-in-pandas-92fc125ed8eb) [## 熊猫中的 loc 与 iloc

towardsdatascience.com](/loc-vs-iloc-in-pandas-92fc125ed8eb)