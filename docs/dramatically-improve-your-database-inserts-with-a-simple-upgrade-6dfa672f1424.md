# 通过简单的升级，显著提高数据库插入速度

> 原文：<https://towardsdatascience.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424?source=collection_archive---------3----------------------->

## 创建速度惊人的 Python 数据库连接的 4 个级别

![](img/8dc8af6a263f20c918740c6283a471e5.png)

你用 fast_executemany 升级后的 python 脚本(图片由 [NASA](https://unsplash.com/@nasa) 在 [Unsplash](https://unsplash.com/photos/dCgbRAQmTQA) 上提供)

使用 Python 将数据上传到数据库很容易。将您的数据加载到 Pandas 数据框架中，并使用 dataframe.to_sql()方法。但是，您有没有注意到，在处理大型表时，插入会花费很多时间？我们没有时间无所事事，等待我们的查询完成！

![](img/d1d0a0a838d5bc912d83c4f54f59c9f0.png)

图片基于 leblancfg.com[上的](https://leblancfg.com/benchmarks_writing_pandas_dataframe_SQL_Server.html)[弗朗索瓦·勒布朗](https://leblancfg.com/pages/about.html#about)

只要稍加调整，你就可以更快地制作插页。比较棕色条(默认为 _sql()方法)和绿色条(我们的目标)的写时间。还要注意垂直轴在*对数*刻度*上！在本文结束时，您将能够执行闪电般的数据库操作。准备好了吗？我们走吧！*

# 目标和步骤

我们将分两步研究超快速插入方法。**第一部分**集中在如何正确连接到我们的数据库，第二部分**将探索 4 种以升序快速插入数据的方法。**

# 1.连接到我们的数据库

为了与任何数据库进行通信，您首先需要创建一个数据库引擎。这个引擎把你的 python 对象(比如熊猫数据帧)翻译成可以插入数据库的东西。为此，它需要知道如何访问您的数据库。这就是连接字符串的用途。我们将首先创建一个连接字符串，然后用它来创建我们的数据库引擎。

![](img/bdd839aa91ffc705b332931c9b6c5653.png)

连接字符串可以看作是使用我们数据库的关键(图片由[pix abay](https://www.pexels.com/@pixabay)on[Pexels](https://www.pexels.com/photo/door-handle-key-keyhole-279810/)提供)

## 1.1 创建连接字符串

连接字符串包含有关数据库类型、odbc 驱动程序和我们访问数据库所需的数据库凭证的信息。

```
constring = "mssql+pyodbc://USERNAME:PASSWORD@DATABASESERVER_IP/DATABASENAME?driver=SQL+Server+Native+Client+11.0"
```

在上面的例子中，我们创建了一个连接到 Microsoft SQL Server 数据库的字符串。如您所见，我们指定了数据库凭证(用户名、密码、数据库服务器的 IP 和数据库服务器的名称)，以及我们使用的驱动程序。连接字符串的格式因数据库而异，请查看[connectionstrings.com](https://www.connectionstrings.com/)以了解您的连接字符串应该是什么样子。

**关于驱动程序:**驱动程序需要与你正在使用的数据库版本相匹配。在上面的示例中，我们使用 MS SQL Server 2011，因此我们需要一个 SQL Server Native Client 11 驱动程序。这个驱动程序需要安装在运行 Python 脚本的机器上。通过搜索“ODBC 数据源”(在“驱动程序”选项卡下)，检查安装了哪些驱动程序。如果您需要的驱动程序没有安装，您可以轻松下载(例如 https://www.microsoft.com/en-us/download/details.aspx?[id=36434](https://www.microsoft.com/en-us/download/details.aspx?id=36434) 。

## 1.2 使用我们的连接字符串创建数据库引擎

一旦连接字符串有效，就很容易创建数据库引擎。我通常创建我的如下。

```
import sqlalchemy
dbEngine = sqlalchemy.create_engine(constring, connect_args={'connect_timeout': 10}, echo=False)
```

将 echo 设置为 True 允许您查看执行的所有查询。如果您将 echo 设置为字符串“debug ”,结果行也会被打印出来。

## 1.3 测试我们的数据库引擎

使用下面的小脚本来测试您的连接。如果一切顺利，它应该打印引擎是有效的。如果出了问题，它会打印错误。

```
try:
    with dbEngine.connect() as con:
        con.execute("SELECT 1")
    print('engine is valid')
except Exception as e:
    print(f'Engine invalid: {str(e)}')
```

# 2.快速插入方法的四个级别

既然我们能够连接到我们的数据库，我们可以开始插入数据到数据库中。我们将探索 4 种插入数据的方式，以最快的方式结束。在本文的最后，您会发现 pandas 的 to_sql 方法的详细总结。

![](img/8938698c721ee2854b27abfc226fced1.png)

让我们比赛这些方法，看看哪一个赢了(图片由[绝对魅力](https://www.pexels.com/@absolute-charm-1561240)在[像素](https://www.pexels.com/photo/men-on-horses-3015224/)上)

## 2.1 普通的 to_sql 方法

您可以在数据帧上调用此方法，并将其传递给数据库引擎。这是一个相当简单的方法，我们可以调整，以获得速度的每一滴。在下面的例子中，我们创建了一个数据帧并上传它。

```
import pandas as pd# 1\. Create a dataframe
df = pd.DataFrame({'numbers': [1, 2, 3], 'colors': ['red', 'white', 'blue']})
print(df.head())# dataframe looks like:
   numbers colors
0        1    red
1        2  white
2        3   blue# 2\. Upload this dataframe
df.to_sql(con=dbEngine, schema="dbo", name="colortable", if_exists="replace", index=False)
```

如您所见，我们只需指定我们的连接(我们之前创建的数据库引擎)，我们希望将新表和新表名放在哪个模式中。此外，如果指定的 schema.tablename 已经存在(在我们的例子中是 replace ),我们可以决定做什么，以及我们是否要在表上放一个索引(进一步查看完整的参数列表)。

上面的例子是插入数据最简单的方法，但也是最慢的。问题是我们一次写入整个数据帧，为每个记录创建一个 insert 语句。在小桌子上，比如我们的彩色桌子，这不是一个大问题，但在大桌子上，这肯定是一个大问题。

## 2.2 分块

如果我们增加一个 chunksize，我们的代码会运行得更好。这将按指定的块大小批量写入数据，从而节省大量内存。

```
df_large.to_sql(con=dbEngine, schema="dbo", name="largetable", if_exists="replace", index=False, chunksize=1000)
```

这个方法仍然为我们的表中的每个记录创建一个 insert 语句，这仍然非常慢。

## 2.3 多重插入

添加“multi”方法将大大提高插入速度。我们现在可以在一条语句中发送多行，而不是为每条记录编写一条 insert 语句。databast 可以在一个操作中处理多个记录，而不是每个记录一个操作。

重要说明:对于 Microsoft SQL Server 数据库，此方法不起作用，也不是必需的。请参见方法 4。

```
df_target.to_sql(con=dbEngine, schema="dbo", name="targettable", if_exists="replace", index=False, chunksize=1000, method='multi')
```

理想的块大小取决于表的尺寸。有很多列的表比只有 3 列的表需要更小的块大小。

对于许多数据库来说，这是写入数据库的最快方式。然而，对于微软服务器，仍然有一个更快的选择。

## 2.4 SQL Server 快速执行许多

SQLAlchemy 1.3 在为 SQL server 创建 dbEngine 时为我们提供了 fast_executemany 选项。此方法是将数据帧写入 SQL Server 数据库的最快方法。

```
dbEngine = sqlalchemy.create_engine(constring, fast_executemany=True, connect_args={'connect_timeout': 10}, echo=False) df_target.to_sql(con=dbEngine, schema="dbo", name="targettable", if_exists="replace", index=False, chunksize=1000)
```

在上面的代码中，你可以看到我们必须对数据库引擎进行一些调整；我们必须添加 fast_executemany 选项。就是这样。然后，当我们将数据帧写入数据库时，我们必须记住的唯一一件事就是我们不要指定我们的方法(或者设置方法=None)。这是因为默认情况下 fast_executemany 是多插入的。

![](img/49095e054a08a11e34285c21fd6c3df6.png)

fast_executemany 就像赛马中的 F1 赛车！(图片由[上的](https://unsplash.com/@chuttersnap)和[下的](https://unsplash.com/photos/5Yo1P9ErikM)拍摄)

# 结论

通过这些简单的升级，您现在拥有了改进 python 到数据库连接的工具。我在下面包含了一个带有示例代码和步骤的总结。编码快乐！

迈克

页（page 的缩写）学生:比如我正在做的事情？[跟我来](https://mikehuls.medium.com/membership)！

快速插入一百万行到我们的数据库中

## 概述:to_sql 参数

下面是对 to_sql 函数所有设置的概述和解释。

*   **con (sqlalchemy 引擎)**
    数据库连接(sqlalchemy 引擎)
*   **name (str)** :必需的
    要写入的表格的名称
*   ***schema(str)****:默认值:默认数据库模式(dbo)*
    要写入的模式的名称。
*   ***if _ exists(str):****默认:' fail'*
    如果指定的表已经存在怎么办？
    - *'fail'* :抛出错误
    -*-【append’*:将数据追加到指定的表格
    -*' replace’*:将表格全部替换(警告:这很危险)
*   ***index(bool):****默认值:True*
    如果 Index 设置为 True，它会创建一个名为“id”的额外列，该列被编入索引
*   ***index_label (str 或 sequence):*** *默认:无*
    列标签为索引列。默认无。
*   ***【chunksize(int)****:默认:无*
    批量写入大小为【chunk size】的行。如果您有大量记录需要上传，这是一个好主意。这样可以节省内存。如果没有:一次写入所有行
*   ***dtype (dict 或 scalar)*** *:默认无*
    指定数据类型
    如果指定了 scalar:在写入数据库之前，将此数据类型应用于 dataframe 中的所有列。要指定每列的数据类型，请提供一个字典，其中数据帧列名是键。这些值是 sqlalchemy 类型(例如，sqlalchemy。浮动等)
*   ***method(str)****:默认 None*
    控制 SQL 插入子句
    - None:使用标准的 SQL 插入子句(每行一个)
    --“multi”:将多个值传递给单个插入子句。