# 四种不同寻常的 Python 代码味道

> 原文：<https://towardsdatascience.com/four-unusual-python-code-smells-7bf3d93f5e6c?source=collection_archive---------19----------------------->

## 以及如何处理它们。

![](img/51e124de6046dc76461ac7ad3a5a845d.png)

照片由来自 [Pexels](https://www.pexels.com/photo/crop-man-smelling-white-flower-in-garden-7125403/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Michael Burrows](https://www.pexels.com/@michael-burrows?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄，由作者使用 Python 修改

当我想到编程和教育的时候，我畏缩了。我畏缩是因为课程涉及所有编程方面，除了一个——软件设计。可悲的是，软件设计是软件开发的关键要素。

> "写代码不是生产，也不总是技艺，尽管它可以是，它是设计."— [乔尔·斯波尔斯基](https://quotefancy.com/programming-quotes)

如果没有软件设计，人们会停留在 20 世纪 90 年代——那时软件通常是从零开始实现的。然而今天，最小的创业公司都会产生数千行代码，更不用说科技巨头、游戏工作室、汽车制造商等等。

这些公司通常基于以前版本的代码开发新软件，这为创新和创造提供了空间。当然，如果没有一个可维护、可重用、可读和高效的代码库，这是不可能的。这些特性共同阐明了良好的代码重构实践或反代码味道的本质。

“代码味道”这个术语是由 fowler 和 kent 在他们的书《重构:改进现有代码的设计》中提出的。但实际上，这个术语只不过是糟糕的代码设计的一个厚颜无耻的同义词。

也就是说，在这篇文章中，我们将讨论四种不寻常的坏代码味道以及重构它们的方法。它们是“不寻常的”,因为据我所知，我们将要讨论的代码味道不是你通常在互联网上偶然发现的。所以，我希望你能像我一样从中受益。让我们开始吧:

# Lambda 函数的误用

函数是传统函数的语法糖。它们是运行时调用的匿名构造。它们的效用源于它们的简洁。

然而，当 lambda 变得太长而难以阅读或太复杂而难以理解时，它就失去了魅力和有效性。更重要的是，lambda 在封装不可重用代码方面崛起并大放异彩。否则，您最好使用标准函数。

为了解决 lambda 不恰当使用的不同场景，我们汇编了 lambda 开始发臭的三个用例:

## #1.将 Lambda 绑定到变量

考虑这个例子:

好闻:

```
def f(x, y): 
    return x + y
```

难闻的气味:

```
f = lambda x, y: x + y
```

乍一看，人们可以自信地说，将 lambda 绑定到一个变量就像显式的 *def* 声明一样好。但是实际上，这种实践是一种软件反模式，因为:

*   首先，它违背了 lambda 函数的定义。也就是说，你给了一个**匿名**函数一个**名字**。
*   第二，它把 lambda 函数的目的抹杀了。也就是把 lambda 嵌入到更大的表达式中( [PEP 8](https://legacy.python.org/dev/peps/pep-0008/) )。

## #2.长λ

到目前为止，很明显长 lambda 函数是糟糕代码设计的暗示。然而，问题是允许测量这种长度的启发式方法。

嗯，研究表明 lambda 表达式应该遵守以下标准:

*   NOC ≤ 80

NOC:字符数

因此，lambda 表达式不应超过 80 个字符。

## #3.肮脏的λ

Lambda 函数吸引了许多开发人员，尤其是初级开发人员。它的便利性和美学设计很可能会让人陷入肮脏的λ陷阱。

你看，lambda 被设计成执行一个表达式。建议此表达式的字符数少于一定数量。为了避开这种约束，出现了一些卑鄙的手段和变通方法。

嵌套的 lambda 函数和内部标准函数是臭名昭著的肮脏的变通方法。让我们举一个例子来近距离观察这一点:

难闻的气味:

```
# inner lambda function
func = lambda x= 1, y= 2:lambda z: x + y + z
f = func()
print(f(4))
```

好闻:

```
def func(x=1, y=2):
    def f(z):
        return x + y + z
    return f
f = func()
print(f(4))
```

正如您所看到的，虽然 lambda 示例更简洁(3 行对 6 行)，但是 lambda 代码更混乱，更难破译。这种做法引起的混乱的一个例子是这个[线程](https://stackoverflow.com/questions/36391807/understanding-nested-lambda-function-behaviour-in-python)。

# 长范围链接

长范围链接是嵌套在封闭函数中的内部函数的集合。内部函数在技术上被称为*闭包。*以下示例提供了一个更清晰的画面:

```
def foo(x):
    def bar(y):
        def baz(z):
            return x + y + z
        return baz(3)
    return bar(10)print(foo(2))  # print(10+3+2)
```

用内部函数填充函数是一个非常有吸引力的解决方案，因为它模拟了隐私，这对于 Python 这样的语言来说特别有用和方便。这是因为，与 C++和 Java 不同，Python 几乎没有私有和公共类变量的区别(尽管有一些黑客攻击)。然而，这种实践开始嗅到闭包越深的味道。

为了解决这个问题，已经为闭包设置了一个阈值试探。该指标规定最多有 3 个闭包。否则，代码开始看起来模糊，变得难以维护。

# 无用的异常处理

异常处理程序是程序员用来捕捉异常的常用工具。它们在测试代码中非常有用。然而，如果异常是(1)不准确的或(2)空的，它们就变得没有用了。

## #1.不准确的例外

try … except 语句给了程序员管理异常的自由。这导致了非常一般和不精确的异常。看看这个:

```
try:
   pass
except Exception:
   raise
# OR
try:
   pass
except StandardError:
   raise
```

在这个例子中，异常太过笼统，很可能预示着一系列的错误，很难发现问题的根源。这就是为什么建议对异常要精确和具体。下面的示例是一个很好的实践，它专门用于指示导入错误:

```
try:
    import platform_specific_module
except ImportError:
    platform_specific_module = None
```

## #2.空异常

谈到错误处理程序，没有什么比纯粹的异常更糟糕的了。

空`except:`捕捉`systemExit`和`KeyboardInterrupt`异常，用 Ctrl+C 渲染程序中断更加困难。更不用说掩饰其他问题了。

为了解决这个问题，Python 风格指南 [PEP 8](https://legacy.python.org/dev/peps/pep-0008/) 建议将简单的异常限制为两种用例:

*   无论问题的性质如何，用户都希望标记错误或记录追溯。
*   如果用户想从底部到顶部提出异常，`try … finally`是一个很好的选择。

## #3.补救措施？

与其他引用的代码味道不同，重构异常没有放之四海而皆准的解决方案。然而，一般的补救措施是尽可能具体和仔细地编写异常处理程序。

# 著名系列(len(sequence))

坦率地说，`range(len())`是一个坏习惯。这曾经是我默认的循环机制。但是，我很高兴现在我不记得我最后一次使用它。

`range(len())`吸引新的 Python 开发者。它甚至吸引了经验丰富的开发人员，他们的数字循环(C++之类的循环)已经在他们的大脑中形成。对于这些人来说，`range(len())`感觉像家一样，因为它复制了与传统数值循环相同的循环机制。

另一方面，`range(len())`被 Python 战士和经验丰富的开发人员所轻视，因此被认为是迭代反模式。原因是`range(len())`使得代码容易出现 bug。这些错误很大程度上源于程序员忘记了`range()`的第一个参数是包含性的，而第二个是排他性的。

为了一劳永逸地解决这个问题，我们将列举使用`range(len())`的常见借口及其正确的替代表达。

*   您需要序列的索引:

```
**for** index, value **in** enumerate(sequence): 
        **print** index, value
```

*   您希望同时迭代两个序列:

```
**for** letter, digit **in** zip(letters, digits):
        **print** letter, digit
```

*   您想要迭代序列的一个块:

```
**for** letter **in** letters[4:]: #slicing        **print** letter
```

如你所见，避免`range(len())`是可能的。尽管如此，当序列索引的使用超出了序列本身(例如函数)时，使用`range(len())`似乎是一个明智的选择。例如:

```
**for** x **in** range(len(letters)):
    **print** f(x)
```

# 外卖食品

技术进步给人们编写和分析代码的方式带来了巨大的变化。变得更好。然而，反模式、代码味道、糟糕的代码设计等等，仍然是一个非常主观和开放的话题。

软件设计原则是主观的，因为它们基于现实生活的经验和固执己见的观点。这也许就是为什么软件开发者聚会不能没有软件设计辩论的原因。

例如，一些开发人员发现长 lambda 函数会使代码变得很臭，而其他开发人员则喜欢 lambda，并认为它是 pythonic 式的，无害的。我个人认为，袖手旁观站在前者一边。

简而言之，没有一个软件能够在没有重构实践的情况下生存下来，正如俗话所说:

> “臭了就换。”—肯特·贝克和马丁·福勒

我希望这篇文章能让你的代码看起来更好。

# 参考

<https://www.semanticscholar.org/paper/JSNOSE%3A-Detecting-JavaScript-Code-Smells-Fard-Mesbah/86dd17663c963772e6dd3ec8e2b1ab4a8a0e377f>  <https://ieeexplore.ieee.org/document/7780188>   