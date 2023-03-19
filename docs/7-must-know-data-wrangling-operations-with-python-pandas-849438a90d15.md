# 与 Python 熊猫的 7 个必须知道的数据争论操作

> 原文：<https://towardsdatascience.com/7-must-know-data-wrangling-operations-with-python-pandas-849438a90d15?source=collection_archive---------7----------------------->

综合实践指南

![](img/a46350732d5101fdb416681152bb6cc4.png)

娜奥米·赫伯特在 Unsplash 上的照片

Pandas 是一个非常流行的数据分析和操作库。它提供了许多功能来将原始数据转换为更有用或更适合数据分析和机器学习管道的格式。

现实生活中的数据几乎总是杂乱无章的，需要进行大量的预处理才能转换成整洁的格式。由于其多功能和强大的功能，熊猫加快了数据争论的过程。

在本文中，我们将讨论在典型的数据争论过程中可能会遇到的 7 种操作。

我们将使用 Kaggle 上的墨尔本房产[数据集](https://www.kaggle.com/dansbecker/melbourne-housing-snapshot)作为例子。我们首先使用 read_csv 函数读取 csv 文件。

```
import numpy as np
import pandas as pdmelb = pd.read_csv("/content/melb_data.csv")print(melb.shape)
(13580, 21)melb.columnsIndex(['Suburb', 'Address', 'Rooms', 'Type', 'Price', 'Method', 'SellerG','Date', 'Distance', 'Postcode', 'Bedroom2', 'Bathroom', 'Car','Landsize', 'BuildingArea', 'YearBuilt', 'CouncilArea', 'Lattitude','Longtitude', 'Regionname', 'Propertycount'],
dtype='object')
```

该数据集包含墨尔本约 13580 所房屋的 21 个要素。

## 1.处理日期

日期通常存储为对象或字符串。数据集中的日期列存储为对象。

```
melb.Date.dtypes
dtype('o')
```

为了使用 Pandas 的日期时间特定功能，我们需要将日期转换成适当的格式。一种选择是使用 to_datetime 函数。

```
# Before converting
melb.Date[:2]
0    3/12/2016 
1    4/02/2016 
Name: Date, dtype: objectmelb['Date'] = pd.to_datetime(melb['Date'])# After converting
melb.Date[:2]
0   2016-03-12 
1   2016-04-02 
Name: Date, dtype: datetime64[ns]
```

## 2.更改数据类型

除了日期，我们可能还需要做一些其他的数据类型转换。需要转换的典型情况是将整数存储为浮点数。例如，我们数据集中的属性列存储为 float，但它应该是 integer。

astype 函数可用于进行数据类型转换。

```
# Before converting
melb['Propertycount'][:2]
0    4019.0 
1    4019.0 
Name: Propertycount, dtype: float64melb['Propertycount'] = melb['Propertycount'].astype('int')# After converting
melb['Propertycount'][:2]
0    4019 
1    4019 
Name: Propertycount, dtype: int64
```

## 3.替换值

另一个常见的操作是替换值。类型列包含 3 个不同的值，分别是“h”、“u”和“t”。我们可以用这些值所代表的内容来替换它们，从而使这些值包含更多的信息。

replace 函数用于完成这项任务。

```
# Before converting
melb.Type.unique()
array(['h', 'u', 't'], dtype=object)melb.Type.replace({
   'h': 'house', 'u': 'unit', 't': 'town_house'
}, inplace=True)# After converting
melb.Type.unique()
array(['house', 'unit', 'town_house'], dtype=object)
```

## 4.类别数据类型

典型的数据集包含数值列和分类列。分类列通常与对象数据类型一起存储。如果与行数相比，不同类别的数量非常少，我们可以通过使用 category 数据类型来节省大量内存。

我们的数据集包含 13580 行。“类型”列中的类别数是 3。我们先来检查一下本专栏的内存消耗情况。

```
melb.Type.memory_usage()
108768 # in bytes
```

我们将把它转换成类别数据类型，并再次检查内存消耗。

```
melb['Type'] = melb['Type'].astype('category')melb['Type'].memory_usage()
13812
```

它从 108768 字节减少到 13812 字节，这是一个显著的减少。

## 5.从日期中提取信息

在某些情况下，我们可能需要从工作日、月份、年份等日期中提取特定的部分。我们可以使用 dt 访问器下的函数来提取几乎所有关于日期的信息。

让我们举几个例子。

```
# Extract month
melb['Month'] = melb['Date'].dt.monthmelb['Month'][:5]
0    3 
1    4 
2    4 
3\.   4 
4    4 
Name: Month, dtype: int64# Extract weekday
melb['Date'].dt.weekday[:5]
0    5 
1    5 
2    0 
3\.   0 
4    2 
Name: Date, dtype: int64
```

## 6.从文本中提取信息

文本数据通常包含多条信息。就像我们处理日期一样，我们可能需要从文本中提取一条信息。Pandas 字符串访问器提供了许多功能来有效地执行这样的操作。

让我们来看看地址栏。

```
melb.Address[:5]
0        85 Turner St 
1     25 Bloomburg St 
2        5 Charles St 
3    40 Federation La 
4         55a Park St 
Name: Address, dtype: object
```

最后几个字符代表位置的类型。例如，“st”代表街道，“dr”代表车道。这可能是对地址进行分组的有用信息。

我们可以通过在空格字符处拆分字符串并进行最后一次拆分来提取地址的最后一部分。下面是我们如何用 str 访问器完成这个操作。

```
melb['Address'].str.split(' ').str[-1]0    St 
1    St 
2    St 
3    La 
4    St 
Name: Address, dtype: object
```

split 函数，顾名思义，在指定的字符(在我们的例子中是空格)处分割字符串。下一个 str 用于访问分割后的片段。“-1”表示最后一个。

## 7.标准化文本数据

在许多情况下，我们基于文本数据进行比较。这种比较的一个典型问题是字符串没有标准。例如，如果一个单词以大写字母开头，而另一个不以大写字母开头，则可能无法检测到相同的单词。

为了解决这个问题，我们应该对字符串进行标准化。我们可以分别用 str 访问器的 upper 和 lower 函数使它们都是大写或小写字母。

```
melb.Address.str.upper()[:5]0        85 TURNER ST 
1     25 BLOOMBURG ST 
2        5 CHARLES ST 
3    40 FEDERATION LA 
4         55A PARK ST 
Name: Address, dtype: object
```

另一种选择是将字符串大写。

```
melb.Suburb.str.capitalize()[:5]0    Abbotsford 
1    Abbotsford 
2    Abbotsford 
3    Abbotsford 
4    Abbotsford 
Name: Suburb, dtype: object
```

## 结论

我们已经讨论了在数据争论过程中可能会遇到的 7 种典型操作。熊猫为他们提供了高效和通用的解决方案。

当然，在数据清理和预处理过程中，我们可能会面临许多其他问题。不可能在一篇文章中涵盖所有这些内容。然而，我很确定熊猫可以解决你需要处理的大部分任务。

感谢您的阅读。如果您有任何反馈，请告诉我。