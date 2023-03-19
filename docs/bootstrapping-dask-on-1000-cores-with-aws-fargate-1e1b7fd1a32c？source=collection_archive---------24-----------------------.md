# 使用 AWS Fargate 在 1000 个内核上引导 Dask

> 原文：<https://towardsdatascience.com/bootstrapping-dask-on-1000-cores-with-aws-fargate-1e1b7fd1a32c?source=collection_archive---------24----------------------->

## 利用云的力量来供应和部署 1000 个内核，以处理大量数据并完成工作

![](img/fa145ce0d80cf521cbb224427142d440.png)

美国宇航局在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

作为一名数据工程师，我发现自己多次需要一个系统，在这个系统中，我可以构建繁重的大数据任务的原型。我的团队传统上使用 EMR 和 Apache Spark 来完成此类任务，但是随着关于 [Apache 淘汰 hadoop 项目](https://www.zdnet.com/article/apache-software-foundation-retires-slew-of-hadoop-related-projects/)的消息以及 python 是数据分析的首选工具这一常见范例的出现，我开始追求一个更现代的 python 原生框架。

我的目标是实现 1000 核集群的一键式部署，可以运行约 1TB 数据的原型处理，然后拆除。另一个条件是，这些资源不需要干扰我团队的其他资源。因此一键 eks-fargate-dask 工具诞生了！

# AWS Fargate

> AWS Fargate 是一个用于容器的无服务器计算引擎，它与[亚马逊弹性容器服务(ECS)](https://aws.amazon.com/ecs/) 和[亚马逊弹性库本内特服务(EKS)](https://aws.amazon.com/eks/) 一起工作。

简而言之，Fargate 使开发人员能够根据 pods 指定的需求来分配 CPU 和内存，而不是预先分配节点。

# eksctl

eksctl 是一个用于管理 EKS 集群的 cli 工具。它允许通过 EC2(计算节点)或 Fargate(无服务器)后端使用一个命令来配置和部署 EKS 集群。截至本文撰写之时，它还不支持指定 Fargate spot 实例，这太糟糕了。

eksctl alsl 提供了实用程序命令，用于提供对 kubernetes 服务帐户的 IAM 访问。

# 达斯克

Dask 是一个库，它支持在远程或本地集群上通过并行计算来处理大于内存的数据结构。它为数组(numpy)和数据帧(pandas)等数据结构提供了熟悉的 python APIs。建立基于本地流程的集群非常简单，但依我个人之见，Dask 的亮点在于其令人惊叹的生态系统，允许在 Kubernetes、YARN 和许多其他多处理集群上建立集群。

# 把所有的放在一起

首先，我使用这个脚本部署一个 EKS Fargate 集群。该脚本需要在 IAM 角色的上下文中运行，该角色拥有许多 AWS 部署操作的权限(主要是部署 Cloudformation 堆栈)。用于 dask workers 的 IAM 角色并没有提供对 S3 服务的完全访问权限，因为我是从 S3 的 parquet 文件中加载数据的。

运行这个命令后，您应该有一个名为 *dask* 的 EKS 集群启动并运行。在接下来的部分，我们将部署一个 [dask 头盔](https://helm.dask.org/)版本。可以通过设置值来定制舵释放。这些是我在 dask 集群中使用的值:

然后安装舵释放装置:

就是这样！您已经安装了 dask 集群！您可以在 s3 上运行这个使用开放的 [ookla 数据集](https://registry.opendata.aws/speedtest-global-performance/)的示例笔记本，看看扩展集群如何提高性能。

Dask 数据框 API 与 pandas 并不是 100%兼容(可以说是设计上的)。Dask 的文档网站上有一堆有用的资源。

**使用完集群后，记得缩小规模！**

# 结论

我一直在寻找一种快速的方法，不依赖于 python 原生框架的原型大数据处理。这绝不适合生产使用，并且可能也没有进行成本优化，但是它完成了工作，在几分钟内我就能够处理超过 300GB 的 parquet 文件(数据帧中可以膨胀到 1TB)