# 调试 Dbt:当表名是保留关键字时

> 原文：<https://towardsdatascience.com/debugging-dbt-when-your-table-name-is-a-reserved-keyword-faf81ab6376b?source=collection_archive---------22----------------------->

## 如何配置 dbt 来使用名称类似 order 和 group 的表

![](img/331f6a6ae835e1898aa079de10be351d.png)

罗曼·博日科在 [Unsplash](https://unsplash.com/s/photos/table?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

欢迎来到我的最新系列-调试 dbt。作为一名分析工程师，我每天都在工作中使用 dbt 来编写健壮的数据模型。它是简化和容器化数据模型运行方式的一个很好的工具。虽然它使事情变得更容易，但我仍在不断地学习关于这个工具的一些东西。我想用我的奋斗和学习作为一种方式来教导社区中可能遇到同样问题的其他人。

今天，我将解决一个问题，我曾使用雪花表与保留关键字同名。如果您正在使用 Fivetran 接收您的数据，那么您可能会遇到类似的问题。

我们使用 Fivetran 获取的两个数据源是 Zendesk 和 Shopify。这两个源都包含名为*订单*和*组*的表。因为这些词在许多数据库中被认为是保留关键字，在我的例子中是雪花，那么我们将会遇到一些用 dbt 运行它们的问题。

这可能会以如下所示的错误形式出现:

```
SQL compilation error: syntax error line 1 at position 26 unexpected 'group'. syntax error line 1 at position 31 unexpected '<EOF>'.
```

如何解决这个问题:

1.  转到 src.yml 文件，该文件包含相应源表的数据库/模式信息。
2.  在你的表名下添加 [*标识符*](https://docs.getdbt.com/reference/resource-properties/identifier) 和表名。

```
identifier: ORDER
# or 
identifier: GROUP
```

3.还要添加另一行，说明*引用*，并将嵌套的*标识符*设置为真。

```
quoting:
  identifier: true
```

将这些内容添加到这些源代码的 src.yml 文件中，改变了在 dbt 中编译 SQL 的方式。而不是将源表作为

```
select * from raw.zendesk.group
```

现在它将被读作

```
select * from raw.zendesk."GROUP"
```

更改只是在表名中添加引号，让 SQL 知道这实际上是表名，而不是保留关键字。

诸如此类的问题通常表现在许多不同类型的错误消息中。可能很难确切地知道是什么导致了某个问题，但它通常以相当简单的方式结束。请务必关注我的调试 Dbt 系列的其余部分。接下来是增量模型！