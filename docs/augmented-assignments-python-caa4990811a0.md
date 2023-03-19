# Python 中的扩充赋值

> 原文：<https://towardsdatascience.com/augmented-assignments-python-caa4990811a0?source=collection_archive---------25----------------------->

## 理解增强赋值表达式在 Python 中是如何工作的，以及为什么在可变对象中使用它们时要小心

![](img/cc762922bb700073f3ed2be19318a850.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/add?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

Python 从 C 语言中借用的一个概念是**扩充赋值**，本质上是标准赋值的简称。

在今天的简短指南中，我们将讨论什么是扩充赋值，以及为什么在对可变数据类型使用它们时要格外小心。

## Python 中的扩充赋值

扩充赋值创建了一种速记法，将二进制表达式合并到实际的赋值中。例如，以下两个语句是等效的:

```
a = a + b
a +=  b  # augmented assignment
```

在下面的代码片段中，我们展示了 Python 语言中允许的所有扩充赋值

```
a += b
a -= b
a *= b
a /= b
a //= ba &= b
a |= b
a ^= ba >>= b
a <<= b
a %= b
a **= b
```

扩充赋值适用于任何支持隐含二进制表达式的数据类型。

```
>>> a = 1
>>> a = a + 2
>>> a
3
>>> a += 1
>>> a
4>>> s = 'Hello'
>>> s = s + ' '
>>> s 
'Hello '
>>> s += 'World'
>>> s
'Hello World'
```

## 扩充赋值如何处理可变对象

当使用扩充赋值表达式时，将自动选取最佳操作。这意味着对于支持就地修改的特定对象类型，将应用就地操作，因为它比先创建副本再进行分配要快。如果你想添加一个条目到一个列表中，通常使用`append()`方法比使用连接更快:

```
>>> my_lst = [1, 2, 3]
>>> my_lst.append(4)  # This is faster 
>>> my_lst = my_lst + [4]  # This is slower
```

由于就地操作速度更快，它们将被自动选择用于扩充任务。为了弄清楚这些表达式是如何处理可变对象类型的，让我们考虑下面的例子。

如果我们使用串联，那么将会创建一个新的列表对象。这意味着，如果另一个名称共享同一引用，它将不受新对象列表中任何未来更新的影响:

```
>>> lst_1 = [1, 2, 3]
>>> lst_2 = lst_1
>>> lst_1 = lst_1 + [4, 5]
>>> lst_1
[1, 2, 3, 4, 5]
>>> lst_2
[1, 2, 3]
```

现在，如果我们使用增强赋值表达式，这意味着表达式将使用`extend()`方法来执行更新，而不是连接，那么我们也将影响所有其他拥有共享引用的名称:

```
>>> lst_1 = [1, 2, 3]
>>> lst_2 = lst_1
>>> lst2 += [4, 5]
>>> lst_1
[1, 2, 3, 4, 5]
>>> lst_2
[1, 2, 3, 4, 5]
```

## 扩充赋值的三个优点

总而言之，扩充赋值表达式提供了以下三个优点:

1.  它们需要更少的击键，而且更干净
2.  它们速度更快，因为表达式的左边只需要计算一次。在大多数情况下，这可能不是一个巨大的优势，但当左侧是一个复杂的表达式时，它肯定是。

```
x += y  # x is evaluated only once
x = x + y  # x is evaluated twice
```

3.将选择最佳操作，这意味着如果对象支持就地更改，则就地操作将应用于串联。但是，您应该非常小心，因为有时这可能不是您想要的，因为在应用就地操作区域时，名称共享引用可能会相互影响。

## 最后的想法

在今天的文章中，我们讨论了 Python 中的增强赋值表达式，以及它们如何帮助您编写更 Python 化、更清晰甚至更快的代码。

此外，我们强调了这种类型的表达式如何应用于可变数据类型，这可能会给我们带来共享引用的麻烦。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership>  

**你可能也会喜欢**

<https://codecrunch.org/what-does-if-name-main-do-e357dd61be1a>  </dynamic-typing-in-python-307f7c22b24e> 