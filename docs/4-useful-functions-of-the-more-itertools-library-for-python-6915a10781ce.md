# Python 的 More-Itertools 库的 4 个有用函数

> 原文：<https://towardsdatascience.com/4-useful-functions-of-the-more-itertools-library-for-python-6915a10781ce?source=collection_archive---------20----------------------->

## 使得使用 Python iterables 更加容易

![](img/88bbfae8b9ccd1386d21031c479bb806.png)

照片由 [Chor 曾](https://unsplash.com/@chortsang?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/levels?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Python 的 itertools 模块提供了几个函数，允许创建迭代器来执行高效的循环操作。

[more-itertools](https://more-itertools.readthedocs.io/en/stable/) 是一个 Python 库，为 iterables 带来了额外的功能。

我们将浏览 more-itertools 库的 5 个非常有用的函数，并查看演示如何使用它们的示例。

先简单解释一下 Python 中的可迭代器和迭代器。

*   iterable 是一个 Python 对象，它可以一次返回一个元素，比如列表和元组。iterable 对象有一个返回迭代器的`__iter__`方法。
*   迭代器是一个拥有`__next__`方法的对象。我们可以把迭代器想象成一个表示数据流的对象。每次调用 __next__ 方法时，迭代器都返回行中的下一个元素。因此，迭代器有一个在迭代过程中记住当前位置的状态。

这里有一个简单的例子来说明 iterable 和 iterator 之间的区别。

```
mylist = [3, 1, 4, 21, 13, 5]print(mylist)
[3, 1, 4, 21, 13, 5]
```

变量“mylist”是一个列表，所以它是可迭代的。

```
myiterator = iter(mylist)print(myiterator)
<list_iterator object at 0x000002C9E5860070>
```

当我们在“mylist”上应用`iter`函数时，我们得到一个迭代器对象。我们不能像打印可迭代的那样打印迭代器。但是我们可以得到下一个项目。

```
next(myiterator)
3next(myiterator)
1
```

我们还可以使用如下的循环来打印每个项目:

```
for i in myiterator:
    print(i)# output
4
21
13
5
```

当我们到达终点时它就停止了。如果我们现在调用下一个函数，我们会得到一个`StopIteration`错误。一旦我们到达一个迭代器的末尾，就是这样！我们不能从头开始。因此，从某种意义上说，迭代器是可任意使用的。

我们现在已经理解了什么时候可迭代和迭代器。让我们开始探索 more-itertools 库的伟大特性。

它可以很容易地通过 pip 安装。然后，我们只需要导入它。

```
# install from cmd
pip install more-itertools# install in jupyter notebook
pip install more-itertools
```

# 1.块状的

我们有一个需要分割成固定长度的小块的列表。有多种方法可以完成这个操作，但是使用`chunked`功能非常简单。

```
from more_itertools import chunkedmylist = [1,2,3,4,5,6,7,8,9,10]chunked_list = list(chunked(mylist, 3))chunked_list
[[1, 2, 3], [4, 5, 6], [7, 8, 9], [10]]
```

`chunked`函数返回一个迭代器，使用`list`函数可以很容易地将它转换成一个列表。它接受要分块的列表和每个块的大小。

在我们的例子中，块大小是 3，所以最后一个块只剩下一个项目。

我们可以通过使用列表索引来访问块:

```
chunked_list[0]
[1, 2, 3]
```

# 2.变平

`flatten`函数接受嵌套的 iterable 并将它们转换成平面 iterable。例如，我们可以通过使用列表的列表来创建列表。

```
nested_list = [[1,4], [3,8], [1,10]]list(flatten(nested_list))
[1, 4, 3, 8, 1, 10]
```

类似地，我们可以使用嵌套元组列表和列表。

```
nested = [(5,2), (9,2), [1,5]]list(flatten(nested))
[5, 2, 9, 2, 1, 5]
```

# 3.最小最大值

`minmax`函数是一种快速找到 iterable 中的最小值和最大值的方法。

```
from more_itertools import minmaxmylist = [3, 1, 4, 21, 13, 5]minmax(mylist)
(1, 21)
```

它返回一个具有最小值和最大值的元组。

它还可以处理字符串，并根据字母顺序查找最小值和最大值。

```
mylist = ["A", "C", "Z", "M"]minmax(mylist)
('A', 'Z')
```

# 4.每个都是唯一的

假设我们有一组可重复项。`unique_to_each`函数给出了每个可迭代变量的唯一值。换句话说，这个函数返回的值只存在于相关的 iterable 中。

当我们做一个例子时，它会变得更加清楚。

```
from more_itertools import unique_to_eachA = ["John", "Jane"]
B = ["John", "Max"]
C = ["Jane", "Emily"]unique_to_each(A, B, C)
[[], ['Max'], ['Emily']]
```

列表 A 中的值是同样存在于列表 C 中的“John”和同样存在于列表 C 中的“Jane”。因此，列表 A 没有唯一的值。正如我们在输出中看到的,`unique_to_each`函数为列表 A 返回一个空列表。

列表 B 中的“Max”在任何其他列表中都不存在，因此它对于列表 B 是唯一的，我们也可以在输出中看到。

我们也可以在字符串上使用这个函数。请注意，字符串也是可迭代的。

```
unique_to_each("John","Jane")
[['o', 'h'], ['a', 'e']]
```

我们已经在 more-itertools 库中介绍了 4 个有用的函数。我强烈建议浏览一下这个库的[文档](https://more-itertools.readthedocs.io/en/stable/index.html)，因为它有更多的功能。

文档组织得很好，因此您可以毫无问题地使用它。

你可以成为一名[媒介会员](https://sonery.medium.com/membership)来解锁我的作品的全部权限，以及媒介的其余部分。如果您使用以下链接，我将收取您的一部分会员费，无需您支付额外费用。

<https://sonery.medium.com/membership>  

感谢您的阅读。如果您有任何反馈，请告诉我。