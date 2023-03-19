# 10 个流行的递归编码面试问题

> 原文：<https://towardsdatascience.com/10-popular-coding-interview-questions-on-recursion-2ddd8aa86039?source=collection_archive---------2----------------------->

![](img/4c2962fccb18a195fafe50c030d497ab.png)

照片由[马克十分位数](https://unsplash.com/@markdecile?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 聪明工作

准备编码面试需要相当长的时间。有这么多不同的主题、数据结构和算法要讨论。递归是最重要的算法类型之一。因为它是许多重要算法的基础，如分治算法、图算法、动态编程、一些基于树的搜索和排序算法等等。这是不可避免的。因此，在参加编码面试之前进行一些练习是很重要的。

这篇文章将集中在递归的基本问题上，这些问题在基本编码访谈中非常普遍和流行。如果你在谷歌上搜索，你会发现这些问题到处都有。**我只是为你整理了一些常见的面试问题。**你会在这里看到几种不同模式的递归算法。

本文并不保证你只会看到这些问题。这些是一些常见的类型，这应该给你一些很好的实践！

> 我会从容易到难。这不是一个递归教程。我只是在这里提供一些练习材料。由于我大部分时间都在使用 python，所以这些解决方案将以 python 语言提供。

建议你先自己试着解决所有问题，再看解决方案。

> ***问题 1***

编写一个递归函数，该函数接受一个数字，并返回从零到该数字的所有数字的总和。

我将称这个函数为'**累计'。**如果我提供 10 作为输入，它应该返回从 0 到 10 的所有数字的总和。那是 55。

以下是 python 解决方案:

```
def cumulative(num):
    if num in [0, 1]:
        return num
    else:
        return num + cumulative(num-1)
```

> ***问题 2***

编写一个递归函数，将一个数字作为输入，并返回该数字的阶乘。

我相信我们都知道阶乘是什么。我将把这个函数命名为“阶乘”。

以下是 python 解决方案:

```
def factorial(n):
    assert n >=0 and int(n) == n, 'The number must be a positive integer only!'
    if n in [0,1]:
        return 1
    else:
        return n * factorial(n-1)
```

> ***问题 3***

编写一个递归函数，它接受一个数字“n ”,并返回斐波那契数列的第 n 个数字。

提醒一下，斐波那契数列是以 0 和 1 开始的正整数序列，其余的数字是前两个数的和:0，1，1，2，3，5，8，11…

以下是 python 解决方案:

```
def fibonacci(n):
    if n in [0, 1]:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```

> ***第四题***

编写一个递归函数，它将一组数字作为输入，并返回列表中所有数字的乘积。

如果您不是 python 用户，python 中的列表就像 Java、JavaScript 或 PHP 中的数组。

以下是 python 解决方案:

```
def productOfArray(arr):
    if len(arr) == 0:
        return 0
    if len(arr) == 1:
        return arr[0]
    else:
        return arr[len(arr)-1] * productOfArray(arr[:len(arr)-1])
```

> ***第五题***

写一个函数，接受一个字符串，如果这个字符串是回文就返回。

提醒一下，如果一个字符串等于它的倒数，就叫做回文。比如 Madam，civic，kayak。如果你颠倒这些单词中的任何一个，它们都保持不变。

以下是 python 中的递归解决方案:

```
def isPalindrom(strng):
    if len(strng) == 0:
        return True
    if strng[0] != strng[len(strng)-1]:
        return False
    return isPalindrome(strng[1:-1])
```

如果字符串是回文，这个函数返回 True，否则返回 false。

> ***第六题***

编写一个递归函数，它接受一个字符串，并反转该字符串。

如果输入是'惊人的'，它应该返回' gnizama '。

以下是 Python 解决方案:

```
def reverse(st):
    if len(st) in [0, 1]:
        return st
    else:
        return st[len(st)-1] + reverse(st[:len(st)-1])
```

> ***问题 7***

编写一个递归函数，该函数采用一个可能包含更多数组的数组，并返回一个所有值都已展平的数组。

假设这是输入数组:

```
[[1], [2, 3], [4], [3, [2, 4]]]
```

输出应该是:

```
[1, 2, 3, 4, 3, 2, 4]
```

以下是 python 解决方案:

```
def flatten(arr):
    res = []
    for i in arr:
        if type(i) is list:
            res.extend(flatten(i))
        else:
            res.append(i)
    return res
```

> ***问题 8***

编写一个递归函数，它接受一个单词数组，并返回一个包含所有大写单词的数组。

如果这是输入数组:

```
['foo', 'bar', 'world', 'hello']
```

输出数组应该是:

```
['FOO', 'BAR', 'WORLD', 'HELLO']
```

以下是 python 解决方案:

```
def capitalizeWords(arr):
    if len(arr) == 0:
        return []
    else:
        return [arr[0].upper()]+capitalizeWords(arr[1:])
```

> ***问题 9***

编写一个递归函数，它接受一个数组和一个回调函数，如果该数组的任何值从该回调函数返回 True，则返回 True，否则返回 False。

在这个解决方案中，我使用函数‘isEven’作为回调函数，如果一个数字是偶数，则返回 True，否则返回 False。

下面是回调函数:

```
def isEven(num):
    if num%2==0:
        return True
    else:
        return False
```

如果输入数组中有一个元素从‘isEven’函数返回 True，那么我们的主递归函数应该返回 True，否则返回 False。这是一个数组:

```
[1, 2, 3, 5]
```

这里递归函数应该返回 True，因为这个数组有一个偶数元素。

以下是 python 解决方案:

```
def anyEven(arr, cb):
    if len(arr) == 0:
        return False
    if cb(arr[0]):
        return True
    return anyEven(arr[1:], cb)
```

> ***第十题***

写一个递归函数，返回一个字典中所有正数的和，这个字典可能包含更多的嵌套字典。

这里有一个例子:

```
obj = {
  "a": 2,
  "b": {"x": 2, "y": {"foo": 3, "z": {"bar": 2}}},
  "c": {"p": {"h": 2, "r": 5}, "q": 'ball', "r": 5},
  "d": 1,
  "e": {"nn": {"lil": 2}, "mm": 'car'}
```

这应该会返回 10。因为这本字典包含五个 2，没有其他偶数。

以下是 python 解决方案:

```
def evenSum(obj, sum=0):
    for k in obj.values():
        if type(k) == int and k%2 ==0:
            sum += k
        elif isinstance(k, dict):
            sum += evenSum(k, sum=0)
    return sum
```

## 结论

要精通递归需要大量的练习。但是在去面试之前看一下这些问题是个好主意。没有人能保证面试问题，但准备不同模式的编码问题是关键。但是到目前为止，无论我面对什么样的面试，他们都没有给我任何难题。他们通常会问问题来测试知识和解决问题的整体方法。

欢迎在推特上关注我，喜欢我的脸书页面。

## 更多阅读

[](/the-detailed-guide-to-master-method-to-find-the-time-complexity-of-any-recursive-algorithm-b40c8250ed67) [## 掌握计算任何递归算法时间复杂度的方法的详细指南

### 做中学

towardsdatascience.com](/the-detailed-guide-to-master-method-to-find-the-time-complexity-of-any-recursive-algorithm-b40c8250ed67) [](/introduction-to-graph-algorithm-breadth-first-search-algorithm-in-python-8644b6d31880) [## 图算法简介:Python 中的广度优先搜索算法

### 清晰理解可视化广度优先搜索算法

towardsdatascience.com](/introduction-to-graph-algorithm-breadth-first-search-algorithm-in-python-8644b6d31880) [](/a-full-length-machine-learning-course-in-python-for-free-f2732954f35f) [## 免费的 Python 全长机器学习课程

### 吴恩达用 Python 写的机器学习教程

towardsdatascience.com](/a-full-length-machine-learning-course-in-python-for-free-f2732954f35f) [](/how-i-switched-to-data-science-f070d2b5954c) [## 我如何转向数据科学

### 我的旅程、错误和收获

towardsdatascience.com](/how-i-switched-to-data-science-f070d2b5954c) [](/an-ultimate-cheat-sheet-for-data-visualization-in-pandas-4010e1b16b5c) [## 熊猫数据可视化的终极备忘单

### 熊猫的所有基本视觉类型和一些非常高级的视觉…

towardsdatascience.com](/an-ultimate-cheat-sheet-for-data-visualization-in-pandas-4010e1b16b5c) [](/an-ultimate-guide-to-time-series-analysis-in-pandas-76a0433621f3) [## 熊猫时间序列分析终极指南

### 在 Pandas 中执行时间序列分析所需的所有 Pandas 功能。您也可以将此用作备忘单。

towardsdatascience.com](/an-ultimate-guide-to-time-series-analysis-in-pandas-76a0433621f3) [](/all-the-datasets-you-need-to-practice-data-science-skills-and-make-a-great-portfolio-857a348883b5) [## 练习数据科学技能和制作优秀投资组合所需的所有数据集

### 一些有趣的数据集提升你的技能和投资组合

towardsdatascience.com](/all-the-datasets-you-need-to-practice-data-science-skills-and-make-a-great-portfolio-857a348883b5)