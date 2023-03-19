# Python 中的 append 和 extend List 方法有什么区别？

> 原文：<https://towardsdatascience.com/what-is-the-difference-between-append-and-extend-list-methods-in-python-a29dc21f8076?source=collection_archive---------26----------------------->

## 探索如何在 Python 中追加或扩展列表

![](img/69afdfc2fc288eae63ffe2184161f07e.png)

汤姆·威尔逊在 [Unsplash](https://unsplash.com/s/photos/dominos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

列表可能是 Python 中最常用的数据结构，因为它们的特性适合许多不同的用例。由于这种特殊的对象类型是**可变的**(也就是说，它可以被就地修改)，所以添加或删除元素更加常见。

在今天的文章中，我们将探讨两个内置列表方法`append()`和`extend()`之间的区别，这两个方法可用于通过向列表对象添加更多元素来扩展列表对象。最后，我们还将讨论如何在列表中的特定索引处插入元素。

## Python 列表简单明了

列表是一个 Python 数据结构，它是一个由**有序的**和**可变的**项目集合。每个元素可以是任何类型——内置 Python 类型或自定义对象，而单个列表可以包含不同类型的项目。

```
lst = ['Hello World', 10, 5.0, True]
```

由于列表项可以是任何类型，这意味着它们也可以是列表对象(即嵌套列表)。例如，列表的列表将如下所示。

```
lst = [[1, 2, 3], [4, 5, 6]]
```

如前所述，列表是有序的，这意味着指定元素的顺序一直保持不变。这意味着只有当两个列表以相同的顺序包含相同的元素时，才认为它们是相等的。

```
>>> [10, 20] == [20, 10]
False
```

列表中的每一项都可以通过其索引**访问。**

```
>>> my_lst = ['foo', 'bar]
>>> my_lst[0]
'foo'
>>> my_lst[1]
```

如果您想了解更多关于有序集合(如列表)的索引和切片，请务必阅读下面的文章。

[](/mastering-indexing-and-slicing-in-python-443e23457125) [## 掌握 Python 中的索引和切片

### 深入研究有序集合的索引和切片

towardsdatascience.com](/mastering-indexing-and-slicing-in-python-443e23457125) 

因为 Python 列表是可变的，所以它们可以按需扩展或收缩。换句话说，您甚至可以在特定的索引中插入或删除元素。在接下来的几节中，我们将探讨两种内置的 list 方法，它们可以用来向现有的 list 对象中添加更多的元素。

## append()做什么

`list.append()`用于**将一个项目添加到列表**的末尾。

```
>>> my_lst = [1, 2, 3]
>>> my_lst.append(4)
>>> my_lst
[1, 2, 3, 4]
```

方法会将列表的长度增加 1。这意味着如果`append`的输入是一个元素列表，那么列表本身将被追加，而不是单个元素:

```
>>> my_lst = [1, 2, 3]
>>> my_lst.append([4, 5])
>>> my_lst
[1, 2, 3, [4, 5]]
```

## extend()是做什么的

另一方面，`extend()`方法接受一个 iterable，它的元素将被追加到列表中。例如，如果`extend()`的输入是一个列表，它将通过迭代输入列表的元素将第一个列表与另一个列表连接起来。这意味着结果列表的长度将随着传递给`extend()`的 iterable 的大小而增加。

```
>>> my_lst = [1, 2, 3]
>>> my_lst.extend([4, 5, 6])
>>> my_lst
[1, 2, 3, 4, 5, 6]
```

## 在特定索引中添加元素

`insert()`方法可用于在特定索引处插入列表项。

```
>>> my_lst = [1, 2, 3]
>>> my_lst.insert(1, 4) # Add integer 4 to index 1
>>> my_lst
[1, 4, 2, 3]
```

与`append()`方法一样，`insert()`只会增加一个列表的长度。

```
>>> my_lst = [1, 2, 3]
>>> my_lst.insert(1, [1, 2])
>>> my_lst
[1, [1, 2], 2, 3]
```

## 最后的想法

在今天的文章中，我们讨论了 Python 列表的主要特征，它们是**有序的**和**可变的**项目集合。我们讨论了如何通过`append()`和`extend()`向现有的列表对象中添加更多的元素。前者将给定元素添加到列表的末尾，方法是将列表的长度增加 1。后者将迭代给定的输入，并将每个单独的元素添加到列表中，这意味着。最后，我们探索了如何使用`insert()`方法在列表的特定索引处插入元素。

## 你可能也喜欢

[](/what-are-named-tuples-in-python-59dc7bd15680) [## Python 中的命名元组是什么

### Python 中一个被忽略的扩展数据类型

towardsdatascience.com](/what-are-named-tuples-in-python-59dc7bd15680) [](https://levelup.gitconnected.com/what-are-python-sets-59eb890ab522) [## 什么是 Python 集合

### Python 集合的简明介绍

levelup.gitconnected.com](https://levelup.gitconnected.com/what-are-python-sets-59eb890ab522)