# Python 3.10 中的所有重要特性和变化

> 原文：<https://towardsdatascience.com/all-the-important-features-and-changes-in-python-3-10-e3d1fe542fbf?source=collection_archive---------1----------------------->

## Python 3.10 的发布越来越近了，所以是时候看看它将带来的最重要的新特性和变化了

![](img/323972c67383472087a2dcedb51028bb.png)

[大卫·克洛德](https://unsplash.com/@davidclode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

又到了一年中最后一个 Python alpha 版本发布的时候了，第一个 beta 版本也在路上了，所以这是一个理想的时间来体验一下新版本的 Python，看看这一次 Python 3.10 中有哪些很酷的新特性！

# 安装 Alpha/Beta 版本

如果你想尝试最新最好版本 Python 的所有特性，那么你需要安装 *Alpha/Beta* 版本。然而，考虑到这还不是一个稳定的版本，我们不想用它覆盖我们的默认 Python 安装。因此，要安装 Python 3.10 和我们当前的解释器，我们可以使用以下代码:

运行上述代码后，您将看到 Python 3.10 Alpha IDLE:

随着 Python 3.10 的安装，我们可以看看所有的新功能和变化…

# 类型检查改进

如果你在 Python 中使用类型检查，你会很高兴听到 Python 3.10 将包括许多类型检查改进，包括带有更简洁语法的*类型联合操作符*:

最重要的是，这个简单的改进不仅限于类型注释，还可以用于`isinstance()`和`issubclass()`函数:

# 类型别名语法更改

在 Python 的早期[版本中，添加了类型别名，以允许我们创建代表用户定义类型的别名。在 Python 3.9 或更早的版本中，应该是这样的:](https://www.python.org/dev/peps/pep-0484)

这里的`FileName`是基本 Python 字符串类型的别名。但是，从 Python 3.10 开始，定义类型别名的语法将更改如下:

这个简单的改变将使程序员和类型检查人员更容易区分普通的变量赋值和类型别名。这一更改也是向后兼容的，因此您不必更新任何使用类型别名的现有代码。

除了这两个变化之外，打字模块还有其他的改进——即 [PEP 612](https://www.python.org/dev/peps/pep-0612) 中的*参数规格变量*。然而，这些在大多数 Python 代码库中并不常见，因为它们用于将一个可调用的参数类型转发给另一个可调用的参数类型(例如在 decorators 中)。如果你有这样的用例，去看看上面提到的 PEP。

# 种群统计

从 Python 3.10 开始，你可以使用`int.bit_count()`来计算一个整数的二进制表示的位数。这也被称为*人口计数(popcount)* :

这当然很好，但是现实一点，实现这个函数并不困难，它只是一行代码:

也就是说，这是另一个方便的功能，在某些时候可能会派上用场，这些有用的小功能是 Python 如此受欢迎的原因之一——似乎所有东西都是现成的。

# `Distutils`已被弃用

在新版本中，不仅添加了一些东西，还取消/删除了一些东西。对于`distutils`包来说就是这样，它在 3.10 中被弃用，并将在 3.12 中被移除。这个包已经被`setuptools`和`packaging`取代有一段时间了，所以如果你正在使用这两个中的任何一个，那么你应该没问题。也就是说，你可能应该检查一下你的代码中`distutils`的用法，并开始准备在不久的将来去掉它。

# 上下文管理器语法

Python 上下文管理器非常适合打开/关闭文件、处理数据库连接和许多其他事情，在 Python 3.10 中，它们的语法将获得一点生活质量的改善。这一变化允许带括号的上下文管理器跨越多行，如果您想在一条`with`语句中创建多个上下文管理器，这将非常方便:

正如你从上面看到的，我们甚至可以引用一个上下文管理器(`... as some_file`)创建的变量到它后面的另一个上下文管理器中！

这些只是 Python 3.10 中众多新格式中的两种。这种改进的语法非常灵活，所以我不会费心展示每一个可能的格式选项，因为我非常确定无论你在 Python 3.10 中抛出什么，它都很可能会工作。

# 性能改进

和所有最新版本的 Python 一样，Python 3.10 也带来了一些性能改进。首先是对`str()`、`bytes()`和`bytearray()`构造函数的优化，应该会快 30%左右(摘自 Python bug tracker [示例](https://bugs.python.org/issue41334)):

另一个更值得注意的优化(如果使用类型注释)是函数参数及其注释不再在运行时计算，而是在编译时计算。这使得创建带参数注释的函数的速度提高了大约 2 倍。

除此之外，Python core 的各个部分还进行了一些优化。你可以在 Python bug tracker 的以下几个问题中找到细节: [bpo-41718](https://bugs.python.org/issue41718) 、 [bpo-42927](https://bugs.python.org/issue42927) 和 [bpo-43452](https://bugs.python.org/issue43452) 。

# 模式匹配

你肯定已经听说过的一个重要特性是*结构模式匹配*。这就要加上`case`语句，这个语句我们都是从其他编程语言中知道的。我们都知道如何使用`case`语句，但是考虑到这是 Python——它不仅仅是简单的 *switch/case* 语法，它还增加了一些强大的特性，我们应该探索一下。

模式匹配最基本的形式是由`match`关键字后跟表达式组成，然后根据连续`case`语句中的模式测试其结果:

在这个简单的例子中，我们使用`day`变量作为表达式，然后与`case`语句中的单个字符串进行比较。除了带有字符串文字的`case`之外，您还会注意到最后一个`case`，它使用了`_` *通配符*，相当于其他语言中的`default`关键字。不过可以省略这个通配符，在这种情况下可能会出现 no-op，这实质上意味着返回`None`。

在上面的代码中需要注意的另一件事是`|`的用法，它使得使用`|` ( *或*)操作符组合多个文字成为可能。

正如我提到的，这种新的模式匹配并不以基本语法为结束，而是带来了一些额外的特性，比如复杂模式的匹配:

在上面的代码片段中，我们使用`tuple`作为匹配的表达式。然而，我们并不局限于使用元组——任何可迭代的都可以。此外，正如您在上面看到的，通配符`_`也可以用在复杂模式中，而不像前面的例子那样单独使用。

使用普通元组或列表可能并不总是最好的方法，所以如果您更喜欢使用类，那么可以用以下方式重写:

这里我们可以看到，用类似于类构造函数的模式来匹配类的属性是可能的。当使用这种方法时，单个属性也被捕获到变量中(与前面显示的元组相同)，然后我们可以在各自的`case`的主体中使用这些变量。

上面我们还可以看到模式匹配的一些其他特性——在第一个`case`语句中，它是一个*保护*，这是一个遵循模式的`if`条件。如果按值匹配还不够，并且您需要添加一些额外的条件检查，这将非常有用。看看这里剩下的`case`，我们还可以看到，关键字(例如`name=name`)和位置参数都使用这种类似于*构造函数的*语法，同样的情况也适用于`_`(通配符或*“一次性”*)变量。

模式匹配还允许使用嵌套模式。这些嵌套模式可以使用任何可迭代对象，既可以使用类似于*构造函数的*对象，也可以使用更多的可迭代对象:

在这些类型的复杂模式中，将子模式捕获到变量中以便进一步处理可能是有用的。这可以使用`as`关键字来完成，如上面第二个`case`所示。

最后，`*`操作符可用于*“解包”*模式中的变量，这也适用于使用`*_`模式的`_`通配符。

如果你想看更多的例子和完整的教程，那就去看看 [PEP 636](https://www.python.org/dev/peps/pep-0636/) 。

# 结束语

Python 3.10 带来了许多有趣的新特性，但这是 *alpha* (很快将是 *beta* )版本，它还远未经过全面测试和生产准备。因此，现在就开始使用它绝对不是一个好主意。因此，最好是坐下来等待 10 月份的完全发布，并不时查看[Python 3.10 页面](https://docs.python.org/3.10/whatsnew/3.10.html)中的新内容，以了解任何最新的补充。

也就是说——如果你渴望升级——抓住第一个 *beta* 版本(将在 6 月的某个时候发布)并进行测试，看看你现有的代码库是否与所有即将到来的变化、功能/模块的弃用或删除兼容，这可能不是一个坏主意。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/46?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_46)

[](/scheduling-all-kinds-of-recurring-jobs-with-python-b8784c74d5dc) [## 使用 Python 调度各种循环作业

### 让我们探索一下运行 cron 作业、延迟任务、重复任务或任何其他计划作业的所有库…

towardsdatascience.com](/scheduling-all-kinds-of-recurring-jobs-with-python-b8784c74d5dc) [](/the-magic-of-python-context-managers-adb92ace1dd0) [## Python 上下文管理器的魔力

### 使用和创建令人敬畏的 Python 上下文管理器的方法，这将使你的代码更具可读性、可靠性和…

towardsdatascience.com](/the-magic-of-python-context-managers-adb92ace1dd0) [](/writing-more-idiomatic-and-pythonic-code-c22e900eaf83) [## 编写更加地道和 Pythonic 化的代码

### 使你的 Python 代码可读、有效、简洁和可靠的习惯用法和惯例。

towardsdatascience.com](/writing-more-idiomatic-and-pythonic-code-c22e900eaf83)