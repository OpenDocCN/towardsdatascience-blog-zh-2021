# 简化您生活的 3 个 Python 技巧

> 原文：<https://towardsdatascience.com/3-python-tricks-that-will-ease-your-life-6b48952caedf?source=collection_archive---------7----------------------->

## 举例说明

![](img/5a2501b7d893decee1349fe3c2879d8c.png)

照片由 [Z S](https://unsplash.com/@kovacsz1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/simple?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Python 是数据科学生态系统工作人员的首选语言。Python 成为该领域的主要选择有几个原因，比如简单易学的结构、简单的语法和丰富的第三方库选择。

我最喜欢 Python 的地方是它如何简化某些任务。在这篇文章中，我将分享 3 个我认为非常实用的技巧或诀窍。我在工作中使用过它们，我想你也会发现它们很有用。

我想强调一下，这些技巧并不是一些高级的 Python 特性。其实都可以认为是基本操作。然而，它们对于某些任务来说是很方便的。

## 1.字符串插值

第一个是关于字符串插值，它允许通过使用变量名来操作字符串。我们经常在打印功能中使用它们来调试代码或确认结果。

字符串插值的一个典型用例是创建包含基于某些变量的信息的文件路径。例如，我使用字符串插值法将日期信息集成到文件路径中。

Python 的 datetime 模块返回不带前导零的月和日，因此一些值显示为一位数字。

```
from datetime import datetimemydate = datetime(2020, 4, 2, 16, 40)print(mydate)
2020-04-02 16:40:00mydate.month
4mydate.day
2
```

月和日都可以包含一位数或两位数。因此，对于只有一个数字的路径，我们需要添加一个前导零，使路径具有标准格式。

让我们首先创建几个没有标准化月份和年份数字的文件路径。

```
# one digit month and day
mydate = datetime(2020, 4, 2)
file_path = f"Results_{mydate.year}{mydate.month}{mydate.day}"print(file_path)
Results_202042# two-digit month and day
mydate2 = datetime(2020, 11, 24)
file_path = f"Results_{mydate2.year}{mydate2.month}{mydate2.day}"print(file_path)
Results_20201124
```

正如我们从打印的文件路径中注意到的，它们不是标准格式。幸运的是，向字符串中使用的变量名添加前导零非常简单。

```
mydate = datetime(2020, 4, 2)
file_path = f"Results_{mydate.year}{mydate.month:02}{mydate.day:02}"print(file_path)
Results_20200402
```

在我们的例子中，“02”意味着相关变量将占用两位数，前导字符变成零。如果月或日包含两位数，Python 不会添加前导零，因为变量已经占用了两位数。因此，我们将保持一个标准。

## 计数器功能

下一个是 Python 的 collections 模块中的 Counter 函数。它是 dictionary 的子类，用于计算可散列对象的数量。Counter 返回一个字典，其中的项存储为键，计数存储为值。

我们先做一个单子上的例子。

```
from collections import Counterlist_a = [1, 2, 3, 3, 3, 5, 5]Counter(list_a)
Counter({1: 1, 2: 1, 3: 3, 5: 2})
```

Python 列表也有一个 count 方法，但是它只能用于计算单个元素的出现次数。

```
list_a.count(3)
3
```

counter 函数也可以用来计算字符串中字符出现的次数。

```
mystring = "aaaabbcbbbcbbcccaaaa"Counter(mystring)
Counter({'a': 8, 'b': 7, 'c': 5})
```

我们还可以计算一篇文章中的字数和频率。这里有一个简单的例子。

```
mytext = "Python python data science Data Science data"Counter(mytext.lower().split(" "))
Counter({'data': 3, 'python': 2, 'science': 2})
```

我们首先将所有字符转换成小写或大写以保持标准。下一步是在空格处分割字符串以提取单词。然后我们可以应用计数器函数。

## 对列表进行排序

在 Python 中对列表进行排序非常简单。我们可以使用 sort 函数对列表进行排序，并且不返回任何内容。另一个选项是 sorted 函数，它返回一个列表的排序版本。

考虑一个稍微复杂一点的情况。我们有一个包含其他列表作为其元素的列表。例如，我们可能有一个成员列表，每个元素可能代表一些关于他们的信息，比如名字、姓氏和年龄。

```
members = [["John", "Doe", 28], 
           ["Betsy", "Ash", 32], 
           ["Megan", "Sam", 44]]
```

我们可能希望根据名字、姓氏或年龄对成员进行排序。Python 允许通过使用 sort 函数的 key 参数进行这种定制排序。

下面的代码根据姓氏对成员列表进行排序。

```
members.sort(key = lambda inner: inner[1])members
[['Betsy', 'Ash', 32], 
 ['John', 'Doe', 28], 
 ['Megan', 'Sam', 44]]
```

我们使用嵌套列表元素的索引。例如，“inner[2]”根据年龄对列表进行排序。

```
members.sort(key = lambda inner: inner[2])members
[['John', 'Doe', 28], 
 ['Betsy', 'Ash', 32], 
 ['Megan', 'Sam', 44]]
```

## 结论

我们已经介绍了 3 个简单而有用的 Python 技巧。您可能并不总是需要使用它们，但是在某些情况下，这些技巧可以有效地解决问题。令人惊讶的是 Python 为各种情况提供了简单而直观的方法。本文中的 3 个例子可以被认为是 Python 简单性的很小一部分。

感谢您的阅读。如果您有任何反馈，请告诉我。