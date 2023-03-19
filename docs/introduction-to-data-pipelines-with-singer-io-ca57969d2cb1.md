# Singer.io 数据管道简介

> 原文：<https://towardsdatascience.com/introduction-to-data-pipelines-with-singer-io-ca57969d2cb1?source=collection_archive---------16----------------------->

数据管道在各种数据平台中扮演着至关重要的角色，无论是预测分析还是商业智能，或者仅仅是各种异构数据存储之间的 ETL(提取—传输—加载)。它们都依赖于实时或批量接收数据，并进一步处理这些数据以获得见解和做出预测，或者只是总结和重塑数据以用于报告。让我们探索数据管道的解剖结构，以便更好地理解这个概念，然后我们将开始使用 Python 中称为 [Singer](https://www.singer.io/) 的开源 ETL 工具创建数据管道。

![](img/7348a08f81b0c5d70eeea6c1b68e1836.png)

[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 什么是数据管道？

就像水管将水从水源输送到目的地一样，我们也使用数据管道将数据从一个源系统输送到目的地系统。使用数据管道的几种情况:

1)在异构数据存储之间同步数据。例如，将数据从 Oracle 数据库实例同步到 PostgreSQL 数据库实例。

2)在数据清理、整形、汇总等之后，将数据从暂存区移动到生产系统。例如，从各种网站收集的数据可以被收集在暂存区中，该暂存区然后以结构化的方式适合消费，然后被传送到数据库。

3)从中央数据源将分段数据分布到各个子系统。例如:由一个组织在 Google Sheets 中收集的各种产品的调查数据可能会被划分并广播给相应的产品团队进行处理。

# 数据管道的关键特征

**1)数据频率:**目标系统要求数据的速度，即定期小批量或实时。管道应该能够保持目标系统所需的数据传输频率。

**2)弹性:**数据管道的容错性和弹性如何，即如果管道因突然的数据加载或被忽略的代码错误而崩溃，数据不应丢失。

**3)可扩展性:**如果数据负载增加，用于开发数据管道的工具和技术必须能够重新配置数据管道，以扩展到更多的硬件节点。

# 什么是*歌手*？

*歌手*的核心目标是成为“编写移动数据脚本的开源标准”。它涉及使用标准化脚本进行数据提取和接收，可以根据需要与各种源/目标混合和匹配。

## 歌手的核心特征

1)Tap——从中提取数据的数据源称为 Tap，Singer 网站上有现成的 Tap 可供使用，也可以创建自定义 Tap。

2)目标—从 Tap 获取数据的数据目标称为目标。与 Taps 一样，我们可以使用歌手网站上的现成目标，也可以创建自己的目标。

3)数据交换格式——在 singer 中，JSON (JavaScript 对象符号)格式被用作数据交换格式，使其与源/目标无关。

Taps 和目标很容易与基于 Unix 的管道操作符结合在一起，而不需要任何守护程序或复杂的插件。

5)它通过维护调用之间的状态来支持增量提取，即时间戳可以在调用之间存储在 JSON 文件中，以记录目标消耗数据的最后实例。

## Singer 中数据管道的组件

我们可以根据我们的要求为歌手定制水龙头/水槽，或者只安装和使用歌手网站上已经有的[水龙头/水槽。](https://www.singer.io/#taps)

# 用 Singer 构建数据管道

让我们创建一个数据管道，从 REST API 获取雇员记录，并使用 Singer 的 tap 和 sink 方法将它们插入 PostgreSQL 数据库表。

> 先决条件:Python 环境和 PostgreSQL 数据库实例。

由于 Singer.io 需要设置 tap 和 sink，我们将为两者创建单独的 virtualenvs，以避免版本冲突，因为它们中的每一个都可能依赖于不同的库。这被认为是与 Singer 合作的最佳实践。

## 设置 Tap: Rest API

在这个演示中，我们将创建自己的 Tap 来从 REST API 中获取雇员记录。

1)创建并激活虚拟

```
python3 -m venv ~/.virtualenvs/Singer.io_rest_tapsource ~/.virtualenvs/Singer.io_rest_tap/bin/activate
```

2)安装 Singer python 库

```
pip install singer-python
```

3)在您喜欢的编辑器中打开一个名为 tap_emp_api.py 的新文件，并添加以下代码

```
import singerimport urllib.requestimport json#Here is a dummy JSON Schema for the sample data passed by our REST API.schema = {‘properties’: {‘id’: {‘type’: ‘string’},‘employee_name’: {‘type’: ‘string’},‘employee_salary’: {‘type’: ‘string’},‘employee_age’: {‘type’: ‘string’},‘profile_image’: {‘type’: ‘string’}}}#Here we make the HTTP request and parse the responsewith urllib.request.urlopen(‘http://dummy.restapiexample.com/api/v1/employees') as response:emp_data = json.loads(response.read().decode(‘utf-8’))#next we call singer.write_schema which writes the schema of the employees streamsinger.write_schema(‘employees’, schema, ‘id’)#then we call singer.write_records to write the records to that streamsinger.write_records(‘employees’, records=emp_data[“data”])
```

## 设置目标:PostgreSQL

PostgreSQL 的一个目标已经在 Singer 网站上可用，所以我们将设置一个 virtualenv 来安装它。

1)创建并激活虚拟

```
python3 -m venv ~/.virtualenvs/Singer.io_postgres_targetsource ~/.virtualenvs/Singer.io_ postgres_target /bin/activate
```

2)安装 Singer python 库

```
pip install singer-target-postgres
```

3)接下来，我们需要在以下位置创建一个配置文件

> *~/。virtualenvs/singer . io _ postgres _ target/bin/target _ postgres _ config . JSON*

使用 postgres 连接信息和目标 postgres 模式，如下所示:

```
{“postgres_host”: “localhost”,“postgres_port”: 5432,“postgres_database”: “singer_demo”,“postgres_username”: “postgres”,“postgres_password”: “postgres”,“postgres_schema”: “public”}
```

# 使用 Singer 数据管道

一旦我们设置了龙头和目标，就可以通过简单地敲击由壳体中的管道操作者分开的龙头和目标来使用管道，即

```
python ~/.virtualenvs/Singer.io_rest_tap/bin/tap_emp_api.py | ~/.virtualenvs/Singer.io_postgres_target/bin/target-postgres — config database_config.json
```

这就对了。根据我们为 Tap 创建的 database_config 文件和模式，从 API 获取的所有记录都将被插入到一个表中。很简单，不是吗？！

## 下一步是什么？

浏览 [Singer.io](https://www.singer.io) 文档，根据您的使用案例尝试各种点击/目标组合，或者创建您自己的点击和目标。

您还可以探索数据管道编排工具，如[气流](https://airflow.apache.org/)和 [Luigi](https://github.com/spotify/luigi) ，它们也具有广泛的功能。

在我即将发布的帖子中，我会提供一些关于气流的有趣指南。直到那时..保持快乐的管道您的数据管道！