# Azure Cosmos DB:用例与权衡

> 原文：<https://towardsdatascience.com/azure-cosmos-db-use-cases-and-trade-offs-5a65727daaa9?source=collection_archive---------26----------------------->

## 如何在 Azure Cosmos DB 中选择正确的数据模型？SQL，MongoDB，Cassandra，图还是表？

![](img/f87a5226d1ec9f1472393a49122cd35f.png)

吉列尔莫·费拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction?WT.mc_id=data-12713-abhishgu) 是一个完全托管的、可弹性伸缩的全球分布式数据库，采用多模型方法，为您提供使用文档、键值、宽列或基于图形的数据的能力。

在这篇博客中，我们将深入探讨多模型功能，并探索可用于存储和访问数据的选项。希望它能帮助你在正确的 API 上做出明智的决定。

*   核心(SQL) API:结合了 SQL 查询功能的 NoSQL 文档存储的灵活性。
*   [MongoDB API](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction?WT.mc_id=data-12713-abhishgu) :支持 MongoDB wire 协议，这样现有的 MongoDB 客户端可以继续使用 Azure Cosmos DB，就像它们运行在实际的 MongoDB 数据库上一样。
*   [Cassandra API](https://docs.microsoft.com/azure/cosmos-db/cassandra-introduction?WT.mc_id=data-12713-abhishgu) :支持 Cassandra wire 协议，因此现有的符合 CQLv4 的 Apache 驱动程序可以继续与 Azure Cosmos DB 一起工作，就像它们是在实际的 Cassandra 数据库上运行一样。
*   [Gremlin API](https://docs.microsoft.com/azure/cosmos-db/graph-introduction?WT.mc_id=data-12713-abhishgu) :用 Apache TinkerPop(一个图形计算框架)和 Gremlin 查询语言支持图形数据。
*   [Table API](https://docs.microsoft.com/azure/cosmos-db/table-introduction?WT.mc_id=data-12713-abhishgu) :为针对 Azure Table 存储编写的应用提供高级功能。

> *您可以进一步了解一些* [*的主要优势*](https://docs.microsoft.com/azure/cosmos-db/introduction?WT.mc_id=data-12713-abhishgu#key-benefits)

使用 Azure Cosmos DB 的一些常见场景(无论如何不是一个详尽的列表)包括:

*   **现代游戏服务**需要提供定制和个性化的内容，如游戏内统计数据、社交媒体整合和高分排行榜。作为一个完全托管的产品，Azure Cosmos DB 需要最少的设置和管理，以允许快速迭代，并缩短上市时间。
*   **电子商务平台**和零售应用程序存储目录数据，并用于订单处理管道中的事件采购。
*   **零售**用例存储产品目录信息，这些信息在结构上可以是动态的。

…还有更多！

# 使用哪种 API，何时使用？

没有完美的公式！有时，选择是明确的，但是其他场景可能需要一些分析。

要考虑的一个相当明显的问题是，是否存在通过 wire 协议使用任何受支持的 API 的现有应用程序(例如 Cassandra 和 MongoDB)？如果答案是肯定的，您应该考虑使用特定的 Azure Cosmos DB API，因为这将减少您的迁移任务，并充分利用您团队中以前的经验。

如果您希望模式发生很大变化，您可能希望利用文档数据库，使 Core (SQL)成为一个不错的选择(尽管也应该考虑 MongoDB API)。

如果您的数据模型由实体之间的关系和相关元数据组成，那么您最好使用 Azure Cosmos DB Gremlin API 中的图形支持。

如果你目前正在使用 [Azure Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview?WT.mc_id=data-12713-abhishgu) ，Core (SQL) API 将是一个更好的选择，因为它提供了更丰富的查询体验，并改进了 Table API 的索引。如果你不想重写你的应用，考虑[迁移到 Azure Cosmos DB Table API](https://docs.microsoft.com/azure/cosmos-db/table-api-faq?WT.mc_id=data-12713-abhishgu#table-api-vs-table-storage) 。

让我们看看每个 API 并应用其中的一些。

# 核心(SQL) API:两全其美

核心(SQL) API 是默认的 Azure Cosmos DB API。您可以使用 [JSON 文档存储数据来表示您的数据](https://docs.microsoft.com/azure/cosmos-db/modeling-data?WT.mc_id=data-12713-abhishgu)，但是有几种方法可以检索它:

*   [SQL 查询](https://docs.microsoft.com/azure/cosmos-db/sql-query-getting-started?WT.mc_id=data-12713-abhishgu):使用结构化查询语言(SQL)作为 JSON 查询语言编写查询
*   点读取:在单个条目 ID 和分区键上进行键/值样式的查找(这些比 SQL 查询更便宜、更快)

对于许多应用程序来说，半结构化数据模型可以提供他们所需要的灵活性。例如，具有大量产品类别的电子商务设置。您将需要添加新的产品类别和支持操作(搜索，排序等。)跨多个产品属性。您可以用关系数据库对此进行建模，但是，不断发展的产品类别可能需要停机来更新表模式、查询和数据库。

使用 Core (SQL ),引入新产品类别就像为新产品添加文档一样简单，无需更改模式或停机。Azure Cosmos DB MongoDB API 也适合这些需求，但是 Core (SQL) API 具有优势，因为它在灵活的数据模型之上支持类似 SQL 的查询。

考虑一个博客平台的例子，用户可以创建帖子，喜欢这些帖子并添加评论。

你可以这样表示一个`post`:

```
{
    "id": "<post-id>",
    "type": "post",
    "postId": "<post-id>",
    "userId": "<post-author-id>",
    "userUsername": "<post-author-username>",
    "title": "<post-title>",
    "content": "<post-content>",
    "commentCount": <count-of-comments>,
    "likeCount": <count-of-likes>,
    "creationDate": "<post-creation-date>"
}
```

以及`comments`和`likes`:

```
{
    "id": "<comment-id>",
    "type": "comment",
    "postId": "<post-id>",
    "userId": "<comment-author-id>",
    "userUsername": "<comment-author-username>",
    "content": "<comment-content>",
    "creationDate": "<comment-creation-date>"
}{
    "id": "<like-id>",
    "type": "like",
    "postId": "<post-id>",
    "userId": "<liker-id>",
    "userUsername": "<liker-username>",
    "creationDate": "<like-creation-date>"
}
```

为了处理喜欢和评论，您可以使用一个[存储过程](https://docs.microsoft.com/en-us/azure/cosmos-db/stored-procedures-triggers-udfs):

```
function createComment(postId, comment) {
  var collection = getContext().getCollection(); collection.readDocument(
    `${collection.getAltLink()}/docs/${postId}`,
    function (err, post) {
      if (err) throw err; post.commentCount++;
      collection.replaceDocument(
        post._self,
        post,
        function (err) {
          if (err) throw err; comment.postId = postId;
          collection.createDocument(
            collection.getSelfLink(),
            comment
          );
        }
      );
    })
}
```

> *如果你想了解更多，请参考* [*在 Azure Cosmos DB 中使用 JSON*](https://docs.microsoft.com/azure/cosmos-db/sql-query-working-with-json?WT.mc_id=data-12713-abhishgu)

# 从现有的 MongoDB 实例迁移

Azure Cosmos DB 为 MongoDB 实现了 [wire 协议，它允许与原生 MongoDB 客户端 SDK、驱动程序和工具透明兼容。这意味着任何理解这个协议版本的 MongoDB 客户端驱动程序(以及 Studio 3T](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-feature-support-36) 等[现有工具)都应该能够本地连接到 Cosmos DB。](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-feature-support-36)

例如，如果您有一个现有的 MongoDB 数据库作为采购订单处理应用程序的数据存储，以捕获不同格式的失败和部分订单、履行数据、运输状态。由于数据量不断增加，您希望迁移到可扩展的基于云的解决方案，并继续使用 MongoDB——使用 Azure Cosmos DB 的 API for MongoDB 是非常合理的。但是，您希望对现有应用程序进行最少的代码更改，并在尽可能短的停机时间内迁移当前数据。

借助 [Azure 数据库迁移服务](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db-online?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json&WT.mc_id=data-12713-abhishgu)，您可以执行*在线*(最小停机时间)迁移，并根据您的需求灵活扩展为您的 Cosmos 数据库提供的吞吐量和存储，并且只需为您需要的吞吐量和存储付费。这导致了显著的成本节约。

> *参考*[](https://docs.microsoft.com/azure/cosmos-db/mongodb-pre-migration?WT.mc_id=data-12713-abhishgu)**和* [*迁移后*](https://docs.microsoft.com/azure/cosmos-db/mongodb-post-migration?WT.mc_id=data-12713-abhishgu) *指南。**

*一旦迁移了数据，您就可以继续使用现有的应用程序。下面是一个使用 [MongoDB 的例子。网络驱动](https://docs.mongodb.com/ecosystem/drivers/csharp/):*

*初始化客户端:*

```
*MongoClientSettings settings = new MongoClientSettings();
settings.Server = new MongoServerAddress(host, 10255);
settings.UseSsl = true;
settings.SslSettings = new SslSettings();
settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
MongoIdentityEvidence evidence = new PasswordEvidence(password);settings.Credential = new MongoCredential("SCRAM-SHA-1", identity, evidence);MongoClient client = new MongoClient(settings);*
```

*检索数据库和集合:*

```
*private string dbName = "Tasks";
private string collectionName = "TasksList";var database = client.GetDatabase(dbName);
var todoTaskCollection = database.GetCollection<MyTask>(collectionName);*
```

*将任务插入集合:*

```
*public void CreateTask(MyTask task)
 {
     var collection = GetTasksCollectionForEdit();
     try
     {
         collection.InsertOne(task);
     }
     catch (MongoCommandException ex)
     {
         string msg = ex.Message;
     }
 }*
```

*检索所有任务:*

```
*collection.Find(new BsonDocument()).ToList();*
```

*您还应该仔细考虑如何将 [MongoDB 写/读问题映射到 Azure Cosmos 一致性级别](https://docs.microsoft.com/azure/cosmos-db/mongodb-consistency?WT.mc_id=data-12713-abhishgu)、[管理索引](https://docs.microsoft.com/azure/cosmos-db/mongodb-indexing?WT.mc_id=data-12713-abhishgu)并利用[更改提要支持](https://docs.microsoft.com/azure/cosmos-db/mongodb-change-streams?tabs=javascript&WT.mc_id=data-12713-abhishgu)。*

*请注意，如果不需要重用现有代码和从现有 MongoDB 数据库导入数据，那么核心(SQL) API 将是一个合适的选择。*

# *用 Cassandra 处理实时数据*

*如果您的应用程序需要处理大量的实时数据，Apache Cassandra 是一个理想的选择。使用 Azure Cosmos DB Cassandra API，您可以使用符合 CQ v4 的现有 Apache 驱动程序，并且在大多数情况下，您应该能够通过更改连接字符串，从使用 Apache Cassandra 切换到使用 Azure Cosmos DB 的 Cassandra API。也可以继续使用基于 Cassandra 的工具，比如`cqlsh`。*

*对于高级分析用例，您应该将 [Azure Cosmos DB Cassandra API 与 Apache Spark](https://docs.microsoft.com/azure/cosmos-db/cassandra-spark-generic?WT.mc_id=data-12713-abhishgu) 相结合。你可以使用熟悉的[Spark connector for Cassandra](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector)来连接 Azure Cosmos DB Cassandra API。此外，您还需要来自 Azure Cosmos DB 的[Azure-Cosmos-Cassandra-spark-helper](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar)helper 库，用于定制连接工厂和重试策略等功能。*

> **可以从*[*Azure data bricks*](https://docs.microsoft.com/azure/cosmos-db/cassandra-spark-databricks?WT.mc_id=data-12713-abhishgu)*以及* [*Spark on YARN 使用 HDInsight*](https://docs.microsoft.com/azure/cosmos-db/cassandra-spark-hdinsight?WT.mc_id=data-12713-abhishgu) 访问 Azure Cosmos DB Cassandra API*

*要连接到 Cosmos DB，您可以像这样使用 Scala API:*

```
*import org.apache.spark.sql.cassandra._
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector
import com.microsoft.azure.cosmosdb.cassandraspark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")*
```

*可以进行聚合，如`average`、`min/max`、`sum`等。：*

```
*spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(avg("book_price"))
  .showspark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_id","book_price")
  .agg(min("book_price"))
  .showspark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(sum("book_price"))
  .show*
```

# *使用 Gremlin (Graph) API 连接一切*

*作为个性化客户体验的一部分，您的应用程序可能需要在网站上提供产品推荐。比如“买了产品 X 的人也买了产品 Y”。您的用例可能还需要预测客户行为，或者将人们与具有相似兴趣的人联系起来。*

*[Azure Cosmos DB 支持 Apache Tinkerpop 的图遍历语言](https://docs.microsoft.com/azure/cosmos-db/gremlin-support?WT.mc_id=data-12713-abhishgu)(即 Gremlin)。有许多用例会产生与缺乏灵活性和关系方法相关的常见问题。例如，管理连接的社交网络、地理空间使用案例，如在一个区域内查找感兴趣的位置或定位两个位置之间的最短/最佳路线，或将物联网设备之间的网络和连接建模为图表，以便更好地了解设备和资产的状态。除此之外，您可以继续享受所有 Azure Cosmos DB APIs 共有的功能，如全球分布、存储和吞吐量的弹性扩展、自动索引和查询以及可调的一致性级别。*

*例如，您可以使用以下命令将产品的三个顶点和相关采购的两条边添加到图形中:*

```
*g.addV('product').property('productName', 'Industrial Saw').property('description', 'Cuts through anything').property('quantity', 261)
g.addV('product').property('productName', 'Belt Sander').property('description', 'Smoothes rough edges').property('quantity', 312)
g.addV('product').property('productName', 'Cordless Drill').property('description', 'Bores holes').property('quantity', 647)g.V().hasLabel('product').has('productName', 'Industrial Saw').addE('boughtWith').to(g.V().hasLabel('product').has('productName', 'Belt Sander'))
g.V().hasLabel('product').has('productName', 'Industrial Saw').addE('boughtWith').to(g.V().hasLabel('product').has('productName', 'Cordless Drill'))*
```

*然后，您可以查询随“工业锯”一起购买的其他产品:*

```
*g.V().hasLabel('product').has('productName', 'Industrial Saw').outE('boughtWith')*
```

*下面是结果的样子:*

```
*[
  {
    "id": "6c69fba7-2f76-421f-a24e-92d4b8295d67",
    "label": "boughtWith",
    "type": "edge",
    "inVLabel": "product",
    "outVLabel": "product",
    "inV": "faaf0997-f5d8-4d01-a527-ae29534ac234",
    "outV": "a9b13b8f-258f-4148-99c0-f71b30918146"
  },
  {
    "id": "946e81a9-8cfa-4303-a999-9be3d67176d5",
    "label": "boughtWith",
    "type": "edge",
    "inVLabel": "product",
    "outVLabel": "product",
    "inV": "82e1556e-f038-4d7a-a02a-f780a2b7215c",
    "outV": "a9b13b8f-258f-4148-99c0-f71b30918146"
  }
]*
```

*除了传统的客户端，你可以使用图形[批量执行程序。NET 库](https://docs.microsoft.com/en-us/azure/cosmos-db/bulk-executor-overview)在 Azure Cosmos DB Gremlin API 中执行批量操作。与使用 Gremlin 客户端相比，使用此功能将提高数据迁移效率。传统上，使用 Gremlin 插入数据将要求应用程序在需要验证、评估、然后执行以创建数据的时候发送一个查询。批量执行器库将处理应用程序中的验证，并为每个网络请求一次发送多个图形对象。以下是如何创建顶点和边的示例:*

```
*IBulkExecutor graphbulkExecutor = new GraphBulkExecutor(documentClient, targetCollection);BulkImportResponse vResponse = null;
BulkImportResponse eResponse = null;try
{
    vResponse = await graphbulkExecutor.BulkImportAsync(
            Utils.GenerateVertices(numberOfDocumentsToGenerate),
            enableUpsert: true,
            disableAutomaticIdGeneration: true,
            maxConcurrencyPerPartitionKeyRange: null,
            maxInMemorySortingBatchSize: null,
            cancellationToken: token); eResponse = await graphbulkExecutor.BulkImportAsync(
            Utils.GenerateEdges(numberOfDocumentsToGenerate),
            enableUpsert: true,
            disableAutomaticIdGeneration: true,
            maxConcurrencyPerPartitionKeyRange: null,
            maxInMemorySortingBatchSize: null,
            cancellationToken: token);
}
catch (DocumentClientException de)
{
    Trace.TraceError("Document client exception: {0}", de);
}
catch (Exception e)
{
    Trace.TraceError("Exception: {0}", e);
}*
```

*虽然可以使用核心(SQL) API 作为 JSON 文档来建模和存储这些数据，但是它不适合需要确定实体(例如产品)之间关系的查询。*

> **您可能想要探索 Azure Cosmos DB Gremlin API 的* [*图形数据建模*](https://docs.microsoft.com/azure/cosmos-db/graph-modeling?WT.mc_id=data-12713-abhishgu) *以及* [*数据分区*](https://docs.microsoft.com/azure/cosmos-db/graph-partitioning?WT.mc_id=data-12713-abhishgu) *。**

# *Azure 桌面储物系列！*

*如果你已经有了为 Azure Table storage 编写的应用程序，它们可以通过使用 Table API 迁移到 Azure Cosmos DB，而不需要修改代码，并利用高级功能。*

*将您的数据库从 Azure Table Storage 迁移到 Azure Cosmos DB，以较低的吞吐量，可以在延迟(一位数毫秒的读写)、吞吐量、全局分布、全面的 SLA 以及成本方面带来很多好处(您可以使用[基于消耗的](https://docs.microsoft.com/azure/cosmos-db/serverless?WT.mc_id=data-12713-abhishgu)或[供应容量](https://docs.microsoft.com/azure/cosmos-db/set-throughput?WT.mc_id=data-12713-abhishgu)模式。将表数据存储在 Cosmos DB 中会自动索引所有属性(没有索引管理开销),而表存储只允许对分区和行键进行索引。*

*Table API 具有可用于的客户端 SDK。例如，对于 Java 客户端，使用连接字符串来存储表端点和凭证:*

```
*public static final String storageConnectionString =
    "DefaultEndpointsProtocol=https;" + 
    "AccountName=your_cosmosdb_account;" + 
    "AccountKey=your_account_key;" + 
    "TableEndpoint=https://your_endpoint;" ;*
```

*..并执行 CRUD(创建、读取、更新、删除)操作，如下所示:*

```
*try
{
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString); CloudTableClient tableClient = storageAccount.createCloudTableClient(); CloudTable cloudTable = tableClient.getTableReference("customers");
    cloudTable.createIfNotExists(); for (String table : tableClient.listTables())
    {
        System.out.println(table);
    } CustomerEntity customer = ....;
    TableOperation insert = TableOperation.insertOrReplace(customer);
    cloudTable.execute(insert); TableOperation retrieve =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);
    CustomerEntity result =
        cloudTable.execute(retrieve).getResultAsType(); TableOperation del = TableOperation.delete(jeff);
    cloudTable.execute(del);
}*
```

> **参考* [*表 API 哪里和 Azure 表存储行为不一样？*](https://docs.microsoft.com/azure/cosmos-db/table-api-faq?WT.mc_id=data-12713-abhishgu) *了解详情。**

# *结论*

*Azure Cosmos DB supports 在 API 方面提供了许多选项和灵活性，并且根据您的用例和需求有其优缺点。虽然核心(SQL)非常通用，并且适用于广泛的场景，但是如果您的数据能够更好地用关系来表示，那么 Gremlin (graph) API 是一个合适的选择。如果您有使用 Cassandra 或 MongoDB 的现有应用程序，那么将它们迁移到各自的 Azure Cosmos DB APIs 提供了一条阻力最小且具有额外好处的路径。如果您想从 Azure 表存储中迁移，并且不想重构您的应用程序以使用核心(SQL) API，请记住，您可以选择 Azure Cosmos DB 表 API，它可以提供与表存储的 API 兼容性以及诸如保证高可用性、自动二级索引等功能！*