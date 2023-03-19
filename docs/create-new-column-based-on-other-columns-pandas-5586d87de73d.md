# 如何基于 Pandas 中其他列的值创建新列

> 原文：<https://towardsdatascience.com/create-new-column-based-on-other-columns-pandas-5586d87de73d?source=collection_archive---------0----------------------->

## 讨论如何从 pandas 数据框架中的现有列创建新列

![](img/90539da560d853d315f193edbd28a3db.png)

照片由[张秀坤镰刀](https://unsplash.com/@drscythe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/new?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 介绍

作为数据处理或特征工程的一部分，我们通常需要在现有列的基础上创建额外的列。在今天的简短指南中，我们将探索如何在熊猫身上进行这样的操作。

具体来说，我们将探索如何以几种不同的方式实现这一点，使用

*   `apply()`方法
*   `numpy.select()`方法(对于矢量化方法)
*   `loc`地产

首先，让我们创建一个示例 DataFrame，我们将在整篇文章中引用它来演示一些概念，并展示如何基于现有列的值创建新列。

```
import pandas as pd

df = pd.DataFrame(
    [
        (1, 'Hello', 158, True, 12.8),
        (2, 'Hey', 567, False, 74.2),
        (3, 'Hi', 123, False, 1.1),
        (4, 'Howdy', 578, True, 45.8),
        (5, 'Hello', 418, True, 21.1),
        (6, 'Hi', 98, False, 98.1),
    ],
    columns=['colA', 'colB', 'colC', 'colD', 'colE']
)
print(df)
   colA   colB  colC   colD  colE
0     1  Hello   158   True  12.8
1     2    Hey   567  False  74.2
2     3     Hi   123  False   1.1
3     4  Howdy   578   True  45.8
4     5  Hello   418   True  21.1
5     6     Hi    98  False  98.1
```

## 使用 apply()方法

如果您需要在现有列上应用一个方法，以便计算一些最终将作为新列添加到现有数据框架中的值，那么`[pandas.DataFrame.apply()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html)`方法应该可以做到。

例如，您可以定义自己的方法，然后将其传递给`apply()`方法。假设我们想要创建一个名为`colF`的新列，它将基于列`colC`的值使用下面定义的`categorise()`方法创建:

```
def categorise(row):  
    if row['colC'] > 0 and row['colC'] <= 99:
        return 'A'
    elif row['colC'] > 100 and row['colC'] <= 199:
        return 'B'
    elif row['colC'] > 200  and row['colC'] <= 299:
        return 'C'
    return 'D'
```

你需要做的就是将上述方法作为 lambda 表达式传递给`apply()`:

```
df['colF'] = df.apply(lambda row: categorise(row), axis=1)

print(df)
   colA   colB  colC   colD  colE colF
0     1  Hello   158   True  12.8    B
1     2    Hey   567  False  74.2    D
2     3     Hi   123  False   1.1    B
3     4  Howdy   578   True  45.8    D
4     5  Hello   418   True  21.1    D
5     6     Hi    98  False  98.1    A
```

对于更简单的操作，您可以将 lambda 表达式直接指定给`apply()`方法。例如，假设我们想要创建另一个名为`colG`的列，它将把列`colC`和`colE`的值加在一起。以下内容应该可以解决问题:

```
df['colG'] = df.apply(lambda row: row.colC + row.colE, axis=1)

print(df)
   colA   colB  colC   colD  colE colF   colG
0     1  Hello   158   True  12.8    B  170.8
1     2    Hey   567  False  74.2    D  641.2
2     3     Hi   123  False   1.1    B  124.1
3     4  Howdy   578   True  45.8    D  623.8
4     5  Hello   418   True  21.1    D  439.1
5     6     Hi    98  False  98.1    A  196.1
```

## 使用 NumPy 的 select()方法

现在，一种更矢量化的方法(在性能方面可能更好)是使用 NumPy 的`select()`方法，如下所述。

同样，假设我们想要创建一个名为`colF`的新列，它将基于列`colC`的值来创建。这一次，我们将创建一个包含所需条件的列表，而不是定义一个函数。

```
import numpy as np
conditions = [
  np.logical_and(df['colC'].gt(0), np.less_equal(df['colC'], 99)),
  np.logical_and(df['colC'].gt(100), np.less_equal(df['colC'],199)),
  np.logical_and(df['colC'].gt(200), np.less_equal(df['colC'],299)),
]
```

然后，我们定义一个额外的列表，其中包含新列将包含的相应值。注意，在下面的列表中，我们不包括默认值`D`。

```
outputs = ['A', 'B', 'C']
```

最后，我们使用`select()`方法来应用条件，并指定当指定的条件都不满足时将使用的默认值。

```
df['colF'] = pd.Series(np.select(conditions, outputs, 'D'))

print(df)
    colA   colB  colC   colD  colE colF
0     1  Hello   158   True  12.8    B
1     2    Hey   567  False  74.2    D
2     3     Hi   123  False   1.1    B
3     4  Howdy   578   True  45.8    D
4     5  Hello   418   True  21.1    D
5     6     Hi    98  False  98.1    A
```

## 使用 loc 属性

最后，另一个选择是`loc`属性，在某些情况下，它可能比`apply()`方法更有效。请注意，与我们之前讨论过的解决方案相比，这种方法可能会更加冗长。

```
df.loc[
  np.logical_and(df['colC'].gt(0), np.less_equal(df['colC'], 99)), 
  'colF'
] = 'A'
df.loc[
  np.logical_and(df['colC'].gt(100), np.less_equal(df['colC'], 199)),'colF'
] = 'B'
df.loc[
  np.logical_and(df['colC'].gt(200), np.less_equal(df['colC'], 299)),'colF'
] = 'C'
df['colF'].fillna('D', inplace=True)

print(df)
   colA   colB  colC   colD  colE colF
0     1  Hello   158   True  12.8    B
1     2    Hey   567  False  74.2    D
2     3     Hi   123  False   1.1    B
3     4  Howdy   578   True  45.8    D
4     5  Hello   418   True  21.1    D
5     6     Hi    98  False  98.1    A
```

## 最后的想法

在今天的简短指南中，我们讨论了根据现有列的值在 pandas 数据框架中添加新列。具体来说，我们展示了如何在 pandas 中使用`apply()`方法和`loc[]`属性来实现这一点，如果您对更加矢量化的方法感兴趣，还可以使用 NumPy 的`select()`方法。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

[](/sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a) [## spark session vs spark context vs SQLContext vs hive context

### SparkSession、SparkContext HiveContext 和 SQLContext 有什么区别？

towardsdatascience.com](/sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a) [](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [## 加快 PySpark 和 Pandas 数据帧之间的转换

### 将大型 Spark 数据帧转换为熊猫时节省时间

towardsdatascience.com](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [](https://medium.com/geekculture/how-to-refine-your-google-search-and-get-better-results-c774cde9901c) [## 如何优化你的谷歌搜索并获得更好的结果

### 更智能的谷歌搜索的 12 个技巧

medium.com](https://medium.com/geekculture/how-to-refine-your-google-search-and-get-better-results-c774cde9901c)