# 你应该知道的 6 个很酷的 Python 技巧

> 原文：<https://towardsdatascience.com/6-cool-python-tricks-you-should-know-bab1b6a42446?source=collection_archive---------10----------------------->

## 超越常规

![](img/05fd2e0f0f152cb9114ada7aa99a58fb.png)

由 [Unsplash](https://unsplash.com/s/photos/trick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的[网络甜甜圈](https://unsplash.com/@webdonut?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

近年来，数据科学经历了巨大的发展。Python 作为数据科学领域最常见的编程语言，也越来越受欢迎。

在本文中，我将提到 6 个我认为非常酷的 Python 技巧。它们还会通过提供完成某些任务的实用方法，让你的生活变得更轻松。

要成为一名优秀的程序员，仅仅编写代码来完成给定的任务是不够的。你的程序应该在时间和计算复杂度方面高效。此外，代码应该是干净的，易于阅读和调试，并可维护。

本文中的技巧将帮助您实现编写干净高效代码的最终目标。

## 1.部分

切片是对象，因此可以存储在变量中。一些数据结构允许索引和切片，例如列表、字符串和元组。

我们可以使用整数来指定切片的上下限，或者使用切片对象。

```
s = slice(3,6)lst = [1, 3, 'a', 'b', 5, 11, 16]
text = 'DataScience'
tpl = (1,2,3,4,5,6,7)print(lst[s])
['b', 5, 11]print(text[s])
aScprint(tpl[s])
(4, 5, 6)
```

切片 s 表示从第四元素到第六元素的切片。我们将相同的切片对象应用于列表、字符串和元组。

## 2.交换变量

我们很可能会遇到需要交换两个变量的值的情况。一种方法是使用中间变量来临时保存值。

```
a = 'John'
b = 'Emily'c = a
a = b
b = cprint(a)
'Emily'print(b)
'John'
```

这种方式有点乏味。Python 提供了一种更好的交换方式。

```
a = 20
b = 50a, b = b, aprint(a)
50print(b)
20
```

## 3.对列表进行排序

假设我们有一个列表列表。

```
lst = [[2, 7], [7, 3], [3, 8], [8, 7], [9, 7], [4, 9]]
```

我们可以通过使用带有 lambda 函数的 sort 函数，根据内部列表的第一项或第二项对列表进行排序。

```
lst.sort(key = lambda inner:inner[1])print(lst)
[[7, 3], [2, 7], [8, 7], [9, 7], [3, 8], [4, 9]]
```

列表根据第二项排序。我们可以对第一个项目做同样的事情，只需将 1 改为 0。

```
lst.sort(key = lambda inner:inner[0])print(lst)
[[2, 7], [3, 8], [4, 9], [7, 3], [8, 7], [9, 7]]
```

## 4.参数解包

假设我们有一个给定数字相乘的函数。

```
def mult(a, b, c):
   return a * b * cmult(2, 3, 4)
24
```

如果我们只需要将三个数相乘，这个函数就能很好地工作。必须给出正好三个数字。通过使用参数解包，我们可以使函数更加灵活。

```
def mult(*args):
   result = 1
   for i in args:
      result *= i
   return result
```

现在 mult 函数能够乘以任意数量的值。

```
mult(2, 3)
6mult(2, 3, 4)
24mult(2, 3, 4, 5)
120
```

参数解包在 Python 中非常常用。如果你阅读一个包或库的文档，你一定见过 [*args 和**kwargs](/10-examples-to-master-args-and-kwargs-in-python-6f1e8cc30749) 。

## 5.分解收藏

假设我们有一个返回两个值的元组的函数，我们想将每个值赋给一个单独的变量。一种方法是使用如下索引:

```
tpl = (1, 2)x = tpl[0]
y = tpl[1]print(x, y)
1 2
```

有一个更好的选项，允许我们在一行中执行相同的操作。

```
x, y = tplprint(x, y)
1 2
```

它可以扩展到具有两个以上值的元组或一些其他数据结构，如列表或集合。

```
x, y, z = {1, 2, 3}print(x, y, z)
1 2 3x, y, z = ['a', 'b', 'c']print(x, y, z)
a b c
```

## 6.f 弦

在字符串中添加变量是一种常见的做法。f 弦是迄今为止最酷的做法。为了更好地欣赏 f 字符串，让我们先用 format 函数执行操作。

```
name = 'John'
age = 15print("{} is {} years old".format(name, age))
John is 15 years old
```

我们通过在最后使用 format 函数来指定花括号内的变量。f 字符串允许在字符串中指定变量。

```
print(f"{name} is {age} years old")
John is 15 years old
```

f 字符串更容易理解和输入。此外，它们使代码更具可读性。

## 结论

我们已经介绍了 6 个简单却非常实用的技巧。它们引起微小的变化，但微小的改进会累积起来。你最终会写出更高效、更易于阅读和调试、更易于维护的代码。

感谢您的阅读。如果您有任何反馈，请告诉我。