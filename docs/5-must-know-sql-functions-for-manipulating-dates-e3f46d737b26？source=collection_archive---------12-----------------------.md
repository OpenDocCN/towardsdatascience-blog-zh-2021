# 操纵日期的 5 个必备 SQL 函数

> 原文：<https://towardsdatascience.com/5-must-know-sql-functions-for-manipulating-dates-e3f46d737b26?source=collection_archive---------12----------------------->

## SQL 实践教程。

![](img/e578d0f7d722f9b665e4366a40e6834b.png)

在 [Unsplash](https://unsplash.com/s/photos/calendar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[埃斯特·扬森斯](https://unsplash.com/@esteejanssens?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

如果你在我找到第一份工作之前问我数据科学家最重要的技能是什么，我的答案肯定是 Python。毫无疑问！

我现在是一名数据科学家，如果你问我同样的问题，我的答案仍然是 Python。然而，我有第二个想法。让我三思的是 SQL。

SQL 是数据科学家的必备技能。它主要用于查询关系数据库，但它能够执行更多。SQL 配备了如此多的功能，使其成为高效的数据分析和操作工具。

在本文中，我们将介绍 5 个用于操作日期的 SQL 函数。当处理涉及基于日期或时间的信息的数据时，它们会派上用场。

**注意:** SQL 被很多关系数据库管理系统使用，比如 MySQL、SQL Server、PostgreSQL 等等。尽管它们大多采用相同的 SQL 语法，但可能会有一些细微的差别。在本文中，我们将使用 SQL Server。

## 1.获取日期

顾名思义，getdate 函数给出今天的日期。我们来做一个例子。

```
DECLARE @mydate Date
SET @mydate = GETDATE()print @mydate
'2021-08-22'
```

我们创建一个名为“mydate”的变量，并将其值指定为今天的日期。

## 2.Dateadd

这个函数的名字甚至比前一个函数更容易理解。dateadd 函数用于向日期添加时间或日期间隔。

和往常一样，语法通过例子更容易理解。

```
DECLARE @mydate Date
SET @mydate = GETDATE()SELECT DATEADD(MONTH, 1, @mydate) AS NextMonthNextMonth
2021-09-22
```

第一个参数表示间隔，第二个参数是间隔的数量。第三个参数是基值。

```
DATEADD(interval, number of intervals, date)
```

我们也可以使用其他间隔。

```
DECLARE @mydate Date
SET @mydate = GETDATE()SELECT DATEADD(WEEK, -2, @mydate) AS TwoWeeksBeforeTwoWeeksBefore
2021-08-08
```

如果在间隔数前加一个减号，它会从给定的日期中减去指定的间隔。

可以添加基于时间的间隔，但我们需要使用日期时间变量。

```
DECLARE @mydate DateTime
SET @mydate = GETDATE()SELECT DATEADD(HOUR, 10, @mydate)
'2021-08-23 00:10:17.287'
```

## 3.Datediff

datediff 函数用于根据给定的时间间隔计算两个日期之间的差异。

```
DECLARE @mydate Date
SET @mydate = '2018-08-08'SELECT DATEDIFF(MONTH, @mydate, GETDATE())
36
```

mydate 变量保存值“2018–08–08”。在 select 语句中，我们计算这个变量和当前日期之间的差值。

就像 dateadd 函数一样，datediff 函数也接受其他间隔。

```
DECLARE @mydate Date
SET @mydate = '2021-10-08'SELECT DATEDIFF(DAY, @mydate, GETDATE()) AS DayDifferenceDayDifference
47
```

## 4.日期名称

datename 函数可用于提取日期的各个部分。例如，我们可以从日期中获取月和日的名称，如下所示:

```
DECLARE @mydate Date
SET @mydate = '2021-10-08'SELECT DATENAME(MONTH, @mydate)
OctoberSELECT DATENAME(WEEKDAY, @mydate)
Friday
```

## 5.年、月、日

年、月和日是独立的函数，但我认为最好将它们放在一起。

我们已经介绍了 datename 函数，它给出了月份和日期的名称。在某些情况下，我们需要数字形式的信息。年、月和日函数允许分解日期。

让我们做一个例子来演示如何使用它们。

```
DECLARE @mydate Date
SET @mydate = '2021-10-08'SELECT
   Date = @mydate,
   Year = YEAR(@mydate),
   Month = MONTH(@mydate),
   Day = DAY(@mydate)Date        Year  Month  Day
2021-10-08  2021  10     8
```

我们现在可以访问给定日期的每个部分。

## 结论

操作日期对于数据分析非常重要，尤其是在处理时间序列数据时。SQL 提供了几个函数来使这个过程简单而高效。

本文中的函数涵盖了您需要对日期进行的大量操作。

感谢您的阅读。如果您有任何反馈，请告诉我。