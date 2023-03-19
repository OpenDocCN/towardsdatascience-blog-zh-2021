# Python 熊猫 vs. R Dplyr

> 原文：<https://towardsdatascience.com/python-pandas-vs-r-dplyr-5b5081945ccb?source=collection_archive---------4----------------------->

## 完整的备忘单

![](img/ab96d1190ee3b7d5607d7f976a80f77e.png)

图片由作者根据[https://allisonhorst.github.io/](https://allisonhorst.github.io/)的 Dplyr 标志和[https://pandas.pydata.org/about/citing.html](https://pandas.pydata.org/about/citing.html)的熊猫标志制作

# 期待什么

对于许多数据科学家来说， **Pandas** for Python 和 **Dplyr** for R 是两个最流行的处理表格/结构化数据的库。关于哪个框架更好，总会有一些激烈的讨论。老实说，这真的重要吗？最后，这是关于完成工作，熊猫和 dplyr 都提供了很好的数据争论工具。**不用担心，这篇文章并不是另一个试图证明任何一个库的观点的比较！**因此，本条的目的是:

*   帮助其他人从一种语言/框架过渡到另一种语言/框架
*   探索可以作为数据科学家增加技能的新工具
*   创建一个参考备忘单，以防您需要查找两种语言中最常用的数据争论函数

# **数据**

在本教程中，我们将使用 **iris** 数据集，它是 Pythons sklearn 和 base R 的一部分。

```
Sepal_length  Sepal_width  Petal_length     Petal_width  Species
          5.1         3.5           1.4             0.2   setosa
          4.9         3.0           1.4             0.2   setosa
          4.7         3.2           1.3             0.2   setosa
          4.6         3.1           1.5             0.2   setosa
          5.0         3.6           1.4             0.2   setosa
```

# 小抄

免责声明:长阅读

## 选择列

这里已经变得令人困惑了。首先，在每个框架中，如何从数据框架中选择列有多种方法。在 Pandas 中，您可以简单地传递带有列名的列表，或者使用 **filter()** 方法。这令人困惑，因为 dplyr 中的 **filter()** 函数用于根据条件而不是根据列对行进行子集化！在 dplyr 中，我们使用 **select()** 函数来代替:

熊猫

```
#Pass columns as list
dataframe[[“Sepal_width”, “Petal_width”]] #Use Filter Function
dataframe.filter(items=['Sepal_width', 'Petal_width'])
```

**Dplyr**

```
dataframe %>% select(Sepal_width, Petal_width)
```

## 基于条件的过滤

同样，有多种方法可以根据一列或多列的条件过滤数据帧中的记录。

熊猫

在 Pandas 中，你可以使用索引方法或者尝试方便的查询 API，这是我个人更喜欢的。

```
#indexing
dataframe[(dataframe["Sepal_width"] > 3.5) & (dataframe["Petal_width"] < 0.3)] #query API
dataframe.query("Sepal_width > 3.5 & Petal_width < 0.3")
```

**Dplyr**

在 **dplyr** 中过滤记录的标准方式是通过 filter **函数()**。

```
dataframe %>% filter(Sepal_width > 3.5 & Petal_width < 0.3)
```

## 重命名单列

重命名听起来像是一个简单的任务，但是要小心，注意这里的细微差别。如果我们想在熊猫中将我们的列**从物种重命名为类**，我们提供一个字典，上面写着**{‘物种’:‘类’}**，而在 Dplyr 中正好相反**类=物种**:

**熊猫**

```
dataframe.rename(columns = {'Species': 'Class'}, inplace = True)
```

**Dplyr**

```
dataframe <- dataframe %>% rename(Class=Species)
```

## 根据条件重命名多个列

假设我们想根据一个条件一次重命名多个列。例如，将我们所有的特征列(萼片长度、萼片宽度、花瓣长度、花瓣宽度)转换为大写。在 Python 中，这实际上相当复杂，您需要首先导入另一个库，并手动迭代每一列。在 Dplyr 中，如果您想根据条件访问/更改多个列，有一个更简洁的界面。

**熊猫**

```
import re#prepare pattern that columns have to match to be converted to upper case
pattern = re.compile(r".*(length|width)")#iterate over columns and covert to upper case if pattern matches.
for col in dataframe.columns:
  if bool((pattern.match(col))):
    dataframe.rename(columns = {col: col.upper()}, inplace = True)
```

**Dplyr**

```
dataframe <- dataframe %>% rename_with(toupper, matches("length|width"))
```

**结果**

请注意大写特征列名:

```
SEPAL_LENGTH  SEPAL_WIDTH  PETAL_LENGTH     PETAL_WIDTH  Species
          5.1         3.5           1.4             0.2   setosa
          4.9         3.0           1.4             0.2   setosa
```

## 根据条件改变单元格值

假设我们想要基于条件重新编码/改变单元格值:在我们的示例中，我们将尝试将物种字符串“setosa，versicolor 和 virginica”重新编码为从 0 到 2 的整数:

**熊猫**

```
dataframe.loc[dataframe['Species'] == 'setosa', "Species"] = 0
dataframe.loc[dataframe['Species'] == 'versicolor', "Species"] = 1
dataframe.loc[dataframe['Species'] == 'virginica', "Species"] = 2
```

**Dplyr**

```
dataframe <- dataframe %>%
  mutate(Species = case_when(Species == 'setosa' ~ 0,
                             Species == 'versicolor' ~ 1,
                             Species == 'virginica' ~ 2))
```

## 每列的不同值

有时我们想看看一列中有哪些不同的/唯一的值。请注意这两个框架中的函数调用有多么不同:Pandas 使用 **unique()** 方法，dplyr()使用 **distinct()** 函数来获得相同的结果:

熊猫

```
dataframe.Species.unique()#array(['setosa', 'versicolor', 'virginica'], dtype=object)
```

**Dplyr**

```
dataframe %>% select(Species) %>% distinct()# Species   
# setosa    
# versicolor
# virginica
```

## 记录计数(每组)

如果您想计算数据帧中总共有多少个条目，或者获取某个组的计数，您可以执行以下操作:

熊猫

```
# Total number of records in dataframe
len(dataframe)#150 # Number of records per Group
dataframe.value_counts('Species')#Species
#virginica     50
#versicolor    50
#setosa        50# Note that you can also use the .groupby() method followed by size()
dataframe.groupby(['Species']).size() 
```

**Dplyr**

```
# Total number of records in dataframe
dataframe %>% nrow()#[1] 150 # Number of records per Group (count and tally are interchangeable)
dataframe %>% group_by(Species) %>% count()
dataframe %>% group_by(Species) %>% tally()#  Species        n
#  <fct>      <int>
#1 setosa        50
#2 versicolor    50
#3 virginica     50
```

## 对整个列进行汇总/聚合

如果要为数据框中的一列或多列创建描述性统计数据，可执行以下操作:

**熊猫**

```
#get mean and min for each column
dataframe.agg(['mean', 'min'])#      Sepal_length  Sepal_width  Petal_length  Petal_width Species
#mean      5.843333     3.057333         3.758     1.199333     NaN
#min       4.300000     2.000000         1.000     0.100000  setosa
```

**Dplyr**

不幸的是，我没有找到如何一次在多个列上使用多个聚合函数的方法。这就是为什么您需要多次调用 summarise 函数来获得相同的结果:

```
#first aggregation over all columns using mean
dataframe %>% summarise(across(everything(), mean))#  Sepal_length Sepal_width Petal_length Petal_width Species
#         5.84        3.06         3.76        1.20      NA#second aggregation over all columns using min
dataframe %>% summarise(across(everything(), min))#Sepal_length Sepal_width Petal_length Petal_width Species
#        5.84        3.06         3.76        1.20      NA
```

## 按组汇总/汇总

如果想要在数据集中按组聚集统计数据，必须使用 Pandas 中的 **groupby()** 方法和 Dplyr 中的 **group_by()** 函数。您可以对所有列或特定列执行此操作:

**熊猫**

请注意 Pandas 如何使用多级索引来清晰地显示结果:

```
# aggregation by group for all columns
dataframe.groupby(['Species']).agg(['mean', 'min'])#                 Sepal_length      Sepal_width       ...
#                   mean  min        mean min         ...
#Species                                                         
#setosa            5.01  4.3       3.43             ...
#versicolor        5.94  4.9       2.77             ...
#virginica         6.59  4.9       2.97             ... # aggregation by group for a specific column
dataframe.groupby(['Species']).agg({'Sepal_length':['mean']})#   Sepal_length
#                   mean
#Species                
#setosa            5.01
#versicolor        5.94
#virginica         6.59
```

**Dplyr**

由于 Dplyr 不支持多级索引，所以第一次调用的输出与 Pandas 相比看起来有点乱。在此输出中，显示了第一个函数的统计数据(mean-fn1)，然后是第二个函数的统计数据(min-fn2)。

```
# aggregation by group for all columns
dataframe %>% group_by(Species) %>% summarise_all(list(mean,min))Species    Sepal_length_fn1  Sepal_width_fn1         ...
setosa                 5.01            3.43          ...
versicolor             5.94            2.77          ...         
virginica              6.59            2.97          ... # aggregation by group for a specific column
dataframe %>% group_by(Species) %>% summarise(mean=mean(Sepal_length))#Species     mean
# setosa      5.01
# versicolor  5.94
# virginica   6.59
```

## 列数学/添加新列

有时，您希望创建一个新列，并通过某种数学运算将两个或多个现有列的值组合起来。以下是如何在熊猫和 Dplyr 中做到这一点:

熊猫

```
dataframe["New_feature"] = dataframe["Petal_width"]* dataframe["Petal_length"] / 2
```

**Dplyr**

```
dataframe <- dataframe %>% mutate(New_feature= Petal_width*Petal_length/2)
```

## 删除列

为了清理数据帧，删除列有时会非常方便:

**熊猫**

在 Pandas 中，你可以用 **drop()** 删除一列。您也可以使用 **inplace=True** 来覆盖当前数据帧。

```
dataframe.drop("New_feature", axis=1, inplace=True)
```

**Dplyr**

在 Dplyr 中，您可以在 select()函数中使用前导的**减号来指定要删除的列名。**

```
dataframe <- dataframe %>% select(-New_feature)
```

## 按值对记录排序

要对值进行排序，您可以在 Pandas 中使用 **sort_values()** ，在 Dplyr 中使用 **arrange()** 。两者的默认排序都是升序。请注意每个函数调用在降序排序方面的差异:

**熊猫**

```
dataframe.sort_values('Petal_width', ascending=0)
```

**Dplyr**

```
dataframe %>% arrange(desc(Petal_width))
```

## 重命名单列

重命名听起来像是一个简单的任务，但是要小心，注意这里的细微差别。如果我们想在 Pandas 中将我们的列**从 Species 重命名为 Class** ，我们提供一个字典，上面写着 **{'Species': 'Class'}** ，而在 Dplyr 中，情况正好相反 **Class=Species** :

熊猫

```
dataframe.rename(columns = {'Species': 'Class'}, inplace = True)
```

**Dplyr**

```
dataframe %>% relocate(Species)
dataframe %>% relocate(Species, .before=Sepal_width)
```

## 更改列的顺序

我不经常使用这个功能，但是如果我想为一个演示创建一个表格，并且列的排序没有逻辑意义，这个功能有时会很方便。以下是如何移动列:

**熊猫**

在 Python Pandas 中，您需要通过使用列表来重新索引您的列。假设我们想将列物种移到前面。

```
#change order of columns
dataframe.reindex(['Species','Petal_length','Sepal_length','Sepal_width','Petal_Width'], axis=1)
```

**Dplyr**

在 Dplyr 中，您可以使用方便的 **relocate()** 函数。同样，假设我们想将列物种移到前面。

```
dataframe %>% relocate(Species)#Note that you can use .before or .after to place a columne before or after another specified column - very handy!
dataframe %>% relocate(Species, .before=SEPAL_WIDTH)
```

# 限幅

切片本身就是一个完整的主题，有很多方法可以实现。下面让我们来看一下最常用的切片操作:

## 按行切片

有时您知道想要提取的确切行号。虽然 Dplyr 和 Pandas 中的过程非常相似**，但是请注意 Python 中的索引从 0 开始，而 R 中的索引从 1 开始。**

**熊猫**

```
dataframe.iloc[[49,50]]#  Sepal_length  Sepal_width  Petal_length  Petal_width    Species
#          5.0          3.3           1.4          0.2      setosa
#          7.0          3.2           4.7          1.4  versicolor
```

**Dplyr**

```
dataframe %>% slice(50,51)#  Sepal_length Sepal_width Petal_length Petal_width Species     
#1            5         3.3          1.4         0.2 setosa    
#2            7         3.2          4.7         1.4 versicolor
```

## 分割第一个和最后一个记录(头/尾)

有时我们希望看到数据帧中的第一条或最后一条记录。这可以通过提供一个**固定数量 n** 或一个**比例 prop** 值来实现。

**熊猫**

在 Pandas 中，您可以使用 **head()** 或 **tail()** 方法来获取固定数量的记录。如果你想提取一个比例，你必须自己计算一下:

```
#returns the first 5 records
dataframe.head(n=5)#returns the last 10% of total records
dataframe.tail(n=len(dataframe)*0.1)
```

**Dplyr**

在 Dplyr 中，有两个为这个用例指定的函数: **slice_head()** 和 **slice_tail()** 。请注意如何指定固定数量或比例:

```
#returns the first 5 records
dataframe %>% slice_head(n=5)#returns the last 10% of total records
dataframe %>% slice_tail(prop=0.1)
```

## 按值对第一条和最后一条记录进行切片

有时，选择每列具有最高或最低值的记录很有用。同样，这可以通过提供固定的数量或比例来实现。

**熊猫**

对于熊猫来说，这比 Dplyr 更棘手。例如，假设您想要 20 个具有最长“花瓣长度”的记录，或者 10%的总记录具有最短的“花瓣长度”。要在 Python 中进行第二个操作，我们必须做一些数学计算，首先对我们的值进行排序:

```
#returns 20 records with the longest Petal_length (for returning the shortest you can use the function nsmallest)
dataframe.nlargest(20, 'Petal_length')#returns 10% of total records with the shortest Petal_length
prop = 0.1 
dataframe.sort_values('Petal_length', ascending=1).head(int(len(dataframe)*prop))
```

**Dplyr**

在 Dplyr 中，这要简单得多，因为此用例有指定的函数:

```
#returns 20 records with the longest Petal_length
dataframe %>% slice_max(Petal_length, n = 20)#returns 10% of total records with the shortest Petal_lengthdataframe %>% slice_min(Petal_length, prop = 0.1)
```

## 按值和组对第一条和最后一条记录进行切片

有时，选择每列具有最高或最低值的记录**，但由组**分隔，这很有用。同样，这可以通过提供固定的数量或比例来实现。想象一下，例如我们想要 3 个每个物种花瓣长度最短的记录。

**熊猫**

对熊猫来说，这也比 Dplyr 更棘手。我们首先按照物种对数据帧进行分组，然后应用一个 **lambda** 函数，该函数利用了上述的 **nsmallest()** 或 **nlargest()** 函数:

```
#returns 3 records with the shortest Petal_length per Species
(dataframe.groupby('Species',group_keys=False)
        .apply(lambda x: x.nsmallest(3, 'Petal_length')))#Sepal_length  Sepal_width  Petal_length  Petal_width     Species
#        4.6          3.6           1.0          0.2      setosa
#        4.3          3.0           1.1          0.1      setosa
#        5.8          4.0           1.2          0.2      setosa
#        5.1          2.5           3.0          1.1  versicolor
#        4.9          2.4           3.3          1.0  versicolor
#        5.0          2.3           3.3          1.0  versicolor
#        4.9          2.5           4.5          1.7   virginica
#        6.2          2.8           4.8          1.8   virginica
#        6.0          3.0           4.8          1.8   virginica#returns 5% of total records with the longest Petal_length per Species
prop = 0.05
(dataframe.groupby('Species',group_keys=False)
        .apply(lambda x: x.nlargest(int(len(x) * prop), 'Petal_length')))#Sepal_length  Sepal_width  Petal_length  Petal_width     Species
#         4.8          3.4           1.9          0.2      setosa
#         5.1          3.8           1.9          0.4      setosa
#         6.0          2.7           5.1          1.6  versicolor
#         6.7          3.0           5.0          1.7  versicolor
#         7.7          2.6           6.9          2.3   virginica
#         7.7          3.8           6.7          2.2   virginica
```

**Dplyr**

在 Dplyr 中，这要简单得多，因为这个用例有指定的函数。注意如何提供 **with_ties=FALSE** 来指定是否应该返回 ties(具有相等值的记录)。

```
#returns 3 records with the shortest Petal_length per Species
dataframe %>% group_by(Species) %>% slice_min(Petal_length, n = 3, with_ties = FALSE)#returns 5% of total records with the longest Petal_length per Species
dataframe %>% group_by(Species) %>% slice_max(Petal_length, prop = 0.05, with_ties=FALSE)
```

## 切片随机记录(每组)—抽样

对随机记录进行切片也可以称为抽样。这也可以通过证明一个固定的数字或比例来实现。此外，这可以在整个数据集上进行，也可以基于组平均分布。因为这是一个相当频繁的用例，所以在两个框架中都有这样的函数:

**熊猫**

在 Pandas 中，您可以使用 **sample()** 函数，或者指定 **n** 为固定数量的记录，或者指定 **frac** 为一定比例的记录。此外，您可以指定**替换**来允许或不允许对同一行进行多次采样。

```
#returns 20 random samples
dataframe.sample(n=20)#return 20% of total records
dataframe.sample(frac=0.2, replace=True)#returns 10% of total records split by group
dataframe.groupby('Species').sample(frac=0.1)
```

**Dplyr**

Dplyr 中的界面非常相似。您可以使用 **slice_sample()** 函数，或者为固定数量的记录指定 **n** ，或者为一定比例的记录指定 **prop** 。此外，您可以指定**替换**以允许或不允许对同一行进行多次采样。

```
#returns 20 random samples
dataframe %>% slice_sample(n=20)#return 20% of total records
dataframe %>% slice_sample(prop=0.2, replace=True)#returns 10% of total records split by group
dataframe %>% group_by(Species) %>% slice_sample(prop=0.1)
```

## 连接

连接数据框架也是一个常见的用例。(join 操作范围很广，但我不打算在此详述)

但是，随后您将学习如何在 Pandas 和 Dplyr 中执行完全(外部)连接。

图像您有两个共享一个公共变量“key”的数据帧:

```
#Python Pandas
A = dataframe[[“Species”, “Sepal_width”]]
B = dataframe[[“Species”, “Sepal_length”]]#R Dplyr:
A <- dataframe %>% select(Species, Sepal_width)
B <- dataframe %>% select(Species, Sepal_length)
```

**熊猫**

对于所有的加入操作，你可以使用 Pandas 中的“[合并](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html)”功能，并指定你想加入什么，如何加入(外部，内部，左侧，右侧，..)您想加入哪个键:

```
#Join dataframe A and B (WHAT), with a full join (HOW) by making use of the key "Species" (ON) 
pd.merge(A, B, how="outer", on="Species")
```

**Dplyr**

在 Dplyr 中，语法非常相似，但是，对于每种连接类型，都有[单独的函数。在本例中，我们将再次使用 **full_join()** 函数执行完全连接:](https://dplyr.tidyverse.org/reference/join.html)

```
#Join dataframe A and B (WHAT), with a full join (HOW) by making use of the key "Species" (ON)
A %>% full_join(B, by="Species")
```

## 连接/绑定行和列

有时我们不想连接我们的数据帧，而只是通过行或列附加两个现有的数据帧。熊猫和 Dplyr 都有一个很好的界面来实现这一点:

熊猫

在 Pandas 中，可以用 **concat()** 方法连接两个数据帧。默认值按行连接数据帧。通过指定轴(例如轴= 1)，可以通过列连接两个数据帧。

请注意，如果某个值没有出现在其中一个数据帧中，它会自动填充 NA。

```
#Concatenate by rows
pd.concat([A,B])#       Species  Sepal_width  Sepal_length
#0       setosa          3.5           NaN
#1       setosa          3.0           NaN
#2       setosa          3.2           NaN
#3       setosa          3.1           NaN
# ... #Concatenate by columns 
pd.concat([A,B], axis=1)#       Species  Sepal_width    Species  Sepal_length
#0       setosa          3.5     setosa           5.1
#1       setosa          3.0     setosa           4.9
#2       setosa          3.2     setosa           4.7
#3       setosa          3.1     setosa           4.6
# ...
```

**Dplyr**

在 Dplyr 中，有两个独立的函数用于绑定数据帧: **bind_rows()** 和 **bind_columns()。**

请注意，如果某个值没有出现在其中一个数据帧中，那么在应用 bind_rows()时，它会自动填充 NA。另外，请注意 R 如何自动更改列名(以避免重复)。使用**可以改变这种行为。name_repair 参数。**

```
#Bind by rows
A %>% bind_rows(B)#   Species Sepal_width Sepal_length
# 1 setosa          3.5           NA
# 2 setosa          3             NA
# 3 setosa          3.2           NA
# 4 setosa          3.1           NA
# ...#Bind by columns
A %>% bind_cols(B)#  Species...1 Sepal_width Species...3 Sepal_length
# 1 setosa              3.5 setosa               5.1
# 2 setosa              3   setosa               4.9
# 3 setosa              3.2 setosa               4.7
```

Pfew！恭喜你！你可能是第一个读到这篇文章/备忘单结尾的人。您可以通过给我鼓掌或在下面给我留言来获得奖励:)

图片来自 [Giphy](http://gph.is/13EZhH1)