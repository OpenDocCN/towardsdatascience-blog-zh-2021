# AWS EMR 中的 RStudio 服务器简单、可重复且快速

> 原文：<https://towardsdatascience.com/rstudio-server-in-aws-emr-easy-reproducible-and-fast-f066aa6135d?source=collection_archive---------33----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

![](img/030ea8ec802d95f31466c03ab4f78ee3.png)

由[马克·佩津](https://unsplash.com/@fennings?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在我之前写的一篇文章[(可以在这里找到)](/data-science-meets-infrastructure-as-code-d2f62328646c?source=your_stories_page-------------------------------------)中，我分享了我对使用基础设施作为代码工具的看法，如 Packer 和 Terraform，以创建 Amazon 机器映像(AMI)，为数据科学家和数据分析师创建一个可复制的环境，以利用机器中预装的许多工具(Jupyterhub 和 RStudio Server)探索云资源。

我不会在这篇文章中扩展自己，因为已经讨论过了，但是与另一篇博客文章相关的存储库使用 Ansible playbooks 来安装所需的包和库，以便使用 R 和 Python 来开发分析和模型，然而，我探索创建自定义 ami 的可能性的主要原因是在 AWS EMR 集群的部署中使用它们作为基础映像。

# 推理

许多过去部署过 EMR 集群的人已经知道，集群的部署可以与引导脚本相关联，引导脚本将提供数据科学家、分析师或工程师在工作中使用的必要工具，尽管这是一个重要的解决方案，但它增加了服务器部署的时间代价(与集群初始化相关联的包和库越多，集群部署的时间就越长)。

EMR 的默认部署允许配置 Jupyter 笔记本或 Zeppelin 笔记本以及集群的初始化，但是，R 和 RStudio 用户落后了，因为这两种解决方案在 EMR 集群中都不可用，所以我觉得有必要探索其他既可扩展又快速的解决方案，允许每个分析师/科学家/工程师部署他们自己的集群，以最适合他们需求的语言完成他们的工作，然后在完成后关闭集群。

经过一番挖掘，我发现在 EMR 5.7(以及更高版本)发布后，可以使用定制的 Amazon 机器映像来部署集群，只需遵循一些建议和最佳实践，如这里建议的[这里的](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-custom-ami.html)，比如为高于 EMR 5.30 和 6.x 的 EMR 版本使用 Amazon Linux 2。 使用 Packer 和 Ansible 创建一个定制的 Amazon 机器映像(以确保 RStudio 服务器的安装和配置)，然后使用这个映像部署一个 EMR 集群，再做一些配置以确保 RStudio 用户能够访问 Spark 环境、Hadoop 和其他通过使用 EMR 集群可用的特性。

# 操作方法

和我的另一篇博文一样，这篇博文与一个[库](https://github.com/paeselhz/emr-spark-client)相关联，它帮助用户:

*   使用 Terraform 和自定义 AMI 部署 EMR 集群
*   配置默认的 RStudio 服务器用户来访问 Hadoop 文件系统和 Spark 环境
*   将 AWS Glue Metastore 与 Spark 集群一起使用，以便 EMR 集群可以访问 Glue 中已经存在的数据目录

请注意，这个部署是以 AWS 为中心的，我现在没有在其他云中开发这个相同项目的计划，尽管我知道通过使用 Google 云平台和 Azure 上可用的工具这是可能的。

# 使用 Terraform 部署 EMR 集群

在项目[仓库](https://github.com/paeselhz/emr-spark-client)中，我们有一个堆栈来使用模块化的 Terraform 脚本部署 EMR 集群，因此任何满足您的基础设施需求的必要调整(如创建密钥或角色许可)都可以单独实现。到目前为止，Terraform 希望变量如下所述，以允许创建集群主服务器和核心节点。

该实现还支持在主节点和核心节点中使用现场实例，但是，在任何生产环境中，建议至少将主节点部署为按需实例，因为如果主节点关闭，整个集群将被终止。

我的地形配置中可用的变量如下:

```
# EMR general configurations
name = "" *# Name of the EMR Cluster*
region = "us-east-1" *# Region of the cluster, must be the same region that the AMI was built*
key_name = "" *# The name of the key pair that can be used to SSH into the cluster*
ingress_cidr_blocks = "" *# Your IP address to connect to the cluster*
release_label = "emr-6.1.0" *# The release of EMR of your choice*
applications = ["Hadoop", "Spark", "Hive"] *# The applications to be available as the cluster starts up*

# Master node configurations
master_instance_type = "m5.xlarge" *# EC2 instance type of the master node* *The underlying architecture of the machine must be compatible with the one used to build the custom AMI*
master_ebs_size = "50" *# Size in GiB of the EBS disk allocated to master instance*
master_ami = ""  *# ID of the AMI created with the custom installation of R and RStudio*
# If the user chooses to set a bid price, it will implicitly create a SPOT Request
*# If left empty, it will default to On-Demand instances*master_bid_price = ""

# Core nodes configurations
core_instance_type = "m5.xlarge" *# EC2 instance type of each core node* *The underlying architecture of the machine must be compatible with the one used to build the custom AMI*
core_ebs_size = "50" *# Size in GiB of the EBS disk allocated to each core instance*
core_instance_count = 1 *# Number of core instances that the cluster can scale*
# If the user chooses to set a bid price, it will implicitly create a SPOT Request
*# If left empty, it will default to On-Demand instances*core_bid_price = "0.10"
```

这些配置应该足以让您启动并运行一个定制的 EMR 集群。

# 允许 AWS EMR 中的自定义端口

自 2020 年 12 月以来，增加了一些增强安全性的更改，要求用户手动允许端口公开可用，可在此处找到。如果不是这种情况，您可以跳到下一个会话，但是，如果您希望通过 internet 公开您的 RStudio 服务器实例，您必须遵循下述步骤:

*   进入 [AWS 控制台> EMR](https://console.aws.amazon.com/elasticmapreduce/home?region=us-east-1#block-public-access:)
*   选择阻止公共访问(应被激活)
*   编辑端口间隔，以允许 RStudio 服务器公开使用端口 8787

如果您希望部署只对少数 ip 地址可用的自定义服务器，您可以跳过这一步，将 ingress_cidr_blocks 设置为您的个人 IP 地址。

# 配置 RStudio 服务器以使用全部 AWS EMR 资源

下面描述的步骤已经嵌入到部署集群的 Terraform 脚本中，如果有人希望对其进行调整和定制，这里只是为了提供信息。

在部署服务器之后，添加了配置环境变量的进一步步骤，这使得 RStudio 更容易找到必要的文件，以便与 Spark、Hive、Hadoop 和集群中可用的其他工具一起顺利运行。此外，该步骤将 RStudio 用户添加到 Hadoop 组，并允许它修改 HDFS。

**这个步骤可以在这个项目中找到。**

# AWS 将 metastore 作为 JSON 配置粘附在 Terraform 模块中

随着现成工具(如 AWS Glue)的出现，允许以一种简单的方式转换数据和存储数据及其元数据，需要将 EMR 集群与该堆栈集成。这也已经在 Terraform 堆栈中完成，因此如果用户希望使用 Hive Metastore 而不是托管 AWS Glue Metastore，则需要删除这部分代码。

这个步骤由 EMR main Terraform 中的 configurations_json 完成，它接收关于应该如何配置集群的多个输入，更改 spark-defaults(例如，允许不同的 SQL 目录实现)，配置单元站点配置使用 AWS Glue Metastore 作为默认配置单元站点，等等。要配置的可能性列表可在[这里](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-configure-apps.html)找到。

# 结束语

该项目的部署旨在缩小数据科学相关堆栈的供应之间的差距，试图确保无论数据科学家/分析师/工程师打算使用什么工具，他们都有机会使用它。在部署结束时，人们应该能够在不到 10 分钟的时间内访问功能完整的 EMR 集群，在 EMR 的主节点上安装和配置 R 和 RStudio 服务器。

任何与这个项目相关的问题，包括建议，都可以添加到 Github 资源库中。