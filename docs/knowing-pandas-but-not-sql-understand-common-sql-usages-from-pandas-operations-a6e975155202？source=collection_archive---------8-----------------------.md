# 懂熊猫却不懂 SQL？从 Pandas 操作中了解常见的 SQL 用法

> 原文：<https://towardsdatascience.com/knowing-pandas-but-not-sql-understand-common-sql-usages-from-pandas-operations-a6e975155202?source=collection_archive---------8----------------------->

## 从熊猫到 SQL

![](img/dac60180b6e3c40984a52441a79aaf1f.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

随着数据科学就业市场的不断增长，许多人正在转行。不是每个人都有时间去攻读数据科学或类似领域的学士或硕士学位。相反，许多人使用逆向工程方法开始他们的数据科学教育。他们只是直奔主题，看看数据科学家在做什么。

好的。似乎大多数数据科学家只是用熊猫来处理一堆数据帧。因此，有抱负的数据科学追求者开始学习熊猫。你猜怎么着他们中的大多数人都可以很好地掌握这个库的要点——不仅是因为他们的辛勤工作，也是因为这个令人敬畏的数据处理工具的用户友好的设计。

然而，他们可能会错过数据科学领域的一些重要支持工具，其中一个工具是 SQL(结构化查询语言)。由于英语是国际公认的语言，SQL 是数据库世界中的一种共享语言。所有主流数据库，如 Microsoft SQL、Oracle、PostgreSQL 和 MySQL，都支持 SQL，尽管确实存在一些变体。

在本文中，我想向您展示我们如何从可比较的 pandas 操作中理解常见的 SQL 用例。可以看出，这篇文章是为那些擅长熊猫，但需要学习 SQL 的人准备的。请注意熊猫是多才多艺的，所以当我向你展示熊猫手术时，几乎可以肯定你有另一种方法来做。关键是从 pandas 转移到 SQL，而不是不同 pandas 操作之间的交叉对话。

事不宜迟，让我们开始吧。

## 0.样本数据集

为了当前的教程，让我们使用 seaborn 包的 mpg 数据集，它有一堆汽车的燃油效率数据，如下所示。因为有比我们需要的更多的列，为了简单起见，我只保留一些列。

演示数据集

当我介绍 SQL 语句时，我会尽量做到与数据库平台无关。在数据库中，我们就简单的称这个表为 mpgtbl 吧。虽然有些人喜欢使用大写字母作为关键字，但我个人习惯于小写字母。

## 1.基于特定标准选择数据

在 pandas 中，我们使用以下方法选择满足特定标准的所需行。假设我们想选择汽车，其原产地是美国，重量超过 4900 磅。

熊猫精选数据

相应的 SQL 语句如下。

```
**select** * 
**from** mpgtbl 
**where** origin = 'usa' **and** weight > 4900
```

*   `select *` : select 是关键字，表示选择……星号表示所有列。如果要选择一些列，可以简单的列出来:`select mpg, horsepower`。
*   `from the_table`:指定从中选择数据的表格
*   `where`:指定选择标准。请注意，比较运算符只是简单的`=`，而不是`==`。AND 逻辑运算符是`and`。
*   字符串:对字符串使用单引号通常是个好主意，尽管有些数据库确实支持双引号。

## 2.选择前 3 行

在 Pandas 中，我们可以使用`head`函数来选择前 N 行。相应的 SQL 语句使用`limit`关键字，如果定义了标准，它会指定匹配标准的前 N 行。

```
# Pandas
df.head(3)# SQL
**select** * 
**from** mpgtbl 
**limit** 3
```

你想选择第 4-6 行吗？这里有一个可能的解决方案。

```
# Pandas
df.iloc[3:6, :]# SQL
**select** *** **from** mpgtbl 
**limit** 3 
**offset** 3;
```

在 Pandas 中，我们可以使用`iloc`属性进行数据选择。请注意`iloc`使用 index，index 从 0 开始，所以要得到第 4–6 行，我们需要指定 3:6，在 6 之前结束。

在 SQL 中，我们可以通过请求偏移量 3 来提供额外的限制，这样我们就可以得到第 4–6 行。

## 3.选择唯一的值

Pandas 有内置的`unique`函数，可以为特定的列选择不同的值。

```
# Pandas
df['origin'].unique()# SQL
**select** **distinct** origin 
**from** mpgtbl
```

在 SQL 中，我们将简单地把`distinct`关键字放在列或列列表之前。

## 4.排序行

我们使用`sort_values`对熊猫中的行进行排序。在 SQL 中，我们使用 order 关键字。默认情况下，在这两种情况下，值都按升序排序。当您需要按降序排列数据时，我们有不同的选项。

```
# Pandas
df.sort_values(['mpg', 'weight'], ascending=[False, True])# SQL
**select** *** **from** mpgtbl 
**order** **by** mpg **desc**, weight
```

在 SQL 中，当需要反转列的顺序时，需要在列名后面使用`desc`。

## 5.成员资格检查

这个操作在 pandas 和 SQL 之间是相似的，都涉及到以某种形式使用与`in`相关的关键字。

```
# Pandas
df[df['origin'].isin(['japan', 'europe'])]
df[~df['origin'].isin(['usa'])]# SQL
**select** *** **from** mpgtbl 
**where** origin **in** ('japan', 'europe');**select** *** **from** mpgtbl 
**where** origin **not** **in** ('usa')
```

*   Pandas 使用`isin`方法来确定行的值是否包含在项目列表中。要否定布尔值，只需在表达式前使用~符号。
*   SQL 使用`in`或`not in`进行成员资格检查。SQL 不使用方括号，而是使用一对括号来列出所有项目。

## 6.分组频率表

我们经常需要知道按一列或多列分组的项目出现的频率。你应该这么做。

```
# Pandas
df.groupby('origin').size()
df.groupby(['origin', 'model_year']).size()# SQL
**select** origin, ***count*(***)** 
**from** mpgtbl 
**group** **by** origin; **select** origin, model_year, ***count*(***) 
from** mpgtbl 
**group** **by** origin, model_year
```

*   `groupby`函数创建一个`GroupBy`对象，`size`方法将使用该对象计算每组的行数。
*   在 SQL 中，`count`方法将计算记录的数量。当数据被分组时，它将分别对每组进行计数。这里的关键语法是`group by col1, col2, col3…`。
*   在这两种情况下，您都可以指定多个列。

## 7.创建频率表

是上一个的后续操作。假设我们想要创建一个数据表用于长期存储。在熊猫身上，我们创造了一个新的`DataFrame`。为了引入一个新特性，假设我们想给 frequency count 列命名为`car_count`。

```
# Pandas>>> df_summary = df.groupby('origin', as_index=False).size().rename(columns={"size": "car_count"})
>>> df_summary
   origin  car_count
0  europe         70
1   japan         79
2     usa        249
```

如上图所示，该操作使用参数`as_index`的设置作为`groupby`功能中的`False`。

让我们看看如何在 SQL 中实现它。

```
# SQL**create** **table** car_origin **as** 
**select** origin, ***count*(***) as** car_count 
**from** mpgtbl 
**group** **by** origin;
```

*   创建新表的语法是`create table tbl_name as the_select_stmt`。实际上，从语句中选择的数据将作为数据保存在新创建的表中。
*   在前一节中，我们已经使用了`count`方法，但是在这里，使这个操作不同的是给 count 字段一个名称，语法是`count(*) as the_name`。

## 8.总体聚合操作

除了频率之外，我们还对数据集执行了一些聚合操作，比如最大值、最小值和平均值。

```
# Pandas
df.agg({"mpg": ["mean", "min", "max"]})
```

如果希望对其他列执行这些聚合函数，可以向 dictionary 参数添加额外的键值对。相应的 SQL 操作如下所示。

```
# SQL
select *avg*(mpg), *min*(mpg), *max*(mpg) from mpgtbl
```

如果您想给这些计算字段命名，您仍然可以使用`calculated_field as the_name`格式。

## 9.按组的聚合操作

扩展前面的操作，我们经常需要按组创建聚合。这是通过在 pandas 中使用`groupby`函数操作 GroupBy 对象来实现的，如下所示。

```
# Pandas
df.groupby('origin').agg(
    mean_mpg=("mpg", "mean"),
    min_hp=("horsepower", "min")
)
```

上面的操作使用了命名聚合，它不仅指定了列，还指定了要应用的函数。当然，正如开始时提到的，还有其他方法来创建集合。您可以在我的上一篇文章中找到更多关于 GroupBy 对象的操作。

[](/8-things-to-know-to-get-started-with-with-pandas-groupby-3086dc91acb4) [## 熊猫小组入门的 8 件事

### Groupby 功能如此强大，对于新手来说可能听起来有些望而生畏，但你不必了解它的所有特性。

towardsdatascience.com](/8-things-to-know-to-get-started-with-with-pandas-groupby-3086dc91acb4) 

当您在 SQL 中需要聚合时，它的语法对我来说看起来稍微干净一点。

```
# SQL**select** ***avg***(mpg) **as** mean_mpg, ***min***(horsepower) **as** min_hp 
**from** mpgtbl 
**group** **by** origin
```

到目前为止，一切对你来说都应该很简单。

## 10.连接表格

当我们有多个`DataFrame`想要合并时，可以在 pandas 中使用`merge`函数，如下图。请注意，df_summary 是在#7 中创建的，它有两列:`origin`和`car_count`。

```
df.merge(df_summary, on="origin")
```

在 SQL 中，我们可以有非常相似的连接逻辑。

```
**select** *** **from** mpgtbl
**join** car_origin **using** (origin)
```

这里的新关键字是连接和使用。join 连接要联接的两个表，而 using 用于指定列。请注意，当相同的列用于连接时，该语法是相关的。

在更一般的情况下，表可能有不同的列用于连接。以下是您在这两种方法中可以做的事情。

```
# Pandas
df.merge(another_df, left_on="colx", right_on="coly")# SQL
**select** *
**from** tbl1 x
**join** tbl2 y **on** x.col1 = y.col2
```

您现在使用`on`而不是`using`来指定 SQL 中用于连接的列。另一件要注意的事情是，您可以创建一个简短的表引用，以便于引用。在示例中，我们分别将这两个表称为`x`和`y`。

默认情况下，连接是`inner`类型的。SQL 中也有其他类型的连接。这里有一个有用的参考，你可以开始。

[](https://www.w3schools.com/sql/sql_join.asp) [## SQL 连接

### JOIN 子句用于根据两个或多个表之间的相关列来组合它们中的行。让我们看一个…

www.w3schools.com](https://www.w3schools.com/sql/sql_join.asp) 

## 11.更新记录

当您需要更新特定列的记录时，您可以在 Pandas 中执行以下操作。假设“福特都灵”的正确重量应该是 3459，而不是 3449。

```
>>> df.loc[df["name"] == "ford torino", "weight"]
4    3449
Name: weight, dtype: int64
>>> df.loc[df["name"] == "ford torino", "weight"] = 3459
>>> df.loc[df["name"] == "ford torino", "weight"]
4    3459
Name: weight, dtype: int64
```

我们使用`loc`属性访问特定的单元格并成功更新记录。SQL 有类似的逻辑，下面显示了一个可能的解决方案。

```
**update** mpgtbl
**set** weight = 3459
**where** name = 'ford torino'
```

这里新增的关键词是`update`和`set`。一般语法如下:

```
**update** tbl_name
**set** col1 = val1, col2 = val2, ...
**where** the_condition
```

## 12.插入记录

当您需要向现有数据帧添加记录时，有多种方法可以完成。这里向您展示了一个可能的解决方案。为了简单起见，让我们假设您想要再添加两条记录，而这些记录只是现有的`DataFrame`对象中的最后两行。

```
new_records = df.tail(2)
pd.concat([df, new_records]).reset_index(drop=True)
```

当您插入新记录时，您可以使它们成为另一个`DataFrame`对象，然后您可以使用`concat`方法来连接这些`DataFrame`对象。下面是如何在 SQL 中进行同样的操作。

```
**insert** **into** mpgtbl
**select** *** **from** mpgtbl **order** **by** "index" **desc** **limit** 2
```

*   我们使用`insert into`语法向现有表中添加新记录。
*   `select`部分只是提取表中最后两条记录的一种方法。请注意，该表包含一个名为`index`的列，它恰好是一个 SQL 关键字，因此我们可以使用双引号使它成为一个列名。顺便说一下，我倾向于对任何名字使用非关键字，只是为了避免这样的麻烦。

更多情况下，我们希望添加单独的数据记录，而不是从现有表中提取的记录。在 SQL 中，更常见的插入操作如下所示。

```
**insert** **into** tbl_name (col_list)
**values** (value_list1), (value_list2), ...
```

具体来说，我们在值`keyword`之后指定每个记录的值。请注意，如果要为所有列添加值，可以省略`(col_list)`部分。

## 13.删除记录

数据集可能包含我们不需要的记录。在大多数情况下，我们会删除符合特定标准的记录。对于熊猫来说,`drop`方法是完美的。一个可能的解决方案如下所示。

```
df.drop(df[df['name'] == 'ford torino'].index)
```

SQL 解决方案涉及到 delete 关键字的使用。**请对条件的指定极其谨慎，因为如果您离开条件，所有内容都将从表中删除。**

```
**delete from** mpgtbl
**where** name = 'ford torino'
```

## 14.添加列

有时我们需要向现有的数据集中添加新的列。在熊猫身上，我们可以做一些如下的事情。请注意，这只是一个微不足道的例子，我们知道数据集只包含 20 世纪的年份，所以我们只需添加 1900，使年份成为 4 位数。

```
df['full_year'] = df['model_year'] + 1900
```

要在 SQL 中做同样的操作，我们需要引入一个新的关键字— `alter`，它涉及到表的更新。让我们看看。

```
**alter** **table** mpgtbl
**add** full_year **as** (model_year + 1900)
```

我们只需要在关键字`add`之后指定新的列并包含计算。如果希望将计算出的列物理存储在数据库中，可以在计算之后添加`persisted`关键字。

## 15.移除列

如您所知，在 Pandas 中，`drop`方法不仅适用于行，也适用于列。下面的代码片段向您展示了如何在 Pandas 中放置列。

```
df.drop(columns=["mpg", "horsepower"])
```

与添加列类似，删除列是对现有表的一种修改。因此，正如你可能猜到的，这个操作使用了`alter`。

```
**alter** **table** mpgtbl **drop** **column** name;
```

## 16.串联数据集

在上一节中，当我们学习插入记录时，我们在 Pandas 中使用了`concat`方法。如您所知，这种方法通常用于连接共享相同列的数据集。

```
pd.concat([df, df])
```

在 SQL 中，这个操作涉及到`union`关键字，如下所示。

```
**select** *** **from** mpgtbl
**union** **all**
**select** *** **from** mpgtbl
```

请注意，您可以选择省略`all`，在这种情况下，将只保留后一个表中的不同记录。

# 结论

在本文中，我们回顾了您在数据科学项目中可能遇到的 16 种常见数据操作。具体来说，我们假设您对基于 Pandas 的操作有很好的理解，并且我们访问了使用 SQL 执行的相同或基本相同的操作。我希望您现在已经很好地理解了 SQL。

感谢阅读这篇文章。通过[注册我的简讯](https://medium.com/subscribe/@yong.cui01)保持联系。还不是中等会员？使用我的会员链接，通过[支持我的写作。](https://medium.com/@yong.cui01/membership)