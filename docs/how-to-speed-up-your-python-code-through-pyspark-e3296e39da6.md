# 如何通过 PySpark 加速您的 Python 代码

> 原文：<https://towardsdatascience.com/how-to-speed-up-your-python-code-through-pyspark-e3296e39da6?source=collection_archive---------19----------------------->

## 环境设置

## 关于如何安装和运行 Apache Spark 和 PySpark 以提高代码性能的教程。

![](img/881f06a5e00ea950cdaba61584d055e2.png)

图片由[拍摄](https://pixabay.com/users/taken-336382/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1098059)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1098059)

当你处理巨大的数据集时，**瓶颈不是你的代码**(我希望如此…)，**而是对你的数据集执行某些操作所耗费的时间**。出于这个原因，开发一些可以加速代码的库是非常重要的。

**阿帕奇 Spark 或许能帮到你**。Apache Spark 是一个开源项目，相对于标准技术，它可以将工作负载加速 100 倍。它可以在分布式环境中工作(**集群**)，但也可以在本地使用，例如当您的机器中有多个处理器时。

实际上，在 Apache Spark 中有两种类型的节点:主节点**和许多工作节点**，主节点是集群的主计算机。主服务器组织工作并在工人之间分配，然后检索结果。

在本教程中，我涉及以下几个方面:

*   下载并安装 Apache Spark
*   安装 PySpark 以配置 Python 来与 Apache Spark 一起工作
*   运行一个简单示例

# 下载并安装 Apache Spark

Apache Spark 可以从其官方网站下载:

<https://spark.apache.org/downloads.html>  

你可以选择火花释放和包装时间。如果您没有任何特殊需求，可以下载 Apache Hadoop 的最新版本和预编译版本。下载后，您可以将其移动到您的首选目录，并用一个较短的名称重命名。

Apache Spark 可以通过 **PySpark 包**与 Python 结合使用。

Apache Spark 也需要安装 Java。Java 的兼容版本从 8 到 11 不等。不支持其他版本。

# 安装 PySpark

现在您可以安装 PySpark，例如通过`pip`管理器:

```
pip install pyspark
```

安装完成后，您需要配置`SPARK_HOME`并修改您的`.bash_profile`或`.profile`文件中的`PATH`变量。该文件是隐藏的，位于您的主目录中。您可以打开它，并在文件末尾添加以下代码行:

```
export SPARK_HOME="**/path/to/spark**/spark"
export PATH="$SPARK_HOME/python:$PATH"
```

您可以保存文件并启动终端。您可以输入以下命令:

```
pyspark
```

`pyspark`终端启动。要退出它，只需写下`quit()`并按回车键。

**作为选项，您可以配置** `**pyspark**` **使用 Jupyter 笔记本。**

在这种情况下，您可以使用`findspark`包，它会为您搜索 Spark 在哪里。实际上，`findspark`包会从你的概要文件中读取`SPARK_HOME`目录。

您可以通过以下命令安装`findspark`:

```
pip install findspark
```

安装完成后，您可以启动 Jupyter notebook，并在代码开头添加以下代码行:

```
import findspark
findspark.init()
```

# 简单的例子

现在，您已经准备好运行您的第一个`pyspark` 示例了。首先，您可以创建一个`SparkContext`，它对应于您的集群的主节点。您可以指定一些配置参数，例如应用程序名称(`myproject`)和 url ( `local`):

```
from pyspark import SparkContext, SparkConfconf = SparkConf().setMaster("local").setAppName("myproject")
sc = SparkContext.getOrCreate(conf=conf)
```

`getOrCreate()`函数创建一个新的`SparkContext`，如果它还不存在，否则它检索现有的。这是因为只能有一个运行中的`SparkContext`实例。

现在您可以创建一个 Spark 数据帧，它的行为很像一个 SQL 表。火花数据帧可从`SparkContext`对象创建，如下所示:

```
from pyspark.sql import SparkSessionspark = SparkSession.builder.getOrCreate()
```

现在您可以使用`spark`对象来读取 CSV 文件:

```
df = spark.read.csv("/path/to/your/csv/file",inferSchema=True, header=True)
```

`inferSchema=True`参数允许自动识别数据类型，但需要更多时间。您可以通过`show()`函数列出数据帧的第一行:

```
df.show()
```

结果看起来像一个 SQL 表。

您可以通过经典的 SQL 查询来查询数据帧。为此，首先必须将新的数据帧注册到可用表列表中:

```
df_sql = df.createOrReplaceTempView('df_name')
```

其中`df_name`是您希望在 SQL 中使用的表的名称。您可以列出所有可用的表格:

```
print(spark.catalog.listTables())
```

现在，您可以通过以下代码查询您的表:

```
query = 'SELECT count(*) FROM df_name'
sc.sql(query)
```

# 摘要

在本教程中，我演示了如何在您的计算机上安装和运行 Apache Spark 和 PySpark。此外，我还举例说明了如何将 CSV 文件读入 Spark DataFrame。

如果你想了解我的研究和其他活动的最新情况，你可以在 [Twitter](https://twitter.com/alod83) 、 [Youtube](https://www.youtube.com/channel/UC4O8-FtQqGIsgDW_ytXIWOg?view_as=subscriber) 和 [Github](https://github.com/alod83) 上关注我。

# 相关著作

</how-to-load-huge-csv-datasets-in-python-pandas-d306e75ff276>  <https://medium.com/geekculture/the-top-25-python-libraries-for-data-science-71c0eb58723d>  </how-to-install-python-and-jupyter-notebook-onto-an-android-device-900009df743f> [## 如何在 Android 设备上安装 Python 和 Jupyter Notebook

towardsdatascience.com](/how-to-install-python-and-jupyter-notebook-onto-an-android-device-900009df743f) 

# 参考

<https://medium.com/tinghaochen/how-to-install-pyspark-locally-94501eefe421>  <https://www.sicara.ai/blog/2017-05-02-get-started-pyspark-jupyter-notebook-3-minutes>  </pyspark-and-sparksql-basics-6cb4bf967e53> 