# 使用 PostgreSQL 作为数据仓库

> 原文：<https://towardsdatascience.com/using-postgresql-as-a-data-warehouse-23fb50fb2bb0?source=collection_archive---------27----------------------->

## 经过一些调整，Postgres 可以成为一个很好的数据仓库。下面是如何配置的。

![](img/5a5383ab9a8adf303fb9525d6c8b07bb.png)

*照片由* [*瑞恩帕克*](https://unsplash.com/@dryanparker?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)*/*[*Unsplash*](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

在[旁白](https://www.narrator.ai)我们支持许多数据仓库，包括 Postgres。尽管 Postgres 是为生产系统设计的，但稍加调整，它可以作为数据仓库工作得非常好。

对于那些想切入正题的人，这里有 TL；博士；医生

*   不要使用与您的生产系统相同的服务器
*   升级到 pg 12+(或者在查询中避免[通用表表达式](#b49b)
*   轻松完成指标——少即是多
*   考虑对[长表进行分区](#ff00)
*   确保你没有被 [I/O 束缚](#5e73)
*   [批量插入后真空分析仪](#c673)
*   探索[并行查询](#4134)
*   增加[统计抽样](#cd70)
*   在经常查询的表上少用[列](#b883)
*   在规模上考虑一个[专用仓库](#17a3)

# 数据仓库和关系数据库的区别

# 生产查询

典型的生产数据库查询从潜在的大型数据集中选择少量的行。它们被设计用来快速回答许多这类问题。

想象一个 web 应用程序——成千上万的用户可能同时查询

> *select * from users where id = 1234*

数据库将被调优以快速处理大量这样的请求(在几毫秒内)。

为了支持这一点，包括 Postgres 在内的大多数数据库都是按行存储数据的——这允许从磁盘上高效地装载整行数据。他们经常使用索引来快速查找相对较少的行。

# 分析查询

分析查询通常相反:

*   一个查询将处理许多行(通常占整个表的很大一部分)
*   查询可能需要几秒到几分钟才能完成
*   一个查询将从一个宽(多列)表中选择少量的列

因此，专用数据仓库(如 Redshift、BigQuery 和 Snowflake)使用面向列的存储，没有索引。

Holistics.io 有一个很好的[指南](https://www.holistics.io/blog/the-rise-and-fall-of-the-olap-cube/)更详细地解释了这一点。

# 这对 Postgres 意味着什么

Postgres 虽然是面向行的，但也可以轻松处理分析查询。它只需要一些调整和一些测量。虽然 Postgres 是一个很好的选择，但是请记住，像雪花这样基于云的仓库(从长远来看)更容易管理和维护。

# 将 Postgres 配置为数据仓库

![](img/8cc83f7586bc69ed172b1db30f921f72.png)

图片由 [PostgreSQL](https://www.postgresql.org/about/press/presskit13/en/) 提供

> *警告:不要将您的生产 Postgres 实例用于数据报告/指标。一些查询是没问题的，但是分析工作负载与典型的生产工作负载有很大不同，它们将对生产系统产生相当大的性能影响。*

# 避免常见的表表达式

公共表表达式(cte)也称为“WITH”查询。它们是避免深度嵌套子查询的好方法。

```
WITH my_expression AS ( 
  SELECT customer as name FROM my_table 
) 
SELECT name FROM my_expression
```

不幸的是，Postgres 的查询规划器(在版本 [12](https://www.postgresql.org/docs/12/release-12.html#id-1.11.6.11.3) 之前)将 [CTEs 视为一个黑盒](https://hakibenita.com/be-careful-with-cte-in-postgre-sql)。Postgres 将有效地自己计算 CTE，具体化结果，然后在使用时扫描结果。在许多情况下，这会大大降低查询速度。

在讲述者中，从我们的一些常见查询中删除 3 个 cte 使它们的速度提高了 4 倍。

简单的解决方法是将 cte 重写为子查询(或者升级到 12)。

```
SELECT name FROM ( SELECT customer as name FROM my_table )
```

cte 越长，可读性就越差，但对于分析工作负载，这种性能差异是值得的。

# 谨慎使用索引

索引对于分析工作负载的重要性实际上不如传统的生产查询。其实像红移、雪花这样的专用仓根本没有。

虽然索引对于快速返回少量记录很有用，但是如果查询需要表中的大部分行，它就没有用了。例如，对“讲述人”的一个常见查询是这样的

> 获取每个客户打开的所有电子邮件，并计算按月分组查看主页的转化率。

不用写出 SQL，很明显这个查询可以覆盖很多行。它必须考虑所有客户、所有打开的电子邮件和所有页面浏览量(其中 page = '/')。

即使我们为这个查询准备了一个索引，Postgres 也不会使用它——在加载许多行时进行表扫描会更快(磁盘上的布局更简单)。

**不使用索引的理由**

1.  对于许多分析查询，Postgres 进行表扫描比索引扫描更快
2.  索引增加了表的大小。表格越小，内存就越大。
3.  索引增加了每次插入/更新的额外成本

**什么时候使用指数**

有了索引，有些查询会快得多，值得花时间。对我们来说，我们经常第一次询问客户做了什么。我们为此创建了一个列(`activity_occurrence`)，所以我们构建了一个部分索引。

```
create index first_occurrence_idx on activity_stream(activity) where activity_occurrence = 1;
```

# 分割

[对表进行分区](https://www.postgresql.org/docs/12/ddl-partitioning.html)是提高表扫描性能的一个很好的方法，而不需要支付索引的存储成本。

从概念上讲，它将一个较大的表分成多个块。理想情况下，大多数查询只需要从一个(或少数几个)中读取，这可以极大地加快速度。

[最常见的场景](https://www.enterprisedb.com/blog/postgres-table-partitioning)是按时间(`range partitioning`)分解事物。如果您只查询上个月的数据，那么将一个大表分成几个月的分区可以让所有查询忽略所有旧的行。

在“讲述人”中，我们通常查看所有时间的数据，因此范围没有用。但是，我们有一个非常大的表来存储客户活动(查看页面、提交支持请求等)。我们很少一次查询一两个以上的活动，所以`list partitioning`非常好用。

好处是双重的:我们的大多数按活动的查询无论如何都要进行全表扫描，所以现在它们扫描的是一个更小的分区，我们不再需要大的活动索引(它主要用于不太频繁的活动)。

对分区的主要警告是，它们需要管理的工作量稍微多一些，并且并不总是能提高性能——创建太多分区或大小极不相等的分区并不总是有帮助。

# 最小化磁盘和 I/O

因为表扫描更常见(参见上面的索引)，所以磁盘 I/O 变得相当重要。按性能影响排序

1.  确保 Postgres 有足够的可用内存来缓存最常访问的表——或者使表变小
2.  选择固态硬盘而不是硬盘(尽管这取决于成本/数据大小)
3.  查看有多少 I/O 可用——如果数据库读取磁盘太多，一些云托管提供商会限制 I/O。

检查长时间运行的查询是否命中磁盘的一个好方法是使用`pg_stat_activity`表。

```
SELECT pid, now() - pg_stat_activity.query_start AS duration, usename, query, state, wait_event_type, wait_event FROM pg_stat_activity WHERE state = 'active' and (now() - pg_stat_activity.query_start) > interval '1 minute';
```

如果查询是从磁盘读取，那么`wait_event_type`和`wait_event`列将显示`IO`和`DataFileRead`。上面的查询对于查看其他可能阻塞的东西也非常有用，比如锁。

# 批量插入后抽真空

[清空](https://www.postgresql.org/docs/current/sql-vacuum.html)表是保持 Postgres 平稳运行的一个重要方法——它节省空间，当作为`vacuum analyze`运行时，它将计算统计数据，以确保查询规划器正确地估计一切。

Postgres 默认运行一个[自动真空](https://www.postgresql.org/docs/current/routine-vacuuming.html#AUTOVACUUM)进程来处理这个问题。通常最好不要去管它。

也就是说，`vacuum analyze`最好在插入或删除大量数据后运行。如果您正在运行一个定期插入数据的任务，那么在您完成插入所有内容后立即运行`vacuum analyze`是有意义的。这将确保新数据会立即有统计数据，以便进行高效的查询。一旦你运行了它，自动吸尘程序就会知道不要再吸尘了。

# 看看并行查询

Postgres，当它可以的时候，将并行运行部分查询。这是仓储应用的理想选择。并行查询增加了一点延迟(必须产生工作人员，然后将他们的结果一起返回)，但对于分析工作负载来说，这通常无关紧要，因为查询需要几秒钟。

实际上，并行查询大大加快了表或索引扫描的速度，这也是我们的查询需要花费大量时间的地方。

查看它是否按预期运行的最好方法是使用`explain`。您应该会看到一个`Gather`后跟一些并行工作(连接、排序、索引扫描、序列扫描等)

```
-> Gather Merge (cost=2512346.78..2518277.16 rows=40206 width=60) Workers Planned: 2 Workers Launched: 2 ... -> Parallel Seq Scan on activity_stream s_1
```

工作者是并行执行工作的进程的数量。工人数量由两个设置控制: [max_parallel_workers](https://www.postgresql.org/docs/11/runtime-config-resource.html#GUC-MAX-PARALLEL-WORKERS) 和[max _ parallel _ workers _ per _ gather](https://www.postgresql.org/docs/11/runtime-config-resource.html#GUC-MAX-PARALLEL-WORKERS-PER-GATHER)

```
show max_parallel_workers; -- total number of workers allowed show max_parallel_workers_per_gather; -- num workers at a time on the query
```

如果你使用`explain(analyze, verbose)`，你可以看到每个工人花费了多少时间，处理了多少行。如果数字大致相等，那么并行工作可能会有所帮助。

```
Worker 0: actual time=13093.096..13093.096 rows=8405229 loops=1 Worker 1: actual time=13093.096..13093.096 rows=8315234 loops=1
```

值得尝试不同的查询并调整`max_parallel_workers_per_gather`的数量来看看效果。根据经验，Postgres 作为一个仓库比作为一个生产系统更能受益于更多的工人。

# 增加统计抽样

Postgres 在一个表上收集统计信息，以通知查询规划器。它通过对表进行采样并存储(除其他外)最常见的值来实现这一点。需要的样本越多，查询规划器就越精确。对于分析性工作负载，运行时间较长的查询较少，增加 Postgres 收集的数据量会有所帮助。

这可以在每列的基础上完成

`ALTER TABLE table_name ALTER COLUMN column_name set statistics 500;`

增加列的统计信息

或者针对整个数据库

`ALTER DATABASE mydb SET default_statistics_target = 500;`

增加数据库的统计数据

默认值为 100；任何高于 100 到 1000 的值都是好的。请注意，这是应该测量的设置之一。在一些常见的查询上使用`EXPLAIN ANALYZE`,看看查询规划器错误估计了多少。

# 使用较少的列

这只是需要注意的一点。Postgres 使用基于行的存储，这意味着行在磁盘上是按顺序排列的。它实际上存储整个第一行(及其所有列)，然后存储整个第二行，依此类推。

这意味着当您从一个有很多列的表中选择相对较少的列时，Postgres 将加载大量它不会使用的数据。所有的表数据都是以固定大小(通常为 4KB)的块读取的，所以它不能只是有选择地从磁盘中读取一行的几列。

相比之下，大多数专用数据仓库是列存储，只能读取所需的列。

> *注意:不要用每个查询都需要连接的多个表替换单个宽表。可能会慢一些(尽管总是要测量)。*

这更像是一条经验法则——在所有条件相同的情况下，最好选择更少的列。在实践中，性能提升通常不会很显著。

# 考虑一个大规模的数据仓库

Postgres 和基于云的数据仓库之间的最后一个主要区别是极端的规模。与 Postgres 不同，它们是作为分布式系统从头开始构建的。这允许他们随着数据大小的增长相对线性地增加更多的处理能力。

当数据库变得太大，应该转移到分布式系统时，我没有一个好的经验法则。但是当您到达那里时，您可能已经具备了处理迁移和理解权衡的专业知识。

在我的非正式测试中，使用 50-100m 行的表，Postgres 表现得非常好——大体上符合红移之类的东西。但是，性能取决于许多因素——磁盘与 ssd、CPU、数据结构、查询类型等，如果不进行一些面对面的测试，真的不可能一概而论。

如果您要将 Postgres 扩展为数十亿行，Citus 是值得考虑的。

*原载于 2021 年 5 月 10 日* [*https://www .叙述者. ai*](https://www.narrator.ai/blog/using-postgresql-as-a-data-warehouse/) *。*