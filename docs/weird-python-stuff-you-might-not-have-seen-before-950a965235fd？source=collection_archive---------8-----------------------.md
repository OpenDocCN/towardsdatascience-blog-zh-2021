# 你以前可能没见过的奇怪的 Python 东西

> 原文：<https://towardsdatascience.com/weird-python-stuff-you-might-not-have-seen-before-950a965235fd?source=collection_archive---------8----------------------->

## Python 的一些鲜为人知的最奇怪的秘密。

![](img/418b969d9ac3e07c8f582ff0462969d9.png)

(src =[https://unsplash.com/photos/vb-3qEe3rg8](https://unsplash.com/photos/vb-3qEe3rg8)

# 介绍

Python 编程语言是一种充满了惊人的多范例特性的编程语言。这很棒，因为它给了 Python 做许多不同任务的灵活性。例如，如果 Python 是一种纯面向对象的编程语言，不支持泛型编程概念，那么它在数据科学领域可能不会那么流行。这是因为数据科学家通常需要一种高级声明式编程语言，在这种语言中，可以在全局范围内定义事物。想象一下为 Jupyter-Notebook 编写一个主函数。那就奇怪了。

自然地，有了所有这些关于范式和语法的有趣探索，Python 的世界实际上可以是相当广阔的，通常有些东西很容易被忽略，因为在这种语言中有许多其他的东西要看。我花了两篇文章，每篇有二十个模块，介绍了标准库中我认为对某些事情有用的模块！也就是说，有很多东西需要用 Python 来解包。不仅在它所吹嘘的生态系统中，而且在它的标准库和基础内部。尽管读者可能已经知道了所有提到的 Python 特性，但毫无疑问，他们很可能不了解 Python 编程语言的每个方面。也许这种语言的一些奇怪的怪癖和特性会在你的下一个项目中派上用场！

# 指定返回类型

## [PEP3107](http://www.python.org/dev/peps/pep-3107/)

如果你是一个 C 程序员，很可能你熟悉返回类型。对于所有的非 C 程序员(或其他具有这种特性的语言，Java，C++，等等)。)返回类型就是函数结束时将要返回的数据类型。此外，如果你对 C 感兴趣，但在阅读之后不知道从哪里开始，我确实有一篇文章提供了对 C 的简单介绍，对新程序员来说非常友好。如果你想要这本书，你可以在这里查阅:

[](/an-understandable-introduction-to-c-e2cc12a52053) [## 浅显易懂的 C 语言介绍

### 对 C 语法结构的有效观察，以及对 C 编程的介绍

towardsdatascience.com](/an-understandable-introduction-to-c-e2cc12a52053) 

有些人可能会惊讶地发现，在 Python 中也可以这样做。我实际上是在一艘直到最近才知道这件事的人的船上。Python 首先处理类型和函数的方式相当奇怪…来自像 Julia 这样具有健壮的、强类型系统的语言，尽管 Python 是相对强类型的并且几乎没有隐式类型，但我仍然觉得奇怪的是指定参数类型基本上没有任何作用:

```
# This is weird.def add5(x : int): return(x + 5)print(add5(5))10print(add5("Hi"))TypeError: can only concatenate str (not "int") to str
```

对我来说很奇怪的是，即使我们没有提供正确的类型，这个函数还是试图执行。同样奇怪的事情也适用于获取返回类型。为了指定返回类型，我们使用带有`->.`的函数注释

```
def add5(n : int) -> int: return "Hi " + str(n)
```

您可能会注意到，尽管该函数的指定返回类型是整数，但该函数返回的是字符串。在像 C 这样的语言中，这是行不通的。在 C 语言中，一个有趣的事情是，它会从一个指针生成一个整数。换句话说，我们实际上正在经历 C 语言中的隐式类型。下面是演示这一点的代码示例:

```
#include <stdio.h>int main(){printf("Hello World");return "Hello";}
```

编译这段代码会产生以下结果:

```
main.c:15:12: warning: returning ‘char *’ from a function with return type ‘int’ makes integer from pointer without a cast [-Wint-conversion]15 |     return "Hello";|            ^~~~~~~Hello World...Program finished with exit code 0Press ENTER to exit console.
```

然而，Python 示例的问题在于，类型没有被隐式更改。因此，我们几乎可以从返回类型为 integer 的函数中返回一个字符串:

```
type(add5(5))str
```

这很奇怪——因为指定返回类型到底对我们的 Python 代码有什么影响？从这个角度来看，客观地说，当涉及到类型系统时，它什么也不做。我决定在这方面做一些研究，以便找出每当我们指定一个返回类型时到底会发生什么。

为了更好地理解这个特性，我在 Python.org 上读了一些 PEP 对这个特性的解释。如果你想读我读的东西，你可以在这里:

[](https://www.python.org/dev/peps/pep-3107/#return-values) [## PEP 3107 -功能注释

### 这个 PEP 引入了一个向 Python 函数添加任意元数据注释的语法[1]。因为 Python 的 2.x…

www.python.org](https://www.python.org/dev/peps/pep-3107/#return-values) 

然而，基本上所有这些反复出现在我脑海中的是，这是一个完全没有意义的功能。这看起来很奇怪，这样的东西被编程到 Python 中，却没有一个函数类型的系统真正使用它。我唯一的想法是，这可能实际上是有用的，也许它可以帮助编译。在我读到之前，这是有可能的

> “……也就是说，现在参数列表后面可以跟一个文字->和一个 Python 表达式。与参数的注释一样，当执行函数定义时，将计算该表达式。

看起来这个特性在很大程度上并不那么有用，也许它可以用来为文档字符串生成一些元数据，也就是说，通过返回类型，它将使文档更容易自动生成。不管这个功能实际上是为谁或为了什么，它肯定是一个怪异的功能。

# (functools)缓存

谈到 Python，我最喜欢的一件事就是装饰者。在我看来，装饰器对 Python 来说是一个非常有价值的特性，它给这种语言的编程带来了巨大的好处。老实说，我对业界远离 Python 感到兴奋，因为这种语言有太多的缺点，我真的觉得类型系统一点也不健壮。也就是说，我在许多其他编程语言中没有看到的一个可取之处是 decorators。

装饰器是 Python 中最简单、最容易使用的特性之一，它可以对您生成的代码产生巨大的影响。它们可以用来加速代码，改变代码的工作方式，甚至操纵对象在 Python 中存储数据的方式。也就是说，我的下一个奇怪的 Python 特性来自于标准库的 functools 模块中的缓存功能。此外，如果您想了解更多关于这个库的信息，我有一整篇关于它的文章，非常激进:

[](/functools-an-underrated-python-package-405bbef2dd46) [## FuncTools:一个被低估的 Python 包

### 使用 functools 将您的 Python 函数提升到一个新的水平！

towardsdatascience.com](/functools-an-underrated-python-package-405bbef2dd46) 

为了利用缓存，我们将像这样导入 lru_cache:

```
from functools import lru_cache
```

这个装饰器让一个函数缓存一定数量的计算。递归对于计算机来说是一个很大的问题，有很多情况下可以避免使用递归函数来解决问题。然而，一种常见的递归实现是阶乘函数。

```
def factorial(n):
    return n * factorial(n-1) if n else 1
```

让我们用数字 15 来试试我们的新阶乘函数。我还会计时，以便我们能掌握计算机做这样的操作需要多长时间。

```
%timeit factorial(15)1.94 µs ± 68.2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
```

现在让我们尝试一个更大数字的阶乘，我们看到这仍然需要很长时间:

```
%timeit factorial(20)1.94 µs ± 68.2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
```

现在我们将使用新的定义 fact()编写相同的函数。唯一不同的是，这次我们将使用 lru_cache 来修饰我们的函数。

```
@lru_cachedef fact(n): return n * factorial(n-1) if n else 1
```

现在让我们重复 15 的阶乘计算:

```
%timeit fact(15)63.8 ns ± 1.83 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

我们看到，与之前没有缓存的尝试相比，这种计算基本上没有区别。真正的诀窍是当我们现在提高到 20 的阶乘时:

```
%timeit fact(20)64.1 ns ± 1.51 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

我们看到这个值的计算有了显著的改进。为什么会这样呢？lru_cache 函数将把一些计算结果保存到缓存中。这使得随后的调用已经知道函数调用的答案，并简单地从其先前的内存中返回那些值。这是非常有价值的，这个函数是一个突出的应用程序的很好的例子。在某些情况下，这将使二项式分布计算减半。

# (数据类)数据类

我列出的怪异 Python 的下一个例子是 dataclass。这是另一个我认为非常有价值的装饰。虽然 decorator 的工作非常简单，但是它可以通过消除对初始化函数和典型 Python 类的其他组件的需求，非常有效地节省大量代码。如果你想阅读更多关于这个特定主题的内容，我有一整篇关于数据类的文章，以及为什么我认为它们很酷，你可以在这里阅读:

[](/pythons-data-classes-are-underrated-cc6047671a30) [## Python 的数据类被低估了

towardsdatascience.com](/pythons-data-classes-are-underrated-cc6047671a30) 

dataclass 的全部意义在于在 Python 中有效地使之更类似于 C 语言中的经典结构类型。鉴于 Python 面向对象的特性，很容易理解为什么它有时会妨碍创建仅仅是数据结构的类型，而不是整个类。使用数据类，只需简单地调用一个装饰器，就可以有效地保护自己免受 Python 中特定类型的面向对象部分的影响。考虑下面的例子:

```
[@dataclass](http://twitter.com/dataclass)
class Food:
    name: str
    unit_price: float
    stock: int = 0

    def stock_value(self) -> float:
        return(self.stock * self.unit_price)
```

注意，在这个实例中没有 __init__()函数。通常，如果我们编写这个类并提供我们的值、名称、单价和股票，我们会遇到一个错误。这是因为该类实际上接受 0 个参数，而不是我们试图提供的 3 个参数。不用说，如果我们只是想做一些更像结构的东西，这可能会有问题。如果在初始化过程中除了分配成员变量之外没有其他事情要做，这也可以节省很多代码行。这是我刚刚谈到的例子的一个用法，我们将会看到这个类没有参数:

```
class Other: name: str unit_price: float stock: int = 0spaghetti = Other("Spaghetti", 8.75, 28)TypeError: Other() takes no arguments
```

但是在之前的食物类的例子中，

```
lasagna = Food("Lasagna", 10.26, 45)
```

# 就地字符串串联

字符串有时很难精确地表达和输入不同的值。如果我想在 Python 中把一个值放在一个字符串的中间，我该怎么做呢？有些人可能会采取拆分的方法，或者尝试在字符串中的特定短语后插入——这些可能是不错的解决方案。然而，有一个更好的方法可以解决这个问题。假设我有以下两个数据部分，我的姓名和年龄:

```
name = "Emmett"age = 22
```

我想打印一个字符串，在一个完整的字符串中告诉我的名字和年龄。作为一个例子，我只允许再初始化一个字符串类型。我该怎么做？在字符串定义中的第一个引号前添加一个`f`将允许我们使用括号{}输入代码中标识符的值，如下所示:

```
print(f"Hello, my name is {name} and I am { age }")Hello, my name is Emmett and I am 22
```

还有另一种方法可以做到这一点，就像这样:

```
print("I'm a {Ds} Named {nm}".format(Ds = "Data Scientist", nm="Bob"))I'm a Data Scientist Named Bob
```

当然，在很多情况下，这样的东西会被证明是有用的。我想到的一个例子当然是标签处理。我正好有一篇关于 Python 中标签处理的文章，如果你想了解更多关于这个主题的内容，你可以在这里阅读这篇文章:

[](/essential-python-string-processing-techniques-aa5be43a4f1f) [## Python 字符串处理的基本技术

### 在 Python 中处理字符串类型数据的完整过程

towardsdatascience.com](/essential-python-string-processing-techniques-aa5be43a4f1f) 

# 结论

Python 确实有许多奇怪的地方，其中一些比其他的更有用。这些只是其中的一些，还有很多。我希望在你的软件之旅中，这些奇怪的小轨迹能派上用场！非常感谢您阅读我的文章。如果这篇文章中有一个部分对我来说很突出，那可能是两个装饰者，因为根据我的经验，我发现他们非常有用。我有一篇文章列出了一堆我认为有用的不同装饰者，我强烈推荐它们！你可以在这里读到:

[](/10-fabulous-python-decorators-ab674a732871) [## 10 个神话般的 Python 装饰者

towardsdatascience.com](/10-fabulous-python-decorators-ab674a732871) 

非常感谢您阅读我的文章。祝你好运，利用这些奇怪的附加到他们的最大潜力！