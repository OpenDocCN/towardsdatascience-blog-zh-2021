# 开始使用 Azure Data Explorer 和 Azure Synapse Analytics 进行大数据处理

> 原文：<https://towardsdatascience.com/getting-started-with-azure-data-explorer-and-azure-synapse-analytics-for-big-data-processing-25500821e370?source=collection_archive---------12----------------------->

## 通过实际例子，了解如何利用这些服务之间的集成，通过 Apache Spark 处理数据

[Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/data-explorer-overview?WT.mc_id=data-21329-abhishgu) 是一种完全托管的数据分析服务，可以处理来自任何数据源的大量不同数据，如网站、应用程序、物联网设备等。Azure Data Explorer 使获取这些数据变得简单，并使您能够在几秒钟内对数据进行复杂的即席查询。它可以在几分钟内快速扩展到万亿字节的数据，允许快速迭代数据探索，以发现相关的见解。它已经通过[数据源和数据接收器连接器](https://github.com/Azure/azure-kusto-spark)与 Apache Spark work 集成，用于为近实时数据处理、数据存档、机器学习等解决方案提供动力。

![](img/a5f65c59f33eb6fc0f719d019291517f.png)

面向大数据工作负载的 Azure Data Explorer

得益于对该解决方案的扩展，Azure Data Explorer 可作为 Azure Synapse Analytics 中的链接服务，允许 Azure Data Explorer 和 Azure Synapse 中的 Apache Spark 池之间的无缝集成。

[Azure Synapse](https://docs.microsoft.com/azure/synapse-analytics/overview-what-is?WT.mc_id=data-21329-abhishgu) 汇集了企业数据仓库中使用的 SQL 技术、用于大数据的 Spark 技术、用于数据集成和 ETL/ELT 的管道，以及与 Power BI、CosmosDB 和 AzureML 等其他 Azure 服务的深度集成。

这篇博文是一个入门指南，展示了 Azure Data Explorer 和 Azure Synapse 之间的集成。它包括:

*   如何使用 Spark 和 Azure Synapse 处理 Azure Data Explorer 中的现有数据？
*   使用 Spark 处理流和批处理数据，并将其写回 Azure data explore。

> *笔记本在本次 GitHub repo 中有售—*[*https://GitHub . com/abhirockzz/synapse-azure-data-explorer-101*](https://github.com/abhirockzz/synapse-azure-data-explorer-101)

要学习，你需要的只是一个 Azure 账户(你可以[免费获得一个](https://azure.microsoft.com/free/?WT.mc_id=data-21329-abhishgu))。一旦你准备好了，就进入下一部分吧！

# 初始设置和配置

首先创建一个 [Azure Synapse workspace](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace?WT.mc_id=data-21329-abhishgu) 以及一个 [Apache Spark pool](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-apache-spark-pool-portal?WT.mc_id=data-21329-abhishgu) 。然后，[创建一个 Azure Data Explorer 集群和数据库](https://docs.microsoft.com/azure/data-explorer/create-cluster-database-portal?WT.mc_id=data-21329-abhishgu)

## 调整摄取策略

在摄取过程中，Azure Data Explorer 试图通过在等待摄取时将小的入口数据块批处理在一起来优化吞吐量，这由[摄取批处理策略](https://docs.microsoft.com/azure/data-explorer/kusto/management/batchingpolicy?WT.mc_id=data-21329-abhishgu)控制。默认策略值为:最大延迟时间为 5 分钟，1000 个项目，批处理的总大小为 1G。这意味着从触发数据接收到准备好进行查询之间有一定的延迟。好的一面是，该策略可以根据需要进行微调。

出于演示的目的，我们专注于让我们的数据尽快可供查询。因此，您应该使用`30`秒的`MaximumBatchingTimeSpan`值来更新策略

```
.alter database adxdb policy ingestionbatching @'{"MaximumBatchingTimeSpan": "00:00:30"}'
```

> 将此策略设置为非常小的值会增加成本并降低性能，这只是为了演示

## 从 Synapse 连接到 Azure 数据浏览器

在 Azure Synapse Analytics 中，**链接服务**是您定义与其他服务的连接信息的地方。您可以使用 Azure Synapse 分析工作区为 Azure Data Explorer [创建链接服务。](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-connect-azure-data-explorer?WT.mc_id=data-21329-abhishgu#connect-an-azure-data-explorer-database-to-an-azure-synapse-workspace)

![](img/a97097ea3f32a5b628c7c26fa1dd7519.png)

创建链接服务(按作者分类的图像)

> 与服务主体相反，托管身份正被用作**身份验证方法**

创建链接服务后，它将显示在列表中:

![](img/a718be3e2d6188e64f878026056c65f7.png)

Azure Data Explorer 链接服务(图片由作者提供)

好了，你都准备好了！

如果你已经在使用 Azure Data Explorer，很可能你已经有了大量的数据，等待处理！所以让我们从探索这方面开始。

# 在 Azure Data Explorer 中处理现有数据

数据接收是 Azure Data Explorer 等大数据分析服务的关键组件。难怪，它支持多种方式[来从各种来源获取数据。虽然对摄取技术和选项进行了详细的讨论，但是欢迎您在文档中阅读。](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview?WT.mc_id=data-21329-abhishgu#ingestion-methods-and-tools)

为了节省时间，让我们手动[摄取数据](https://docs.microsoft.com/azure/data-explorer/ingest-sample-data?WT.mc_id=data-21329-abhishgu)。不要让“手动”这个词误导你。相当简单快捷！

首先在数据库中创建一个表(姑且称之为`StormEvents_1`):

```
.create table StormEvents_1 (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)
```

…并将 CSV 数据接收到表格中(直接从 Blob 存储中):

```
.ingest into table StormEvents_1 'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?sv=2019-12-12&ss=b&srt=o&sp=r&se=2022-09-05T02:23:52Z&st=2020-09-04T18:23:52Z&spr=https&sig=VrOfQMT1gUrHltJ8uhjYcCequEcfhjyyMX%2FSc3xsCy4%3D' with (ignoreFirstRecord=true)
```

> *如果你觉得这个技术有用，我鼓励你也试试* [*一键摄取*](https://docs.microsoft.com/azure/data-explorer/one-click-ingestion-new-table?WT.mc_id=data-21329-abhishgu) *！*

消化可能需要一分钟左右的时间。确认数据是否可用，并执行简单的查询:

```
.show ingestion failuresStormEvents_1| count 
StormEvents_1| take 5 StormEvents_1| take 5 | project StartTime, EndTime, State, EventType, DamageProperty, Source
```

`StormEvents_1`表提供了一些关于美国发生的风暴的信息。看起来是这样的:

![](img/0ccd230ed18f78ea6e87aa5dca512783.png)

`Azure Data Explorer table data` (图片作者提供)

对于后续步骤，您可以将代码直接粘贴到 Azure Synapse Analytics 中的 [Synapse Studio 笔记本中，或者将](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-development-using-notebooks?tabs=classical&WT.mc_id=data-21329-abhishgu)[该笔记本](https://github.com/abhirockzz/synapse-azure-data-explorer-101/blob/master/notebooks/synapse-adx-read.ipynb)导入到工作区中。

![](img/a6de2e355e342b28fe6b45f7045b8b52.png)

Azure Synapse 工作空间笔记本(图片由作者提供)

从简单的事情开始:

```
kustoDf  = spark.read \
            .format("com.microsoft.kusto.spark.synapse.datasource") \
            .option("spark.synapse.linkedService", "adx") \
            .option("kustoDatabase", "adxdb") \
            .option("kustoQuery", "StormEvents_1 | take 10") \
            .load()display(kustoDf)
```

为了从 Azure Data Explorer 中读取数据，我们需要使用`kustoQuery`选项指定查询。在这种情况下，我们只是执行`StormEvents_1 | take 10`来验证数据。

这次让我们尝试另一个 Kusto 查询:

```
filtered_df = spark.read \
.format("com.microsoft.kusto.spark.synapse.datasource") \
.option("spark.synapse.linkedService", "AzureDataExplorer1") \
.option("kustoDatabase", "mydb") \
.option("kustoQuery", "StormEvents_1 | where DamageProperty > 0 and DeathsDirect > 0 | project EventId, State, StartTime, EndTime, EventType, DamageProperty, DeathsDirect, Source") \
.load()filtered_df.createOrReplaceTempView("storm_dataset")
```

这将把所有记录读入到一个`DataFrame`中，选择相关的列并过滤数据。例如，我们排除了没有任何财产损失或死亡的事件。最后，我们创建一个临时视图(`storm_dataset`)，以便使用 Apache Spark SQL 执行进一步的数据探索。

在此之前，让我们使用`Seaborn`(一个 Python 数据可视化库)来绘制一个简单的条形图:

```
import seaborn as sns
import matplotlib.pyplot as pltfiltered_df = filtered_df.toPandas()ax = sns.barplot(x="DeathsDirect", y="EventType",data=filtered_df)
ax.set_title('deaths per event type')
ax.set_xlabel('Deaths#')
ax.set_ylabel('Event Type')
plt.show()
```

![](img/daac706ccfbb727fd2ac078716271445.png)

Seaborn 情节(图片由作者提供)

下面是一个基于临时视图的 Spark SQL 示例。

```
%%sqlSELECT EventType, AVG(DamageProperty) AS avg_property_damage
FROM storm_dataset 
GROUP BY EventType
ORDER BY avg_property_damage DESC
```

我们计算了每种事件类型(雪崩、冰暴等)造成的平均损失。).下面的输出是柱形图的形式(但也有其他选项):

![](img/d8bb11b2e9ab0a498a5b4b2b697b6e99.png)

图表输出(图片由作者提供)

这是上面的一个小变化，我们找出每个州的最大死亡人数。

```
%%sqlSELECT 
    State
    , MAX(DeathsDirect) AS deaths
FROM storm_dataset 
GROUP BY State
ORDER BY deaths DESC
```

这次还有一个饼状图输出:

![](img/3e128a7a5b5243be5f02230e848e70f2.png)

饼图输出(图片由作者提供)

现在，您知道了如何通过使用 Azure Synapse 中的 Apache Spark 池进行处理，从 Azure Data Explorer 中的现有数据集中提取见解。

# 处理数据并将数据写入 Azure Data Explorer

本节将介绍如何使用 Spark(准确地说是 Synapse Spark Pools)处理数据，并将其写入 Azure Data Explorer 以供进一步分析。

首先创建另一个表`StormEvents_2`

```
.create table StormEvents_2 (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)
```

我们将使用现有的 CSV 数据。这与我们之前在 Azure Data Explorer 中获取的数据相同。但是，这一次，我们将把它下载到我们的本地机器，并上传到与 Azure Synapse workspace 关联的 ADLS Gen2 帐户。

从下载该文件开始:

```
curl -o StormEvents.csv "https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?sv=2019-12-12&ss=b&srt=o&sp=r&se=2022-09-05T02:23:52Z&st=2020-09-04T18:23:52Z&spr=https&sig=VrOfQMT1gUrHltJ8uhjYcCequEcfhjyyMX%2FSc3xsCy4%3D"
```

使用工作区将其上传到 ADLS 文件系统:

![](img/32a989d8ffdaf52bb42761b90b0e517e.png)

将文件上传到 Azure 数据湖存储(图片由作者提供)

> *对于后续步骤，您可以将代码直接粘贴到 Azure Synapse Analytics**中的* [*Synapse Studio 笔记本中，或者将*](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-development-using-notebooks?tabs=classical&WT.mc_id=data-21329-abhishgu) [*该笔记本*](https://github.com/abhirockzz/synapse-azure-data-explorer-101/blob/master/notebooks/synapse-adx-write-batch.ipynb) *导入到工作区中。*

将数据集从 ADLS Gen2 加载到一个`DataFrame`:

```
events = (spark.read
                .csv("/StormEvents.csv", header=True, inferSchema='true')
              )
```

使用 Apache Spark 应用一些基本的*过滤*——忽略包含空数据的行，删除我们不需要处理的列，过滤没有任何属性损坏的行。

```
events_filtered = events.dropna() \
                        .drop('StormSummary', 'EndLat','EndLon','BeginLat','BeginLon') \
                        .filter((events.DamageProperty > 0))print(events_filtered.count())
display(events_filtered.take(10))
```

最后，将`DataFrame`写入 Azure 数据浏览器:

```
events_filtered.write \
    .format("com.microsoft.kusto.spark.synapse.datasource") \
    .option("spark.synapse.linkedService", "adx") \
    .option("kustoDatabase", "adxdb") \
    .option("kustoTable", "StormEvents_2") \
    .option("tableCreateOptions","FailIfNotExist") \
    .mode("Append") \
    .save()
```

> 请注意，我们使用了`FailIfNotExist`，这意味着如果在请求的集群和数据库中没有找到该表，操作将会失败。
> 另一个选项是`CreateIfNotExist` —如果在请求的集群和数据库中找不到该表，将创建该表，其模式与正在写入的数据帧相匹配。
> 
> 更多信息请参考[https://github . com/Azure/Azure-kusto-spark/blob/master/docs/kustosink . MD # supported-options](https://github.com/Azure/azure-kusto-spark/blob/master/docs/KustoSink.md#supported-options)

给它一分钟让数据被写入。然后你就可以随心所欲地执行 Azure Data Explorer 查询了！尝试以下内容:

```
.show ingestion failuresStormEvents_2| take 10StormEvents_2
| summarize event_count=count() by bin(StartTime, 1d)
| render timechart
```

![](img/c2b8a4207f511610e528e8facc99f24c.png)

时间图输出(图片由作者提供)

您刚刚执行的只是典型的基于批处理的数据处理设置的一瞥。但是情况并不总是这样！

# 快速回顾

在这篇博文中，你学到了:

*   如何**设置和配置** Azure Synapse 和 Azure Data Explorer(包括安全访问)。
*   **如何充分利用 Azure Data Explorer 中的现有数据**并使用 Azure Synapse 中的 Apache Spark pools 对其进行处理。
*   **如何处理来自外部来源的数据**并将结果写回 Azure Data Explorer 以供进一步分析。

# 总结一下！

这些简单的例子可以帮助你开始。但是，Apache Spark SQL、 [Python](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-version-support?WT.mc_id=data-17928-abhishgu#python-libraries) 和 [Scala/Java](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-version-support?WT.mc_id=data-17928-abhishgu#scala-and-java-libraries) 库的全部功能都可供您使用。如果我不提及 Synapse SQL Pools(有[无服务器](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview?WT.mc_id=data-17928-abhishgu)和[专用](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&WT.mc_id=data-17928-abhishgu)模式)将是我的失职，它允许通过 T-SQL 进行数据访问，并为广泛的商业智能、专用查询工具和流行的驱动程序提供了可能性。

🙏🏻感谢 [Manoj Raheja](https://www.linkedin.com/in/manoj-b-raheja/) 和 [Adi Polak](https://twitter.com/AdiPolak?) 的评论和反馈！🙏🏻