# 谷歌 Colab 101 上的 PySpark

> 原文：<https://towardsdatascience.com/pyspark-on-google-colab-101-d31830b238be?source=collection_archive---------8----------------------->

## 使用 Google Colab 的 PySpark 初学者实践指南

![](img/5c63acf00277c662fb96b2ff7fafea0e.png)

克里斯里德在 [Unsplash](https://unsplash.com/s/photos/python-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Apache Spark 是一个用于数据处理的闪电般快速的框架，可以在大规模数据集上执行超快速的处理任务。它还可以独立地或与其他分布式计算工具协作，将数据处理任务分布在多个设备上。

PySpark 是使用 Python 编程语言访问 Spark 的接口。PySpark 是用 python 开发的 API，用于 Spark 编程和用 Python 风格编写 spark 应用程序，尽管底层执行模型对于所有 API 语言都是相同的。

Google 的 Colab 是一个基于 Jupyter Notebook 的非常强大的工具。由于它运行在谷歌服务器上，我们不需要在我们的系统中本地安装任何东西，无论是 Spark 还是任何深度学习模型。

在本文中，我们将看到如何在 Google 协作笔记本中运行 PySpark。我们还将执行大多数数据科学问题中常见的一些基本数据探索任务。所以，让我们开始吧！

*注意——我假设你已经熟悉 Python、Spark 和 Google Colab 的基础知识。*

# 在 Colab 建立 PySpark

Spark 是用 Scala 编程语言编写的，需要 Java 虚拟机(JVM)才能运行。所以，我们的首要任务是下载 Java。

```
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
```

接下来，我们将用 **Hadoop 2.7** 下载并解压 **Apache Spark** 进行安装。

*注意——对于本文，我正在下载 Spark 的****3 . 1 . 2****版本，这是目前最新的稳定版本。如果这一步失败了，那么很可能 spark 的新版本已经取代了它。所以，上网查查他们的最新版本，然后用那个代替。*

```
!wget -q [https://www-us.apache.org/dist/spark/spark-3.1.2/spark-3.1.2-bin-hadoop2.7.tgz](https://www-us.apache.org/dist/spark/spark-3.1.1/spark-3.1.1-bin-hadoop2.7.tgz)!tar xf spark-3.1.2-bin-hadoop2.7.tgz
```

现在，是时候设置“环境”路径了。

```
import os
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-8-openjdk-amd64"
os.environ["SPARK_HOME"] = "/content/spark-3.1.2-bin-hadoop2.7"
```

然后我们需要安装并导入'[**find spark**](https://pypi.org/project/findspark/)**'**库，它将在系统上定位 Spark 并将其作为常规库导入。

```
!pip install -q findsparkimport findspark
findspark.init()
```

现在，我们可以从 pyspark.sql 导入 sparkSession 并创建一个 SparkSession，这是 Spark 的入口点。

```
from pyspark.sql import SparkSession
spark = SparkSession.builder\
        .master("local")\
        .appName("Colab")\
        .config('spark.ui.port', '4050')\
        .getOrCreate()
```

就是这样！现在让我们从 PySpark 开始吧！

![](img/2c22689f41cbb9aa445a747bd807520b.png)

迈克·范·登博斯在 [Unsplash](https://unsplash.com/s/photos/loading?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 将数据加载到 PySpark

Spark 有多种模块可以读取不同格式的数据。它还自动确定每一列的数据类型，但是它必须检查一次。

对于本文，我在 Github 中创建了一个样本 JSON 数据集。您可以使用“**wget”**命令将文件直接下载到 Colab 中，如下所示:

```
!wget --continue https://raw.githubusercontent.com/GarvitArya/pyspark-demo/main/sample_books.json -O /tmp/sample_books.json
```

现在使用**读取**模块将该文件读入 Spark 数据帧。

```
df = spark.read.json("/tmp/sample_books.json")
```

现在是时候使用 PySpark dataframe 函数来研究我们的数据了。

# **使用 PySpark 进行探索性**数据分析

## 让我们来看看它的模式:

在对数据集进行任何切片之前，我们应该首先了解它的所有列及其数据类型。

```
df.printSchema()**Sample Output:** root  
  |-- author: string (nullable = true)  
  |-- edition: string (nullable = true)  
  |-- price: double (nullable = true)  
  |-- title: string (nullable = true)  
  |-- year_written: long (nullable = true)
```

## 给我看一些样品:

```
df.show(4,False)**Sample Output:** +----------------+---------------+-----+--------------+------------+
|author          |edition        |price|title         |year_written|
+----------------+---------------+-----+--------------+------------+
|Tolstoy, Leo    |Penguin        |12.7 |War and Peace |1865        | 
|Tolstoy, Leo    |Penguin        |13.5 |Anna Karenina |1875        | 
|Woolf, Virginia |Harcourt Brace |25.0 |Mrs. Dalloway |1925        |
|Dickens, Charles|Random House   |5.75 |Bleak House   |1870        |
+----------------+---------------+-----+--------------+------------+
```

## 数据集有多大:

```
df.count()**Sample Output:** 13
```

## 选择几个感兴趣的列:

```
df.select(“title”, “price”, “year_written”).show(5)**Sample Output:**
+----------------+-----+------------+ 
|           title|price|year_written| 
+----------------+-----+------------+ 
|Northanger Abbey| 18.2|        1814| 
|   War and Peace| 12.7|        1865| 
|   Anna Karenina| 13.5|        1875| 
|   Mrs. Dalloway| 25.0|        1925| 
|       The Hours|12.35|        1999| 
+----------------+-----+------------+ 
```

## 过滤数据集:

```
**# Get books that are written after 1950 & cost greater than $10**df_filtered = df.filter("year_written > 1950 AND price > 10 AND title IS NOT NULL")df_filtered.select("title", "price", "year_written").show(50, False)**Sample Output:** +-----------------------------+-----+------------+ 
|title                        |price|year_written| 
+-----------------------------+-----+------------+ 
|The Hours                    |12.35|1999        | 
|Harry Potter                 |19.95|2000        | 
|One Hundred Years of Solitude|14.0 |1967        | 
+-----------------------------+-----+------------+**# Get books that have Harry Porter in their title**df_filtered.select("title", "year_written").filter("title LIKE '%Harry Potter%'").distinct().show(20, False)**Sample Output:** +------------+------------+ 
|title       |year_written| 
+------------+------------+ 
|Harry Potter|2000        | 
+------------+------------+
```

## 使用 Pyspark SQL 函数:

```
from pyspark.sql.functions import max**# Find the costliest book** maxValue = df_filtered.agg(max("price")).collect()[0][0]
print("maxValue: ",maxValue)df_filtered.select("title","price").filter(df.price == maxValue).show(20, False)**Sample Output:**
maxValue:  29.0 
+-----------------------------+------+ 
|title                        |price | 
+-----------------------------+------+ 
|A Room of One's Own          |29.0  | 
+-----------------------------+------+
```

# 结束注释

我希望你和我写这篇文章时一样喜欢在 Colab 与 PySpark 一起工作！您可以在我的 Github 资源库-[https://github.com/GarvitArya/pyspark-demo](https://github.com/GarvitArya/pyspark-demo)找到这个完整的工作示例 Colab 文件。

▶️ *请在下面的评论中提出任何问题/疑问或分享任何建议。*

▶️ *如果你喜欢这篇文章，那么请考虑关注我&把它也分享给你的朋友:)*

▶️ *可以联系我在—*[*LinkedIn*](https://www.linkedin.com/in/garvitarya/)*|*[*Twitter*](https://twitter.com/garvitishere)*|*[*github*](https://github.com/GarvitArya/)*|*[*insta gram*](https://www.instagram.com/garvitarya/)*|*[*Facebook*](https://www.facebook.com/garvitishere)

*![](img/26eb9950ccaeafba69e8f996cfb2dedb.png)*

*在 [Unsplash](https://unsplash.com/s/photos/thank-you?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[Courtney hedge](https://unsplash.com/@cmhedger?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片*