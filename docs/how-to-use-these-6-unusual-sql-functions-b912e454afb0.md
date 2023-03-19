# 如何使用这 6 个不寻常的 SQL 函数

> 原文：<https://towardsdatascience.com/how-to-use-these-6-unusual-sql-functions-b912e454afb0?source=collection_archive---------19----------------------->

## 把这些放在你的工具箱里，你会准备好解决最困难的问题

![](img/a285075c58cbe4d239f784163fae7858.png)

照片由 [Joni Ludlow](https://unsplash.com/@joniludlow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/unusual?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

无论您是数据分析师、数据工程师还是分析工程师，您都需要了解您的 SQL。在数据世界中，这是一门必须掌握的语言。我们一直在使用它！

作为一名分析工程师，我经常为我的 [dbt](https://www.getdbt.com/) 数据模型编写 SQL。我一直在努力扩大我所知道的函数的数量，并改进我如何使用我已经知道的函数。

这里有一些不同寻常但很有帮助的 SQL 函数，可以帮助你了解你是一个 SQL 忍者还是仅仅在学习这门语言。

## 符号()

`SIGN()`函数根据给定的列是正数还是负数返回值。如果一个数字是> 0，则返回 1。如果一个数字是< 0，则返回-1。如果数字= 0，则返回 0。此函数仅适用于数字数据类型的列。

这个函数对于在`CASE`语句和`WHERE`子句中使用很有帮助。这使得按符号筛选列变得更加容易。

假设我们是一家银行，我们需要查看客户的月结单。我们希望标记所有帐户中有 0 美元或负金额的客户。

我们可以这样做:

```
SELECT
   customer_name,
   amount
FROM bank_statements 
WHERE SIGN(amount) IN (-1, 0)
```

这将为帐户中有负美元或零美元的客户生成一个带有`customer_name`和`amount`的表。

## 地板()

这个函数是在 SQL 查询中进行数学计算时常用的函数。它返回等于或小于所提供数字的最大整数。

请注意，它返回的数字等于或小于所提供的数字。这意味着当你对一个负数使用`FLOOR()`时，会产生一个更大的负数。

```
SELECT FLOOR(-15.5) FROM numbers
```

这将返回-16 作为您的答案，因为这是大于-15.5 的最大整数。

## 天花板()

这个函数是`FLOOR()`函数的姊妹函数。它不是返回最大的整数，而是返回大于或等于所提供数字的最小整数。

让我们看看上面的例子，但是现在使用 CEILING()函数。

```
SELECT CEILING(-15.5) FROM numbers
```

不是像`FLOOR()`函数那样返回-16，而是返回-15。-15 是大于或等于-15.5 的最小整数。

你有没有去过杂货店，收银员问你是否想凑够你的钱，把多余的零钱捐给当地的慈善机构？这个函数在杂货店数据库的后台会很有帮助。使用`CEILING`将为收银员提供向客户收费的新金额以及他们将捐赠的金额。

```
SELECT 
   transaction_id,
   transaction_amount,
   CEILING(transaction_amount) AS transaction_with_donation,
   CEILING(transaction_amount) - transaction_amount AS donation_amount 
FROM grocery_db
```

使用此功能，您可以获得您需要的关于交易的所有信息！

## 铅()

如果不包含至少一个窗口函数，这就不是一篇合格的 SQL 文章。我总是被教导说他们没有必要学习，但是我不确定是谁说的。窗口函数是 SQL 中最有用的函数之一。

`LEAD()`函数使您不必再对另一个表进行混乱的连接。它允许您访问当前正在查看的行之后的行中的值。这让比较变得超级容易。

让我们继续杂货店的例子。在一家杂货店中，有多个不同的结账通道 1-10。然后，在每一个收银台里都有不同的顾客。谁先到，谁就排在第一位，谁最后到，谁就排在最后。

![](img/74bf79a28c149d4a8e7786b097eb0ca0.png)

作者图片

我们想找到排在每个顾客后面的人。为了做到这一点，我们将在`customer_name`列上使用`LEAD()`函数，这样我们就可以得到下一个客户的名字。然后，我们通过`line_number`对其进行划分，因为只有同一行中的客户才能排在彼此之前/之后。最后，我们在`arrived_at`时间前订购，这样我们就可以准确地了解每一行的客户顺序。

```
SELECT 
   customer_name,
   LEAD(customer_name) OVER(PARTITION BY line_number ORDER BY arrived_at ASC) AS next_in_line
FROM grocery_customers
```

这将导致如下所示的结果:

![](img/1c3ecdfa0ae13e005b72f8bf3b73e8fc.png)

请注意，排在最后的客户在 next_in_line 列中有一个`NULL`值。只要当前行之后没有满足指定条件的行，`LEAD()`就会返回一个`NULL`值。由于 Fred 和 Haley 在他们的队列中排在最后，所以他们的`next_in_line`列有一个`NULL`值。因为 Sebastian 是第 2 行中的唯一一个人，所以他在该列中也有一个`NULL`值。

## 滞后()

与`LEAD()`功能相反，`LAG()`允许您将当前行与其之前的行进行比较，而不是与其之后的行进行比较。它仍然用于比较，它只是服务于一个不同的目的。

类似地，使用 LEAD()，您可以根据希望如何分隔列，从当前行和分区之前的行中选择希望输出的列。然后，最重要的部分是`ORDER BY`。这将决定你是否得到你想要的价值。如果你选择`ASC`或`DESC`值，那会改变你的整个查询。只要确保它符合你的问题和你要解决的问题的背景。

```
SELECT 
   customer_name,
   LAG(Name) OVER(PARTITION BY line_number ORDER BY arrived_at ASC)    AS ahead_in_line
FROM grocery_customers
```

这里，我们有相同的查询，只是现在我使用了`LAG()`函数。该功能将使客户排在我之前而不是之后。

![](img/62e665f20f37dd9723536cdbff4bb3cd.png)

作者图片

请注意，`NULL`值现在与使用`LEAD()`函数时相反。现在，那些排在第一位的人有了`NULL`值。Sebastian 仍然有一个`NULL`值，因为他是唯一排队的人。

在我的深入文章[这里](/how-to-use-sql-lead-and-lag-functions-35c0db633c5e?source=your_stories_page----------------------------------------)中可以看到更多关于如何使用这些函数的例子。

## IFF()

我个人从来没有使用过这个函数，但是我现在需要开始使用它，因为我知道它有多有用。它本质上是一个更简单的`CASE`语句。`IFF()`函数测试一个条件，如果条件为真，则返回一个指定值，如果条件为假，则返回另一个指定值。

> IFF(条件，真值，假值)

当使用一个列创建布尔列时，这很有帮助。假设您想要检查数据库中的哪些客户是老年人，或者超过 65 岁。您可以编写如下所示的查询:

```
SELECT 
   customer_name, 
   IFF(age >= 65, TRUE, FALSE) AS is_elderly 
FROM grocery_customers 
```

现在，我们可以简单地使用布尔型`is_elderly`列来过滤我们的客户，而不是查看年龄列并基于此添加过滤器。

## 结论

我不经常使用这些 SQL 函数，但是当我使用它们时，我的查询的速度和复杂度会有很大的不同。在您的工具箱中拥有这些函数对于在出现需要这些函数的问题时拥有最佳解决方案是至关重要的。

在面试中完成 SQL 测试时，它们也非常有用。有时你一时想不出简单的解决方案，知道这些奇特的功能会救你一命。相信我，我也经历过。事实上，面试官会对你的这些知识印象更加深刻。

[通过订阅我的电子邮件列表，了解有关 SQL 和分析工程师使用的其他工具的更多信息](https://mailchi.mp/e04817c8e57e/learn-analytics-engineering)。