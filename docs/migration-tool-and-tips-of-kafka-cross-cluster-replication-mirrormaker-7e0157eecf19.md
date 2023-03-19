# Kafka 跨集群复制的迁移工具和技巧:MirrorMaker

> 原文：<https://towardsdatascience.com/migration-tool-and-tips-of-kafka-cross-cluster-replication-mirrormaker-7e0157eecf19?source=collection_archive---------25----------------------->

## 从其他复制工具迁移到 MirrorMaker 的开源工具

![](img/a950fd0b656381c1fd7abbc9d664f430.png)

[图片](https://unsplash.com/photos/fnkrDm7kL8g)提供:[杰瑞米·毕晓普](https://unsplash.com/@jeremybishop)在 [unsplash](https://unsplash.com/)

这篇文章是关于从其他**跨集群** Kafka 复制工具迁移到新的 [MirrorMaker](https://github.com/apache/kafka/tree/trunk/connect/mirror) (或“MirrorMaker 2”)的工具和技巧

新的 MirrorMaker 有几个优点。仅举几个例子:

*   在 Apache Kafka 生态系统中始终开源
*   由拥有大量不同用户群的[开放社区](https://lists.apache.org/list.html?users@kafka.apache.org)提供支持
*   其他工具**不**支持的关键特性，例如，精确一次语义，参见[我以前的博客](https://ning-zhang.medium.com/exactly-once-semantics-across-multiple-kafka-clusters-is-possible-9560b08a7dc6)

# 问题

最近，来自社区的用户已经从 [Confluent Replicator](https://docs.confluent.io/5.5.0/connect/kafka-connect-replicator/index.html) (一个企业商业“跨集群”复制工具)迁移到 MirrorMaker 和[，他们面临以下问题](https://lists.apache.org/thread.html/r928922036031df0db11a873ac076dae071a57a7f638bcb5911d34580%40%3Cusers.kafka.apache.org%3E):

> 在相同主题上启动 MirrorMaker 时，已由汇合复制程序复制的消息将再次被复制。这不应该发生，因为消息在目标集群上被复制。

由于合流复制器和 MirrorMaker 都是作为 [Kafka Connect](https://kafka.apache.org/documentation/#connect) 的“连接器”构建的，或者更具体地说是作为[源连接器](https://github.com/apache/kafka/tree/trunk/connect/api/src/main/java/org/apache/kafka/connect/source)构建的，因此它们维护一个内部“偏移主题”来跟踪消费者偏移，并在合流复制器或 MirrorMaker 重新启动时加载。

对于合流复制器，偏移主题默认为“*连接-偏移”*。然而，在 MirrorMaker 中，偏移主题被命名为"*mm2-offsets . primary . internal "*。

另一个区别是内容格式。对于汇合复制程序，邮件内容如下所示:

```
[“replicator”,{“topic”:”foo”,”partition”:2}] {“offset”:1}
```

*(“复制器”是合流复制器的默认消费群名称)*

对于 MirrorMaker，消息上下文如下所示:

```
["MirrorSourceConnector",{"cluster":"primary","partition":1,"topic":"foo”}] {"offset":10}
```

*(“MirrorSourceConnector”是 MirrorMaker 的默认消费群名称)*

# 解决

使用不同偏移主题调整开源 MirrorMaker 并适应不同的消息格式听起来可能是可行的。但从实践来看，这需要对 MirrorMaker 有深入的了解和代码更改。

正如[社区帖子](https://lists.apache.org/thread.html/r928922036031df0db11a873ac076dae071a57a7f638bcb5911d34580%40%3Cusers.kafka.apache.org%3E)所讨论的，这里有一个非侵入性的、简洁的解决方案:

```
For each topic and partition, whenever a new message is replicated a new message with same key but increased offset is produced to the connect-offsets topic, convert the key of this message to Mirror Maker format and produce it in the internal "offset topic" of Mirror Maker.Key : ["replicator-group",{"topic":"TEST","partition":0}]
Value: {"offset":24}After posting the message, once the mirror maker is restarted, it will read the internal topic to get the latest offset of that topic for which the message has to be replicated and this way we can ensure no duplicate messages are replicated.Key: ["mm-group",{"cluster":"primary","partition":0,"topic":"TEST"}]
Value: {"offset":24}
```

以下代码片段通过利用本机*Kafka-console-consumer . sh*和*Kafka-console-producer . sh*([Apache Kafka](https://github.com/apache/kafka/tree/trunk/bin)的一部分)实现了上述想法，带来了几个好处:

*   没有第三方依赖
*   与编程语言和操作系统无关
*   更好的与卡夫卡融合(如 [*卡夫卡-控制台-*](https://github.com/apache/kafka/tree/trunk/bin) **)。sh 应始终工作*
*   易于执行和监控，例如玉米作业

自述在这里:[https://github.com/ning2008wisc/mm2-migration](https://github.com/ning2008wisc/mm2-migration)

# 结论

社区主题中提出的想法简洁明了，可以应用于其他类似的涉及 Apache Kafka 的迁移场景。在未来，更多有用的工具和技巧将在这里讨论。敬请关注！