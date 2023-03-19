# Spark 3.0 中确定的数组排序方式

> 原文：<https://towardsdatascience.com/the-definitive-way-to-sort-arrays-in-spark-1224f5529961?source=collection_archive---------11----------------------->

## Spark 3.0 中数组排序技术的差异

去年早些时候(2020 年)我有对一个数组排序的需求，我发现有两个函数，名字非常相似，但是功能不同。

分别是 ***array_sort*** 和 ***sort_array。***

用哪个？起初，我感到困惑，为什么会有两个功能做同样的事情？

嗯，不同的是`array_sort`:

```
**def array_sort(e: Column):***Sorts the input array in* ***ascending*** *order and* ***null*** *elements will be placed at the* ***end*** *of the returned array.*
```

而`sort_array`:

```
**def sort_array(e: Column, asc: Boolean)***Sorts the input array for the given column in* ***ascending*** *or* ***descending*** *order elements.* ***Null*** *elements will be placed at the* ***beginning*** *of the returned array in* ***ascending*** *order or at the* ***end*** *of the returned array in* ***descending*** *order.*
```

看到这一点后，我决定打开一个[拉请求](https://github.com/apache/spark/pull/25728)来统一仅在`array_sort`中的这种行为，但是在与提交者讨论后，他们决定添加一种给定**比较器**函数的数组排序方法，以匹配 [Presto](https://prestodb.io/docs/current/functions/array.html) 数组函数，这将是一个好主意。

显然，这比最初统一两个功能行为的想法要困难得多，但是在提交者/评审者的帮助下，两者合并了。

历史讲够了，让我们看看新的`array_sort`在 Spark 3.0 中是如何工作的

它接收一个**比较器**函数，在这个函数中你将定义用于比较数组元素的逻辑。

> 那又怎样？

当您想要使用自定义逻辑对数组进行排序，或者比较选择排序中要使用的字段的结构数组时，比较器非常强大。

> 好吧，但是怎么做？

让我们用一组结构来定义我们的数据帧

```
case class Person(name: String, age: Int)val df = Seq(Array(
Person(“andrew”, 23), Person(“juan”,28), Person(“peter”, 22))
).toDF
```

如果我们打印模式并显示内容:

```
> df.printSchema
root
 |-- value: array (nullable = true)
 |    |-- element: struct (containsNull = true)
 |    |    |-- name: string (nullable = true)
 |    |    |-- age: integer (nullable = false)> df.show(false)
+---------------------------------------+
|value                                  |
+---------------------------------------+
|[[andrew, 23], [juan, 28], [peter, 22]]|
+---------------------------------------+
```

现在，让我们使用名称列对数据帧进行排序:

```
df.withColumn("arr_sorted", 
expr("array_sort(value,(l, r) -> case when l.name > r.name then -1 when l.name < r.name then 1 else 0 end)"))
```

现在让我们看看内容

```
+---------------------------------------+
|arr_sorted                             |
+---------------------------------------+
|[[peter, 22], [juan, 28], [andrew, 23]]|
+---------------------------------------+
```

不错！我们得到了按名称排序的数组！

> 还有别的方法吗？

是啊！您可以通过在比较器逻辑中注册一个 UDF 来更好地编程。

> 什么？

好的，假设现在你想按名称长度对数组进行排序，那么你应该这样做:

```
spark.udf.register("fStringLength", (x: Person, y: Person) => {
  if (x.name.length < y.name.length) -1
  else if (x.name.length == y.name.length) 0
  else 1
})
```

然后，给 UDF 打电话

```
df.selectExpr(“array_sort(value, (x, y) -> fStringLength(x, y)) as arr_sorted”).show(false)
```

这就对了。按名称长度排序的数组

```
+---------------------------------------+
|arr_sorted                             |
+---------------------------------------+
|[[juan, 28], [peter, 22], [andrew, 23]]|
+---------------------------------------+
```

希望新的`array_sort`看完帖子更清楚，绝对是 Spark 3.0 的强大功能

## 结论

Spark 中有多种方法对数组进行排序，新函数为复杂数组的排序带来了新的可能性。