# Python 中重载函数的正确方法

> 原文：<https://towardsdatascience.com/the-correct-way-to-overload-functions-in-python-b11b50ca7336?source=collection_archive---------5----------------------->

## 有人教过你函数重载在 Python 中是不可能的吗？下面是如何使用通用函数和多重分派来实现的！

![](img/d3d063fbfe938cc916a5a96c8fb00d86.png)

[罗迪昂·库察耶夫](https://unsplash.com/@frostroomhead?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

函数重载是一种常见的编程模式，似乎只适用于静态类型的编译语言。然而，借助于*多重分派*或 Python *多重方法*的帮助，有一种简单的方法可以在 Python 中实现它。

# 过载

首先，你可能会问，在我们都知道不可能的情况下，我们如何在 Python 中实现方法重载呢？嗯，尽管 Python 是动态类型语言，因此不能有适当的方法重载，因为这要求语言能够在编译时区分类型，但我们仍然可以用一种适合动态类型语言的稍微不同的方式来实现它。

这种方法被称为*多重分派*或*多重方法*，其中解释器基于动态确定的类型在运行时区分函数/方法的多个实现。更准确地说，该语言使用在函数调用期间传递给函数的参数类型来动态选择使用(或分派)多个函数实现中的哪一个。

现在你可能会想:*“我们真的需要这个吗？如果它不能正常实现，也许我们不应该在 Python 中使用它……”*是的，有道理，但有很好的理由想在 Python 中实现某种形式的函数/方法重载。这是一个强大的工具，可以使代码更加简洁，易读，并将其复杂性降至最低。如果没有多重方法，那么*“显而易见的方法”*就是使用带有`isinstance()`的类型检查。这是一个非常丑陋、脆弱的解决方案，不允许扩展，我称之为反模式。

除此之外，Python 中已经有了操作符的方法重载，像使用所谓的 *dunder* 或 *magic* 方法的`len()`或`new()`方法(参见文档[这里的](https://docs.python.org/3/reference/datamodel.html#special-method-names))我们都经常使用，那么为什么不对*所有*函数使用适当的重载呢，对吗？

现在我们知道我们可以用 Python 来实现重载，那么我们具体是怎么做的呢？

# 单一调度

上面我们谈到了*多重分派*，但是 Python 不支持这种开箱即用，或者换句话说*多重分派*不是 Python 标准库的特性。然而，我们所能得到的叫做*单次发货*，所以让我们先从这个简单的例子开始。

*多*和*单*调度之间唯一的实际区别是我们可以重载的参数数量。因此，对于标准库中的这个实现，它只有一个。

提供这个特性的函数(和装饰器)被称为`singledispatch`，可以在`functools` [模块](https://docs.python.org/3/library/functools.html#functools.singledispatch)中找到。

这整个概念最好用一些例子来解释。我们可能都已经见过很多重载函数的例子(几何图形、加法、减法……)。与其讨论这个，不如让我们看一些实际的例子。所以，这里是`singledispatch`格式化日期、时间和日期时间的第一个例子:

我们从定义将要重载的基本`format`函数开始。这个函数用`@singledispatch`修饰，并提供基本实现，如果没有更好的选择，就使用这个函数。接下来，我们为我们想要重载的每种类型定义单独的函数——在本例中是`date`、`datetime`和`time`——每个函数都有名称`_`(下划线)，因为无论如何它们都将通过`format`方法被调用(调度)，所以不需要给它们起一个有用的名称。它们中的每一个都装饰有`@format.register`，将它们连接到前面提到的`format`功能。然后，为了区分类型，我们有两种选择——我们可以使用类型注释——如前两种情况所示，或者显式地将类型添加到 decorator，如示例中的最后一种。

在某些情况下，对多种类型使用相同的实现可能是有意义的——例如对于像`int`和`float`这样的数字类型——在这些情况下，装饰堆栈是允许的，这意味着您可以列出(堆栈)多行`@format.register(type)`来将一个函数与所有有效类型相关联。

除了重载基本函数的能力之外，`functools`模块还包含可以应用于类的方法的`[singledispatchmethod](https://docs.python.org/3/library/functools.html#functools.singledispatchmethod)`。这方面的例子如下:

# 多重调度

通常*单一分派*是不够的，你可能需要适当的*多重分派*功能。这可从`multipledispatch`模块获得，该模块可在此处[找到](https://pypi.org/project/multipledispatch/)，并可与`pip install multipledispatch`一起安装。

这个模块和它的装饰者——`@dispatch`，行为与标准库中的`@singledispatch`非常相似。唯一的实际区别是它可以接受多种类型作为参数:

上面的代码片段展示了我们如何使用`@dispatch`装饰器来重载多个参数，例如实现不同类型的连接。正如你可能注意到的，使用`multipledispatch`库，我们不需要定义和注册基本函数，而是创建多个同名的函数。如果我们想提供基本实现，我们可以使用`@dispatch(object, object)`，它将捕捉任何非特定的参数类型。

前面的例子展示了概念验证，但是如果我们想真正实现这样的`concatenate`函数，我们需要使它更加通用。这可以通过使用联合类型来解决。在这个具体示例中，我们可以将第一个函数更改如下:

这将使得函数的第一个参数可以是`list`或`tuple`中的任何一个，而第二个参数可以是`str`或`int`。这已经比前一个解决方案好得多，但是可以使用抽象类型进一步改进。不用列出所有可能的序列，我们可以使用`Sequence`抽象类型(假设我们的实现可以处理它)，它涵盖了类似`list`、`tuple`或`range`的内容:

如果您想采用这种方法，那么最好查看一下`collections.abc`模块，看看哪种容器数据类型最适合您的需求。主要是为了确保您的函数能够处理属于所选容器的所有类型。

所有这些参数类型的混合和匹配都很方便，但是在为一些特定的参数集选择合适的函数时，也会导致歧义。幸运的是，`multipledispatch`提供了`AmbiguityWarning`，如果可能出现不明确的行为，就会引发该事件:

# 结束语

在本文中，我们讨论了一个简单而强大的概念，我很少看到它在 Python 中使用，考虑到它可以极大地提高代码可读性并摆脱反模式，如使用`isinstance()`的类型检查，这是一个遗憾。另外，我希望你会同意这种函数重载的方法应该被认为是*“显而易见的方法”*，我希望你在需要的时候会用到它。

如果你想更深入地研究这个主题，你可以自己实现多重方法，如 Guido 的文章所示——这是理解多重分派实际上是如何工作的一个很好的练习。

最后，我可能还应该提到，这篇文章省略了我在开始提到的众所周知的[操作符重载](https://docs.python.org/3/reference/datamodel.html#basic-customization)的例子，以及一些重载构造函数的方法，例如使用[工厂](https://stackoverflow.com/a/141777)。所以，如果那是你正在寻找的，去看看这些链接/资源，它们对这些主题有很好的概述。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/50?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_50)

</all-the-important-features-and-changes-in-python-3-10-e3d1fe542fbf>  </scheduling-all-kinds-of-recurring-jobs-with-python-b8784c74d5dc>  </the-magic-of-python-context-managers-adb92ace1dd0> 