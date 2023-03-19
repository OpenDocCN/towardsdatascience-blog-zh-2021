# 你喜欢云原生技术，但你知道它提倡什么吗？

> 原文：<https://towardsdatascience.com/you-love-cloud-native-tech-but-do-you-know-what-it-advocates-8d0ea9f59727?source=collection_archive---------31----------------------->

## 云原生(Cloud-Native)似乎是最近的另一个流行词，但它真正的意思是什么呢？

![](img/61644c68c24eecb321025483adf2a799.png)

由 [Unsplash](https://unsplash.com/s/photos/cloud?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Pero Kalimero](https://unsplash.com/@pericakalimerica?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

如果您是 web 或软件开发人员、DevOps 工程师或数据科学家，您肯定听说过术语云原生。有时，当我们总是偶然发现一个术语时，我们不知何故会想到它的意思。类似于大纲的东西。**然而，我们真的知道这意味着什么吗？**

那么，到底什么是云原生工具和云原生应用。你能向一个五岁的孩子描述这些术语吗？因为，真的，如果你不能通过五岁的考验，你还没有内化这些术语，你的真理是片面的。

这个故事试图让我们了解以前黑暗的角落，并一劳永逸地澄清术语云原生。为此，我们不会遵循字典的方法，而是试图建立一种直觉。我们开始吧。

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=cloud_native)是我每周给那些对 AI 和 MLOps 世界好奇的人发的简讯。你会在每周五收到我关于最新人工智能新闻、研究、回购和书籍的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=cloud_native)！

# 云原生

Cloud-native 封装了一组实践，组织可以使用公共、私有或混合云提供商来构建和管理大规模应用程序。但是我们承诺不遵循字典的方法。

> 使用云原生实践的组织旨在更快地交付新功能，并对变化或意外事件做出更快的响应。

所以，你应该问的第一个问题是，“这有什么意义？”让我们把它分解成对企业的意义和对开发者的意义。

从业务角度来看，使用云原生实践的组织旨在更快地交付新产品功能。此外，公司可以对任何变化或意外事件做出快速反应。**因此，这里的关键术语是速度和敏捷性。**

采用云原生战略的公司可以向客户交付更多价值，交付速度更快，并轻松扩展其产品的功能。同样，增长和服务可用性的提高也是云原生实践的诱人成果。该公司可以让客户满意，增加其客户群，并确保其产品全天候可用。

从技术角度来看，采用云原生思维意味着自动化、流程编排和可观察性。这些似乎本身就是流行语，我们再深入探讨一下。

自动化意味着开发人员可以更快地将新特性发布到产品中，而无需人工干预。编排意味着管理和配置成千上万的服务几乎不费吹灰之力。最后，可观察性意味着，在任何时候，工程师都可以获得系统状态的整体视图，同时还可以知道每个组件分别在做什么。

# 但是怎么做呢？

通常，术语原生云与容器和容器编排引擎密切相关。

容器很小，通常是不可变的单元，应用程序的一部分在里面运行；想一个微服。开发人员可以创建一个容器来封装一个简单的服务，快速部署它，轻松更新它，并在不再需要时销毁它。

但是设计这样的架构，其中应用程序被分解成许多微服务，产生了新的挑战。如何管理所有这些容器？我们如何发现可用的服务，如何配置它们，以及如何确保它们按照预期的方式工作？

许多项目被引入来解决这个问题。Docker Swarm 和 Apache Mesos 是几年前最引人注目的两个项目，但 Kubernetes 赢得了容器编排引擎之战，现在是事实上的标准。

Kubernetes 是一个可移植、可扩展的开源平台，用于管理容器化的工作负载和服务。它采用了一种声明性的配置方法，在这种方法中，用户指定世界的期望状态，而不明确指定这应该如何表现。这种想法让我们可以灵活地运行分布式系统，并根据用户的需求进行扩展。

简而言之，Kubernetes 是一个容器协调器，帮助确保每个容器都在它应该在的地方，并且容器之间可以相互通信。如果你想知道为什么你应该关注 Kubernetes，请阅读下面的故事:

</kubernetes-101-why-should-you-care-6793a09a096a> [## Kubernetes 101:你为什么要关心？

towardsdatascience.com](/kubernetes-101-why-should-you-care-6793a09a096a) 

随着时间的推移，几个 CNCF(云本地计算基金会)项目扩展了 Kubernetes 的功能。服务网格工具允许对集群内的流量进行粒度控制。日志记录和跟踪有助于调试每个事件。新的存储选项允许有状态应用程序无问题地运行:所有这些以及更多的东西使 Kubernetes 成为了增长最快的开源项目之一。

# 结论

如果您是 web 或软件开发人员、DevOps 工程师或数据科学家，您肯定听说过术语云原生，但它真正的含义是什么呢？

从业务角度来看，使用云原生实践的组织旨在更快地交付新功能，并对变化或意外事件做出更快的响应。

从技术角度来看，采用云原生实践的工程团队包含自动化、编排和可观察性，将应用程序分解为许多小型的容器化服务。

推动这一发展的关键技术是容器和容器运行时以及编排引擎，如 Kubernetes。

简单地说，容器很小，通常是封装了工作负载及其依赖关系的不可变单元。Kubernetes 是一个容器协调器，它帮助确保每个容器都在它应该在的地方，并且容器之间可以相互通信。

云原生应用和工具已经存在，所以让我们熟悉一下这些术语吧！

# 关于作者

我叫[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=cloud_native)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。