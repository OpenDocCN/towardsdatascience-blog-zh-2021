# 如何让您的数据科学解决方案进入放手状态

> 原文：<https://towardsdatascience.com/how-to-move-your-data-science-solution-into-a-hands-off-state-b2a02b1df758?source=collection_archive---------26----------------------->

## 将数据科学解决方案投入生产的一些工具和技巧

![](img/f763df545c5fc011cf7bff2b7749123c.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [S Migaj](https://unsplash.com/@simonmigaj?utm_source=medium&utm_medium=referral) 拍摄的照片

一旦您开发了一个数据科学解决方案，就很容易无限期地修补它:手动运行它，对输出进行小的更改，迭代模型。

这并不理想——一旦你有了一个满意的解决方案，理想情况下，你应该能够让它在你最少的投入下运行，除非有什么东西倒下了。

无论您是移交给支持团队，还是自己支持该工具，当您试图将您的解决方案转移到放手状态，或者将其投入生产时，这篇文章都会给出一些提示。

首先，你想要记录。日志记录是将代码输出打印到文件中的地方，以便您(或支持团队)可以在以后引用它。要将代码转移到放手状态，您需要将日志保存到一个文件中，以便在工具出现问题时可以检查这些日志。

![](img/cf8c99d31c68c115f5b7060c85d0ed37.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)上的 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你应该记录什么？工具可能出现的任何明显的问题，比如数据验证输入，或者工具无法连接到 web 服务，等等。应该标记为警告或错误并记录下来。

另一个好的经验法则是标记任何“交易”。我指的是像读取文件、连接数据库或调用 API 这样的事情。这些都是可能失败的事情，并且可以作为代码失败的地方和可能出错的地方的粗略指南。最好在日志的末尾有一个摘要，指出工具是否成功完成，如果失败了，是哪里出错了。日志摘要真的很受支持团队的欢迎。

另一个有用的工具是警报。这是您的工具在出错或失败时通知您(或支持团队)的地方。最有效的方法之一是让你的工具在 slack 或 MS 团队出错时给你发消息。

![](img/204db661ae2db82d62856e4ade612907.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

使用 webhooks 很容易做到这一点。您也可以使用电子邮件进行此操作，例如使用 AWS SNS。当出现问题时，以及工具启动和完成时，通知您是一个好主意；如果没有这些警报，也会告诉您有问题。

伤检分类表是非常宝贵的，尤其是当你移交给一个支持团队或者有人在你离开时照看这个工具的时候。

分类表列出了该工具最容易出错的地方，以及应该联系哪个团队来解决这个问题。例如，如果您的工具依赖于由另一个团队管理的数据馈送；如果这种情况发生，您可以直接联系该团队请求支持。这样，问题就可以得到解决，而不需要你充当中间人，浪费每个人的时间。这样做的行为也迫使您仔细考虑您的工具以及应该有什么样的错误处理，以确保从日志中清楚地看到哪里出错了。

最后，你必须确保你心中有企业的最大利益。作为一名数据科学家，对模型进行不断的迭代，或者同意客户提出的每一个请求都很容易。但归根结底，你的工作是为企业提供尽可能多的价值。你希望一个模型是有效的，但是你不需要它是完美的，在某个点之后，你可能可以在不同的解决方案上提供更多的价值。

同样，倾听客户的需求也是必要的，但是在确保满足客户需求一段时间后，您可能能够通过为客户构建不同的工具来提供更好的价值。

*原载于 2021 年 4 月 17 日*[*【https://jackbakerds.com】*](https://jackbakerds.com/posts/data-science-solution-to-hands-off-state/)*。*