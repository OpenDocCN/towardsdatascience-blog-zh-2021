# 了解熊猫时写 PySpark 的语法陷阱

> 原文：<https://towardsdatascience.com/syntax-gotchas-writing-pyspark-when-knowing-pandas-57ace4ae769b?source=collection_archive---------31----------------------->

![](img/241e5bbab5fe09671b89657be10622c8.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 如果您对 Python Pandas 的数据分析有一些基础知识，并且对 PySpark 很好奇，不知道从哪里开始，请跟随我。

[Python 熊猫](https://pandas.pydata.org/)鼓励我们抛弃 excel 表格，转而从编码者的角度来看待数据。数据集变得越来越大，从数据库变成了数据文件，变成了数据湖。Apache 的一些聪明人用基于 Scala 的框架 Spark 祝福我们在合理的时间内处理更大的数量。由于 Python 是当今数据科学的主流语言，很快就有了一个 Python API，叫做 [PySpark](https://spark.apache.org/docs/latest/api/python/index.html) 。

有一段时间，我试图用大数据世界中每个人都称赞的非 pythonic 语法征服这个 Spark 接口。我尝试了几次，现在仍在进行中。然而，在这篇文章中，我想告诉你，谁也开始学习 PySpark，如何复制相同的分析，否则你会做熊猫。

我们要看的数据分析示例可以在 Wes McKinney 的书[“Python for Data Analysis”](https://wesmckinney.com/pages/book.html)中找到。在该分析中，目标是从 MovieLens 1M 数据集中找出排名最高的电影，该数据由明尼苏达大学的 [GroupLens](https://grouplens.org/) 研究项目获取和维护。

作为一个编码框架，我使用了 [Kaggle](https://www.kaggle.com/) ，因为它带来了笔记本电脑的便利，安装了基本的数据科学模块，只需点击两下就可以使用。

完整的分析和 Pyspark 代码你也可以在 [**这本 Kaggle 笔记本**](https://www.kaggle.com/christine12/movielens-1m-dataset-pyspark) 和[**这本**](https://www.kaggle.com/christine12/movielens-1m-dataset-python-pandas) 中找到熊猫代码。这里我们不会重复相同的分析，而是集中在处理 Pandas 和 Pyspark 数据帧时的语法差异上。我将始终首先显示熊猫代码，然后显示 PySpark 等价物。

此分析所需的基本功能是:

*   从 csv 格式加载数据
*   组合不同表中的数据集
*   提取信息

# **加载数据**

Pandas 有不同的[读取功能](https://pandas.pydata.org/docs/reference/io.html)，这使得根据存储数据的文件类型导入数据变得容易。

```
# Pandas
pd.read_table(path_to_file, sep='::', header=None, names=column_names, engine='python')
```

使用 PySpark，我们首先需要创建一个 [Spark 上下文](https://spark.apache.org/docs/latest/sql-getting-started.html)作为 Sparks 功能的入口点。这并不是说当您将它复制粘贴到代码中时，缩进可能不匹配。

```
# PySpark
from pyspark.sql import SparkSessionspark = SparkSession.Builder().getOrCreate() *#--> Spark Context**spark.read                        #--> what do you want to do*
 *.format("csv")                #--> which format is the file*
 *.option("delimiter", "::")    #--> specify delimiter that's used* 
 *.option("inferSchema", "true")#--> default value type is string*
 *.load(path_to_file)            #--> file path*
```

# 组合数据集

Pandas 中合并数据集的默认函数是[](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html)****()，**，它合并一列或多列上的数据集。就连 Pandas **join()** 函数也使用 merge 与一个索引列相结合。评级、用户和电影都在这个代码片段中，它们共享各自的列，所以我们可以调用合并函数。在名称不匹配的情况下，我们需要使用 join()来代替。**

```
# Pandas
data = pd.merge(pd.merge(ratings, users), movies)
```

**在 PySpark 中没有 merge 函数，这里默认的是对选择的列使用 **join()** 。否则看起来相当相似。**

```
# PySpark
data = ratings.join(users, ["user_id"]).join(movies, ["movie_id"])
```

# ****提取信息****

**现在我们已经有了可以处理的数据形式，我们可以看看如何真正提取内容。**

## **显示行条目**

**Pandas 数据帧源自 numpy 数组，因此可以使用方形括号来访问元素。如果我们想查看表格的前 5 个元素，我们可以这样做:**

```
# Pandas
users[:5]
```

**在 Spark 中，数据帧是对象，因此我们需要使用函数。在显示前 5 行的情况下，我们可以使用 **show()** ，它来自 Spark dataframes 使用的底层 SQL 语法。函数 show 只是表示数据集的一种方式，如果你想从选择的条目中创建一个新的 dataframe，你需要使用 **take()** 。**

```
# PySpark
users.show(5) # --> print result users.take(5) # --> list of Row() objects
```

## ****过滤****

**在熊猫身上，我们可以用不同的方式过滤。我们可以使用的一个函数是 **loc** ，这是一个基于标签的过滤器，用于访问行或列。在数据分析示例中，我们希望过滤数据帧中出现在标题列表中的所有行。**

```
# Pandas
mean_ratings = mean_ratings.loc[active_titles]
```

**PySpark 数据帧不支持 **loc，**因此我们需要使用过滤函数。处理 PySpark 数据帧中的列的一种简单方法是使用 **col()** 函数。用列名调用这个函数，将从 dataframe 返回相应的列。**

```
# PySpark
from pyspark.sql.functions import col
mean_ratings = mean_ratings.filter(col('title').isin(active_titles))
```

## **分组**

**Pandas 和 PySpark 中的分组看起来非常类似于 **groupby()** 。Pandas 数据帧有许多为分组对象实现的聚合函数[和](https://pandas.pydata.org/docs/reference/groupby.html)，如中位数、平均值、总和、方差等。对于 Pyspark 数据帧，有必要将其导入，例如从 pyspark.sql.functions 导入，如 **mean()** 和 standard dev **stddev()** 。**

```
# Pandas
ratings_by_title = data.groupby('title')['rating'].std()# PySpark
from pyspark.sql.functions import mean, stddev
coldata_sdf.groupBy('title').agg(stddev(col('rating')).alias('std'))
```

## **数据透视表**

**数据透视表常用于数据分析。熊猫数据框的排列和聚集都在一个函数[**pivot _ table**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.pivot_table.html)**中。****

```
# Pandas
mean_ratings = data.pivot_table('rating',
                                index='title',
                                columns='gender',
                                aggfunc='mean')
```

**为了用 Pyspark dataframes 复制相同的结果，我们需要连接分组、透视和聚合函数。**

```
# PySpark
mean_ratings_pivot = data
                       .groupBy('title')
                       .pivot('gender')
                       .agg(mean('rating'))
```

## **整理**

**要对一列中的值进行升序或降序排序，我们可以为 Pandas 数据帧调用 **sort_values()** 函数。请记住，默认的排序顺序是升序。**

```
# Pandas
top_female_ratings = mean_ratings.sort_values(by='F',
                                              ascending=False)
```

**在 PySpark 中有一个类似的函数叫做 **sort()** ，但是我们想要排序的列，我们必须作为输入给出。类似地，我们可以使用函数 **orderBy()** 并得到相同的结果。**

```
# PySpark
top_female_ratings = mean_ratings.sort(mean_ratings.F.desc())
```

**随着时间的推移，Pandas 和 PySpark 的语法将会改变。也许我们运气好，它们会变得更蟒蛇。还有一些模块结合了 provide 和 PySpark 的一个熊猫 API，叫做[考拉](https://koalas.readthedocs.io/en/latest/)。如果它的性能可以与 PySpark 竞争，我们可能会在未来看到更多这种情况。**

**我希望你找到了一些有用的提示，并请让我知道你从学习 Spark，语法和功能的见解！**