# 如何连接 SQL 表？

> 原文：<https://towardsdatascience.com/how-do-you-join-sql-tables-a4b6ceb898bb?source=collection_archive---------14----------------------->

## WHERE 或 ON 子句？

![](img/98e835adac456ab74c518b877dbb07b3.png)

照片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

马上，我不使用也永远不会使用`WHERE`子句来连接表。

如果你正在使用`WHERE`子句，这篇文章试图说服你也这样做。相反，使用`ON`子句来实现表连接。

您使用`WHERE`子句执行表连接吗？请不要让我检查或帮助你找到你的查询中令人沮丧的障碍。

你在问我为什么吗？

我告诉过你，我不喜欢用于连接表的`WHERE`子句。我只用它来过滤数据。当用于实现表连接时，它以一种糟糕的方式(畏缩)让我恼火。我的思维变得超速，特别是在用于实现两个表之外的多个连接时。

类似下面的 SQL 查询伤害了我的眼睛，更糟糕的是我的大脑。使用`WHERE` 子句来实现表连接，会使表连接的顺序变得不清晰，也没有任何视觉提示。

使用 WHERE 子句连接多个表的 SQL 查询示例。这个 SQL 使用相同的 WHERE 子句进行过滤。

参见上面使用`WHERE`子句连接 6 个表的 SQL 查询。更糟糕的是，它还使用 3 个带有相同的`WHERE`子句的`AND`逻辑操作符来执行数据过滤。

使用`ON`子句实现相同结果的重写看起来像这样。奖励是非常明显的。

首先，我们使用了`WHERE`子句的查询尾部的混乱被很好地整理了。现在很明显`WHERE`子句在做什么——一件事——只有行过滤。

第二，简单地说，您可以识别和理解连接表的顺序，以创建保存查询结果的概念上的巨型表。

重写看起来也很有凝聚力。

# 表连接不使用 WHERE But ON 子句的更多理由

## 1.数据过滤

`WHERE`子句很优秀，更适合数据过滤，而不是连接表。

## 2.认知超载

在实现数据过滤和表连接时使用`WHERE`子句会造成认知超载。`WHERE`子句在同时用于实现表连接和数据过滤时会影响可读性和查询理解。

## 3.有限的

`WHERE`子句限制了你在不同的表上可以实现的连接。`WHERE`子句只实现了`INNER JOIN`，这是您可以使用`ON`子句执行的 5 种连接类型之一。

## 4.缺失记录

因为使用`WHERE`子句执行表连接在技术上与`INNER JOIN`的行为相同，所以连接的表中的记录必须满足`WHERE`子句中表达的条件，才能成为连接的表的结果的一部分。忽略这些基础知识可能会导致查询结果中遗漏重要的记录。在银行和汇款应用程序这样的金融应用程序中，这种疏忽可能是代价高昂的。

# 使用 ON 子句实现比内部连接更多的功能

正如我前面指出的，使用`WHERE`子句产生的结果与同时使用`ON`子句和`INNER JOIN`子句产生的结果相同。

除了使用`ON`子句实现`INNER JOIN`之外，您还可以实现其他形式的 SQL 表连接。这些其他类型的表连接的好处是有足够的参数将`WHERE`子句严格留给数据过滤。这样做也为您的查询带来了一致性，并减少了其他开发人员的 SQL 审查和维护难题，因为他们不必费力区分用于表连接和数据过滤的条件。

在本文的其余部分，除了内部连接之外，我将通过示例数据展示其他三种形式的 SQL 连接。

如果您想亲自操作，那么首先创建表并用示例数据作为种子。

创建表格，`person`和`contact`

创建表“person”的 SQL 查询

创建表“联系人”的 SQL 查询

下载样本数据 [person.csv](https://gist.github.com/ofelix03/2ef0d1cdfec43a10755ec871342daa05/archive/b1751827ba2d0801cdd38204747a1e2c00dd244a.zip) 和 [contact.csv](https://gist.github.com/ofelix03/957dc0c1e718d8758ef5ebc003455fd3/archive/f79bd45fe9c11b9c8cbd162db281ec6d47e48412.zip) 并分别植入表`person`和`contact`。

使用下面的查询解压缩并最终用示例数据播种这些表。记得用你下载的解压副本的路径替换`/path/to/person.csv`和`/path/to/contact.csv`。

如果您做的一切都是正确的，那么表上的 select 语句应该产生下面的输出

## 餐桌人员

```
SELECT * FROM person;
```

person.csv

## 表格联系人

```
SELECT * FROM contact;
```

contact.csv

# 除了内部连接之外的其他连接类型

除了`INNER JOIN`，还有 4 种其他类型的连接，分别是`FULL OUTER JOIN`、`LEFT OUTER JOIN`、`RIGHT OUTER JOIN`和`CROSS JOIN`。

在本文中，我们将讨论`LEFT OUTER JOIN , RIGHT OUTER JOIN`和`FULL OUTER JOIN`

## 左外部连接

如果两个表都满足用`ON`子句指定的条件，左外连接通过列出左表中的指定列并将它们连接到右表中的列来组合两个表。

当条件与左表中的列不匹配时，为右表中的列设置一个`NULL`值。

考虑下面的例子，一个`person`表位于表`contact`的左外部连接中。

工作台`person`为左工作台`contact`为右工作台。

您会注意到左边表格`person`中的`name`、`gender`、`age`列被连接到右边表格`contact`中的`phone_number`列。同样明显的是，没有匹配`person_id`的最后 4 行将`phone_number`设置为空。

```
SELECT person.name,
       person.gender,
       person.age, 
       person.phone_number
FROM person
LEFT OUTER JOIN contact
ON person.id = contact.person_id
```

## 输出:

表 person 和 contact 之间左外部连接的输出

## 右外部联接

右外连接通过列出右表中的指定列并在两个表都满足用`ON`子句指定的条件时将它们连接到左表中的列来组合两个表。

当条件与左表中的列不匹配时，为左表中的列设置一个`NULL`值。

考虑下面的例子，右外部连接中的一个`person`表与表`contact`。工作台`person`是左工作台`contact`是右工作台。

您会注意到右表中所有行的`phone_number`都被列出，并连接到左表中匹配记录的`name`、`gender`、`age`。

因为`person`表和`contact`表之间的关联是关系型的，确切地说是一个`One2many`关系，所以如果没有一个根实体 person 来拥有它，就不会有联系人记录。因此，我们的`RIGHT OUTER JOIN`结果仅限于有联系记录的人员。

如果来自`person`表的`name`、`gender`、`age`列表都满足`ON`条款规定的条件，则它们被连接到来自`contact`表的`phone_number`。

```
SELECT person.name, 
       contact.mobile_number, 
       contact.home_address
FROM person
RIGHT OUTER JOIN contact
ON person.id = contact.person_id
```

**输出:**

表 person 和 contact 之间右外连接的输出

## 完全外部连接

完全外部联接通过列出两个表中的列来组合这两个表。当条件不满足时，为左表或右表的列设置一个`NULL`值。

考虑下面的例子，一个`person`表和`contact`通过一个更完整的外部连接组合在一起，条件是`person`上的`id`列与`contact`表上的`person_id`列匹配。每个表中不满足该条件的行仍然显示在结果中，它们的列设置为`NULL`

```
SELECT person.name,
       person.gender,
       person.age, 
       person.phone_number
FROM person
FULL OUTER JOIN contact
ON person.id = contact.person_id
```

## 输出:

表 person 和 contact 之间完全外部连接的输出

# 结束语

希望我成功说服您放弃使用`WHERE`子句来连接表。如果什么都没有，你已经注意到使用`ON`子句给你的查询带来了多大的清晰度。还记得使用`ON`子句时可以执行的其他类型的连接。

希望这篇文章对你有所帮助。

知识是给世界的，所以我们分享。
祝你一切顺利。下次见。