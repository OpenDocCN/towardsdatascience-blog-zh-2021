# 在 AWS 中处理地理空间数据

> 原文：<https://towardsdatascience.com/handling-geospatial-data-in-aws-a82ae364f80c?source=collection_archive---------13----------------------->

![](img/f8f8355a58a490ad1339a64f391fee7d.png)

美国宇航局在 [Unsplash](https://unsplash.com/s/photos/earth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 数据工程

## 简要介绍如何使用 AWS 存储、处理、分析和可视化地理空间数据

随着数据库、仓库、分析和可视化工具扩展其处理地理空间数据的能力，我们比以往任何时候都更有能力捕捉和使用 GIS 数据。虽然单个产品支持 GIS，但云提供商并没有在早期提供完整的 GIS 解决方案。随着时间的推移，这种情况已经发生了很大变化。像 AWS 这样的云提供商在任何需要的地方都提供支持。对于像 AWS 这样的软件公司来说，我们需要理解他们构建的产品的选择直接受到通常是他们的高收入客户的需求的影响。

现在，许多公司看到了对地理空间数据的需求，并且这些公司能够以合理的低成本获取和存储地理空间数据(因为位置数据无处不在，存储变得越来越便宜)，我们已经看到地理空间数据库、分析和可视化工具被更广泛地采用。虽然 AWS 没有一个成熟的地理空间数据解决方案，但它确实在不同的服务中提供了一些很好的功能。在这篇文章中，我们将对此进行一些讨论。

## 关系数据库

AWS 有两个托管关系数据库服务— RDS 和 Aurora。很明显，您可以使用 EC2 安装自己的关系数据库，但这不是一个托管解决方案，所以我们在这里不做讨论。地理空间数据的高效存储对于我们今后想用这些数据做的任何事情都非常重要。

AWS RDS 只支持它正在运行的任何版本的关系数据库服务，因此对 GIS 的支持取决于数据库的版本。例如，如果您运行 MySQL 的 RDS，那么您将拥有该 MySQL 版本支持的 PostGIS 特性。如果您运行 PostgreSQL，[您将需要在 RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.PostgreSQL.CommonDBATasks.html#Appendix.PostgreSQL.CommonDBATasks.PostGIS) 上运行的 PostgreSQL 上安装 PostGIS 扩展。

在 AWS RDS PostgreSQL 上安装 PostGIS 扩展的示例脚本。

另一方面，使用 Aurora，它是 AWS 为不同的关系数据库编写的一个引擎，您可以在最初版本的关系数据库所支持的通常特性的基础上获得一些额外的特性、改进和优化。例如，阅读[Aurora 如何通过使用 Z 顺序曲线](https://aws.amazon.com/blogs/database/amazon-aurora-under-the-hood-indexing-geospatial-data-using-z-order-curves/)优化地理空间索引。

## 仓库、湖泊和可视化

红移虽然基于红移，但最长时间没有支持地理空间数据，终于在 2019 年推出了对它的支持。目前，在 Redshift 中，我们可以使用 find 和使用原生 GIS 数据类型。这里是 GIS 数据红移支持的[函数的完整列表](https://docs.aws.amazon.com/redshift/latest/dg/geospatial-functions.html)。

<https://aws.amazon.com/blogs/aws/using-spatial-data-with-amazon-redshift/>  

这是针对红移的，但是当你把地理定位数据存储在 S3 时会发生什么呢？你如何质疑这一点？假设您已经以 GeoJSON 格式存储了地理位置数据，那么您应该能够使用 nature GIS 类型在 Athena 中创建一个外部表，并使用 Athena 查询数据。更多细节见[下例](https://docs.aws.amazon.com/athena/latest/ug/geospatial-example-queries.html)。

对地理空间数据使用 Lambda & Step 函数的完整数据湖解决方案。

还有一个关于可视化的快速说明——通过 AWS，您可以使用 Quicksight 使用 Athena 和其他地方的数据创建地理/地理空间图表。虽然不如 Looker 和 Tableau 等成熟的可视化工具丰富和强大，但在需要简单的可视化时，Quicksight 确实很有用。

像 SNS，SQS 这样的服务不关心他们收到的是什么样的数据，只要是规定格式的。这意味着如果有一个地理位置数据的生产者，那么它可以被 AWS 中的事件/流服务消费。话虽如此，AWS 物联网事件使处理地理位置数据(一般来说是物联网事件)变得非常容易。

## 亚马逊定位服务

我不知道这一说法是否适合亚马逊定位服务(ALS)，该服务将在不损害数据隐私和安全的情况下，与谷歌地图等公司竞争，因为将有数据永远不会离开 AWS 网络的规定，尽管这可能会产生额外的成本。

<https://techcrunch.com/2020/12/16/aws-launches-amazon-location-a-new-mapping-service-for-developers/>  

我们还没有看到这项服务有多成熟。它也将提供地址转换、细化和规范化吗？几天前，我遇到了一个叫 Placekey 的神奇服务，它试图以极高的准确性解决地址规范化的问题。我们将拭目以待亚马逊是否有锦囊妙计。目前，亚马逊位置服务正在预览中。

<https://aws.amazon.com/blogs/aws/amazon-location-add-maps-and-location-awareness-to-your-applications/>  

## 结论

随着亚马逊定位服务已经进入预览版，AWS 是否会更加专注于获取更多地理定位数据特定服务，也许是专门的地理定位数据库或可视化引擎？在过去的几年里，AWS 在时间序列、图形、文档数据库市场上占据了一定的份额。更多的地理定位服务可能真的会出现。