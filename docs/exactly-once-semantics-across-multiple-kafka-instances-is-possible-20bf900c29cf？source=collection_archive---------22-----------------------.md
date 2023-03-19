# 跨多个 Kafka 实例的“恰好一次”语义是可能的

> 原文：<https://towardsdatascience.com/exactly-once-semantics-across-multiple-kafka-instances-is-possible-20bf900c29cf?source=collection_archive---------22----------------------->

## 用代码解决 Kafka 中的跨集群事务问题

![](img/7ce6a0f448f37e8aa3fde6c58a5573fb.png)

供图:[内森·杜姆劳](https://unsplash.com/@nate_dumlao)在 [unsplash](https://unsplash.com/)

“恰好一次”语义在分布式系统中是一个具有挑战性的问题。为了解决这个问题，一些著名的协议和算法有:[两阶段提交](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)、 [Paxos](https://en.wikipedia.org/wiki/Paxos_(computer_science)) 和 [Raft](https://en.wikipedia.org/wiki/Raft_(algorithm)) 。当跨越分布式系统的两个实例时，这个问题变得更加困难。

[Apache Kafka 已经在三年前在一个实例或**一个集群**的上下文中支持“恰好一次](https://www.confluent.io/blog/exactly-once-semantics-are-possible-heres-how-apache-kafka-does-it/)”(又名事务)**，并且在那段时间内保持迭代:[KIP-447](https://cwiki.apache.org/confluence/display/KAFKA/KIP-447%3A+Producer+scalability+for+exactly+once+semantics)[KIP-360](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=89068820)[KIP-588](https://cwiki.apache.org/confluence/display/KAFKA/KIP-588%3A+Allow+producers+to+recover+gracefully+from+transaction+timeouts)。**

大型企业用例通常不会只运行一个 Kafka 实例，托管多个 Kafka 实例的常见场景包括(不限于):

(1) [灾难恢复](https://www.confluent.io/kafka-summit-lon19/disaster-recovery-with-mirrormaker-2-0/)

(2) [本地的多个“本地”实例，然后“聚合”成一个中央实例](https://engineering.linkedin.com/kafka/running-kafka-scale)

(3)针对不同目的的特殊调整的实例:“摄取”实例针对高吞吐量摄取进行供应和调整，而“处理”实例针对计算密集型作业。

因此，**在多个实例**的上下文中，自然问题变成:

> 在一个集群中，我们还能拥有与我们期望的“事务性”或“恰好一次”数据处理相同的“恰好一次”语义吗？

本文将分享关于如何基于新的 [MirrorMaker](https://github.com/apache/kafka/blob/trunk/connect/mirror/README.md) (或 MirrorMaker 2)，Kafka 生态系统中新的跨数据中心复制工具，在 2 个集群上实现上述功能的高级想法。最后，发布代码实现。

*(免责声明:在我写这篇博客的时候，下面的“恰好一次”功能处于“审查”状态。虽然它已经引起了很多关注，并在某些环境下经过了验证正确性的繁重工作测试，但它可能会发生变化，并遵循与*[*Apache License 2.0*](https://www.apache.org/licenses/LICENSE-2.0)*)*相同的保修

## 为什么跨多个实例的恰好一次语义很难

为简单起见，我们以 MirrorMaker 2 为例。

Kafka 消费者从一个集群(称为“源”)消费一批数据，然后 Kafka 生产者立即将其生产到另一个集群(称为“目标”)。为了确保“恰好一次”的交付，[生产者每次从消费者那里收到一批数据时，都会通过“协调者”](https://cwiki.apache.org/confluence/display/KAFKA/KIP-98+-+Exactly+Once+Delivery+and+Transactional+Messaging)创建一个新的事务。通过“恰好一次”协议，协调器位于生产者指向的集群(“目标”)中。

如果一切正常，在新生成的数据在目标集群上可见以供消费之前，完成事务的最后一步是让生产者将消费者补偿提交给一个“补偿”主题(通常该主题被命名为 *_consumer_offsets_* )，因此消费者将知道在下一批中在哪里消费。然而，消费者偏移是由消费者初始化的，并且偏移主题必须位于源集群中。指向目标集群的生成器无法生成/写入位于不同集群中的偏移主题。

## 如何跨多个集群支持恰好一次

[KIP-656](https://cwiki.apache.org/confluence/display/KAFKA/KIP-656%3A+MirrorMaker2+Exactly-once+Semantics) 就是整个提案。简而言之，我们仍然利用[当前为一个集群](https://cwiki.apache.org/confluence/display/KAFKA/KIP-98+-+Exactly+Once+Delivery+and+Transactional+Messaging)设计的一次性框架，但是在将它应用于多个集群时解决了上述挑战。

关键点是:关于消费者补偿和补偿主题，它的**单一真值来源**由生产者管理、提交并存储在**目标**集群上。

*等一下——在上面的部分中，消费者偏移和偏移主题必须在源集群上？*

为了从源集群中提取数据，消费者仍然必须生活在源集群中，但是“真实的源”消费者偏移量不再存储在源集群中。当数据传输作业(在当前上下文中，作业是 MirrorMaker)重新启动或重新平衡时，我们建议使用以下想法来正确地回滚消费者，同时“真实来源”消费者偏移存储在目标集群中:

*   消费者偏移量使用一个“假”消费者组存储在目标集群中，只要我们知道消费者组的名称，就可以通过编程方式创建消费者组。“假的”意味着该组没有使用实际的记录，只有存储在 *__consumer_offsets* 主题中的补偿。然而，目标集群上的 *__consumer_offsets* 主题(由“假冒”消费者组管理)是“真实来源”的消费者补偿。
*   有了目标集群上的“假”使用者组，MirrorMaker 中的使用者不依赖于源集群上的 Connect 的内部偏移量跟踪或 *__consumer_offsets* 。
*   与“假”消费者组相关联的消费者偏移量仅由生产者写入目标集群。
*   所有记录都写入一个事务中，就像在单个集群中一样。
*   当 MirrorMaker 重新启动或重新平衡时，它会在目标群集上加载来自 *__consumer_offsets* 主题的初始偏移。

以上想法的结果:

*   如果交易成功，目标集群上的 *__consumer_offsets* 主题将按照当前的一次性框架协议进行更新。
*   如果事务中止，所有数据记录都会被丢弃，目标集群上的 *__consumer_offsets* 主题不会更新。
*   当 MirrorMaker 重新启动/重新启动时，它将在目标群集中存储的最后提交的偏移量处恢复。

https://github.com/apache/kafka/pull/9451 是体验这种乐趣的地方！

# 摘要

多个 Kafka 实例已经成为大规模企业的流行部署，对于许多流用例来说，恰好一次语义是强烈首选的，甚至是必需的。我希望 [KIP-656](https://cwiki.apache.org/confluence/display/KAFKA/KIP-656%3A+MirrorMaker2+Exactly-once+Semantics) 将有助于在 Kafka 生态系统中启用多集群恰好一次语义。