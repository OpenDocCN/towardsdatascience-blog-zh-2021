# 合成数据的新进展

> 原文：<https://towardsdatascience.com/the-new-step-forward-in-synthetic-data-dc854319166d?source=collection_archive---------28----------------------->

## 如何使用 GANs 改进您的数据

![](img/a6dc0559ec8b09d28d8567c739db5232.png)

照片由 [Unsplash](https://unsplash.com/) 上的西提·拉赫马纳·马特·达乌德拍摄

在您的数据科学职业生涯中，您迟早会遇到一个问题，其中一个事件(通常是您试图预测的事件)比另一个或其他事件发生的频率低。

毕竟现实就是这样——车祸或者有疾病的人更稀缺(谢天谢地！)比汽车或健康人完成的轨迹。

这类问题被归类为不平衡数据。而且，虽然没有一个数字来定义它，但是当你的类分布是偏斜的时候，你知道你的数据是不平衡的。

此时你可能会想，如果我的数据代表现实，那么这是一件好事。嗯，你的机器学习(ML)算法不敢苟同。

我不打算深究与不平衡分类相关的问题的细节(如果你想了解更多这个特定的主题，你可以在这里阅读)但是请耐心听我说一会儿:

想象一下，你的 ML 算法需要“看到”健康患者 1000 次才能识别出什么是健康患者，不健康患者和健康患者的比例是 1:1000。但是你希望它也能识别疾病患者，所以你也需要“喂”他 1000 个不健康的患者。这意味着您实际上需要有一个包含 1000000 名患者的数据库，以便您的 ML 算法有足够的信息来识别这两种类型的患者。

旁注:这仅仅是一个例子，在引擎盖下事情并不完全是那样发生的。

我敢打赌，现在你已经开始明白这个问题是如何迅速扩大的了。

值得庆幸的是，我们最亲爱的统计学家朋友们已经找到了帮助我们解决这个问题的方法。

事实上，自 20 世纪 30 年代以来，已经出现了几种方法，每种方法都有自己的用例，从排列测试到 bootstrap，有很多选项。

如果你不是数据科学的新手，你可能已经在模型训练过程中应用了一些重采样技术，比如交叉验证。

自举是当今最常见的方式之一，它包括:

> bootstrap 背后的想法很简单:如果我们用替换数据中的重新采样点**，我们可以将重新采样的数据集视为我们在平行宇宙中收集的新数据集。**
> 
> — [马诺吉特·南迪](https://blog.dominodatalab.com/imbalanced-datasets/)

为此，我们需要:

> 假设每个观察值都是从总体中随机选取的。换句话说，任何观察都同样可能被选择，并且它的选择是独立的。
> 
> — [影响点](https://influentialpoints.com/Training/bootstrap_confidence_intervals-principles-properties-assumptions.htm)

这取决于特定的自举方法。

然而，这可能会导致重复的值，这在这个[视频](https://www.youtube.com/watch?v=isEcgoCmlO0)中有所解释。

由于我们正在尝试添加更多少数类的示例，毕竟我们已经有了足够多的多数类，重复值不会给模型带来额外的信息。

那么如果我们能合成新的例子呢？

这正是 SMOTE，**合成少数过采样技术的首字母缩写，**所做的。

SMOTE 可能是最广泛使用的合成新示例的方法，它利用 [KNN](https://www.youtube.com/watch?v=HVXime0nQeI) 聚类算法来:

> 选择特征空间中接近的示例，在特征空间中的示例之间画一条线，并在沿着该线的一点处画一个新的样本。
> 
> — [杰森·布朗利](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/)

然而，在生成新数据时，我们只查看少数类，这意味着我们否决了多数类可能产生的影响，如果存在类的重叠，最终可能会产生模糊的示例。

不仅如此，由于我们正在使用聚类，维度和变量类型可能会成为一个问题，这增加了准备步骤的难度，以实现良好的结果。

可以找到以某种方式解决这些问题的几种变体(我建议看一看 [scikit-learn 文档](https://imbalanced-learn.org/stable/references/over_sampling.html)以更好地理解如何处理这些问题),但是它们都有一个共同点，它们都做出假设。

如果您可以在没有任何假设的情况下生成新数据，会怎么样？

这就是最新技术发挥作用的地方:**生成对抗网络，**或简称 GANs。

如果你从未听说过它们，请花时间在这里查看它们是什么。

但是如果你时间不够，或者只是想简单地自己看看 GANs 能做什么(如果你只是停下来看视频:是的，你刚刚看到了不存在的人)。

令人震惊吧。

但是，我在这里要告诉你的不是 GANs 是什么，而是它们如何为你产生新的数据(毕竟图像只是数据)！

因为它们抽象了假设部分，因为它们是不受监督的，所以它们能够检测新的看不见的模式，从而在生成的数据中增加了更大的可变性。同时能够处理更大的规模。

不仅如此，它们还允许在数据准备步骤中有更大的自由度，因为它们有相当多的不同架构可以适应您自己的用例。

好了，现在你可能会问自己，我该如何使用它们呢？

好吧，让我们从:

```
pip install ydata-synthetic
```

没错，我们来自 [Ydata](https://ydata.ai/) 的亲爱的朋友在 [Github](https://github.com/ydataai/ydata-synthetic) 上发布了一个开源工具来帮助你实现这一点。

不仅如此，他们还在 Slack 上添加了一个[合成数据社区](https://syntheticdatahq.slack.com/join/shared_invite/zt-qzpwpfp2-QD2Tzsx6TQsh~ieKqXYnZw#/shared-invite/email)，在那里你可以谈论这个话题，并提出关于这个工具的问题。

安装后，您可以选择更适合您的数据的架构，并开始享受乐趣:

使用定时器的[示例创建要点](https://colab.research.google.com/github/ydataai/ydata-synthetic/blob/master/examples/timeseries/TimeGAN_Synthetic_stock_data.ipynb#scrollTo=Mk5hsfeEYub3)

瞧，你有了新的数据可以处理，最终有了一个平衡的数据集可以“输入”到你的 ML 算法中。

因此，每当你面临数据不平衡的情况时，停下来想一想哪种解决方案更适合你的需求，然后着手平衡它。

您必须记住，生成合成数据并不是不平衡数据的神奇解决方案:

> 如果目标类不平衡，但仍有足够的代表性，重采样可以提高模型性能。在这种情况下，问题真的是缺乏数据。重采样随后会导致过拟合或欠拟合，而不是更好的模型性能。
> 
> — [玛莉亚·威德曼](https://www.kdnuggets.com/2020/12/resampling-imbalanced-data-limits.html)

最后，要知道它的极限和用途，小心负责地使用它。

如果你想讨论或了解更多关于这个话题，我强烈推荐你加入[合成数据社区。](https://syntheticdatahq.slack.com/join/shared_invite/zt-qzpwpfp2-QD2Tzsx6TQsh~ieKqXYnZw#/shared-invite/email)

页（page 的缩写）S: Ydata 所有者允许我在本文中使用他们的例子。

**其他来源**:

1.  [重采样方法](https://us.sagepub.com/sites/default/files/upm-assets/57234_book_item_57234.pdf)。