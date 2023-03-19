# Python 大师用这 6 样东西来设定

> 原文：<https://towardsdatascience.com/master-python-sets-with-these-6-things-23dce2c7e0d8?source=collection_archive---------24----------------------->

## 在 Python 项目中明智地使用集合

![](img/9e6c076b15d4786b8ba7f9f484df85bb.png)

由[克里斯托弗·罗拉](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在每个 Python 项目中，我们到处都使用内置数据类型。在数据容器中，我注意到使用最多的是列表和字典，其次是元组。不知何故，集合是最少使用的。但是，有一些特殊的方法是集合所独有的，这将使我们的工作变得得心应手。在这篇文章中，我想回顾一下集合的所有重要基础知识。在您的项目中使用 set 会更舒服。

## 1.结构:字面、构造和理解

当我们创建一个 set 对象时，最常见的方式是在我们知道什么对象构成了`set`对象的情况下使用 literal 方式。具体来说，我们使用一对花括号将元素括起来。

集合创建

我们也可以直接使用 set 构造函数，在其中我们可以传递任何 iterable，比如列表、元组和字典。下面是一些例子。

```
>>> # Create a set from a list using the constructor
>>> set([1, 3, 2, 3, 2])
{1, 2, 3}
>>> # Create a set from a dictionary using the constructor
>>> set({1: "one", 2: "two"})
{1, 2}
```

你可能听说过列表综合，以类似的方式，我们可以使用集合综合和现有的 iterable 来创建集合。

```
>>> # Starting with a list of integers
>>> numbers = [-3, -2, -1, 1, 2, 3]
>>> # Create a set from the integers
>>> squares = {x*x for x in numbers}
>>> squares
{9, 4, 1}
```

如您所见，集合理解具有以下语法。这就像列表理解，除了我们用花括号代替方括号。

```
{expression for item in iterable}
```

## 2.哈希能力

并非所有数据都可以保存在集合中，只有可哈希对象可以作为集合项目。在 Python 中，哈希是应用哈希算法将对象转换为整数形式的哈希值的过程。然而，并不是每个对象都是可散列的。通常，只有不可变的对象是可哈希的，比如整数、字符串和元组。相比之下，可变对象在默认情况下是不可哈希的，一些内置的可变对象类型包括列表、集合和字典。下面的代码向您展示了这些情况。

```
>>> # Sets consist of immutable objects
>>> {1, "two", (3, 4)}
{1, (3, 4), 'two'}
>>> # Mutable objects can't be set items
>>> {[1, 2, 3]}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

找出对象的哈希能力的一种方法是简单地使用内置的哈希函数。您将看到不可变对象与有效的哈希值相关联，而可变对象则不是。

```
>>> hash("Hello, World!")
-7931929825286481919
>>> hash({1: "one", 2: "two"})
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'dict'
```

## 3.非常适合会员测试

您可能想知道为什么集合只存储可散列的对象，因为在幕后，Python 使用散列表来保存这些数据。本质上，在哈希表中，我们使用哈希值来跟踪存储的对象。最大的好处是条目的查找时间恒定，这使得集合在成员关系方面是一个完美的数据结构。

作为直接比较，知道一个列表或一个元组是否包含一个特定的项需要相当长的时间，因为 Python 需要遍历所有的项才能知道成员资格是否为真。下面的代码向您展示了我们如何对集合和列表之间的成员资格检查进行计时。

成员资格检查:列表与集合

如您所见，即使 set 对象的项目数变为 100，000，成员资格检查时间也是一致的。相比之下，检查成员资格所需的时间几乎与列表对象成线性增长，这突出了与列表相比，集合的成员资格检查的效率。

## 4.检查集合之间的关系和操作

当我们有多个集合时，我们可以评估它们之间的关系。为了简单起见，让我们研究最基本的场景:两个集合对象之间可能的关系。下面的总结向您展示了关于集合之间的关系和操作的最常用的方法。

```
**Check Relationships**
**issubset**: whether a set is a subset of the other
**issuperset**: whether a set is a superset of the other
**isdisjoint**: whether two sets have no intersection
<=: equivalent to issubset
<: a proper subset
>=: equivalent to issuperset
>: a proper superset**Between-set Operations**
**intersection**: a set of items that belong to both sets
**difference:** a set of items that belong to the set but not others **union**: a set of all the items from both sets
**symmetric_difference**: a set of items that belong to only one set
&: equivalent to intersection
-: equivalent to difference
|: equivalent to union
^: symmetric difference
```

这些定义应该很简单，好奇的读者可以自己尝试这些操作。需要注意的一点是，您不必使用集合作为调用这些方法的参数，因为它们可以接受任何可迭代对象。下面是一个简单的例子。

```
>>> {1, 2}.issubset([1, 2, 3])
True
```

## 5.使用和更新成员

当我们使用集合对象中的所有成员时，一种常见的方式是 for 循环，因为集合是一种可迭代的对象，如下所示。

```
>>> for number in {1, 2, 3}:
...     print(number)
... 
1
2
3
```

如果您想要使用集合中的一个项目，请不要使用订阅，因为集合不支持此操作。原因很简单——set 项没有排序，它们也不是序列数据，因此当您使用 subscription 时，它不知道应该呈现哪个项。

```
>>> {1}[0]
Traceback (most recent call last):
  File "<input>", line 1, in <module>
TypeError: 'set' object is not subscriptable
```

相反，您可以使用`pop`方法，该方法将返回一个项目。然而，与此同时，该项目将从设置对象中删除，你必须记住这个副作用。如果您想简单地删除一个项目，您可以使用`remove`方法，该方法将返回`None`。

```
>>> primes = {2, 3, 5, 7}
>>> primes.pop()
2
>>> primes
{3, 5, 7}
>>> primes.remove(3)
>>> primes
{5, 7}
```

请注意，当您使用`pop`方法时，如果器械包是空的，则`KeyError`将会升高。如果您使用`remove`方法移除不在 set 对象中的项目，您也会遇到`KeyError`。为了避免引发`KeyError`的可能性，一个相关的方法是`discard`，当它是一个实际的成员时，它将移除指定的项，或者忽略移除而不引发`KeyError`异常。

如果要移除 set 对象中的所有项目，只需调用`clear`，如下图所示。有些人可能会想到做`primes = set()`，不推荐这样做，因为它做的是将一个空的 set 对象赋给 primes 变量，而不是真正修改同一个 set 对象。

```
>>> primes = {2, 3, 5, 7}
>>> primes.clear()
>>> primes
set()
```

要向 set 对象添加一个项目，只需调用:

```
>>> primes = {2, 3, 5, 7}
>>> primes.add(11)
>>> primes
{2, 3, 5, 7, 11}
```

当您想要添加多个项目时，可以使用`update`方法。类似于前面提到的方法(例如，`issubset`)，我们可以在`update`方法中使用任何可重复项，如下所示。

```
>>> primes.update([5, 7, 11, 13, 17])
>>> primes
{2, 3, 5, 7, 11, 13, 17}
```

## 6.另一种选择:冷冻

最后但并非最不重要的一点是，有必要了解集合数据类型的一种替代形式——冻结集合(`frozenset`)。顾名思义，冻结集是不可变的，因此一旦创建，就不能修改项目。请参见以下代码片段中的示例。

```
>>> primes_frozen = frozenset([2, 3, 5])
>>> primes_frozen.add(7)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'add'
```

还有一点可能有些人不知道，就是你可以拿一套和一套冷冻的做比较。不是因为他们看起来不同的类型而返回`False`，而是只通过他们的成员进行比较，如下所示。

```
>>> {1, 2, 3} == frozenset([1, 2, 3])
True
>>> {1, 2, 3} < frozenset([1, 2, 3, 4])
True
```

您可能想知道，既然现有的集合数据类型似乎可以提供一切，我们为什么要使用冻结的集合。考虑使用冻结集有几个原因。

*   当您使用集合对象并且不希望项目被意外更改时，这是使用冻结集合的好时机，因为它具有不变性，可以防止不希望的更改。
*   不变性伴随着 hashability，hash ability 允许在 set 对象中包含冻结的集合。因为集合是可变的，所以它们不能是集合项。请参考下面的代码进行快速对比。

```
>>> {{1, 2}, {2, 3}}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'set'
>>> {frozenset([1, 2]), frozenset([2, 3])}
{frozenset({2, 3}), frozenset({1, 2})}
```

## 结论

在本文中，我们回顾了 Python 中使用 set 对象的最基本的操作。正如您所看到的，虽然与列表和字典相比，它们不太常用，但是集合具有有用的特征，比如集合之间的唯一成员和操作，这使得它们在特定的用例中很有价值。