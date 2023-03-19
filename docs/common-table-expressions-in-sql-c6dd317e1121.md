# 一个永远改变了我编写查询方式的技巧

> 原文：<https://towardsdatascience.com/common-table-expressions-in-sql-c6dd317e1121?source=collection_archive---------10----------------------->

## 利用通用表表达式来简化复杂查询的编写和故障排除

![](img/e47add8238b11d7aac8af35c57916cb9.png)

Jan Antonin Kolar 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 什么是常见的表表达式

当编写复杂的查询时，为了可读性和调试，将它们分成更小的块通常是有用的。通用表表达式(cte)提供了这种能力，我发现它们是我的 SQL 工具箱中最有用的工具之一。

cte 实现起来非常简单。他们以一个简单的`WITH`陈述开始，你要去的新 CTE 的名字`SELECT`。他们是这样开始的:

```
WITH
    cte_name AS (
        SELECT
            ...
    )
```

美妙之处在于，您可以将多个 cte 链接在一起，数量不限。让我们看看他们中的一些人会是什么样子。

```
WITH
    cte_name AS (
        SELECT
            ...
    ),

another_cte AS (
    SELECT * FROM foo
    JOIN cte_name ON cte_name.id = foo.id
)

SELECT * FROM another_cte
LIMIT 10
```

这很好地说明了这个概念。第一个 CTE 运行第一个查询并将其存储在名为`cte_name`的内存中，第二个 CTE 将`cte_name`表连接到第二个 CTE 中的`foo`表。您可以以多种方式使用这种模式，但是它通过将复杂的查询分解成逻辑部分来简化构造。

**注意:**需要注意的一件小事是在第一个 CTE 分隔每张表之后`,`在哪里。

最后，通过在生成的 CTE 上运行一个独立的`SELECT`语句来完成这个过程。

当然，强大的功能是运行复杂得多的逻辑。每个 CTE 可以包含许多`SELECT`语句、`JOIN`语句、`WHERE`子句等。为了可读性和可理解性，使用它们来组织您的查询。

**提示:**为了方便调试或构建您的查询，您可以通过简单地注释掉剩余的代码并在每个 cte 之后运行 select 来测试每个 cte。像这样。

```
WITH
    cte_name AS (
        SELECT
            ...
    ) --, Make sure to comment out the comma

SELECT * FROM cte_name
LIMIT 10

-- another_cte AS (
--     SELECT * FROM foo
--     JOIN cte_name ON cte_name.id = foo.id
-- )

-- SELECT * FROM another_cte
-- LIMIT 10
```

# 现实生活中的例子

我为在雪花中创建的视图编写了一个查询。如果没有 cte，这将会变得更加困难。

```
WITH DAILY as (
    SELECT ID
    FROM "LOGS_DAILY"),
MAP AS (
    SELECT SOURCE_ID AS ID, ANY_VALUE(UUID) AS UUID
    FROM "CONTACT_MAP"
    WHERE SOURCE_ID_NAME = 'ID'
    AND DT = (SELECT MAX(DT) FROM "CONTACT_MAP")
    GROUP BY SOURCE_ID),
CONTACT AS (
    SELECT CONTACT_UUID, SITE_UUID
    FROM "CONTACT_MASTER"
    WHERE DT = (SELECT MAX(DT) FROM "CONTACT_MASTER")),
ACCOUNT AS (
    SELECT *
    FROM "ACCOUNT"
    WHERE SITE_STATUS = 'Active')
SELECT DISTINCT *
FROM DAILY
LEFT JOIN MAP ON MAP.ID = DAILY.ID
LEFT JOIN CONTACT ON CONTACT.CONTACT_UUID = MAP.CONTACT_UUID
LEFT JOIN ACCOUNT ON ACCOUNT.SITE_UUID = CONTACT.SITE_UUID
LIMIT 100
```

# 结论

公共表表达式(cte)是查询工具箱中的一个强大工具，它允许您获取复杂的、分层的 SELECT 语句，将它们分解成更易于管理的块，然后最终将它们组合在一起。如果你今天没有使用它们，试一试，我相信它们会成为你经常去的地方！

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。一个月 5 美元，让你可以无限制地访问成千上万篇文章。如果你使用[我的链接](https://medium.com/@broepke/membership)注册，我会赚一小笔佣金，不需要你额外付费。