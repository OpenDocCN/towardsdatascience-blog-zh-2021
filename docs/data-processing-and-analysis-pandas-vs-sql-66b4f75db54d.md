# 数据处理和分析:熊猫 vs SQL

> 原文：<https://towardsdatascience.com/data-processing-and-analysis-pandas-vs-sql-66b4f75db54d?source=collection_archive---------45----------------------->

![](img/9d3c521ad7330ae1fe6064b16fa60ab7.png)

照片由伊戈尔·沙巴林拍摄

SQL 是许多数据库中访问和操作数据的主要工具。它设计用于处理存储在数据库容器(称为表)中的表格数据，允许您有效地访问、分组、合并和分析不同表中的数据。反过来，Pandas 库被设计为高效地对加载到 Python 脚本中的数据集执行所有这些操作。所以，pandas DataFrame 是一个类似于 SQL 表的二维数据结构。

本文提供了 pandas 和 MySQL 示例，说明了如何使用这些工具来组合数据集，以及如何对其中的数据进行分组、聚合和分析。

# 准备您的工作环境

为了遵循本文提供的示例，请确保在 Python 环境中安装了 pandas 库。如果您还没有它，最简单的安装方法是使用 pip 命令:

```
pip install pandas
```

要使用 SQL 示例，您需要访问 MySQL 数据库。有关如何在您的系统中安装它的详细信息，请参考《MySQL 8.0 参考手册》中位于[https://dev.mysql.com/doc/refman/8.0/en/installing.html](https://dev.mysql.com/doc/refman/8.0/en/installing.html)的“安装和升级 MySQL”一章，或 MySQL 未来版本的相应章节。

一旦有了 MySQL 数据库，就需要在其中创建几个示例表。为此，您可以在 mysql >提示符下执行以下 SQL 语句:

```
CREATE DATABASE mydb;
USE mydb;CREATE TABLE stocks(
 dt DATE,
 symbol CHAR(10),
 price DECIMAL(10, 4)
);
```

此外，您需要安装一个 Python 驱动程序，以便从 Python 内部与 MySQL 服务器进行通信。这可以使用 pip 命令来完成，如下所示:

```
pip install mysql-connector-python
```

# 获取数据

要继续下去，您需要获得一些数据来处理。许多流行 API 的 Python 包装器将数据作为 pandas 对象返回。在下面的代码片段中，您通过 yfinance Python 包装器调用 Yahoo Finance API。特别是，您获得了几个报价机的股票价格数据。数据以熊猫数据帧的形式返回，将在文章示例中使用:

```
import pandas as pd
import yfinance as yf
stocks = pd.DataFrame()
symbols = ['MSFT','AAPL','TSLA','ORCL','AMZN']
for symbol in symbols:
 tkr = yf.Ticker(symbol)
 hist = tkr.history(period='5d')
 hist[‘Symbol’]=symbol
 stocks = stocks.append(hist[['Symbol', 'Close']].rename(columns={'Close: 'Price'}))
```

然后，您可能希望将相同的数据插入到 MySQL 数据库表中，以便可以对相同的数据集执行 SQL 示例。但是，在这样做之前，您需要执行一些转换:

```
#Preparing the data 
stocks = stocks.reset_index().rename(columns={'Date': 'Dt'})
stocks[‘Dt’] = stocks[‘Dt’].astype(str)
s = stocks.values.tolist()
```

在下面的代码片段中，您将数据插入数据库:

```
#Inserting the data into the database
import mysql.connector
from mysql.connector import errorcode
try:
 cnx = mysql.connector.connect(user='root', password='pswd',
 host='127.0.0.1',
 database=’mydb’)
 cursor = cnx.cursor()
 query_add_stocks = """INSERT INTO stocks (dt, symbol, price) 
 VALUES (STR_TO_DATE(REPLACE( %s, '-', '/' ), '%Y/%m/%d'), %s, %s)"""
 #inserting the stock rows
 cursor.executemany(query_add_stocks, s)
 cnx.commit() 
except mysql.connector.Error as err:
 print("Error-Code:", err.errno)
 print("Error-Message: {}".format(err.msg))
finally:
 cursor.close()
 cnx.close()
```

# 对数据集执行分组

使用 DataFrame.groupby()方法，您可以将 DataFrame 的数据拆分为具有一个或多个列的匹配值的组，从而允许您对每个组应用聚合函数。在下面的示例中，您按股票数据框架中的符号列进行分组，然后对形成的组中的符号列应用 sum()聚合函数:

```
print(stocks.groupby('Symbol').mean().round(2))
```

结果，您应该会看到类似这样的内容(当然，您会得到不同的数字):

```
Price
     Symbol 
AAPL 125.88
AMZN 3231.97
MSFT 245.61
ORCL 78.91
TSLA 583.09
```

如果使用以下 SQL 查询请求数据库中的股票表，也可以得到相同的结果:

```
SELECT
 symbol,
 ROUND(AVG(price),2) TOTAL_mean
FROM
 stocks
GROUP BY
 symbol
ORDER BY 
 symbol;
```

以下是输出:

```
+ — — — — + — — — — — — +
| symbol | TOTAL_mean |
+ — — — — + — — — — — — +
| AAPL | 125.88 |
| AMZN | 3231.97 |
| MSFT | 245.61 |
| ORCL | 78.91 |
| TSLA | 583.09 |
+ — — — — + — — — — — — +
5 rows in set (0.00 sec)
```

# 分析数据处理

要执行您的分析，您可能需要执行比调用单个聚合函数更复杂的数据聚合操作，如前面的示例所示。继续我们的股票价格数据集，假设您想要确定波动性最高和最低的股票。首先，您需要通过 ticker 对数据集中的数据进行分组。然后，您可以使用 lambda 机制将几个聚合函数应用于 groupby 对象:

```
print(stocks.groupby("Symbol").agg({(‘vol’, lambda x: 100*(x.max() — x.min())/x.mean())}).round(2))
```

所以你得到了每个股票的波动性:

```
Price
     vol
Symbol 
AAPL 2.08
AMZN 1.38
MSFT 3.36
ORCL 0.87
TSLA 7.37
```

对于 SQL，您可以使用以下 SELECT 语句获得相同的结果:

```
SELECT
 symbol,
 ROUND(100*((MAX(price) — MIN(price))/AVG(price)),2) volatility
FROM
 stocks
GROUP BY
 symbol
ORDER BY
 symbol;
```

以下是输出:

```
+ — — — — + — — — — — — +
| symbol | volatility |
+ — — — — + — — — — — — +
| AAPL | 2.08 |
| AMZN | 1.38 |
| MSFT | 3.36 |
| ORCL | 0.87 |
| TSLA | 7.37 |
+ — — — — + — — — — — — +
5 rows in set (0.00 sec)
```

有关更多示例，请查看我最近在 Oracle Connect 上发表的文章[如何使用 pandas 运行 SQL 数据查询](https://www.oracle.com/news/connect/run-sql-data-queries-with-pandas.html)和[使用 Python 和 pandas 编程滚动窗口数据分析](https://www.oracle.com/news/connect/using-python-pandas-time-series-data-analysis.html)。