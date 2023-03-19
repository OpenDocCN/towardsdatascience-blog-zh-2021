# 如果你能写函数，你可以用 Dask

> 原文：<https://towardsdatascience.com/if-you-can-write-functions-you-can-use-dask-bbb6d8b3a248?source=collection_archive---------30----------------------->

## 立即加速你的代码，而不用花大量的时间学习新的东西。

![](img/9f53ce7abd1d78597418929a0e2f14f1.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

*本文是关于实践中使用 Dask 的系列文章的第二篇。本系列的每一篇文章对初学者来说都足够简单，但是为实际工作提供了有用的提示。该系列的第一篇文章是关于使用*[*local cluster*](https://saturncloud.io/blog/local-cluster/)*。本系列的下一篇文章是关于使用*[*Dask data frame*](https://saturncloud.io/blog/dask-is-not-pandas/)*。*

在 Saturn Cloud，[我们管理着一个数据科学平台](https://www.saturncloud.io)，该平台提供 Jupyter 笔记本、Dask 集群以及部署模型、仪表盘和作业的方法。我们已经看到一些客户在完全不必要的情况下开始使用多节点集群。当您第一次使用 Dask 时，应该选择 LocalCluster。

我一直在和许多听说过 Dask(用于分布式计算的 Python 框架)的数据科学家聊天，但是不知道从哪里开始。他们知道，Dask 可能会通过让许多工作流在一个机器集群上并行运行来加快它们的速度，但学习一种全新的方法似乎是一项艰巨的任务。但是我在这里告诉您，您可以开始从 Dask 获得价值，而不必学习整个框架。如果你花时间等待笔记本单元执行，Dask 很有可能会帮你节省时间。如果您仅仅知道如何编写 Python 函数，那么您可以利用这一点，而无需学习其他任何东西！这篇博文是一篇“不学全如何使用 Dask”教程。

# Dask，dataframes，bags，arrays，schedulers，workers，graphs，RAPIDS，哦不！

关于 Dask 有很多复杂的内容，让人不知所措。这是因为 Dask 可以利用一个工人机器集群来做很多很酷的事情！但是现在忘掉这一切吧。这篇文章关注的是可以节省你时间的简单技巧，而不需要改变你的工作方式。

# 对于循环和函数

几乎每个数据科学家都做过类似的事情，将一组数据帧存储在单独的文件中，使用 for 循环读取所有数据帧，执行一些逻辑操作，然后将它们组合起来:

```
results **=** **[]**
**for** file **in** files**:**
    defer **=** pd**.**read_csv**(**file**)** *## begin genius algorithm*

    brilliant_features **=** **[]**
    **for** feature **in** features**:**
        brilliant_features**.**append**(**compute_brilliant_feature**(**df**,** feature**))**
    magical_business_insight **=** make_the_magic**(**brilliant_features**)**

    results**.**append**(**magical_business_insight**)**
```

随着时间的推移，你会得到更多的文件，或者`genius_algorithm`变得更复杂，运行时间更长。你最终会等待。还有等待。

![](img/f9414c8757b25d4d9a22df74112ecce6.png)

Aleksandra Sapozhnikova 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**第一步**就是把你的代码封装在一个函数里。你想封装进入 for 循环的东西。这更容易理解代码在做什么(通过魔法将文件转换成有用的东西)。更重要的是，除了 for 循环之外，它还使代码的使用变得更加容易。

```
**def** make_all_the_magics**(**file**):**
    df **=** pd**.**read_csv**(**file**)**
    brilliant_features **=** **[]**
    **for** feature **in** features**:**
        brilliant_features**.**append**(**compute_brilliant_feature**(**df**,** feature**))**
    magical_business_insight **=** make_the_magic**(**brilliant_features**)**
    **return** magical_business_insight

results **=** **[]**

**for** file **in** files**:**
    magical_business_insight **=** make_all_the_magics**(**file**)**
    results**.**append**(**magical_business_insight**)**
```

**第二步**用 Dask 将其并行化。现在，Dask 将在集群上并行运行它们，而不是使用 for 循环，每次迭代都在前一次迭代之后进行。这将给我们带来更快的结果，并且只比 for 循环代码多了三行。

```
**from** dask **import** delayed
**from** dask.distributed **import** Client

*# same function but with a Dask delayed decorator*
**@delayed**
**def** make_all_the_magics**(**file**):**
    df **=** pd**.**read_csv**(**file**)**
    brilliant_features **=** **[]**
    **for** feature **in** features**:**
        brilliant_features**.**append**(**compute_brilliant_feature**(**df**,** feature**))**
    magical_business_insight **=** make_the_magic**(**brilliant_features**)**
    **return** magical_business_insight

results **=** **[]**
**for** file **in** files**:**
    magical_business_insight **=** make_all_the_magics**(**file**)**
    results**.**append**(**magical_business_insight**)**

*# new Dask code*
c **=** Client**()**
results **=** c**.**compute**(**results**,** sync**=**True**)**
```

工作原理:

*   延迟装饰器，转换你的函数。当你调用它的时候，它不会被求值。相反，您得到的是一个`delayed`对象，Dask 可以稍后执行。
*   `Client().compute`将所有被延迟的对象发送到 Dask 集群，在那里它们被并行计算！就这样，你赢了！
*   实例化一个`Client`会自动提供一个`LocalCluster`。这意味着 Dask 并行工作进程与调用 Dask 的进程在同一台机器上。这是一个简洁的例子。对于实际工作，我建议在终端中创建[本地集群。](https://saturncloud.io/blog/local-cluster)

# 实用话题

以上止于大多数 Dask 教程止于的地方。我在自己的工作中和众多客户中使用过这种方法，总会出现一些实际问题。接下来的这些技巧将帮助你从上面的教科书例子过渡到实践中更有用的方法，它们涵盖了经常出现的两个主题:大型对象和错误处理。

# 大型物体

为了在分布式集群上计算函数，需要将调用函数的对象发送给工作者。这可能会导致性能问题，因为这些需要在您的计算机上序列化(pickled ),并通过网络发送。假设您正在处理千兆字节的数据——您不希望每次在其上运行函数时都必须传输这些数据。如果您不小心发送了大型对象，您可能会看到来自 Dask 的如下消息:

```
Consider scattering large objects ahead of time with client.scatter to reduce scheduler burden and keep data on workers
```

有两种方法可以避免这种情况发生，你可以将较小的对象发送给工人，这样负担就不会太重，或者你可以尝试将每个对象只发送给工人一次，这样你就不必一直进行传输。

## 修复 1:尽可能发送小对象

这个例子很好，因为我们发送的是文件路径(小字符串)，而不是数据帧。

```
*# good, do this*
results **=** **[]**
**for** file **in** files**:**
    magical_business_insight **=** make_all_the_magics**(**file**)**
    results**.**append**(**magical_business_insight**)**
```

下面是*不*要做的事情。这不仅是因为您将在循环中进行 CSV 读取(昂贵且缓慢)，这不是并行的，还因为现在我们正在发送数据帧(可能很大)。

```
*# bad, do not do this*
results **=** **[]**
**for** file **in** files**:**
    df **=** pd**.**read_csv**(**file**)**
    magical_business_insight **=** make_all_the_magics**(**df**)**
    results**.**append**(**magical_business_insight**)**
```

通常情况下，可以重写代码来改变数据的管理位置——要么在客户机上，要么在工人上。根据您的情况，通过考虑哪些函数作为输入以及如何最小化数据传输，可能会节省大量时间。

## 修复 2:只发送一次对象

如果一定要发一个大的对象，就不要多次发了。例如，如果我需要发送一个大的模型对象来进行计算，简单地添加参数将会多次序列化模型(每个文件一次)

```
*# bad, do not do this*
results **=** **[]**
**for** file **in** files**:**
    *# big model has to be sent to a worker each time the function is called*
    magical_business_insight **=** make_all_the_magics**(**file**,** big_model**)**
    results**.**append**(**magical_business_insight**)**
```

我可以告诉 Dask 不要这样做，把它包装在一个延迟对象中。

```
*# good, do this*
results **=** **[]**
big_model **=** client**.**scatter**(**big_model**)** *#send the model to the workers first*

**for** file **in** files**:**
    magical_business_insight **=** make_all_the_magics**(**file**,** big_model**)**
    results**.**append**(**magical_business_insight**)**
```

# 处理失败

随着计算任务的增加，您经常会希望能够克服失败。在这种情况下，我的 CSV 中可能有 5%的坏数据是我无法处理的。我想成功地处理 95%的 CSV，但是要跟踪失败，以便我可以调整我的方法并再次尝试。

这个循环是这样的。

```
**import** traceback
**from** distributed.client **import** wait**,** FIRST_COMPLETED**,** ALL_COMPLETED

queue **=** c**.**compute**(**results**)**
futures_to_index **=** **{**fut**:** i **for** i**,** fut **in** enumerate**(**queue**)}**
results **=** **[**None **for** x **in** range**(**len**(**queue**))]**

**while** queue**:**
    result **=** wait**(**queue**,** return_when**=**FIRST_COMPLETED**)**
    **for** future **in** result**.**done**:**
        index **=** futures_to_index**[**future**]**
        **if** future**.**status **==** 'finished'**:**
            **print(**f'finished computation #{index}'**)**
            results**[**index**]** **=** future**.**result**()**
        **else:**
            **print(**f'errored #{index}'**)**
            **try:**
                future**.**result**()**
            **except** **Exception** **as** e**:**
                results**[**index**]** **=** e
                traceback**.**print_exc**()**
    queue **=** result**.**not_done

**print(**results**)**
```

由于这个函数乍一看相当复杂，所以我们来分解一下。

```
queue **=** c**.**compute**(**results**)**
futures_to_index **=** **{**fut**:** i **for** i**,** fut **in** enumerate**(**queue**)}**
results **=** **[**None **for** x **in** range**(**len**(**queue**))]**
```

我们在`results`上调用`compute`，但是因为我们没有经过`sync=True`，所以我们立即返回代表尚未完成的计算的期货。我们还创建了从未来本身到生成它的第 n 个输入参数的映射。最后，我们填充一个结果列表，其中暂时没有结果。

```
**while** queue**:**
    result **=** wait**(**queue**,** return_when**=**FIRST_COMPLETED**)**
```

接下来，我们等待结果，并在结果出现时进行处理。当我们等待期货时，它们被分为`done`和`not_done`两种。

```
 **if** future**.**status **==** 'finished'**:**
            **print(**f'finished computation #{index}'**)**
            results**[**index**]** **=** future**.**result**()**
```

如果未来是`finished`，那么我们打印我们成功了，我们存储结果。

```
 **else:**
            **print(**f'errored #{index}'**)**
            **try:**
                future**.**result**()**
            **except** **Exception** **as** e**:**
                results**[**index**]** **=** e
                traceback**.**print_exc**()**
```

否则，我们存储异常，并打印堆栈跟踪。

```
 queue **=** result**.**not_done
```

最后，我们将队列设置为那些尚未完成的期货。

# 结论

Dask 绝对能帮你节省时间。如果您花时间等待代码运行，您应该使用这些简单的提示来并行化您的工作。使用 Dask 还可以做许多高级的事情，但这是一个很好的起点。

声明:我是[土星云](https://www.saturncloud.io/s/home/)的 CTO。我们让您的团队轻松连接云资源。想用 Jupyter 和 Dask？部署模型、仪表板或作业？在笔记本电脑或 4 TB Jupyter 实例上工作？完全透明地了解谁在使用哪些云资源？我们做所有这些，甚至更多。

*原载于 2021 年 8 月 26 日*[*https://Saturn cloud . io*](https://saturncloud.io/blog/dask-for-beginners/)*。*