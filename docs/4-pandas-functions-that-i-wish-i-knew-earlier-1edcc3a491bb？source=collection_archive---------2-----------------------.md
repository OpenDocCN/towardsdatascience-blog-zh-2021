# 4 熊猫的功能，我希望我能早点知道

> 原文：<https://towardsdatascience.com/4-pandas-functions-that-i-wish-i-knew-earlier-1edcc3a491bb?source=collection_archive---------2----------------------->

## 你不需要重新发明轮子。

![](img/2e3b966dc3b5debc6ff1b784df48a130.png)

照片由 [kishan bishnoi](https://unsplash.com/@kishanbishnoi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果您在数据科学项目中使用 Python，那么 pandas 很可能是使用最多的库之一。对我来说，作为一名生物医学科学家，我几乎每天都用熊猫来处理和分析数据。时不时地，我发现我是在多此一举，因为我不知道存在可以轻松完成这项工作的相关函数。在这篇文章中，我想分享一些很多人在开始使用熊猫时不知道的功能。

## 1.最小和最大

在处理数值时，我们经常需要找到特定列的极值来进行数据质量检查。我们当然可以对值进行排序并选择最上面的行，如下所示。

重新发明轮子以获得极限记录

然而，使用`sort_values`来获得极端记录是重新发明轮子，因为 pandas 已经实现了具有更好性能的函数来实现相同的结果。下面的代码向您展示了这种用法。干净多了吧？

```
>>> df.nlargest(3, "Score")
    Student  Score
1  Jennifer    100
3    Joshua     99
0      John     95
>>> df.nsmallest(3, "Score")
  Student  Score
4    Jack     88
6   Jason     89
5  Jeremy     92
```

## 2.idxmin 和 idxmax

有一次，我需要按组找出最大的记录。例如，假设我们有以下数据帧。我想提取各自组中得分最高的记录。

示例数据帧

我的第一反应是将`groupby`函数与`max`函数一起使用，如下所示。虽然它告诉我每组的最高分，但它不包括我需要的其他列(例如，姓名)。也许我可以用这些最大值来过滤原始记录。我的直觉是这太复杂了。一定还有其他原因。

```
>>> df.groupby("group")["score"].max()
group
1     97
2     95
3    100
Name: score, dtype: int64
```

在我做了一些研究之后，我终于能够找到我正在寻找的解决方案——使用`idxmax`函数。这个函数是找出指定列的最大值的索引，通过这个索引，我们可以使用`loc`属性检索整个记录。下面的代码片段向您展示了这种用法。是不是整洁多了？

```
>>> record_indices = df.groupby("group")["score"].idxmax()
>>> record_indices
group
1    0
2    4
3    6
Name: score, dtype: int64
>>> df.loc[record_indices]
   group name  score
0      1   A1     97
4      2   B2     95
6      3   A3    100
```

以类似的方式，如果您对找出具有最小值的记录感兴趣，有一个对应的`idxmax` — `idxmin`，它专门用于检索最小值的索引。

## 3.qcut

当我们处理连续变量时，有时有必要根据现有数值的分位数创建序数值。理论上，有些人，包括我最初，可能倾向于找出某一列的分位数，并通过硬编码临界值来创建分类列。

然而，pandas 有一个简单的选项来实现这个操作——使用`qcut`函数，该函数根据它们的排名创建不同的值。使用我们在上一节中定义的 DataFrame，我们可以基于`Score`列对数据进行二分法，如下所示。

```
>>> df["score_dichotomized"] = pd.qcut(df["score"], 2, labels=['bottom50', 'top50'])
>>> df
   group name  score score_dichotomized
0      1   A1     97              top50
1      1   B1     93           bottom50
2      1   C1     92           bottom50
3      2   A2     94              top50
4      2   B2     95              top50
5      2   C2     93           bottom50
6      3   A3    100              top50
7      3   B3     92           bottom50
8      3   C3     93           bottom50
```

## 4.日期范围

当我处理日期数据时，我只是使用基本操作。例如，如果我需要创建一系列日期，我将只使用开始日期，并通过使用内置的`datetime`模块中的`timedelta`函数来创建额外的日期。

假设我需要创建 2021 年第一周的日期，作为数据帧的索引。这里有一个可能的解决方案。

```
>>> import datetime
>>> start_date = datetime.datetime(2021, 1, 1)
>>> first_week = [start_date + datetime.timedelta(days=x) for x in range(7)]
>>> pd.DatetimeIndex(first_week)
DatetimeIndex(['2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04', '2021-01-05', '2021-01-06', '2021-01-07'], dtype='datetime64[ns]', freq=None)
```

然而，有一个更好的方法——使用`date_range`功能。下面的代码片段向您展示了这种用法。

```
>>> pd.date_range(start='1/1/2021', periods=7)
DatetimeIndex(['2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04', '2021-01-05', '2021-01-06', '2021-01-07'], dtype='datetime64[ns]', freq='D')
```

## 结论

熊猫身上总是有很多值得发现的东西。当您发现当前的实现很麻烦时，有可能会有更好的解决方案来解决您的问题，可能只需要一个简单的函数调用。

继续发现熊猫！