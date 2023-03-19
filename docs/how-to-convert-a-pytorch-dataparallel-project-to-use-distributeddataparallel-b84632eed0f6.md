# 如何将 PyTorch DataParallel 项目转换为使用 DistributedDataParallel

> 原文：<https://towardsdatascience.com/how-to-convert-a-pytorch-dataparallel-project-to-use-distributeddataparallel-b84632eed0f6?source=collection_archive---------18----------------------->

![](img/f53a214dbcbfaafbb6c9e7e4f0ffb669.png)

泰勒·维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

许多帖子讨论 PyTorch [DataParallel](https://pytorch.org/docs/stable/generated/torch.nn.DataParallel.html#dataparallel) 和 [DistributedDataParallel](https://arxiv.org/pdf/2006.15704.pdf) 之间的区别，以及为什么使用 DistributedDataParallel 是[最佳实践](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html#comparison-between-dataparallel-and-distributeddataparallel)。

PyTorch 文档对此进行了总结:

> 由于线程间的 GIL 争用、每次迭代的复制模型以及分散输入和收集输出带来的额外开销，即使在单台机器上，DataParallel 通常也比 DistributedDataParallel 慢

但事实是，对于开发和原型设计，当处理一个新项目时，或者当在现有的 GitHub 存储库(已经使用了 DataParallel)之上构建时，使用现成的 DataParallel 版本更简单，特别是当使用一个服务器(有一个或几个 GPU)时。最突出的优点是用 DataParallel 更容易调试。

尽管如此，总有一天，您会希望将现有的 DataParallel 项目转换为大联盟，并使用分布式 DataParallel 这显然不是小事，因为它应该是。

![](img/eeaaa955ea313824d385f06bd0a77dba.png)

所以在这篇文章中，我不会进一步讨论每种方法的优缺点，而是集中在将一个用 DataParallel 实现的现有项目转换成一个分布式 DataParallel 项目的实际方面。

我将尽可能概括地描述不同的部分，但是当然，您的用例是独特的😃并且可能需要根据您的实现和代码进行仔细和具体的调整。

我们走吧…

第一步是包装。DataParallel 使用带有多线程的单进程，但是 DistributedDataParallel 在设计上是多进程的，所以我们要做的第一件事就是使用多进程包装器包装整个代码——我们的主要功能。

为此，我们将使用由 FAIR 在 [Detectron2](https://github.com/facebookresearch/detectron2) 存储库中提供的[包装器](https://github.com/facebookresearch/detectron2/blob/9246ebc3af1c023cfbdae77e5d976edbcf9a2933/detectron2/engine/launch.py)。我们还需要 [comm](https://github.com/facebookresearch/detectron2/blob/master/detectron2/utils/comm.py) 文件，它为处理分发资源提供了一些不错的功能。

从 Detectron2 GitHub 库复制的包装代码

我们需要运行的函数叫做`launch()`。让我们回顾一下输入参数:

我们需要向它提供我们正在使用的机器的数量，对于这个帖子，我们将使用一台带有四个 GPU 的机器，所以`num_machines=1`和`num_gpus_per_machine=4`。使用多台机器时，`machine_rank`用于指定机器的编号[或索引],例如，在分布式培训中使用多台机器时，`machine_rank=0.`T5 用于提供主机 IP 地址，但由于我们只使用一台机器，因此我们可以使用`dist_url=“auto”`,它将使用本地主机作为 IP 和自由端口。`main_func`变量提供了我们代码的主要功能，这应该是到目前为止在 DataParallel 情况下运行您的流所使用的主要函数。最后，我们可以提供`args`变量，这些都是我们的`main_func`函数的输入参数。

在引擎盖下，launch 函数将使用 torch 多处理模块[torch.multiprocessing]和 spawn 函数旋转多个进程，这将启动作为新的子进程的进程-每个 GPU 一个。

值得注意的是，在运行启动函数后，完全相同的代码将同时在每个子进程中运行。因此，所有只需要在开始时运行一次的预设置代码，都应该在运行启动函数之前调用。例如，连接到您的实验管理器应用程序，从应该预先传递给所有进程的配置文件中获取参数，将数据同步到本地服务器，等等。

现在我们有了一个工作的包装器，我们可以调整我们的模型了。我们需要将在 CPU 上初始化的模型“转换”为分布式数据并行 GPU 模型。

如果在数据并行的情况下，我们需要用以下内容包装我们的模型:

`model = torch.nn.DataParallel(model)`

在分布式的情况下，我们将使用 DistributedDataParallel 类包装模型，这个调用将由每个进程独立完成。所以，我们首先需要弄清楚我们当前进程的“等级”是什么[等级基本上是我们的 GPU 索引]，将模型从 CPU 复制到那个特定的 GPU，并将其设置为 DistributedDataParallel 模型:

`broadcast_buffers`是一个重要的参数，它意味着在训练过程中是否要同步 GPU 之间基于统计的变量，例如 BatchNorm 层的均值和方差。

我们有了主包装器和模型，接下来我们需要调整数据加载部分。

在数据并行的情况下，我们有一个大小为 N 的单批，在向前传递期间，它会自动均匀地分布在我们的 4 个 GPU 上，为每个 GPU 提供大小为 N/4 的小批。现在，在分布式情况下，我们需要每个 GPU 进程从完整的样本批次中只读取与其小批次相关的样本。这是通过 PyTorch [分布式取样器](https://pytorch.org/docs/stable/data.html#torch.utils.data.distributed.DistributedSampler)完成的:

采样器根据我们拥有的流程数量分割样本，并为每个流程提供其迷你批次的相关样本索引。初始化采样器后，我们需要为 DataLoader 类提供这个采样器实例，并将其 shuffle 参数设置为 False。我们还需要调用采样器的`set_epoch(epoch)` [函数](https://pytorch.org/docs/stable/data.html#torch.utils.data.distributed.DistributedSampler)作为我们的历元 for-loop 的一部分，以在每个历元中获得不同顺序的训练样本。

至此，我们基本上拥有了三个主要组件:多进程包装器、定义为分布式类型的模型和处理数据加载部分的方法。接下来，我们将回顾我们可以在代码中进行的与日志记录、报告和度量计算相关的其他重要调整。

由于所有进程都运行相同的代码，使用打印和记录器向控制台(或文件)报告将会重复导致写入相同的打印。虽然有些消息可能很重要，并且与每个进程的报告相关，但是许多消息并不重要，我们可以使用下面的命令在主进程中只报告一次消息[这是我们的 0 级进程]:

```
if comm.is_main_process():
    print(“something that is printed only once”)
```

当向外部工具报告指标或者保存模型权重时，这个技巧也很有价值。您可以使用它来确保一个动作在主流程的流程中只发生一次。

那么，指标呢？

使用 PyTorch DistributedDataParallel 模块，您不需要管理和“收集”[收集]所有进程的损失值来运行后退步骤，`loss.backward()`将在幕后为您完成，并且由于它为每个进程运行，它将为所有 GPU 上的所有模型复制提供相同的梯度校正。

但是，这仅与反向过程中使用的损失值相关。为了报告实际损失值或它们的平均值，您需要收集每个进程的损失，并让主进程报告它们的平均值:

```
loss = criterion(outputs, labels)
...
all_loss = comm.gather(loss)
if comm.is_main_process():
    report_avg(all_loss)
```

现在，`all_loss`有了所有进程的损耗值，主进程可以对它们进行平均，累加，并报告需要什么。

请注意，`gather`命令要求所有进程向主进程提供它们的值，从而降低了整个流程的速度。处理这个问题的一个更好的方法是让每个进程收集并存储它的损失值，并在每次迭代或在一个时期结束时运行收集。

其他重要评论:

*   一个有价值的技巧是能够在代码中的特定点同步所有进程，并强制它们相互等待。这可以解决以下情况:流运行，但由于某种原因挂起。查看`nvidia-smi`,您会发现所有 GPU 的利用率都为 100%,而一个 GPU 的利用率为 0%。这是一个令人沮丧的例子，发生在使用分布式模式时，可能是因为主 GPU 正在处理一些需要更多时间的事情，如保存和上传您的模型。在流程中的某些特定点强制过程同步可能会有所帮助，例如，当从评估步骤回到下一个训练时期时。这可以通过使用下面的命令用一个`barrier`函数来完成:`comm.synchronize()`
    这个函数在代码中放置一个屏障，强制所有进程等待其余进程到达并一起通过那个点😃。
*   DistributedDataParallel 模块在进程之间传输信息，为此，PyTorch 会序列化属于数据加载器类的变量。这要求此类变量对序列化有效。这个错误提供了相当多的信息，所以当你面临这样的问题时你会知道。我在序列化部分日志实例时遇到了问题，因为它有一个过滤器使用了一个不适合序列化的不同类。

就是这样。我希望本指南将帮助您从 PyTorch 数据并行实现过渡到分布式数据并行机制，并享受它提供的好处和速度。

延伸阅读:

*   [https://pytorch.org/tutorials/beginner/dist_overview.html](https://pytorch.org/tutorials/beginner/dist_overview.html)
*   [https://py torch . org/tutorials/intermediate/DDP _ tutorial . html # comparison-between-data parallel-and-distributed data parallel](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html#comparison-between-dataparallel-and-distributeddataparallel)
*   PyTorch 分发:加速数据并行训练的经验[【https://arxiv.org/pdf/2006.15704.pdf】T4