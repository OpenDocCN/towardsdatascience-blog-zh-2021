# 使用 PyTorch 几何图形的神经网络初学者指南—第 1 部分

> 原文：<https://towardsdatascience.com/a-beginners-guide-to-graph-neural-networks-using-pytorch-geometric-part-1-d98dc93e7742?source=collection_archive---------1----------------------->

## PyTorch 几何入门

> 不知何故，你偶然发现了图形神经网络，现在你有兴趣用它来解决一个问题。让我们探索一下如何使用我们可以使用的不同工具来建模一个问题。

![](img/91968ffe2edb4e802b5b291eb4fa1b0a.png)

来自 [Pexels](https://www.pexels.com/photo/ferris-wheel-at-night-2911364/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的照片

首先，我们需要解决一个问题。

# 开始使用。

让我们挑选一个简单的图形数据集，比如[扎卡里的空手道俱乐部](https://en.wikipedia.org/wiki/Zachary%27s_karate_club)。这里，节点代表参与俱乐部的 34 名学生，链接代表俱乐部外成对成员之间的 78 种不同互动。有两种不同类型的标签*即*两派。我们可以使用这些信息来制定节点分类任务。

我们将图分为训练集和测试集，其中我们使用训练集来构建图神经网络模型，并使用该模型来预测测试集中的缺失节点标签。

这里我们用 [**PyTorch 几何**](https://github.com/rusty1s/pytorch_geometric) (PyG) python 库来建模图形神经网络。或者，[深度图形库](https://docs.dgl.ai/) (DGL)也可以用于同样的目的。

PyTorch Geometric 是一个基于 PyTorch 构建的几何深度学习库。已经使用 PyG 实现了几种流行的图形神经网络方法，您可以使用内置数据集来研究代码，或者创建自己的数据集。PyG 使用了一个漂亮的实现，它提供了一个 InMemoryDataset 类，可以用来创建自定义数据集(*注意:InMemoryDataset 应该用于小到足以加载到内存中的数据集*)。

Zachary 的空手道俱乐部图表数据集的简单可视化如下所示:

![](img/98a5d90559060a838ccb1a068d2fb44f.png)

扎卡里空手道俱乐部图形数据可视化(来源:me)

# 公式化问题。

为了阐明这个问题，我们需要:

1.  图形本身和每个节点的标签
2.  [坐标格式的边缘数据](https://scipy-lectures.org/advanced/scipy_sparse/coo_matrix.html)(首席运营官)
3.  节点的嵌入或数字表示

> 注意:对于节点的数字表示，我们可以使用图的属性，如度，或者使用不同的嵌入生成方法，如 [node2vec](https://github.com/eliorc/node2vec) 、 [DeepWalk](https://github.com/phanein/deepwalk) 等。在这个例子中，我将使用节点度作为它的数字表示。

让我们进入编码部分。

# 准备工作。

空手道俱乐部数据集可以直接从 NetworkX 库中加载。我们从图中检索标签，并以坐标格式创建边索引。节点度被用作节点的嵌入/数字表示(在有向图的情况下，入度可以用于相同的目的)。由于学位值往往是多种多样的，我们在使用这些值作为 GNN 模型的输入之前对它们进行归一化。

至此，我们已经准备好了构建 Pytorch 几何自定义数据集的所有必要部分。

# 自定义数据集。

*KarateDataset* 类继承自 *InMemoryDataset 类*，并使用一个数据对象来整理与空手道俱乐部数据集相关的所有信息。然后将图形数据分割成训练集和测试集，从而使用分割创建训练和测试掩码。

数据对象包含以下变量:

> *数据(edge_index=[2，156]，num_classes=[1]，test_mask=[34]，train_mask=[34]，x=[34，1]，y=[34])*

这个自定义数据集现在可以与 Pytorch 几何库中的几个图形神经网络模型一起使用。让我们挑选一个图卷积网络模型，并使用它来预测测试集上的缺失标签。

> 注意:PyG 库更侧重于节点分类任务，但它也可以用于链接预测。

# 图卷积网络。

GCN 模型由两个隐层构成，每个隐层包含 16 个神经元。让我们训练模型！

# 训练 GCN 模型。

随机超参数的初始实验给出了这些结果:

> 训练精度:0.913
> 测试精度:0.727

这并不令人印象深刻，我们当然可以做得更好。在我的下一篇文章中，我将讨论我们如何使用 [Optuna](https://optuna.org/#code_examples) (关于超参数调优的 python 库)来轻松地调优超参数并找到最佳模型。本例中使用的代码取自 PyTorch Geometric 的 GitHub 库，并做了一些修改([链接](https://github.com/rusty1s/pytorch_geometric/blob/master/examples/gcn.py))。

# 总结。

总结一下我们到目前为止所做的一切:

1.  为图中的每个节点生成数字表示(在本例中为节点度数)。
2.  构建一个 PyG 自定义数据集，并将数据分为训练和测试两部分。
3.  使用像 GCN 这样的 GNN 模型并训练该模型。
4.  对测试集进行预测，并计算准确度分数。

鸣谢:
这篇文章中的大部分解释都是我在 Cesson-sévigne 的 Orange Labs 实习期间学到并应用的概念。我研究过图形嵌入方法，也研究过可以应用于知识图的图形神经网络。

在接下来的部分中，我将更多地解释我们如何使用图嵌入作为初始节点表示来处理相同的节点分类任务。

感谢阅读，干杯！

```
**Want to Connect?**Reach me at [LinkedIn](https://www.linkedin.com/in/rohithteja/), [Twitter](https://twitter.com/rohithtejam), [GitHub](https://github.com/rohithteja) or just [Buy Me A Coffee](https://www.buymeacoffee.com/rohithteja)!
```