# 为什么 Python 中的装饰者是纯粹的天才

> 原文：<https://towardsdatascience.com/why-decorators-in-python-are-pure-genius-1e812949a81e?source=collection_archive---------3----------------------->

## [*小窍门*](https://towardsdatascience.com/tagged/tips-and-tricks)

## 用一个@符号来分析、测试和重用你的代码

![](img/abddf7ff6b8c62f08ea2cab0f7b2910e.png)

软件中没有什么是神奇的。但是装修工就很接近了！作者图片

![If](img/879311ef88ea597d740be6a54ae99925.png) 如果说有什么东西让 Python 取得了难以置信的成功，那就是它的可读性。其他一切都取决于此:如果代码不可读，就很难维护。它对初学者也不友好——一个被不可读的代码困扰的新手不会有一天尝试自己写代码。

在装饰者出现之前，Python 已经是可读的和初学者友好的了。但是随着这种语言开始用于越来越多的事情，Python 开发人员感到需要越来越多的特性，而不要弄乱环境和使代码不可读。

装饰者是一个完美实现的特性的黄金时代的例子。确实需要一段时间来理解，但这是值得的。当你开始使用它们时，你会注意到它们并没有使事情变得过于复杂，而是使你的代码变得整洁而时髦。

[](/software-developers-might-be-obsolete-by-2030-cb5ddbfec291) [## 到 2030 年，软件开发人员可能会被淘汰

### 为什么你不会丢掉工作

towardsdatascience.com](/software-developers-might-be-obsolete-by-2030-cb5ddbfec291) 

# 首先是高阶函数

简而言之，decorators 是一种处理高阶函数的好方法。所以我们先来看看那些！

## 函数返回函数

假设你有一个函数，`greet()`——不管你传递给它什么对象，它都会打招呼。假设您有另一个函数，`simon()`——它在适当的地方插入“Simon”。怎么才能把两者结合起来呢？看下面之前想一分钟。

```
def greet(name):
    return f"Hello, {name}!"def simon(func):
    return func("Simon")simon(greet)
```

输出是`'Hello, Simon!'`。希望这对你有意义！

当然，我们本可以直接调用`greet("Simon")`。然而，重点是我们可能想要将“Simon”放入许多不同的函数中。如果我们不使用“西蒙”，而是使用更复杂的东西，我们可以通过将它打包成一个类似`simon()`的函数来节省大量代码。

## 其他函数中的函数

我们也可以在其他函数中定义函数。这很重要，因为装修工也会这么做！没有装饰者，它看起来像这样:

```
def respect(maybe):
    def congrats():
        return "Congrats, bro!"
    def insult():
        return "You're silly!" if maybe == "yes":
        return congrats
    else:
        return insult
```

函数`respect()`返回一个函数；`respect("yes")`返回祝贺函数，`respect("brother")`(或其他参数代替`"brother"`)返回侮辱函数。要调用这些功能，输入`respect("yes")()`和`respect("brother")()`，就像普通功能一样。

明白了吗？那你就可以做装修工了！

![](img/f98b02c42562bb5fc109d7c5c52420a1.png)

代码非常乏味。图片作者。

# Python 装饰者的 ABC

## 带有@符号的函数

让我们试试前面两个概念的组合:一个函数接受另一个函数并定义一个函数。听起来难以置信？考虑一下这个:

```
def startstop(func):
    def wrapper():
        print("Starting...")
        func()
        print("Finished!")
    return wrapperdef roll():
    print("Rolling on the floor laughing XD")roll = startstop(roll)
```

最后一行确保我们不再需要调用`startstop(roll)()`；`roll()`就够了。你知道那个调用的输出是什么吗？不确定的话自己试试！

现在，作为一个很好的选择，我们可以在定义`startstop()`之后插入这个:

```
@startstop
def roll():
    print("Rolling on the floor laughing XD")
```

这做同样的事情，但是在开始时将`roll()`粘合到`startstop()`。

[](/how-to-use-python-classes-effectively-10b42db8d7bd) [## 如何有效地使用 Python 类

### 当一堆功能对你来说是更好的选择时

towardsdatascience.com](/how-to-use-python-classes-effectively-10b42db8d7bd) 

## 增加灵活性

为什么这很有用？这不是和以前一样消耗了很多行代码吗？

在这种情况下，是的。但是一旦你处理稍微复杂一点的东西，它就会变得非常有用。这一次，您可以将所有装饰器(即上面的`def startstop()`部分)移动到它自己的模块中。也就是说，您将它们写入一个名为`decorators.py`的文件，并将类似这样的内容写入您的主文件:

```
from decorators import startstop@startstop
def roll():
    print("Rolling on the floor laughing XD")
```

原则上，你可以不使用 decorators 来做这件事。但是这种方式使生活变得更容易，因为你不必再处理嵌套函数和无休止的括号计数。

你也可以嵌套装饰者:

```
from decorators import startstop, exectime@exectime
@startstop
def roll():
    print("Rolling on the floor laughing XD")
```

注意，我们还没有定义`exectime()`，但是您将在下一节中看到它。这是一个函数，可以测量 Python 中一个过程需要多长时间。

这种嵌套相当于这样一行:

```
roll = exectime(startstop(roll))
```

括号计数开始！想象一下，你有五六个这样的函数嵌套在一起。装饰符号不是比这种嵌套的混乱更容易阅读吗？

你甚至可以在[接受参数](https://realpython.com/primer-on-python-decorators/#decorating-functions-with-arguments)的函数上使用装饰器。现在，想象一下上面一行中的几个参数，你的混乱将会结束。装修工把它弄得干净整洁。

最后，你甚至可以给你的装饰器添加参数[——就像`@mydecorator(argument)`一样。是的，没有装修工你也可以做这些。但是我希望当你在三周内再次阅读你的无装饰代码时，你能从中获得很多乐趣…](https://realpython.com/primer-on-python-decorators/#decorators-with-arguments)

![](img/13a5aa8ab2f1fc41cd989b1b151974b5.png)

装修工让一切变得更简单。图片作者。

# 应用:装饰者切奶油的地方

既然我已经满怀希望地让你相信了装修工让你的生活变得简单了三倍，那么让我们来看看一些装修工基本上不可或缺的经典例子。

## 测量执行时间

假设我们有一个名为`waste time()`的函数，我们想知道它需要多长时间。好吧，就用装修工吧！

```
import timedef measuretime(func):
    def wrapper():
        starttime = time.perf_counter()
        func()
        endtime = time.perf_counter()
        print(f"Time needed: {endtime - starttime} seconds")
    return wrapper@measuretime
def wastetime():
    sum([i**2 for i in range(1000000)])wastetime()
```

十几行代码就完成了！另外，你可以在任意多的功能上使用`measuretime()`。

[](/the-flawless-pipes-of-python-pandas-30f3ee4dffc2) [## 蟒蛇/熊猫的完美管道

### 在本文中，您将学习如何在 Python 和 Pandas 中使用管道来使您的代码更高效，更好地…

towardsdatascience.com](/the-flawless-pipes-of-python-pandas-30f3ee4dffc2) 

## 降低代码速度

有时你不想立即执行代码，而是等待一段时间。这就是慢下来的装饰派上用场的地方:

```
import timedef sleep(func):
    def wrapper():
        time.sleep(300)
        return func()
    return wrapper@sleep
def wakeup():
    print("Get up! Your break is over.")wakeup()
```

调用`wakeup()`可以让你休息 5 分钟，之后你的控制台会提醒你继续工作。

## 测试和调试

假设你有很多不同的函数，你在不同的阶段调用它们，你失去了对什么时候调用什么的了解。每个函数定义都有一个简单的装饰器，这样可以更加清晰。像这样:

```
def debug(func):
    def wrapper():
        print(f"Calling {func.__name__}")
    return wrapper@debug
def scare():
    print("Boo!")scare()
```

这里有一个更加详细的例子。注意，为了理解这个例子，你必须检查[如何用参数来修饰函数。尽管如此，它还是值得一读！](https://realpython.com/primer-on-python-decorators/#decorating-functions-with-arguments)

## 重用代码

这不言而喻。如果你已经定义了一个函数`decorator()`，你可以在你的代码中到处散布`@decorator`。老实说，我认为没有比这更简单的了！

## 处理登录

如果你有一些只有在用户登录时才能访问的功能，那么对于 decorators 来说这也是相当容易的。我会让你参考[的完整例子](https://realpython.com/primer-on-python-decorators/#is-the-user-logged-in)作为参考，但是原理很简单:首先你定义一个类似`login_required()`的函数。在任何需要登录的函数定义之前，弹出`@login_required`。很简单，我会说。

[](/object-oriented-programming-is-dead-wait-really-db1f1f05cc44) [## 面向对象编程已经死了。等等，真的吗？

### 函数式编程的鼓吹者们，你们把枪口对准了错误的敌人

towardsdatascience.com](/object-oriented-programming-is-dead-wait-really-db1f1f05cc44) 

# 句法糖——或者为什么 Python 如此可爱

这并不是说我对 Python 不[持批评态度，或者在适当的时候不使用](/why-python-is-not-the-programming-language-of-the-future-30ddc5339b66)[替代语言](/bye-bye-python-hello-julia-9230bff0df62)。但是 Python 有一个很大的吸引力:它非常容易消化，即使你不是受过训练的计算机科学家，只是想让东西工作。

如果 C++是一个橙子，那么 Python 就是一个菠萝:同样有营养，但甜三倍。装饰者只是其中的一个因素。

但我希望你已经明白为什么它是如此大的甜蜜因素。句法糖给你的生活增添一些乐趣！没有健康风险，除了眼睛盯着屏幕。

祝你有很多甜蜜的代码！