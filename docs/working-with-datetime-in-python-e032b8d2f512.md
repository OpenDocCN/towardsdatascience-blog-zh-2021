# 在 Python 中使用日期时间

> 原文：<https://towardsdatascience.com/working-with-datetime-in-python-e032b8d2f512?source=collection_archive---------17----------------------->

## 编程；编排

## 立刻成为日期和时间的主人

![](img/424861c95c521236e6895b42ad0a16a2.png)

来源:[Undraw.co](https://undraw.co/)

# 介绍

在您作为数据科学家的职业生涯中，不可避免地会遇到包含日期和时间的数据。出于这个原因，这篇内容丰富的指南旨在帮助使用 Python 的`datetime`模块轻松操作日期和时间数据。我们将涉及一系列主题，比如处理日期和时间字符串、测量时间跨度以及在`pandas`中使用 datetime 对象。

# Python 的日期时间类

为了向您介绍`datetime`模块，有五个主要的对象类可以方便地用于不同的目的。它们如下:

*   `datetime` —允许我们一起操作日期和时间(月、日、年、小时、秒、微秒)。
*   `date` —仅允许我们操作日期(月、日、年)。
*   `time` —你大概猜到了；这个类只允许我们操纵时间(小时、分钟、秒、微秒)。
*   `timedelta` —用于测量持续时间，即两个日期或时间之间的差异。
*   `tzinfo` —用于处理时区。我们不会在本教程中涉及这一点。

让我们来看看何时以及如何使用这些类。

# 使用日期时间

首先，重要的是要注意到`datetime`既是一个模块，也是该模块中的一个类，用于编写您的导入。

我们将演示使用`datetime`对象的两种方法。

首先，您可以将参数传递给`datetime`，从最大的时间单位开始，以最小的时间单位结束(年、月、日、小时、分钟、秒)。值得一提的是，你在现实生活中不会经常遇到这种方法，因为你很可能会发现自己正在使用现成的数据，但尽管如此，知道这一点还是有好处的。

第二，如果你想得到当前的`datetime`，你可以很容易地使用`.now()`函数。

```
# import datetime
import datetime from datetime# create datetime object 
# datetime_object1 = datetime(2021, 6, 1, 15, 23, 25)# get current date
datetime_object2 = datetime.now()
```

使用`datetime`很酷的一点是，我们可以对不同的`datetime`对象进行运算。例如，让我们假设我们想要找出前两个日期时间之间经过的秒数。我们可以这样做:

```
duration = datetime_object2 - datetime_object1
duration.total_seconds()
```

## 访问单个组件

您可以使用日期时间的属性访问`datetime`对象的单个组件；例如年、日和小时属性。

```
# extract data
datetime_object2.year
datetime_object2.day 
datetime_object2.hour
```

此外，您还可以使用`.weekday()`功能找到星期几。

```
datetime_object2.weekday()
```

运行前一行得到的结果是`2`，这是一个星期三。请注意，Python 从 0 开始计算工作日，从星期一开始。这意味着:

*   0 =星期一
*   1 =星期二
*   2 =星期三
*   …
*   6 =星期日

既然我们已经介绍了`datetime`对象的来龙去脉，你也可以很容易地使用`date`和`time`对象，因为它们使用相同的方法。

# 使用时间增量

在我们想要测量持续时间的情况下,`timedelta`类很方便，因为它表示两个日期或时间之间的时间量。当我们想要从日期或时间中加减时，这特别有用。

我们将继续演示如何一起使用`timedelta`对象和`datetime`对象进行数学运算。例如，我们将在当前时间上增加 27 天。

```
# import timedelta
from datetime import timedelta# create a 27 day timedelta
td = timedelta(days=27)#add 27 days to current date
27_days_later = datetime_object2 + td
```

类似于我们上面所做的，我们也可以使用`timedelta`从当前日期或任何其他日期中减去。

注意`timedelta`可用于任意周数、天数、小时数、分钟数等。它可以使用小到一微秒的时间单位，大到*270 万年*！令人印象深刻。

# 使用日期和时间字符串

通常，我们会发现我们想要将对象从字符串转换成`datetime`对象，反之亦然。`datetime`包括两个有用的方法，可以帮助我们实现这一点；也就是说，`strptime()`和`strftime().`我们可以使用`strptime()`读取包含日期和时间信息的字符串并将它们转换成`datetime`对象，使用`strftime()`将`datetime`对象转换回字符串。

当使用`strptime()`时，我们必须考虑到它不能将任何字符串转换成日期和时间，因此我们必须自己指示时间格式。最好用一个例子来说明这一点:

```
date_string = '2020-11-27'

# Create date object with time format yyyy-mm-dd
date = datetime.strptime(date_string, "%Y-%m-%d")
```

注意`strptime()`有两个参数:

*   字符串-字符串格式的时间
*   格式—字符串中时间的特定格式

如果你不熟悉帮助`strptime()`解释我们的字符串输入所需的格式化代码，你可以在这里找到[有用的参考](https://strftime.org/)。

# 使用熊猫日期时间对象

`pandas`是最受欢迎的 Python 模块之一，这是有充分理由的。其中之一是它使处理时间序列数据变得轻而易举。与`datetime`模块相似，pandas 也有与`datetime`模块功能相似的`datetime`和`timedelta`对象。

简单介绍一下，我们可以使用以下函数将日期时间和持续时间字符串转换为 pandas 日期时间对象:

*   `to_datetime()` —将字符串格式的日期和时间转换为 Python `datetime`对象。
*   `to_timedelta()` —用于测量持续时间。

这些函数在将字符串转换成 Python `datetime`时做得很好，因为它们自动检测日期的格式，而不需要我们像对`strptime()`那样定义它。

让我们用一个具体的例子来检验这一点:

```
# import pandas 
import pandas as pd# create date object 
date = pd.to_datetime("4th of oct, 2020")
```

即使我们的输入看起来是一个复杂的字符串，`pandas`仍然能够正确地解析该字符串并返回一个`datetime`对象。

## 提取单个组件

假设我们想从日期中提取月、小时或分钟，并将其作为一个新列存储在 pandas 数据帧中。我们使用`dt`属性很容易做到这一点。

例如，我们可以使用`df['date'].dt.month`从包含完整日期的 pandas 列中提取月份，并将其存储在一个新列中。这个例子当然也可以扩展到不同的时间单位。

类似于我们之前使用的`datetime`类，pandas 也能够提取元素，例如星期几。我们再次使用`dt`属性来完成这项工作。

比如我们可以用`df['date'].dt.weekday`提取一天的数字，0 代表星期一等等。如果我们想直接提取工作日的名称，我们可以方便地使用`df['date'].dt.weekday_name`来完成。

# 结论

在本指南中，我们介绍了如何通过处理字符串、使用`timedelta`测量持续时间和进行简单的算术运算以及使用`pandas`中的日期时间对象来成功地浏览日期和时间的世界。

如果你有任何问题，我很乐意在评论中回答！

我希望这个指南对你有所帮助。如果你这样做了，你可能会发现我的其他一些文章也很有趣:

[](/a-machine-learning-approach-to-credit-risk-assessment-ba8eda1cd11f) [## 信用风险评估的机器学习方法

### 预测贷款违约及其概率

towardsdatascience.com](/a-machine-learning-approach-to-credit-risk-assessment-ba8eda1cd11f) [](/a-quick-and-easy-guide-to-code-profiling-in-python-58c0ed7e602b) [## Python 中代码剖析的快速简易指南

towardsdatascience.com](/a-quick-and-easy-guide-to-code-profiling-in-python-58c0ed7e602b) [](https://medium.com/codex/web-scraping-using-scrapy-and-python-7ad6d1cd63d0) [## 使用 Scrapy 和 Python 进行 Web 抓取

### 收集网络数据的初学者友好指南

medium.com](https://medium.com/codex/web-scraping-using-scrapy-and-python-7ad6d1cd63d0)