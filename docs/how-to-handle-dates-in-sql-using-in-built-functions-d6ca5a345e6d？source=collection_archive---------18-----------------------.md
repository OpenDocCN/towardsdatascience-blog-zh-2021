# 如何使用内置函数在 SQL 中处理日期

> 原文：<https://towardsdatascience.com/how-to-handle-dates-in-sql-using-in-built-functions-d6ca5a345e6d?source=collection_archive---------18----------------------->

## 日期函数是 SQL 编程中谈论最多的领域之一

![](img/480b828c8bb10ada34798b03335d5953.png)

埃德加·莫兰在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

作为一名数据分析师，我用不同的时间粒度构建仪表板和报告，如每周、每月、每季度、每年。对于时间序列分析，我需要聚合数据中的时间戳。这就是日期函数发挥作用的地方。这些内置功能使分析师的任务变得更加容易。我无法想象没有内置日期功能的生活。

今天，我将专门讨论 PostgreSQL 附带的那些。但是几乎所有的 SQL 数据库都以某种形式支持这些。

以下是我的一些主要约会功能。

## 1.日期 _trunc

Date_trunc 用于将日期截断为周、月、季度或年。此函数最广泛地用于创建时间序列和粒度级聚合。例如，如果您想要绘制 KPI 的趋势，如季度销售额、活跃用户、月订单等，可以使用该函数。

**语法:**

```
SELECT DATE_TRUNC('month',calendar_date) AS Month,
       COUNT(DISTINCT USER) AS active_users
FROM usage
GROUP BY 1
```

## 2.当前日期

我认为这是我所有查询中最常用的函数。顾名思义，这个函数给出当前日期。几乎每次分析数据集时，我都会根据手头的问题将数据集限制在过去几年/几周/几个月，以缩短查询执行时间。这个函数通过获取当前日期来帮助我这样做，当前日期将成为我的结束日期，然后我提取相对于这个结束日期的开始日期。例如，让我们假设我想看看去年我的销售额逐月增长的情况。我可以使用 current_date 获得最近的一个月，然后从中减去 12 个月，得到完整的一年。

**语法:**

```
SELECT DATE_TRUNC('month',calendar_date) AS Month,
       SUM(sales) AS monthly_sales
FROM sales
WHERE calendar_date BETWEEN DATE_TRUNC('month',CURRENT_DATE) -INTERVAL '12 Months' AND DATE_TRUNC('month',calendar_date)
GROUP BY 1
```

## 3.Datediff

此函数给出您指定的日期部分中两个日期/时间戳之间的差异。您可以将日期部分指定为秒、分、小时、天、周、月、年等。例如，下面的查询根据用户开始和结束观看会话的时间，以分钟为单位计算电影观众人数。

**语法:**

```
SELECT DISTINCT movie,
       SUM(datediff ('minutes',watch_start_time,watch_end_time)) AS watch_minutes
FROM movie
GROUP BY 1
```

## 4.日期 _ 部分

date_part 函数提取日期部分的值。例如，有时我必须从我的数据集中排除/突出周末数据来处理异常值。这个功能非常好地完成了这项工作。

**语法:**

```
SELECT CASE
         WHEN DATE_PART('dow',calendar_date) IN (1,6) THEN 'Weekend'
         ELSE 'Weekday'
       END AS time_of_week,
       COUNT(DISTINCT session_id) AS sessions
FROM sessions
GROUP BY 1
```

## 5.Trunc

Trunc 函数将时间戳截断为日期值。这类似于铸造一个时间戳。
**语法:**

```
Select DISTINCT trunc(start_time) as day
FROM usage
```

## 6.到时间戳

这个函数将时间戳转换为带时区的时间戳。当我试图分析基于事件的数据时，我不得不使用它，其中一个数据源捕获 UTC 时区的数据，另一个数据源捕获本地时区的数据。默认情况下，会添加 UTC/GMT 时区。

**语法:**

```
SELECT to_timestamp('2011-12-18 04:38:15', 'YYYY-MM-DD HH24:MI:SS')
```

**结果:**

```
to_timestamp 
---------------------- 
2011-12-19 04:38:15+00
```

## 7.Dateadd

Dateadd 按指定的间隔递增日期或时间戳。例如，假设我们有一个数据集，其中包含一个持续时间为 30 分钟的在线测试的数据，该测试在 30 分钟后自动结束。如果我们只有每个考生的开始时间，我们可以使用 dateadd 来计算每个考生的结束时间。

**语法:**

```
SELECT DATE_TRUNC('month',calendar_date) AS Month,
       COUNT(DISTINCT USER) AS active_users
FROM usage
GROUP BY 1
```

## 8.最后一天

Last_day 返回包含给定日期的月份的最后一天的日期。例如，你可以用它来检查一年是否是闰年，一个季度的最后几天销售额如何变化，等等。

**语法:**

```
SELECT DISTINCT year,
       CASE
         WHEN last_day ((year|| '-02-01')::DATE) = (year|| '-02-29')::DATE THEN 'Leap Year'
         ELSE 'Not a leap Year'
       END AS is_leap_year
FROM years
```

## 9.间隔

Interval 实际上是一个文字，而不是一个内置函数。这种文字允许您确定特定的时间段，如 6 个月、30 天、45 天、2 周等。例如，在下面的例子中，我用它将一个纪元值转换成一个时间戳值。

**语法:**

```
SELECT epoch_time,
       TIMESTAMP 'epoch' +(epoch_time*INTERVAL '1 seconds') AS date_time_value
FROM date_time_data
```

## 总结

在执行时间序列分析时，理解处理日期-时间数据类型的技术是极其重要的。我觉得 SQL 是一个在进一步分析之前尽可能清理数据集的好地方。通过学习这些函数，您将能够分析与趋势相关的数据，并在更短的时间内找到更深入的见解。

我希望你尝试这些功能，并看到使用这些功能的好处。

如果你有任何问题或建议，请在评论区告诉我。

感谢阅读！

*下次见……*