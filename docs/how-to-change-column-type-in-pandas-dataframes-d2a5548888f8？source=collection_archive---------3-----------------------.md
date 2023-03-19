# 如何更改 Pandas 数据框架中的列类型

> 原文：<https://towardsdatascience.com/how-to-change-column-type-in-pandas-dataframes-d2a5548888f8?source=collection_archive---------3----------------------->

## 探索在 pandas 中改变列的数据类型的 3 种不同选择

![](img/6e02c9243a298f584b3761c8c8dd94b9.png)

照片由[梅根·卡森](https://unsplash.com/@mcarsience_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/transform?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

## 介绍

在处理熊猫数据帧时，最常见的操作之一是**数据** **类型(或** `**dtype**` **)转换**。在今天的文章中，我们将探索 3 种不同的方法来改变熊猫的栏目类型。这些方法是

*   `astype()`
*   `to_numeric()`
*   `convert_dtypes()`

在开始讨论可以用来更改某些列的类型的各种选项之前，让我们首先创建一个伪数据帧，我们将在整篇文章中使用它作为示例。

```
import pandas as pddf = pd.DataFrame(
  [
    ('1', 1, 'hi'),
    ('2', 2, 'bye'),
    ('3', 3, 'hello'),
    ('4', 4, 'goodbye'),
  ],
  columns=list('ABC')
)print(df)#    A  B        C
# 0  1  1       hi
# 1  2  2      bye
# 2  3  3    hello
# 3  4  4  goodbyeprint(df.dtypes)# A    object
# B     int64
# C    object
# dtype: object
```

## 使用 astype()

`[DataFrame.astype()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.astype.html)`方法用于将熊猫列转换为指定的*数据类型*。指定的 dtype 可以是内置 Python、`numpy`或`pandas` dtype。

假设我们想要将列`A`(目前是一个类型为`object`的字符串)转换成一个保存整数的列。为此，我们只需在 pandas DataFrame 对象上调用`astype`，并显式定义我们希望转换列的 dtype。

```
**df['A'] = df['A'].astype(int)**print(df)#    A  B        C
# 0  1  1       hi
# 1  2  2      bye
# 2  3  3    hello
# 3  4  4  goodbyeprint(df.dtypes)# A     int64
# B     int64
# C    object
# dtype: object
```

您甚至可以一次转换多个列。举个例子，

```
**df = df.astype({"A": int, "B": str})**print(df)#    A  B        C
# 0  1  1       hi
# 1  2  2      bye
# 2  3  3    hello
# 3  4  4  goodbyeprint(df.dtypes)# A     int64
# B    object
# C    object
# dtype: object
```

此外，您甚至可以指示`astype()`在观察到所提供的 dtype 的无效数据时如何行动。这可以通过传递相应的`errors`参数来实现。您可以选择`'raise’`无效数据异常或`'ignore’`抑制异常。

例如，假设我们有一个混合了`dtypes`和`Series`的列，如下所示:

```
s = pd.Series(['1', '2', 'hello'])print(s)
# 0        1
# 1        2
# 2    hello
# dtype: object
```

现在，如果我们试图将该列转换为`int`，将会引发一个`ValueError`:

```
s = s.astype(int)
ValueError: invalid literal for int() with base 10: 'hello'
```

我们可以通过传递`errors='ignore'`来抑制`ValueError`。在这种情况下，原始对象将被返回

```
s = s.astype(int, errors='ignore')
print(s)# 0        1
# 1        2
# 2    hello
# dtype: object
```

*从 1.3.0 版本开始，用于将时区简单类型转换为时区感知类型的* `*astype*` *已被弃用。你现在应该用* `[*Series.dt.tz_localize()*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.tz_localize.html#pandas.Series.dt.tz_localize)`代替*。*

## 使用 to_numeric()

`[pandas.to_numeric](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.to_numeric.html)`用于将非数字`dtypes`列转换为最合适的数字时间。例如，为了将列`A`转换为`int`,您需要运行的全部内容是

```
**df['A'] = pd.to_numeric(df['A'])**print(df)
#    A  B        C
# 0  1  1       hi
# 1  2  2      bye
# 2  3  3    hello
# 3  4  4  goodbyeprint(df.dtypes)
# A     int64
# B     int64
# C    object
# dtype: object
```

现在，如果您想将多个列转换成数字，那么您必须使用如下所示的`apply()`方法:

```
**df[["A", "B"]] = df[["A", "B"]].apply(pd.to_numeric)**print(df.dtypes)
# A     int64
# B     int64
# C    object
# dtype: object
```

`to_numeric()`也接受`errors`参数(像`astype()`一样),但是它有一个额外的选项，即`*'*coerce*'*` 将所有不能使用指定的`dtype`解析的值设置为`NaN`。要了解更多细节，请务必查看官方[文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.to_numeric.html)。

*你可能还想看看* `*to_datetime()*` *和* `*to_timedelta()*` *方法，如果你想把一列分别投射到* `*datetime*` *或* `*timedelta*` *的话。*

## 使用`convert_dtypes()`

`[convert_dtypes()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.convert_dtypes.html#pandas.DataFrame.convert_dtypes)`方法包含在 pandas 版本`1.0.0`中，用于使用支持`pd.NA`(缺失值)的`dtypes`将列转换为最佳的`dtypes`。这意味着`dtype`将在运行时根据指定列中包含的值来确定。

```
**df = df.convert_dtypes()**print(df.dtypes)# A    string
# B     Int64
# C    string
# dtype: object
```

注意列 A 和 C 是如何被转换成`string`类型的(最初它们是`object`类型)。尽管这对于列`C`来说相当准确，但理想情况下，我们希望将列`A`转换为 int 列。如果你想让这个方法算出应该用什么`dtypes`，这基本上是你必须付出的代价。如果这不是您所期待的，您仍然可以使用本文前面提到的任何方法来显式指定目标`dtypes`。

## 最后的想法

在今天的文章中，我们探讨了 pandas 中的许多选项，可以用来转换数据帧中特定列的数据类型。我们讨论了如何使用`astype()`来显式指定列的`dtypes`。此外，我们还探索了如何使用`to_numeric()`方法将列转换成数字类型。最后，我们展示了如何使用`convert_dtypes()`方法，根据每个列中包含的值计算出最合适的`dtypes`列。