# 云中的 MongoDB:2021 年的三种解决方案

> 原文：<https://towardsdatascience.com/mongodb-in-the-cloud-three-solutions-for-2021-ee87c4ef4ef?source=collection_archive---------29----------------------->

## MongoDB Atlas、AWS DocumentDB、Azure Cosmos DB 的定价和兼容性概述。

作者:[爱德华·克鲁格](https://www.linkedin.com/in/edkrueger/)和[道格拉斯·富兰克林](https://www.linkedin.com/in/douglas-franklin-1a3a2aa3/)。

![](img/b59d844d543ab17392679b4c4e0285c7.png)

照片由[约尔·温克勒](https://unsplash.com/@yoel100?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mango?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*本文将调查 MongoDB 的* MongoDB Atlas *、AWS Amazon DocumentDB 和 Azure Cosmos DB 提供的成本和功能。我们假设我们不是在谈论超大规模的企业解决方案。我们正在寻找一个更典型的应用程序。*

与此相关的一个原因是，MongoDB 公司决定在今年晚些时候终止 mLab 服务。这是为了将用户转移到该公司较新的平台 MongoDB Atlas。然而，今天有许多 MongoDB 的云提供商。我们将讨论 MongoDB Atlas、Azure Cosmos DB 和 AWS DocumentDB。

# 什么是 MongoDB？

**MongoDB** 是一个面向文档的数据库，它将数据存储为 [BSON 对象](http://bsonspec.org/)或*文档*组成的*集合。*这些文档可以作为 JSON 的文档进行检索。这意味着您可以不使用 RDBMS 中的列和行结构来存储记录。

MongoDB 云数据库是内容管理、产品目录、地理空间数据应用程序以及任何具有快速变化的数据需求的项目的绝佳解决方案。

# 云提供商

有许多可用的 MongoDB 云解决方案。开发团队或公司最终决定采用什么解决方案，很可能取决于特性、价格以及与您现有架构的集成。

这里讨论的所有选项都是托管云服务。这意味着现代数据库中所需的所有数据库管理和安全任务都将由服务提供商来处理。忽略这些解决方案的共同点，让我们分解一些差异。

# MongoDB 地图集

MongoDB 成立于 2007 年，面临着获得更多用户和赚更多钱的压力。该公司在 2016 年建立了 Atlas，在 2018 年以 6800 万美元收购了 mLab，然后在 2020 年底弃用了 mLab。

MongoDB Atlas 是 MongoDB 公司提供的解决方案。因此，MongoDB Atlas 是最完整的 MongoDB 解决方案。使用 MongoDB Atlas，您将可以访问所有的 MongoDB 特性和方法，包括 **map-reduce** 。此外，您将能够运行**4.4 版、4.0 版或 3.6 版 API。**

亚多拉斯数据库的另一个好处是，你可以灵活地使用其他云服务提供商(AWS、Azure 和 GCP)。我们可以很容易地将我们的应用程序转换成运行在 GCP MongoDB Atlas 上的应用程序。

**MongoDB Atlas 确实有一个免费层。**然而，**如果您需要更强大的计算能力，10GB 存储、2GB ram 和 1 个 vCPU 的价格约为每月 57 美元。**

*有关设置* MongoDB Atlas *或迁移到* MongoDB Atlas *的更多信息，请查看本文。*

[](/mongodb-migrating-from-mlab-to-atlas-1070d06fddca) [## MongoDB:从 mLab 迁移到 Atlas

### 迁移您的 MongoDB 以保持 Heroku 应用程序正常工作

towardsdatascience.com](/mongodb-migrating-from-mlab-to-atlas-1070d06fddca) 

# 天蓝色宇宙数据库

Azure Cosmos DB 是微软针对文档数据库的 Azure 解决方案。Azure Cosmos DB 使用*ru 或资源单位，*使得定价比其他云平台稍微不透明。但是，您可以使用 RU 计算器来转换成美元并估计成本。

这里有一个 Azure DB 月度[成本计算器](https://cosmos.azure.com/capacitycalculator/)。

**Azure 确实有一个免费层**，授予帐户中的前 400 RU/s 和 5 GB 存储空间。

Azure Cosmos DB free tier 让您可以轻松开始、开发和测试您的应用程序，甚至免费运行小型生产工作负载。免费层拥有普通 Azure Cosmos DB 帐户的所有[好处和功能](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction#key-benefits)。 **Cosmos DB 使用 MongoDB v3.6 API，不支持 map-reduce。**

在你的免费积分用完之后， **Cosmos DB 费用**比竞争对手低。对于使用最少的 1.5 GB ram 的 10GB 数据，每月大约需要 25 美元。

此外，Azure 现在提供了一个无服务器选项。这对于使用不一致或使用率低的应用程序非常有用，如内部业务应用程序或概念证明。在无服务器模式下使用 Azure Cosmos DB 没有最低费用。[Azure Cosmos DB server less](https://docs.microsoft.com/en-us/azure/cosmos-db/serverless)只对数据库操作消耗的 ru 和数据消耗的存储进行计费。这可以大幅降低不常访问或不定期访问的应用程序的成本。

也就是说，对于常规的数据加载，无服务器不是一个好的解决方案。无服务器确实提供了自动扩展；然而，成本可能很高。

关于设置 Azure Cosmos DB 或迁移到 Cosmos DB 的更多信息，请查看本文。

[](/mongodb-migrating-from-mlab-to-azure-cosmos-db-88c508f72d24) [## MongoDB:从 mLab 迁移到 Azure Cosmos DB

### 如何将您的 mLab 数据库迁移到 Azure Cosmos DB 以保持您的应用程序运行。

towardsdatascience.com](/mongodb-migrating-from-mlab-to-azure-cosmos-db-88c508f72d24) 

# AWS 文档 b

亚马逊网络服务解决方案是亚马逊文档数据库。 **AWS DocumentDB 没有空闲层。**目前最便宜的实例是 *db t3.medium* ，它有 **10GB 的存储空间，配有 4GB RAM 和 2 个 vCPUs，价格大约是每月 60 美元**。请注意，这大约是 Azure 和 Atlas 提供的 RAM 的两倍。

有了 AWS DocumentDB，您可以选择使用 **v3.6 或 v4.o API** 。该平台缺少 Atlas 中可用的地图缩小功能。供应商锁定是 AWS 需要记住的另一件事。如果您的公司已经在其他云提供商上使用 AWS 服务，这个解决方案可以很好地协同工作。

供应商锁定并非不可避免，但这是额外的工作和费用。例如，要在 Heroku 应用程序中使用 AWS DocumentDB，您需要获得 Heroku enterprise，这很贵。

Amazon DocumentDB 是一项仅限 VPC 的服务，不支持公共端点。因此，您不能从 AWS 之外的环境直接连接到 Amazon DocumentDB 集群。但是，您可以使用 SSH 隧道从 AWS 外部的机器进行连接。

## 结论

团队需要考虑功能、定价和现有架构，以选择最佳的云 MongoDB 解决方案。有些决定会很容易；比如需要 map-reduce，选择 MongoDB Atlas。如果你打算做一个低使用率的应用，使用 Azure Cosmos Serverless 来节省成本。

对于可预测的负载，Azure serverless 可能很粗糙。人们经常选择无服务器选项，因为它们非常容易扩展。这个问题是大规模无服务器扩展的成本会高得出乎意料。但是，如果每一次扩展都意味着额外的收入，那么可变成本就对应于可变收入。例如，考虑一个黑色星期五的网上商店。无法通过自动扩展来容纳额外的流量对于收入损失来说是一场灾难。

如果你的应用是托管在 Heroku(不是企业)上，选择 MongoDB Atlas 或者 Azure Cosmos。使用 MongoDB Atlas，您可以选择任何您喜欢的云提供商，并使用最新的 MongoDB API 功能。这种灵活性是 Amazon Document DB 或 Azure Cosmos DB 所不具备的。这些选项将您锁定在各自的云供应商，同时运行旧版本的 DB API。

如果您的公司有一些使用 AWS 的现有项目，或者您喜欢该平台，AWS DocumentDB 可能是一个很好的解决方案。请记住与其他云服务的兼容性问题。

AWS Amazon Document DB 和 Azure Cosmons 支持创建应用程序所需的大部分功能。我们注意到唯一缺少的特性是 map-reduce 方法。所以如果你是用 MongoDB 来使用这个特性，Document DB 或者 Cosmos 对你来说可能不是一个好的解决方案；Atlas 将更好地满足您的需求。