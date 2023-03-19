# 检测财务数据中的欺诈和异常情况

> 原文：<https://towardsdatascience.com/detect-fraud-and-abnormalities-in-financial-data-f9f00696f657?source=collection_archive---------29----------------------->

## 如何在内部审计、财务和会计或控制等领域进行数据分析

![](img/222d9638cb394f41741455d148e16d37.png)

帕特里克·亨德利在 [Unsplash](https://unsplash.com/t/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

像 SAP 这样的软件处理公司的所有业务流程，如记账、控制、销售等。它还托管大量数据，尤其是财务数据，这些数据可能会带来重要的见解，需要由商业智能、会计或内部审计等部门进行控制。下面对常见检查的概述应该提供了一个实用的分析用例列表。在本文中，以 SAP FiCo 为例进行了说明，但是其他系统也有类似的情况。

## 可疑的变化

在 CDHDR 和 CDPOS 表的帮助下，您可以分析表中的变化，还可以识别可疑的过程，如订单中的不同值，该值从 20.000 €变为 19.999 €，刚好低于现有的限制。您需要的是对提到的表的访问(至少是读取)，对这些表如何工作的理解和一些 SQL。

*示例:SQL 结果—值的变化:*

```
UDATE       |CHANGENR |VALUE_NEW |VALUE_OLD                 01/01/2020  |1234     |20.000    | 
02/03/2020  |1234     |19.999    |20.000
```

另一个例子是查看客户的信用额度的变化频率[1]。某个订单之前的许多更改或更新也值得一看。

*示例:SQL 结果—计数变化:*

```
OBJECTCLAS  |OBJECTID  |FNAME    |Count_Changes                   KLIM        |543       |KLIMK    |6
```

## 检查重复项

为了监控主数据的质量，同时防止不正确的预订甚至欺诈，检查重复数据总是一个好主意—一个著名的例子是 SAP 数据中的客户数据。对于像这样的更深入的分析，您可能需要 SQL 之外的其他方法，并且您可能更喜欢 python 笔记本。

*示例:使用 Python 进行字符串相似性检查[2]:*

```
import distance
distance.levenshtein("customer_abc", "customer_abcd")
```

## 双重支付

重复支付意味着赔钱，因此你可以检查 BSEG 表中的财务记录是否有重复。以下 SQL 连接 BSEG 的 BSEG，以标识具有相同公司代码、金额等的记录。但是具有不同文档编号。

*示例:SQL 检查重复项[3]:*

```
SELECT B1.MANDT,B1.BUKRS,B1.GJAHR,B1.BELNR BELNR1,B1.BUZEI BUZEI1,B2.BELNR BELNR2,B2.BUZEI BUZEI2,B1.DMBTR FROM BSEG BSEG_1 JOIN BSEG BSEG_2 ON (BSEG_1.MANDT=BSEG_2.MANDT AND BSEG_1.BUKRS=BSEG_2.BUKRS AND BSEG_1.DMBTR=BSEG_2.DMBTR AND BSEG_1.SHKZG=BSEG_2.SHKZG) WHERE B1.BELNR!=B2.BELNR
```

## 周末和节假日交易

由于权责发生制会计(年度账目、流转税的提前返还等),在不寻常的日期过账可能会有风险。)或者诈骗。要获得带有创建日期或更改日期的记录，您可能需要来自 getfestivo [4]之类的开放 API 的假日日期。

## 不寻常的预订文本

要确定不应该存在的费用，您可以在 BKPF 表中使用以下内容搜索不寻常的预订文本:

*SQL 文本搜索示例:*

```
... FROM BKPF
WHERE UPPER(BKTXT) LIKE = “Cancellation”
OR UPPER(BKTXT)= “Credit note”
UPPER(BKTXT)= “fee”
UPPER(BKTXT)= “Switzerland”
.....
```

## 结论

这些只是 SAP 系统中少数可能的分析问题，但由于我们讨论的是财务数据，这是一个重要的话题，您和您的公司应该意识到这些问题，并渴望培养分析能力。在本文中，示例与 SAP 相关，但是其他具有财务模块的 ERP 系统(如 Oracle 或 DATEV)也将以相同的方式工作，并且案例也是相似的。如何将 SAP 与强大的谷歌云分析平台(BigQuery、Data Studio 或 recently looker 等数据分析工具的提供商)相结合，以获得强大的数据分析平台和宝贵的见解。如果您对 SAP 数据的实用数据分析方法感兴趣，[这篇文章](https://medium.com/@christianlauer90/sap-data-analytics-examples-for-internal-audit-financial-tax-accounting-and-controlling-analysis-1fbc81d01eac)可能也会让您感兴趣。

## 进一步的资料和阅读

[1]石，董，(2015)使用 CAATs 对 SAP 进行审计。

[2]pypi.org，【https://pypi.org/project/Distance/】T4(2021)

[3]DAB-GmbH，[https://www . da b-Europe . com/file admin/public data/Downloads/2016 _ 04 _ DAB _ Gesamtkatalog _ es . pdf](https://www.dab-europe.com/fileadmin/PublicData/Downloads/2016_04_dab_Gesamtkatalog_ES.pdf)(2016)

[4]费斯蒂沃，[https://getfestivo.com/documentation](https://getfestivo.com/documentation)(2021)