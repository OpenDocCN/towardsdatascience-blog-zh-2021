# 使用 Itertools 的超快速 Python

> 原文：<https://towardsdatascience.com/wicked-fast-python-with-itertools-55c77443f84c?source=collection_archive---------2----------------------->

## 快速浏览一种通过使用 itertools 模块使 Python 更快、更有效地进行机器学习的简单方法。

![](img/72fc911b461345b31f4d1f9266c3ca9d.png)

[https://unsplash.com/photos/eH_ftJYhaTY](https://unsplash.com/photos/eH_ftJYhaTY)

# 介绍

最近，我写了一篇文章，详细介绍了一些我认为在 Python 编程语言中非常有用的标准库工具。那篇文章的反响非常好，所以我决定再写一篇文章，列出标准库中我最喜欢的一些模块。如果你有兴趣阅读这两篇文章，你可以在这里查阅:

</10-surprisingly-useful-base-python-functions-822d86972a23>  </15-more-surprisingly-useful-python-base-modules-6ff1ee89b018>  

然而，每当我写这些文章时，我都会遇到一个小问题。对于标准库中的许多模块，在模块内部有一个相对较小的方法或类型集合是很常见的。然而，偶尔会有一些更有用的 Python 模块，它们更加广泛，具有如此多的函数和类，以至于在那些文章中不可能涉及到它们。考虑到这一点，我决定从一个名为 functools 的模块开始这项任务。我写了一篇关于它有多有用的文章，你可以在这里查看:

</functools-an-underrated-python-package-405bbef2dd46>  

这些文章中提到的另一个模块是一个叫做 itertools 的小模块。Itertools 是一个工具箱，用于在比我们通常在 Python 中使用的更高效、更古老的算法上构建定制迭代。不用说，在 Python 这样的语言中，当处理大量数据时，几乎总是需要考虑性能，像这样提供更有效迭代的工具非常有价值。

一般来说，编程速度变慢的最常见原因是过多的循环。迭代循环，特别是在单线程应用程序中，会导致很多严重的速度下降，这肯定会在 Python 这样的编程语言中引起很多问题。幸运的是，标准库模块 itertools 为我们处理迭代问题的典型方法提供了一些替代方法。不用说，这对于减少语言性能的障碍非常有价值。

> [笔记本](https://github.com/emmettgb/Emmetts-DS-NoteBooks/blob/master/Python3/faster%20it%20with%20iterttools.ipynb)

# 值得注意的迭代器

与原始文章列表中的大多数模块一样，在这个模块中，肯定有一些函数比其他函数有用得多。记住整个模块或包的内容可能会令人不知所措和困难。然而，有一种方法可以使你的学习适应一种更好的方法，在这种方法中，为了更快地学习，更有用的功能被优先考虑。记住这一点，下面是 itertools 使用的一些最重要的函数，以及它们的作用的简要描述。

```
import itertools as its
```

## 计数()

count 函数是一个迭代器，它将返回按特定步骤递增的值。这类似于迭代一个范围生成器，但是，有一些关键的区别。count()方法允许我们将步长数量作为关键字参数。此外，count()方法也将一直持续计数到无穷大。考虑到这一点，包含一个打破这个循环的条款可能是一个好主意，否则，它将永远运行下去。

```
for i in its.count(start = 0, step = 1):
    if i < 20:
        print(i)
    else:
        break
```

与基本迭代的解决方案相比，这种迭代方法也具有明显更好的性能。

## 周期()

cycle()方法可用于移动到 iterable 参数的下一次迭代。使用该函数的一个值得注意的部分是，它将创建并保存提供给它的每个迭代的副本。与 count 类似，这个迭代器也将无限返回，并在下次返回之前返回给定数组中的所有 dim。

```
array = [5, 10, 15, 20]
for z in its.cycle(array):
    print(z)
```

## 重复()

repeat()迭代器将一次又一次地不断返回一个提供的对象。这在某些情况下很有用，并且会无限返回，这是目前为止所有这些函数的共同点。

```
for z in its.repeat(array):
 print(z)
```

## 链条()

chain()迭代器从第一个 iterable 开始返回元素，直到用完为止，然后继续到下一个 iterable，直到用完所有 iterable。基本上，这将允许我们将多个 iterable 合并成一个长 iterable 来循环。当然，为了利用这个函数，我们还需要提供另一个 iterable:

```
array2 = [7, 8, 9, 10]
```

我们将每个 iterable 作为一个单独的参数提供，而不是一个列表:

```
for z in its.chain(array, array2):
    print(z)
```

## 压缩()

compress()迭代器过滤 dim 中的元素，只返回那些在*选择器*中有对应元素的元素，该元素的值为 True。

```
for z in its.compress('ABCDEF', [1,0,1,0,1,1]):
    print(z)
```

这个迭代器主要用于剔除不为真的参数。在上面的例子中，位数组中有两个零，这意味着映射到被迭代的字符。这两个零在 B 字符和 D 字符上，所以这两个字母在例子中不会被重复。

## 伊斯利斯()

我想介绍的最后一个迭代器是 islice()迭代器。这个迭代器返回数组位置的切片。例如，如果我们在位置 2 分割迭代，那么在迭代器到达该位置的值后，它将停止迭代。

```
for i in its.islice(array2, 2):
    print(i)
```

# FizzBuzz 的一个例子

FizzBuzz 游戏是编码面试和一般编程实践中常用的迭代问题的一个经典例子。通常，每当这被写入 Python 时，都是使用条件来编写的，就像这样:

```
for i in range(1,101):
    fizz = 'Fizz' if i%3==0 else ''
    buzz = 'Buzz' if i%5==0 else ''
    print(f'{fizz}{buzz}' or i)
```

虽然这是一个非常好的方法，但是重要的是要记住，使用 itertools 迭代器意味着在其他地方使用迭代器的 Pythonic 实现。也就是说，itertools 的迭代器通常比标准 Python for 循环的常规迭代快得多。这肯定是要记住的，因为当你能够比其他人更快地编写一个函数来解决这样的问题时，它可能会给招聘经理留下深刻印象。

为了促进这种方法，我们需要做的第一件事是使用循环迭代器找出将被我们的两个数整除的 dim:

```
fizzes = its.cycle([""] * 2 + ["Fizz"])
buzzes = its.cycle([""] * 4 + ["Buzz"])
```

每当我们决定遍历 count()迭代器时，这将找到所有能被 4 或 2 整除的整数。我们还将为 fizz 和 buzz 一起打印的时候设置一个新的阵列。

```
fizzes_buzzes = (fizz + buzz for fizz, buzz in zip(fizzes, buzzes))
```

最后但同样重要的是，我们将使用另一个带有 its.count()迭代器的 zip()循环来获得结果。看一看:

```
result = (word or n for word, n in zip(fizzes_buzzes, its.count(1)))
```

最后，我们将对结果进行切片，以便只获得迭代器返回的 100 个值。

```
for i in its.islice(result, 100):
        print(i)
```

现在我们的整个 FizzBuzz 函数看起来像这样:

```
def fizz_buzz(n):
    fizzes = its.cycle([""] * 2 + ["Fizz"])
    buzzes = its.cycle([""] * 4 + ["Buzz"])
    fizzes_buzzes = (fizz + buzz for fizz, buzz in zip(fizzes, buzzes))
    result = (word or n for word, n in zip(fizzes_buzzes, its.count(1)))
    for i in its.islice(result, 100):
        print(i)
```

# 结论

使用 itertools 的 Pythonic 代码通常更快、更简洁。有了这两个观察，很容易想象为什么这么多人相信这个模块。不用说，itertools 肯定是一个充满迭代器的非常有价值的模块。显然，其中一些迭代器可能比其他迭代器更有价值。希望这个对工具的小偷偷摸摸和详细描述能启发你试着捡起它并更熟悉这门语言。不管你在这方面的情绪如何，我认为 itertools 仍然是该语言中最有用的模块之一。感谢您的阅读，以及您的支持！