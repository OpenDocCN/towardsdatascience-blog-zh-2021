# 单子解释道

> 原文：<https://towardsdatascience.com/monads-from-the-lens-of-imperative-programmer-af1ab8c8790c?source=collection_archive---------4----------------------->

![](img/ea194fd755d8f74f769aba1ff589e4c2.png)

[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

所以，我费了很大的劲才明白“单子”到底是什么！到处都有人用公式或长篇大论来解释它，以至于人们几乎不可能理解它。在这里，我将用简单的语言解释复杂的逻辑。另外，如果你有兴趣看这个主题的视频，请看看下面的视频。

此外，为了便于理解，我将用 Python 编码，这样 Haskell 的语法就不会把你吓跑。

在开始之前，让我首先向您介绍 Haskell 的主要特性，因为它首先需要单子——函数式编程。函数式编程是一种思维模式，在这种模式下，所有的设计都是根据纯函数来考虑的。

*   职能是一等公民。所以，所有简单的逻辑都是一个函数，所有复杂的逻辑都是通过函数间的运算来处理的。
*   函数必须是纯的。这意味着无论你给什么输入，输出都应该保持不变。与纯函数交互的唯一方式是只通过输入和输出。它不能访问全局状态，也不能打印任何东西，甚至不能抛出异常，除非在函数定义中进行了定义。

Monad 是一个通用的概念，它有助于在纯函数之间进行操作，以处理副作用。就是这样。让我用一个例子来解释一下，一会儿就明白了。

# 问题陈述

对一个数做多次平方运算。

```
def square(num: int) -> int:
    return num * num;print(square(square(2)));**--------------------------------------------------------------------
Output**
16
```

# 引入副作用

现在，让我们通过打印输入的当前值来给函数添加一个副作用。

```
def sqaure_with_print(num: int) -> int:
    print("Currrent num: ", num);
    return num * num;print(sqaure_with_print(sqaure_with_print(2)));**--------------------------------------------------------------------
Output** Currrent num:  2
Currrent num:  4
16
```

# 有副作用的纯函数

由于 print 语句的引入，上述函数不再是一个纯函数。现在该怎么办？如何处理纯函数中的副作用？记住，与纯函数交互的唯一方式是通过输入和输出。我们需要在输出本身中获取日志。

```
def sqaure_with_print_return(num: int) -> (int, str):
    logs = "Currrent num " + str(num);
    return (num * num, logs);print(sqaure_with_print_return(sqaure_with_print_return(2)));**--------------------------------------------------------------------
Output** Traceback (most recent call last):
File "hello.py", line 21, in <module>
print(sqaure_with_print_return(sqaure_with_print_return(2)));
File "hello.py", line 17, in sqaure_with_print_return
return (num * num, logs);
TypeError: can't multiply sequence by non-int of type 'tuple'
```

# 自定义排版

哎呀！虽然我们能够把它变成一个纯粹的函数，但是为了把函数链接起来，同时输出副作用，破坏了程序。但是为什么会这样呢？该函数需要`int`，而我们通过了`(int, str)`。看起来只是期望不匹配。看起来需要做一些特殊处理。

我们可能需要修改`square_with_print_return,`，以便它可以接受`(int, str)`而不是`int`。我们是否应该改变一个函数的输入签名，以便它们可以组合？这是可扩展的吗？

相反，让我们添加一个定制的 compose 函数，它知道如何处理上述参数不匹配的情况。

```
def sqaure_with_print_return(num: int) -> (int, str):
    logs = "Currrent num " + str(num);
    return (num * num, logs);def compose(func2, func1, num: int):
    res1 = func1(num)
    res2 = func2(res1[0])
    return (res2[0], res1[1] + res2[1]);print(compose(sqaure_with_print_return, sqaure_with_print_return, 2));**--------------------------------------------------------------------
Output** (16, 'Currrent num 2 Currrent num 4')
```

耶！成功了！现在，假设我们想要链接 3 个函数，必须重写这个组合函数，对吗？我们需要找到一种可伸缩的方式来编写这个函数，以便它可以处理任意数量的函数。

# 包装纯函数

更好的解决方案是使用其他包装函数，这有助于以所需的格式处理输入和输出参数。这个函数将知道如何在不改变纯函数参数的情况下处理副作用。

```
from typing import Tupledef sqaure_with_print_return(num: int) -> (int, str):
    logs = "Currrent num " + str(num);
    return (num * num, logs);def bind(func, tuple: Tuple[int, str]):
   res = func(tuple[0])
   return (res[0], tuple[1] + res[1])def unit(number: int):
   return (number, "");print(bind(sqaure_with_print_return, (bind(sqaure_with_print_return, unit(2)))))**--------------------------------------------------------------------
Output** (16, 'Currrent num 2 Currrent num 4')
```

*   **绑定函数:**这只是`square_with_print_return` 上的一个包装函数，它接受一个元组，而不仅仅是 int。所以，所有这样的函数都可以用绑定函数来包装。因此，现在您的纯函数可以处理任何类型的输入参数
*   **单元函数:**但是我们的第一个参数是一个整数。有人应该把它转换成一个元组。这就是单位函数的作用。

这是我的朋友，叫莫纳德！

最后，我们可以说，单子只是处理纯函数中副作用的一种漂亮而通用的方式，并提供了一种通过使用绑定和单元概念来组合纯函数的可伸缩方法。