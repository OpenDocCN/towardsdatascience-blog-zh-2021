# 了解如何使用 Kafka Connect 建立从 PostgreSQL 到 Cassandra 的数据管道

> 原文：<https://towardsdatascience.com/data-pipeline-between-postgresql-and-cassandra-using-kafka-connect-77d807bd2f59?source=collection_archive---------16----------------------->

## 关于如何使用 Kafka Connect 进行实时数据同步的教程

![](img/7924977319946e1909e24e5aacbfb9f7.png)

昆腾·德格拉夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[Apache Kafka](https://kafka.apache.org/) 通常作为整体数据架构的核心组件，其他系统将数据注入其中。但是，Kafka(主题)中的数据只有在被其他应用程序使用或被其他系统接收时才有用。尽管使用您选择的语言和客户端 SDK 可以使用 [Kafka 生产者/消费者](https://kafka.apache.org/documentation/#api)API[构建解决方案，但是 Kafka 生态系统中还有其他选择。](https://cwiki.apache.org/confluence/display/KAFKA/Clients)

其中之一是 [Kafka Connect](https://kafka.apache.org/documentation/#connect) ，它是一个平台，以可伸缩和可靠的方式在 [Apache Kafka](https://kafka.apache.org/) 和其他系统之间传输数据。它支持几个现成的连接器，这意味着您不需要定制代码来集成外部系统与 Apache Kafka。

本文将演示如何使用 Kafka 连接器的组合来建立一个数据管道，以便将关系数据库(如 [PostgreSQL](https://www.postgresql.org/) )中的记录实时同步到[Azure Cosmos DB Cassandra API](https://docs.microsoft.com/azure/cosmos-db/cassandra-introduction?WT.mc_id=data-12969-abhishgu)。

该应用程序的代码和配置可在 GitHub repo 中获得—[https://github.com/abhirockzz/postgres-kafka-cassandra](https://github.com/abhirockzz/postgres-kafka-cassandra)

# 这是一个高层次的概述…

…本文中介绍的端到端流程。

针对 PostgreSQL 表中数据的操作(在本例中适用于`INSERT` s)将作为`change data`事件被推送到 Kafka 主题，这要感谢 [Debezium PostgreSQL 连接器](https://debezium.io/documentation/reference/1.2/connectors/postgresql.html)，它是一个 Kafka Connect **source** 连接器——这是使用一种称为 **Change Data Capture** (也称为 CDC)的技术实现的。

## 变更数据捕获:快速入门

这是一种用于跟踪数据库表中行级变化以响应创建、更新和删除操作的技术。这是一个强大的功能，但只有在有办法利用这些事件日志并使其对依赖于该信息的其他服务可用时才是有用的。

[Debezium](https://debezium.io/) 是一个开源平台，构建于不同数据库中可用的变更数据捕获特性之上。它提供了一组 [Kafka Connect 连接器](https://debezium.io/documentation/reference/1.2/connectors/index.html)，这些连接器利用数据库表中的行级更改(使用 CDC)并将它们转换成事件流。这些事件流被发送到 [Apache Kafka](https://kafka.apache.org/) 。一旦变更日志事件在 Kafka 中，它们将对所有下游应用程序可用。

> *这与* [*卡夫卡连接 JDBC 连接器*](https://github.com/confluentinc/kafka-connect-jdbc) 所采用的“轮询”技术不同

## 第二部分

在管道的后半部分， [DataStax Apache Kafka 连接器](https://docs.datastax.com/en/kafka/doc/kafka/kafkaIntro.html)(Kafka Connect**sink**connector)将变更数据事件从 Kafka topic 同步到 Azure Cosmos DB Cassandra API 表。

## 成分

这个例子使用 [Docker Compose](https://docs.docker.com/compose/) 提供了一个可重用的设置。这非常方便，因为它使您能够用一个命令在本地引导所有组件(PostgreSQL、Kafka、Zookeeper、Kafka Connect worker 和示例数据生成器应用程序),并允许迭代开发、实验等更简单的工作流。

> *使用 DataStax Apache Kafka 连接器的特定功能，我们可以将数据推送到多个表中。在这个例子中，连接器将帮助我们将变更数据记录持久化到两个 Cassandra 表中，这两个表可以支持不同的查询需求。*

下面是组件及其服务定义的分类——你可以参考 GitHub repo 中完整的`docker-compose`文件[。](https://github.com/abhirockzz/postgres-kafka-cassandra/blob/master/docker-compose.yaml)

*   卡夫卡和动物园管理员使用 [debezium](https://hub.docker.com/r/debezium/kafka/) 图像。
*   Debezium PostgreSQL Kafka 连接器在[Debezium/connect](https://hub.docker.com/r/debezium/connect)Docker 图片中开箱即用！
*   为了作为 Docker 容器运行，DataStax Apache Kafka 连接器是在 debezium/connect 映像之上构建的。这个图像包括 Kafka 及其 Kafka Connect 库的安装，因此添加定制连接器非常方便。可以参考 [Dockerfile](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/connector/Dockerfile) 。
*   `data-generator`服务将随机生成的(JSON)数据植入 PostgreSQL 的`orders_info`表中。可以参考[GitHub repo](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/data-generator/)中的代码和`Dockerfile`

## 在继续之前，

*   安装[对接器](https://docs.docker.com/get-docker/)和[对接器组合](https://docs.docker.com/compose/install)。
*   [提供 Azure Cosmos DB Cassandra API 帐户](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-dotnet?WT.mc_id=data-12969-abhishgu#create-a-database-account)
*   [使用 cqlsh 或托管 shell 进行验证](https://docs.microsoft.com/azure/cosmos-db/cassandra-support?WT.mc_id=data-12969-abhishgu#hosted-cql-shell-preview)

# 首先创建 Cassandra 键空间和表

> *使用与下面*相同的键空间和表名

```
CREATE KEYSPACE retail WITH REPLICATION = {'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1};CREATE TABLE retail.orders_by_customer (order_id int, customer_id int, purchase_amount int, city text, purchase_time timestamp, PRIMARY KEY (customer_id, purchase_time)) WITH CLUSTERING ORDER BY (purchase_time DESC) AND cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;CREATE TABLE retail.orders_by_city (order_id int, customer_id int, purchase_amount int, city text, purchase_time timestamp, PRIMARY KEY (city,order_id)) WITH cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;
```

# 使用 Docker Compose 启动所有服务

```
git clone https://github.com/abhirockzz/postgres-kafka-cassandracd postgres-kafka-cassandra
```

正如承诺的那样，使用一个命令来启动数据管道的所有服务:

```
docker-compose -p postgres-kafka-cassandra up --build
```

> 下载和启动容器可能需要一段时间:这只是一个一次性的过程。

检查是否所有容器都已启动。在不同的终端中，运行:

```
docker-compose -p postgres-kafka-cassandra ps
```

数据生成器应用程序将开始将数据注入 PostgreSQL 中的`orders_info`表。你也可以做快速检查来确认。使用`[psql](https://www.postgresql.org/docs/current/app-psql.html)`客户端连接到 PostgreSQL 实例...

```
psql -h localhost -p 5432 -U postgres -W -d postgres
```

> *当提示输入密码时，输入* `*postgres*`

…并查询表格:

```
select * from retail.orders_info;
```

此时，您所拥有的只是 PostgreSQL、Kafka 和一个向 PostgreSQL 写入随机数据的应用程序。您需要启动 Debezium PostgreSQL 连接器来将 PostgreSQL 数据发送到 Kafka 主题。

# 启动 PostgreSQL 连接器实例

将连接器配置(JSON)保存到文件示例`pg-source-config.json`

```
{
    "name": "pg-orders-source",
    "config": {
        "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
        "database.hostname": "localhost",
        "database.port": "5432",
        "database.user": "postgres",
        "database.password": "password",
        "database.dbname": "postgres",
        "database.server.name": "myserver",
        "plugin.name": "wal2json",
        "table.include.list": "retail.orders_info",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter"
    }
}
```

要启动 PostgreSQL 连接器实例，请执行以下操作:

```
curl -X POST -H "Content-Type: application/json" --data @pg-source-config.json [http://localhost:9090/connectors](http://localhost:9090/connectors)
```

要检查 Kafka 主题中的变更数据捕获事件，请查看运行 Kafka connect worker 的 Docker 容器:

```
docker exec -it postgres-kafka-cassandra_cassandra-connector_1 bash
```

一旦您进入容器外壳，只需启动通常的 Kafka 控制台消费者进程:

```
cd ../bin./kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic myserver.retail.orders_info --from-beginning
```

> *注意，根据* [*连接器约定*](https://debezium.io/documentation/reference/1.3/connectors/postgresql.html#postgresql-topic-names) ，题目名称为 `*myserver.retail.orders_info*`

*您应该会看到 JSON 格式的变更数据事件。*

*到目前为止一切顺利！数据管道的前半部分似乎在按预期工作。对于下半年，我们需要…*

# *启动 DataStax Apache Kafka 连接器实例*

*将连接器配置(JSON)保存到文件 example，`cassandra-sink-config.json`，并根据您的环境更新属性。*

```
*{
    "name": "kafka-cosmosdb-sink",
    "config": {
        "connector.class": "com.datastax.oss.kafka.sink.CassandraSinkConnector",
        "tasks.max": "1",
        "topics": "myserver.retail.orders_info",
        "contactPoints": "<Azure Cosmos DB account name>.cassandra.cosmos.azure.com",
        "loadBalancing.localDc": "<Azure Cosmos DB region e.g. Southeast Asia>",
        "datastax-java-driver.advanced.connection.init-query-timeout": 5000,
        "ssl.hostnameValidation": true,
        "ssl.provider": "JDK",
        "ssl.keystore.path": "<path to JDK keystore path e.g. <JAVA_HOME>/jre/lib/security/cacerts>",
        "ssl.keystore.password": "<keystore password: it is 'changeit' by default>",
        "port": 10350,
        "maxConcurrentRequests": 500,
        "maxNumberOfRecordsInBatch": 32,
        "queryExecutionTimeout": 30,
        "connectionPoolLocalSize": 4,
        "auth.username": "<Azure Cosmos DB user name (same as account name)>",
        "auth.password": "<Azure Cosmos DB password>",
        "topic.myserver.retail.orders_info.retail.orders_by_customer.mapping": "order_id=value.orderid, customer_id=value.custid, purchase_amount=value.amount, city=value.city, purchase_time=value.purchase_time",
        "topic.myserver.retail.orders_info.retail.orders_by_city.mapping": "order_id=value.orderid, customer_id=value.custid, purchase_amount=value.amount, city=value.city, purchase_time=value.purchase_time",
        "key.converter": "org.apache.kafka.connect.storage.StringConverter",
        "transforms": "unwrap",
        "transforms.unwrap.type": "io.debezium.transforms.ExtractNewRecordState",
        "offset.flush.interval.ms": 10000
    }
}*
```

*启动连接器:*

```
*curl -X POST -H "Content-Type: application/json" --data @cassandra-sink-config.json [http://localhost:8080/connectors](http://localhost:8080/connectors)*
```

*如果一切都已正确配置，连接器将开始从 Kafka 主题向 Cassandra 表泵送数据，我们的端到端管道将开始运行。*

*很明显你会想…*

# *查询 Azure Cosmos DB*

*检查 Azure Cosmos DB 中的 Cassandra 表。如果您在本地安装了`cqlsh`,您可以简单地像这样使用它:*

```
*export SSL_VERSION=TLSv1_2 &&\
export SSL_VALIDATE=false &&\
cqlsh.py <cosmosdb account name>.cassandra.cosmos.azure.com 10350 -u <cosmosdb username> -p <cosmosdb password> --ssl*
```

*如果没有，Azure 门户中的[托管的 CQL shell](https://docs.microsoft.com/azure/cosmos-db/cassandra-support?WT.mc_id=data-12969-abhishgu#hosted-cql-shell-preview) 也相当好用！*

*以下是您可以尝试的一些查询:*

```
*select count(*) from retail.orders_by_customer;
select count(*) from retail.orders_by_city;select * from retail.orders_by_customer;
select * from retail.orders_by_city;select * from retail.orders_by_city where city='Seattle';
select * from retail.orders_by_customer where customer_id = 10;*
```

# *结论*

*总之，您学习了如何使用 Kafka Connect 在 PostgreSQL、Apache Kafka 和 Azure Cosmos DB 之间进行实时数据集成。由于样品采用基于 Docker 容器的方法，您可以根据自己的独特要求轻松定制，冲洗并重复！*

## *以下主题可能也会引起您的兴趣…*

*如果您觉得这很有用，您可能还想浏览以下资源:*

*   *[使用 Blitzz 将数据从 Oracle 迁移到 Azure Cosmos DB Cassandra API](https://docs.microsoft.com/azure/cosmos-db/oracle-migrate-cosmos-db-blitzz?WT.mc_id=data-12969-abhishgu)*
*   *[使用 Azure Databricks 将数据从 Cassandra 迁移到 Azure Cosmos DB Cassandra API 帐户](https://docs.microsoft.com/azure/cosmos-db/cassandra-migrate-cosmos-db-databricks?WT.mc_id=data-12969-abhishgu)*
*   *[快速入门:构建一个 Java 应用程序来管理 Azure Cosmos DB Cassandra API 数据(v4 驱动程序)](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-java-v4?WT.mc_id=data-12969-abhishgu)*
*   *Azure Cosmos DB Cassandra API 支持的 Apache Cassandra 特性*
*   *[快速入门:用 Python SDK 和 Azure Cosmos DB 构建 Cassandra 应用](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-python?WT.mc_id=data-12969-abhishgu)*