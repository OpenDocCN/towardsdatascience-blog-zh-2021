# 什么是动态图，为什么它们很有趣？

> 原文：<https://towardsdatascience.com/what-are-dynamic-graphs-and-why-they-are-interesting-180b9fab9229?source=collection_archive---------11----------------------->

由于复杂的现实生活情况可以用图表来模拟，它们随时间的演变可以用动态图表来捕捉。

![](img/134858b4bfd6f5b6fd6fec3f40607fe3.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

一个无向图可以表示为 **G** ( *V，E* )其中 V 是顶点/节点，E 是两个顶点之间的边/连接。

我们以类似的方式定义一个动态图，但是作为一系列快照，即 **G** = (G₁，G₂，…)。Gₜ)，其中 gₜ=(eₜvₜ)，t 是快照的数量。在动态图中，新节点可以形成并创建与现有节点的链接，或者节点可以消失，从而终止现有链接。

这在某种程度上是一个正式的定义，但我们将使用一个简单的社交网络示例来理解动态图。

# 社交网络的演变

![](img/c3091907291e63eb855bdaea8e502086.png)

图一。社交网络示例(来源:作者)

让我们构建一个由 5 个人组成的小型社交网络图(见图 1)。这里的节点代表人，边代表两个人之间的友谊。这是 T = 0 时的情况。当他们相互作用一段时间后，一些新的关系会形成，一些会破裂。此外，请记住，新人可以在任何时间进入这个社交网络，现有成员也可以离开。

![](img/a05ed9f4ece839723e83bd6dfe09b86c.png)

社交网络的演变(来源:作者)

在社交网络的发展过程中，我们在 3 个时间点获得了 3 个快照。所有这三个快照构成了动态图。我们见证了一些新友谊的建立，也见证了一些破裂。我们可能会遇到这样的情况:有新的输入节点(人们加入网络)和一些输出节点(人们离开网络)。

研究这些动态图表将为我们提供一些关于社交网络中人际关系的复杂行为的深刻见解。

这些动态图不仅适用于社交网络，还可以应用于其他领域，例如生物医学领域。让我们再看一个生物医学领域的例子，我们可以用动态图的形式来模拟一些生物实体。

# 蛋白质相互作用

![](img/2d074e9d1ec9e3e574535d42f7e8a6a4.png)

[国立癌症研究所](https://unsplash.com/@nci?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/dna?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

蛋白质对于人类细胞中的大多数生物过程都很重要，如基因表达、细胞生长等等。但是蛋白质很少单独发挥作用，它们往往通过[蛋白质-蛋白质相互作用](https://en.wikipedia.org/wiki/Protein%E2%80%93protein_interaction) (PPIs)形成关联，以便在细胞内执行不同的分子过程。这些 PPI 是两个或多个蛋白质分子之间的物理接触，是细胞中发生的生化事件的结果。

这里，节点是编码蛋白质的基因，边是不同蛋白质之间的相互作用。有像 [DNA 微阵列](https://en.wikipedia.org/wiki/DNA_microarray)这样的实验使我们能够同时研究数千个基因的特性。这些微阵列为我们提供了检测基因的信息，这些基因可能会也可能不会被翻译成蛋白质。

来自微阵列实验的数据可以从几个样本细胞(健康的人类细胞、被冠状病毒感染的细胞等)中产生，并被存储为基因表达数据。这些实验在相同的基因上以规则的时间间隔进行，因此我们从这些基因表达数据中获得了时间信息。

![](img/79d8b376290210cb8758962b0be41561.png)

作为动态图的 PPI 网络(来源:作者)

简单来说，我们有一个 T = 0 时的蛋白质相互作用图。我们使用来自基因表达数据的时间信息来识别哪些基因在什么时间点被激活，以便构建动态图。

例如，如果我们的 DNA 微阵列实验在 5 个不同的时间点(0h、3h、6h、9h 和 12h)进行，那么我们将在 5 个时间点得到 PPI 网络的动态图。

# 动态图的使用

![](img/ae5e9d94cffdf159967b7a8f1545f6f2.png)

照片由[艾米丽·莫特](https://unsplash.com/@emilymorter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/research?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

正如我之前提到的，动态图有助于捕捉真实世界实体的复杂行为(比如社会网络中的人类，或者 PPI 网络中的基因编码蛋白质)。我们还将时间维度添加到数据集，以解释一个过程从开始到结束的演变。

为了从动态图数据中提取有价值的信息，我们可以采用机器学习技术。首先，我们需要学习动态图中存在的实体的数字表示，然后我们可以使用图形神经网络等方法来模拟几种用例，如节点分类、链接预测等。

这种图上的一个非常常见的任务是社区检测，这在 PPI 的上下文中被称为复杂检测。基本上，我们使用无监督的机器学习技术，如聚类，来识别每个时间点的重要组。对于 PPI 网络来说，这是有趣的，因为识别重要的蛋白质簇/复合物将导致更好的疾病药物发现。

我们还有一个图形比对任务，我们的目标是识别在不同类型的样品(如健康或受感染的细胞)中显示差异的蛋白质或复合物。这有助于生物学家找到他们在寻找治疗方法时应该关注的蛋白质。

在社交网络的情况下，我们可以使用动态图分析来识别网络中的垃圾邮件发送者(垃圾邮件检测)。动态图的核心思想是相同的，但不同的领域有自己的用例，我们可以使用机器学习模型来满足他们的需求。

使用链接预测，我们可以通过分析网络随时间的增长，向社交网络中的用户推荐朋友。对于 PPIs，我们可以提供一个蛋白质建议列表，这意味着我们希望提出一个具有形成新相互作用潜力的蛋白质列表。

# 结论

动态图的时间方面捕捉了网络的演变，这使我们能够对几个实体之间复杂的现实生活交互进行建模。存在最先进的机器学习技术，以在动态图的基础上建立预测模型，这具有强大的潜力来推动药物发现、人造肉(基于植物的肉生成)等的研究。

如果你看到了这篇文章的这一部分，感谢你的阅读和关注。我希望你觉得这篇文章内容丰富，并且可以通过 [LinkedIn](https://www.linkedin.com/in/rohithteja/) 、 [Twitter](https://twitter.com/RohithTeja15) 或 [GitHub](https://github.com/rohithteja) 联系到我。