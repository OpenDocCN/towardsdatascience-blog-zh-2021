# 如何在熊猫中组合两个字符串列

> 原文：<https://towardsdatascience.com/combine-two-string-columns-pandas-fde0287485d9?source=collection_archive---------4----------------------->

## 了解如何在 pandas 数据框架中更有效地将两个字符串列连接成一个新列

![](img/fa7a1b7d56f2966da972b6cf6528d405.png)

帕斯卡·米勒在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

通过连接其他列来创建新列是一项相当常见的任务。在今天的简短指南中，我们将展示如何将 string DataFrame 列的内容连接成一个新列。我们将探讨几个不同的选项，您应该始终根据您正在处理的数据集大小来考虑这些选项。

这些方法中的一些在应用于小数据集时往往更有效，而其他方法在应用于大数据集时可能执行得更快。

此外，我们还将探索如何将字符串与非字符串(例如整数)列连接起来。

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示一些概念。

```
import pandas as pddf = pd.DataFrame(
  [
    (1, '2017', 10, 'Q1'),
    (2, '2017', 20, 'Q2'),
    (3, '2016', 35, 'Q4'),
    (4, '2019', 25, 'Q2'),
    (5, '2020', 44, 'Q3'),
    (6, '2021', 51, 'Q3'),
  ], 
  columns=['colA', 'colB', 'colC', 'colD']
)print(df)
 *colA  colB  colC colD
0     1  2017    10   Q1
1     2  2017    20   Q2
2     3  2016    35   Q4
3     4  2019    25   Q2
4     5  2020    44   Q3
5     6  2021    51   Q3*
```

## 在小型数据集中串联字符串列

对于相对较小的数据集(最多 100–150 行)，您可以使用`[pandas.Series.str.cat()](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.cat.html)`方法，该方法用于使用指定的分隔符(默认情况下分隔符设置为`''`)连接序列中的字符串。

例如，如果我们想将列`colB`和`colD`连接起来，然后将输出存储到一个名为`colE`的新列中，下面的语句就可以做到:

```
**df['colE'] = df.colB.str.cat(df.colD)** print(df)
 *colA  colB  colC colD* ***colE*** *0     1  2017    10   Q1* ***2017Q1*** *1     2  2017    20   Q2* ***2017Q2*** *2     3  2016    35   Q4* ***2016Q4*** *3     4  2019    25   Q2* ***2019Q2*** *4     5  2020    44   Q3* ***2020Q3*** *5     6  2021    51   Q3* ***2021Q3***
```

现在，如果我们想要指定一个将被放置在连接的列之间的分隔符，那么我们只需要传递`sep`参数:

```
**df['colE'] = df.colB.str.cat(df.colD, sep='-')**print(df)
 *colA  colB  colC colD* ***colE*** *0     1  2017    10   Q1* ***2017-Q1*** *1     2  2017    20   Q2* ***2017-Q2*** *2     3  2016    35   Q4* ***2016-Q4*** *3     4  2019    25   Q2* ***2019-Q2*** *4     5  2020    44   Q3* ***2020-Q3*** *5     6  2021    51   Q3* ***2021-Q3***
```

或者，你也可以使用列表理解，这种理解稍微有点冗长，但速度稍快:

```
**df['colE'] =** **[''.join(i) for i in zip(df['colB'], df['colD'])]**print(df)
 *colA  colB  colC colD* ***colE*** *0     1  2017    10   Q1* ***2017Q1*** *1     2  2017    20   Q2* ***2017Q2*** *2     3  2016    35   Q4* ***2016Q4*** *3     4  2019    25   Q2* ***2019Q2*** *4     5  2020    44   Q3* ***2020Q3*** *5     6  2021    51   Q3* ***2021Q3***
```

## 在较大的数据集中串联字符串列

现在，如果您正在处理大型数据集，连接两列的更有效方法是使用`+`操作符。

```
**df['colE'] = df['colB'] + df['colD']**print(df)
 *colA  colB  colC colD* ***colE*** *0     1  2017    10   Q1* ***2017Q1*** *1     2  2017    20   Q2* ***2017Q2*** *2     3  2016    35   Q4* ***2016Q4*** *3     4  2019    25   Q2* ***2019Q2*** *4     5  2020    44   Q3* ***2020Q3*** *5     6  2021    51   Q3* ***2021Q3***
```

如果您想要包含分隔符，只需将其作为字符串放在两列之间即可:

```
**df['colE'] = df['colB'] + '-' + df['colD']**
```

## 连接字符串和非字符串列

现在让我们假设您尝试连接的列之一不是字符串格式:

```
import pandas as pddf = pd.DataFrame(
  [
    (1, **2017**, 10, **'Q1'**),
    (2, **2017**, 20, **'Q2'**),
    (3, **2016**, 35, **'Q4'**),
    (4, **2019**, 25, **'Q2'**),
    (5, **2020**, 44, **'Q3'**),
    (6, **2021**, 51, **'Q3'**),
  ], 
  columns=['colA', 'colB', 'colC', 'colD']
)print(df.dtypes)*colA     int64
colB     int64
colC     int64
colD    object
dtype: object*
```

在这种情况下，您可以简单地使用`[pandas.DataFrame.astype()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.astype.html)`或`map()`方法来转换列。

```
# Option 1
**df['colE'] = df.colB.astype(str).str.cat(df.colD)**# Option 2
**df['colE'] = df['colB'].astype(str) + '-' + df['colD']**# Option 3
**df['colE'] =** **[
  ''.join(i) for i in zip(df['colB'].map(str), df['colD'])
]**
```

## 最后的想法

在今天的简短指南中，我们讨论了在 pandas 数据帧中连接字符串列。根据您正在使用的数据集的大小，您可能需要选择执行效率更高的最合适的方法。

此外，我们还展示了如何利用 pandas 中的`astype()`方法连接字符串和非字符串列。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒体上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership>  

**你可能也会喜欢**

</how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  </how-to-upload-your-python-package-to-pypi-de1b363a1b3> [## 如何将 Python 包上传到 PyPI

towardsdatascience.com](/how-to-upload-your-python-package-to-pypi-de1b363a1b3)