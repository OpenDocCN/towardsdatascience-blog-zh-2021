# 用 FedJAX 进行联合学习研究

> 原文：<https://towardsdatascience.com/conducting-federated-learning-research-with-fedjax-7be86d349a94?source=collection_archive---------15----------------------->

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)； [ToF](http://towardsdatascience.com/tagged/tof)

## 关于使用 FedJAX 进行联合学习研究的十个或更少的要点

![](img/5694aa25eda163d857dc9105ad30ddf7.png)

(图片由作者提供)ToF 中“o”顶部的️symbol 表示该 t of 的内容本质上更具技术性，因此可能更适合对机器学习以及编码概念更有经验的读者。

****十个或更少的*** *(ToF 的简称，可以读作“tuff”)博客背后的想法是高效地分享围绕特定主题的主要概念、要点、观点等。具体来说，我会尽量简短，只说十件或更少的相关事情。**

**隶属关系披露:
我在*[*integrate . ai*](http://integrate.ai)*领导机器学习科学团队，在这里我们专注于让联合学习(FL)更容易实现。我不隶属于 FedJAX，也没有参与过这个开源项目。**

**附送代码:*[*https://github . com/integrate ai/openlab/tree/main/fed jax _ tutorial*](https://github.com/integrateai/openlab/tree/main/fedjax_tutorial)*

*(0) *(一本 FL 入门书，你可以随意跳过，只要需要就可以回头查阅。)* **FL 允许建立机器学习模型，而无需首先将数据带到中央位置。**在典型的 FL 设置中，有*个客户端* ，它们各自将自己的数据集存放在自己的单独节点中，还有一个中央*服务器*，它“利用”这些非集中式数据集，以便构建一个全局 ML 模型(即，通常称为*服务器模型)*，然后由所有客户端共享。重要的是，数据集从不聚集在一起形成大规模的训练数据集，而是保存在各自的客户端节点上，并且仅通过在每个客户端节点和中央服务器之间传递模型参数(或它们的更新)来促进训练。FL 框架的基本步骤如下(改编自 [Ro，Suresh，Wu (2021)](https://arxiv.org/pdf/2108.02117.pdf) )。*

```
*Note that that FL training is done in multiple rounds, where in each round the following steps are taken:1\. Server selects a set of clients to participate in the current round;2\. Server communicates the current model parameters of the global, server model to the participating clients;3\. Each client creates a local version of the server model using the snapshot provided in 2\. and updates this by training it on its local data.4\. After performing it's individual client update, each client then communicates back the model parameters updates to the server.5\. The server takes the collection of client model parameter updates and performs an aggregation on them (e.g., via averaging).6\. The server updates the server model via the aggregated model parameter update computed in the previous step.*
```

*(1) **FedJAX 是一个进行 FL 研究的库。FedJAX 旨在缩短开展 FL 研究的周期时间(例如，为 FL 进行实验)并使其更加标准化。它通过两种方式做到这一点。首先，由于在进行 FL 模型的算法研究时，执行服务器和客户机之间的通信是完全不必要的，因此 FedJAX 模拟基本上跳过了步骤 2。第四。上面列出的。这是因为使用 FedJAX，多客户端 FL 设置只是在单台机器上虚拟创建的，因此，FedJAX 不应该用于构建和部署实际的 FL 解决方案。其次，FedJAX 为 FL 框架中的核心元素提供了方便的接口，例如:`FederatedData`、`ClientDataset`、`Models`、`ClientSampler`和`FederatedAlgorithm`，它们调用函数来处理客户端模型更新和服务器模型更新。***

*在 FedJAX 中执行 FL 培训的代码片段。*

*(2) **上 FedJAX 一点都不差。**我发现在 FedJAX 上提升到可以在数据集上执行普通水平 FL 的水平相对简单。尽管在写这篇博客的时候还处于早期阶段，文档[和官方](https://fedjax.readthedocs.io/en/latest/)[代码](https://github.com/google/fedjax)还是很有帮助的。此外，正如学习任何新库的典型情况一样，有一点相关的学习曲线，所以我花了大约 1 周的时间才能够学习足够的库来完成两个示例:I)使用被分成 3400 个客户端的 EMNIST 数据集的示例；以及 ii)一个使用 CIFAR-10 数据集的示例，该数据集被分成 100 个客户端。对于 EMNIST 示例，我使用的数据集和 CNN 模型都是由 FedJAX 库提供的。相比之下，对于 CIFAR-10 示例，我使用 FedJAX 提供的帮助函数创建了一个定制数据集和一个定制 CNN 模型。下面分享了运行时和最终服务器模型的准确性，但是您也可以找到这两个示例的代码，以便自己执行它们[这里](https://github.com/integrateai/openlab/tree/main/fedjax_tutorial)！*

*打印输出报告两个 FL 示例的性能指标和运行时间:EMNIST 和 CIFAR-10。*

*(3) **事先熟悉 JAX 并不是入门的必要条件，但对于使用 FedJAX 进行更严肃的研究来说，了解这一点是很重要的。在深入研究 FedJAX 之前，我没有和 [JAX](https://jax.readthedocs.io/en/latest/) 一起工作过，但是我没有发现这是学习 FedJAX 接口和在前面提到的两个例子上运行 vanilla FL 的主要障碍。然而，当更深入地查看源代码时，我肯定开始意识到，我可以从更熟悉 JAX 中受益。所以，我觉得如果有人想使用 FedJAX 在外语方面做更严肃的研究，那么花时间熟悉 JAX，或许还有其他基于 JAX 的深度学习库，比如俳句，肯定是值得的。***

**接下来要说的三件事涵盖了 FedJAX (* `*v=0.0.7*` *)库中提供的许多核心类中的三个，这些核心类支持标准化的外语模拟。**

*(4) `FederatedData` **是包含关于客户端数据集的元信息和用于在模拟外语训练下执行任务的便利方法的数据结构**。此外，由于`FederatedData`可以被认为是提供了`client_id`和`ClientDataset`之间的映射，因此访问表示实际观察的类似数组的数据结构需要更多的工作——这在一定程度上反映了实际的 FL 设置，其中从客户端节点访问数据是具有挑战性的，或者出于隐私考虑，在没有干预的情况下甚至是不可能的。最后，FedJAX 方便地提供了从一个`numpy.array`创建一个小的定制`FederatedData`的功能，这是我在第二个例子中用来创建 CIFAR-10 数据集的功能。*

*从 numpy.array 创建 FederatedData 并访问客户端数据观察的示例代码片段。(改编自 FedJAX [文档](https://fedjax.readthedocs.io/en/latest/notebooks/dataset_tutorial.html)中教程的代码)*

*(5) `Model` **是用于指定服务器 ML 模型结构和损失函数的 FedJAX 类。**重要的是要注意到`Model`类是无状态的，这意味着在每个历元/回合之后的训练期间没有`params`属性被更新。相反，`Model`将其信息提供给`jax.grad()`，后者自动计算相关目标函数的梯度，并将这些信息传递给选定的`Optimizer`，后者最终返回一组完全符合`Model`结构的`params`。通过调用`apply_for_eval()`(或者`apply_for_train()`，如果需要一个随机键来为像 dropout 这样的东西注入随机性的话)可以生成对例子的预测。FedJAX 还提供了助手函数，用于从其他基于 JAX 的深度学习库(如 Haiku 或`jax.example_libraries.stax`)创建`Model`。*

*对一批示例进行训练后获取一组新参数的代码片段。然后，新的参数可以被传递给另一轮训练或评估函数，该函数将参数和模型都作为输入。*

*(6) `FederatedAlgorithm` **是促进 FL 训练的类(即，客户端和服务器更新，或步骤 3。& 5。本博客顶部列出了一个** `Model` **上一个** `FederatedData` **。构建一个定制的`FederatedAlgorithm`将需要定义执行客户端模型更新以及聚合和服务器模型更新的函数，这些函数应该封装在它的`apply()`方法中。只有另外一个方法属于这个类，即`init()`，它初始化服务器模型的状态。FedJAX 还旨在通过利用可用的加速器来提高客户端更新的效率，因此，您需要以特定的方式定义客户端更新函数(即，通过三个函数`client_init`、`client_step`和`client_final`)。***

*(7) **总的来说，FedJAX 得到了很多承诺。加速和规范外语研究的前提是好的。在撰写本文时，FedJAX 提供的接口既有用又方便，为更深入地研究 FL 打下了良好的基础。在不久的将来，我希望看到更多的数据集和基准有助于这个项目，以提高可重复性和整个领域的水平设置。此外，超越普通 FL 的更多模块和示例将使该项目更加相关，例如考虑不同隐私、跨客户端数据集的非 iid 案例、垂直 FL，甚至可能是非 SGD 类型的客户端/服务器模型的方法。***