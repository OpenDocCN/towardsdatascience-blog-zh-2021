# 从 SQLite 到 Pandas——你需要知道的 7 个基本操作

> 原文：<https://towardsdatascience.com/from-sqlite-to-pandas-7-essential-operations-you-need-to-know-c7a5dd71f232?source=collection_archive---------11----------------------->

## 相信我——这很简单。

![](img/61397ce5cf1663b816da70374a49484f.png)

兰迪·塔兰皮在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

大约十年前，当我在做 iOS 开发时，还没有成熟的移动应用程序数据库解决方案，因此，我必须在应用程序中实现自己的数据库。我选择使用 SQLite，这是一种成熟的轻量级关系数据库解决方案。

当我将部分职业重心转向数据科学时，我很高兴地得知 Python 也有管理 SQLite 的 API。重要的是，pandas 是数据处理的首选库，它提供了与各种数据库(包括 SQLite)通信的相关功能。将它们结合使用，我们可以对本地存储和数据操作做很多事情。在本文中，让我们探索一些基本的操作。

## 1.连接到数据库

为了使用 SQLite 数据库，我们利用了内置的`sqlite3`模块，它提供了完整的通用 SQLite 数据库操作。下面向您展示了我们如何连接到数据库。

```
import sqlite3

con = sqlite3.connect("test.db")
```

通过调用`connect`函数，将会发生两件事。

1.  该模块将连接到`test.db`数据库。如果它不存在，它将在当前目录中创建这样命名的数据库。
2.  这个函数调用将创建一个代表数据库的`[Connect](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection)`对象。从现在开始，我们用这个`Connection`对象执行任何与数据库相关的操作。

如果你不想处理一个物理测试数据库，模块可以通过运行下面的代码在内存中创建一个数据库。

```
con_memory = sqlite3.connect(":memory:")
```

为了当前的教程，我们将坚持使用链接到`test.db`的`con`对象。

## 2.创建新表并插入记录

首先，我们把一些数据放入数据库。为简单起见，假设我们想要两个数据字段(姓名和年级),将有四条记录，如下所示。

```
names = ['John', 'Mike', 'Jane', 'Bella']
grades = [90, 95, 92, 98]
```

下面的代码向您展示了我们如何创建一个表并相应地插入这些记录。

新表和记录

*   就像其他数据库一样，我们首先需要创建一个游标来执行 SQLite 语句。*有一点需要注意的是，你可以使用非标准的* `*execute*` *和* `*executemany*` *方法直接与* `*Connect*` *对象在一起，让你的代码看起来更整洁一点，尽管在这个遮光罩下，光标仍然是由 Python 为你创建的。关于是否显式创建光标，这是您的个人选择。*
*   为了在 SQLite 数据库中创建一个表，我们使用了`CREATE TABLE table_name (field0 field0_type, field1 field1_type, …)`。你可以在 SQLite 网站[这里](https://www.sqlite.org/datatype3.html)找到支持的数据类型。简而言之，它支持文本、整数、blob(可以用 blob 保存二进制数据)和实数。值得注意的是，SQLite 没有 boolean 类型，您可以考虑使用整数 0 和 1 来表示布尔值。
*   要插入一条记录，只需调用`cur.execute(“INSERT into transcript values (‘John’, 90)”)`。但是，要用一个函数调用插入多条记录，您应该使用`executemany`方法，在该方法中，您将 SQL 语句模板和一个`iterator`一起传递，后者的项将被顺序添加到模板中。
*   为了提交所有这些事务来更新数据库，我们在`con`对象上调用`commit`方法。

## 3.查询记录

要查询记录，您可以应用类似的方法——使用`execute`方法提交所需的 SQL 语句。下面的代码向您展示了我们如何查询按成绩排序的记录。

```
>>> cur.execute("select * from transcript order by grade desc")
<sqlite3.Cursor object at 0x1103b5ea0>
```

您可能会注意到，调用`execute`并没有直接返回我们预期的结果——所有记录。相反，这个调用返回同一个`cursor`对象。运行 SELECT 语句后，可以将游标视为迭代器。因此，我们可以将它包含在一个列表构造函数中，以显示所有记录。

```
>>> list(cur)
[('Bella', 98), ('Mike', 95), ('Jane', 92), ('John', 90)]
```

或者，cursor 对象有内置的方法`fetchall`来显示记录，或者`feathone`来检索一个匹配的记录。顺便提一下，为了避免返回值调用`execute`，我们使用了下划线。

```
>>> _ = cur.execute("select * from transcript order by grade desc")
>>> cur.fetchall()
[('Bella', 98), ('Mike', 95), ('Jane', 92), ('John', 90)]
>>> _ = cur.execute("select * from transcript order by grade desc")
>>> cur.fetchone()
('Bella', 98)
```

## 4.更新记录

为了更新记录，我们使用下面的 SQL 语句语法:`update table_name set field_name=new_value, another_field=new_value where condition`。应用这个语法，让我们考虑下面的更新。

```
>>> cur.execute("update transcript set grade = 100 where name = 'John'")
<sqlite3.Cursor object at 0x1103b5ea0>
>>> list(cur.execute("select * from transcript order by grade desc"))
[('John', 100), ('Bella', 98), ('Mike', 95), ('Jane', 92)]
```

如上所示，我们成功地更新了 John 的分数，这样我们就有了不同的分数顺序。

## 5.删除记录

要删除记录，我们使用下面的 SQL 语句语法:`delete from table_name where condition`。不要忽略条件，这一点非常重要，否则会删除表中的所有行，在大多数情况下，这不是我们想要的操作。下面是一个例子。

```
>>> _ = cur.execute("delete from transcript where name='John'")
>>> list(cur.execute("select * from transcript order by grade desc"))
[('Bella', 98), ('Mike', 95), ('Jane', 92)]
```

如上所示，我们已经删除了 John 的记录。

## 6.用熊猫读取 SQLite 数据

用熊猫操纵 SQLite 数据库很好玩。Pandas 提供了一个函数`read_sql`，它允许我们直接执行 SQL 语句，而不用担心底层的基础设施。

```
>>> import pandas as pd
>>> df = pd.read_sql("select * from transcript", con)
>>> df
    name  grade
0   Mike     95
1   Jane     92
2  Bella     98
```

本质上，这个函数调用创建了一个`DataFrame`，从这里您可以利用 pandas 库提供的所有通用方法。

有些人可能见过熊猫功能`read_sql_table`和`read_sql_query`。实际上，`read_sql`函数只是这两个函数的包装器。它将评估输入并调用适当的函数。因此，在我们的日常使用中，我们可以简单地使用`read_sql`功能，让熊猫为我们做繁重的工作。

## 7.将数据帧写入 SQLite

在使用 pandas 处理数据之后，是时候将 DataFrame 写回 SQLite 数据库进行长期存储了。Pandas 为此操作提供了`to_sql`方法。下面显示了一种可能的用法。

```
>>> df['gpa'] = [4.0, 3.8, 3.9]
>>> df.to_sql("transcript", con, if_exists="replace", index=False)
>>> list(cur.execute("select * from transcript order by grade desc"))
[('Bella', 98, 3.9), ('Mike', 95, 4.0), ('Jane', 92, 3.8)]
```

*   不像`read_sql`，它是熊猫库中的一个函数，`to_sql`是`DataFrame`类的一个方法，因此它将被`DataFrame`对象直接调用。
*   在`to_sql`方法中，您指定要保存`DataFrame`的表。
*   `if_exists`参数很重要，因为默认情况下，该参数被设置为`“fail”`，这意味着当表已经存在时，您不能将当前的`DataFrame`写入该表——将引发一个`ValueError`。因为在我们的例子中，由于更新的 GPA 信息，我们想要替换现有的表，我们将`if_exisits`参数指定为`“replace”`。
*   将`index`设置为`False`会在保存到表格时忽略`DataFrame`对象的索引。它与您可能更熟悉的`to_csv`方法具有相同的效果。

## 结论

在本文中，我们回顾了使用 Python 内置的 sqlite3 模块来使用 SQLite 数据库的基本操作。正如您所看到的，使用提供的方法，我们可以非常方便地操作常见的 SQL 操作，如插入、更新和删除。本质上，如果您已经了解 SQL，就没有太多的学习曲线。

我们还探索了 pandas 如何与 SQLite 数据库接口。记住接口的诀窍很简单——read _ SQL 是从 SQLite 数据库中提取任何内容，而 to_sql 是将数据转储回 SQLite 数据库。也就是说，假设您的主要数据处理工具是 pandas，相对于 SQLite 数据库，`read_sql`用于 out，`to_sql`用于 in。