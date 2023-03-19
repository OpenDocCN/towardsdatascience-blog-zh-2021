# 熊猫时间序列数据的聚合

> 原文：<https://towardsdatascience.com/aggregations-on-time-series-data-with-pandas-5c79cc24a449?source=collection_archive---------9----------------------->

## Python 熊猫和 SQL 时间聚合和语法解释。

![](img/2aeeae92263577ec50f66fd8efa526f7.png)

等待一个好时机，波兰 2021。作者照片。

# 介绍

处理时间序列数据本身通常就是一项挑战。它是一种特殊的数据，数据点在时间上相互依赖。在分析它的时候，你获得洞察力的效率在很大程度上取决于你处理时间维度的能力。

通常，时间序列数据是在很长一段时间内收集的，特别是当它们来自硬件设备或代表金融交易等序列时。此外，即使数据集中没有字段为“空”，如果时间戳没有规则地间隔、移位、丢失或以任何方式不一致，数据仍然可能有问题。

有助于从依赖于时间的数据中获取有用信息的关键技能之一是高效地执行聚合。它不仅允许大大减少数据总量，而且有助于更快地发现有趣的事实。

在本文中，我将介绍几种方法来帮助您进行分析的最流行的 Python 库 [Pandas](https://pandas.pydata.org) 如何帮助您执行这些聚合，以及当您处理时间时有什么特别之处。除此之外，我还将在 SQL 中放一个等效的语法以供参考。

# 数据

为了演示，我使用来自 Kaggle 的[信用卡交易](https://www.kaggle.com/ealtman2019/credit-card-transactions?select=credit_card_transactions-ibm_v2.csv)数据集。然而，为了简单起见，我将重点放在`"Amount"`列上，并按单个用户对其进行过滤，尽管聚合总是可以扩展以包含更多的标准。关于时间的信息分布在`"Year", "Month", "Day"`和`"Time"`列中，所以用一个单独的列来表示它是有意义的。

因为整个数据集重约 2.35 GB，所以让我们使用较小的批次来动态转换数据。

```
import pandas as pd
import numpy as np
from tqdm import tqdm
from pathlib import Path

SRC = Path("data/credit_card_transactions-ibm_v2.csv")
DST = Path("data/transactions.csv")
USER = 0

def load(filepath=SRC):
    data = pd.read_csv(
        filepath,
        iterator=True,
        chunksize=10000,
        usecols=["Year", "Month", "Day", "Time", "Amount"],
    )

    for df in tqdm(data):
        yield df

def process(df):
    _df = df.query("User == @USER") 
    ts = _df.apply(
        lambda x: f"{x['Year']}{x['Month']:02d}{x['Day']:02d} {x['Time']}",
        axis=1,
    )

    _df["timestmap"] = pd.to_datetime(ts)
    _df["amount"] = df["Amount"].str.strip("$").astype(float)

    return _df.get(["timestamp", "amount"])

def main():
    for i, df in enumerate(load()):
        df = process(df)
        df.to_csv(
            DST,
            mode="a" if i else "w",
            header=not(bool(i)),
            index=False,
        )

if __name__ == "__main__":
    main()
```

…提供:

```
| timestamp           |   amount |
|:--------------------|---------:|
| 2002-09-01 06:21:00 |   134.09 |
| 2002-09-01 06:42:00 |    38.48 |
| 2002-09-02 06:22:00 |   120.34 |
| 2002-09-02 17:45:00 |   128.95 |
| 2002-09-03 06:23:00 |   104.71 |
```

这个框架的“头”给了我们上表。对于单个用户(这里是`USER = 0`)，我们有将近 20k 个时间戳，以一分钟的分辨率标记 2002 年到 2020 年之间的事务。

感谢第 31 行中的`pd.to_datetime`,我们转换了从四列串联的数据，并将其存储为以统一数据类型描述时间的`np.datetime64`变量。

## 什么是 np.datetime64？

`np.datetime64` ( [doc](https://numpy.org/doc/stable/reference/arrays.datetime.html) )类型是 pythonic】对象的数字版本。它是矢量化的，因此可以快速对整个数组执行操作。同时，该对象识别典型的`datetime`方法( [doc](https://docs.python.org/3/library/datetime.html) )，这些方法有助于自然地操作这些值。

在熊猫这边，相关的对象是`Timestamp`、`Timedelta`和`Period`(对应`DatetimeIndex`、`TimedeltaIndex`和`PeriodIndex`)，分别描述时间中的瞬间、时移和时间跨度。然而，在本质上，仍然有`np.datetime64`和类似的`np.timedelta64`拥有便利的属性。

将与时间相关的值转换成这些对象是任何时间序列分析的最佳起点。它既方便又快捷。

# 基本重采样

时序聚合的最简单形式是使用聚合函数将值输入到间隔均匀的容器中。它有助于调整分辨率和数据量。

以下代码片段显示了使用两个函数对天数进行重采样的示例:sum 和 count:

```
SELECT
    sum(amount),
    count(amount),
    DATE(timestamp) AS dt
FROM transactions
GROUP BY dt;
```

熊猫为我们提供了至少两种方法来达到同样的结果:

```
# option 1
df["amount"].resample("D").agg(["sum", "count"])# option 2
df["amount"].groupby(pd.Grouper(level=0, freq="D")) \
    .agg(["sum", "count"])
```

这两种选择是等效的。第一种更简单，它依赖于这样一个事实，即`timestamp`列已经被设置为 dataframe 的索引，尽管也可以使用可选参数`on`指向特定的列。第二种方法结合使用了更通用的聚合对象`pd.Grouper`和`.groupby`方法。它是高度可定制的，有许多可选参数。这里，我使用的是`level`而不是`key`，因为`timestamp`是一个索引。另外，`freq="D"`代表*天*。还有其他的[代码](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects)，尽管一个类似的 SQL 语句可能更复杂。

# 几个时间跨度的聚合

假设您想要聚合时间戳的多个部分的数据，比如`(year, week)`或`(month, day-of-week, hour)`。由于`timestamp`属于`np.datetime64`类型，所以可以使用所谓的`.dt`访问器来引用它的方法，并将它们用于聚合指令。

在 SQL 中，您应该:

```
SELECT
    AVG(amount),
    STRFTIME('%Y %W', timestamp) AS yearweek
FROM transactions
GROUP BY yearweek
```

在熊猫身上有两种方法:

```
df = df.reset_index()  # if we want `timestamp` to be a column
df["amount"].groupby(by=[
    df["timestamp"].dt.year, 
    df["timestamp"].dt.isocalendar().week
]).mean()

df = df.set_index("timestamp")  # if we want `timestamp` to be index
df["amount"].groupby(by=[
    df.index.year,
    df.index.isocalendar().week,
]).mean()
```

他们做同样的事情。

```
|            |   amount |
|:-----------|---------:|
| (2002, 1)  |  40.7375 |
| (2002, 35) |  86.285  |
| (2002, 36) |  82.3733 |
| (2002, 37) |  72.2048 |
| (2002, 38) |  91.8647 |
```

另外值得一提的是，`.groupby`方法并不强制使用聚合函数。它所做的只是将帧分割成一系列帧。您可能也想使用单独的“子帧”,并直接对它们执行一些变换。如果是这种情况，只需重复:

```
for key, group in df.groupby(by=[
        df.index.year,
        df.index.isocalendar().week
    ]):
    pass
```

这里的`key`将是一个`(year, week)`的元组，而`group`将是一个子帧。

## 注意

值得一提的是，在不同风格的 SQL 和 Pandas 中，时间窗口的边界可能会有不同的定义。当使用 SQLite 进行比较时，两者给出的结果略有不同。

SQL:

```
SELECT
    STRFTIME('%Y %W %w', timestamp),
    timestamp
FROM TRANSACTIONS
LIMIT 5;--gives:
| timestamp           | year | week | day |
|:--------------------|-----:|-----:|----:| 
| 2002-09-01 06:21:00 | 2002 | 34   |  0  |                    
| 2002-09-01 06:42:00 | 2002 | 34   |  0  |                    
| 2002-09-02 06:22:00 | 2002 | 35   |  1  |                    
| 2002-09-02 17:45:00 | 2002 | 35   |  1  |                    
| 2002-09-03 06:23:00 | 2002 | 35   |  2  |
```

熊猫:

```
df.index.isocalendar().head()# gives:
| timestamp           |   year |   week |   day |
|:--------------------|-------:|-------:|------:|
| 2002-09-01 06:21:00 |   2002 |     35 |     7 |
| 2002-09-01 06:42:00 |   2002 |     35 |     7 |
| 2002-09-02 06:22:00 |   2002 |     36 |     1 |
| 2002-09-02 17:45:00 |   2002 |     36 |     1 |
| 2002-09-03 06:23:00 |   2002 |     36 |     2 |
```

概念是一样的，只是参照物不一样。

# 窗口功能

最后一种通常用于时间数据的聚合类型是使用滚动窗口。与按某些特定列的值按行分组相反，这种方法定义了一个行的间隔来选择一个子表，移动窗口，并再次这样做。

让我们来看一个计算连续五行(当前加上过去的四行)的移动平均值的例子。在 SQL 中，语法如下:

```
SELECT
    timestamp,
    AVG(amount) OVER (
        ORDER BY timestamp
        ROWS BETWEEN 4 PRECEDING AND CURRENT ROW
    ) rolling_avg
FROM transactions;
```

Pandas 声明了一个简单得多的语法:

```
# applying mean immediatiely
df["amount"].rolling(5).mean()

# accessing the chunks directly
for chunk in df["amount"].rolling(5):
    pass
```

同样，在 Pandas 中，可以使用可选参数进行不同的调整。窗口的大小由`window`属性决定，这在 SQL 中是通过一系列语句实现的(第 5 行)。此外，我们可能希望将窗口居中，使用不同的窗口，例如加权平均，或者执行可选的数据清理。然而，`.rolling`方法返回的`pd.Rolling`对象的用法在某种意义上类似于`pd.DataFrameGroupBy`对象。

# 结论

在这里，我展示了我在处理时序数据时经常使用的三种类型的聚合。虽然并不是所有包含时间信息的数据都是时间序列，但是对于时间序列来说，将时间信息转换成`pd.Timestamp`或其他实现 numpy 的`np.datetime64`对象的类似对象几乎总是有益的。如图所示，它使得跨不同时间属性的聚合变得非常方便、直观和有趣。

*最初发表于*[*【https://zerowithdot.com】*](https://zerowithdot.com/time-series-aggregations-pandas/)*。*