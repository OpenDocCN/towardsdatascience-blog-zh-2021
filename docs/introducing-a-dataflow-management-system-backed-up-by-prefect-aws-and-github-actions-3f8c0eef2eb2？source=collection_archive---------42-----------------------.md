# 引入由 Prefect、AWS 和 Github Actions 支持的数据流管理系统

> 原文：<https://towardsdatascience.com/introducing-a-dataflow-management-system-backed-up-by-prefect-aws-and-github-actions-3f8c0eef2eb2?source=collection_archive---------42----------------------->

该 Github 项目如何通过提供 AWS 和 Prefect 云基础设施将您的本地 Prefect 工作流提升到一个新的水平，以及作为其中一部分的重要角色 *Github Actions* 。

![](img/fbbc3547566a5d2cc0fe9daf3b7a0188.png)

由[萨法尔萨法罗夫](https://unsplash.com/@codestorm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**TL；DR:** [该项目](https://github.com/maikelpenz/dataflow-automation-infra)建立了一个*数据流管理系统*，由[总监](https://www.prefect.io/)和 [AWS](https://aws.amazon.com/) 提供动力。它的部署已经通过 [Github Actions](https://github.com/features/actions) 完全自动化，此外[还公开了一个可重用的接口](https://docs.github.com/en/actions/creating-actions)以向 [Prefect Cloud](https://www.prefect.io/cloud/) 注册工作流。

# 问题是

perfect 是一个开源工具，让团队能够用 Python 编排工作流。它的云解决方案- [Prefect Cloud](https://www.prefect.io/cloud/) -在框架之上增加了一个管理层。这使得团队能够管理工作流，同时在保证执行层保持在用户一侧的混合模型下运行。

这么说的话，用户有责任*启动*并将*执行层与 Prefect Cloud*集成，这意味着需要一些工程工作来使解决方案端到端地启动和运行。

# 动机:当自动化需要自动化时

几个月前，我与 Prefect 和 Python 一起做了一个个人项目。当我完成工作流开发并向 Prefect Cloud 注册时，我注意到*注册步骤*与工作流定义不太匹配，对我来说变成了一个手动和重复的过程。然后，我决定为未来的工作设计一些通用的和可重用的东西。

我最初的方法是创建一个私有的*助手项目*，唯一的目的是自动化本地工作流和完美云之间的这个*桥梁*。然而，随着这个想法的成熟和它的价值变得更加清晰，代码库也逐渐涵盖了在云中运行执行环境的自动化。该项目的目的从狭隘的关注交付*一个可重用的工作流注册工具*发展到更大的东西，为希望*在 AWS 上执行工作流并通过完美云*管理它们的团队提供一个起点或模板。

# 解决方案:数据流-自动化-基础设施

![](img/fc3740f3911818f1b62fc046edf4a541.png)

作者图片

我将这个项目命名为[*data flow-automation-infra*](https://github.com/maikelpenz/dataflow-automation-infra)*it*现在已经达到了一个很好的状态，我自己或者任何遵循 README 文件的[部署部分](https://github.com/maikelpenz/dataflow-automation-infra#deployment)的人都可以毫无问题地将基础设施推到一个全新的环境中。**

**该项目有三个主要特性，部署和自动化测试都是通过自动触发的 Github 动作(T21)管道来完成的。**

## **1 -在 AWS 上自动创建执行环境**

**这加快了 AWS 环境执行工作流的速度。目前，有一个可用的环境运行在 [ECS](https://aws.amazon.com/ecs) (弹性容器服务)和 [Fargate](https://aws.amazon.com/fargate) 上。这种方法利用 AWS [计算服务](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/compute-services.html)，同时运行 100% [无服务器](https://aws.amazon.com/serverless/)，不涉及任何管理。**

**这组执行环境可以很容易地扩展以适应新的需求。一个很好的例子就是包含一个 [Kubernetes](https://kubernetes.io/) 执行环境。**

## **2 -将执行环境与完美的云相集成**

**[提督代理](https://docs.prefect.io/orchestration/agents/overview.html)是一个长期运行的进程，它在云*执行环境*和*提督云*之间架起了一座沟通的桥梁。该流程监听工作流*运行*请求，将它们委托给云提供商的执行环境，并不断将*运行*状态反馈给 Prefect Cloud。**

**在 Prefect 上有一些[代理类型](https://docs.prefect.io/orchestration/agents/overview.html#agent-types)可用，但是由于贯穿整个项目的单一执行环境是在 ECS Fargate 上运行的，所以它目前提供了具有 [ECS 代理类型](https://docs.prefect.io/orchestration/agents/ecs.html#requirements)的代理。提督代理作为一个任务在 ECS Fargate 上运行，在一个专门用于代理的集群上运行。**

## **3 -提供注册工作流的界面**

**到目前为止，您已经知道 Github Actions 运行项目管道来创建 AWS 和完美的云环境。然而，你可能知道也可能不知道的是，Github Actions 也提供了一个[市场](https://github.com/marketplace)，人们[在那里发布他们自己的动作](https://docs.github.com/en/actions/creating-actions)，以便其他开发者/存储库可以使用它们。**

**我利用这个特性创建了一个集中的、可重用的方法来*向 Prefect Cloud* 注册工作流。通过这个*定制动作，*任何人都可以从单独的存储库中发布工作流，同时能够在由 *dataflow-automation-infra 部署的基础设施之上运行它们。***

# **进一步开发和扩展**

**如上所述，该项目目前用它的完美代理建立了一个单一的执行环境。对于生产用例，这可以扩展到提供多种配置，既适合简单的用例，也适合更复杂的用例。例如:用于分析的简单数据摄取工作与训练资源密集型机器学习模型。**

**对于任何想要尝试的人来说，自述文件包含了详细的部署步骤。如果您有任何反馈或希望为项目做出贡献，请联系我们。下次见！**