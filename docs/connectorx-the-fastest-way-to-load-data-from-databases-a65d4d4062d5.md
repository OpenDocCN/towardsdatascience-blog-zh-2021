# ConnectorX:加载 Python 数据框最快的库

> 原文：<https://towardsdatascience.com/connectorx-the-fastest-way-to-load-data-from-databases-a65d4d4062d5?source=collection_archive---------4----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

## 用一行代码将 Pandas read_sql 加速 10 倍

![](img/f8f8355a58a490ad1339a64f391fee7d.png)

由 [NASA](https://unsplash.com/@nasa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/data-center?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

ConnectorX 旨在通过向开发人员提供高效、轻量级且易于使用的工具来加快从数据库加载数据的过程。在本文中，我们将通过回答以下问题为您简要介绍 ConnectorX:

*   [什么是 ConnectorX？](#cb2e)
*   [如何使用 ConnectorX？](#a4f5)
*   [为什么 ConnectorX 是更好的选择？](#d086)
*   接下来会发生什么？

# 什么是 ConnectorX？

ConnectorX 是一个开源库，可以加速将数据从数据库加载到像 *pandas 这样的数据结构中。数据框*用于进一步处理和分析。

![](img/b519206807574c8d3ca5e3c45d34504a.png)

ConnectorX 的高级工作流程示例

ConnectorX 由两个主要概念组成:**源**(例如 *PostgreSQL* )和**目的地**(例如*熊猫。数据帧*。ConnectorX 会将用户给出的 SQL 查询转发给**源**，然后高效地将查询结果从**源**传输到**目的地**。上图显示了当前支持的源和目的地的示例。

为了同时提供**高效的**性能和**强大的**功能，ConnectorX 中繁重的提升工作量被写成了 Rust。它还有一个带有简单 API 的 Python 绑定，可以轻松地与 Python 中的其他数据科学库进行互操作。

接下来，让我们看看如何通过一行代码以不同的方式使用 ConnectorX。

# 如何使用 ConnectorX？

步骤 1:安装连接器 x:

```
pip install connectorx
```

第二步:用 *read_sql* 从数据库加载数据。使用连接字符串定义源，默认情况下目的地是 *pandas。数据帧*可以通过设置 [*return_type*](https://github.com/sfu-db/connector-x#parameters) 来改变:

```
import connectorx as cx

# source: PostgreSQL, destination: pandas.DataFrame
df = cx.read_sql("postgres://postgres:postgres@localhost:5432/tpch", "SELECT * FROM lineitem")
```

查询结果将返回一只*熊猫。数据帧*如下所示:

![](img/8d823840752f29f2367ec069d6020bd7.png)

为了进一步提高速度，ConnectorX 支持通过对查询进行分区并并行执行分区查询来提高 CPU 和带宽资源的利用率。用户可以指定分区列，也可以手动定义分区查询:

```
# specify the partition column and number of partitions
df = cx.read_sql("postgres://postgres:postgres@localhost:5432/tpch", "SELECT * FROM lineitem", partition_on="l_orderkey", partition_num=4)

# manually partition the query
# the partition bellow is equivalent with the one above
df = cx.read_sql("postgres://postgres:postgres@localhost:5432/tpch", [
    "SELECT * FROM lineitem WHERE l_orderkey > 0 AND l_orderkey <= 15000000",
    "SELECT * FROM lineitem WHERE l_orderkey > 15000000 AND l_orderkey <= 30000000",
    "SELECT * FROM lineitem WHERE l_orderkey > 30000000 AND l_orderkey <= 45000000",
    "SELECT * FROM lineitem WHERE l_orderkey > 45000000 AND l_orderkey <= 60000000"
])
```

ConnectorX 将并行运行所有分区查询，并将所有查询结果连接成一个单独的 *pandas。数据帧*。至于分区，目前，对于 SPJA 查询，ConnectorX 支持对整数列进行自动分区。下面是一个更复杂的查询示例:

```
query = f”””
SELECT l_orderkey,
 SUM(l_extendedprice * ( 1 — l_discount )) AS revenue,
 o_orderdate,
 o_shippriority
FROM customer,
 orders,
 lineitem
WHERE c_mktsegment = ‘BUILDING’
 AND c_custkey = o_custkey
 AND l_orderkey = o_orderkey
 AND o_orderdate < DATE ‘1995–03–15’
 AND l_shipdate > DATE ‘1995–03–15’
GROUP BY l_orderkey,
 o_orderdate,
 o_shippriority 
“””df = read_sql(“postgresql://postgres:postgres@localhost:5432/tpch”, query, partition_on=”l_orderkey”, partition_num=4)
```

请查看我们的 [Github repo](https://github.com/sfu-db/connector-x#detailed-usage-and-examples) 获取更多用法和示例！

# 为什么 ConnectorX 是更好的选择？

其他常用的 Python 库如 *Pandas* 已经提供了类似的 *read_sql* 函数，那么为什么还要使用 ConnectorX 呢？为了回答这个问题，让我们来看一个简单的基准测试，我们将 ConnectorX 与其他三个现有的解决方案( [Pandas](https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html) 、 [Modin](https://modin.readthedocs.io/en/latest/supported_apis/io_supported.html) 和 [Dask](https://docs.dask.org/en/latest/dataframe-sql.html#loading-from-sql-with-read-sql-table) )进行了比较，这三个解决方案也能够将数据从数据库加载到数据框。

我们使用来自 [TPC-H](http://www.tpc.org/tpch/) 的 *LINEITEM* 表，并将比例因子设置为 10。该表包含 60M 记录 x 16 列，如果转储到 CSV 文件中，其大小为 8 GB，在 PostgreSQL 中为 11 GB。我们测量每个解决方案从数据库加载整个表并写入到 *pandas 所需的时间。不同并行度(1 到 10 核)和带宽条件下的 DataFrame* 。我们还测量了该过程中每种方法的峰值内存使用量。我们在这里展示的测试结果是在一个 AWS *r5.4xlarge* 实例上进行的，我们在该实例上运行 ConnectorX，并从运行在 AWS RDS 上的 *db.m6g.4xlarge* 实例上的 PostgreSQL 加载数据。(有关其他数据库的更多基准测试结果，请点击查看[。)结果显示，在所有场景中，ConnectorX 都是速度最快、内存效率最高的解决方案！](https://github.com/sfu-db/connector-x/blob/main/Benchmark.md#benchmark-result-on-aws-r54xlarge-with-dbm6g4xlarge-rds)

1.  **更快的数据加载**

下图显示了使用 4 个 CPU 内核测试不同方法速度的结果。我们可以看到，ConnectorX 是所有解决方案中速度最快的，在 PostgreSQL 上比 *Modin* 、 *Pandas* 、 *Dask* 分别加快了 **3x** 、 **10x、**和 **20x** 的数据加载速度！

![](img/7e2ae1f97e77a9ff7d8d75fb399bca26.png)

read_sql (4 核)的时间比较

我们还改变了用于 *read_sql* 的内核(分区)数量，从 1 到 10 不等，并在不同的网络条件和机器下进行了测试。( *Dask* 由于内存不足(OOM)无法使用完一个内核。Pandas 不支持并行，所以我们只使用一个内核。)我们发现 ConnectorX 在所有设置中始终优于其他方法。

**2。更小的内存占用**

接下来，我们来看看每种方法的内存使用情况。这里我们绘制了 4 核设置的比较结果。我们可以看到 ConnectorX 使用的内存比其他方法少 3 倍。在不同的并行度下，结果是相同的。

![](img/7e47daa84fa4508df34f87c48dde9b23.png)

read_sql 的内存比较(4 核)

【ConnectorX 是如何做到这一点的？

ConnectorX 实现这一性能的三个主要原因是:

1.  **用原生语言编写:**与其他库不同，ConnectorX 是用 Rust 编写的，避免了用 Python 实现数据密集型应用的额外成本。
2.  **精确复制一次:**当从数据库下载数据时，现有的解决方案或多或少会进行多次数据复制，而 ConnectorX 的实现遵循“零复制”原则。我们设法精确地复制数据一次，直接从**源**到**目的地**，即使是在并行的情况下。
3.  **CPU 缓存高效:**我们应用了多项优化，使 ConnectorX CPU 缓存友好。与“零拷贝”实现不同，ConnectorX 中的数据处理是以流的方式进行的，以减少缓存缺失。另一个例子是，当我们在 Python 中构造字符串时，我们将一批字符串写入一个预先分配的缓冲区，而不是为每个字符串分配单独的位置。

# 接下来会发生什么？

到目前为止，ConnectorX 支持广泛使用的**源**，包括 PostgreSQL、MySQL、SQLite 以及其他采用相同有线协议的数据库(例如，通过 PostgreSQL 的 Redshift、通过 MySQL 的 Clickhouse)。我们现在正致力于增加更多的[流行数据库和数据仓库](https://github.com/sfu-db/connector-x/discussions/61)。我们还计划在未来支持以不同的文件格式(如 CSV、JSON、Parquet)从数据存储(如[亚马逊 S3](https://aws.amazon.com/s3/) )传输数据。

至于**目的地**，ConnectorX 支持 Python 和 Rust 中所有流行的数据帧，包括[熊猫](https://pandas.pydata.org/)、[阿帕奇箭头](https://arrow.apache.org/)、[摩丁](https://modin.readthedocs.io/en/latest/#)、 [Dask](https://dask.org/) 和 [Polars](https://github.com/pola-rs/polars) 。已完成和正在进行的源和目的地的完整列表可在[这里](https://github.com/sfu-db/connector-x#supported-sources--destinations)找到。让我们知道你是否有其他的建议！

除了支持更多的源和目的地，该团队还在开发新功能，以帮助最大限度地减少数据加载的时间成本，例如维护一个[客户端数据缓存](https://github.com/sfu-db/connector-x/discussions/64)。我们还对优化查询分区感兴趣，例如支持从数据库收集元数据的自动分区。您可以在 Github 上关注我们，了解我们的最新实施情况和下一步计划！