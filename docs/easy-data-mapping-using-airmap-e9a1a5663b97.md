# 使用 AirMap 轻松绘制数据

> 原文：<https://towardsdatascience.com/easy-data-mapping-using-airmap-e9a1a5663b97?source=collection_archive---------10----------------------->

## AirMap 是一个由 Airtable 支持的数据映射器

![](img/ef4e887f67f9f8a3d5a3448e26991f6f.png)

图片由 Pixabay 提供

轻松跟踪数据字典、映射、源、列和验证。 [AirMap](https://github.com/eyan02/AirMap) 是一个由 Airtable 支持的数据映射器。查看 [Github repo](https://github.com/eyan02/AirMap) 了解更多信息。

AirMap 是一个数据 ETL 工具，旨在使用云托管的文档表来帮助管理数据流。这允许我们使用一个集中的“真实来源”来获取我们需要了解的关于数据的任何信息(需求、来源、验证等)。AirMap 利用我们在 Airtable 中创建的文档表来处理数据映射、数据源合并以及这两者之间的一切，因此您不必。无需不断更新数据管道来满足新的数据集要求。只需更新 Airtable 中的主映射，让 AirMap 完成剩下的工作。

## AirMap 的优势:

*   Airtable 将您需要的所有数据信息集中在一个地方。
*   更新 Airtable 中的主数据映射，AirMap 将自动更新您的管道，以匹配您在云中拥有的内容！
*   无需来回发送静态文档，通过 Airtable 易于使用的 web/桌面 GUI 轻松与其他人协作。
*   快速管理、控制和共享您的数据映射、验证和数据字典。

*想自己试试 AirMap 吗？你可以在这里* *下载一份 demo* [*。*](https://github.com/eyan02/AirMap/tree/main/test/demo)

## 将所有数据源放在一个地方

我们可以使用 Airtable 来跟踪我们所有的数据源。提供数据集及其来源的集中文档。

AirMap 使用此信息根据您在 Airtable 中概述的要求来验证管道中的数据。

## 轻松跟踪和更新数据需求

Airtable 类似数据库的结构允许我们在数据源和它们对应的列之间创建链接。

这意味着我们可以确保使用正确的数据细节、验证和/或需求来操作我们的数据。每条记录都是独一无二的，这使得 AirMap 避免了代价高昂的人为错误。

**快速设计和部署数据映射**

Airtable 类似 excel 的 GUI 允许用户轻松地更新和创建数据映射。当传入(或传出)数据集需求不断变化时，这尤其有用。

点击下面的链接查看完整的 Airtable 库:

<https://airtable.com/embed/shr9Tr2wp5rs2Bm7a?backgroundColor=red>  

# 如何使用 AirMap

AirMap 很容易集成到现有的数据管道结构中。像往常一样聚合数据源，然后将数据传递给 AirMap。

你可以尝试使用我在这个 [Github repo](https://github.com/eyan02/AirMap/tree/main/test/demo) 中提供的样本数据和 Jupyter 笔记本来测试 AirMap。

AirMap 读取您在 Airtable 中创建的映射结构，以生成预期的数据集输出格式。

从 Airtable 中检索指定的数据映射，并转换输入数据以生成映射的输出

来自 AirMap 的结果数据

您还可以直接在 Python 环境中轻松检查您的数据映射需求。这使我们能够仔细检查我们的数据源，验证需求，并确保我们通过 AirMap 传递正确的数据集。

查询 Airtable 以获取所选数据映射的映射详细信息

“项目 A —客户总结”数据图的结果数据图详情

AirMap 的当前版本是一个概念验证。将添加数据验证和(潜在的)数据清理的附加功能。感谢阅读！