# 一部有两个时间维度的三幕达克斯戏剧

> 原文：<https://towardsdatascience.com/a-dax-drama-in-3-acts-with-two-time-dimensions-aa65abbf5f41?source=collection_archive---------12----------------------->

## 我的一个客户提出了一个看起来相对容易的请求，但我花了三天时间和很多精力去解决。我重新学习了一些关于达克斯的课程，我邀请你跟随我的三幕剧来解决问题。

![](img/73679e2c44aa0d0b8154be9b7304118a.png)

[engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 搭建舞台

我的客户每天将销售文档的所有元数据加载到他的数据仓库数据库的快照表中。从每月的第一天到最后一天，每天都会覆盖快照数据。随着新月份的开始，新的月度快照开始，上个月不再更改。每个快照都有过去十年中创建的文档的元数据。这个表非常大，有超过 2 . 2 亿行，需要将近 11 GB 的 RAM，并且还在增长。

因为他们使用 SQL Server Analysis Services (SSAS)将数据存储在表格模型中，所以我可以在 Vertipaq Analyzer 中向您展示该表的大小:

有些数据是引用的。这些报价是企业要求的报告中有趣的部分。

他们需要查看一段时间内的报价计数和报价销售额，并且希望将这两个数字与 12 个月前的结果进行比较。

最大的问题是，需要从 12 个月前的快照中检索 12 个月前创建的报价数据。

用户应该能够用 Power BI 中的限幅器定义任意时间窗口。该报告必须使用相应的快照，自动调整 12 个月前相同时间窗口的检索。

很简单，不是吗？

还有一点复杂的是，他们想要区分公开报价、成功报价和失败报价。他们使用“提供状态”维度来区分每种状态。

现在，让我们拉开帷幕。

![](img/090095138708dbce846681bb040b00f5.png)

照片由[格温·金](https://unsplash.com/@gwenking?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 第一个动作—检索当前期间

您可能已经注意到，我需要处理两个日期维度:

*   在 SSAS 模型中，快照日期被命名为“时间”
*   文档创建日期在 SSAS 模型中命名为“创建时间”
    我在这个表的日期列上使用了一个切片器，允许用户过滤报告中报价的创建日期

我现在不想透露多个日期维度的利弊。这是我需要面对的事实，我无法改变。

第一步是找到实际时间窗口的正确快照:

```
Last Snapshot by Creation Date = VAR LastCreationDate =
        MAX(‘Time Creation’[Date]) VAR GetSnapshot =
        CALCULATE(
            LASTNONBLANK(
                ‘Document’[Snapshot Date]
                ,SUM ( Document[SalesAmountNet] ) )
            ,’Document’[Snapshot Date] <= LastCreationDate
        )RETURN
    GetSnapshot
```

我将这个公式存储在 Power BI 文件的一个中间度量中，因为我在其他度量中需要这个值。

哦，我忘了说，所有需要的度量都是独一无二的，所以我直接在 PowerBI 文件中创建它们，它使用一个到 SSAS 模型的活动连接。

获取韩元报价的方法如下:

```
Quotes won =CALCULATE (
    SUM ( Document[SalesAmountNet] )
    ,FILTER ( ‘Offer Status’, ‘Offer Status’[Offer Status Number] IN {“2”, “11”, “13” } )
    ,FILTER(
        VALUES(‘Document’[Snapshot Date]),
        ‘Document’[Snapshot Date] = [Last Snapshot by Creation Date]
    )
)
```

我需要两个进一步的措施来获得开放和丢失的报价。这三种衡量标准的区别仅在于用于过滤数据的报价状态编号列表不同。我还需要另外三个度量来计算引用的数量。所以，我总共需要六个基本度量。

在这个测量中有一个小故障，这是我在验证结果时发现的。稍后我将向您展示这一点，在看到我如何解决其他问题后，这将更有意义。

# 第二步——找到 12 个月前的快照

计算所选创建日期的前一期是非常简单的。您需要使用 [PARALLELPERIOD()](https://dax.guide/parallelperiod/) 函数来回溯 12 个月:

```
VAR LastCreationDatePY =
    PARALLELPERIOD(‘Time Creation’[Date], -12, MONTH)
```

我可以使用这种机制来获取匹配的快照。

现在戏剧开始了。

我的第一次尝试是这样的:

```
Last Snapshot by Creation Date (PY) = VAR MaxLastCreationDatePY = EOMONTH(
                                LASTDATE(‘Time Creation’[Date]), -6) VAR GetSnapshot =
        CALCULATE(
            MAX(
                ‘Document’[Snapshot Date])
                ,’Document’[Snapshot Date] <= MaxLastCreationDatePY
                ,ALL(‘Document’[Snapshot Date])
            )RETURN
    GetSnapshot
```

我的理由是:我喜欢过滤文档表上的快照日期。因此，我从这个列中移除过滤器并更换过滤器。

令人惊讶的是我总是得到一个空的结果。为什么？

我花了一些时间，在一位同事的帮助下，才意识到在切片器的“时间创建”表上有一个过滤器。我需要用 ALL(时间创建)从那里移除过滤器来操作过滤器。

工作 DAX 公式如下:

```
Last Snapshot by Creation Date (PY) = VAR MaxLastCreationDatePY = EOMONTH(
                               LASTDATE(‘Time Creation’[Date]), -12) VAR GetSnapshot =
        CALCULATE(
            MAX(
               ‘Document’[Snapshot Date])
            ,’Document’[Snapshot Date] <= MaxLastCreationDatePY
            ,ALL(‘Time’)
            ,ALL(‘Time Creation’)
        )RETURN
    GetSnapshot
```

我需要全部(“时间”)，因为报告中的“时间”表也有一些外部影响。

**第一课:**不要忘记查看整张图片，并在正确的表格和列上操作过滤器。

# 第三步—使用正确的时间窗口计算正确快照上的数量

现在，我需要将正确的快照与文档创建日期的前一时期的时间窗口结合起来。

我一步一步地创建度量，直到得到有意义的结果。

这是我的第一个工作措施:

```
Quotes won (PY) = VAR LastCreationDatesPY =
        PARALLELPERIOD(‘Time Creation’[Date], -12, MONTH) VAR Result =
        CALCULATE (
            SUM ( Document[SalesAmountNet] )
            ,FILTER(‘Offer Status’,
            ‘Offer Status’[Offer Status Number] IN {“2”, “11”, “13” }
            )
            ,ALL ( ‘Time Creation’)
            ,LastCreationDatesPY
        )RETURN
    CALCULATE(
        Result
        ,FILTER(
            VALUES(‘Document’[Snapshot Date])
            ,’Document’[Snapshot Date] = [Last Snapshot by Creation Date (PY)]
        )
    )
```

我从这个措施中得到了一些有意义的结果，我认为我满足了要求。

但是，当我开始用来自源数据库的数据验证结果时，我注意到了显著的差异。有些事情还不太对劲。

我回到了绘图板。

经过几个小时的尝试和失败，我停下来休息了一会儿。

**第二课:**如果你陷入困境，走开，做些不同的事情

休息过后，我重新开始，尝试了一种完全不同的方法。

我使用 [DAX Studio](https://daxstudio.org/) 编写了一个 DAX 查询并获得中间结果，我可以用源数据库验证这些结果。

**第三课:**走一步算一步。不要试图做一个巨大的飞跃，并期望一切都会顺利。

![](img/252dbcf320b7f27f2bf4f73009c3ebc9.png)

由[在](https://unsplash.com/@historyhd?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上高清拍摄的历史

然后我意识到这个查询产生了正确的结果，我可以在 DAX 度量中使用这个查询。

这种方法是正确的选择，最终措施如下:

```
Quotes won (PY) = VAR LastCreationDatesPY =
        PARALLELPERIOD(‘Time Creation’[Date], -12, MONTH)RETURN
    SUMMARIZE(
        CALCULATETABLE(
            Document
            ,ALL(‘Time’)
            ,FILTER(
                VALUES(‘Time’[Date])
            ,’Time’[Date] = [Last Snapshot by Creation Date (PY)]
        )
        ,FILTER(‘Offer Status’,
            ‘Offer Status’[Offer Status Number] IN {“2”, “11”, “13” }
            )
        ,ALL(‘Time Creation’)
        ,LastCreationDatesPY
        )
    ,”Sum sales”, SUM ( Document[SalesAmountNet] )
    )
```

这项措施中有一些有趣的部分:

*   按“时间”[日期]过滤:我需要过滤时间维度表上的数据，以获得正确的快照。如前所述，第一幕的小节出现了小故障。这里的诀窍是过滤“时间”[日期]，而不是“文件”[快照日期]。
    过滤维度表总是比事实表更好的方法。它给了你更多的使用度量的灵活性，在我的例子中，它产生了正确的结果。
*   对报价状态和时间创建的过滤也是如此。我只过滤维度表。
*   不使用变量。
    这种方法是最大的游戏规则改变者。
    使用变量时应用过滤器是有区别的。简单地说，但不完全准确，过滤器不是“向后”应用于变量。当您需要在 DAX 度量中应用多个筛选器时，必须仔细测试筛选器是否按预期应用。你可以在这里阅读更多关于这个问题的内容:[DAX(SQLBI)中的变量](https://www.sqlbi.com/articles/variables-in-dax/)

第四课:记住基础知识。它们仍然适用，不管你认为你知道多少。

![](img/a4b5d0c3e8bff480a34bbbd5db69e715.png)

照片由[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

顺便说一句。第一幕的正确步骤如下:

```
Quotes won = CALCULATE (
        [Sales (Document)]
        ,FILTER ( ‘Offer Status’, ‘Offer Status’[Offer Status Number] IN {“2”, “11”, “13” } )
        ,FILTER(
            VALUES(‘Time’[Date]),
            ‘Time’[Date] = [Last Snapshot by Creation Date]
            )
        )
```

# 收场白

除了第一课，我不再重复上面提到的四课。

如果您尽可能过滤维度表，那将是最好的。如果你不知道下面的维度建模是如何工作的，你可以找到一些有用的链接。

通常，我更喜欢只有一个日期表；正如我的文章中所讨论的，用扩展的日期表改进你的报告的 3 种方法。

我怀疑这个特定的案例可能更难用一个中央日期表来解决。我没有用另一个型号试过。所以，我可能错了。

但是只要想想一个度量中需要的[user relationship()](https://dax.guide/userelationship/)调用以及我在变量方面遇到的问题——如果没有必要，我不想尝试一下。

最后但同样重要的是:
**第五课:**对照源数据测试和验证你的结果，直到你知道一切都是正确的。

了解维度建模的链接:

[数据建模最佳实践—第 1 部分 Power BI 和分析服务(YouTube)](https://www.youtube.com/watch?v=kiVXI7zjSzY)

[维度数据建模逐步指南](https://dwgeek.com/guide-dimensional-modeling.html/)

[尺寸建模示例(PDF)](http://www.databaseanswers.org/downloads/Dimensional_Modelling_by_Example.pdf)

[维度建模技术(金博尔集团)](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/)