# pandas-to-sql——将代码从 SQL 转移到 Python 和 Pandas

> 原文：<https://towardsdatascience.com/pandas-to-sql-moving-code-from-sql-to-python-and-pandas-afc4c62a644c?source=collection_archive---------14----------------------->

![](img/0f4b80234582fd41027572d6176bef6d.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[pandas-to-sql](https://github.com/AmirPupko/pandas-to-sql) 是一个 python 库，允许使用 python 的 [Pandas](https://pandas.pydata.org/) [DataFrames](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) 来创建 sql 字符串，这些字符串稍后可用于查询数据库。[这里有一些你可以运行](https://colab.research.google.com/github/AmirPupko/pandas-to-sql/blob/main/pandas_to_sql_colab_example.ipynb)的例子。

数据科学项目的常见结构将从数据获取和准备阶段开始。如果保存数据的数据库支持 SQL，数据提取甚至一些数据准备都可以使用 SQL 来执行(受 DB 支持，为什么不呢？)

这方面的问题:

*   用两种不同的语言(SQL 和 Python)维护代码
*   该项目成为一个两步项目(SQL 部分，然后是 Python 部分)。这可能是设计的选择，但也可能是项目中两种编码语言的副作用
*   在 SQL 代码上更难使用编码原则和最佳实践(例如 [SOLID](https://en.wikipedia.org/wiki/SOLID)
*   通常，SQL 代码不会被测试

pandas-to-sql 如何尝试解决这些问题？

[pandas-to-sql](https://github.com/AmirPupko/pandas-to-sql) 是一个 python 库，允许用户使用 pandas 数据帧，创建不同的操作，并最终使用 **get_sql_string()** 方法获得描述操作的 sql 字符串。此时，可以使用这个字符串来查询数据库。

如何安装:

```
pip install pandas-to-sql
```

**简单的例子:**

输出:

```
‘SELECT (sepal_length) AS sepal_length, (sepal_width) AS sepal_width, (petal_length) AS petal_length, (petal_width) AS petal_width, (species) AS species FROM iris’
```

在上面的例子中，我们将虹膜数据集加载到熊猫数据帧中。然后我们使用 **pandas_to_sql.wrap_df** 转换数据帧。从这一点开始，所有操作都将保存为 SQL 字符串。在任何时候，都可以使用 **df.get_sql_string()** 获得这个 SQL 字符串。

这里还有一些例子…

**过滤示例:**

输出:

```
‘SELECT (sepal_length) AS sepal_length, (sepal_width) AS sepal_width, (petal_length) AS petal_length, (petal_width) AS petal_width, (species) AS species FROM iris WHERE (((petal_length > 1.4) AND (petal_width < 0.2)))’
```

**分组示例:**

输出:

```
‘SELECT (avg(petal_length)) AS petal_length_mean, (sum(petal_length)) AS petal_length_sum, (count(petal_length)) AS petal_length_count, (species) AS species FROM iris GROUP BY species’
```

注意**约定的使用。由于 Pandas groupby 返回一个多索引数据帧，而该数据帧不适用于 SQL，因此该方法编辑 SQL 查询中的列名，以匹配 **reset_index()** 返回的默认值(并对 groupby 返回的数据帧执行 **reset_index()** )。**

**串联示例:**

输出:

```
‘SELECT (sepal_length) AS sepal_length, (sepal_width) AS sepal_width, (petal_length) AS petal_length, (petal_width) AS petal_width, (species) AS species, ((CASE WHEN ((ABS(sepal_width) — ROUND(ABS(sepal_width)-0.5))==.5) AND ((CAST(sepal_width AS INT))%2 == 0) THEN (CASE WHEN sepal_width>0 THEN ROUND(sepal_width-0.001) ELSE ROUND(sepal_width+0.001) END) ELSE (ROUND(sepal_width)) END)) AS sepal_width_rounded FROM iris WHERE ((species = ‘setosa’)) UNION ALL SELECT (sepal_length) AS sepal_length, (sepal_width) AS sepal_width, (petal_length) AS petal_length, (petal_width) AS petal_width, (species) AS species, ((CASE WHEN ((ABS(sepal_width) — ROUND(ABS(sepal_width)-0.5))==.5) AND ((CAST(sepal_width AS INT))%2 == 0) THEN (CASE WHEN sepal_width>0 THEN ROUND(sepal_width-0.001) ELSE ROUND(sepal_width+0.001) END) ELSE (ROUND(sepal_width)) END)) AS sepal_width_rounded FROM iris WHERE ((species = ‘versicolor’))‘
```

这里注意 PD**pandas _ to _ SQL . wrap _ PD(PD)**的包装，以便覆盖 **pd.concat()**

以下是一个要点中的所有示例:

该库尚未投入生产。我的主要目标是检查这个想法的吸引力，以及是否有更多的人觉得这个有用。如果是的话，会增加更多的支持。目前的支持只针对 [SQLite](https://www.sqlite.org/index.html) ，希望很快增加对[的支持。对于任何 bug 或新功能请求，请随时提出问题。](https://prestodb.io/)[问题](https://github.com/AmirPupko/pandas-to-sql/issues)

感谢阅读:-)