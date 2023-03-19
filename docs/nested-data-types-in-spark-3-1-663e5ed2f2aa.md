# Spark 3.1 中的嵌套数据类型

> 原文：<https://towardsdatascience.com/nested-data-types-in-spark-3-1-663e5ed2f2aa?source=collection_archive---------3----------------------->

## 在 Spark SQL 中使用结构

![](img/3b996443da8a64fe077cc7f42dc4cc8f.png)

埃利斯·陈嘉炜在 [Unsplash](https://unsplash.com/s/photos/complex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在之前关于高阶函数的[文章](/higher-order-functions-with-spark-3-1-7c6cf591beaa)中，我们描述了三种复杂的数据类型:数组、映射和结构，并特别关注了数组。在这篇后续文章中，我们将研究 structs，并了解 Spark 3.1.1 版本中发布的用于转换嵌套数据的两个重要函数。对于代码，我们将使用 Python API。

# 结构体

*StructType* 是一种非常重要的数据类型，允许表示嵌套的层次数据。它可用于将一些字段组合在一起。 *StructType* 的每个元素被称为 *StructField* ，它有一个名称，也有一个类型。元素通常也称为字段或子字段，它们通过名称来访问。 *StructType* 也用于表示整个数据帧的模式。让我们看一个简单的例子

```
from pyspark.sql.types import *my_schema = StructType([
    StructField('id', LongType()),
    StructField('country', StructType([
        StructField('name', StringType()),
        StructField('capital', StringType())
    ])),
    StructField('currency', StringType())
])l = [
        (1, {'name': 'Italy', 'capital': 'Rome'}, 'euro'),
        (2, {'name': 'France', 'capital': 'Paris'}, 'euro'),
        (3, {'name': 'Japan', 'capital': 'Tokyo'}, 'yen')
    ]df = spark.createDataFrame(l, schema=my_schema)df.printSchema()root
 |-- id: long (nullable = true)
 |-- country: struct (nullable = true)
 |    |-- name: string (nullable = true)
 |    |-- capital: string (nullable = true)
 |-- currency: string (nullable = true)df.show()
+---+---------------+--------+
| id|        country|currency|
+---+---------------+--------+
|  1|  {Italy, Rome}|    euro|
|  2|{France, Paris}|    euro|
|  3| {Japan, Tokyo}|     yen|
+---+---------------+--------+
```

创建的 DataFrame 有一个结构 *country* ，它有两个子字段: *name* 和 *capital* 。

## 创建结构

至少有四种基本方法可以在数据框中创建一个*结构类型*。第一种方法我们已经在上面看到过——从本地集合创建 DataFrame。第二种也是最常见的方式是从支持复杂数据结构的数据源读取数据，比如 JSON 或 Parquet。接下来，有一些函数将创建一个结构作为结果。这种转换的一个特殊例子是通过 [*窗口*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.window.html#pyspark.sql.functions.window) 进行分组，这将产生一个具有两个子字段 *start* 和 *end* 的结构，正如您在这里看到的:

```
l = [(1, 10, '2021-01-01'), (2, 20, '2021-01-02')]dx = spark.createDataFrame(l, ['id', 'price', 'date'])(
    dx
    .groupBy(window('date', '1 day'))
    .agg(sum('price').alias('daily_price'))
).printSchema()root
 |-- window: struct (nullable = false)
 |    |-- start: timestamp (nullable = true)
 |    |-- end: timestamp (nullable = true)
 |-- daily_price: long (nullable = true)
```

第四种创建 struct 的方法是使用函数[*【struct()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.struct.html#pyspark.sql.functions.struct)。该函数将从作为参数传递的其他列创建一个 *StructType* ，并且 *StructFields* 将与原始列具有相同的名称，除非我们使用 *alias()* 重命名它们:

```
df.withColumn('my_struct', struct('id', 'currency')).printSchema()root
 |-- id: long (nullable = true)
 |-- country: struct (nullable = true)
 |    |-- name: string (nullable = true)
 |    |-- capital: string (nullable = true)
 |-- currency: string (nullable = true)
 |-- my_struct: struct (nullable = false)
 |    |-- id: long (nullable = true)
 |    |-- currency: string (nullable = true)
```

这里，我们创建了一个列 *my_struct* ，它有两个子字段，这两个子字段是从数据帧中的两个列派生出来的。

## 访问元素

正如我们上面提到的，结构的子字段是通过名称来访问的，这是通过点符号来完成的:

```
df.select('country.capital').show()+-------+
|capital|
+-------+
|   Rome|
|  Paris|
|  Tokyo|
+-------+
```

可能不明显的是，这也适用于结构数组。假设我们有一个数组*国家*，数组的每个元素都是一个结构体。如果我们只想访问每个结构的*大写*子字段，我们可以用完全相同的方法，得到的列将是一个包含所有大写的数组:

```
my_new_schema = StructType([
    StructField('id', LongType()),
    StructField('countries', ArrayType(StructType([
        StructField('name', StringType()),
        StructField('capital', StringType())
    ])))
])l = [(1, [
        {'name': 'Italy', 'capital': 'Rome'},
        {'name': 'Spain', 'capital': 'Madrid'}
    ])
]

dz = spark.createDataFrame(l, schema=my_new_schema)# we have array of structs:
dz.show(truncate=False)+---+--------------------------------+
|id |countries                       |
+---+--------------------------------+
|1  |[{Italy, Rome}, {Spain, Madrid}]|
+---+--------------------------------+# access all capitals:
dz.select('countries.capital').show(truncate=False)+--------------+
|capital       |
+--------------+
|[Rome, Madrid]|
+--------------+
```

关于访问数组内部嵌套结构中元素的另一个具体例子，参见[这个](https://stackoverflow.com/questions/57810876/how-to-check-if-a-spark-data-frame-struct-array-contains-a-specific-value/57812763#57812763)堆栈溢出问题。

## 添加新元素

从 Spark 3.1 开始，支持使用函数 [*withField()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.Column.withField.html#pyspark.sql.Column.withField) 向现有结构添加新的子字段。让我们看看我们的例子，其中我们将列*货币*添加到结构*国家:*

```
(
  df
  .withColumn(
    'country', 
    col('country').withField('currency', col('currency'))
  )
).show(truncate=False)+---+---------------------+--------+
|id |country              |currency|
+---+---------------------+--------+
|1  |{Italy, Rome, euro}  |euro    |
|2  |{France, Paris, euro}|euro    |
|3  |{Japan, Tokyo, yen}  |yen     |
+---+---------------------+--------+
```

在 Spark 3.1 之前，情况更复杂，通过重新定义整个结构，可以向结构中添加新字段:

```
new_df = (
  df.withColumn('country', struct(
    col('country.name'),
    col('country.capital'),
    col('currency')
  ))
)
```

如您所见，我们必须列出所有的结构子字段，然后添加新的字段——这可能非常麻烦，尤其是对于具有许多子字段的大型结构。在这种情况下，有一个很好的技巧可以让你一次处理所有的子字段——使用星形符号:

```
new_df = (
  df.withColumn('country', struct(
    col('country.*'),
    col('currency')
  ))
)
```

*国家中的星号。** 将获取原始结构的所有子字段。然而，在下一个我们想要删除字段的例子中，情况会变得更加复杂。

## 移除元素

从 Spark 3.1 开始，从结构中删除子字段又是一项简单的任务，因为函数 [*dropFields()*](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.Column.dropFields.html#pyspark.sql.Column.dropFields) 已经发布。现在让我们使用修改后的数据框架 *new_df* ，其中该结构包含三个子字段 *name* 、 *capital、*和 *currency* 。例如，删除一个子字段*大写*可以按如下方式完成:

```
new_df.withColumn('country',col('country').dropFields('capital')) \
.show(truncate=False)+---+--------------+--------+
|id |country       |currency|
+---+--------------+--------+
|1  |{Italy, euro} |euro    |
|2  |{France, euro}|euro    |
|3  |{Japan, yen}  |yen     |
+---+--------------+--------+
```

正如我们所见，子字段*大写*被删除。在 Spark 3.1 之前的版本中，情况又变得复杂了，我们必须重新定义整个结构，并删除我们想要删除的子字段:

```
(
    new_df
    .withColumn('country', struct(
        col('country.name'),
        col('country.currency')
    ))
)
```

对于大型结构来说，这又是一个繁琐的过程，所以我们可以通过如下列出所有子字段来使其更加可行:

```
# list all fields in the struct:
subfields = new_df.schema['country'].dataType.fieldNames()# remove the subfield from the list:
subfields.remove('capital')# use the new list to recreate the struct:
(
    new_df.withColumn(
        'country',
        struct(
            ['country.{}'.format(x) for x in subfields]
        )
    )
).show()+---+--------------+--------+
| id|       country|currency|
+---+--------------+--------+
|  1| {Italy, euro}|    euro|
|  2|{France, euro}|    euro|
|  3|  {Japan, yen}|     yen|
+---+--------------+--------+
```

注意，两个函数 *withField* 和 *dropFields* 都是 [*列*](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#column-apis) 类的成员，因此它们被称为列对象上的方法(要了解更多如何使用列类中的方法，请查看我最近的[文章](/a-decent-guide-to-dataframes-in-spark-3-0-for-beginners-dcc2903345a5)，在那里我会更详细地讨论它)。

# SQL 表达式中的结构

当您查看 [SQL](https://spark.apache.org/docs/latest/api/sql/index.html) 文档时，您会发现有两个函数可用于创建结构，即 [*struct()*](https://spark.apache.org/docs/latest/api/sql/index.html#struct) 和[*named _ struct()*](https://spark.apache.org/docs/latest/api/sql/index.html#named_struct)，它们在语法上有所不同，因为 *named_struct* 也要求为每个子字段传递一个名称:

```
(
  df
  .selectExpr("struct(id, currency) as my_struct")
).show(truncate=False)+---------+
|my_struct|
+---------+
|{1, euro}|
|{2, euro}|
|{3, yen} |
+---------+(
  df.selectExpr(
    "named_struct('id', id, 'currency', currency) as my_struct")
).show()+---------+
|my_struct|
+---------+
|{1, euro}|
|{2, euro}|
| {3, yen}|
+---------+
```

# 结论

在本文中，我们继续描述 Spark SQL 中的复杂数据类型。在之前的[文章](/higher-order-functions-with-spark-3-1-7c6cf591beaa)中，我们讨论了数组，这里我们关注结构，在未来的文章中，我们将讨论映射。我们已经看到了最近版本 3.1.1 中发布的两个重要函数 *withField()* 和 *dropFields()* ，它们可以在操作现有结构的子字段时大大简化代码。