# 卡夫卡笔下的死信队列(DLQ)

> 原文：<https://towardsdatascience.com/dead-letter-queue-dlq-in-kafka-29418e0ec6cf?source=collection_archive---------12----------------------->

## 编程；编排

## 卡夫卡 DLQ 简介及其 Python 实现

![](img/cf79df4936ec7a6986b72c6d84b54ace.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3217049) 的 [DaKub](https://pixabay.com/users/dakub-8222964/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3217049)

D

![](img/c4e40696db49c94892b2e4b39a863a44.png)

图片作者( [Jimit Dholakia](https://www.linkedin.com/in/jimit105/) )

# 装置

Python 中有各种库可以用来连接 Kafka 集群。其中一些是:

1.  [卡夫卡-巨蟒](https://kafka-python.readthedocs.io/en/master/)
2.  [汇流——卡夫卡](https://docs.confluent.io/clients-confluent-kafka-python/current/index.html)
3.  [PyKafka](https://pykafka.readthedocs.io/en/latest/)

我将使用 kafka-python 连接到 kafka 集群，并创建 Kafka 生产者和消费者客户端。

使用 pip 安装 kafka-python:

```
pip install kafka-python
```

# 履行

我们将首先导入必要的包，并定义引导服务器、主主题名和 DLQ 主题名，以创建 Kafka 生产者和消费者的实例。

```
from kafka import KafkaProducer, KafkaConsumer
import json
bootstrap_servers = ['localhost:9092']
primary_topic = 'primary-topic-name'
dlq_topic = 'dlq-topic-name'
```

现在，让我们为 DLQ 主题创建一个生成器，错误的消息将被发送到这个生成器中。

```
dlq_producer = KafkaProducer(
    bootstrap_servers=bootstrap_servers,
    value_serializer=lambda x: x.encode('utf-8'),
    acks='all'
)
```

接下来，我们将创建 Kafka Consumer 来消费主主题中的消息。

```
consumer = KafkaConsumer(
    primary_topic,
    bootstrap_servers=bootstrap_servers,
    auto_offset_reset='latest',
    enable_auto_commit=True,
    value_deserializer=lambda x: x.decode('utf-8')
)
```

现在，我们将把代码放在 try-except 块中。如果收到的消息不是 JSON 格式或者发生任何异常，那么消息将被发送到 DLQ 主题。

```
for msg in consumer:
    print(f'\nReceived:\nPartition: {msg.partition} \tOffset: {msg.offset}\tValue: {msg.value}')

    try:
        data = json.loads(msg.value)
        print('Data Received:', data)

    except:
        print(f'Value {msg.value} not in JSON format')
        dlq_producer.send(dlq_topic, value=msg.value)
        print('Message sent to DLQ Topic')
```

# 结论

创建 DLQ 主题有助于在不干扰其他消息的情况下识别格式错误的消息。DLQ 主题还帮助我们分析格式错误的消息，并可用于报告目的。

## 资源

这篇文章的代码片段可以在我的 GitHub 页面上找到。

## 参考

*   [https://kafka-python.readthedocs.io/en/master/install.html](https://kafka-python.readthedocs.io/en/master/install.html)

## 让我们连接

LinkedIn:[https://www.linkedin.com/in/jimit105/](https://www.linkedin.com/in/jimit105/)
GitHub:[https://github.com/jimit105](https://github.com/jimit105)Twitter:[https://twitter.com/jimit105](https://twitter.com/jimit105)