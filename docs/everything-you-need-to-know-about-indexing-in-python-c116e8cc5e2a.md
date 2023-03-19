# 关于 Python 中的索引，您需要知道的一切

> 原文：<https://towardsdatascience.com/everything-you-need-to-know-about-indexing-in-python-c116e8cc5e2a?source=collection_archive---------13----------------------->

## Python 中不同数据类型和结构的索引概述。

![](img/bb0248159ee410598432f9ba8aaa4592.png)

(src =[https://pixabay.com/images/id-1690423/](https://pixabay.com/images/id-1690423/)

# 介绍

在通用编程中，使用数据结构是很常见的。数据结构是由更小的数据类型组成的类型。数据结构的一个例子是列表或字典。数据结构允许我们方便地将几个组件作为同一个一致变量的成员进行组织和工作。正如您所想象的，这使得数据结构成为数据科学中非常重要的组成部分。假设数据结构是由更小的组件组成的，那么肯定有一种方法可以基于某些特性来访问各个组件。为此，我们使用索引。

索引很重要，因为它允许我们毫不费力地调用数据结构的一部分，以便单独处理结构内部的组件。当然，对于数据科学来说，掌握这一点是非常重要的，因为数据科学家可能会经常使用数据结构。

> [笔记本](https://github.com/emmettgb/Emmetts-DS-NoteBooks/blob/master/Python3/Indexing%20in%20Python.ipynb)

# 索引类型

在我们开始使用索引之前，让我们看看哪些类型实际上可以被索引。在 Python 编程语言中，可以使用给定类中的 __getitem__()方法对类型进行索引。这意味着我们可以将索引方法应用于任何类型，只需简单地添加这个方法，并返回准确的结果。为了尝试这一点，我们将首先创建一个类。考虑下面的例子:

```
class Winners: def __init__(self, first, second, third): self.first, self.second = first, second self.third = third
```

现在我们可以将 __getitem__()方法添加到这个新类中。这将增加使用简单索引从类中轻松获取我们的位置的能力。

```
def __getitem__(self, x):
```

对于这个例子，我认为最好的攻击方法是在初始化这个类的构造函数时创建一个字典。使用这种方法，我们将能够简单地调用基于数字的字典索引，以便接收这场比赛的位置。

```
class Winners: def __init__(self, first, second, third): self.first, self.second = first, second        self.third = third self.index = dict({1 : self.first, 2 : self.second, 3 : self.third})
```

最后，我们将结束 __getitem__()方法，只需用提供的数字调用字典键:

```
class Winners: def __init__(self, first, second, third): self.first, self.second = first, second self.third = third self.index = dict({1 : self.first, 2 : self.second, 3 : self.third}) def __getitem__(self, x): return(self.index[x])
```

虽然这当然很好，但是让我们记住，我们只是这里的日志赢家，所以任何超过 4 点的人都不包含在这个类中。记住，如果我们在这个类上调用索引 4，我们将得到一个 KeyError 作为回报。每当我们创建软件时，尤其是在我们的软件用户可能永远不会看到的类中，我们会希望抛出一些错误，使错误比这更明显一些。考虑到这一点，我们将在这个方法中添加一个 try 和 catch，以便打印出更详细的错误。最终结果看起来有点像这样:

```
class Winners: def __init__(self, first, second, third): self.first, self.second = first, second self.third = third self.index = dict({1 : self.first, 2 : self.second, 3 : self.third}) def __getitem__(self, x): try: return(self.index[x]) except KeyError: print("""KeyError!\nOnly keys 1-3 are stored in this                 class!""")
```

现在让我们试着索引这种类型。首先，我们当然需要初始化这个对象的一个新实例，然后我们将索引它。这是通过[]语法完成的:

```
race_winners = Winners("Nancy", "Bobby", "Reagan")
```

首先让我们试着用 4 来表示它，看看我们得到什么样的回报！

```
race_winners[4]KeyError!
Only keys 1-3 are stored in this class!
```

现在我们将在该类的索引中打印 1:3:

```
print(race_winners[1])print(race_winners[2])print(race_winners[3])Nancy
Bobby
Reagan
```

## Python 中的可索引类型

Python 编程语言提供了几种可以立即索引的数据类型和数据结构。本文中我们首先要看是字典数据结构。

```
dct = dict({"A" : [5, 10, 15], "B" : [5, 10, 15]})
```

我们可以使用相应的字典键来索引字典。这将给出给定键的值对。这将方便地为我们提供下一个数据结构，列表:

```
lst = dct["A"]
```

列表可以用我们想要访问的元素的位置来索引。例如，我们新列表的第二个元素是 10。我们可以用这个来称呼它

```
lst[1]
```

当然，这是一个，而不是两个，因为在 Python 中索引是从零开始的。不用说，使用列表索引肯定会派上用场。我们可以索引的另一种类型是字符串:

```
"Hello"[1]'e'
```

# 设置索引

与调用索引同等重要的是设置索引。设置索引不仅允许我们在列表和其他可迭代的数据结构上创建新的位置，还允许我们改变数据结构内部的现有值。此外，我们可以使用这种方法将关键字放入字典，将列添加到 Pandas 数据帧，等等。在 Python 中，索引设置调用 __setitem__()方法。让我们继续为此编写一个函数:

```
def __setitem__(self, x, y):
    pass
```

现在，我们将在这个新函数中编写一点逻辑，允许它将相应的字典键的值设置为新的输入值:

```
def __setitem__(self, x, y):self.index[x] = y
```

现在，我们可以将比赛位置的指数设置为新值:

```
print(race_winners.index.values())dict_values(['Nancy', 'Bobby', 'Reagan'])race_winners[2] = "John"print(race_winners.index.values())dict_values(['Nancy', 'John', 'Reagan'])
```

当然，同样的概念也适用于 Python 中的所有数据结构。我们可以将此应用于列表、字典值，但不能应用于字符串，如下所示:

```
z = [5, 10]z[1] = 1d = dict({"h" : 5, "z" : 6})d["z"] = 5assert d["z"] == d["h"]assert z[1] < z[0]
```

# 重要功能

现在我们已经了解了索引的基本知识，让我们来看一下在处理数据结构时可能会用到的一些重要函数。这些函数中有许多对于处理大量数据非常有用。应该注意的是，这些函数主要用于列表和矩阵，尽管有些函数可能适用于其他结构。这将是我们要使用的这些函数的列表:

```
lst = [5, 1, 6, 4, 5, 7, 3,5, 4, 3, 1, 2, 3, 4,]
```

## 1.插入()

第一个有用函数是 insert。Insert 将允许你在一个数组中的任何索引处放置任何值。如果您希望特定数组在某个位置包含特定组件，这将非常有用。它非常容易使用，只需添加两个参数，位置，然后是你想要添加的值。

```
lst.insert(5, 3)
```

## 2.追加()

下一个函数是 append 函数。这个函数经常用于生成列表，通常是在迭代中。这将为下一个可用索引添加一个值，并且只需要一个值作为参数。

```
lst.append(5)
```

对于这种特殊情况，更好的方法可能是 lambda with mapping，或者如果是 series 类型，可能是 apply 方法，在这种情况下，我将使用迭代循环和 append 函数来演示如何以这种方式使用它:

```
lst2 = []for z in lst: lst2.append(z * 3 + 2)
```

## 3.移除()

Remove 将从给定列表中删除一个值。注意，这需要一个值，而不是一个索引！它也只会移除给定值的一个实例。

```
lst.remove(1)
```

## 4.扩展()

extend 函数本质上只是 append()方法，但是允许我们将一个列表追加到列表的末尾:

```
lst.extend([101, 503])
```

## 5.计数()

Count 将返回一个整数，它是给定列表中某个元素的所有实例的计数。

```
lst.count(5)4
```

这个函数的一个很好的用例是计算模式。观察我如何使用 count 和字典在迭代循环中获得模式:

```
digit_n = {}for x in set(lst): cnt = lst.count(x) digit_n[cnt] = x mode = digit_n[max(digit_n.keys())]print(mode)5
```

## 6.排序()

我想讨论的最后一个重要列表函数是 sort()函数。该函数不带参数，将对列表进行逻辑排序，或者按照关键字参数的指定进行排序。在下面的例子中，我使用 sort 来查找列表的中值:

```
lst.sort()
medianindex = int(len(lst) / 2)print(medianindex)print(lst[medianindex])
```

# 结论

不用说，在编写任何涉及数据的代码时，索引是一件非常重要的事情。list 类的标准函数对于操纵、创建和从数据中获得洞察力也非常有用。我认为这是所有人都想知道的关于 Python 中索引的内容。非常感谢你的阅读，我希望你有一个美好的白天或夜晚休息！