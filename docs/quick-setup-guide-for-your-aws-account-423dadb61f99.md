# AWS 帐户的快速设置指南

> 原文：<https://towardsdatascience.com/quick-setup-guide-for-your-aws-account-423dadb61f99?source=collection_archive---------33----------------------->

## 现在就开始在云中构建 ML 和数据解决方案

![](img/1003369a3718124a99083fa8922869b0.png)

Rafael Garcin 在 [Unsplash](https://unsplash.com/s/photos/cloud?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

云技术现在在行业中无处不在，被各行各业的技术专家使用，而不仅仅是 DevOps 工程师。越来越多的数据科学家和机器学习工程师正在云上运行工作负载，以训练模型和托管端点来进行实时推理。

有几个云提供商，但 [AWS(亚马逊网络服务)是领导者，控制着超过 31%的市场](https://www.parkmycloud.com/blog/aws-vs-azure-vs-google-cloud-market-share/#:~:text=AWS%20has%2031%25%20of%20the,%25%2C%20Alibaba%20Cloud%20close%20behind.)——几乎是 Azure、谷歌云和阿里云的总和。由于亚马逊的主导地位，与其他任何云提供商相比，更多的技术职位将 AWS 列为必备或首选技能。

# 开始

本指南中的说明旨在帮助您快速启动并运行 AWS。这不是一个设置 AWS 帐户的全面教程，而是一个帮助您现在就开始在云中开发的简单指南。

我们将介绍如何:

1.  创建一个 IAM 用户。
2.  为特定服务创建 IAM 角色。
3.  安装和配置 AWS CLI。
4.  创建一个 S3 桶。
5.  请求服务限制增加。

# 1.创建管理员 IAM 用户

IAM(或身份和访问管理)是 AWS 系统，用于管理各种 AWS 资源的访问和权限。首先，您需要一个管理员用户:

1.  登录或注册 [AWS 账户](https://aws.amazon.com/console/)。
2.  搜索并选择 **IAM** 以创建管理员用户。在**访问管理**下，点击**用户**然后**添加用户。**填写**用户名**字段，选择**程序化访问**和 **AWS 管理控制台访问**，然后点击**下一步:权限**。
3.  选择**创建组**，填写**组名**字段，选择**管理员访问**策略。
4.  点击**下一步:标签**，然后**下一步:审核**，最后**创建用户**。
5.  点击**下载。csv** 按钮，并将凭证保存在安全的地方。当我们稍后配置 AWS CLI 时，您将需要这些。

# 2.创建 IAM 角色

IAM 角色可以由授权用户担任，并允许该用户执行由附加到该角色的策略指定的任务。对于这个例子，我们将创建一个 SageMaker 执行角色，它将允许我们的用户执行各种 SageMaker 任务。

1.  在控制台中搜索并选择 **IAM** 。在**权限管理**下，点击**角色**，然后**创建角色**。
2.  遵循指南，选择您想要为其创建角色的服务。在本例中，我们将选择 **SageMaker** 。点击**下一步:权限**然后**下一步:审核**。填写**角色名称**字段，点击**创建角色**。
3.  注意屏幕顶部附近的**角色 ARN** 字段。这是您将用于以编程方式承担此角色的值。
4.  如果该角色需要额外的策略，您可以搜索**角色名称**。选择您的角色并点击**附加策略**。对于本例，让我们通过检查并单击**附加策略**来搜索并附加 **AmazonS3FullAccess** 。

# 3.安装和配置 AWS CLI

AWS CLI(或命令行界面)将允许您从本地机器或任何安装机器的地方以编程方式运行 AWS 工作负载。这里，我们将介绍如何安装和配置 AWS CLI 版本 2。*注意:版本 1 也广泛使用，因此请注意您遵循的任何说明中使用的版本，以确保一切顺利运行。*

1.  按照此处的说明[安装 AWS CLI 版本 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) 。
2.  安装完成后，在您的终端上运行`$ aws configure`。您将需要在 **1 中生成的凭证文件。创建管理员 IAM 用户。**填写您的 **AWS 访问密钥 ID** 和 **AWS 秘密访问密钥**。设置您的**默认地区**(我使用 us-east-1，但您可以使用您帐户可用的任何地区)。点击回车键，将 json 作为您的默认输出格式。

# 4.创建 S3 存储桶

S3(简单存储服务)AWS 解决方案简单地在云中存储数据——顾名思义。设置一个存储数据的桶很简单——在控制台搜索栏中搜索 **S3** ，然后点击**创建桶**。对于一个基本的私有 bucket，只需指定一个名称，其他的都使用默认值。

# 5.创建一个 EC2 密钥对文件

密钥对文件允许您使用私钥保护 EC2(弹性云计算)实例。您可以使用此凭据，而不是使用密码来访问您的实例。跟随我的逐步指导[到这里](https://medium.com/@brentlemieux/create-a-key-pair-file-for-aws-ec2-b71c6badb16)。

# 包扎

感谢您的关注！[请务必关注我，并查看我关于在云中构建数据和机器学习产品的其他教程](https://medium.com/@brentlemieux)。如果你有兴趣和我打招呼，请在 LinkedIn 上联系我。

# 接下来阅读

了解如何使用以下工具在 AWS 上构建数据管道:

*   [在 AWS EMR 上开始使用 py spark](/getting-started-with-pyspark-on-amazon-emr-c85154b6b921)
*   [用 Spark 处理生产数据](/production-data-processing-with-apache-spark-96a58dfd3fe7)