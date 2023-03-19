# 更有效地使用 Python F 字符串的 4 个技巧

> 原文：<https://towardsdatascience.com/4-tricks-to-use-python-f-strings-more-efficiently-4f389e890514?source=collection_archive---------6----------------------->

## 完全控制您打印的内容。

![](img/9c17c4a9207135ca62d8b3a51cb393ab.png)

乔希·拉科瓦尔在 [Unsplash](https://unsplash.com/s/photos/cool?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

字符串插值是一种将变量嵌入字符串的方法。它使得操纵和丰富字符串变得容易。因此，使用字符串插值，print 语句更加强大。

格式化字符串文字，也称为 f 字符串，是一种非常实用的字符串插值方法。他们使用花括号作为变量占位符。

在这篇文章中，我们将学习 4 个技巧来更有效地使用 f 弦。让我们从一个简单的例子开始演示 f 弦是如何工作的。

```
age = 24print(f"John is {age} years old.")John is 24 years old.
```

## 1.格式化大数

当处理大量数字时，为了更好的可读性，最好使用千位分隔符。f 弦使得正确放置这些分隔符变得非常简单。

以下是没有千位分隔符的情况:

```
number = 3454353453print(f"The value of the company is {number}")The value of the company is 3454353453
```

让我们放置分离器，看看有什么不同。

```
print(f"The value of the company is {number:,d}")The value of the company is 3,454,353,453
```

## 2.格式化日期

在我们的脚本中有各种方法来表示日期。这取决于地理位置或只是你的喜好。

我们可以把日期放在 f 字符串中，而不像其他变量那样格式化。

```
from datetime import datetimetoday = datetime.today().date()print(f"Today is {today}")Today is 2021-06-23
```

在某些情况下，以下可能是更好的表示。

```
print(f"Today is {today:%B %d, %Y}")Today is June 23, 2021
```

如果你居住的国家是月写在日之前，你可以使用下面的格式。

```
print(f"Today is {today:%m-%d-%Y}")Today is 06-23-2021
```

## 3.衬垫号码

在某些情况下，数字以前导零书写，以便所有数字的位数相同。典型的用例可能是产品号或 id 号。

我们可以在 f 字符串中为变量放置任意数量的前导零。

```
a = 4b = 123print(f"Product numbers are \n{a:03} \n{b:03}")Product numbers are  
004  
123
```

“b:03”表示总共有 3 个数字，前导空格将用零填充。在一位数的情况下，我们有两个前导零。如果是“b:04”，数字就写成 0004 和 0123。

## 4.写表达式

f 字符串还允许在变量占位符中使用表达式。这些表达式可能涉及函数执行。这是一个方便的特性，因为我们不需要为只使用一次的值创建变量。

让我们做一个包含日期操作的例子。

```
from datetime import datetime, timedeltatoday = datetime.today().date()print(f"The test was 3 days ago which is {today - timedelta(days=3)}")The test was 3 days ago which is 2021-06-20
```

另一个例子是查找列表中的项目数，并将其用作变量。

```
mylist = [1, 2, 4, 6, 3]print(f"The list contains {len(mylist)} items.")The list contains 5 items.
```

## 结论

字符串是我们脚本的重要组成部分。出于调试目的，我们还将它们与 print 语句一起使用。字符串插值帮助我们充分利用 print 语句。它允许操作或定制字符串很容易。

F-strings 为字符串插值提供了清晰的语法和易读的代码。我们在文章中介绍的技巧为 f 弦的标准使用增加了更多的灵活性。

感谢您的阅读。如果您有任何反馈，请告诉我。