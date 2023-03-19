# Python 中的高级多任务处理:用 6 行代码应用线程池和进程池并进行基准测试

> 原文：<https://towardsdatascience.com/advanced-multi-tasking-in-python-applying-and-benchmarking-threadpools-and-processpools-90452e0f7d40?source=collection_archive---------9----------------------->

## 安全轻松地对您的代码应用多任务处理

![](img/062a14d3a4a54f4d0f3223c770bc35fe.png)

我们的一名工作人员正在执行一项重要任务(图片由 [Unsplash](https://unsplash.com/photos/c3RWaj8L3M8) 上的 [Krysztof Niewolny](https://unsplash.com/@epan5) 拍摄)

当你的机器可以执行多任务时，为什么要按顺序执行呢？使用进程的线程，你可以通过同时运行来大大提高代码的速度。本文将向您展示用 Python 实现这一奇妙技术的安全而简单的方法。在本文结束时，您将:

*   了解哪些任务适合多任务处理
*   知道何时应用线程池或进程池
*   能够向同事和朋友吹嘘只用几行简单的代码就可以将执行速度提高 10 倍

在我们开始之前，我强烈建议先看一看 [**这篇文章**](https://mikehuls.medium.com/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e) 来理解 Python 是如何工作的。为什么 Python 一开始就不是多线程的？它向您展示了我们在本文中试图解决的问题。我还推荐阅读这篇解释线程和进程区别的文章。

# 为什么要用泳池？

池是希望保护员工数量的应用程序的理想选择。假设您在一个 API 中运行一个函数，该函数创建 5 个 workers 来处理提供的数据。如果您的 API 在一秒钟内突然收到 500 个请求，该怎么办？创建 2500 个都执行繁重任务的工人可能会杀死你的计算机。

池通过限制可以创建的工作线程的数量来防止您的计算机像这样被终止。在 API 示例中，您可能希望创建一个最多包含 50 个工作线程的池。当 500 个请求进来时会发生什么？只有 50 个工人被创造出来。还记得每个请求需要 5 个工人吗？这意味着只有前 10 个请求得到处理。一旦工人完成并返回到池中，它可以再次被发送出去。

总之:池确保在任何给定的时间都不会有超过一定数量的工作线程处于活动状态。

# 创建池

使用并发库，我们可以轻松地为线程和进程创建池。在下面的部分，我们将进入代码。最后，我们会有一个例子:

*   如何创建池
*   限制池的最大工作线程数
*   将目标函数映射到池中，以便工作人员可以执行它
*   收集函数的结果
*   等待所有工人完成后再继续

## 设置

正如您在 [**这篇文章**](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e) 中看到的，线程更适合 IO 类型的任务(并发等待)，而进程最适合 CPU 密集型任务(使用更多 CPU)。为了正确测试我们的代码，我们将定义两种类型的函数:一种是 IO 密集型的，另一种是 CPU 密集型的:

这些是我们的目标函数；承担重任的职能。注意，两者都有一个打印输出参数。这不是特别有用的
，但是我们需要它来证明我们可以传递额外的(关键字)参数。

![](img/5bf0431d11de2022023ba38a2a53fa2a.png)

每个蜂巢都有一个固定数量的工蜂池(图片由[汉斯约格·凯勒](https://unsplash.com/@kel_foto)在 [Unsplash](https://unsplash.com/photos/OJHxRwXWXBs) 上拍摄)

## 线程池 IO 繁重任务

我们将从 I/O-heavy 函数开始。我们有一个 100 个数字的列表，我们想传递给这个函数。按顺序，代码如下所示:

如果我们执行上面的代码，大约需要 **100 秒**来执行(100 个调用，每 2 秒一次)。让我们使用线程池来运行相同的代码:

如你所见，我们使用了 concurrent . futures . threadpoolexecutor。我们定义最多 10 个工人，然后在我们的范围内循环，为每个数字创建一个线程。每个线程一完成，我们就将结果添加到 _sum 变量中，并最终返回它。执行这段代码大约需要 10 秒钟。这并不奇怪，因为我们有 10 个工人，所以我们应该比顺序运行代码快 10 倍。

下面你会发现同样的代码只是格式不同。

我们将把对函数的调用定义为部分函数，这样我们就可以把它映射到执行器。第三行对范围(10)中的每个值执行部分函数。

## CPU 繁重的任务

当谈到将 CPU 繁重功能映射到进程池的代码时，我们可以简单地说:代码非常相似。

只需将第 3 行中的函数换成(cpu_heavy_task)并将 ThreadPoolExecutor 切换成 ProcessPoolExecutor。就是这样！

![](img/74cf51933d209cfa143268be559ab94d.png)

让我们来看看顺序、线程和进程之间的区别(图片由 [Glen Rushton](https://unsplash.com/@glen_rushton) 在 [Unsplash](https://unsplash.com/photos/Uwl7TRuTZqE) 上提供)

# 标杆管理

让我们来测试一下这些功能吧！我们将顺序执行 IO 密集型和 CPU 密集型函数，线程化和多处理。
以下是结果:

```
IO heavy function
sequential:      took 100.44 seconds
threaded:        took 10.04  seconds (max pool size = 10)
processed:       took 10.20  seconds (max pool size = 10)

CPU heavy function 
sequential:      took 27.89  seconds
threaded:        took 26.65  seconds (max pool size = 10)
processed:       took 6.58   seconds (max pool size = 10)
```

正如[**这篇**](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e) 中所解释的，这些结果都是意料之中的。顺序执行当然是最慢的，一个接一个地执行所有的函数。

![](img/c90d29f9d8a2fe22d20005c7b50ecae1.png)

不仅仅是一辆车，还有游泳池，我们有整个车队可以使用(图片由乔治·贝尔在 Unsplash 上拍摄)

*线程*IO 繁重功能的速度快了 10 倍，因为我们的工作人员是它的 10 倍。*处理*IO 密集型函数大约与 10 个线程一样快。它会慢一点，因为设置这些流程的成本更高。注意，虽然两者速度一样快，但是线程是更好的选择，因为它们提供了共享资源的能力。

当我们对 CPU 密集型函数进行基准测试时，我们看到线程化与顺序方法一样快。这是由于 GIL 在本文 **中解释了 [**。**进程在处理 CPU 密集型任务时效率更高，速度提高了约 4.3 倍。](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e)**

另外，请注意，在这些情况下，我们的池大小相当小，并且可以调整(取决于您的目标函数)以创建更快的程序！

# 结论

正如您在本文中读到的，使用池非常容易。此外，我希望已经阐明了为什么以及如何使用它们。有时候多任务处理是不够的。查看 [**这篇**](https://mikehuls.medium.com/getting-started-with-cython-how-to-perform-1-7-billion-calculations-per-second-in-python-b83374cfcf77) 或 [**这篇**](https://mikehuls.medium.com/write-your-own-c-extension-to-speed-up-python-x100-626bb9d166e7) 的文章，这篇文章向您展示了如何编译一小部分代码以获得 100 倍的速度提升(也可以是多任务的)。

如果你有建议/澄清，请评论，以便我可以改进这篇文章。与此同时，请查看我的[关于各种编程相关主题的其他文章](https://mikehuls.medium.com/)，例如:

*   [Python 中的多任务处理:通过同时执行，将程序速度提高 10 倍](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e)
*   [用 FastAPI 用 5 行代码创建一个快速自动记录、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)
*   [从 Python 到 SQL——安全、轻松、快速地升级](https://mikehuls.medium.com/python-to-sql-upsert-safely-easily-and-fast-17a854d4ec5a)
*   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)
*   [创建您的定制私有 Python 包，您可以从您的 Git 库 PIP 安装该包](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893)
*   [面向绝对初学者的虚拟环境——什么是虚拟环境以及如何创建虚拟环境(+示例)](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)
*   [通过简单升级，显著提高数据库插入速度](https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424)

编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟着我！](https://mikehuls.medium.com/membership)

<https://mikehuls.medium.com/membership> 