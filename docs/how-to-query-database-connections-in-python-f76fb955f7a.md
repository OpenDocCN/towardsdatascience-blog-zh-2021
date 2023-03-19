# 如何在 Python 中查询数据库连接

> 原文：<https://towardsdatascience.com/how-to-query-database-connections-in-python-f76fb955f7a?source=collection_archive---------20----------------------->

## 使用 pandas.real_sql

为了便于浏览，下面是用于从雪花中查询数据的最终代码。在下面的文章中，我将分析每个步骤的原因，以及如何以同样的方式查询 MySQL 和 PostgreSQL 数据库。

# 导入详细信息

我选择从一个单独的文件中导入连接详细信息的原因是，我可以将该文件添加到`.gitignore`中，以确保没有信息被提交给在线回购。这样，即使回购变得公开，私有连接细节也保持隐藏。

请注意，这些键的命名与`snowflake.connector.connect`函数所需的参数完全相同。

这从例如 yaml 文件创建了一个 python 字典。

```
user: anthony
account: account.region
authenticator: externalbrowser
warehouse: DEV
database: DEV
```

对于 json 文件，上面的要点将改为使用:

```
import jsonwith open("snowflake_details.json", 'r') as stream:
     snowflake_details = json.load(stream)
```

# 创建连接器

![](img/02721cbffd88fd1859078f92c4f6cb78.png)

照片由[以色列总统府](https://unsplash.com/@othentikisra?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在创建`snowflake_connection`连接器对象时，通过使用`**`字典解包操作符，细节被用作函数参数。卸载的细节的键值必须与函数的参数相匹配(例如，对于 MySQL 连接，将需要`host`参数)。

对于 MySQL 连接，上面的要点将改为使用:

```
import pymysqlmysql_connection = pymysql.connect(host='', user='', password='',
                                   db='' port='')
```

或者对于 PostgreSQL 连接:

```
import psycopg2postgresql_connection = psycopg2.connect(host='', user='',
                                        password='', db='', port='')
```

# 查询数据库

为了便于阅读，为我们想要运行的查询创建一个字符串变量通常更容易。对于更大的查询，使用三个双引号`"""query"""`而不仅仅是双引号`"query”`，使得查询能够像上面的要点一样整齐地跨越多行。

现在我们可以使用`[pd.read_sql](https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html)`从一个查询和一个连接中立即创建一个熊猫数据帧。

```
df = pd.read_sql(query, connection)
```

这比首先使用游标获取数据，然后使用该信息构建 pandas 数据框架的方法要干净得多，后者只需要更多的步骤。

```
with connection.cursor() as cursor:
	sql = """
              select * 
              from table 1 
              limit 10
              """
	cursor.execute(sql)
	result = cursor.fetchall()
	fieldnames = [i[0] for i in cursor.description]
connection.close()df = pd.DataFrame(list(result), columns=field_names)
```

# 最后的想法

我写这篇文章的主要原因是，了解了`pd.read_sql`之后，用 Python 查询数据库连接对我来说变得容易多了。我希望除了像三个双引号和`**`操作符这样的技巧之外，这篇文章也能帮助其他人。