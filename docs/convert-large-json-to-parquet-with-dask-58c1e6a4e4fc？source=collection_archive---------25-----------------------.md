# 使用 Dask 将大型 JSON 转换为拼花地板

> 原文：<https://towardsdatascience.com/convert-large-json-to-parquet-with-dask-58c1e6a4e4fc?source=collection_archive---------25----------------------->

## 使用 Coiled 将 75GB 数据集上的 ETL 管道扩展到云

![](img/b15a9062c117e6169a0e73c0c413545e.png)

图片由阿德里安·匡威通过[unsplash.com](http://unsplash.com/)拍摄

# TL；博士；医生

这篇文章演示了一个 75GB 数据集的 JSON 到 Parquet 的转换，它无需将数据集下载到本地机器上就可以运行。首先在本地迭代 Dask 来构建和测试你的管道，然后将相同的工作流程转移到云计算服务中，如 [Coiled](http://coiled.io/) 和最小的代码变更。

*免责声明:我在 Coiled 工作，是一名数据科学传道者实习生。* [*Coiled*](http://coiled.io/) *由*[*Dask*](https://dask.org/)*的最初作者 Matthew Rocklin 创立，是一个面向分布式计算的开源 Python 库。*

# 为什么要将 JSON 转换成 Parquet

从 web 上抓取的嵌套 JSON 格式的数据通常需要转换成表格格式，用于探索性数据分析(EDA)和/或机器学习(ML)。Parquet 文件格式是存储表格数据的最佳方法，允许像列修剪和谓词下推过滤这样的操作，这[极大地提高了工作流的性能](https://coiled.io/blog/parquet-column-pruning-predicate-pushdown/)。

本文展示了一个 JSON 到 Parquet 的管道，用于 Github Archive 项目中的 75GB 数据集，使用 Dask 和 Coiled 将数据转换并存储到云对象存储中。该管道不需要将数据集本地存储在您的计算机上。

完成这篇文章后，你将能够:

1.  首先在本地构建并测试您的 ETL 工作流，使用一个简单的测试文件，这个文件可以很容易地放入本地机器的内存中。
2.  使用 Coiled 将相同的工作流扩展到云中，以处理整个数据集。

*剧透——y*你将在两种情况下运行完全相同的代码，只是改变了计算运行的位置。

你可以在本笔记本中找到完整的代码示例。要在本地运行笔记本，使用位于笔记本存储库中的 environment.yml 文件构建 conda 环境。

# 在本地构建您的管道

首先在本地构建管道是一个很好的实践。上面链接的笔记本一步一步地引导你完成这个过程。我们将在这里总结这些步骤。

我们将在 2015 年使用来自 [Github 档案项目](https://www.gharchive.org/)的数据。这个数据集记录了 Github 上的所有公共活动，在未压缩的情况下占用大约 75GB。

## 1.

首先从 Github 档案中提取一个文件。这代表 1 小时的数据，占用大约 5MB 的数据。这里不需要使用任何类型的并行或云计算，所以现在可以在本地迭代。

```
!wget [https://data.gharchive.org/2015-01-01-15.json.gz](https://data.gharchive.org/2015-01-01-15.json.gz)
```

*只有在必要时才扩展到云，以避免不必要的成本和代码复杂性。*

## 2.

太好了，你已经从源头提取了数据。现在，您可以将它转换成表格数据帧格式。数据中有几个不同的模式重叠，这意味着您不能简单地将其转换为 pandas 或 Dask 数据框架。相反，您可以过滤掉一个子集，比如 PushEvents，并使用它。

```
records.filter(lambda record: record["type"] == "PushEvent").take(1)
```

您可以应用 process 函数(在笔记本中定义)将嵌套的 JSON 数据展平成表格格式，现在每一行代表一个 Github 提交。

```
records.filter(lambda record: record["type"] == "PushEvent").take(1)flattened = records.filter(lambda record: record["type"] ==   
  "PushEvent").map(process).flatten()
```

然后使用`.to_dataframe()`方法将这些数据转换成数据帧。

```
df = flattened.to_dataframe()
```

## 3.

现在，您已经准备好使用 Dask DataFrame `.to_parquet()`方法将数据帧作为. parquet 文件写入本地目录。

```
df.to_parquet( "test.parq", engine="pyarrow", compression="snappy" )
```

# 利用卷绕式 Dask 集群进行横向扩展

伟大的工作建立和测试你的工作流程在本地！现在让我们构建一个工作流，该工作流将收集一整年的数据，对其进行处理，并将其保存到云对象存储中。

我们将首先在云中启动一个 Dask 集群，它可以在整个数据集上运行您的管道。要运行本节中的代码，您需要登录 [Coiled Cloud](http://cloud.coiled.io) 获得一个免费的 Coiled 帐户。你只需要提供你的 Github 凭证来创建一个帐户。

然后，您需要用正确的库创建一个软件环境，以便集群中的工作人员能够执行我们的计算。

```
import coiled # create Coiled software environment coiled.create_software_environment(
    name="github-parquet", 
    conda=["dask", "pyarrow", "s3fs", "ujson", "requests", "lz4", "fastparquet"]
)
```

*您还可以使用 Docker images、environment.yml (conda)或 requirements.txt (pip)文件创建 Coiled 软件环境。更多信息，请查看* [*盘绕文档*](https://docs.coiled.io/user_guide/software_environment_creation.html) *。*

现在，让我们启动您的盘绕式集群，指定集群名称、它运行的软件环境以及 Dask 工作线程的数量。

```
# spin up a Coiled cluster 
cluster = coiled.Cluster(
    name="github-parquet", 
    software="coiled-examples/github-parquet", 
    n_workers=10
)
```

最后，让 Dask 在您的盘绕式集群上运行计算。

```
# connect Dask to your Coiled cluster 
from dask.distributed import Client 
client = Client(cluster) client
```

我们期待已久的时刻！您的集群已经启动并运行，这意味着您已经准备好运行您在整个数据集上构建的 JSON to Parquet 管道。

这需要对您的代码进行两处细微的更改:

1.  下载*Github 的所有*存档文件，而不仅仅是一个测试文件
2.  将 df.to_parquet()指向 s3 存储桶，而不是本地存储桶

请注意，下面的代码使用了一个文件名列表，其中包含 2015 年的所有文件和上面提到的流程函数。关于这两个物体的定义，请参考[笔记本](https://github.com/coiled/coiled-resources/blob/main/json-to-parquet/github-parquet.ipynb)。

```
%%time 
# read in json data 
records = db.read_text(filenames).map(ujson.loads) # filter out PushEvents 
push = records.filter(lambda record: record["type"] == "PushEvent") # process into tabular format, each row is a single commit 
processed = push.map(process) # flatten and cast to dataframe 
df = processed.flatten().to_dataframe() # write to parquet 
df.to_parquet( 's3://coiled-datasets/etl/test.parq', engine='pyarrow', compression='snappy' ) 
CPU times: user 15.1 s, sys: 1.74 s, total: 16.8 s 
Wall time: 19min 17s
```

太好了，这很有效。但是让我们看看是否可以加快一点速度…

让我们纵向扩展我们的集群以提升性能。我们将使用`cluster.scale()`命令将集群中的工作线程数量增加一倍。我们还将包括一个对`client.wait_for_workers()`的调用，它将阻塞活动，直到所有的工人都在线。这样，我们就可以确信我们在计算中已经竭尽全力了。

```
# double n_workers 
cluster.scale(20) # this blocks activity until the specified number of workers have joined the cluster 
client.wait_for_workers(20)
```

现在，让我们在扩展后的集群上重新运行相同的 ETL 管道。

```
%%time 
# re-run etl pipeline 
records = db.read_text(filenames).map(ujson.loads) 
push = records.filter(lambda record: record["type"] == "PushEvent") processed = push.map(process) 
df = processed.flatten().to_dataframe() 
df.to_parquet( 's3://coiled-datasets/etl/test.parq', engine='pyarrow', compression='snappy' ) CPU times: user 11.4 s, sys: 1.1 s, total: 12.5 s 
Wall time: 9min 53s
```

我们已经将运行时间减少了一半，干得好！想象一下，如果我们将 n_workers 增加 10 倍或 20 倍，会发生什么！

# 将大型 JSON 转换为拼花摘要

在这个笔记本中，我们将原始 JSON 数据转换成扁平的数据帧，并以高效的 Parquet 文件格式存储在云对象存储中。我们首先在本地的单个测试文件上执行这个工作流。然后，我们使用 Coiled 上的 Dask 集群将相同的工作流扩展到云上运行，以处理整个 75GB 的数据集。

主要要点:

*   Coiled 允许您将常见的 ETL 工作流扩展到大于内存的数据集。
*   仅在需要时扩展到云。云计算带来了自己的一系列挑战和开销。因此，要战略性地决定是否以及何时导入 Coiled 和 spin up 集群。
*   纵向扩展您的集群以提高性能。通过将我们的集群从 10 个工人扩展到 20 个工人，我们将 ETL 功能的运行时间减少了一半。

我希望这篇文章对你有帮助！[在 Twitter 上关注我](https://twitter.com/richardpelgrim)获取每日数据科学内容。

或者来我的博客打招呼:

[](https://crunchcrunchhuman.com/2021/12/22/kaggle-xgboost-distributed-cloud/) [## 在 20 秒内对 20GB 数据进行 XGBoost 训练

### 如果您正在寻找方法来更快地训练 XGBoost 模型，或者在大于您的机器内存的数据集上训练，这里有一个…

crunchcrunchhuman.com](https://crunchcrunchhuman.com/2021/12/22/kaggle-xgboost-distributed-cloud/) 

*原载于 2021 年 9 月 15 日*[*https://coiled . io*](https://coiled.io/blog/convert-large-json-to-parquet-with-dask/)*。*