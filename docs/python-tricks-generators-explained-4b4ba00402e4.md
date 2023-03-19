# Python 技巧:生成器解释

> 原文：<https://towardsdatascience.com/python-tricks-generators-explained-4b4ba00402e4?source=collection_archive---------29----------------------->

![](img/30d5b31f8c1e2050a810a8f54765d7ce.png)

照片由[大卫·卡尔波尼](https://unsplash.com/@davidcarboni?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## Python 中的生成器是如何工作的？

欢迎阅读一系列短文，每篇短文都有方便的 Python 技巧，可以帮助你成为更好的 Python 程序员。在这篇博客中，我们将研究发电机。

## 介绍

生成器是 iterable 的子类。

为了理解生成器是如何工作的，我们需要首先修改 iterables 是如何工作的。

## 可重复的

iterable 是可以迭代的对象。

当我们写 for 循环时，它以一个`for x in y:`语句开始。`for`语句正在迭代表达式`y`。因此，`y`是可迭代的。

一些非常常见的可重复项有:列表、元组、字符串等

```
for i in [0, 1, 2]:
    print(i)for i in (0, 1, 2):
    print(i)for i in "012":
    print(i)
```

Iterables 将值存储在内存中，并允许使用`__iter__`和`__next__`方法进行迭代。以上面的例子为例，在使用`__iter__`进行迭代之前，首先创建一个值为`"012"`的字符串并存储在内存中。因此，iterables 的一个主要优点是它们可以被访问和迭代任意多次。同时，作为将它存储在内存中的折衷，当你的 iterables 变长时，它将占用更多的内存。

## 发电机

生成器是只能迭代一次的 iterable 的子类。

本质上，生成器生成(产出)一个值，忘记它，并生成下一个值，直到满足结束条件。

在选择使用生成器还是迭代器时，有几个关键因素值得考虑:

1.  你需要内存中的全部结果吗？
2.  您需要按原样重用原始结果吗？
3.  你的结果是否足够小，可以放入内存？
4.  是否要在获得所有结果后处理结果？

如果以上都是肯定的，那么迭代器就足够了。否则，您可能想考虑使用一个生成器来从延迟执行和动态让步中获益。

您可以通过两种方式创建发生器:

```
**# 1\. list comprehension but with parenthesis**g = (i ** 2 for i in range(10))
type(g)# generator**# 2\. use yield**def create_generator(i):
    for i in range(i):
        yield i ** 2type(create_generator(10))
# generator
```

假设你有一个生成器，你想把值存储在一个列表中。由于生成器会动态地生成值，因此需要对其进行迭代，以获取用于创建列表的值。由于生成器只能迭代一次，因此之后生成器将为空。如果我们迭代并中途停止，情况也是如此:

```
g = (i for i in range(5))**# Cast the generator to a list for the first time**
l = list(g)
l# [0, 1, 2, 3, 4]**# Cast the generator to a list for the second time**
l = list(g)
l# []g = (i for i in range(5))**# Iterate the generator and stop halfway through**
for i in g:
    if i == 2:
        breaklist(g)# [3, 4]
```

## 收益陈述&发电机是如何工作的？

现在我们已经介绍了生成器的基本机制，让我们通过下面的例子来看看生成器实际上是如何工作的:

```
import numpy as np
import timedef create_generator(n):
    """ A generator that generate n arrays of 10^n random number
    between 0 and 1\. """ length = 10 ** n
    print(f"length: {length}") for _ in range(n):
        time.sleep(_)
        print(_)
        yield np.random.rand(length)
        print("After yielding") print("End of generator")
```

收益率报表是更现实的生成器的核心。

一个普通的函数在被调用时将被执行，并使用一个`return`语句返回结果(如果没有语句，则返回`None`)。

一个生成器函数**在被调用时不会被执行**，而是只会返回一个生成器对象。

好吧，但那是什么意思？

假设我们有一个使用生成器的 for 循环:

```
g = create_generator(3)
print(type(g))
print("Before For Loop")for i in g:
    print("Got something!")print("After For Loop")
```

我们将从片段中得到的是:

```
<class 'generator'>
Before For Loop
length: 1000
0
Got something!
After yielding
1
Got something!
After yielding
2
Got something!
After yielding
End of generator
After For Loop
```

发电机的工作方式是:

1.  代码在创建时不会被执行，而是返回一个生成器对象
2.  当`for`语句第一次使用生成器时，生成器代码将一直执行到`yield`语句
3.  发电机的任何后续使用将恢复进度并运行，直到它再次到达`yield`语句
4.  一旦生成器到达末尾，它将运行剩余的代码

这篇博文就讲到这里吧！我希望你已经发现这是有用的。如果你对其他 Python 技巧感兴趣，我为你整理了一份简短博客列表:

*   [Python 技巧:扁平化列表](/python-tricks-flattening-lists-75aeb1102337)
*   [Python 技巧:如何检查与熊猫的表格合并](/python-tricks-how-to-check-table-merging-with-pandas-cae6b9b1d540)
*   [Python 技巧:简化 If 语句&布尔求值](/python-tricks-simplifying-if-statements-boolean-evaluation-4e10cc7c1e71)
*   [Python 技巧:对照单个值检查多个变量](/python-tricks-check-multiple-variables-against-single-value-18a4d98d79f4)

如果你想了解更多关于 Python、数据科学或机器学习的知识，你可能想看看这些帖子:

*   [改进数据科学工作流程的 7 种简单方法](/7-easy-ways-for-improving-your-data-science-workflow-b2da81ea3b2)
*   [熊猫数据帧上的高效条件逻辑](/efficient-implementation-of-conditional-logic-on-pandas-dataframes-4afa61eb7fce)
*   [常见 Python 数据结构的内存效率](/memory-efficiency-of-common-python-data-structures-88f0f720421)
*   [与 Python 并行](/parallelism-with-python-part-1-196f0458ca14)
*   [为数据科学设置必要的 Jupyter 扩展](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72)
*   [Python 中高效的根搜索算法](/mastering-root-searching-algorithms-in-python-7120c335a2a8)

如果你想了解更多关于如何将机器学习应用于交易和投资的信息，这里有一些你可能感兴趣的帖子:

*   [Python 中交易策略优化的遗传算法](https://pub.towardsai.net/genetic-algorithm-for-trading-strategy-optimization-in-python-614eb660990d)
*   [遗传算法——停止过度拟合交易策略](https://medium.com/towards-artificial-intelligence/genetic-algorithm-stop-overfitting-trading-strategies-5df671d5cde1)
*   [人工神经网络选股推荐系统](https://pub.towardsai.net/ann-recommendation-system-for-stock-selection-c9751a3a0520)

<https://www.linkedin.com/in/louis-chan-b55b9287> 