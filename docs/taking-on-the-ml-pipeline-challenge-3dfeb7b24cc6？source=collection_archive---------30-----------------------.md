# 迎接 ML 管道挑战

> 原文：<https://towardsdatascience.com/taking-on-the-ml-pipeline-challenge-3dfeb7b24cc6?source=collection_archive---------30----------------------->

## [行业笔记](https://towardsdatascience.com/tagged/notes-from-industry)

## 为什么数据科学家需要在生产中拥有自己的 ML 工作流

本文讨论了在生产环境中让数据科学家拥有其工作流的好处。我们还讨论了 ZenML，这是一个开源框架，专门为促进这种所有权而构建。

# 为什么我们需要 ML 管道？

让我们从最明显的问题开始:为什么我们首先需要 ML 管道？以下是一些确凿的理由:

*   ML 管道是以一种可重复的、健壮的和可再现的**方式**自动化 ML 工作流程**的最佳方式。**
*   组织可以通过**标准化**界面集中、协作和管理工作流。
*   ML 管道可以看作是不同部门之间**协调**的一种手段。

然而，尽管 ML 管道很棒，但它们确实存在一些可能会被证明有问题的内在问题。

# 所有权困境

![](img/bf4644da2b3f156eb38fe1586c09ccbe.png)

生产中的 ML 令人困惑——图片由作者提供

开发机器学习的组织需要回答的一个问题是**谁拥有生产中的 ML 管道**？创建模型的是数据科学家吗？是数据工程师在生产中部署它吗？完全是别人吗？

> 注意:对于 ML 过程中所涉及的角色的概述，请查看[加州大学伯克利分校的全栈深度学习课程](https://fall2019.fullstackdeeplearning.com/course-content/ml-teams/roles)的这一部分。

ML 的数据维度使其[比生产中的其他部署](https://research.google/pubs/pub46555/)复杂得多。通常情况下，数据科学家不具备可靠地将模型引入生产的技能。因此，团队以这样一种方式组织是很自然的，即所有权从开发的培训阶段转移到工程部门方向的部署阶段。

以这种方式组织可以很容易地在生产中得到一个模型**。然而，当事情(不可避免地)出错时，所有权问题才真正发挥作用。这可能是因为模型/概念漂移，或者某些东西超时，或者数据格式改变，或者工程部门不知道的一百万件其他事情，因为他们**没有生产模型**。**

**此外，你把数据科学家推得离生产环境越远，就越难建立一个 [**数据飞轮效应**](https://blog.modyo.com/posts/data-flywheel-scaling-a-world-class-data-strategy) 持续改进你的生产模型。数据科学家需要更广泛的背景，以便在训练模型时能够做出正确的判断。例如，如果这意味着该模型可能在生产中执行得更快并产生更高的收入，我们是否应该牺牲 AUROC 几个百分点？如果只看像 AUROC 这样的模型指标，人们甚至不可能问这个问题。**

**事实上，我们越是将数据科学家从生产中推出，整个 ML 过程的[效率就会越低，协调成本和等待时间就会越长](https://multithreaded.stitchfix.com/blog/2019/03/11/FullStackDS-Generalists/#back-1)。这对于 ML 来说是不可行的，ML 应该是一个快速迭代的过程。**

# **解决所有权困境**

**最近，有很多关于[数据科学家不应该需要了解 Kubernetes](https://huyenchip.com/2021/09/13/data-science-infrastructure.html) 的讨论，或者底层生产基础设施需要从他们那里抽象出来以在高水平上执行。我不仅同意这一点，而且认为我们必须更进一步。我们不仅需要从数据科学家那里抽象出基础设施，还要帮助他们在生产过程中获得模型的所有权。**

**如果一个团队把编写 ML 管道留到以后，他们会很快积累技术债务。有了正确的抽象，组织可以激励他们的数据科学家在开发过程的早期开始编写端到端的 ML 管道。一旦数据科学家开始使用容易“转移”的 ML 管道编写他们的培训/开发工作流，一旦这些管道投入生产，他们就会发现自己处于一个熟悉的环境中。然后，他们将拥有必要的工具，也可以进入生产系统，在问题出现时解决问题，或者随着时间的推移进行改进。**

**理想情况下，管道成为一种机制，通过这种机制，模型的生产者(即数据科学家)可以在生产过程中一直拥有他们的模型。**

**请注意，这并不**而不是**意味着数据科学家现在应该知道工程工具箱中的每一个工具箱(比如 Kubernetes 集群的复杂部署)。相反，争论的焦点是我们需要设置数据科学家，以便他们可以在必要工具的帮助下，将他们的代码投入生产。**

# **进入 ZenML:一个为现代 MLOps 设计的框架**

**[ZenML](https://github.com/zenml-io/zenml) 是一个开源的 *MLOps 管道框架*专为解决上述问题而构建。让我们来分解一下 MLOps 管道框架的含义:**

*   **MLOps :它在操作化机器学习的领域中运行，即，将 ML 工作流投入生产。**
*   ****管道**:它通过帮助创建管道来实现这一点，即在这种情况下执行的一系列步骤，特别是针对 ML 设置。**
*   ****框架:**最后，它用提供通用功能的抽象来创建这些管道软件，这些功能可以由额外的用户编写的代码有选择地更改。**

**这是一个让你定义管道的工具，但是它和其他的有什么不同呢？这是它与众不同的地方:**

## **适应爆炸式的 ML 工具环境**

**众所周知，我们现在正处于 ML/MLOps 工具领域的大爆发之中。ZenML 被明确地设计成对你想要使用的底层基础设施/工具没有意见。相反，它公开了像`Metadata Stores`、`Artifact Stores`和`Orchestrators`这样具有公共接口的高级概念。然后，ML 团队可以更换他们的管道后端的单个组件，它就会“正常工作”。**

**![](img/b18c6ebc5cd68eddb0f15927cb1ef6b7.png)**

**仔细看:这不是[隐藏技术债务图](https://proceedings.neurips.cc/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf)；-) —作者图片**

**因此，如果您甚至想使用 [MLFlow](https://mlflow.org/) 来跟踪您的实验，在[气流](https://airflow.apache.org/)上运行管道，然后将模型部署到 [Neptune](https://neptune.ai/) 模型注册表，ZenML 将为您提供这个 MLOps 堆栈。这个决定可以由数据科学家和工程师共同做出。因为 ZenML 是一个框架，所以也可以在这里添加定制的部分来适应遗留的基础设施。**

## **关注机器学习工作流程(在每个阶段)**

**有许多工具可以让你将工作流定义为管道，但很少有工具明确地将**聚焦于机器学习用例**。**

```
@trainer
def trainer(dataset: torch.Dataset) -> torch.nn.Module:
   ... 
   return model
```

**举个例子，在上面的代码中，ZenML 会明白这不仅仅是管道中的一个步骤，而是一个训练器步骤。它可以使用该信息来帮助特定于 ML 的用例，如将结果存储在模型注册表中、在 GPU 上部署该步骤、超参数调整等。**

**还要注意，ZenML 完全支持来自常见 ML 框架的对象，如`torch.Dataset`和`torch.nn.Module`。这些对象可以在步骤和缓存的结果之间传递，以实现更快的实验。**

## **激励数据科学家编写这些管道**

**ZenML 明白管道会随着时间而改变。因此，它鼓励在本地运行这些管道，并在产生结果时进行试验。您可以在本地 Jupyter 笔记本中查询管道，并使用不同的预先制作的可视化工具(如统计可视化和模式异常)将其具体化。**

**![](img/cdae9ef75513384135fddfed52649c8c.png)**

**运行管道后，无论是否在本地运行，都可以轻松地获取它们并查看结果——图片由作者提供**

**这是一种不同的管道开发方法，更能代表数据科学家在项目早期阶段的工作方式，即快速迭代和可视化，帮助他们对实验做出明智的决策。我们称这种方法为`Pipelines As Experiments (PaE)`。**

**简而言之，ZenML 允许您用简单的、可扩展的抽象来创建自动化的 ML 工作流，从而减轻您的公共模式负担。在这样做的时候，它不会对底层基础设施发表意见，而是旨在做到与云和工具无关。**

**通过帮助目标受众，即数据科学家，在开发生命周期的早期用 ZenML 管道**编写他们的代码，从实验阶段到生产阶段的过渡变得更加容易。目标是在 ML 生命周期的实验阶段结束时，数据科学家可以切换到生产 ML 堆栈，让他们的管道在生产中运行。此时，他们将拥有这些管道的完全所有权，并且可以随心所欲地管理、更新和调试它们。****

# **故事到此为止**

**迄今为止，ZenML 已收到:**

*   **1336 [GitHub 群星](https://github.com/zenml-io/zenml)。**
*   **10 [贡献者](https://github.com/zenml-io/zenml/graphs/contributors)。**
*   **约 200 名[松弛](https://zenml.io/slack-invite)成员。**

**看到人们对最初只是一个简单的想法感兴趣真是太棒了。我们现在公开构建 [ZenML(这里没有隐藏)](https://zenml.io/newsletter)，而[刚刚发布了一个重大版本，对代码库](https://github.com/zenml-io/zenml/releases)进行了完全重构。因此，如果以上任何一个吸引你，如果你用一个在生产中部署管道的[端到端的例子](https://docs.zenml.io/guides/low-level-api)来给 ZenML 一个旋转，那将是非常好的。欢迎反馈和投稿！**

**我最诚挚地感谢亚历克斯·斯特里克和亚当·普罗布斯特帮助编辑这篇文章。**

## **参考和欣赏**

**本帖中引用了一些有用的链接:**

**[](/why-ml-should-be-written-as-pipelines-from-the-get-go-b2d95003f998) [## 为什么 ML 应该从一开始就写成管道

### 通过迭代、可复制的管道消除技术债务。

towardsdatascience.com](/why-ml-should-be-written-as-pipelines-from-the-get-go-b2d95003f998) [](https://fall2019.fullstackdeeplearning.com/course-content/ml-teams/roles) [## 角色

### 机器学习团队内部有哪些不同的角色？他们每个人都需要哪些技能？

fall2019.fullstackdeeplearning.com](https://fall2019.fullstackdeeplearning.com/course-content/ml-teams/roles) [](/avoiding-technical-debt-with-ml-pipelines-3e5b6e0c1c93) [## 避免 ML 管道的技术债务

### 开始考虑管道而不是脚本，以避免开发 ML 系统时的技术债务。

towardsdatascience.com](/avoiding-technical-debt-with-ml-pipelines-3e5b6e0c1c93) [](https://huyenchip.com/2021/09/13/data-science-infrastructure.html) [## 为什么数据科学家不需要了解 Kubernetes

### 这篇文章旨在说明，虽然数据科学家拥有整个堆栈是件好事，但他们可以这样做，而不必…

huyenchip.com](https://huyenchip.com/2021/09/13/data-science-infrastructure.html) [](https://eugeneyan.com/writing/end-to-end-data-science/#from-start-identify-the-problem-to-finish-solve-it) [## 不受欢迎的观点-数据科学家应该更加端到端

### 最近，我在 Reddit 上看到一个关于数据科学和机器学习中不同角色的帖子:数据科学家…

eugeneyan.com](https://eugeneyan.com/writing/end-to-end-data-science/#from-start-identify-the-problem-to-finish-solve-it) [](https://multithreaded.stitchfix.com/blog/2019/03/11/FullStackDS-Generalists/#back-1) [## 当心数据科学引脚工厂:全栈数据科学通才和

### 在《国富论》中，亚当·斯密展示了劳动分工是生产率提高的主要来源…

multithreaded.stitchfix.com](https://multithreaded.stitchfix.com/blog/2019/03/11/FullStackDS-Generalists/#back-1) [](https://docs.zenml.io/) [## ZenML 101

### ZenML 管道执行特定于 ML 的工作流，从数据源到分割、预处理、训练，一直到…

docs.zenml.io](https://docs.zenml.io/)**