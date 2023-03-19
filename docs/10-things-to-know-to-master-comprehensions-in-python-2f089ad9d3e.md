# 掌握 Python 语言理解的 10 件事

> 原文：<https://towardsdatascience.com/10-things-to-know-to-master-comprehensions-in-python-2f089ad9d3e?source=collection_archive---------28----------------------->

## 列表理解，字典理解，等等…

![](img/b8cb7cc25202c3ababac1d06743a8e4f.png)

Jay Patel 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们经常使用的一个重要 Pythonic 特性是理解。最值得注意的是列表理解——一种创建列表的简明方法。但是，这个特性可能会让初学者感到困惑。此外，除了列表理解，还有其他一些相关的基于理解的技术，如字典和集合理解。

事不宜迟，让我们开始吧——探索 10 个基本的构建模块来理解 Python 中的理解。

## 1.列表理解的基本形式

让我们从语法开始。列表理解是从现有的 [iterable](/why-iterables-are-powerful-in-python-understand-these-5-distinct-usages-130f364bd0ba) 中创建列表的简洁方法。

```
# The syntax for list comprehension
created_list = [expression for item in iterable]
```

它所做的是依次将每一项应用到表达式，表达式将返回一个值，该值将作为一项进入创建的列表。为了帮助你理解它的意思，我们可以使用扩展的形式——一个`for`循环来说明列表理解是如何工作的。

```
# The expanded form using for loop
created_list = []
for item in iterable:
    created_item = certain_expression_to_process_the_item
    created_list.append(created_item)
```

在`for`循环中的`created_list`将等同于从列表理解中创建的那个。

## 2.从 Iterable 创建列表

最常见的用例是从现有的 iterable 创建一个 list 对象。需要注意的是，Python 有很多种可迭代对象，比如字符串、列表、集合、字典、地图对象等等。任何 iterable 都可以用于列表理解。没有必要给你看所有的例子。因此，我将简单地向您展示来自集合、字典和元组的列表理解。

列表理解(集合、字典和元组)

如果您知道列表理解的语法，那么一切都应该很简单，除了`dict`对象，我使用`items`方法来检索键值对。在表达式中，`key`和`value`都可以用来为列表对象创建项目。

## 3.保存需要的物品

当我们使用列表理解时，我们并不总是想要将现有列表中的每一项都发送给表达式来创建新项。在这种情况下，我们可以应用条件评估来检查是否应该包含某个项目。一般形式如下:

```
# condition in a list comprehension
items = [expression for item in iterable if condition]# Equivalent for loop
items = []
for item in iterable:
    if condition_of_the_item:
        created_item = expression
        items.append(created_item)
```

下面是一个简单的例子，告诉你我们只保存 3 或 5 的倍数的斐波那契数列。

```
>>> fibonacci = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
>>> squares_fib = [x*x for x in fibonacci if x%3 == 0 or x%5 == 0]
>>> squares_fib
[0, 9, 25, 441]
```

## 4.条件赋值(三元表达式)

Python 中的一个特殊表达式是三元表达式。您可以使用一行代码，而不是编写多行版本的`if…else…`语句。

```
a if condition else b
```

如果条件为`True`，则该表达式的计算结果为`a`，如果条件为`False`，则该表达式的计算结果为`b`。我们可以在列表理解中应用三元表达式，它会对原列表的条目做条件赋值:`[expr0 if condition else expr1 for item in iterable]`。应用此语法，我们有以下示例:

列表理解:三元表达式

## 5.嵌套列表理解

到目前为止，你应该明白，从某种角度来说，列表理解是一种代替`for`循环来创建列表对象的方式。我们还知道我们可以嵌套 for 循环:

```
for items in items_list:
    for item in items:
        expression
```

有没有可能把这种嵌套的 for 循环转化为列表理解？是的，我们可以使用嵌套列表理解。观察下面的例子。

嵌套列表理解

觉得很难理解？你可以简单的从第一个 for 循环开始读，这是第一级，第二个 for 循环是内循环。有了这个，一切就好理解了。

从技术上讲，您可以为嵌套列表理解编写多个级别。然而，为了更好的可读性，我不认为有两个以上的层次是一个好主意。

## 6.替换地图()

内置的`[map](https://docs.python.org/3/library/functions.html#map)`函数将一个函数应用于 iterable 的每一项，创建另一个 iterable——`map`对象。如您所见，这与列表理解非常相似，在列表理解中，表达式应用于 iterable 的每个项目。因此，有些人使用`map`来创建一个列表，如下所示。

```
>>> animals = ['tiger', 'Lion', 'doG', 'CAT']
>>> uppercased_animals = list(map(str.upper, animals))
>>> uppercased_animals
['TIGER', 'LION', 'DOG', 'CAT']
```

需要注意的是，我们需要使用一个`list`构造函数，其中我们传递了一个不同于`list`对象的可迭代对象`map`。我们可以使用列表理解来代替使用`map`函数。

```
>>> [x.upper() for x in animals]
['TIGER', 'LION', 'DOG', 'CAT']
```

尽管`map`函数有其他用途，但是当目标是创建一个列表对象时，列表理解通常更具可读性。

## 7.适用时使用列表构造函数

关于列表理解，要避免的一个误用是在适当的时候使用列表构造函数。什么是列表构造函数？所谓的`list`功能。

```
*class* **list**([*iterable*])Rather than being a function, [list](https://docs.python.org/3/library/stdtypes.html#list) is actually a mutable sequence type, as documented in [Lists](https://docs.python.org/3/library/stdtypes.html#typesseq-list) and [Sequence Types — list, tuple, range](https://docs.python.org/3/library/stdtypes.html#typesseq).
```

正如函数签名所示，列表构造函数可以直接接受任何 iterable。因此，当您不操作 iterable 中的项目时，您应该将它直接发送给 list 构造函数。考虑下面的代码片段来进行对比。

列表理解与列表构造函数

上面的例子只是提供了一个概念证明。要点是，除非你操作 iterable 的项(例如，应用一个函数)，否则你应该直接使用 list 构造函数。以下是另一个例子，供感兴趣的读者参考。

```
>>> numbers_dict = {1: 'one', 2: 'two'}
>>> **# list comprehension**
>>> [(key, value) for key, value in numbers_dict.items()]
[(1, 'one'), (2, 'two')]
>>> **# list constructor**
>>> list(numbers_dict.items())
[(1, 'one'), (2, 'two')]
```

## 8.集合理解

除了列表理解，我们还可以使用理解从现有的 iterable: `{expression for item in iterable}`创建一个集合。与列表理解相比，集合理解使用大括号而不是方括号。当我们使用集合理解时，还有两件事需要注意。

*   创建的集合中没有重复项。即使将从表达式中创建重复项，最终的 set 对象中也只会保留一个副本。
*   按照设计，set 对象只能存储可哈希的数据，比如字符串和整数，而不能存储列表和字典。

下面的代码向您展示了集合理解的一个用例。顺便提一下，集合理解也支持上面讨论的许多其他操作，比如条件赋值。感兴趣的读者可以探索一下这些特性。

```
>>> numbers = [-3, -2, -1, 1, 2, 3, 4, 5]
>>> squares_set = {x*x for x in numbers}
>>> squares_set
{1, 4, 9, 16, 25}
```

## 9.词典理解

你可能已经猜到了，除了 list 和 set comprehension，你不会惊讶地知道我们可以使用 dict comprehension 来创建一个`dict`对象:`{key_expr: value_expr for item in iterable}`。与其他两种理解不同，字典理解需要两个表达式，一个表示键，另一个表示值。需要注意的一点是字典键必须是可哈希的，所以`key_expr`应该产生一个可哈希的对象。

```
>>> numbers = [-3, -2, -1, 1, 2, 3, 4, 5]
>>> squares_dict = {x: x*x for x in numbers}
>>> squares_dict
{-3: 9, -2: 4, -1: 1, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

## 10.生成器表达式

通常，最常用的内置数据容器是元组、列表、字典和集合。我们对后三种有所理解。有没有元组理解？可惜没有这回事。就潜在的语法而言，您可能会想到一个可能的实现:`(expression for item in iterable)`。

注意圆括号的用法吗？就像元组一样。有趣的是，这种用法确实存在，但它不是创建一个元组对象。相反，它创建了一个[生成器](https://medium.com/swlh/generators-in-python-5-things-to-know-c76a1f60427a)，这是一种特殊的迭代器，具有更好的内存效率。因为不像其他迭代器那样在内存中加载所有的项，生成器在需要的时候呈现一个项，这避免了加载所有项的开销。因此，当您处理大量数据时，可以考虑使用生成器而不是其他迭代器，比如列表或元组。下面的代码向您展示了首选生成器的用例。

```
>>> numbers = list(range(10_000_000))
>>> **# Creating a list of squares using list comprehension**
>>> squares = [x*x for x in numbers]
>>> squares.__sizeof__()
89095144
>>> **# Using generator instead**
>>> squares_gen = (x*x for x in numbers)
>>> squares_gen.__sizeof__()
96
>>> # Having the same result
>>> sum(squares) == sum(squares_gen)
True
```

正如您所看到的，中间步骤(list vs. generator)向您展示了一个从生成器表达式创建的生成器，与 list 对象相比，它的大小微不足道，突出了生成器的性能。

## 结论

本文回顾了您需要了解的 Python 理解技巧的所有基本内容。这些技术是创建所需数据结构的简洁方法，只要适用，就应该使用它们。

感谢阅读这篇文章。通过[注册我的简讯](https://medium.com/subscribe/@yong.cui01)保持联系。还不是中等会员？[用我的会员链接](https://medium.com/@yong.cui01/membership)支持我的写作。