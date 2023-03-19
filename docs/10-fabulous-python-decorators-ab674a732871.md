# 10 个神话般的 Python 装饰者

> 原文：<https://towardsdatascience.com/10-fabulous-python-decorators-ab674a732871?source=collection_archive---------1----------------------->

## Python 编程语言中一些我最喜欢的装饰器的概述。

![](img/016a56c77c7083c7f1f5a23b728af163.png)

(src =[https://pixabay.com/images/id-3359870/](https://pixabay.com/images/id-3359870/)

# 介绍

Python 编程语言的一个伟大之处在于，它将所有的特性打包成一个非常有用的小软件包。许多上述特性可以完全改变 Python 代码的功能，这使得该语言更加通用。此外，如果使用得当，这些功能中的一些可以缩短编写有效软件所需的时间。Python 特性很好地实现了这些目标的一个很好的例子是 Python 的 decorators。

装饰器是快速编程宏，可用于改变 Python 对象的行为。它们可以应用于类和函数，实际上可以做很多真正有趣的事情！装饰器可以用来缩短代码，加速代码，并完全改变代码在 Python 中的行为方式。不用说，这个肯定能派上用场！今天我想展示一些我认为值得一试的装饰者。有很多装饰者，但是我选择了一些我认为功能最酷的。

# №1: @lru_cache

这个列表中的第一个装饰器来自 functools 模块。这个模块包含在标准库中，非常容易使用。它还包含了比这个装饰器更酷的特性，但是这个装饰器肯定是我最喜欢的。这个装饰器可以用来加速使用缓存的函数和操作的连续运行。当然，在使用时应该注意交换和缓存，但是在一般用途的情况下，大多数时候这个装饰器是值得使用的。如果你想了解更多关于 Functools 的知识以及我喜欢它的原因，我实际上写了一整篇文章，你可以在这里阅读:

[](/functools-an-underrated-python-package-405bbef2dd46) [## FuncTools:一个被低估的 Python 包

### 使用 functools 将您的 Python 函数提升到一个新的水平！

towardsdatascience.com](/functools-an-underrated-python-package-405bbef2dd46) 

能够用一个简单的装饰器来加速代码是非常棒的。可以从这样的装饰器中获益的函数的一个很好的例子是递归函数，例如计算阶乘的函数:

```
def factorial(n):
    return n * factorial(n-1) if n else 1
```

递归在计算时间上相当困难，但是添加这个装饰器可以帮助显著地加速这个函数的连续运行。

```
[@lru_cache](http://twitter.com/lru_cache)
def factorial(n):
    return n * factorial(n-1) if n else 1
```

现在，每当我们运行这个函数时，前几个阶乘计算将被保存到缓存中。因此，下一次我们去调用函数时，我们只需要计算我们之前使用的阶乘之后的阶乘。当然，并不是所有的阶乘计算都会被保存，但是很容易理解为什么这是这个装饰器的一个很好的应用，可以加速一些本来就很慢的代码。

# №2: @jit

JIT 是实时编译的缩写。通常每当我们在 Python 中运行一些代码时，首先发生的是编译。这种编译会产生一些开销，因为类型会被分配内存并存储为未赋值但已命名的别名。通过即时编译，我们在执行时完成了大部分工作。从很多方面来说，我们可以认为这类似于并行计算，Python 解释器同时处理两件事，以节省时间。

Numba JIT 编译器因在 Python 中提供了这一概念而闻名。与@lru_cache 类似，这个装饰器可以很容易地被调用，从而立即提高代码的性能。Numba 包提供了 jit decorator，这使得运行更密集的软件变得容易得多，而不必使用 c。如果您想阅读关于这个包的更多内容，我还有另外一篇文章，您可以在这里查看:

[](/numba-jit-compilation-but-for-python-373fc2f848d6) [## Numba: JIT 编译，但是用于 Python

### 快速浏览一下 2020 年让 Python 变得更好的神奇工具。

towardsdatascience.com](/numba-jit-compilation-but-for-python-373fc2f848d6) 

```
from numba import jit
import random

@jit(nopython=True)
def monte_carlo_pi(nsamples):
    acc = 0
    for i in range(nsamples):
        x = random.random()
        y = random.random()
        if (x ** 2 + y ** 2) < 1.0:
            acc += 1
    return 4.0 * acc / nsamples
```

# №3: @do_twice

do_twice 装饰器完成了它名字中的大部分工作。这个装饰器可以用来在一次调用中运行一个函数两次。这当然有一些用途，我发现它对调试特别有帮助。它可以用来测量两个不同迭代的性能。以 Functools 为例，我们可以让一个函数运行两次，以检查是否有改进。这个函数是由 Python 中的 decorators 模块提供的，它在标准库中。

```
from decorators import do_twice
@do_twice
def timerfunc():
    %timeit factorial(15)
```

# №4: @count_calls

伴随 do_twice 装饰器的简单性的是 count_calls 装饰器。这个装饰器可以用来提供一个函数在软件中被使用了多少次的信息。像 do_twice 一样，这对于调试来说肯定会很方便。当添加到一个给定的函数中时，我们将收到一个输出，告诉我们该函数每次运行时已经运行了多少次。这个装饰器也在标准库中的装饰器模块中。

```
from decorators import count_calls
@count_calls
def function_example():
    print("Hello World!"function_example()
function_example()
function_example()
```

# №5:@数据类

为了节省编写类的时间，我一直在使用的最好的装饰器之一是 dataclass 装饰器。这个装饰器可以用来快速编写我们编写的类中常见的标准方法。如果您想了解更多关于这个装饰者的信息，我也有一篇关于它的文章，您可以在这里阅读:

[](/pythons-data-classes-are-underrated-cc6047671a30) [## Python 的数据类被低估了

towardsdatascience.com](/pythons-data-classes-are-underrated-cc6047671a30) 

这个装饰器来自 dataclass 模块。这个模块也在标准库中，所以不需要 PIP 来试用这个例子！

```
from dataclasses import dataclass[@dataclass](http://twitter.com/dataclass)
class Food:
    name: str
    unit_price: float
    stock: int = 0

    def stock_value(self) -> float:
        return(self.stock * self.unit_price)
```

这段代码将自动创建一个初始化函数 __init__()，其中包含填充我们的类中的数据所必需的位置参数。它们也会自动提供给 self，所以没有必要仅仅为了将一些数据参数放入一个类而编写一个很长的函数。

# №6:@单身

为了理解单例装饰器的目的，我们需要首先理解什么是单例。从某种意义上说，单例是全局类型的一个版本。这意味着这些类型被定义为只存在一次。尽管这些在 C++等语言中很常见，但在 Python 中却很少见。对于单例，我们创建一个只使用一次的类，并改变这个类，而不是一个初始化的构造类型。在这种情况下，类型不太像模板，而更像一个单独的受控对象。

通常情况下，单例装饰器是由用户制作的，实际上并不是导入的。这是因为 singleton 仍然是对我们的 singleton decorator 中提供的模板的引用。为了在我们的类中使用这个装饰器，我们可以命名一个单例函数并编写一个包装器:

```
def singleton(cls):
    instances = {}
    def wrapper(*args, **kwargs):
        if cls not in instances:
          instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return wrapper@singleton
class cls:
    def func(self):
```

解决这个问题的另一种方法是使用元类。如果你想了解更多关于元类的知识，我去年写了一篇文章，更详细地介绍了这些元类以及它们的用途，你可以在这里查看:

[](/pythonic-metaprogramming-with-metaclasses-19b0df1e1760) [## 具有元类的 Pythonic 元编程

### 你的装饰和元类生存指南。

towardsdatascience.com](/pythonic-metaprogramming-with-metaclasses-19b0df1e1760) 

```
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]

class Logger(object):
    __metaclass__ = Singleton
```

# №7: `@use_unit`

对于科学计算来说，一个可能经常派上用场的装饰器是 use_unit 装饰器。这个装饰器可以用来改变方法返回的输出。对于那些不想在数据中添加测量单位，但又想让人们知道这些单位是什么的人来说，这很有用。这个装饰器在任何模块中都不可用，但是非常常见，对于科学应用程序非常有用。

```
def use_unit(unit):
    """Have a function return a Quantity with given unit"""
    use_unit.ureg = pint.UnitRegistry()
    def decorator_use_unit(func):
        @functools.wraps(func)
        def wrapper_use_unit(*args, **kwargs):
            value = func(*args, **kwargs)
            return value * use_unit.ureg(unit)
        return wrapper_use_unit
    return decorator_use_unit

@use_unit("meters per second")
def average_speed(distance, duration):
    return distance / duration
```

# №7:**@ wrapps**

函数包装器是在 Python 中处理相对复杂的函数时使用的一种设计模式。包装器函数通常用于完成一些更低级的迭代任务。包装函数的好处是它可以用来显著提高性能。和 lru_cache 装饰器一样，这个装饰器是由 FuncTools 包提供的。

```
import functools as ft
```

包装装饰器本身只是一个方便的装饰器，用于更新给定函数的包装器。当这个装饰器被调用时，这个函数将在每次使用时更新它的包装器。这对于提高性能大有帮助，FuncTools 中的许多可用工具就是如此。

```
**def** my_decorator(f):
    **@wraps**(f)
    **def** wrapper(*args, **kwds):
        print('Calling decorated function')
        **return** f(*args, **kwds)
    **return** wrapper
```

包装器本身通常是装饰器，包装器装饰器调用通常用在装饰器调用中的包装器函数上。编写完代码后，我们可以用这个包装器来修饰我们的新函数:

```
@my_decorator
def func(x):
    print(x)
```

# №8:@静态方法

在某些情况下，可能会有这样的情况，人们可能希望能够访问在更广泛的意义上被私下定义的东西。有时，我们有一些函数包含在我们希望被系统化的类中，这正是 staticmethod decorator 的用途。

使用这个装饰器，我们可以制作 C++静态方法来处理我们的类。通常，在类的范围内编写的方法对于该类是私有的，除非作为子类调用，否则不可访问。但是，在某些情况下，您可能希望在方法与数据交互的方式上采用更具功能性的方法。使用这个装饰器，我们可以创建两个选项，而不需要创建多个函数。

```
**class** Example:
    **@staticmethod
**    **def** our_func(stuff):
        print(stuff)
```

我们也不需要显式地提供我们的类作为参数。staticmethod decorator 为我们处理这个问题。

# №9: @singledispatch

FuncTools 用非常有用的 singledispatch 装饰器再次出现在这个列表中。单一分派是一种在许多编程语言中很常见的编程技术，因为它是一种非常好的编程方式。虽然我倾向于选择多个派遣，但我认为单个派遣可以在许多方面用于扮演相同的角色。

这个装饰器使得在 Python 中处理类型更加容易。当我们处理多个类型，并且希望通过同一个方法传递时，情况就更是如此。我在我的 FuncTools 文章中写了更多关于这个的内容，所以如果你对使用单一分派方法感兴趣，我推荐你使用它(链接在#1 中)。)

```
[@singledispatch](http://twitter.com/singledispatch)
def fun(arg, verbose=False):
        if verbose:
            print("Let me just say,", end=" ")
        print(arg)[@fun](http://twitter.com/fun).register
def _(arg: int, verbose=False):
    if verbose:
        print("Strength in numbers, eh?", end=" ")
    print(arg)
[@fun](http://twitter.com/fun).register
def _(arg: list, verbose=False):
    if verbose:
        print("Enumerate this:")
    for i, elem in enumerate(arg):
        print(i, elem)
```

# №10:@注册

寄存器函数来自模块 atexit。给定该模块的名称和手法，您可能会想到这个装饰器可能与在终止时执行某些操作有关。寄存器装饰器命名一个在终止时运行的函数。例如，这将适用于一些需要在退出时保存的软件。

```
from atexit import register
@register
def termin():
    print(" Goodbye!")
```

# 结论

不用说，Python 的装饰器非常有用。它们不仅可以用来降低编写一些代码的时间，而且在加速代码方面也非常有帮助。当你发现装饰者时，他们不仅非常有用，而且编写自己的装饰者也是一个好主意。这些装饰器之所以强大，只是因为装饰器作为一个特性在 Python 中非常强大。感谢您阅读我的文章，我希望它能让您注意到 Python 的一些很酷的功能！