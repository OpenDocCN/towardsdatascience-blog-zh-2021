# Spark 3.1 的高阶函数

> 原文：<https://towardsdatascience.com/higher-order-functions-with-spark-3-1-7c6cf591beaa?source=collection_archive---------9----------------------->

## 在 Spark SQL 中处理数组。

![](img/e9064f9be3f3275b6500f6485ab13f05.png)

唐纳德·詹纳蒂在 [Unsplash](https://unsplash.com/s/photos/vla?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

复杂的数据结构，如数组、结构和映射在大数据处理中非常常见，尤其是在 Spark 中。每当我们希望在一列中表示每行上的多个值时，就会出现这种情况，在数组数据类型的情况下，这可以是一个值列表，在映射的情况下，这可以是一个键值对列表。

从 Spark 2.4 开始，通过发布高阶函数(Hof)，对处理这些复杂数据类型的支持增加了。在本文中，我们将了解什么是高阶函数，如何有效地使用它们，以及在最近几个 Spark 和 3.1.1 版本中发布了哪些相关功能。对于代码，我们将使用 Python API。

继我们在上一篇[文章](/spark-sql-102-aggregations-and-window-functions-9f829eaa7549)中提到的聚合和窗口函数之后，HOFs 是 Spark SQL 中另一组更高级的转换。

让我们首先看看 Spark 提供的三种复杂数据类型之间的区别。

## 数组类型

```
l = [(1, ['the', 'quick', 'braun', 'fox'])]df = spark.createDataFrame(l, schema=['id', 'words'])df.printSchema()root
 |-- id: long (nullable = true)
 |-- words: array (nullable = true)
 |    |-- element: string (containsNull = true)
```

在上面的示例中，我们有一个包含两列的数据帧，其中列 *words* 是数组类型，这意味着在数据帧的每一行上，我们可以表示一个值列表，并且该列表在每一行上可以有不同的大小。此外，数组的元素是有顺序的。重要的属性是数组在元素类型方面是同质的，这意味着所有元素必须具有相同的类型。要访问数组的元素，我们可以使用如下的索引:

```
df.withColumn('first_element', col('words')[0])
```

## 结构类型

*StructType* 用于将一些可能具有不同类型(不同于数组)的子字段组合在一起。每个子字段都有一个类型和一个名称，并且对于数据帧中的所有行都必须相同。可能出乎意料的是，一个结构中的子字段是有顺序的，所以比较两个具有相同字段但顺序不同的结构 *s1==s2* 会导致 *False* 。

请注意数组和结构之间的基本区别:

*   数组:类型相同，允许每行有不同的大小
*   结构:类型异构，每行都需要相同的模式

## 地图类型

您可以将 map 类型视为前面两种类型的混合:array 和 struct。设想这样一种情况，每一行的模式都没有给出，您需要在每一行上支持不同数量的子字段。在这种情况下，您不能使用 struct。但是使用数组对您来说并不是一个好的选择，因为每个元素都有一个名称和一个值(它实际上是一个键-值对),或者因为元素有不同的类型——这是 map 类型的一个很好的用例。使用 map 类型，您可以在每一行上存储不同数量的键-值对，但是每个键必须具有相同的类型，并且所有的值都必须是相同的类型(可以与键的类型不同)。配对的顺序很重要。

# 变换数组

在我们开始讨论转换数组之前，让我们先看看如何创建一个数组。第一种方法我们已经在上面看到了，我们从一个本地值列表中创建了数据帧。另一方面，如果我们已经有了一个数据帧，我们想将一些列组成一个数组，我们可以使用函数 [*array()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.array.html#pyspark.sql.functions.array) 来实现这个目的。它允许您从其他现有的列中创建一个数组，因此如果您有列 *a* 、 *b* 、 *c* ，并且您希望将值放在一个数组中，而不是放在单独的列中，您可以这样做:

```
df.withColumn('my_arr', array('a', 'b', 'c'))
```

除此之外，还有一些函数产生一个数组作为转换的结果。例如，函数 [*split(* )](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.split.html#pyspark.sql.functions.split) 会将一个字符串拆分成一个单词数组。另一个例子是[*collect _ list()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.collect_list.html#pyspark.sql.functions.collect_list)或[*collect _ set()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.collect_set.html#pyspark.sql.functions.collect_set)，它们都是聚合函数，也会产生一个数组。

实际上，将数组放入数据帧的最常见方式是从支持复杂数据结构的数据源(如 Parquet)读取数据。在这种文件格式中，一些列可以存储为数组，因此 Spark 自然也会将它们读取为数组。

现在，当我们知道如何创建一个数组时，让我们看看数组是如何转换的。

从 Spark 2.4 开始，有大量的函数用于数组转换。有关它们的完整列表，请查看 PySpark [文档](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#functions)。例如，所有以 *array_* 开头的函数都可以用于数组处理，您可以找到最小-最大值、对数组进行重复数据删除、排序、连接等等。接下来，还有[*concat()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.concat.html#pyspark.sql.functions.concat)[*flatten()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.flatten.html#pyspark.sql.functions.flatten)[*shuffle()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.shuffle.html#pyspark.sql.functions.shuffle)[*size()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.size.html#pyspark.sql.functions.size)[*【slice()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.slice.html#pyspark.sql.functions.slice)[*sort _ array()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.sort_array.html#pyspark.sql.functions.sort_array)。正如你所看到的，API 在这方面已经相当成熟，你可以用 Spark 中的数组做很多操作。

除了上述这些函数，还有一组函数将另一个函数作为参数，然后应用于数组的每个元素，这些函数被称为高阶函数(Hof)。了解它们很重要的一点是，在 Python API 中，从 3.1.1 开始就支持它们，而在 Scala API 中，它们是从 3.0 开始发布的。另一方面，对于 SQL 表达式，从 2.4 开始就可以使用了。

要查看一些具体示例，请考虑以下简单的数据框架:

```
l = [(1, ['prague', 'london', 'tokyo', None, 'sydney'])]df = spark.createDataFrame(l, ['id', 'cities'])df.show(truncate=False)+---+-------------------------------------+
|id |cities                               |
+---+-------------------------------------+
|1  |[prague, london, tokyo, null, sydney]|
+---+-------------------------------------+
```

假设我们想要完成这五个独立的任务:

1.  将每个城市的首字母转换成大写。
2.  去掉数组中的空值。
3.  检查是否有以字母 t 开头的元素。
4.  检查数组中是否有空值。
5.  对数组中每个城市的字符数(长度)求和。

这些是可以用 HOFs 解决的问题的一些典型例子。所以让我们一个一个来看:

## 改变

对于第一个问题，我们可以使用 [*transform*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.transform.html#pyspark.sql.functions.transform) HOF，它只是采用一个匿名函数，将它应用于原始数组的每个元素，并返回另一个转换后的数组。语法如下:

```
df \
.withColumn('cities', transform('cities', lambda x: initcap(x))) \
.show(truncate=False)+---+-------------------------------------+
|id |cities                               |
+---+-------------------------------------+
|1  |[Prague, London, Tokyo, null, Sydney]|
+---+-------------------------------------+
```

如您所见， *transform()* 有两个参数，第一个是需要转换的数组，第二个是匿名函数。在这里，为了实现我们的转换，我们在匿名函数中使用了 [*initcap()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.initcap.html#pyspark.sql.functions.initcap) ，它被应用于数组的每个元素——这正是*转换* HOF 允许我们做的。对于 SQL 表达式，可以按如下方式使用:

```
df.selectExpr("id", "TRANSFORM(cities, x -> INITCAP(x)) AS cities")
```

注意，SQL 中的匿名函数是用箭头(->)符号表示的。

## 过滤器

在第二个问题中，我们希望从数组中过滤出空值。这一点(以及任何其他过滤)可以使用 [*过滤器*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.filter.html#pyspark.sql.functions.filter) HOF 来处理。它允许我们应用一个匿名函数，该函数对每个元素返回布尔值( *True* / *False* )，并且它将返回一个新数组，该数组只包含该函数返回 *True* 的元素:

```
df \
.withColumn('cities', filter('cities', lambda x: x.isNotNull())) \
.show(truncate=False)+---+-------------------------------+
|id |cities                         |
+---+-------------------------------+
|1  |[prague, london, tokyo, sydney]|
+---+-------------------------------+
```

这里，在匿名函数中我们调用 PySpark 函数 [*isNotNull()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.Column.isNotNull.html#pyspark.sql.Column.isNotNull) 。SQL 语法如下所示:

```
df.selectExpr("id", "FILTER(cities, x -> x IS NOT NULL) AS cities")
```

## 存在

在下一个问题中，我们希望检查数组是否包含满足某些特定条件的元素。请注意，这是一个更一般的例子，在这种情况下，我们希望检查某个特定元素的存在。例如，如果我们想检查数组是否包含城市*布拉格*，我们可以只调用[*array _ contains*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.array_contains.html#pyspark.sql.functions.array_contains)函数:

```
df.withColumn('has_prague', array_contains('cities', 'prague'))
```

另一方面， [*存在*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.exists.html#pyspark.sql.functions.exists) HOF 允许我们对每个元素应用更一般的条件。结果不再是像前两个 Hof 那样的数组，而只是*真* / *假*:

```
df \
.withColumn('has_t_city', 
  exists('cities', lambda x: x.startswith('t'))) \
.show(truncate=False)+---+-------------------------------------+----------+
|id |cities                               |has_t_city|
+---+-------------------------------------+----------+
|1  |[prague, london, tokyo, null, sydney]|true      |
+---+-------------------------------------+----------+
```

这里在匿名函数中，我们使用了 PySpark 函数[*【starts with()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.Column.startswith.html#pyspark.sql.Column.startswith)。

## FORALL

在第四个问题中，我们希望验证数组中的所有元素是否都满足某些条件，在我们的示例中，我们希望检查它们是否都不为空:

```
df \
.withColumn('nulls_free',forall('cities', lambda x: x.isNotNull()))\
.show(truncate=False)+---+-------------------------------------+----------+
|id |cities                               |nulls_free|
+---+-------------------------------------+----------+
|1  |[prague, london, tokyo, null, sydney]|false     |
+---+-------------------------------------+----------+
```

正如你所看到的，对于所有的 来说 [*与*存在*非常相似，但是现在我们正在检查条件是否对所有的元素都成立，之前我们至少要寻找一个。*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.forall.html#pyspark.sql.functions.forall)

## 总计

在最后一个问题中，我们希望对数组中每个单词的长度求和。这是一个例子，我们希望将整个数组简化为一个值，对于这种问题，我们可以使用 HOF [*聚合*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.aggregate.html#pyspark.sql.functions.aggregate) 。

```
 df \
.withColumn('cities', filter('cities', lambda x: x.isNotNull())) \
.withColumn('cities_len', 
  aggregate('cities', lit(0), lambda x, y: x + length(y))) \
.show(truncate=False)+---+-------------------------------+----------+
|id |cities                         |cities_len|
+---+-------------------------------+----------+
|1  |[prague, london, tokyo, sydney]|23        |
+---+-------------------------------+----------+
```

使用 SQL:

```
df \
.withColumn("cities", filter("cities", lambda x: x.isNotNull())) \
.selectExpr(
    "cities", 
    "AGGREGATE(cities, 0,(x, y) -> x + length(y)) AS cities_len"
)
```

如你所见，与之前的 HOFs 相比，语法稍微复杂了一些。*集合*接受更多的参数，第一个仍然是我们想要转换的数组，第二个参数是我们想要开始的初始值。在我们的例子中，初始值是零( *lit(0)* )，我们将把每个城市的长度加进去。第三个参数是匿名函数，现在这个函数本身有两个参数——第一个参数(在我们的例子中是 *x* )是运行缓冲区，我们将第二个参数表示的下一个元素的长度(在我们的例子中是 *y* )添加到这个缓冲区中。

可选地，可以提供第四个参数，这是另一个转换最终结果的匿名函数。如果我们想要进行更复杂的聚合，这是很有用的，例如，如果我们想要计算平均长度，我们需要保留大约两个值，即 *sum* 和 *count* ，我们将在最后的转换中对它们进行如下划分:

```
(
    df
    .withColumn('cities', filter('cities', lambda x: x.isNotNull()))
    .withColumn('cities_avg_len', 
        aggregate(
            'cities', 
            struct(lit(0).alias('sum'), lit(0).alias('count')), 
            lambda x, y: struct(
                (x.sum + length(y)).alias('sum'), 
                (x.count + 1).alias('count')
            ),
            lambda x: x.sum / x.count
        )
    )
).show(truncate=False)+---+-------------------------------+--------------+
|id |cities                         |cities_avg_len|
+---+-------------------------------+--------------+
|1  |[prague, london, tokyo, sydney]|5.75          |
+---+-------------------------------+--------------+
```

如您所见，这是一个更高级的示例，我们需要在聚合过程中保留两个值，我们使用具有两个子字段 *sum* 和 *count 的 [*struct()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.struct.html#pyspark.sql.functions.struct) 来表示它们。*使用第一个匿名函数，我们计算所有长度的最终总和，以及元素总数。在第二个匿名函数中，我们只是将这两个值相除，得到最终的平均值。还要注意，在使用 *aggregate* 之前，我们首先过滤掉空值，因为如果我们在数组中保留空值，总和(以及平均值)将变成空值。

要查看聚合 HOF 与 SQL 表达式一起使用的另一个示例，请检查 [this](https://stackoverflow.com/questions/59181802/how-to-count-the-trailing-zeroes-in-an-array-column-in-a-pyspark-dataframe-witho/59194663#59194663) Stack Overflow 问题。

除了前面提到的这五个 Hof，还有 [*zip_with*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.zip_with.html#pyspark.sql.functions.zip_with) 可以用来将两个数组合并成一个数组。除此之外，还有其他的 Hof，如 [*map_filter*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.map_filter.html#pyspark.sql.functions.map_filter) ， [*map_zip_with*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.map_zip_with.html#pyspark.sql.functions.map_zip_with) ，[*transform _ keys*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.transform_keys.html#pyspark.sql.functions.transform_keys)，以及[*transform _ values*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.transform_values.html#pyspark.sql.functions.transform_values)与地图一起使用，我们将在以后的文章中了解它们。

# 结论

在本文中，我们介绍了高阶函数(Hof ),这是 Spark 2.4 中发布的一个特性。首先，只有 SQL 表达式支持它，但是从 3.1.1 开始，Python API 也支持它。我们已经看到了五个 Hof 的例子，它们允许我们在 Spark 数组中转换、过滤、检查存在性和聚集元素。在 HOFs 发布之前，大多数问题都必须使用用户定义的函数来解决。然而，HOF 方法在性能方面更有效，要查看一些性能基准，请参见我最近的另一篇文章，其中显示了一些具体的数字。