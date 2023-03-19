# 卡夫卡不再需要动物园管理员了

> 原文：<https://towardsdatascience.com/kafka-no-longer-requires-zookeeper-ebfbf3862104?source=collection_archive---------3----------------------->

## 2.8.0 版本让你提前接触到没有动物园管理员的卡夫卡

![](img/8b733162a87b08f4811f321eb833294f.png)

由[克里斯蒂安·兰伯特](https://unsplash.com/@_christianlambert)在 [Unsplash](https://unsplash.com/photos/G2-_RJYifKk) 拍摄的照片

## 介绍

Apache Kafka **2.8.0** 终于发布了，你现在可以提前使用 KIP-500，它消除了 Apache Zookeeper 的依赖性。相反，Kafka 现在依赖于内部 Raft 仲裁，可以通过 **Kafka Raft 元数据模式**激活。这一新功能简化了集群管理和基础设施管理，标志着 Kafka 自身的一个新时代。

> 没有动物园管理员的卡夫卡

在这篇文章中，我们将首先讨论为什么需要消除对 ZooKeeper 的依赖。此外，我们将讨论从 2.8.0 版本开始,`KRaft mode`如何取代 ZooKeeper，并探讨移除这种依赖性的影响以及 Kafka 本身将如何受益于这种增强。

## 什么是 KPI-500

直到现在，Apache ZooKeeper 还被 Kafka 用作元数据存储。分区和代理的元数据存储在 ZooKeeper 仲裁中，该仲裁也负责 Kafka 控制器的选举。

ZooKeeper 是 Kafka 的外部系统，这意味着它有自己的配置语法、管理工具和最佳实践。因此，如果您想要部署 Kafka 集群，那么您还必须管理、部署和监控 Zookeeper。由于这两个分布式系统需要不同的配置，复杂性增加，因此系统管理员更容易出错。拥有两个系统还会导致工作的重复，例如，为了启用安全特性，您必须将相关的配置应用于两个服务。

拥有外部元数据存储在资源方面是低效的，因为您需要运行额外的进程。再者，这也限制了卡夫卡本身的可扩展性。每次集群启动时，Kafka 控制器都必须从 ZooKeeper 加载集群的状态。

选举新的总监时，情况也是如此。考虑到元数据的数量会随着时间的推移而变得更大，这意味着加载此类元数据的效率会随着时间的推移而变得更低，从而导致高负载过程，并因此限制了集群可以存储的分区数量。

KPI 代表 Kafka 改进建议，KPI-500 介绍了无动物园管理员 Kafka 的基本架构。

## 介绍 Kafka Raft 元数据模式

2.8.0 版本引入了对无动物园管理员 Kafka 的早期访问，作为 KPI-500 的一部分。请注意，该实现已经部分完成，因此您**不应该在生产环境**中使用它。

在最新版本中，ZooKeeper 可以被一个内部 Raft 法定数量的控制器所取代。当 **Kafka Raft 元数据模式**启用时，Kafka 会将其元数据和配置存储到一个名为`@metadata`的主题中。该内部主题由内部仲裁管理，并在整个集群中复制。集群的节点现在可以充当代理、控制器或两者兼而有之(称为*组合*节点)。

当`**KRaft mode**` 启用时，只有少数选定的服务器可以充当控制器，这些服务器将构成内部法定人数。控制器可以处于活动或待机模式，如果当前活动的控制器服务器出现故障或关闭，最终将接管工作。

现在每个 *Kafka 服务器*都有一个额外的配置参数，叫做`**process.roles**`。该参数可以采用以下值:

*   `broker`:Kafka 服务器将充当代理
*   `controller`:Kafka 服务器将作为内部 Raft 仲裁的控制器
*   `broker,controller`:Kafka 服务器将同时作为法定人数的控制者和代理

注意，当根本没有提供`process.roles`时，假设集群将以 ZooKeeper 模式运行。因此，暂时`process.roles`配置参数是您可以激活`KRaft mode`的唯一方式。

此外，每个节点现在用其`node.id`标识，并且现在必须提供`controller.quorum.voters`配置参数，该参数相当于 ZooKeeper 模式中的`zookeeper.connect`。该参数用于识别内部仲裁的控制器服务器，数值格式为`serverID@host:port, serverID@host:port, ...`。

现在让我们假设在运行于`KRaft mode`的 Kafka 集群中，我们有 7 个代理和 3 个控制器。下面的代码片段演示了 Raft 仲裁中一个控制器服务器的配置示例。

```
process.roles=controller
node.id=1
listeners=CONTROLLER://controller-1-host:9092
controller.quorum.voters=1@controller-1-host:9092,2@controller-2-host:9092,3@controller-3-host:9092
```

同样，下面的配置演示了如何设置集群的一个代理:

```
process.roles=broker
node.id=4
listeners=PLAINTEXT://:9092
controller.quorum.voters=1@controller-1-host:9092,2@controller-2-host:9092,3@controller-3-host:9092
```

## 再见，动物园管理员

Apache ZooKeeper 依赖性的移除无疑是平台的一大进步。过去几年，整个社区(尤其是合流社区)都在朝着这个方向努力。最早的版本是整个 Kafka 社区的巨大努力，他们仍在努力改进，以便无动物园管理员的 Kafka 模式在今年内功能完整。

> 我们多年来一直朝着这个方向前进
> 
> — **杰森·古斯塔夫森@** [卡夫卡峰会 2019](https://www.confluent.io/kafka-summit-san-francisco-2019/kafka-needs-no-keeper/)

Apache ZooKeeper 依赖性的消除简化了 Kafka 部署的基础设施管理。Kafka 和 ZooKeeper 是两种不同的服务——现在 Kafka 已经统一，因此它不依赖外部服务作为元数据存储，学习曲线将缩短，这最终将有助于扩大 Kafka 的采用范围。

此外，这一增强提供了一个更具可扩展性和健壮性的整体架构。如前所述，在 ZooKeeper 模式下，Kafka 必须将其元数据存储到 ZooKeeper 节点中。每次集群启动或控制器选举发生时，Kafka 控制器都必须从效率低下的外部服务中读取元数据。通过用这个内部 Raft 仲裁替换 ZooKeeper，部署现在可以支持更多的分区。

移除 ZooKeeper 依赖关系还可以支持具有单个节点的集群。当您想要测试 Kafka 作为概念验证的一部分时，您不再需要启动多个过程。

## 结论

对卡夫卡来说，摆脱对动物园管理员的依赖是一个巨大的进步。事实上，新的`KRaft mode`特性将扩展 Apache Kafka 的可伸缩性，并缩短学习曲线，因为现在团队再也不用担心 ZooKeeper 了。它也将使 Kafka 的配置和部署方式更加简单和高效。

除了 KPI-500 之外，Kafka 2.8.0 还提供了许多改进和错误修复，所以一定要看看[发行说明](https://dist.apache.org/repos/dist/release/kafka/2.8.0/RELEASE_NOTES.html)。

**最后，我想再次强调，这是目前的早期访问，这意味着它不应在生产环境中使用。**

## 你可能也喜欢

</overview-of-ui-monitoring-tools-for-apache-kafka-clusters-9ca516c165bd>  <https://betterprogramming.pub/how-to-fetch-specific-messages-in-apache-kafka-4133dad0b4b8> 