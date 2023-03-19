# 如何在 Pandas 中组合数据——你应该知道的 5 个功能

> 原文：<https://towardsdatascience.com/how-to-combine-data-in-pandas-5-functions-you-should-know-651ac71a94d6?source=collection_archive---------8----------------------->

## 了解它们的不同之处，并正确使用它们

![](img/0fefba4652903b434fd182b6398e4c60.png)

Hendrik Cornelissen 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我们使用 Pandas 处理数据时，一个常见的任务是组合来自不同来源的数据。在本文中，我将回顾您可以用于数据合并的 5 个 Pandas 函数，如下所列。如果你想看的话，每个函数都有到官方文档的链接。

```
* [concat](https://pandas.pydata.org/docs/reference/api/pandas.concat.html)
* [join](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.join.html#pandas.DataFrame.join)
* [merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html#pandas.DataFrame.merge)
* [combine](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.combine.html)
* [append](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.append.html#pandas.DataFrame.append)
```

对于当前教程，让我们使用两个简单的`DataFrame`对象，如下所示。请注意，为了更好地说明相应的功能，我将在适当的地方做一些小小的修改。

用于说明的数据帧

## 1.串联

`concat`函数以串联命名，它允许您水平或垂直地并排组合数据。

当您组合具有相同列的数据时(或者实际上它们中的大多数是相同的)，您可以通过将`axis`指定为 0 来调用`concat`，这实际上也是默认值。

```
>>> pd.concat([df0, df1.rename(columns={"c": "a", "d": "b"})], axis=0)
   a  b
0  1  4
1  2  5
2  3  6
0  2  5
1  3  6
2  4  7
```

当您将数据与指示相同实体的行(即，相同有序主题的研究数据)和不同数据的列组合时，您可以将它们并排连接。

```
>>> pd.concat([df0, df1], axis=1)
   a  b  c  d
0  1  4  2  5
1  2  5  3  6
2  3  6  4  7
```

默认情况下，当您水平组合数据时(即沿着列)，Pandas 会尝试使用索引。当它们不相同时，您会看到 nan 来填充不重叠的部分，如下所示。

```
>>> df2 = df1.copy()
>>> df2.index = [1, 2, 3]
>>> pd.concat([df0, df2], axis=1)
     a    b    c    d
0  1.0  4.0  NaN  NaN
1  2.0  5.0  2.0  5.0
2  3.0  6.0  3.0  6.0
3  NaN  NaN  4.0  7.0
```

如果这不是期望的行为，并且您希望通过忽略索引来使它们完全对齐，则应该在连接之前重置索引:

```
>>> pd.concat([df0.reset_index(drop=True), df2.reset_index(drop=True)], axis=1)
   a  b  c  d
0  1  4  2  5
1  2  5  3  6
2  3  6  4  7
```

请注意，`df0`的重置索引是可选的，因为它的索引恰好是从 0 开始的，这将与`df2`的重置索引相匹配。

## 2.加入

与`concat`相比，`join`专门使用索引连接`DataFrame`对象之间的列。

```
>>> df0.join(df1)
   a  b  c  d
0  1  4  2  5
1  2  5  3  6
2  3  6  4  7
```

当索引不同时，默认情况下，连接保持左边的行`DataFrame`。从右边`DataFrame`开始，左边`DataFrame`中没有匹配索引的行被删除，如下所示:

```
>>> df0.join(df2)
   a  b    c    d
0  1  4  NaN  NaN
1  2  5  2.0  5.0
2  3  6  3.0  6.0
```

但是，可以通过设置 how 参数来更改此行为。可用选项有:左、右、外和内，它们的行为如下所示。

```
**# "right" uses df2’s index**
>>> df0.join(df2, how="right")
     a    b  c  d
1  2.0  5.0  2  5
2  3.0  6.0  3  6
3  NaN  NaN  4  7**# "outer" uses the union**
>>> df0.join(df2, how="outer")
     a    b    c    d
0  1.0  4.0  NaN  NaN
1  2.0  5.0  2.0  5.0
2  3.0  6.0  3.0  6.0
3  NaN  NaN  4.0  7.0**# "inner" uses the intersection**
>>> df0.join(df2, how="inner")
   a  b  c  d
1  2  5  2  5
2  3  6  3  6
```

## 3.合并

如果你有这样的经验，那么`merge`函数很像是在数据库中加入动作。相比 join，merge 更通用，可以对列和索引执行合并操作。因为我们已经介绍了使用基于索引的合并的`join`,所以我们将更加关注基于列的合并。让我们看一个简单的例子。

```
>>> df0.merge(df1.rename(columns={"c": "a"}), on="a", how="inner")
   a  b  d
0  2  5  5
1  3  6  6
```

`on`参数定义了两个`DataFrame`对象将在哪些列上合并。请注意，您可以将单个列指定为字符串，也可以在一个列表中指定多个列。这些列必须出现在两个`DataFrame`对象中。更一般地，您可以分别从左边的`DataFrame`和右边的`DataFrame`指定合并列，如下所示。

```
>>> df0.merge(df1, left_on="a", right_on="c")
   a  b  c  d
0  2  5  2  5
1  3  6  3  6
```

除了单独的列`a`和`c`之外，这与之前的合并结果基本相同。

有许多其他的可选参数可以用来进行合并。我想在这里强调两点。

*   `how`:定义要执行的合并类型。支持的类型包括左、右、内(默认)、外和交叉。一切都应该很简单，除了最后一个十字，它从两个帧创建笛卡尔乘积，如下所示。

```
>>> df0.merge(df1, how="cross")
   a  b  c  d
0  1  4  2  5
1  1  4  3  6
2  1  4  4  7
3  2  5  2  5
4  2  5  3  6
5  2  5  4  7
6  3  6  2  5
7  3  6  3  6
8  3  6  4  7
```

*   后缀:当两个 DataFrame 对象具有除了要合并之外的相同列时，此参数设置应该如何使用后缀将这些列重命名为。默认情况下，左右数据框的后缀为`“_x”`和`“_y”`，您可以自定义后缀。这里有一个例子。

```
>>> df0.merge(df1.rename(columns={"c": "a", "d": "b"}), on="a", how="outer", suffixes=("_l", "_r"))
   a  b_l  b_r
0  1  4.0  NaN
1  2  5.0  5.0
2  3  6.0  6.0
3  4  NaN  7.0
```

## 4.结合

`combine`函数执行两个`DataFrame`对象之间的列式合并，与之前的函数有很大不同。`combine`的特别之处在于它带了一个函数参数。该函数采用两个`Series`,每个对应于每个数据帧中的合并列，并返回一个`Series`,作为相同列的元素操作的最终值。听起来很困惑？让我们看看下面代码片段中的一个例子。

组合数据

`taking_larger_square`功能作用于`df0`和`df1` 中的`a`列和`df0`和`df1`中的`b`列。在两个`a`和两个`b`列之间，`taking_larger_square`从较大的列中取值的平方。在这种情况下，`df1`的列`a`和`b`将被视为正方形，这将产生如上面的代码片段所示的最终值。重命名后的`df1`如下图所示，供您参考。

```
>>> df1.rename(columns={"c": "a", "d": "b"})
   a  b
0  2  5
1  3  6
2  4  7
```

虽然这里没有涉及，但是还有另一个密切相关的函数`combine_first`，它只是使用第一个帧的非空值来合并两个帧。

## 5.附加

到目前为止，我们讨论的大多数操作都是针对按列组合数据的。行方式操作怎么样？append 函数专门用于将行追加到现有的`DataFrame`对象，创建一个新的对象。我们先来看一个例子。

```
>>> df0.append(df1.rename(columns={"c": "a", "d": "b"}))
   a  b
0  1  4
1  2  5
2  3  6
0  2  5
1  3  6
2  4  7
```

上面的操作你看着眼熟吗？我希望如此，因为这就像你设置`axis=0`时用`concat`可以实现的一样。然而，append 的独特之处在于，您可以实际追加一个`dict`对象，这为我们提供了追加不同类型数据的灵活性。

```
>>> df0.append({"a": 1, "b": 2}, ignore_index=True)
   a  b
0  1  4
1  2  5
2  3  6
3  1  2
```

上面显示了一个简单的例子。请注意，您必须将`ignore_index`设置为`True`，因为字典对象没有数据帧可以使用的索引信息。

## 结论

在这篇文章中，我们回顾了熊猫的 5 个最常用的数据合并函数。这里有一个快速回顾。

*   `concat`:按行和按列组合数据
*   `join`:使用索引按行组合数据
*   `merge`:按列组合数据，就像数据库连接操作一样
*   `combine`:按列组合数据，进行列间(相同列)元素操作
*   `append`:以`DataFrame`或`dict`对象的形式逐行追加数据

感谢阅读这篇文章。通过[注册我的简讯](https://medium.com/subscribe/@yong.cui01)保持联系。还不是中等会员？[用我的会员链接](https://medium.com/@yong.cui01/membership)支持我的写作。