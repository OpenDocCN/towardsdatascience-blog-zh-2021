# 用图形卷积神经网络学习嗅觉(分子)

> 原文：<https://towardsdatascience.com/learn-to-smell-molecules-with-graph-convolutional-neural-networks-62fa5a826af5?source=collection_archive---------18----------------------->

## 结合化学和图形深度学习的端到端项目

![](img/b5c47376ec3301d36c66a3268ecda945.png)

由[拉斐尔·比斯卡尔迪](https://unsplash.com/@les_photos_de_raph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/molecules?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在这里，我们要完成以下任务:

1.  构建一个**定制的**图形数据集，其格式适合在 DGL 工作(这是本文的主题😎)
2.  随机准备训练和测试数据集(了解这一点很好👍)
3.  定义 Conv 网络图(小菜一碟🍰)
4.  训练和评估准确性(像往常一样🧠)

## 1.构建适用于 DGL 的自定义图表数据集格式

我们要处理的数据集来自于[AIcrowd Learning to smear Challenge](https://www.aicrowd.com/challenges/learning-to-smell)，它由一个列和另一个列组成，其中一个列是识别特定分子的微笑字符串，另一个列是这些分子的气味名称。

[](https://www.aicrowd.com/challenges/learning-to-smell) [## AIcrowd |学习嗅觉|挑战

### 💡试试这个有趣的新方法📹错过市政厅了吗？观看这里开始与一些惊人的解释…

www.aicrowd.com](https://www.aicrowd.com/challenges/learning-to-smell) 

例如，下表中的第二个分子，碳酸二甲酯-13C3，其微笑是 COC(=O)OC，其气味被定义为“清新、飘渺、果味”，这当然与人们对这种物质的了解相对应[1]。对于所有这些 4316 个分子，有 100 多种不同的气味形成无序的组合。

然后，我们希望 a)将每个分子的微笑转换成 DGL 图，b)标记它是否有水果味，c)去除任何有问题的分子。以下代码块处理每个请求:

A)必须为每个分子定义 DGL 图形对象。我们可以使用 RDKit 首先从它的微笑中形成一个 mol RDKit 对象(第 2 行)来实现这一点。由于我们想识别节点及其连接，我们可以得到邻接矩阵(第 3 行和第 4 行)。非零值的索引标识连接的节点(第 5 行)并给出双向图的源节点和目的节点(第 6 行和第 7 行)。

我们使用来自 *ogb* 库【2】的`atom_to_feature_vector`来生成原子的特征向量

例如，当我们对字符串 COC(=O)CO 应用`feat_vec` 时，我们得到:

注意，每一行对应于一个具有 9 个特征的原子。这些特征是原子的物理化学性质，例如，原子序数、氢的数目，或者原子是否在环中，等等。

c)现在我们有了带有特征的图表，是时候定义标签了。正如我们所见，每个分子都与一种以上的气味相关联。这里为了简单起见，问题将是一个**二元分类**来确定分子是否有**水果**气味。下面的代码块执行该任务，如果有水果香味，则分配标签 a **1** ，如果没有，则分配标签 **0** 。

在这个数据集中，我注意到有一小部分分子没有正确地转换成 DGL 图，所以我决定去掉它们。我通过简单地忽略如下所示的异常来做到这一点:

## 2.随机准备训练和测试数据集

完成所有这些步骤后，数据集就准备好了，但我们将进一步将其转换为 DGL 数据集，以便顺利地进行预处理并形成训练和测试子集。这里我将引用 DGL 团队的“[制作自己的数据集](https://docs.dgl.ai/en/0.6.x/new-tutorial/6_load_data.html)”官方教程中的概述[3]:

> 您的自定义图形数据集应该继承`dgl.data.DGLDataset`类并实现以下方法:
> 
> `__getitem__(self, i)`:检索数据集的第`i`个例子。一个例子通常包含一个 DGL 图，偶尔还包含它的标签。
> 
> `__len__(self)`:数据集中的样本数。
> 
> `process(self)`:从磁盘加载并处理原始数据。

过程方法是我们给图形加标签的地方。从一个文件中得到它是可能的，但是在我们的例子中，只有图表列表和标签列表被转换成 torch 张量。因此这是一个非常简单的类:

然后，我们继续构建列车并测试随机抽样。这样做的好处是可以设置批量大小。在这里，我们将其设置为每批五个图形:

## **3。**定义 Conv 网络图

我们正在定义 GCNN 的路上。我已经在之前的一篇文章中解释了 GCN 类是如何构建的。每个节点都有一个输出特征张量，因此为了进行二元分类，我们可以将输出特征的长度设置为类的数量(此处为两个)，然后使用`dgl.mean_nodes`对所有特征节点进行平均，以获得每个图的平均(二维)张量:

## **4。**训练和评估准确性

最后，我们建立模型，每个原子接受 9 个原子特征，并返回一个二维张量，用于定义是否有水果香味。历元在批次上运行，损失用交叉熵计算。最后一部分只是一个循环，对正确的预测进行计数，以评估准确性:

我们得到的精度大约是 0.78，这已经很不错了。现在，你可以尝试预测其他气味，甚至更好地制作你自己的定制数据集，与分子或任何你想要的 DGL 一起工作。

如果你想试代码，打开这个 [Colab 笔记本](https://github.com/napoles-uach/DGL_Smell/blob/main/dgl_smell_fruity.ipynb)。如果你有任何问题，我将非常乐意回答。最后，考虑订阅邮件列表:

[](https://jnapoles.medium.com/subscribe) [## 每当 joséMANUEL náPOLES du arte 发表文章时都会收到电子邮件。

### 编辑描述

jnapoles.medium.com](https://jnapoles.medium.com/subscribe) 

并查看我以前的帖子:

[](/start-with-graph-convolutional-neural-networks-using-dgl-cf9becc570e1) [## 从使用 DGL 的图形卷积神经网络开始

### 轻松的介绍

towardsdatascience.com](/start-with-graph-convolutional-neural-networks-using-dgl-cf9becc570e1) [](/making-network-graphs-interactive-with-python-and-pyvis-b754c22c270) [## 用 Python 和 Pyvis 制作交互式网络图。

### 制作精美图表的简单方法。

towardsdatascience.com](/making-network-graphs-interactive-with-python-and-pyvis-b754c22c270) [](/graph-convolutional-nets-for-classifying-covid-19-incidence-on-states-3a8c20ebac2b) [## 用于状态新冠肺炎关联分类的图卷积网

### 如何将地图映射到图卷积网络？

towardsdatascience.com](/graph-convolutional-nets-for-classifying-covid-19-incidence-on-states-3a8c20ebac2b) 

参考资料:

1)

[](https://doi.org/10.1039/C7GC01764B) [## 碳酸二甲酯及其衍生物的反应

### 世界范围内对可持续发展和生物相容性化学的强烈要求已经导致工业界和学术界开发…

doi.org](https://doi.org/10.1039/C7GC01764B) 

2)

[](https://github.com/snap-stanford/ogb) [## GitHub - snap-stanford/ogb:图形机器的基准数据集、数据加载器和评估器…

### 开放图形基准(OGB)是一个图形机器的基准数据集，数据加载器和评估器的集合…

github.com](https://github.com/snap-stanford/ogb) 

3)

 [## 创建自己的数据集- DGL 0.6.1 文档

### 编辑描述

docs.dgl.ai](https://docs.dgl.ai/en/0.6.x/new-tutorial/6_load_data.html)