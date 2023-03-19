# func tools——Python 中高阶函数的威力

> 原文：<https://towardsdatascience.com/functools-the-power-of-higher-order-functions-in-python-8e6e61c6e4e4?source=collection_archive---------9----------------------->

## 浏览 Python 的 functools 模块，了解如何使用它的高阶函数来实现缓存、重载等等

![](img/5bf0d481b827e09c9793ab94d8a5686d.png)

Joel Filipe 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Python 标准库包括许多伟大的模块，可以帮助你使你的代码更干净、更简单，而`functools`绝对是其中之一。这个模块提供了许多有用的高阶函数，这些函数作用于或返回其他函数，我们可以利用这些函数来实现函数缓存、重载、创建装饰器，并在总体上使我们的代码更具功能性，所以让我们浏览一下它，看看它能提供的所有东西...

# 贮藏

让我们从`functools`模块最简单却非常强大的功能开始。这些是缓存函数(也是装饰器)- `lru_cache`，`cache`和`cached_property`。其中第一个- `lru_cache`提供*最近最少使用的*函数结果缓存，或者换句话说- *结果存储*:

在这个例子中，我们使用`@lru_cache` decorator 获取请求并缓存它们的结果(最多 32 个缓存结果)。为了查看缓存是否真正工作，我们可以使用`cache_info`方法检查函数的缓存信息，它显示缓存命中和未命中的数量。装饰器还提供了`clear_cache`和`cache_parameters`方法，分别用于使缓存的结果无效和检查参数。

如果你想有一个更细粒度的缓存，那么你也可以包含可选的`typed=true`参数，这样不同类型的参数可以分别缓存。

`functools`中的另一个缓存装饰器是一个简单称为`cache`的函数。它是在`lru_cache`之上的一个简单的包装器，省略了`max_size`参数，使它更小，因为它不需要驱逐旧值。

还有一个装饰器可以用于缓存，它叫做`cached_property`。这个——正如您可能猜到的——用于缓存类属性的结果。如果你有一个计算起来很昂贵同时又是不可变的属性，这是非常有用的。

这个简单的例子展示了我们如何使用缓存属性来缓存呈现的 HTML 页面，这些页面会一遍又一遍地返回给用户。对于某些数据库查询或长时间的数学计算也是如此。

`cached_property`的好处是它只在查找时运行，因此允许我们修改属性。修改属性后，将不使用以前缓存的值，而是计算并缓存新值。也可以清除缓存，我们需要做的就是删除属性。

在本节的最后，我要对上述所有装饰器提出警告——如果您的函数有任何副作用或者每次调用都会创建可变对象，请不要使用它们，因为这些不是您想要缓存的函数类型。

# 比较和排序

你可能已经知道在 Python 中可以使用`__lt__`、`__gt__`或`__eq__`实现比较操作符，比如`<`、`>=`或`==`。尽管实现`__eq__`、`__lt__`、`__le__`、`__gt__`或`__ge__`中的每一个都很烦人。幸运的是，`functools`模块包含了`@total_ordering`装饰器，可以帮助我们完成这个任务——我们需要做的就是实现`__eq__`,剩下的方法和 rest 将由装饰器自动提供:

上面显示了即使我们只实现了`__eq__`和`__lt__`我们也能够使用所有丰富的比较操作。这样做最明显的好处是不必编写所有额外的神奇方法，但更重要的可能是减少了代码并提高了可读性。

# 过载

可能我们都被告知函数重载在 Python 中是不可能的，但是实际上有一个简单的方法来实现它，使用 `functools`模块中的两个函数- `singledispatch`和/或`singledispatchmethod`。这些函数帮助我们实现我们所谓的*多重分派*算法，这是 Python 等动态类型编程语言在运行时区分类型的一种方式。

考虑到函数重载本身就是一个很大的话题，我专门为 Python 的`singledispatch`和`singledispatchmethod`写了一篇文章，所以如果你想了解更多，你可以在这里阅读更多:

[](/the-correct-way-to-overload-functions-in-python-b11b50ca7336) [## Python 中重载函数的正确方法

towardsdatascience.com](/the-correct-way-to-overload-functions-in-python-b11b50ca7336) 

# 部分的

我们都使用各种外部库或框架，其中许多提供了需要我们传入回调函数的函数和接口——例如用于异步操作或事件监听器。这没什么新鲜的，但是如果我们还需要在回调函数中传递一些参数呢？这就是`functools.partial`派上用场的地方- `partial`可以用来*冻结*函数的一些(或全部)参数，用简化的函数签名创建新对象。迷惑？让我们看一些实际的例子:

上面的代码片段演示了我们如何使用`partial`来传递函数(`output_result`)及其参数(`log=logger`)作为回调函数。在这种情况下，我们使用`multiprocessing.apply_async`，它异步计算提供的函数(`concat`)的结果，并将其结果返回给回调函数。然而，`apply_async`总是将结果作为第一个参数传递，如果我们想要包含任何额外的参数，就像在本例中的`log=logger`一样，我们必须使用`partial`。

这是一个相当高级的用例，所以一个更基本的例子可能是简单地创建打印到`stderr`而不是`stdout`的函数:

通过这个简单的技巧，我们创建了一个新的 callable(函数),它总是将`file=sys.stderr`关键字参数传递给`print`,这样我们就不必每次都指定关键字参数，从而简化了代码。

最后一个好的衡量标准的例子。我们还可以使用`partial`来利用`iter`函数鲜为人知的特性——可以通过向`iter`传递 callable 和 sentinel 值来创建一个迭代器，这在下面的应用程序中很有用:

通常，当读取一个文件时，我们希望遍历所有行，但是对于二进制数据，我们可能希望遍历固定大小的记录。这可以通过使用读取指定数据块的`partial`创建 callable 并将其传递给`iter`来完成，然后由后者创建迭代器。这个迭代器然后调用`read`函数，直到到达文件末尾，总是只取指定的数据块(`RECORD_SIZE`)。最后，当到达文件结尾时*返回标记值* ( `b''`)，迭代停止。

# 装修工

我们已经在前面的章节中讨论了一些装饰者，但是没有讨论创造更多装饰者的装饰者。一个这样的装饰器是`functools.wraps`，为了理解我们为什么需要它，让我们首先看一下下面的例子:

这个例子展示了如何实现一个简单的装饰器——我们用外部的`decorator`函数包装执行实际任务的函数(`actual_func`),外部的`decorator`函数成为我们可以附加到其他函数的装饰器——例如这里的`greet`函数。当`greet`函数被调用时，你会看到它打印了来自`actual_func`的消息以及它自己的消息。一切看起来都很好，这里没有问题，对不对？但是，如果我们尝试以下方法会怎么样:

当我们检查修饰函数的 name 和 docstring 时，我们发现它被 decorator 函数内部的值替换了。这并不好——我们不能在每次使用 decorator 时覆盖所有的函数名和文档。那么，我们如何解决这个问题呢？—带`functools.wraps`:

`wraps`函数唯一的工作就是复制名称、文档字符串、参数列表等。以防止它们被覆盖。考虑到`wraps`也是一个装饰者，我们可以把它放到我们的`actual_func`上，问题就解决了！

# 减少

在`functools`模块中最后但同样重要的是`reduce`。你可能从其他语言中知道它是`fold` (Haskell)。这个函数的作用是获取一个 iterable，然后*将*(或折叠)它的所有值变成一个值。这有许多不同的应用，以下是其中一些:

从上面的代码中可以看出，`reduce`可以简化并经常将代码压缩成单行，否则代码会很长。也就是说，仅仅为了缩短代码、使*【聪明】*或使*更实用*而过度使用这个函数通常是个坏主意，因为它会很快变得难看和不可读，所以在我看来——少用它。

此外，考虑到使用`reduce`通常会产生一行程序，它是`partial`的理想候选:

最后，如果你不仅需要最终的*简化的*结果，还需要中间结果，那么你可以使用`accumulate`来代替——来自另一个伟大模块`itertools`的函数。这就是你如何使用它来计算运行最大值:

# 结束语

正如你在这里看到的，`functools`提供了许多有用的函数和装饰器，可以让你的生活更轻松，但是这个模块只是冰山一角。正如我在开始时提到的，Python 标准库包括许多可以帮助你构建更好代码的模块，所以除了我们在这里探索的`functools`，你可能还想检查其他模块，比如`operator`或`itertools`(我也写过关于这个的文章👇)或者直接进入 [Python 模块索引](https://docs.python.org/3/py-modindex.html)，点击任何引起你注意的东西，我相信你会在那里找到有用的东西。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/52?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_52)

[](/tour-of-python-itertools-2af84db18a5e) [## Python Itertools 之旅

### 让我们探索两个伟大的 Python 库——ITER tools 和 more_itertools，看看如何利用它们来处理数据…

towardsdatascience.com](/tour-of-python-itertools-2af84db18a5e) [](/making-python-programs-blazingly-fast-c1cd79bd1b32) [## 让 Python 程序快得惊人

### 让我们看看我们的 Python 程序的性能，看看如何让它们快 30%！

towardsdatascience.com](/making-python-programs-blazingly-fast-c1cd79bd1b32) [](/ultimate-guide-to-python-debugging-854dea731e1b) [## Python 调试终极指南

### 让我们探索使用 Python 日志记录、回溯、装饰器等等进行调试的艺术…

towardsdatascience.com](/ultimate-guide-to-python-debugging-854dea731e1b)