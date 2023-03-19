# 你讨厌 Python 有多慢吗？这就是你如何让它跑得更快！

> 原文：<https://towardsdatascience.com/do-you-hate-how-slow-python-is-this-is-how-you-can-make-it-run-faster-532468fe1583?source=collection_archive---------15----------------------->

## 将具有挑战性的 *任务*委托给另一名玩家

![](img/7ae6e6822f680bdcb689e6563e84ab1b.png)

图片由[努格罗霍德威哈特万](https://pixabay.com/users/jambulboy-4860762/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4463366)从[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4463366)

Python 是一种出色的编程语言，也是作者的首选语言。在我看来，当你慢慢进入计算机科学和编程世界时，Python 是最好的学习语言。

然而，每种编程语言都有它的优点和缺点；这就是为什么我们有这么多这样的人。不同的用例需要不同的设计和实现。正如我们所说，没有放之四海而皆准的解决方案。

Python 是一种用户友好、易于学习、免费使用、可移植且易于扩展的编程语言。它可以适应您的编程风格和需求，涵盖各种风格，从面向对象(OOP)到函数式编程方法。

另一方面，Python 是一种解释型语言，它可以设置代码执行速度的上限。它不太适合移动开发，它有内存问题，而且，因为它是一种动态类型的语言，您会在运行时发现大多数错误。

但是并不是所有的希望都破灭了。我认为最关键的问题是速度。但是为什么更快的运行时间如此重要呢？考虑机器学习的用例；你需要快速试验和迭代。当您处理大数据时，让您的功能在几秒钟而不是几分钟内返回至关重要。如果你刚刚发明的新的闪亮的神经网络架构不那么好，你应该能够迅速转向你的下一个想法！

为了解决这个问题，我们可以用另一种语言(例如，C 或 C++)编写要求苛刻的函数，并利用特定的绑定从 Python 调用这些函数。这是许多数字库(例如 NumPy、SciPy 等)都有的。)或深度学习框架(如 TensorFlow、PyTorch 等。)用 Python 做。因此，如果你是一名数据科学家或机器学习工程师，想要调用 CUDA 函数，这个故事适合你。开始吧！

> [Learning Rate](https://www.dimpo.me/newsletter/?utm_source=medium&utm_medium=article&utm_campaign=python-bindings) 是为那些对 AI 和 MLOps 世界好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# 编组的

在这个旅程中，我们必须采取的第一步是理解编组是什么以及它是如何工作的。来自[维基百科](https://en.wikipedia.org/wiki/Marshalling_(computer_science)):

> 编组是将对象的内存表示形式转换为适合存储或传输的数据格式的过程。

为什么这对我们的主题很重要？为了将数据从 Python 移动到 C 或 C++，Python 绑定必须将数据转换成适合传输的形式。

在 Python 中，一切都是对象。整数使用多少字节的内存取决于您安装的 Python 版本和操作系统，以及其他因素。另一方面，C 中的一个`uint8_t`整数总是使用总内存的 8 位。因此，我们必须以某种方式调和这两种类型。

编组是 Python 绑定为我们处理的事情，但是在某些情况下我们可能需要干预。这个故事不会出现这种情况，但我们会在后面的文章中遇到。

# 管理内存

c 和 Python 管理内存的方式不同。在 Python 中，当你声明一个对象时，Python 会自动为它分配内存。当您不需要该对象时，Python 有一个垃圾收集器，可以销毁未使用或未引用的对象，将内存释放回系统。

在 C 语言中，情况完全不同。是你，程序员，必须分配内存空间来创建一个对象，然后又是你，必须将内存释放回系统。

我们应该考虑到这一点，在语言障碍的同一侧释放任何我们不再需要的内存。

# 简单的例子

我们现在已经到了准备下水的时候了。当您完成这一部分时，您将准备好开始使用 Python 绑定和 c。这绝对是一个初学者教程，但我们将在后面的故事中更深入地研究更复杂的示例。

## 你需要什么

对于这个故事，我们需要两样东西:

*   Python 3.6 或更高版本
*   Python 开发工具(例如，`python3-dev`包)

## C 源代码

为了简单起见，我们将创建并构建一个将两个数相加的 C 库。复制下面的源代码:

接下来，我们需要编译源代码并构建一个共享库。为此，执行下面的命令:

```
gcc -shared -Wl,-soname,libcadd -o libcadd.so -fPIC cadd.c
```

这个命令应该会在您的工作目录中产生一个`libcadd.so`文件。现在，您已经准备好进入下一步。

## 用`ctypes`窥探一个 C 库

`ctypes`是 Python 标准库中创建 Python 绑定的工具。作为 Python 标准库的一部分，它非常适合我们的初学者教程，因为您不需要安装任何东西。

要从 Python 脚本执行 C `cadd`函数，请复制下面的源代码:

在第 7 行，我们创建了一个 C 共享库的句柄。在第 12 行，我们声明了 C `cadd`函数的返回类型。这是至关重要的；我们需要让`ctypes`知道如何封送对象来传递它们，以及什么类型可以正确地解封它们。

第 14 行的`y`变量也是这种情况。我们需要声明这是类型`float`。最后，我们可以让`x`保持原样，因为默认情况下，`ctypes`认为一切都是整数。

我们可以像执行任何其他 Python 脚本一样执行这个脚本:

```
python3 padd.py
```

结果是一种神奇！

```
In cadd: int 6 float 2.3 returning  8.3
In Python: int: 6 float 2.3 return val 8.3
```

恭喜你！你从 Python 中调用了一个 C 库的函数！

# 结论

Python 是一种用户友好、易于学习、免费使用、可移植且易于扩展的编程语言。

然而，它也有弱点，其中最突出的是速度。为了解决这个问题，我们可以用另一种语言(例如，C 或 C++)编写要求苛刻的函数，并利用特定的绑定从 Python 调用这些函数。

在本文中，我们使用了`ctypes`，这是一个 Python 库。我们能够从 Python 代码中调用 C 库并返回结果。我知道这个功能没有做任何要求，但这只是一个演示。看看后面的文章能不能做点更有挑战性的！

# 关于作者

我的名字是 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=python-bindings) ，我是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请在 Twitter 上关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。