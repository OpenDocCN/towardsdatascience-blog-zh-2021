# 停止在一个 GPU 上训练模型

> 原文：<https://towardsdatascience.com/pytorch-distributed-on-kubernetes-71ed8b50a7ee?source=collection_archive---------22----------------------->

## 今天，没有什么可以阻止你在多个 GPU 上扩展你的深度学习训练过程

![](img/d5641dd7e88b495d460014d0311167b9.png)

[科学高清照片](https://unsplash.com/@scienceinhd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/operator?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

到目前为止，深度学习需要大量数据。最近的进步主要是由于我们处理的数据量，这是保持引擎运转的燃料。因此，扩大模型培训流程的需求比以往任何时候都高。

与此同时，DevOps 领域也越来越受欢迎。Kubernetes 无处不在；单一的遗留系统正在分解成更容易维护的更小的微服务。如果出现问题，这些服务会被视为一次性代码，可以被立即删除和恢复。

> 我们如何将所有部分整合在一起，并将训练扩展到更多的计算资源？我们如何以自动化的方式实现这一点？可能比你想象的要简单！

那么，我们如何将所有部分整合在一起，并将训练扩展到更多的计算资源？我们如何以自动化的方式实现这一点？

**这个故事比前两个故事更进了一步，在前两个故事中，我们讨论了基础，然后将我们的知识应用到现实世界的 Kaggle 比赛中。它为 Kubernetes 世界带来了分布式培训。可能比你想象的要简单。您需要做的只是添加几行代码！**

[](/distributed-deep-learning-101-introduction-ebfc1bcd59d9) [## 分布式深度学习 101:简介

### 如何用 PyTorch 编写分布式深度学习应用的指南

towardsdatascience.com](/distributed-deep-learning-101-introduction-ebfc1bcd59d9) [](/pytorch-distributed-all-you-need-to-know-a0b6cf9301be) [## PyTorch 分发:所有你需要知道的

### 用 PyTorch 编写分布式应用程序:一个真实的例子

towardsdatascience.com](/pytorch-distributed-all-you-need-to-know-a0b6cf9301be) 

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=pytorch_operator)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# PyTorch 操作员

让我们开门见山； [PyTorch 操作员](https://github.com/kubeflow/pytorch-operator)。PyTorch 操作符是`PyTorchJob`自定义资源定义的实现。使用这个定制资源，用户可以像 Kubernetes 中的其他内置资源一样创建和管理 PyTorch 作业(例如，部署和 pods)。

我们将从上次[时间](/pytorch-distributed-all-you-need-to-know-a0b6cf9301be)停止的地方继续，并将我们的解决方案转换为 [RANZCR CLiP](https://www.kaggle.com/c/ranzcr-clip-catheter-line-classification) Kaggle 挑战，作为`PyTorchJob`操作运行。正如您将看到的，这使事情变得容易多了！你可以跟随你在这个[报告](https://github.com/dpoulopoulos/examples/tree/feature-pytorch-operator/distributed/ranzcr)中找到的代码。使用`feature-pytorch-operator`分支。

## 代码

所以，事不宜迟，让我们从主函数开始。

现在，这个函数在每个进程中都运行。PyTorch 操作员负责将代码分发给不同的`pods`。它还负责通过主流程进行流程协调。

事实上，您需要做的所有不同的事情就是在第 50 行初始化流程组，并在第 65 行将您的模型包装在一个 DistributedDataParallel 类中。然后，在第 70 行，你开始你熟悉的训练循环。所以，我们来看看`train`函数里面有什么。

如果您以前曾经编写过 PyTorch 代码，您不会发现这里有任何不同之处。还是老一套的训练程序。仅此而已！然而，你仍然应该注意如何使用`DistributedSampler`分割数据集，就像我们在上一篇[文章](/pytorch-distributed-all-you-need-to-know-a0b6cf9301be)中看到的那样。

我会让你参考 [GitHub](https://github.com/dpoulopoulos/examples/tree/feature-pytorch-operator/distributed/ranzcr) 来看看每一个实用函数是做什么的，但是我们在这里已经涵盖了基础知识！

## 配置

最后一部分是配置。为了开始我们在 Kubernetes 上的培训过程，我们需要将我们的应用程序容器化，并编写几行 YAML 代码。

要将我们的代码转换成容器映像，我们需要一个 Dockerfile。事实证明，这非常简单:复制您的代码并运行包含`main`函数的文件。

类似地，YAML 配置也相当简单。

我们指定想要启动一个带有一个主节点和一个工作节点的`PyTorchJob`定制资源。正如你所看到的，在这个例子中，我们有两个可用的 GPU。我们分配一个给主人，一个给工人。如果你有更多的 GPU，你可以提高副本的数量。最后，您需要指定保存您的代码和您想要传递的任何参数的容器映像。

恭喜你！您需要做的就是应用配置文件:

```
kubectl apply -f torch_operator.yaml
```

# 怎么实验？

为了开始使用 PyTorch 操作符，我们需要 Kubeflow，这是一个开源项目，致力于使 ML 项目的部署更加简单、可移植和可伸缩。来自[文档](https://www.kubeflow.org/):

> *kube flow 项目致力于使在 Kubernetes 上部署机器学习(ML)工作流变得简单、可移植和可扩展。我们的目标不是重新创建其他服务，而是提供一种简单的方法来将 ML 的最佳开源系统部署到不同的基础设施上。无论你在哪里运行 Kubernetes，你都应该能够运行 Kubeflow。*

![](img/fd437689762737b40c752de8ec6acaa3.png)

MiniKF

但是我们如何从 Kubeflow 开始呢？我们需要 Kubernetes 集群吗？我们应该自己部署整个系统吗？我的意思是，你看过库伯弗洛的[清单回购](https://github.com/kubeflow/manifests)吗？

不要慌；最后，我们需要用 Kubeflow 做实验的只是一个 GCP 或 AWS 账户！我们将使用 MiniKF。MiniKF 是一个单节点 Kubeflow 实例，预装了许多优秀的特性。具体来说:

*   [**Kale**](https://github.com/kubeflow-kale/kale) **:** 用于 Kubeflow 的编排和工作流工具，使您能够从笔记本电脑开始运行完整的数据科学工作流
*   [**Arrikto Rok**](https://www.arrikto.com/rok-data-management)**:**一个数据版本化系统，支持可再现性、缓存、模型血统等等。

因此，要在 GCP 上安装 Kubeflow，请遵循我在下面提供的指南:

[](/kubeflow-is-more-accessible-than-ever-with-minikf-33484d9cb26b) [## 有了 MiniKF，Kubeflow 比以往任何时候都更容易访问

### 10 分钟入门 Kubernetes 最好的机器学习平台。

towardsdatascience.com](/kubeflow-is-more-accessible-than-ever-with-minikf-33484d9cb26b) 

或者，如果您喜欢 AWS:

[](/mini-kubeflow-on-aws-is-your-new-ml-workstation-eb4036339585) [## AWS 上的 Mini Kubeflow 是您的新 ML 工作站

### 通过 AWS 上的 MiniKF 加速您的机器学习模型开发

towardsdatascience.com](/mini-kubeflow-on-aws-is-your-new-ml-workstation-eb4036339585) 

# 结论

深度神经网络(DNNs)一直是机器学习领域大多数最新进展背后的主要力量。像这样的突破主要是由于我们可以处理的数据量，这增加了将训练过程扩展到更多计算资源的需求。

与此同时，DevOps 领域也越来越受欢迎。Kubernetes 无处不在；单一的遗留系统正在分解成更容易维护的更小的微服务。

我们如何将两个世界融合在一起？在这个故事中，我们研究了如何使用 MiniKF 上运行的 PyTorch 操作符来解决一个真实的用例。

# 关于作者

我叫[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=pytorch_operator)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=medium&utm_medium=article&utm_campaign=pytorch_operator)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！