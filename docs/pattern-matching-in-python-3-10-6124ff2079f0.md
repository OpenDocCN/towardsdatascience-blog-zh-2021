# Python 3.10 中的模式匹配

> 原文：<https://towardsdatascience.com/pattern-matching-in-python-3-10-6124ff2079f0?source=collection_archive---------7----------------------->

## 类固醇的转换声明

![](img/953174cf577b6320bc052b86f753f192.png)

[Teo Duldulao](https://unsplash.com/@teowithacamera?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Python 3.10 实现了 *switch* 语句——算是吧。C 或 Java 等其他语言中的 switch 语句对变量进行简单的值匹配，并根据该值执行代码。

> 它可以作为一个简单的 switch 语句使用，但它的功能远不止于此。

这对于 C 来说可能已经足够好了，但是这是 Python，Python 3.10 实现了一个更加强大和灵活的结构，叫做*结构模式匹配。*它可用作简单的 switch 语句，但功能远不止于此。

让我们以简单的开关为例。下面是对单个值进行切换的代码片段。我们通过用值 1、2、3 和 4 在一个循环中运行它来测试它。

```
for thing in [1,2,3,4]:
    match thing:
        case 1:
            print("thing is 1")
        case 2:
            print("thing is 2")
        case 3:
            print("thing is 3")
        case _:
            print("thing is not 1, 2 or 3")
```

首先要注意的是语法的整洁。它以关键字`match`开始，后跟一个变量名。然后是一个以`case`开头的案例列表，后面是匹配的值。注意冒号和缩进的使用。

这与其他语言中的 switch/case 语句并不相似，但与 C 不同，例如，在执行了特定 case 的代码后，控制跳转到`match`语句的末尾。

如果不匹配，则执行默认情况下的代码，由`-`表示。

这就是结果。

```
thing is 1
thing is 2
thing is 3
thing is not 1, 2 or 3
```

没什么好惊讶的。

switch 语句是模式匹配的一个简单例子，但是 Python 做了进一步的发展。看一下这段代码:

```
for thing in [[1,2],[9,10],[1,2,3],[1],[0,0,0,0,0]]:
    match thing:
        case [x]:
            print(f"single value: {x}")
        case [x,y]:
            print(f"two values: {x} and {y}")
        case [x,y,z]:
            print(f"three values: {x}, {y} and {z}")       
        case _:
            print("too many values")
```

这又是一个循环中的`match`语句，但这次循环将遍历的值列表是列表本身——第一个是`[1,2]`,然后是`[9,10]` ,依此类推。

case 语句试图匹配这些列表。第一种情况匹配单个元素的列表，第二种情况匹配两个元素的列表，第三种情况匹配三个元素的列表。最后一种情况是默认的。

但它的作用不止于此。它还绑定与`case`语句中的标识符匹配的值。例如，第一个列表是`[1,2]`，它匹配第二个案例`[x,y]`。因此，在执行的代码中，标识符 x 和 y 分别取值 1 和 2。很好，嗯！

所以，结果是这样的:

```
two values: 1 and 2
two values: 9 and 10
three values: 1, 2 and 3
single value: 1
too many values
```

从第一个简单的程序中我们知道我们可以匹配值，从上面的程序中我们可以匹配更一般的模式。那么我们能匹配包含值的模式吗？当然啦！

```
for thing in [[1,2],[9,10],[3,4],[1,2,3],[1],[0,0,0,0,0]]:
    match thing:
        case [x]:
            print(f"single value: {x}")
        case [1,y]:
            print(f"two values: 1 and {y}")
        case [x,10]:
            print(f"two values: {x} and 10")
        case [x,y]:
            print(f"two values: {x} and {y}")
        case [x,y,z]:
            print(f"three values: {x}, {y} and {z}")       
        case _:
            print("too many values")
```

例如，您可以看到，在第二种情况下，我们匹配一个包含两个元素的列表，其中第一个元素的值为 1。

结果如下:

```
two values: 1 and 2
two values: 9 and 10
two values: 3 and 4
three values: 1, 2 and 3
single value: 1
too many values
```

但是还有更多。

这是一个匹配任意数量元素的列表的程序。

```
for thing in [[1,2,3,4],['a','b','c'],"this won't be matched"]:
    match thing:
        case [*y]:
            for i in y:
                print(i)
        case _:
            print("unknown")
```

在第一种情况下，标识符是带星号的，这意味着整个列表可以绑定到它，正如我们看到的，它可以在一个循环中迭代。

```
1
2
3
4
a
b
c
unknown
```

还有更多的内容，但是希望这篇文章已经让您体验了 Python 3.10 中的结构模式匹配。

*更新:我在这里* *写过更高级的模式匹配* [*。*](/more-advanced-pattern-matching-in-python-3-10-2dbd8598302a)

在[Python.org](http://python.org)网站上有关于结构模式匹配的完整描述，包括一个教程，在 [PEP 636](https://www.python.org/dev/peps/pep-0636/) 中。教程非常好，我鼓励你去读它。

除了结构模式匹配，Python 3.10 还有很多新的发展。几篇文章解释了它们，例如，参见[James Briggs](https://medium.com/u/b9d77a4ca1d1?source=post_page-----6124ff2079f0--------------------------------)[Python 3.10 中的新特性](/new-features-in-python-3-10-66ac05e62fc7)。

但是，请注意，3.10 仍然只是一个测试版，虽然我上面展示的例子运行得非常好，但是在正式发布之前，你不应该愤怒地使用 Python 3.10。