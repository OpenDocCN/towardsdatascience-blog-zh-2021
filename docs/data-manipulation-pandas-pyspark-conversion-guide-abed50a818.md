# 数据操作熊猫-PySpark 转换指南

> 原文：<https://towardsdatascience.com/data-manipulation-pandas-pyspark-conversion-guide-abed50a818?source=collection_archive---------32----------------------->

## 使用 PySpark on Databricks 进行探索性数据分析(EDA)所需的一切

![](img/ca705709bd91a9310cedf0cd600e9c58.png)

萨米·米提特鲁多在 [Unsplash](https://unsplash.com/s/photos/red-and-white-bricks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在处理大数据时，Jupyter notebook 会失败。这也是一些公司使用 ML 平台如 Databricks，SageMaker，Alteryx 等的一个原因。一个好的 ML 平台支持从数据摄取到建模和监控的整个机器学习生命周期，从而提高团队的生产力和效率。在这个简单的教程中，我将分享我关于将 Pandas 脚本转换为 Pyspark 的笔记，这样您也可以无缝地转换这两种语言！

# 介绍

## **什么是火花？**

Spark 是一个开源的云计算框架。这是一个可扩展的、大规模并行的内存执行环境，用于运行分析应用。Spark 是处理 Hadoop 数据的快速而强大的引擎。它通过 Hadoop YARN 或 Spark 的独立模式运行在 Hadoop 集群中，它可以处理 HDFS、HBase、Cassandra、Hive 和任何 Hadoop InputFormat 中的数据。它旨在执行一般的数据处理(类似于 MapReduce)和新的工作负载，如流、交互式查询和机器学习。你可以在这里阅读更多关于 Mapreduce 的内容:[https://medium . com/@ francescomandru/Mapreduce-explained-45a 858 C5 ac1d](https://medium.com/@francescomandru/mapreduce-explained-45a858c5ac1d)(P . S 我最喜欢的解释之一！)

## **什么是数据块？**

Databricks 为您的所有数据提供了一个统一的开放平台。它为数据科学家、数据工程师和数据分析师提供了一个简单的协作环境。就数据服务而言，它是市场领导者之一。它由 Ali Gozii 于 2013 年创建，他是阿帕奇火花三角洲湖和 MLflow 的原始创造者之一。它来自一些世界上最受欢迎的开源项目的原始创建者，Apache Spark、Delta Lake、MLflow 和 Koalas。它建立在这些技术之上，提供了一个真正的湖屋架构，结合了最好的数据湖和数据仓库，形成了一个快速、可扩展和可靠的平台。专为云构建，您的数据存储在低成本的云对象存储中，如 AWS s3 和 Azure data lake storage，通过缓存、优化的数据布局和其他技术实现性能访问。您可以启动包含数百台机器的集群，每台机器都混合了您的分析所需的 CPU 和 GPU。

# 主要概念

**探索数据-** 下表总结了用于获得数据概览的主要函数。

```
 Pandas      |            PySpark               
 -------------------------|------------------------------------ 
  pd.read_csv(path)       | spark.read.csv(path)
  df.shape                | print(df.count(), len(df.columns)) 
  df.head(10)             | df.limit(10).toPandas()            
  df[col].isnull().sum()  | df.where(df.col.isNull()).count()
  df                      | Display(df)
```

# 数据预处理

*   **删除重复项-** 删除所选多列中的重复行
*   **过滤-** 我们可以根据一些条件过滤行
*   **更改列-** 更改列名，转换数据类型，创建新列
*   **条件列-****列可以使用下面的 R 命令针对一组特定的条件取不同的值**
*   ****排序-** 对数据进行排序**
*   ****日期时间转换-** 包含日期时间值的字段从字符串转换为日期时间**
*   ****Groupby -** 数据帧可以根据给定的列进行聚合**
*   ****Join()** -将列转换为逗号分隔的列表**
*   ****填充 nan 值-** 用值替换列中缺失的空值**

```
 Pandas       |              PySpark               
 -------------------------|------------------------------------         
  df.drop_duplicates()    | df.dropDuplicates()
  df.drop(xx,axis=1)      | df.drop(xx)
  df[['col1','col2']]     | df.select('col1','col2')
  df[df.isin(xxx)]        | df.filter(df.xx.isin()).show()
  df.rename(columns={})   | df.withColumnRenamed('col1','col2')
  df.x.astype(str)        |df.withColumn(x,col(x).cast(StringType())
  df.sort_values()        | df.sort() or df.orderby()
  np.datetime64('today')  | current_date()
  pd.to_datetime()        | to_date(column, time_format)
  df.groupby()            | df.groupBy()
  ','.join()         |','.join(list(df.select[col].toPandas()[col]))
  df.fillna(0)            | df.x.fill(value=0)
```

# **数据帧转换**

*   ****合并数据帧-** 我们可以根据给定的字段合并两个数据帧**
*   ****连接数据帧-** 将两个或多个数据帧连接成一个数据帧**

```
 Pandas        |             PySpark               
 -------------------------|------------------------------------ 
  df1.join(df2, on=,how=) | df1.join(df2,df1.id=df2.id, how='')
  pd.concat([df1, df2])   | df1.unionAll(df1,df2)
```

# **结论**

**希望这个简单的对比备忘单可以帮助你更快的上手 PySpark 和 Databricks。以上对比只是起点！更多细节和例子，请查看这个 [Pyspark 初学者教程页面](https://sparkbyexamples.com/)。当谈到学习任何新的语言或工具时，最好的学习方法就是实践。我强烈建议你自己仔细阅读 pyspark 代码，甚至可以在 [Databricks](https://databricks.com/) 上开始一个项目！😉**

**如果你觉得这很有帮助，请关注我，看看我的其他博客。敬请关注更多内容！❤**

**</10-tips-to-land-your-first-data-science-job-as-a-new-grad-87ecc06c17f7>  </how-to-prepare-for-business-case-interview-as-an-analyst-6e9d68ce2fd8>  </10-questions-you-must-know-to-ace-any-sql-interviews-2faa0a424f07> **