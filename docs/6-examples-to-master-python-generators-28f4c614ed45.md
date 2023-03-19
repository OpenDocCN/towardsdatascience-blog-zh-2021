# 掌握 Python 生成器的 6 个例子

> 原文：<https://towardsdatascience.com/6-examples-to-master-python-generators-28f4c614ed45?source=collection_archive---------10----------------------->

## 综合实践指南

![](img/aa1cbb77b5570f1800ee8ff0c69a3edf.png)

贾斯汀·坎贝尔在 [Unsplash](https://unsplash.com/s/photos/sequence?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Python 中的生成器是我们经常使用但很少谈论的工具之一。例如，大多数 For 循环都伴随着 range 函数，它是一个生成器。

生成器允许随着时间的推移生成一系列值。使用生成器的主要优点是我们不必一次创建整个序列并分配内存。相反，生成器一次返回一个值，并一直等到调用下一个值。

在本文中，我们将通过 6 个例子来演示如何在 Python 中使用生成器，以及一些需要记住的技巧。

## 示例 1

让我们从一个简单但经常使用的生成器开始。range 函数用于迭代由给定的开始、停止和步长确定的一系列值。

```
range(5)
out> range(0, 5)for i in range(5):
   print(i)
0
1
2
3
4
```

如果执行 range 函数，将不会返回任何值。但是，我们可以在 for 循环中对其进行迭代，以便按顺序访问这些值。为了提供序列中的数字流，range 函数跟踪最后一个数字和步长。

我们还可以通过将生成器转换为列表来查看整个序列。

```
list(range(5))
[0, 1, 2, 3, 4]
```

## 示例 2

我们可以创建自己的生成器函数。语法与创建普通函数非常相似，但有一个关键的区别。

下面是一个生成器函数，它返回从 1 开始的给定范围内所有其他数字的立方。

```
def mygenerator(n):
   for i in range(1, n, 2):
      yield i**3
```

生成器和普通函数的主要区别在于我们不使用 return 关键字。而是使用 yield 关键字。

我们现在可以在 for 循环中使用我们的生成器:

```
for i in mygenerator(10):
   print(i)
1 
27 
125 
343 
729
```

## 示例 3

创建生成器的另一种方法是使用生成器表达式。它类似于列表理解。

```
mygenerator = (i**3 for i in range(1,10,2))mygenerator
<generator object <genexpr> at 0x7f867be7b9d0>
```

我们现在可以迭代它。

```
for i in mygenerator:
   print(i)
1 
27 
125 
343 
729
```

需要注意的是，一旦我们迭代了一个生成器并到达了末尾，我们就不能再迭代了。例如，如果我们重新运行上面的 For 循环，它将不会返回任何内容。

在示例 2 中情况并非如此，因为我们有一个生成器函数，它在每次执行时都会创建一个生成器。您可能会注意到，在这个示例中我们没有函数调用。相反，我们有一个生成器对象。

## 实例 4

我们可以使用 next 函数手动迭代一个生成器。让我们首先创建一个新的生成器对象。

```
def mygenerator(n):
   for i in range(1, n, 2):
      yield i * (i + 1)my_gen = mygenerator(6)
```

我们现在可以使用 next 函数来请求生成器将返回的下一个值。

```
next(my_gen)
2next(my_gen)
12next(my_gen)
30next(my_gen)
> StopIteration error
```

如果我们在到达终点后调用生成器上的下一个函数，它将返回 StopIteration 错误。for 循环的步骤与我们处理下一个函数的步骤相同。然而，当使用 for 循环时，我们不会得到 StopIteration 错误，因为它会自动捕捉这个错误。

## 实例 5

我们经常看到术语 iterable 和 iterator。iterable 是我们可以迭代的对象，比如列表、集合、字符串等等。然而，我们不能将可迭代对象作为迭代器。

有一个内置的 Python 函数可以将 iterable 转换为 iterator。令人惊讶的是，它是 iter 函数。例如，我们可以迭代字符串，但不能将它们用作迭代器。iter 函数允许使用字符串作为迭代器。

```
my_string = "Python"next(my_string)
> TypeError: 'str' object is not an iterator
```

让我们将其转换为迭代器，然后再次调用下一个函数。

```
my_string_new = iter(my_string)next(my_string_new)
'P'next(my_string_new)
'y'
```

## 实例 6

iter 函数也可以用来将列表转换成迭代器。

```
my_list = ['a', 'b', 'c', 'd', 'e']my_gen = iter(my_list)type(my_gen)
list_iterator
```

让我们在 for 循环中使用它。

```
for i in my_gen:
   print(i)
a
b
c
d
e
```

## 结论

当处理分配大量内存的序列时，发生器非常有用。生成器支持迭代协议，所以当我们调用它们时，它们不会返回值并退出。

序列中的下一个值生成后，它们会自动挂起，并在请求下一个值时的最后一点恢复其执行状态。

优点是生成器一次计算一个值，并等待下一个值被调用，而不是一次计算整个序列。

感谢您的阅读。如果您有任何反馈，请告诉我。