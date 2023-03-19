# 你的深度学习模型好吗？

> 原文：<https://towardsdatascience.com/is-your-deep-learning-model-any-good-ee3807dd5b57?source=collection_archive---------34----------------------->

## 一个新的深度学习框架，使得在 PyTorch 中构建基线变得简单

![](img/a66ef3e9aa93d7a60d8958298a034b88.png)

图片由[格尔德·奥特曼](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=949943)来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=949943)

说你是研究员；你对一个全新的神经网络架构或一个看起来奇怪的优化步骤有一个创新的想法，这有可能成为深度学习领域的突破。你应该如何测试你的方法？

另一方面，假设你是一个机器学习工程师，你想测试你建立的模型是否有意义。这是你能做的最好的吗？选择一个现成的、经过实战检验的算法并部署它不是更好吗？

> **Lightning Flash:一个新的深度学习框架，使得在 PyTorch 中构建基线变得微不足道。真的！**

答案不应该令人惊讶:你需要设定一个*基线*。在学术界，基线通常是关于特定任务和数据集的最新发布的结果(假设这设置了新的艺术状态)。在行业中，基线在很大程度上是特定于领域的。

然而，没有人愿意花费太多的时间来实现基线。如果你有一些快速的东西来集中你的时间迭代你的方法，那将是最好的。**这就是我们今天要讨论的:** `**lightning-flash**` **一个新的框架，让在 PyTorch 中构建基线变得微不足道！**

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=flash-lightning)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# 电闪

![](img/1500e617ce941ad7ef3caafa35562f51.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=828652) 的[自由照片](https://pixabay.com/photos/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=828652)

闪电侠围绕着*任务的想法。*到目前为止，框架附带的*任务*有助于解决图像分类、图像嵌入、文本分类和摘要、表格分类和翻译的挑战。更多的即将到来，当然，你可以随时建立自己的。

所以，我们假设你要解决[膜翅目](https://hymenoptera.elsiklab.missouri.edu/)图像分类挑战，预测某张图像中是有蚂蚁还是有蜜蜂。正如我们已经看到的，首先，我们需要一个基线。怎么做？让我们从工具箱中取出*图像分类任务*并敲钉子。

在要求模型进行预测之前，我们需要在数据集上对其进行微调。这对于`lightning-flash`来说非常简单:

现在，我们已经准备好获取模型对测试数据的预测。让我们将 3 张蚂蚁的图片输入到我们微调过的模型中。我们期望得到相同的类，或者是`0`或者是`1`，这取决于哪个索引被用作‘ant’类的符号。

事实上，我的终端上的结果是这样的:

```
[0, 0, 0]
```

如果我们想进一步测试这一点，我们也可以添加蜜蜂的图像，或者指示我们的模型用整个文件夹来生成预测:

几分钟后，我们得到了基线。最重要的是，记住闪电是无限可扩展的；您可以用几行代码构建自己的*任务*，适应自己的架构，将训练扩展到多个 GPU，并采用最佳实践，如混合精度训练！

# 入门指南

最好的开始方式是阅读[文档](https://lightning-flash.readthedocs.io/en/latest/)。但是，如果您使用 VS 代码并安装了 Docker，我可以让事情变得更简单:

1.  安装`[remote-containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)`扩展
2.  `git clone [https://github.com/dpoulopoulos/medium.git](https://github.com/dpoulopoulos/medium.git)`
3.  导航至`lightning_flash`示例文件夹
4.  启动命令面板(`ctlr + shif + p`)并键入`Reopen Folder in Container`

几分钟后，您将获得一个开发环境，其中包含测试`lightning-flash`所需的一切。阅读下面关于`remote-containers`扩展以及如何使用它进行开发的更多内容。

[](/the-only-vs-code-extension-you-will-ever-need-e095a6d09f24) [## 你唯一需要的 VS 代码扩展

### 如果您必须安装一个 Visual Code Studio 扩展，这就是它！

towardsdatascience.com](/the-only-vs-code-extension-you-will-ever-need-e095a6d09f24) 

# 结论

无论你是研究人员还是工程师，你都需要强大的基线来取得进展。然而，建立基线不应该花费你一整天的时间。

基于 PyTorch 和 PyTorch Lightning 构建的新深度学习框架 Lightning Flash 使这个*任务*变得微不足道。让我们看看我们能用它来建造什么！

# 关于作者

我的名字是[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=flash-lightning)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=medium&utm_medium=article&utm_campaign=flash-lightning)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！