# 用 Apache Kafka 构建 Python 微服务:一分耕耘一分收获

> 原文：<https://towardsdatascience.com/building-python-microservices-with-apache-kafka-all-gain-no-pain-1435836a3054?source=collection_archive---------6----------------------->

![](img/a8ad356ab988816e5156a440eb4cf9d5.png)

图片来自 [freepik](https://ru.freepik.com/premium-photo/abstract-background-with-the-movement-of-luminous-particles-digital-technology-connection-concept_16582967.htm#page=1&position=5&from_view=user#position=2) 的 [mrjo_7](https://ru.freepik.com/mrjo-7)

工程师在日常工作中经常使用阿帕奇卡夫卡。Kafka 执行的主要任务是:阅读消息、处理消息、将消息写到一个主题，或者在一定时间内聚合消息。窗口可以是固定的(例如每小时聚合)或滑动的(例如从某个时间点开始的每小时聚合)。

鉴于 Kafka 作品的复杂性，我不建议您尝试从头构建自己的解决方案。微调、测试和支持将非常困难。幸运的是，有现成的实现，比如 Kafka Streams for Java 和 Kafka Streams for Python——由 [Robinhood](https://robinhood.com/us/en/) 编写的 [Python Faust](https://github.com/robinhood/faust) 。该团队还有一个社区支持的分支— [浮士德流媒体](https://github.com/faust-streaming/faust)。

本文分享了我在 [Provectus](https://provectus.com/) 使用 Apache Kafka 构建异步 Python 微服务进行“通信”的经验。 [Faust](https://faust.readthedocs.io/en/latest/) ，一个将 Kafka 流的思想移植到 Python 的流处理库，被用作微服务基础。Faust 还为间隔任务和调度任务提供了一个 HTTP 服务器和一个调度程序。在一个测试实现中，我将使用 FastAPI、Grafana 和 Prometheus 等工具和库。

## 浮士德

浮士德是[卡夫卡流](https://kafka.apache.org/documentation/streams/)在 Python 上的实现。它最初由 Robinhood 开发，现在仍被用作高性能分布式系统和实时数据管道的默认库。

该库适用于 Python 3.6+和 Kafka 0.10.1+，并支持各种数据存储、数据收集和加速模块。Faust 使您能够利用常见的 Python 库，如 NumPy、SciPy、TensorFlow、SQLAlchemy 等。

## 项目架构

我相信要真正学会如何做一件事，你应该一步一步地去做。下面是我们将要构建的一个简单测试项目的架构图。

![](img/71a113dbce29066b15cbf159e7716614.png)

作者图片

测试数据将在*演示服务器*微服务中编译，而*数据请求者*微服务将每秒请求数据。*数据请求者*是我们 Faust 系统中的第一个微服务。一旦数据被请求，*data _ requestor*会将回复推送给 Kafka。然后，这个消息被 *data_processor* 读取、处理并推回给 Kafka。随后，它被 *data_aggregator* 和 *db_loader* 读取，其中第一个微服务计算平均值，第二个微服务将它们加载到数据库中。 *db_loader* 还加载由 *data_aggregator* 生成的消息。同时，当用户请求时， *api_gateway* 可以从数据库中提取数据。使用 Prometheus 监控所有活动，并可在 Grafana 中可视化。该项目和附加服务在 *docker-compose* 推出。

## 第一步。基础设施设置

因为我们将使用 *docker-compose* 加速项目，所以描述所有第三方服务是有意义的。我们将需要 Apache Kafka，一个数据库，一个管理仪表板来管理它，以及几个[监控服务](/overview-of-ui-tools-for-monitoring-and-management-of-apache-kafka-clusters-8c383f897e80) (Prometheus 和 Grafana)。让我们使用 Postgres 作为我们的数据库，使用 PgAdmin 作为它的管理和监控组件。

在这个阶段，我们的 [docker-compose.yml](https://github.com/KrasnovVitaliy/microservice_in_python/blob/main/01_infrastructure/docker-compose.yml) 看起来是这样的:

*docker-compose，*与基础设施、配置和附加脚本一起，存储在存储库的[单独文件夹](https://github.com/KrasnovVitaliy/microservice_in_python/tree/main/01_infrastructure)中。

## 微服务的常见组件

尽管每个服务都有自己的任务，并使用两个不同的库(FastAPI 和 Faust)编写，但它们有几个共同的组件。

首先是基于配置加载器库的配置加载器。它使您能够使用 yaml 文件加载配置，并通过可变的环境重新定义它们的值。这在需要在 docker 容器中重新定义特定值的情况下非常有用。

其次是普罗米修斯出口商。您可以使用它来导出数据和指标，并在 Grafana 中显示它们。请记住，它在 FastAPI 和 Faust 中的实现略有不同。

## 第二步。数据仿真微服务

为了测试和演示该系统，我们将使用一个简单的服务，以 JSON 格式返回货币对的值。为此，我们也将使用 FastAPI 库。这项服务和 Flask 一样容易使用，但是它自带了 *async* 和 Swagger UI 文档。可以在这里找到:[http://127 . 0 . 0 . 1:8002/docs](http://127.0.0.1:8002/docs)

我们将只为服务指定一个请求:

从上面的代码中可以看出，每个请求都返回一个 JSON，其中包含两对随机生成的值。回复可能是这样的:

```
{
 “USDRUB”: 85.33,
 “EURRUB”: 65.03
}
```

要监控这些值，请将 Prometheus 添加到系统中。它应该作为*中间件*添加，并有单独的指定路径。

您可以在浏览器中查看您的值的指标:[http://127 . 0 . 0 . 1:8002/metrics](http://127.0.0.1:8002/metrics)

## 第三步。数据请求者微服务

在 Faust 的帮助下，*API _ requestor*微服务请求来自演示服务器的数据。因为 Faust 是异步的，我们可以简单地使用 *aiohttp* 客户端来请求测试数据。

首先，让我们创建一个 Faust 应用程序。

创建应用程序时，请确保指定了它的名称，这是一个必需的参数。如果我们启动几个同名的服务版本，Kafka 将在它们之间分配分区，使我们能够横向扩展我们的系统。接下来，我们需要在 *value_serializer* 中指定消息序列化的方法。在这个例子中，我们可以照原样读取原始的*消息，以便随后序列化接收到的消息。我们还应该定义访问 Faust 托管的 HTTP 服务器的地址和端口。*

接下来，我们需要定义将要接收来自 *demo_server* 的回复的主题。

第一个参数是主题名；这是强制性的。然后，您可以指定分区的数量(可选)。请记住，分区的数量必须与 Kafka 中主题分区的数量相似(在创建主题时指定)。

请注意，*data _ requestor*服务并不从主题中读取消息，而是不断地将请求推送到数据模拟器，然后处理回复。要实现这一点，您需要编写一个实时时钟函数，该函数每秒触发一次，或者在指定的时间间隔触发。

我们可以看到该功能是由 *@app.timer* decorator 周期性执行的。它接收每秒间隔作为参数。然后，该函数为 DataProvider 生成一个类实例，负责提供对请求的回复。在每一个随后的请求之后，普罗米修斯计时器的值增加。如果数据是按请求接收的，它将被发送到一个主题。因为 Kafka 使用特定的键和字节内消息，所以我们需要在将数据推送到主题之前对其进行序列化。

在启动应用程序时，您还需要初始化 Prometheus。为此，您可以简单地使用启动应用程序时调用的函数。

请记住，函数是由 *@app.task* decorator 分配和定义的。Prometheus 使用自己的端口作为独立的服务器启动，它与 Faust 的 HTTP 服务器并行工作。

给你！每秒向 Kafka 发出请求和写入数据并不困难。添加监控组件也可以很快完成。

## 第四步。数据处理微服务

我们的下一个微服务——*数据处理器*——处理由 *api 请求者*微服务接收的数据对。应用程序初始化和数据监控的代码与我们用来管理*data _ requestor*的代码相同。然而，在 *data_processor* 的情况下，服务从主题接收消息来处理它们。可以用函数来完成。

该函数基于*@ app . agent(src _ data _ topic)*装饰器，该装饰器告诉该函数处理 *src_data_topic* 中的消息。从 stream.items() 的 msg_key，msg_value 的*异步读取消息。*

然后，我们需要序列化接收到的消息，并提取货币对及其值。每一对都应该分别写一个相应的主题。

## 第五步。数据聚合微服务

请求和处理数据的微服务以流的形式在 Kafka 中读写数据。每个新消息都独立于以前的消息进行处理。但是，偶尔您可能需要同时处理一定数量的消息。假设您需要找到最后十对值的平均值。为此，你的系统应该能够在某个地方存储这 10 对。您不能在本地存储数据:如果您触发了聚合服务的多个版本，每个版本都将只在本地存储其值，这将导致平均值计算不正确。这里是你可以利用浮士德表的地方。

这些表将值存储在 *changelog* 主题和本地——rocks db 中。这允许服务的所有版本同步工作。重启后，它从本地数据库恢复状态，当读取可用的*变更日志*时，它继续工作。

表的主题名是这样分配的: <service-name>- <table-name>-changelog。在我们的系统中，名称是:data-aggregator-average-changelog。</table-name></service-name>

在表中处理和存储新消息非常简单:

从上面的代码可以看出，您需要定义函数如何处理主题中的消息。收到的每个新值都应存储在表中，以计算平均值。您可以以类似于标准 Python 字典的方式使用该表。

## 第六步。数据库加载器微服务

*db_loader* 微服务一次读取两个主题——*data-aggregator-average-changelog*和*processed _ data*——两个。它将消息从 data_aggregator 写入第一个主题，从 data_processor 写入第二个主题。这就是为什么我们需要描述消息处理的两个函数。

与其他服务类似，您需要:读取消息，将其存储在数据库中，更新指标。为了管理数据库，我们将使用 ORM SQLAlchemy。请注意，您应该将其设置为异步工作。为此，定义所需的依赖关系。

```
asyncpg==0.23.0
SQLAlchemy==1.4.0
```

在配置中指定 *DB URI* 。

```
DB_URI: “postgresql+asyncpg://postgres:postgres@127.0.0.1:5432/currencies”
```

在数据库管理代码中使用异步会话。

## 第七步。结果请求者微服务

最后，让我们使用 FastAPI 创建一个简单的服务，从数据库中请求结果。它将从数据库中读取结果，并将它们推送到 JSON。要管理数据库，可以使用 ORM(就像我们之前做的那样)。至于 FastAPI，说明类似于步骤 2。数据仿真微服务。

## 结论

在 Python 上用 Apache Kafka 构建微服务相当简单。你可以把处理和管理卡夫卡的几乎所有工作都交给浮士德。然后，只需描述和定义函数，就能看到它们实时处理您的消息。

我希望这篇教程能帮助你更好地理解如何将微服务、Apache Kafka 和 Python 结合起来。如果您对 Apache Kafka 中的集群监控和管理感兴趣，我建议您也查看一下本文。

有什么问题吗？请在评论区联系我进行讨论。