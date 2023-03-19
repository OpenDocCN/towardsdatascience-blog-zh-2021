# Python 日期时间模块中的 4 个必须知道的对象

> 原文：<https://towardsdatascience.com/4-must-know-objects-in-python-datetime-module-d18565fddd05?source=collection_archive---------23----------------------->

## 综合实践指南

![](img/731966366f745c49b95601764aae72a8.png)

在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[尹新荣](https://unsplash.com/@insungyoon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

时间是许多数据科学相关任务的重要特征。例如，每日销售和库存信息对于零售分析至关重要。算法交易需要分钟级别的交易数据。

我们表示和使用时间相关信息的方式随着任务的不同而不同。对于一个科学实验，我们可能会谈到微秒级的测量记录。但是，对于人口统计信息，如人口、平均家庭收入等，我们不需要这样的精度。

Python 的 datetime 模块帮助我们处理任何精度级别的时间相关信息。在本文中，我们将详细介绍本模块中的 4 个对象，它们是日期、时间、日期时间和时间增量。

## 日期

对象用年、月和日来表示日期。让我们看几个例子来演示如何使用它们。

我们可以创建一个日期对象，通过传递年、月和日信息来存储特定的日期。

```
from datetime import datemydate = date(2020, 4, 11)print(mydate)
2020-04-11
```

我们还可以基于今天的日期创建一个 date 对象。

```
today = date.today()print(today)
2021-04-13
```

可以从日期对象中提取各个属性。

```
type(today)
datetime.datetoday.year
2021today.month
4today.day
13
```

需要注意的是，我们不能在月和日信息中使用前导零。例如，下面的代码将返回一个语法错误。

```
mydate = date(2020, 04, 02)   # SyntaxErrormydate = date(2020, 4, 2)     # OK
```

我们可以对 date 对象使用的另一个有用的方法是 ctime，它以更详细或扩展的格式返回日期。

```
mydate = date(2021, 2, 15)mydate.ctime()
Mon Feb 15 00:00:00 2021
```

我们可以用减号来计算两个日期之间的差异。

```
mydate1 = date(2021, 2, 15)
mydate2 = date(2021, 3, 12)mydate1 - mydate2
datetime.timedelta(days=-25)
```

返回的对象属于 timedelta 类型，我们也将在本文中介绍这一类型。差额以天计算。

## 时间

时间对象表示小时、分钟、秒和微秒级别的时间。

```
from datetime import timemytime = time(14, 20)print(mytime)
14:20:00
```

我们只定义了小时和分钟部分。默认情况下，Python 为剩余的时间属性分配 0。

```
mytime.second
0mytime.microsecond
0
```

我们还可以通过传递所有属性来创建更精确的时间对象。

```
mytime = time(2, 14, 25, 30)print(mytime)
02:14:25.000030
```

## 日期时间

Datetime 是一种日期和时间的组合。它可以表示日期和时间对象的所有属性。让我们跳到例子上来使它更清楚。

```
from datetime import datetimemydatetime = datetime(2021, 1, 14)
print(mydatetime)
2021-01-14 00:00:00mydatetime2 = datetime(2021, 1, 14, 16, 20)
print(mydatetime2)
2021-01-14 16:20:00
```

datetime 对象提供了仅使用日期或组合使用日期和时间的灵活性。参数之间有一个等级，从年份开始，一直到微秒级。

today 函数也可以与 datetime 对象一起使用。

```
datetime.today()
datetime.datetime(2021, 4, 13, 16, 34, 45, 137530)print(datetime.today())
2021-04-13 16:35:49.965258
```

如果打印 datetime 对象，输出将以更结构化的方式显示。

就像日期和时间对象一样，可以提取日期时间对象的各个属性。

```
mydatetime = datetime(2021, 1, 14, 16, 20)print(mydatetime.month)
1print(mydatetime.minute)
20
```

我们还可以计算两个 datetime 对象之间的差异。

```
mydatetime1 = datetime(2021, 1, 14, 16, 20)
mydatetime2 = datetime(2021, 1, 10)diff = mydatetime1 - mydatetime2print(diff)
4 days, 16:20:00
```

## 时间增量

Timedelta 对象表示一个持续时间，因此我们可以使用它们来计算两个日期或时间之间的差异。

下面是一个 timedelta 对象，表示 6 天的持续时间。

```
mydelta = timedelta(days=6)print(mydelta)
6 days, 0:00:00
```

我们可以用周、天、小时、分钟、秒和微秒来创建 timedelta 对象。输出以天数和小时-分钟-秒的组合形式显示。

```
mydelta1 = timedelta(minutes=40)
print(mydelta1)
0:40:00mydelta2 = timedelta(weeks=4)
print(mydelta2)
28 days, 0:00:00
```

不同的单位也可以组合起来创建一个 timedelta 对象。

```
mydelta3 = timedelta(days=2, hours=4)
print(mydelta3)
2 days, 4:00:00
```

我们已经看到了如何计算两个日期或日期时间对象之间的差异。timedelta 对象的另一个常见用例是修改它们。

例如，我们可以给一个日期加上一个特定的持续时间来计算未来的日期。

```
mydate = date.today()
print(mydate)
2021-04-13two_weeks_from_now = mydate + timedelta(weeks=2)
print(two_weeks_from_now)
2021-04-20
```

我们还可以使用 timedelta 对象修改 datetime 对象。

```
mydate = datetime(2020, 10, 4, 15, 10)
print(mydate)
2020-10-04 15:10:00past_date = mydate - timedelta(weeks=7, days=4)
print(past_date)
2020-08-12 15:10:00
```

## 结论

Python 的 Datetime 模块提供了许多不同的方法来操作日期和时间。它在处理日期时非常方便。

我们已经介绍了 datetime 模块的 4 种主要对象类型。这些对象是日期、时间、日期时间和时间增量。datetime 对象是日期和时间对象的一种组合。它可以存储从几年到几微秒的信息。

datetime 模块还提供了其他一些对象类型，如 timezone、strftime 和 strptime。

感谢您的阅读。如果您有任何反馈，请告诉我。