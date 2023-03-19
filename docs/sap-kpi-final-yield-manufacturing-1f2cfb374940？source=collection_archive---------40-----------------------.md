# SAP KPI:最终产量(制造)

> 原文：<https://towardsdatascience.com/sap-kpi-final-yield-manufacturing-1f2cfb374940?source=collection_archive---------40----------------------->

## 连接哪些 SAP 表来分析最终产量和首次通过率

![](img/4cf6be7a47c6cd191213bc255f1e3060.png)

与生产计划 PP 模块相关的 SAP 表格(图片由作者提供)

**动机:**

即使是最好的制造工艺也会产生废料，至少时不时会有一点。分析一段时间内的最终产量，并将 ERP 中的数据存储在数据仓库中，这难道不是很有趣吗？下面的 SQL 从 SAP BI 的角度向您展示了如何实现这一目标。请注意，由于所有 SAP ERP 都是公司独有的，因此对于此请求没有 100%正确和完整的解决方案。但它肯定会指引你正确的方向。

**解决方案:**

如果您在最终产量之后，AFKO 是感兴趣的中央 SAP 表。AFKO(德语为 Auftragskopf)代表订单标题数据 PP 订单。

```
truncate table [etl].[Yield]
select * into [etl].[Yield] from [Afko]
```

QMFE 是关于质量通知的，我们将使用该表来接收与生产相关的中断(字段 Fmgeig 代表缺陷数量)。字段 Fegrp、Fecod 和 Fekat 都存储问题信息。我们在生产订单(Fertanr 和 Aufnr)和 Qmnum(通知信息)级别加入这些信息:

```
update [etl].[Yield] 
set ProductionRelatedOutage= 
(
select sum(fmgeig)
from [qmfe] as a, [qmel] as b
where b.FERTANR=[etl].[Yield].AUFNR and a.qmnum in 
(
 select b.qmnum
 FROM [qmel]
 )
and fegrp=’WhatEverYourCompany’ and FEKAT=’WhatEverYourCompany' and FECOD=’WhatEverYourCompany' 
)
```

然后，通过交付数量(Igmng)除以订单接收量(Gamng)来计算产量:

```
update [etl].[Yield] 
set Yield =[IGMNG] / nullif([GAMNG],0)
```

或者有时甚至更具体地，您将通过交付数量除以交付数量+确认的废料信息(Iasmg)来计算产量-与生产相关的停机:

```
update [etl].[Yield] 
set YieldMoreSpecific =[IGMNG]/nullif(([IGMNG]+[IASMG]-isnull([ProductionRelatedOutage],0)),0)
```

为了接收生产订单状态，我们必须加入 Jest(单个对象状态)表。我们将只考虑非不活跃的(<> X):

```
update [etl].[Yield] 
set Status = b.stat
from [etl].[Yield] as a
inner join [aufk] as c
on a.aufnr = c.AUFNR
inner join [JEST] as b
on b.objnr = c.objnr
where b.inact <>’X’
```

作为补充，我们还可以使用每个内部连接的确认数量信息(Igmng)来增加返工的产量:

```
update [etl].[Yield] 
set YieldFromRework= QuantityFromRework
from [etl].[Yield] as a
inner join (select sum(igmng) as QuantityFromRework, maufnr
from [etl].[Yield]
group by maufnr) as b
on b.maufnr= a.aufnr
```

我们最终可以用它来计算首次通过率，即数量(来自 MSEG)-返工产量除以 Gamng:

```
update [etl].[Yield] 
set FirstComeThroughRate =( MSEGMenge -isnull(YieldFromRework,0))/GAMNG
```

**恭喜:**

我们刚刚计算了 ERP 系统 SAP 的最终产量和首次通过率。非常感谢阅读，希望这是支持！有任何问题，请告诉我。你可以在 [LinkedIn](https://de.linkedin.com/in/jesko-rehberg-40653883) 或者 [Twitter](https://twitter.com/DAR_Analytics) 上和我联系。

最初发布于我的网站 [DAR-Analytics](http://DAR-Analytics.com) 。