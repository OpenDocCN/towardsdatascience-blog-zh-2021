# 让你轻松生活的 5 个 Python 技巧

> 原文：<https://towardsdatascience.com/5-python-tricks-that-will-ease-your-life-ed81987ec467?source=collection_archive---------1----------------------->

## 让你的脚本更有效率和吸引力。

![](img/cb4e69047570ca0411c2e4090db3e67f.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/simple?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

如果你正在或计划从事数据科学领域的工作，Python 可能会是你最好的伙伴。

尽管您将主要使用第三方库，但是全面理解 Python 基础非常重要。此外，这些库也在使用基本 Python 的强大功能。

在这篇文章中，我们将讨论 5 个简单的 Python 技巧，我认为它们将使你的脚本更加高效和吸引人。

## 招数 1

第一个是关于 f-strings，这是 Python 中最新的字符串插值方法。通过使用 f 字符串，我们可以将变量值放在一个字符串中。

这里有一个简单的例子:

```
name = "Jane"print(f"Hello {name}!")
Hello Jane!
```

f 弦是一种非常灵活的方法。假设我们将一个非常大的数字的值放在 f 字符串中。

```
number = 12000000print(f"The value of the company is ${number}")
The value of the company is $12000000
```

使用千位分隔符，输出可能更容易阅读。我们可以很容易地在 f 弦上做这样的修改。

```
print(f"The value of the company is ${number:,d}")
The value of the company is $12,000,000
```

## 招数 2

假设你有几个字符串存储在一个列表中。

```
mylist = ["Data", "Science", "Machine", "Learning"]
```

您希望将列表中的所有项目组合成一个字符串。解决这项任务有许多选择。例如，您可以在 For 循环中遍历列表，将所有字符串添加到一个字符串中。

```
myword = ""
for word in mylist:
   myword += word + " "print(myword)
Data Science Machine Learning
```

当然，这不是最佳解决方案。连接方法要简单得多。

```
print(" ".join(mylist))
Data Science Machine Learning
```

您可以使用任何字符来分隔单个字符串。

```
print("-".join(mylist))
Data-Science-Machine-Learning
```

## 招数 3

假设你需要找到一个字符串中最频繁出现的字符。

```
mystring = "aaaabbbccd"
```

一种方法是使用 Python 内置集合模块中的计数器对象。

```
from collections import Countercnt = Counter(mystring)
```

cnt 对象包含每个字符以及它们在字符串中出现的次数。我们可以使用 most_common 方法得到最频繁的一个。

```
cnt.most_common(1)
[('a', 4)]
```

这不是一个非常复杂的方法，但甚至有一个简单的方法。内置的 max 函数能够找到最常用的字符。我们只需要正确使用关键参数。

```
max(mystring, key=mystring.count)
'a'
```

默认情况下，max 函数返回最大值。在字母的情况下，它是字母表中的最后一个。

我们可以用同样的方法找到列表中最频繁出现的元素。

```
mylist = ["foo", "foo", "bar"]max(mylist, key=mylist.count)
'foo'
```

## 招数 4

这个是关于随机性的，随机性是数据科学中的一个基本概念。这里我们不会详细讨论随机性。然而，我们将看到一个从列表中选择一个随机项目的快速方法。

Python 内置的 random 模块有几个函数和方法。其中之一是 choice 方法，该方法可用于从可索引(或可下标)集合中选择随机元素。

我们来做一个例子。

```
import randommylist = ["Ashley", "Jane", "John", "Matt", "Jenny"]random.choice(mylist)
'Matt'
```

我们还可以在元组上使用 choice 方法。

```
mytuple = ("a","b","c","d","e")random.choice(mytuple)
'c'
```

但是，它不能用于不允许索引的集合(即不可订阅的集合)，如集合。

```
myset = {"a","b","c","d","e"}random.choice(myset)
TypeError: 'set' object is not subscriptable
```

## 招数 5

字典是 Python 中最常用的数据结构之一。它由键值对组成。字典中的键是唯一的，因此我们可以使用它们轻松地访问值。

例如，字典非常适合存储人们的年龄。姓名将作为键，年龄将作为值存储。

请注意，使用姓名作为字典关键字并不是最好的方法，因为很可能有两个或更多的人同名。为了清楚起见，我们将继续使用名称。然而，在现实生活中，我们可以给每个人分配一个唯一的 id 号，并将这些号码用作字典键。

考虑姓名和年龄存储在不同的列表中。我们可以很容易地将两个列表合并成一个字典，如下所示:

```
names = ["Jane", "John", "Adam", "Ashley"]ages = [25, 22, 27, 33]mydict = dict(zip(names, ages))mydict
{'Adam': 27, 'Ashley': 33, 'Jane': 25, 'John': 22}
```

zip 函数创建元组对。

```
for pair in zip(names, ages):
   print(pair)('Jane', 25) 
('John', 22) 
('Adam', 27) 
('Ashley', 33)
```

然后，dict 函数将这个元组集合转换成一个字典。

## 结论

我们介绍的技巧简单而实用。我相信总有一天这些技巧会简化你的代码。

最后但同样重要的是，如果你还不是[中级会员](https://sonery.medium.com/membership)并打算成为其中一员，我恳请你使用以下链接。我将从你的会员费中收取一部分，不增加你的额外费用。

[](https://sonery.medium.com/membership) [## 通过我的推荐链接加入 Medium—Soner yld RM

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

sonery.medium.com](https://sonery.medium.com/membership) 

感谢您的阅读。如果您有任何反馈，请告诉我。