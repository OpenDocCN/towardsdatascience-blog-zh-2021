# 5 个专家提示，让你的 Python 字典技能一飞冲天🚀

> 原文：<https://towardsdatascience.com/5-expert-tips-to-skyrocket-your-dictionary-skills-in-python-1cf54b7d920d?source=collection_archive---------9----------------------->

## 获得有价值的技能

## 你觉得你懂 Python 的字典吗？

![](img/f660926415fbc873698aef30de8bf343.png)

照片由 [Danylo Suprun](https://unsplash.com/@suprunph?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 您的旅程概述

*   [设置舞台](#caf5)
*   [快速回顾—基础知识](#f9cd)
*   [提示 1 —使用字典进行迭代](#666f)
*   [技巧 2 —自信地获取数值](#0290)
*   [提示 3——词典释义](#23bc)
*   [提示 4 —使用压缩功能](#942c)
*   [提示 5 —字典合并](#b23b)
*   [包装完毕](#f045)

# 搭建舞台

字典是 Python 中的基本数据结构之一。你用它来做*计数*、*分组*和*表示关系*。然而，并不是每个人都知道如何有效地使用字典。

字典使用不当往往会导致**冗长的代码**、**缓慢的代码**，甚至**细微的 bug**😧

当我开始使用 Python 时，我几乎不能从字典中访问和检索数据。但是几年后，我开始意识到掌握好词典是至关重要的。

用 Python 核心开发者之一的话说:

> 世界上有两种人:精通字典的人和十足的傻瓜。—雷蒙德·赫廷格

不要做一个傻瓜！在这篇博客中，我将向你展示 5 个技巧，让你的 Python 字典技能达到专业水平🔥

# 快速回顾—基础知识

创建词典的标准方法是使用以下语法:

```
# Creating an empty dictionary
empty_dict = {}# Standard way to create a dictionary
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}# Can use the dictionary constructor function
better_call_saul = dict([("Jimmy", 33), ("Kim", 31), ("Gus", 44)])
```

在上面的代码中:

*   名字`Jimmy`、`Kim`和`Gus`是字典的**键**。
*   年龄`33`、`31`和`44`是字典的**值**。
*   对`"Jimmy": 33`、`“Kim": 31`和`"Gus": 44`是**键值对**。

您可以使用以下语法访问字典`better_call_saul`中的值

```
# Getting Kim's age
print(better_call_saul["Kim"])**Output:** 31# Setting Jimmy's age
better_call_saul["Jimmy"] = 34
```

从上面的代码可以看出，字典是**可变的**。字典中的键也必须是不同的。

您可以使用字典毫不费力地添加或移除键值对:

```
# Setting a new key-value pair
better_call_saul["Jonathan"] = 54# Removing the key-value pair again (sorry Jonathan!)
del better_call_saul["Jonathan"]
```

最后，不是每个 Python 对象都可以是字典中的一个键。精确的规则是对象应该是**可散列的**。请放心，任何**不可变对象**都是可散列的。关于 hashable 的更深入的解释，请查看博客:

<https://betterprogramming.pub/3-essential-questions-about-hashable-in-python-33e981042bcb>  

# 技巧 1 —字典迭代

您应该知道的第一件事是，从 Python 版本开始，字典是**有序的:**

> 当你打印一个字典或者循环遍历它的元素时，你将会看到元素以它们被添加到字典中的相同顺序出现——Eric Matthes，Python 速成教程

## 遍历这些键

要遍历字典的键，可以使用方法`keys()`:

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}for name in better_call_saul.keys():
    print(name)**Output:** 
Jimmy
Kim
Gus
```

如您所见，通过使用`keys()`方法，您可以访问字典的键。但是等等！也可以这样写:

```
for name in better_call_saul:
    print(name)**Output:** 
Jimmy
Kim
Gus
```

如果你遍历字典，那么它将默认遍历所有的键😎

## 遍历这些值

如果您想遍历一个字典的值，那么您应该使用`values()`方法:

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}# Calculating the total age of the characters
total = 0
for age in better_call_saul.values():
    total += ageprint(total)**Output:**
108
```

## 遍历键和值

如果在一次迭代中既需要键又需要值，那么应该使用`items()`方法。让我们先看看它返回了什么:

```
print(better_call_saul.items())**Output:**
dict_items([('Kim', 31), ('Gus', 44)])
```

输出是一个**字典项目**对象。这种对象的功能与元组列表相同。因此，您可以使用以下语法:

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}for name, age in better_call_saul.items():
    print(f"{name} is {age} years old.")**Output:**
Jimmy is 33 years old.
Kim is 31 years old.
Gus is 44 years old.
```

# 技巧 2——自信地获取价值

考虑代码:

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}young_guy_age = better_call_saul["Nacho"]**Result:**
KeyError: 'Nacho'
```

您得到一个`KeyError`，因为键`“Nacho”`在字典`better_call_saul`中不存在。哎哟😬

这实际上很现实。通常你不能完全控制字典里的内容。您字典中的数据可能来自

*   网页抓取，
*   一个 API，
*   或者加载数据。

让你的程序这么容易崩溃会给你带来巨大的痛苦。相反，您可以使用`get()`方法:

```
young_guy_age = better_call_saul.get("Nacho", "Can't find")print(young_guy_age)**Output:**
Can't find
```

`get()`方法有两个参数:

1.  要为其检索相应值的键。
2.  字典中不存在关键字时的默认值。

如果没有给`get()`方法提供第二个参数，那么第二个参数默认为`None`。使用`get()`方法可以避免大量的键错误，从而使你的代码更加健壮。

> **提示:**与`get()`相关的方法是`setdefault()`方法。这两种方法都很棒，你也应该看看 [setdefault](https://www.programiz.com/python-programming/methods/dictionary/setdefault) 。

# 技巧 3——字典理解

你大概知道 [**列表理解**](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) 。列表理解是构建列表的一种 Pythonic 方式。您可以使用它们作为标准循环的强大替代:

```
# Gives the square numbers from 0 to 9
squares = []
for num in range(10):
    squares.append(num ** 2)# List comprehensions do the same thing!
squares = [num ** 2 for num in range(10)]
```

你知道 Python 也支持 [**字典理解**](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) 吗？如果你想要一个快速建立字典的方法，那就不要再找了:

```
# Manually making a squares dictionary
squares = {0:0, 1:1, 2:4, 3:9, 4:16, 5:25, 6:36, 7:49, 8:64, 9:81}# Dictionary comprehensions do the same thing!
squares = {num: num ** 2 for num in range(10)}
```

作为另一个例子，考虑一个名字列表:

```
names = ["Jimmy", "Kim", "Gus"]
```

假设您想要构建一个字典，其中:

*   字典的关键字是列表`names`中的名字。
*   字典的值是名称的长度。

你可以用字典理解在一行中做到这一点！

```
length_of_names = {name: len(name) for name in names}
print(length_of_names)**Output:**
{'Jimmy': 5, 'Kim': 3, 'Gus': 3}
```

# 技巧 4——使用压缩功能

一个常见的问题是有两个单独的列表，您希望将它们合并到一个字典中。例如，假设您有以下两个列表:

```
names = ["Jimmy", "Kim", "Gus"]
ages = [33, 31, 44]
```

如何将`names`和`ages`组合成之前的字典`better_call_saul`？在我向您展示魔术之前，让我们先验证一个简单的方法是否有效:

```
# The "enumerate" approach
better_call_saul = {}
for idx, name in enumerate(names):
    better_call_saul[name] = ages[idx]
```

还不错！在这种情况下,`enumerate`功能通常很有用。

然而，还有一种更巧妙的方法:将`zip`函数与`dict`构造函数一起使用！

```
# The "zip" approach
better_call_saul = dict(zip(names, ages))
```

拉链功能的工作原理就像夹克上的拉链——它从头到尾将物品配对。

值`zip(names, ages)`是一个 **zip 对象**，所以不那么容易理解。但是如果你把它变成一个列表，它就会变得更容易处理:

```
print(list(zip(names, ages)))**Output:**
[('Jimmy', 33), ('Kim', 31), ('Gus', 44)]
```

这看起来像是`dict`构造器能吃的东西！在使用`dict`构造函数之前，您甚至不需要将 zip 对象转换成列表😍

> **有趣的事实:**“zip”方法在执行速度上明显快于“enumerate”方法。

# 技巧 5 —字典合并

有时需要 [**合并两个字典**](https://www.geeksforgeeks.org/python-merging-two-dictionaries/) 。字典合并将两个字典中的元素合并成一个字典。

在 Python 中有许多方法可以进行字典合并:

*   我将向您展示一种使用`update()`方法进行字典合并的传统方法。
*   我将向您展示 Python 3.9 中引入的令人敬畏的**字典合并操作符**。

## 与更新方法合并

假设你有两本字典:

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}
breaking_bad = {"Walter": 46, "Jesse": 23, "Jimmy": 38}
```

如果你想合并字典，那么你可以使用`update()`方法:

```
# Updates better_call_saul
better_call_saul.update(breaking_bad)
print(better_call_saul)**Output:**
{'Jimmy': 38, 'Kim': 31, 'Gus': 44, 'Walter': 46, 'Jesse': 23}
```

您现在已经用来自`breaking_bad`的新信息更新了`better_call_saul`。注意键`“Jimmy”`的值是 38。先前的值 33 已被覆盖。

## 使用字典合并运算符进行合并(Python 3.9+)

在 Python 3.9 中引入了**字典合并操作符** `|`和`|=`。两者都简化了字典合并。如果你又有以前的字典`better_call_saul`和`breaking_bad`

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}
breaking_bad = {"Walter": 46, "Jesse": 23, "Jimmy": 38}
```

然后你可以写:

```
better_call_saul |= breaking_bad
print(better_call_saul)**Output:**
{'Jimmy': 38, 'Kim': 31, 'Gus': 44, 'Walter': 46, 'Jesse': 23}
```

操作符`|=`是**原地字典合并操作符**。它就地修改字典。如果您想要一个包含合并内容的新字典，您可以使用**字典合并操作符** `|`:

```
better_call_saul = {"Jimmy": 33, "Kim": 31, "Gus": 44}
breaking_bad = {"Walter": 46, "Jesse": 23, "Jimmy": 38}gilligan_universe = better_call_saul | breaking_bad
print(gilligan_universe)**Output:**
{'Jimmy': 38, 'Kim': 31, 'Gus': 44, 'Walter': 46, 'Jesse': 23}
```

如果您有两个以上的字典要合并，Python 3.9 中引入的新语法非常有用。

# 包扎

> 你现在可以像一个懂字典的人一样昂首阔步地着手未来的项目了！

![](img/0396dcbd9a42f2727441befe268c65b7.png)

照片由[内森·杜姆劳](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

喜欢我写的东西？查看我的博客文章[类型提示](/modernize-your-sinful-python-code-with-beautiful-type-hints-4e72e98f6bf1)、[黑色格式](/tired-of-pointless-discussions-on-code-formatting-a-simple-solution-exists-af11ea442bdc)和[Python 中的下划线](https://medium.com/geekculture/master-the-5-ways-to-use-underscores-in-python-cfcc7fa53734)了解更多 Python 内容。如果你对数据科学、编程或任何介于两者之间的东西感兴趣，那么请随意在 LinkedIn 上加我，并向✋问好