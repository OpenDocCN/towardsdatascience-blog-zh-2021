# 如何以 2021 年的方式进行图像分类

> 原文：<https://towardsdatascience.com/how-to-do-image-classification-the-2021-way-5962030e48b4?source=collection_archive---------32----------------------->

## 从型号选择到微调

![](img/9525c35f96e60d4330efce4d18c0f211.png)

[GR Stocks](https://unsplash.com/@grstocks?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

图像分类是计算机视觉中最古老的问题，第一个网络是 [AlexNet](/alexnet-8b05c5eb88d4) ，最新的是 [EfficientNetv2](https://arxiv.org/pdf/2104.00298.pdf) 。如今，只要点击一下鼠标，就可以获得所有最先进的模型，测试每一个模型，然后选择最好的一个，就成了一项艰巨的任务。我们将涵盖从型号选择困境到微调狂热的一切。

# 型号选择

虽然有基于变换器的模型可用，但卷积网络仍然有需求，因为变换器的计算简单，并且它们在图像域中的长期使用已经导致了一些强有力的实践。让我们看看我们唯一的选择。

## 效率网

虽然可以有许多选项(ResNet 及其变体等。)，上述模型体系结构与所有其他体系结构不同，因为可以根据您的需要选择各种主干。

从 B0 到 B7，EfficientNet 有 8 种类型，您可以将其视为超参数。此外，它们在 [Tensorflow](https://www.tensorflow.org/api_docs/python/tf/keras/applications/efficientnet) 和 [PyTorch](https://github.com/rwightman/pytorch-image-models) 中均有提供。

**高效网络 B3/B4** 是一个很好的起点。

如果你有必要的计算，你也可以把这些**主干当作一个超参数**。

除此之外，许多 [Kaggle 竞赛](https://github.com/mrgloom/Kaggle-Computer-Vision-Competitions-List)已经广泛证明了它们是一个很好的架构。

# 模型微调

对于微调，应使用以下设置。

**学习率** : 3e-5
**优化器**:亚当
**批量** : 16

## 学习率

当你微调一个模型时，你不需要很大的学习率。即使这样也会影响表演。3e-5 是一个足够低的学习率，可以在微调时给你很好的增益。你也可以看看一个学习率调度器(像[余弦退火和热重启](https://pytorch.org/docs/master/generated/torch.optim.lr_scheduler.CosineAnnealingWarmRestarts.html)等等。).

如果你仍然不接受这个学习速度，试试使用 fastai 的 [lr_finder](https://fastai1.fast.ai/callbacks.lr_finder.html) 功能。

## 【计算机】优化程序

自 2014 年问世以来， [Adam](https://arxiv.org/abs/1412.6980) optimizer 的使用量一直高居榜首。

还有其他的选择，比如[前瞻亚当](https://arxiv.org/abs/1907.08610)、[游侠](https://github.com/lessw2020/Ranger-Deep-Learning-Optimizer)、[山姆](https://arxiv.org/abs/2010.01412)等等。这将需要你找到新的更好的设置，但上面的那些将在那里工作太漂亮了。

## 批量

由于在 EfficientNet 和几乎所有其他图像分类模型中使用批处理规范化，使用批处理大小≥ 16 对于良好的性能变得必不可少(参见此处的)。

对于批量，越大越好。虽然，并不是每个人都有这样的计算能力(在一些大型网络中，即使是 16 个批量也是很困难的)。在这种情况下，尽可能保持较高的批量。

# 失败

有一件事会彻底改变你的结果，那就是失败。对于图像分类，有哪些选项？[交叉熵](https://www.tensorflow.org/api_docs/python/tf/keras/losses/CategoricalCrossentropy)是图像分类的首选损失，但它有一个问题。当你有类不平衡时，用交叉熵可能得不到最好的结果。

几乎总是会收到类别不平衡的数据集，因此，应该使用的损失是 [**焦点损失**](https://www.tensorflow.org/addons/api_docs/python/tfa/losses/SigmoidFocalCrossEntropy) 。它有一个超参数 *gamma，*，决定对样本数量较少的类的关注程度。

# 增大

除了类别不平衡之外，你将面临的另一件事是图像数量的减少(在实际用例中，这将是 90%的时间)。为此，你需要考虑增强。毫无疑问，最好的图像增强库是[albuminations](https://github.com/albumentations-team/albumentations)。

但是，在此之前，请对数据中的图像进行广泛的分析，并选择库中可用的最佳增强(有很多！).

# 结论

计算机视觉领域仍在发展，但随着时间的推移，一些实践已经发展，一些模式已经出现。我根据上面的经验总结了那些模式。希望这有助于快速得到一个好的模型。

祝一切顺利，神经忍者们。