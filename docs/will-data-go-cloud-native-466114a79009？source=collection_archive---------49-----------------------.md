# 数据会成为云原生的吗？

> 原文：<https://towardsdatascience.com/will-data-go-cloud-native-466114a79009?source=collection_archive---------49----------------------->

## 数据专业人员使用的工具和平台越来越多地运行在云原生技术上。

![](img/c4c8282ed7ca53a420017feccf2d35cb.png)

*照片由*[*Jonny Gios*](https://unsplash.com/@supergios)*在 Unsplash 上*

数据工具仍然是一个极其活跃的空间，这对于作为一个懒惰用户的我来说是非常令人兴奋的。别信我的话，你可以看看马特图尔克的精彩帖子: [*韧性与活力:2020 年数据& AI 景观*](https://mattturck.com/data2020/) 。

如果你深入挖掘 Matt 文章中的大量信息图表，你会发现许多列出的技术、工具和公司都是云原生的。这一趋势的惊人之处在于，它发生在现代数据基础架构堆栈[的上上下下](https://a16z.com/2020/10/15/the-emerging-architectures-for-modern-data-infrastructure/) —从接收到存储，再到处理和预测。

不过，我想知道的一件事是，云原生数据工具是否会成为主导。

# 什么是云原生？

出于各种各样的原因，人们开始使用术语“原生云”来表示两种完全不同的东西。

1.  “原生云”意味着使用“容器、服务网格、微服务、不可变基础设施和声明性 API”，正如 CNCF 所描述的那样。
2.  “云原生”意味着使用云服务提供商的“原生”工具。在这种用法中，我们谈论的是紧密集成的特定于提供商的工具，例如与 AWS [控制塔](https://aws.amazon.com/controltower/)、[配置](https://aws.amazon.com/config/)或[并行集群](https://aws.amazon.com/hpc/parallelcluster/)。

不幸的是，这个词有两种几乎相反的用法，但我对此无能为力。(实际上，我认为在某些情况下，可能在无服务器领域的某个地方，您最终会实现这个术语的两种含义，所以它们并不完全相反。)无论如何，对于本文，我将使用第一种用法。

# 云原生数据工具

让我们面对现实:数据工具运行在原生云上。

[Spark 现在的目标是在 Kubernetes 上部署](https://spark.apache.org/docs/latest/running-on-kubernetes.html)。这本身就是巨大的。此外，AWS 最近为他们的托管 Spark 集群推出了[功能，让你可以在他们的托管 Kubernetes 集群](https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/emr-eks.html)上运行它。(我会注册在 Kubernetes 上运行 production Spark 工作负载吗？我不确定。)

[Jupyterhub](https://zero-to-jupyterhub.readthedocs.io/en/stable/) 、 [RStudio](https://solutions.rstudio.com/launcher/kubernetes/) 和 [Kubeflow](https://www.kubeflow.org/) 都是使用 Kubernetes 的数据基础设施即软件的例子。这些是值得尊敬的增值工具，使用他们已经熟悉的工具为数据科学家提供一致的用户体验(Kubeflow 添加了一些新东西，但也嵌入了 Jupyter)。这个空间还远未尘埃落定: [AWS Sagemaker](https://aws.amazon.com/sagemaker/) 、 [Azure ML Studio](https://azure.microsoft.com/en-us/services/machine-learning/) 、 [Google AI 平台](https://cloud.google.com/ai-platform)都是强势的祭品，有时候[会和](https://docs.aws.amazon.com/sagemaker/latest/dg/amazon-sagemaker-operators-for-kubernetes.html)云原生轻度重叠。“开放核心”的后起之秀 Databricks 正在努力推动 IPO。现在 Spark 在 Kubernetes 上运行，Databricks 会开发一个[掌舵图](https://helm.sh/)吗？

最近[发布了 2.0](https://airflow.apache.org/blog/airflow-two-point-oh-is-here/) 的流行数据管道工具 Airflow 运行在 [Kubernetes](https://airflow.apache.org/docs/apache-airflow/stable/kubernetes.html) 上。Kafka，[大型流媒体数据平台](https://vicki.substack.com/p/you-dont-need-kafka)，正在[简化其架构](https://cwiki.apache.org/confluence/display/KAFKA/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum)，而 [Strimzi 项目](https://strimzi.io/)正在简化 Kafka 在 Kubernetes 上的部署。(Strimzi 目前是一个 [CNCF 沙盒](https://www.cncf.io/sandbox-projects/)项目，这意味着你可能要过一段时间才能告别你的专用生产 Kafka 集群。)人们试图将生产数据库放在 Kubernetes 上(我有一些问题)。地理空间数据市场的赢家[ESRI](https://www.esri.com/about/newsroom/announcements/independent-report-highlights-esri-as-leader-in-global-gis-market/#:~:text=%22Esri%20is%2C%20without%20a%20doubt,develop%20Esri%20industry-specific%20solutions)[将在其旗舰产品](https://www.esri.com/arcgis-blog/products/arcgis-enterprise/announcements/arcgis-enterprise-qa-highlights-from-uc-2020/) [ArcGIS](https://www.arcgis.com/index.html) 中添加对 Kubernetes 的支持。[特征库](https://eugeneyan.com/writing/feature-stores/)，一种用于 MLOps 的多工作流数据库[运行在 Kubernetes](https://feast.dev/) 上。

这些技术不会在一夜之间全部融入 Kubernetes，cloud native 也没有取得任何胜利，但显然有一些势头。

# 不要惊慌

尽管很复杂，但我倾向于认为 Kubernetes 扩展到数据基础设施只是 Kubernetes 扩展到*一切*的一个功能。[它正向边缘](https://kubeedge.io/en/)移动。这是[进入 IaaS](https://cluster-api.sigs.k8s.io/) 。显然，你可以在上面运行区块链。[银行都在用](https://kubernetes.io/case-studies/ing/)。

我一般不知道这将如何结束，但我完全期待它保持令人兴奋和充满活力。这些问题处在极其活跃的社区(数据、ML、云原生、无服务器、开源)、各种形状和大小的供应商以及大量投资的交叉点上。在我看来，这就像是有趣时光的公式。

除非你讨厌钱，否则你绝对不应该跑出去给你团队中的每个数据科学家买一本[云原生基础设施书籍](https://www.cnibook.info/)和一个 Kubernetes 训练营。如果你是一名数据专业人士，阅读这篇文章并考虑“学习 Kubernetes”，这是一件非常好的事情。多样化可能是件好事。[只要意识到你正在让自己陷入什么样的境地](https://landscape.cncf.io/)。

*原载于*[*https://theslipbox.substack.com*](https://theslipbox.substack.com/p/will-data-go-cloud-native)*。*