# Python 中的并发和并行

> 原文：<https://towardsdatascience.com/concurrency-and-parallelism-in-python-bbd7af8c6625?source=collection_archive---------3----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

## Python 中可用方法的简明概述

![](img/d4246c039b5c2109517fed74fd420b8c.png)

照片由[在](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/computing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

如果你即将开始一个大数据项目，你将要么检索大量信息，要么在你的机器上处理大量数据，或者两者兼而有之。然而，如果代码是顺序的或同步的，你的应用程序可能会开始挣扎。

让我们看看在每种情况下，哪些概念和 Python 库可以提高应用程序的性能。

## 什么是并发和并行，它们解决什么问题

有两个方面可以提高程序的速度——I/O 和 CPU 消耗。例如，如果您的代码需要通过网络进行大量的文件访问或通信，那么它就是 I/O 受限的。CPU 绑定的代码涉及大量计算。例如，训练统计模型绝对是一项计算密集型工作。

这两种类型的工作在所需资源方面有什么不同？

当 I/O 绑定的代码发送多个请求时，它并没有真正利用机器的 CPU 内核，因为本质上，它是在空闲地等待响应。因此，这样的应用程序不能通过增加更多的计算能力来提高性能。它更多的是关于请求和响应之间的等待时间。

对于 CPU 绑定的代码片段来说，情况正好相反。

缓解任何一种瓶颈的两种机制分别是**并发**和**并行**。

通常，并发被认为是比并行更大的概念。简单来说就是*同时做多件事*。在实践中，有一个特殊的角度来区分这两种思想，尤其是在 Python 中。并发通常被理解为同时“管理”多个作业。实际上，这些作业并不会同时执行。它们巧妙地交替出现。

然而，并行执行意味着同时执行多个任务，或者并行执行*。并行性允许在一台机器上利用多个内核。*

## *三个用于并发和并行的 Python 库*

*在 Python 中，并发由`[threading](https://docs.python.org/3/library/threading.html)`和`[asyncio](https://docs.python.org/3/library/asyncio.html)`表示，而并行是通过 `[multiprocessing](https://docs.python.org/3/library/multiprocessing.html)`实现的。*

## *穿线*

*使用`threading`，您可以创建多个*线程*，您可以在这些线程之间分配一些 I/O 相关的工作负载。例如，如果您有一个简单的函数来下载一些文件`download_file(f)`，您可以使用`[ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor)`来启动线程，然后使用`map`从文件列表中调用每个参数文件:*

```
*with ThreadPoolExecutor() as executor:
    executor.map(download_file, files)*
```

*这里值得一提的是，Python 中的线程工作方式与 Java 等其他语言不太一样——CPython 的全局解释器锁(GIL)实际上确保了内存使用是*线程安全的*，因此一次只能处理一个线程(更多信息请参见`[threading](https://docs.python.org/3/library/threading.html)`文档)。因此，它实际上是一种如上定义的并发机制。*

## *阿辛西奥*

*使用`asyncio`，您可以为类似的目的创建*任务*:*

```
*tasks = [asyncio.create_task(download_file(f)) for f in files]*
```

*但是任务背后的想法不同于线程。事实上，任务运行在单线程上。然而，如果第一个任务正在等待它的响应而不是阻塞它，每个任务都允许操作系统运行另一个任务。这就是*异步 IO* 的本质。(在后面的文章中对异步程序进行了更全面的介绍)。*

*大多数情况下，您还会想要创建一个特殊的*事件循环*对象来管理主函数中的任务。*

## *线程与异步*

*对于 I/O 绑定的程序来说,`asyncio`模块可以显著提高性能，因为创建和管理任务的开销比线程少。*

*线程化方法可以被认为更危险，因为线程之间的切换可以在任何时候发生，甚至在语句执行的中途，这是由于*抢先多任务*，而`asyncio`任务在准备切换时会发出信号——这种机制被称为*协作多任务*。如果此时出现问题，那么使用线程方法来跟踪它会更加困难。*

*然而，使用`asyncio`模块需要编写大量代码来适应它。*

## *多重处理*

*以上两种方法对于加速 I/O 相关的程序都很有效。至于 CPU 受限的程序，多重处理将真正有所帮助。*

*`multiprocessing`模块为每个*进程创建一个 Python 解释器。*它实际上利用了您机器上的 CPU 内核数量。一个典型的 CPU 相关代码的例子是压缩文件。因此，如果您有一个函数`compress_file(f)`，启动新进程并在它们之间分配工作负载的语法将类似于线程示例:*

```
*with ProcessPoolExecutor() as executor:
    executor.map(compress_file, files)*
```

*如果多处理这么神奇，为什么不一直用呢？*

*用`multiprocessing`写代码有几件棘手的事情。首先，您需要能够确定某些数据实际上是否需要被所有进程访问——因为进程之间的内存不是共享的。此外，有时很难确定程序的哪些部分可以清晰地划分为独立的进程。*

*最后，您应该仔细评估`multiprocessing`带来的性能提升和成本之间的权衡。如果计算实际上不是那么密集的话，`multiprocessing`可能不会加速那么多，因为为每个进程增加解释器会带来很大的开销。*

*你对这些模块的实验结果如何？对他们在不同环境中的行为有什么有趣的观察吗？*

*如果你对关于 Python 的文章感兴趣，可以随意查看我关于 Python 3.9 中的[新特性](/dictionary-union-operators-and-new-string-methods-in-python-3-9-4688b261a417)和 [Python 代码优化](/optimizing-your-python-code-156d4b8f4a29)的帖子！*