# 数据科学中更好的软件写作技巧:当你编码时

> 原文：<https://towardsdatascience.com/better-software-writing-skills-in-data-science-while-you-are-coding-c3b059914bd5?source=collection_archive---------29----------------------->

## 实用程序设计员的每周指导章节阅读

![](img/209336990071f74f219e1e158616c28a.png)

来自 [Pixabay](https://pixabay.com/) 的 [Gerd Altmann](https://pixabay.com/users/geralt-9301/) 拍摄的封面图片。

本周的指导和帖子基于“实用程序员:从熟练工到大师”的第六章:当你编码时。本章的重点是你在编程时应该记住的一些小事。编码是一个迭代的过程，需要注意。如果你没有给予它应有的关注，你可能会碰巧陷入编码。让我们来看看书中提到的一些主题。

**巧合编码与故意编程**

如果没坏就不要修理它。对于可信的代码片段来说，这可能是真的，但是当你正在编写新的代码，或者在现有的代码中添加新的代码行时，你不能确定它没有被破坏，直到它经受了考验。到那时可能就太晚了。有人可能会受到这个 bug 的影响，修复这个 bug 需要的时间比早期发现的时间要长。所以，要仔细考虑你在编码什么，为什么要编码。当你对你需要编码的东西有了更好的理解时，不要害怕重构。把事情简单化，确保不重复自己。这并不是因为一段代码之前就在那里，所以它仍然需要在那里。不断地重新评估你正在编写的代码。

**算法速度**

特别是在数据科学中，您会遇到这样的情况，计算算法速度将有助于您理解为什么分析在某些情况下可以工作，但随着数据大小的增加，会花费太多时间。如果你非常依赖 *Jupyter* 笔记本电脑，要知道现在大多数计算资源都有多个内核。如果你想从多核处理中获益，你必须调整你的代码来利用它。例如在 *scikit learn* 中，许多类都有方法，在这些方法中你可以指定类似 *n_jobs* 的东西来告诉这些方法它可以使用多少个处理器。

如果依赖 python 循环，随着数据的增长，处理过程会变得很长。也许这些循环算法中的一些可以用 *numpy* 转换成矩阵运算？如果不是，习惯 Python 中的多处理将是有益的。通过这种方式，您可以将处理任务划分到多个进程上，这些进程将受益于您的可用内核。然而，你需要考虑减少结果，也许只是简单地将所有的输出连接在一起，或者可能更复杂…至少知道可能性可以使程序运行几个小时而不是几天。

为了让多重处理变得容易，我前段时间做了一个小的助手类。如果需要，请随意使用。

用法相当简单，首先，这里有一个简单的用法示例。reducing 函数嵌入在方法调用(reduce.append)中。

```
simpleMultiprocessing::Creating a process for elements 0-124 simpleMultiprocessing::Creating a process for elements 125-249 simpleMultiprocessing::Creating a process for elements 250-374 simpleMultiprocessing::Creating a process for elements 375-499 simpleMultiprocessing::Creating a process for elements 500-624 simpleMultiprocessing::Creating a process for elements 625-749 simpleMultiprocessing::Creating a process for elements 750-874 simpleMultiprocessing::Creating a process for elements 875-999 simpleMultiprocessing::All jobs submitted simpleMultiprocessing::All jobs ended [7750, 23375, 39000, 54625, 70250, 85875, 101500, 117125] 499500
```

你可以很容易地添加一个合适的缩减函数，如下例所示。

```
simpleMultiprocessing::Creating a process for elements 0-124 simpleMultiprocessing::Creating a process for elements 125-249 simpleMultiprocessing::Creating a process for elements 250-374 simpleMultiprocessing::Creating a process for elements 375-499 simpleMultiprocessing::Creating a process for elements 500-624 simpleMultiprocessing::Creating a process for elements 625-749 simpleMultiprocessing::Creating a process for elements 750-874 simpleMultiprocessing::Creating a process for elements 875-999 simpleMultiprocessing::All jobs submitted simpleMultiprocessing::All jobs ended [7750, 23375, 39000, 54625, 70250, 101500, 117125] 413625
```

如果您需要调试其中一个多处理函数，将 simpleMultiprocessing 类的用法改为使用 protectedMultiprocessing 可能会有所帮助。在下一个例子中，我们通过主动为其中一个元素引入一个异常来展示它是多么的有用。如果在 simpleMultiprocessing 的情况下出现异常，我们会悄悄地丢失该作业和所有结果。这里我们意识到了问题，但仍然得到了其他结果。缺点是 reducing 函数被调用得更频繁，但在正常情况下，艰苦的工作无论如何都是由 mapping 函数完成的。

```
protectedMultiprocessing::Creating a process for elements 0-124 protectedMultiprocessing::Creating a process for elements 125-249 protectedMultiprocessing::Creating a process for elements 250-374 protectedMultiprocessing::Creating a process for elements 375-499 protectedMultiprocessing::Creating a process for elements 500-624 protectedMultiprocessing::Creating a process for elements 625-749 protectedMultiprocessing::Creating a process for elements 750-874 protectedMultiprocessing::Creating a process for elements 875-999 protectedMultiprocessing::All jobs submitted An error occured... while processing item: 666 exception: integer division or modulo by zero protectedMultiprocessing::All jobs ended [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, [...], 663, 664, 665, 667, 668, 669, 670, [...], 995, 996, 997, 998, 999] 498834
```

你可以用多重处理做更多有趣的事情，这是值得学习的。同样，对于简单的多处理用例，请随意使用我上面的类。

**还有更**

第六章还有更多内容:重构、单元测试和邪恶巫师。由于这本书写得很好，我就不赘述了。但是我只想说:在你编码的时候不要害怕重构。很多时候，当我给我的工作增加复杂性时，我发现有很多方法可以保持干燥(不要重复)。通过重构，我能够简化流程，甚至使代码更容易理解。参数化可以让你走得更远。

最后，我恳求你，做单元测试！并将它们与您的代码一起保存。python 中有很好的单元测试框架: *unittest* 随 Python 一起发布，我经常使用 *pytest* ，你可以使用 *mock* ，或者*假设*。即使没有框架，你也可以使用准系统 *asserts* 做好单元测试工作，所以没有借口。这本书就单元测试的主要目标给出了很好的指导。当您稍后将更新代码时，这种可能性很高，您会很高兴有现有的单元测试来验证基础。

这是我对第六章“实用程序员:从熟练工到大师”阅读的补充，作为提高数据科学中软件写作技能的媒介。下周我们将看一下第 7 章:项目前。

*原载于 2021 年 4 月 30 日 http://thelonenutblog.wordpress.com*<https://thelonenutblog.wordpress.com/2021/04/30/better-software-writing-skills-in-data-science-while-you-are-coding/>**。**