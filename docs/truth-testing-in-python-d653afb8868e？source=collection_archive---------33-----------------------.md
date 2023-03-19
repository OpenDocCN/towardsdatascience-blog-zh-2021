# Python 中的真值测试

> 原文：<https://towardsdatascience.com/truth-testing-in-python-d653afb8868e?source=collection_archive---------33----------------------->

## 了解 Python 中 bool 的所有功能

![](img/1116ab69f06608d1a4f61eddb0820635.png)

([https://unsplash.com/photos/N4-deIU3kQI](https://unsplash.com/photos/N4-deIU3kQI)

# 介绍

毫无疑问，布尔是计算世界中最重要的类型。布尔值是真/假值，可用于确定计算机内部的硬件以及计算机运行所依赖的软件和内核等多种情况。这种真值和假值的概念从计算机的最底层一直到最高层，也就是编程的最高层:

> 脚本语言。

世界上最流行的编程语言 Python 恰好是一种脚本语言。虽然 Python 确实有一种非常传统的将布尔值作为变量(而不是标志)的方式，但实际上这种语言和不同类型的真或假本质有很多不同之处。bool 数据类型不仅如此，它还是 Python 编程语言中几乎所有对象的寄存器，因为所有类型都有一个 bool 属性，该属性在语言中默认设置为 true。

# 条件式

我们可以用来测试布尔值的方法叫做条件语句。条件语句仅用于根据条件的结果执行某些代码。我们可以认为 1 == 1 会返回 true。这是因为操作数相等，并且==运算符根据两者是否相同来提供布尔返回。

```
1 == 1
True
```

现在我们可以用 if 关键字把它放到一个条件语句中。这个语句中包含的代码当然会运行，因为 1 实际上等于 1。

```
if 1 == 1:
    print(" Who woulda thought?!")
```

如果不满足条件，我们可以将此与 else 结合使用，以获得不同的结果:

```
if 1 == 2:
    print(" Wait... It does?!")
else:
    print(" Those are certainly not equal.")
```

最后，如果第一个条件不满足，我们可以使用 elif 块来执行代码，但这个条件是:

```
if 1 == 2:
    print(" Nope")
elif 2 == 2:
    print(" it does")
else:
    print("What?")
```

请注意，如果我们使用 elif，在这些条件下不会运行其他任何东西。在本例中，一次只会运行这些块中的一个。为了改变这一点，我们将再次使用 if 关键字创建完全独立的条件语句。

# 班级

许多 Python 程序员每天都在编写这种语言，却没有意识到所有的类实际上都有一个 bool 参数。默认情况下，该类将始终设置为 true。我们可以通过创建一个新类型，初始化该类型，然后用 if 语句测试该类型来证明这一点，就像我们之前提到的那样。让我们创建一个新的班级:

```
class Frog:
    def __init__(self, age):
        self.age = age
```

现在让我们初始化构造函数，并在条件语句中使用它:

```
froggy = Frog(5)
if froggy:
    print(" It's awfully froggy out here.")
```

> 很酷，对吧？

我们可以通过重新定义它的 __bool__()函数来改变这个类的属性。我们将把它添加到前面的类中，并返回 false:

```
class Frog:
    def __init__(self, age):
        self.age = age
    def __bool__(self):
        return False
```

现在，如果我们再次通过我们的条件语句运行相同的代码，我们将看到我们的字符串不再被打印:

```
froggy = Frog(5)
if froggy:
    print(" It's awfully froggy out here.")
```

# 结论

在计算世界中，布尔和真值是一个非常有价值和重要的概念。大多数从头开始的编程很可能介于使用类型、循环和条件之间，因为这些几乎是程序员唯一可用的工具。至少在 Python 中是这样，因为这种语言的迭代更少，更高级的特性。

对于 Python 程序员来说，知道如何在类内外处理真值和假值是非常必要的信息。编程中的一切都有真或假的值，所以非常熟悉它们是如何工作的可能是明智的。