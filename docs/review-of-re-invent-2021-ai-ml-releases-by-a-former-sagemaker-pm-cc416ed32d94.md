# 一位前 SageMaker 项目经理关于 re:Invent 2021 AI/ML 发布的评论

> 原文：<https://towardsdatascience.com/review-of-re-invent-2021-ai-ml-releases-by-a-former-sagemaker-pm-cc416ed32d94?source=collection_archive---------16----------------------->

## 意见

## AWS 对所有新 MLOps 工具的深入分析

每年 12 月，亚马逊网络服务(AWS)都会为客户发布新的机器学习(ML)功能。这是拉斯维加斯的一项激动人心的活动，门票通常会销售一空。有很多新闻，我将为数据科学社区提炼出来。

![](img/d7e09a23d7afa3556ca61638a2229fd8.png)

来自 Unsplash 的会议图像

我之前在《走向数据科学》上写过[云 MLOps 平台](/comparing-cloud-mlops-platform-from-a-former-aws-sagemaker-pm-115ced28239b)，目前我正在创建一家应用 ML stealth 初创公司。我之前是 AWS SageMaker 的高级产品经理，脸书的计算机视觉(AR/VR)数据、工具和运营主管，以及投资新兴市场债券的 applied ML 对冲基金的创始人。欢迎[发微博](https://twitter.com/alxchung)或[在 LinkedIn 上给我发信息](https://www.linkedin.com/in/alex-chung-gsd/)你的评论。

在过去的两年里，我观察到有一个简单的框架来应用 ML 产品:应用 AI/ML 服务、MLOps 平台和 ML 框架。最底层是 ***框架*** ，跨越 ML 库(PyTorch、Jax、XGBoost)到编译器和计算芯片(GPU、ARM、TPU、ASICs)。中间是 ***MLOps 平台 SDK***像训练、推理系统、元数据管理、数据处理、工作流引擎和笔记本环境。最后在最顶层的是 ***AI/ML 服务*** ，它抽象了下面的层。AWS 很乐意通过 understand of Textract 向您销售人工智能服务，但我采访的几乎每个企业都喜欢拥有自己的 ML 团队来管理复杂性并将正确的工具粘合在一起。我的框架分解实际上在一个标准的 AWS AI/ML 营销幻灯片中使用。

![](img/931ebb4f533890b9cfbd286a74555396.png)

图片来自 Unsplash

SageMaker 产品从底层框架到 AI 服务都有。他们的产品定位旨在成为万金油，这在 [Gartner 魔力象限](https://virtualizationreview.com/articles/2021/03/15/gartner-cloud-aidev.aspx)中得分很高，但仍受到首席信息官和 ML 团队经理对功效的广泛争论。然而，在 2021 re:Invent 上，SageMaker 模糊了 MLOps 平台和人工智能服务之间的界限，同时还在框架上发布了新的深度学习工具。

毫无疑问，SageMaker 今年非常重视深度学习(DL)能力。早在 7 月份，他们的领导层做了一件前所未有的事情，与拥抱脸合作，进行直接的培训和推理产品合作。与其他云提供商相比，AWS 与开源初创公司合作的记录参差不齐，但 HuggingFace 在自然语言处理(NLP)库中的主导地位使得这种协同作用非常值得期待。

[训练编译器](https://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler.html) —该产品旨在通过使用不同的 AWS 专有库运行张量运算，减少 DL 模型的训练时间。DL 模型由一个多维矩阵组成，神经网络的每一层都在训练过程中运行一系列数学运算。每种运算(加、减等)都可以归类为一个运算符。在 Numpy，有超过 1000 个运营商，你可以从我的朋友[芯片](https://huyenchip.com/2021/09/07/a-friendly-introduction-to-machine-learning-compilers-and-optimizers.html)那里了解更多这个话题。AWS 选择强调 NLP 模型，这是在打赌客户对文本深度学习的使用最多。在我自己的 CIO 电话中，我看到了类似的趋势。然而，为一系列 NLP 客户提供一个普遍可用的产品有几个挑战，我怀疑这些挑战已经被克服了。

首先，SageMaker 有自己的运行培训作业所需的容器。如果缺少操作员，您将无法运行培训作业，直到库支持优化版本。后备机制引入了会降低工作速度的巨大瓶颈。培训产品的常见问题强调了这些问题，“*使用 SageMaker Training Compiler，我总能获得更快的培训工作吗？不，不一定”。*其次，如果你需要完全控制容器和它安装的东西，你需要 AWS 团队的白手套服务。第三，如果你在实验阶段对模型进行多次迭代，这将增加你培训工作的开始时间，从而降低开发速度。我很难向任何一个 ML 团队推荐这个。对于大多数深度学习用例来说，即使运行 POC 也不太可能值得付出努力。

[Ground Truth Plus](https://aws.amazon.com/blogs/aws/announcing-amazon-sagemaker-ground-truth-plus/)—Ground Truth Plus 允许公司提交项目请求，SageMaker 项目经理会将项目与他们管理的一组工人进行匹配。Plus 和 standard Ground Truth 之间的唯一区别是工人管理。在大多数深度学习数据需求中，如音频转录、分割、分类，甚至 3D 点云标签，许多创业公司都有多年提供这些服务的经验。例子包括私人创业规模。AI、社会责任数据标注厂商 Samasource、加拿大 Telus(通过收购 Lionbridge)。

SageMaker 还为他们的工作室笔记本发布了一些新功能。

[sage maker Studio Lab](https://aws.amazon.com/blogs/aws/now-in-preview-amazon-sagemaker-studio-lab-a-free-service-to-learn-and-experiment-with-ml/)——类似谷歌 Colab 的运行笔记本的免费服务。该产品非常适合希望了解更多关于 ML 和免费计算的爱好者社区。然而，这并不能帮助大多数企业客户。我在使用该产品的等待名单上，所以我会保留更深入的评论，直到我使用它之后。

[sage maker Canvas](https://aws.amazon.com/blogs/aws/announcing-amazon-sagemaker-canvas-a-visual-no-code-machine-learning-capability-for-business-analysts/)——如果你使用过 SageMaker Autopilot，Canvas 用更多的工作室图形工具包装该产品，以最大限度地减少组织中没有 Python 数据科学经验的业务分析师的编码，他们希望进行快速的 ML 实验，我可以看到向他们展示该功能的明显好处。换句话说，任何拥有大量雪花和红移用户群的公司都有可能优化 TCO，方法是让他们的数据分析师先用 Canvas 运行 POCs，然后再让数据科学家提供支持。这里的挑战仍然是，这些千篇一律的模型可以解决的问题的类型很窄，并且基于表格数据的 ML 的大部分问题在数据处理中。截至发稿时，Canvas 在 US-West-1 上还不可用，我可以在体验后分享更多细节。

[sage maker Studio Spark Connector](https://aws.amazon.com/blogs/aws/new-create-and-manage-emr-clusters-and-spark-jobs-with-amazon-sagemaker-studio/)—Spark 可能是有史以来使用最广泛的分布式数据处理系统之一。对于 ML 之前的数据预处理，这是简化开发人员体验的起点。大多数财富 500 强公司都在 Spark 上有一些部署，部署从 on instance(裸机)、Kubernetes (Spark 运营商)、Databricks 到 AWS 的 EMR 都有。虽然我看到的数据块比 EMR 客户多，但非 EMR Spark 支持可能会出现。值得注意的是，这是开发人员体验的增强，对性能没有好处。当数据和计算位于同一位置时，大规模分布式处理作业通常运行得最好。然而，用于数据处理的 EMR 和用于 ML 模型训练的 SageMaker 是在两个完全独立的计算环境中运行的。

最后，还有一个 [SageMaker 推理推荐器](https://aws.amazon.com/blogs/aws/announcing-amazon-sagemaker-inference-recommender/)。SageMaker 和大多数 AWS 产品依赖于实例的概念。SageMaker 计算作业本质上是在标准 EC2 节点池上运行的，带有一个定制的运行时、AMI 和容器。因为实例是离散的，但是流量和工作负载是连续分布的，所以我工作过的许多企业都使用 Kubernetes 运行 ML 工作负载。Kubernetes 上有像 [knative](https://knative.dev/docs/) 这样的工具，它们可以从零开始弹性地扩展持久性服务，通过资源配置文件使用 spot 实例。像 [Seldon Core](https://github.com/SeldonIO/seldon-core) 和 [KServe](https://github.com/kserve/kserve) 这样的库将这些特性和其他特性一起打包在一个可安装的 Kubernetes 清单中。

SageMaker 推理推荐器试图通过提供每种实例类型的延迟和吞吐量指标来缩小这一差距。您必须支付实例计算成本。SageMaker 选择了创可贴，而不是进行更大的投资来制造无服务器或绑定到公司现有计算集群的模型服务。如果客户真的需要这个特性，那么编写一个脚本来实现这个推理推荐器所提供的功能是非常简单的。事实上，它的主要客户 Intuit 就是这么做的，你可以在今年早些时候在 Github 上看到他们的[开源代码。](https://github.com/intuit/perfsizesagemaker)

托管服务仍然为公司提供巨大的价值。它们通过卸载普通的配置和设置，降低了 IT 组织的总拥有成本。然而，该领域变化太快，在这一点上只有一个真正的 SageMaker 和其他主要 MLOps 平台的最终游戏。他们需要在 Kubernetes 上运行他们的工具和计算引擎。微软 Azure 和 GCP 已经在他们的 ML 平台中提供了这种形式。

Kubernetes 开源工具中的 MLOps 工具生态系统发展迅速。一些大品牌，如 Kubeflow Pipelines，已经成为企业中非常受欢迎的产品。SageMaker 可以在 MLOps 馅饼中占有很大份额，但今天他们已经采取了一种几乎是围墙花园的方法。不幸的是，这意味着 ML 团队将不得不继续混合和匹配开源、自主开发和多个供应商的服务，以使端到端的 ML 工作流运行良好。

简而言之，至少可以说，2021 年人工智能/人工智能领域的公告乏善可陈。这也不是因为缺乏客户需求——摩根士丹利首席信息官调查显示，分析(AI/ML)仍然是每个季度的前五大考虑因素。企业应该继续投资于他们自己的 MLOps 团队，该团队可以有选择地挑选出对他们的公司有意义的解决方案。所有拥有超过 25 名数据科学家的企业将无法单独在 AWS SageMaker 这样的工具上运行。查看我之前的帖子，深入了解工具选项和生态系统的状态。

尽管如此，作为一个与 SageMaker 的许多工程师一起工作过的人，我相信他们正在为这个行业进行改变游戏规则的长期投资。然而，似乎他们中的许多人还没有为 2021 年的重新发明做好准备。