# Kubernetes 101:你为什么要关心？

> 原文：<https://towardsdatascience.com/kubernetes-101-why-should-you-care-6793a09a096a?source=collection_archive---------20----------------------->

## 什么是 Kubernetes，为什么每个人似乎都在谈论它？

![](img/06606fe7463cac3360e72e67a0782ca2.png)

约瑟夫·巴里恩托斯在 [Unsplash](https://unsplash.com/s/photos/ship-wheel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Kubernetes 是近年来基础设施领域的一个时髦词。Kubernetes 是一个希腊词，用来形容一个人在指挥，除此之外，它还是最成功、发展最快的开源项目之一。

但这是为什么呢？什么是 Kubernetes，它试图解决什么问题？这个故事探讨了 Kubernetes 的现状和原因，作为对容器和编排世界的介绍。

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=kubernetes_what_why)是我每周给那些对 AI 和 MLOps 世界好奇的人发的简讯。你会在每周五收到我关于最新人工智能新闻、研究、回购和书籍的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=kubernetes_what_why)！

# 什么是 Kubernetes？

Kubernetes 是一个可移植、可扩展的开源平台，用于管理容器化的工作负载和服务。它采用了一种声明性的配置方法，在这种方法中，用户指定世界的期望状态，而不明确指定这应该如何表现。这种想法让我们可以灵活地运行分布式系统，并根据用户的需求进行扩展。

简而言之，Kubernetes 是一个容器协调器，帮助确保每个容器都在它应该在的地方，并且容器之间可以相互通信。

如果你还是觉得失落，那也没关系。有时你必须理解为什么我们需要这样的东西，问题是什么，以及这个新的闪亮的东西如何提供解决方案。为此，让我们考察一下 Kubernetes 的目的是什么。

# 为什么是 Kubernetes？

一些应用程序将其所有功能捆绑到一个可部署的构件中。不同的服务、事务和第三方集成存在于同一个核心代码库中。这些应用被称为独石。

![](img/925cd0793dd2000f8b11d752ffae9bc0.png)

照片由[Zoltan·塔斯](https://unsplash.com/@zoltantasi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/monolith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

这种设计使得系统僵化，升级困难，因为所有东西都必须同时推出。假设一个开发服务`x`的团队已经完成了任务。在这种情况下，他们除了等待团队`y`的开发人员完成他们的 sprint 之外什么也做不了，他们正在开发应用程序的一个完全不同的方面。

而且，缩放也有同样的问题；假设应用程序的一部分需要更多的资源来交付。现在，工程师必须为整个应用程序提供更多的资源，即使瓶颈只在单个领域。

为了应对这些挑战，我们转向了微服务；系统的功能被分解成可重复使用的代码，每个代码负责一项操作。如果我们想更新应用程序的一部分，我们可以很容易地只更新负责的服务。此外，我们可以扩展单个服务，以匹配用户期望的性能，而无需过度配置。

因此，分布式计算开始普及，容器在正确的时间出现在正确的地点。有了容器，开发人员可以将他们的服务与所有的依赖项和配置打包在一起。因此，开发人员现在确信，无论底层基础设施如何，他们的服务都可以以相同的方式运行。

然而，仍有一些问题悬而未决:

*   更新一个容器很容易，但是我们怎样才能做到不被用户注意到并且没有任何停机时间呢？
*   服务如何发现彼此，容器如何知道如何通信？
*   我们如何监控系统的性能，在需要时进行干预，并调试问题？

# 管弦乐队指挥

我们需要的是一个能够自动化所有日常工作的编排系统。具体来说，理想的系统应该:

*   在不同节点上调度工作负载(即容器)
*   监控节点和容器运行状况问题并做出反应
*   提供存储、网络、代理、安全性和日志记录
*   声明性的而不是命令性的
*   可扩展

这就是 Kubernetes 发挥作用的地方；像乐队指挥一样，Kubernetes 管理不同节点上容器的生命周期，这些节点可以是物理或虚拟系统。

这些节点组成一个集群。每个容器都有端点、DNS、存储和可伸缩性。Kubernetes 是为了自动化所有这些重复劳动。

# 结论

Kubernetes 是近几年来基础设施领域的热门词汇。什么是 Kubernetes，为什么每个人似乎都在谈论它？

这个故事研究了 Kubernetes 是什么，它试图解决什么问题，以及它是如何做到的。我们看到了是什么让我们来到这里，单片系统和微服务架构之间的差异，声明式流程如何工作，以及理想的编排引擎的关键方面是什么。

在接下来的文章中，我们将深入探讨，创建应用程序，管理它们的生命周期，并更详细地解释 Kubernetes 的几个特性。

# 关于作者

我叫[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=kubernetes_what_why)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。