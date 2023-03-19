# 如何将 Python 数据管道加速到 91X？

> 原文：<https://towardsdatascience.com/how-to-speed-up-python-data-pipelines-up-to-91x-80d7accfe7ec?source=collection_archive---------8----------------------->

## 一个 5 分钟的教程可以为您的大数据项目节省数月时间。

![](img/4140b6e2fad570cf72afced6e475fa8e.png)

照片由 [Vicky Yu](https://unsplash.com/@vicky_yu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为一名数据科学家，令人沮丧的事情是等待大数据管道完工。

尽管 python 是数据科学家的浪漫语言，但它不是最快的。这种脚本语言在执行时被解释，使得它很慢，并行执行很困难。可悲的是，并不是每个数据科学家都是 C++专家。

如果有一种并行执行，以编译代码的速度运行 python 代码的方法会怎样？这就是 Tuplex 正在解决的问题。

Tuplex 是用 Python 编写的并行大数据处理框架。如果您曾经在 Apache Spark 中工作过，这可能对您来说很熟悉。然而，与 spark 不同，Tuplex 不调用 Python 解释器。它优化管道并将其转换为 LLVM 字节码，以极快的速度运行，与手工优化的 C++代码一样快。

Python 使用多重处理库来并行执行。这个库的缺点是它不能在任何 REPL 环境下工作。然而，我们数据科学家喜欢 Jupyter 笔记本。在幕后，多重处理甚至不是一种并行执行技术。它只启动多个子进程，操作系统负责它的并行执行。事实上，不能保证操作系统会并行运行它们。

在本文中，我们将讨论:

*   如何安装 Tuplex
*   如何运行琐碎的数据管道；
*   Tuplex 中便捷的异常处理:
*   高级配置如何帮助您，以及；
*   将它与普通的 python 代码进行对比。

我确信这将是一次公园散步。

# 启动并运行 Tuplex。

尽管它很有用，但 Tuplex 的设置非常简单。PyPI 做到了。

```
pip install tuplex
```

虽然在 Linux 上推荐使用这种方法，但是在 Mac 上可能必须使用 docker 容器。

这里有一点需要注意的是，它还没有在 Windows 电脑上测试过。至少 [Tuplex 的文档](https://tuplex.cs.brown.edu/gettingstarted.html)没有提到。请分享你的 Windows 电脑体验。

# 你的第一条数据管道。

一旦安装了 Tuplex，运行并行任务就很容易了。这是来自 Tuplex 官方文档页面的例子。

来自 [Tuplex 文档](https://tuplex.cs.brown.edu/gettingstarted.html)的片段。

首先，您必须创建一个复合上下文。您可以通过从 Tuplex 模块导入它来做到这一点。

从这里开始，运行并行函数执行只需要三个步骤；并行化、映射和收集。

Tuplex 上下文对象的并行化方法是您的起点。它将输入值列表作为参数传递给函数。该列表中的每个元素都将通过函数与其他元素并行运行。

您可以传递一个用户定义的函数，该函数使用 map 函数转换每个输入。最后，使用 collect 方法收集所有并行执行的输出。

# Tuplex 中方便的异常处理。

我最喜欢 Tuplex 的一点是它在管理异常方面的便利性。数据管道中的错误处理是一种令人畏惧的体验。想象一下，花几个小时处理一个数据流，却发现一个被零除的细微错误扼杀了你所做的一切。

来自 [Tuplex 文档](https://tuplex.cs.brown.edu/gettingstarted.html)的片段。

上面的代码会产生一个被零除的错误。如果您使用 spark 或任何标准 python 模块来处理这个问题，至少情况是这样的。

在 Tuplex 中，错误处理是自动的。它会忽略有错误的那个并返回其余的。上面的代码将返回[2，-4]，因为列表中的第一个和第三个输入无法执行。

但是，忽略错误有时是有问题的。通常你必须以不同的方式处理它们，而 Tuplex 的 API 足够灵活，可以做到这一点。事实上，Tuplex 方法很方便。

来自 [Tuplex 文档](https://tuplex.cs.brown.edu/gettingstarted.html)的片段。

Tuplex 使错误处理变得毫不费力。您必须在“map”和“collect”方法之间链接一个“resolve”方法。在上面的例子中，我们已经传入了 ZeroDivisionError 类型，并通过替换零来处理它。

resolve 方法的第二个参数是一个函数。有了这个函数，您可以告诉 Tuplex 在出现这种类型的错误时应该做什么。

# 为高级用例配置 Tuplex。

您可以用两种方式配置 Tuplex。第一种是直截了当的解决方案；只需将字典传递给上下文初始化。下面是一个将执行内存设置为较高值的示例。

来自 [Tuplex 文档](https://tuplex.cs.brown.edu/gettingstarted.html)的片段。

Tuplex 还支持在 YAML 文件中传递配置。在生产环境中，您可能必须将配置存储在文件中。YAML 文件是处理不同配置并在开发和测试团队之间传递的一种很好的方式。

来自 [Tuplex 文档](https://tuplex.cs.brown.edu/gettingstarted.html)的片段。

下面是一个配置文件的例子，它包含了您可以从 Tuplex 文档中进行的所有不同的定制。

来自 [Tuplex 文档](https://tuplex.cs.brown.edu/gettingstarted.html)的片段。

# 性能基准

Tuplex 的承诺耐人寻味。是时候看看它的性能提升了。

在这次基准测试中，我使用了这个简单的质数计数器函数。我首先使用 for 循环运行这个函数，然后使用 python 内置的多处理模块，最后使用 Tuplex。

作者的片段。

## 用标准 Python 执行密集型任务。

在 for 循环中运行函数是最简单的。我使用 Jupyter 笔记本中的“%%time”助手来跟踪执行时间。

由[作者](https://thuwarakesh.medium.com/)摘录。

多次运行上述代码平均需要 51.2 秒才能完成。

在 for 循环执行中，执行速度会很慢。但是让我们用 python 内置的多处理模块来做同样的尝试。下面的代码不能在 REPL 的 like Jupyter 笔记本上运行。你必须把它放在一个. py 文件中，然后[在命令行](https://realpython.com/run-python-scripts/)中执行它。

由[作者](https://thuwarakesh.medium.com/)摘录。

运行这个多处理脚本的平均时间为 30.76 秒。与 for-loop 方式相比减少了 20.44 秒。

## 并行处理密集型任务。

最后，我们执行相同的素数计数器函数，这次是用 Tuplex。下面这段简洁的代码平均花费了 0.000040 秒，并产生了相同的结果。

由[作者](https://thuwarakesh.medium.com/)摘录。

与其他标准 python 方式相比，Tuplex 的性能提升非常显著。这个小例子的执行时间比多处理短 769k 倍，比普通 for 循环快 1280k 倍。

我们会…让我们坚持 Tuplex 团队的 5–91X 承诺。然而，Tuplex 敦促我在编写另一个 for 循环之前要三思。

# 结论

Tuplex 是一个易于安装的 python 包，可以为您节省大量时间。它通过将数据转换成字节码并并行执行来加速数据管道。

性能基准测试表明，它对代码执行的改进是深远的。然而，它的设置非常简单，语法和配置非常灵活。

Tuplex 最酷的部分是它方便的异常处理。数据管道中的错误处理从未如此简单。它与交互式 shells 和 Jupiter 笔记本集成得很好。编译语言通常不是这种情况。甚至 python 本身也不能像 Jupyter notebook 那样在 REPL 内部处理并行处理。

Tuplex 在提升 Python 性能方面取得了显著的成功。但是与 Python 传统的高性能计算方法相比，它的性能如何呢？这里有一篇文章将它与 Cython 进行了对比。

[](/challenging-cython-the-python-module-for-high-performance-computing-2e0f874311c0) [## 挑战 cy thon——高性能计算的 Python 模块。

### 现代的替代方案看起来很有希望，Python 可以以闪电般的速度运行。

towardsdatascience.com](/challenging-cython-the-python-module-for-high-performance-computing-2e0f874311c0) 

> 谢谢你的阅读，朋友。看来你和我有许多共同的兴趣。一定要看看我的个人博客。
> 
> ***向我问好*** 上[LinkedIn](https://www.linkedin.com/in/thuwarakesh/)[Twitter](https://twitter.com/Thuwarakesh)[中](https://thuwarakesh.medium.com/subscribe)。我会为你打破僵局。

还不是中等会员？请使用此链接 [**成为**](https://thuwarakesh.medium.com/membership) 会员。你可以享受成千上万的有见地的文章，并支持我，因为我赚了一点佣金介绍你。