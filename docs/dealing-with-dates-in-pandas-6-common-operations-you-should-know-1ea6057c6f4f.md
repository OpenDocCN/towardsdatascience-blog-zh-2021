# 处理熊猫的约会——你应该知道的 6 个常见操作

> 原文：<https://towardsdatascience.com/dealing-with-dates-in-pandas-6-common-operations-you-should-know-1ea6057c6f4f?source=collection_archive---------11----------------------->

## 希望不会再和日期混淆了。

![](img/a213f3782d122981a0b5acda06920cda.png)

照片由 [insung yoon](https://unsplash.com/@insungyoon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

除了文本和数字，日期是我们数据集中非常常见的数据类型。当我们使用熊猫来处理日期时，对于大多数数据科学家来说，这绝对不是最简单的任务。当我开始使用熊猫时，我不熟悉与日期相关的功能，所以每次当我处理日期时，我必须在网上搜索解决方案。为了减轻你的痛苦，在这里，我想用熊猫来回顾约会的 6 个常见操作。

## 1.阅读期间解析日期

当数据集包含日期信息时，您希望读取它。对于本教程，让我们创建一个名为`dates_text.csv`的 CSV 文件，以下面的数据行作为起点。

```
text_data = """date,category,balance
01/01/2022,A,100
02/02/2022,B,200
03/12/2022,C,300"""

with open("dates_text.csv", "w") as file:
    file.write(text_data)
```

让我们看看当我们读取这个 CSV 文件时会发生什么。

正在读取 CSV 文件

如上所示，问题在于`date`列被读取为`object`类型而不是日期类型，这使得它无法访问 Pandas 中任何与日期相关的功能。

简单的解决方法是让熊猫为我们解析日期。如下所示，我们为`parse_dates`参数指定一个包含日期列名的列表对象。正如所料，日期列现在是一种日期类型(即`datetime64[ns]`)。请注意，如果您有多个日期列，可以使用`parse_dates=[“date”, “another_date”]`。

读取 CSV 文件—解析日期

应该注意的是，Pandas 集成了强大的日期解析器，可以自动解析许多不同种类的日期。因此，您通常只需要设置`parse_date`参数。

您并不总是拥有包含日期作为单个值的数据集。下面显示了一个可能的场景，其中三列组成了完整的日期信息。

```
text_data_cols = """y,m,d,category,balance
2022,01,01,A,100
2022,02,02,B,200
2022,03,12,C,300"""with open("dates_text_cols.csv", "w") as file:
    file.write(text_data_cols)
```

**要解析多列中的日期，可以向** `**parse_dates**` **参数发送一个 dictionary 对象。**

组合日期列

## 2.指定日期格式

在美国，我们通常使用月前一天，这也是熊猫的默认格式。让我们检查一下我们在上一节中刚刚读到的数据帧。如图所示，日期值表示为 yyyy-mm-dd。

```
>>> df = pd.read_csv("dates_text.csv", parse_dates=["date"])
>>> df
        date category  balance
0 2022-01-01        A      100
1 2022-02-02        B      200
2 2022-03-12        C      300
```

如果你还记得，不清楚我们的日期是先用月还是先用日，后者是许多国家的惯例。

```
Raw Data:
text_data = """date,category,balance
01/01/2022,A,100
02/02/2022,B,200
03/12/2022,C,300"""
```

如果原始日期确实首先列出了日期，那该怎么办？**为了正确读取这样的日期，我们可以指定** `**dayfirst**` **参数**，如下图所示。

```
>>> df = pd.read_csv("dates_text.csv", parse_dates=["date"], dayfirst=True)
>>> df
        date category  balance
0 2022-01-01        A      100
1 2022-02-02        B      200
2 2022-12-03        C      300
```

如果您特别注意第三行，您会发现 12 现在被正确地列为月份，而不是之前被解析为日期。

除了`parse_dates`和`dayfirst`之外，当您在解析日期时发现问题时可以尝试的另一个相关参数是将`infer_datetime_format`设置为`True`，这可以帮助推断默认的日期格式。

## 3.自定义日期解析器

当你有一个复杂的数据集时，你可能需要实现你的日期解析器。考虑以下文件，其中的数据使用特殊的日期格式。

```
text_data_fmt = """date,category,balance
Jan_01_2022,A,100
Feb_02_2022,B,200
Mar_12_2022,C,300"""

with open("custom_dt_fmt.csv", "w") as file:
    file.write(text_data_fmt)
```

当您试图自动解析日期时，它不起作用。

解析日期失败

在这种情况下，我们需要创建一个定制的数据解析器。一个可能的解决方案如下所示。

自定义日期分析器

这里棘手的事情是在`strptime`方法中定义适当的日期格式。你可以在这里找到完整的格式字符串。`data_parser`采用一个单参数函数，其中的参数指向您想要解析日期的列。

## 4.无论如何转换日期

当您不需要正确处理读取和解析日期时，您可能仍然需要在准备数据集的过程中处理日期。在这些情况下，使用熊猫的效用函数就很有用:`to_datetime`，其函数头如下所示([来源](https://pandas.pydata.org/docs/reference/api/pandas.to_datetime.html))。

```
pandas.to_datetime(*arg*, *errors='raise'*, *dayfirst=False*, *yearfirst=False*, *utc=None*, *format=None*, *exact=True*, *unit=None*, *infer_datetime_format=False*, *origin='unix'*, *cache=True)*
```

如您所见，该函数接受一系列参数，这些参数几乎是您处理日期所需的一切。更重要的是，你几乎可以传递任何东西作为`arg`参数。下面给出了一些例子。

熊猫到日期时间示例

您可以看到`to_datetime`可以处理单个字符串、字符串列表或系列对象。尽管返回的数据类型各不相同，但它们本质上是一样的，或者很容易互换。你可以在这里找到更多信息[。](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Timestamp.html)

## 5.创建日期范围

当我们处理可以使用日期作为自然索引的数据时，可以使用 Python 内置的`datetime`模块“手动”创建索引，如下所示。

“手动”创建的日期索引

然而，这个特性已经被 Pandas 考虑到了，我们应该为此使用`date_range`函数:

使用 date_range 创建日期索引

## 6.提取日期、时间等等

有时，我们可以有一个由日期和时间组成的时间戳。考虑下面的例子作为我们的起点。

熊猫的时间戳

如您所见，我们现在有了属于`datetime`类型的时间戳列。但是，我们希望将日期和时间分开，这样我们就可以进行其他操作。为此，您可以对 Pandas 使用`dt`访问器，如下所示。

```
>>> df["date"] = df["timestamp"].dt.date
>>> df["time"] = df["timestamp"].dt.time
>>> df
            timestamp category        date      time
0 2022-01-01 09:01:00        A  2022-01-01  09:01:00
1 2022-01-02 08:55:44        B  2022-01-02  08:55:44
2 2022-05-01 22:01:00        C  2022-05-01  22:01:00
```

实际上，您可以使用`dt`访问器做更多的事情，例如，您可以获得年、月、日，甚至是一年中的星期！下面是一些例子。

```
>>> df["timestamp"].dt.year
0    2022
1    2022
2    2022
Name: timestamp, dtype: int64
>>> df["timestamp"].dt.month
0    1
1    1
2    5
Name: timestamp, dtype: int64
>>> df["timestamp"].dt.weekday
0    5
1    6
2    6
Name: timestamp, dtype: int64
```

## 结论

在这篇文章中，我们回顾了 6 个常见的操作相关的加工日期在熊猫。我肯定忽略了一些方面。如果您有任何问题，请随时留下评论，我们可以在未来的文章中讨论其他功能！

感谢阅读这篇文章。通过[注册我的简讯](https://medium.com/subscribe/@yong.cui01)保持联系。还不是中等会员？使用我的会员链接通过[支持我的写作(对你来说没有额外的费用，但是你的一部分会员费作为奖励由 Medium 重新分配给我)。](https://medium.com/@yong.cui01/membership)