# Python 中计时代码的一种简单方法

> 原文：<https://towardsdatascience.com/a-simple-way-to-time-code-in-python-a9a175eb0172?source=collection_archive---------16----------------------->

## 使用装饰器来计时你的函数

作者:[爱德华克鲁格](https://www.linkedin.com/in/edkrueger/)和[道格拉斯富兰克林](https://www.linkedin.com/in/dougaf/)。

![](img/18803cb651618004f2bc03ea29815f1d.png)

布拉德·尼瑟里在 Unsplash 上拍摄的照片

# 介绍

我们的目标是在 Python 中创建一种简单的函数计时方法。我们通过用 Python 的库`functools`和`time`编写装饰器来实现这一点。然后，这个装饰器将被应用到我们感兴趣的运行时的函数中。

# 计时装饰:@timefunc

下面的代码代表了一个通用的装饰模式，它具有可重用和灵活的结构。注意`functool.wraps`的位置。这是我们关闭的装饰。这个装饰器保存了传递给闭包的`func`的元数据。

timer.py

Functools 在第 16 行变得很重要，我们在打印语句中访问了`func.__name__`。如果我们不使用`functools.wraps`来修饰我们的闭包，将会返回错误的名称。

这个装饰器返回传递给`timefunc()`的函数的运行时。在第 13 行，`start` 开始计时。然后，第 14 行的`result` 存储`func(*args, **kwargs).`的值，之后计算`time_elapsed`。打印语句报告`func`的名称和执行时间。

## 使用@符号应用 timefunc

在 Python 中，decorators 可以很容易地用`@`符号来应用。并非所有装饰者的应用都使用这种语法，但是所有的`@`符号都是装饰者的应用。

我们用符号`@` 用`timefunc` 来修饰`single_thread` 。

用@timefunc 装饰函数

现在`single_thread`被修饰了，当它在第 13 行被调用时，我们将看到它的`func.__name__`和运行时。

![](img/e5666f6d44e2b145ac3c559205191dc8.png)

timefunc 修饰的 single_thread 的输出

如果你想知道这是如何工作的，下面我们将更深入地讨论为什么以及如何为时间函数编写装饰器。

## 为什么一个人可以计时一个函数

原因相对简单。更快的功能是更好的功能。

> 时间就是金钱，朋友。—加斯洛维

时序装饰器向我们展示了一个函数的运行时。我们可以将装饰器应用于一个函数的几个版本，对它们进行基准测试，并选择最快的一个。此外，在测试代码时，知道执行需要多长时间也很有用。提前五分钟运行？这是一个很好的起床、活动双腿和倒满咖啡的窗口！

为了用 Python 编写装饰函数，我们依赖于`functools` 和对作用域的认识。我们来回顾一下范围和装饰。

## 装饰、关闭和范围

修饰是 Python 中的一种设计模式，允许您修改函数的行为。装饰器是一个函数，它接受一个函数并返回一个修改过的函数。

当编写闭包和装饰器时，必须记住每个函数的作用域。在 Python 中，函数定义范围。闭包可以访问返回它们的函数的范围；装饰者的范围。

在将修饰函数传递给闭包时，保留它的元数据是很重要的。了解我们的作用域让我们可以用`functools.wraps`恰当地修饰我们的闭包。

*要了解这些概念的更多信息，请阅读这篇三分钟的文章。*

[](/decorators-and-closures-by-example-in-python-382758321164) [## Python 中的装饰器和闭包示例

### 如何使用装饰器增强函数的行为

towardsdatascience.com](/decorators-and-closures-by-example-in-python-382758321164) 

# 这个装饰器的可重用性

注意`func` 被当作第 7 行的一个参数。然后在第 11 行，我们传递`*args, **kwargs`，进入我们的闭包。这些`*args, **kwargs`用于计算第 10 行`func(*args, **kwargs)`的`result`。

timer.py

`*args`和`**kwargs`的灵活性使得`timefunc`可以处理几乎任何功能。我们闭包的 print 语句被设计用来访问函数`__name__`、`args`、`kwargs`和`result`，为`func`创建一个有用的定时输出。

# 结论

装饰是增强功能行为的有力工具。通过编写一个装饰器来为函数计时，您可以获得一个优雅的、可重用的模式来跟踪函数的运行时。

请随意将`timefunc`复制到您的代码库中，或者您可以尝试编写自己的时序装饰器！