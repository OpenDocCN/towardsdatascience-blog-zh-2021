# Python 中的 Args 和 Kwargs 函数调用变得简单

> 原文：<https://towardsdatascience.com/args-and-kwargs-in-python-function-calling-made-easy-acfe736f988a?source=collection_archive---------37----------------------->

## 理解如何用多个值完整地调用函数。

![](img/dba606f89afae010249066212a1dc6da.png)

[莫兰](https://unsplash.com/@ymoran?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在我第一次阅读 fastai 的书时，我在函数调用中遇到了关键字 ****args*** 和 *****kwargs*** 。自然地，我非常好奇，想了解我如此热爱的编程语言的另一个方面，所以我开始研究。事实证明，这两者结合起来确实使 Python 3 中一些复杂的函数调用看起来像是小孩子的游戏！

自从我研究了这两个操作符(是的，在我们所说的技术语言中，它们被称为 ***解包操作符*** )之后，我就一直在我的日常项目实验中使用它们，我发现它们非常酷。所以现在，我写这篇文章是为了让你了解这些很酷的概念，以防你错过！

我们走吧！

## 什么是解包运算符？

单星号和双星号解包操作符是在 Python 2 中引入的。但是在 Python 3.6 发布之后，它们在使用中变得更加强大。

简而言之，解包操作符是在 Python 中 ***解包来自 iterable 对象的值的操作符。单星运算符`*`可以用在语言中的任何一种 iterable 上，而双星运算符`**`只能用在 ***字典*** 上的各种运算上。***

让我们来看一个例子。我们将定义一些辅助变量，我们将在本教程中使用它们来探索这两个操作符的功能。

```
nums = [1, 2, 3]
alphs = ['a', 'b', 'c']nums_d = {1: 'one', 2: 'two', 3: 'three'}
alphs_d = {'a': 'First', 'b': 'Second', 'c' : 'Third'}
```

现在，让我们通过观察他们的行动来进一步了解他们吧！

## 我们如何使用解包操作符？

首先，让我们在我们的列表 ***nums*** 上进行实验。

下面是列表的解压缩版本:

```
print(*nums) OUTPUT:
1 2 3
```

我们再做一个！我们将打开下一个字符/字符串列表，我们有 ***alphs。***

```
print(*alphs)OUTPUT:
a b c
```

注意到相似之处了吗？你一定知道这个操作员现在对这些列表做了什么，对吗？它把可迭代列表变成了单独的元素，这些元素可以被一些函数单独处理，我们可以把这些值传递给这些函数！

现在让我们来看看这个用例。当我们看到关于使用函数来操作这些由操作符解包的值的例子时，应该就清楚了。

## 在 Python 函数中使用解包运算符

我们可以使用这个简单的调用来解包一个字符串:

```
ex = 'Args and Kwargs'print(*ex)OUTPUT:
A r g s   a n d   K w a r g s# each individual character can be processed separately
```

我们也可以用元素列表来做。让我们定义一个简单的将三个数相加的函数。

```
def sum_of_nums(n1, n2, n3):
    print(n1 + n2 + n3)
```

现在让我们继续将我们的数字列表*num*传递给这个函数，但是附加了解包操作符。

```
sum_of_nums(*nums) OUTPUT:
6
```

看到魔法了吗？！这太酷了。我们也可以用之前定义的字典做同样的事情。

让我们首先定义一个函数来进行字符串连接，并将结果打印给用户。 ***args 代表自变量。**

```
def concat_str(*args):
    res = ''
    for s in args:
        res += s
    print(res)
```

现在，让我们用字典 ***alphs 附带的解包操作符调用它。***

```
concat_str(*alphs)OUTPUT:
abc
```

这看起来与上面的输出非常相似。

结论是什么？*解包操作符允许我们在需要时使用和操作任何 iterable 中的单个元素。这样，我们可以传递一个复杂的列表或字典列表，操作符可以让我们在自定义函数中使用列表的元素和字典的键/值。*

这里，看看操作符对字符串列表做了什么。

```
ex = 'Args and Kwargs'
print([*ex]) OUTPUT:
['A', 'r', 'g', 's', ' ', 'a', 'n', 'd', ' ', 'K', 'w', 'a', 'r', 'g', 's']
```

它返回一个 ***iterable*** (一个新的列表)，其中的元素是被解包的单个字符串！

## 与**kwargs 一起前进一步

****kwargs 代表关键字参数。**

Kwargs 是我们在字典中使用的一个解包操作符。让我们定义一个新函数，将两个或更多字典连接在一起。

```
def concat_str_2(**kwargs):
    result = ''
    for arg in kwargs:
        result += arg
    return result
```

现在，我们可以用任意数量的字典调用这个函数。

```
print(concat_str_2(**alphs_d)) OUTPUT:
abc
```

哎呀！字典中的键在这里被连接起来。但是我们可能想要连接键值，对吗？

我们可以通过对值进行迭代来实现这一点，就像我们对任何普通字典所做的那样。

```
# concatenating the values of the kwargs dictionary
def concat_str_3(**kwargs):
    result = ''
    for arg in kwargs.values():
        result += arg
    return result print(concat_str_3(**alphs_d))
```

你能猜到输出会是什么吗？

```
OUTPUT:
FirstSecondThird
```

还记得我们之前的两本字典吗？通过使用 **** 操作符，我们可以很容易地将它们组合起来。

```
alphanum = {**nums_d, **alphs_d}
print(alphanum)OUTPUT:
{1: 'one', 2: 'two', 3: 'three', 'a': 'First', 'b': 'Second', 'c': 'Third'}
```

最后，我们还可以直接将字典连接在一起。

```
concat_str_3(a = 'Merge', b = ' this ', c = "dictionary's values")
```

这里，我们将键值对传递给我们的函数。在你看下面之前，试着想想我们的输出会是什么。

```
OUTPUT:
"Merge this dictionary's values"
```

就是这样！现在你知道 Python 中这两个令人敬畏的操作符的一切了！

## 最后一件事

我们在函数定义中定义函数参数的顺序，记住这一点很重要。我们不能打乱顺序，否则我们的 Python 解释器会抛出一个错误！

这是函数定义中参数排列的正确顺序:

```
# correct order of arguments in function
def my_cool_function(a, b, *args, **kwargs):
    '''my cool function body'''
```

*顺序如下:标准变量参数，*args 参数，然后是**kwargs 参数。*

与本教程相关的完整 jupyter 笔记本可以在本报告中找到！[https://github.com/yashprakash13/Python-Cool-Concepts](https://github.com/yashprakash13/Python-Cool-Concepts)

[](https://github.com/yashprakash13/Python-Cool-Concepts) [## yashprakash 13/Python-Cool-Concepts

### 在 GitHub 上创建一个帐户，为 yashprakash 13/Python-Cool-Concepts 开发做贡献。

github.com](https://github.com/yashprakash13/Python-Cool-Concepts) 

如果你想了解更多关于我在 fastai 上写的[系列文章，其中我记录了我与这个库的旅程，请访问这篇文章](/a-fast-introduction-to-fastai-my-experience-b18d4457f6a5?source=your_stories_page-------------------------------------)。:)

要不要学会用不到 20 行代码做一个深度学习模型，快速部署成 REST API？[在这里获得我的免费指南！](https://tremendous-founder-3862.ck.page/cd8e419b9c)

在 [Twitter](https://twitter.com/csandyash) 和 [LinkedIn](https://www.linkedin.com/in/yashprakash13/) 上与我联系。