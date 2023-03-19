# 不要过度使用“+”来连接 Python 中的字符串

> 原文：<https://towardsdatascience.com/stop-overusing-to-join-strings-in-python-8a432f0daeab?source=collection_archive---------19----------------------->

## 这里有三种选择，它们将帮助你做更多的事情，而不仅仅是连接字符串

![](img/8f0d1124fa8046ae8c8e4b71746260b7.png)

照片由[纳丁·沙巴纳](https://unsplash.com/@nadineshaabana?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

数据科学家在收集和清理数据时必须处理的一项常见任务是处理字符串。这包括格式化以及连接字符串(也称为字符串连接)。

在 Python 和其他编程语言中连接字符串就像使用加号运算符`+`一样简单。在 Python 中，您可能已经使用下面的代码数百次来连接字符串。

```
>>> str1 = "Hello "
>>> str2 = "World">>> str1 + str2
"Hello World"
```

这种方法很好。然而，这在处理列表、数据框架时变得不切实际，或者如果你的目标是可读性。

在本文中，我将向您展示 3 种替代方法，它们不仅可以帮助您连接字符串，还可以让您轻松地连接列表元素，在 Python 中正确格式化字符串，并使调试变得不那么复杂。

# f 弦

Python f-string 是在 Python 3.6 中引入的，它允许我们像加号操作符那样连接字符串；然而，f 字符串使用花括号`{}`来存储将被格式化为字符串的值。

例如，让我们使用 f-string 打印句子“Python 由吉多·范·罗苏姆创建，并于 1991 年发布”。在这种情况下，我们将使用名称“吉多·范·罗苏姆”和年份“1991”作为变量。

让我们来看看 f 字符串的语法。

```
>>> name = 'Guido van Rossum'
>>> year = 1991>>> f'Python was created by {name} and released in {year}'
```

它的语法比`+`操作符更简洁，不是吗？

您也可以使用 f-string 格式化文本数字或日期。假设我们从`datetime`库获取了今天的日期，并希望将其添加到文本中。

```
>>> import datetime
>>> now = datetime.datetime.now()>>> f'Today is {now:%B} {now:%-d}, {now:%Y}'
```

Python 会打印`‘Today is October 1, 2021’`。作为旁注，`%B`、`%-d` 和`%Y`是格式代码。您可以在这个[Python strftime cheat sheet](https://strftime.org/)中找到其他可用的时间格式代码。

现在让我们格式化一个数值。

```
>>> gpa = 3.355>>> f'His GPA is {gpa:.1f}'
```

Python 会将数字四舍五入到第一个小数，并打印出`‘His GPA is 3.4’`在这个例子中，`.f`是一个表示“浮点数”的格式代码，所以在上面的例子中，我们指定了精度的`1`位数。你可以在这个链接上找到更多类似这些[的格式代码。](https://thepythonguru.com/python-string-formatting/)

f-string 很酷的一点是，开发人员不断增加新的功能。Python 在 Python 3.8 中引入了一个全新的特性，将`=`添加到 f 字符串中。这简化了频繁的打印调试，因此不用编写下面的代码来打印变量名及其值。

```
>>> python_version = 3.8
>>> f"python_version={python_version}"'python_version=3.8'
```

你现在只能这样写:

```
>>> f"{python_version=}"'python_version=3.8'
```

# 加入()

当您想要连接存储在一个列表中的多个字符串时，最简单的方法是使用`join()`方法。您只需要在使用`join()`方法之前指定一个分隔符。

假设我们之前使用了同一个句子，但是现在每个单词都存储在一个名为`words`的列表中。

```
words = ['Python', 'was', 'created', 'by', 'Guido', 'van', 'Rossum', 'and', 'first', 'released', 'in', '1991']
```

我们使用 join 方法和一个空字符串`‘ ’`作为分隔符来构建之前的句子。

```
>>> ' '.join(words)
```

`join()`方法不仅是连接列表元素的有效方法，而且由于其性能优于`+`操作符。`[join()](/do-not-use-to-join-strings-in-python-f89908307273)`[方法可以比使用](/do-not-use-to-join-strings-in-python-f89908307273) `[+](/do-not-use-to-join-strings-in-python-f89908307273)` [来连接列表中的字符串快 4 倍。](/do-not-use-to-join-strings-in-python-f89908307273)

# str.format()

我们可以使用`str.format()`在 Python 中连接字符串。我们只需要为我们想要添加到字符串中的每个变量插入花括号`{}`。花括号操作符将帮助我们在 Python 中格式化字符串。

让我们看一看。

```
>>> name = 'Guido van Rossum'
>>> year = 1991

>>> "Python was created by {} and released in {}".format(name, year)
```

`str.format()`优于`+`操作符的一个原因是我们不需要在连接之前显式地将整数转换成字符串。在上面的例子中，我们不必像使用`+`操作符那样将`year`变量转换成字符串。

不幸的是，使用`str.format()`的一个缺点是，当处理许多变量和长字符串时，代码会变得很长。这就是 f 弦优于`str.format()`的原因。

就是这样！这是 Python 中用`+`操作符连接字符串的 3 种替代方法。明智地使用它们来编写更好的 Python 代码。

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。每月 5 美元，让您可以无限制地访问数以千计的 Python 指南和数据科学文章。如果你使用[我的链接](https://frank-andrade.medium.com/membership)注册，我会赚一小笔佣金，不需要你额外付费。

[](https://frank-andrade.medium.com/membership) [## 通过我的推荐链接加入媒体——弗兰克·安德拉德

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

frank-andrade.medium.com](https://frank-andrade.medium.com/membership)