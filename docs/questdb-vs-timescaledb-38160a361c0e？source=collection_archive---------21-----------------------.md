# QuestDB 与时标 b

> 原文：<https://towardsdatascience.com/questdb-vs-timescaledb-38160a361c0e?source=collection_archive---------21----------------------->

![](img/3221c93a90128408a4f693276d168bb6.png)

照片由 [Guillaume Jaillet](https://unsplash.com/@i_am_g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/fast?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 数据工程，时间序列

## 如何使用时序基准测试套件比较 QuestDB 和 TimescaleDB 的数据库读写性能

# 介绍

在这个联系空前紧密的世界里，数十亿用户正在产生比以往更多的数据。从人类通信到我们创造的数字足迹，物联网传感器已经无处不在，金融交易也实现了数字化。我们有大量以时间为中心的数据，我们正在努力跟上这一切。时间系列数据库正在崛起。OSS 项目，如 QuestDB、InfluxDB、TimescaleDB，以及基于云的解决方案，如 Amazon Timestream 等。的需求量比以往任何时候都大。时间系列数据库已经正式成熟。

所有这些产品都在争夺时间序列领域的更大空间，在这样做的过程中，它们互相做得更好。本文将考察两个主要的时间序列数据库，并使用一个名为 TSBS 的开源基准测试工具——时间序列基准测试套件对它们进行比较。这个基准测试套件基于最初在 InfluxDB 开发的测试脚本，后来被其他主要的 timeseries 数据库增强，目前由 TimescaleDB 维护。

# 什么是时间序列基准测试套件(TSBS)？

对于 MySQL 和 PostgreSQL 等传统数据库，许多流行的选项如 [HammerDB](https://hammerdb.com/) 和 [sysbench](https://github.com/akopytov/sysbench) 都是衡量数据库读写性能的标准工具。不同类型的数据库都有类似的工具。当基准测试工具通过创建真实的突发和读取流来模拟真实场景时，性能测试是有意义的。时间序列数据库的访问模式与传统数据库非常不同——这就是为什么我们需要像 TSBS 这样的工具。

TSBS 目前支持两种负载:

*   **物联网** —模拟卡车运输公司的传感器生成的物联网数据。想象一下，使用车队中每辆卡车的实时诊断数据来跟踪一个货运车队。
*   **DevOps** —模拟服务器生成的数据使用情况，跟踪内存使用情况、CPU 使用情况、磁盘使用情况等。想象一下，查看 Grafana 仪表板上的这些指标，并获得违规警报。

# 先决条件

对于本教程，我们将使用 DevOps 选项。首先，您需要遵循以下步骤:

*   **在您的机器上安装 Docker** ，以便您可以从 Docker Hub 安装并运行 [QuestDB](https://questdb.io/docs/get-started/docker/) 和 [TimescaleDB](https://hub.docker.com/r/timescale/timescaledb/) 。
*   **克隆 TSBS 存储库** —一旦三个都启动并运行，[在您的机器上克隆 TSBS 存储库](https://github.com/questdb/tsbs)。

> ***注:*** *该基准测试是在 AWS EC2 上的 16 核英特尔至强白金 8175M CPU @ 2.50GHz 和 128 GB RAM 上完成的。*

# 使用 TSBS 测试时序数据库性能

我们将分四个阶段测试这两个数据库的性能:

*   **生成一天的** **DevOps 数据**，其中每 10 秒收集 200 个设备的 9 个不同指标。将根据 QuestDB 和 TimescaleDB 各自的格式分别为它们生成数据。为此使用`**tsbs_generate_data**`实用程序。
*   **加载**生成的数据。使用`**tsbs_load_questdb**`和`**tsbs_load**` 实用程序分别将数据加载到 QuestDB 和 TimescaleDB 中。这使我们能够测试每个系统的接收和写入速度。
*   **生成**查询，在 QuestDB 和 TimescaleDB 的加载数据上运行。为此，请使用`**tsbs_generate_queries**` 实用程序。
*   **分别使用`**tsbs_run_queries_questdb**`和`**tsbs_run_queries_timescaledb**`在 QuestDB 和 TimescaleDB 上执行**生成的查询。

让我们一个接一个地检查每个步骤的脚本。

# 生成测试数据

在本教程中，我们将基准测试的`**scale**`限制在 200 台设备。如上所述，数据将在一天内生成，每 10 秒钟跟踪一百个设备中每一个设备的九个指标。使用`**tsbs_generate_data**`命令，您可以为任何支持的数据库和用例生成测试数据。

> ***注意:*** *生成的数据会占用很多空间。您可以根据基准测试要求和磁盘空间的可用性来扩大或缩小规模。*

> ***注意:*** *由于 TimescaleDB 和 QuestDB 使用的格式不同，为数据生成的文件大小也会不同。QuestDB 使用的流入线协议格式比其他任何格式都要简单得多。*

# 加载数据

加载数据甚至比生成数据更简单。对于 TimescaleDB，您可以使用通用实用程序`**tsbs_load**`。对于 QuestDB，您可以使用`**tsbs_load_questdb**`实用程序，因为它支持一些特定于 QuestDB 的标志，比如用于流入线路协议绑定端口的`--**ilp-bind-to**`和表示 QuestDB 的 REST 端点的`**--url**`。您可以使用以下命令将数据分别加载到 TimescaleDB 和 QuestDB 中:

> ***注意:*** *请按照 TimescaleDB 的 config.yaml 文件的说明操作。*

为了更好地了解负载性能，您可以尝试更改`**--workers**` 参数。请确保两个数据库的基准参数和条件是相同的，这样您就可以得到公平的比较。

## 时标 DB 加载/写入性能

```
./tsbs_load load timescaledb --config=./config.yaml
...
Summary:
loaded 174528000 metrics in 59.376sec with 8 workers (mean rate 2939386.10 metrics/sec)
loaded **15552000** rows in **59.376sec** with **8 workers** (mean rate 261925.49 rows/sec)
```

## QuestDB 加载/写入性能

```
./tsbs_load_questdb --file /tmp/questdb-data --workers 8
...
Summary:
loaded 174528000 metrics in 18.223sec with 8 workers (mean rate 9577373.06 metrics/sec)
loaded **15552000** rows in **18.223sec** with **8 workers** (mean rate 853429.28 rows/sec)
```

在这种情况下，有八个工作线程的 QuestDB 的写性能比 TimescaleDB 快 **~3.2x** **。关于这次基准测试的完整输出，请[访问这个 GitHub 库](https://github.com/kovid-r/tsbs-questdb-timescaledb#readme)。**

# 生成查询

TSBS 为 QuestDB 和 TimescaleDB 生成的数据集包含 200 台主机的指标。为了查询所有主机中某个指标高于阈值的所有读数，我们将使用查询类型`**high-cpu-all**`。要在一天内生成 1000 个不同时间范围的查询，您需要运行以下命令:

在本教程中，我们只运行一种类型的读取查询。您可以从[中选择不同类型的查询](https://github.com/questdb/tsbs#appendix-i-query-types-)来测试读取性能。

# 执行查询

现在我们已经生成了数据，并将其加载到 QuestDB 和 TimescaleDB 中，还生成了我们想要运行的基准测试查询，我们最终可以使用以下命令执行读取性能基准测试:

> ***注意:*** *确保在运行命令之前已经正确生成了查询。为此，您可以运行* `***less /tmp/timescaledb-queries-high-cpu-all***` *或* `***less /tmp/queries_questdb-high-cpu-all***` *。*

## 时标 DB 读取性能

```
Run complete after **1000 queries** with **8 workers** (Overall query rate **25.78 queries/sec**):
TimescaleDB CPU over threshold, all hosts:
min: 222.49ms, med: 274.94ms, mean: 308.60ms, max: 580.13ms, stddev: 73.70ms, sum: 308.6sec, count: 1000
all queries :
min: 222.49ms, med: 274.94ms, mean: 308.60ms, max: 580.13ms, stddev: 73.70ms, sum: 308.6sec, count: 1000
wall clock time: **38.827242sec**
```

## QuestDB 读取性能

```
Run complete after **1000 queries** with **8 workers** (Overall query rate **70.18 queries/sec**):
QuestDB CPU over threshold, all hosts:
min: 92.71ms, med: 109.10ms, mean: 113.32ms, max: 382.57ms, stddev: 19.34ms, sum: 113.3sec, count: 1000
all queries :
min: 92.71ms, med: 109.10ms, mean: 113.32ms, max: 382.57ms, stddev: 19.34ms, sum: 113.3sec, count: 1000
wall clock time: **14.275811sec**
```

QuestDB 执行查询的速度比 TimescaleDB 快 2.7 倍。

# QuestDB 与时标 b

总结一下读写基准测试的结果，我们可以说 QuestDB 的写速度明显快于 TimescaleDB，读速度也快得多。当我们谈论读取性能时，只使用一种类型的查询可能是不公平的，这就是为什么您可以尝试自己运行 TSBS 套件来处理这两个数据库的不同类型的查询。总结如下:

> *QuestDB 执行~* ***对于写入/加载工作负载，比 TimescaleDB 快 320%****。*
> 
> 对于读取/分析工作负载，QuestDB 的执行速度比 TimescaleDB 快 270 %。

# 结论

TSBS 是时间序列数据库基准测试的事实标准。在本教程中，您学习了如何使用 TSBS 通过轻松生成测试数据和模拟实际的读写负载来比较两个 timeseries 数据库。如前所述，TSBS 目前支持 DevOps 和 IoT(车辆诊断)的测试数据生成。您可以创建您的测试数据生成脚本来创建更多的用例，例如，实时天气跟踪、交通信号、金融市场等等。