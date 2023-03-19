# 每个 ML 工程师都应该知道的 3 个 Keras 设计模式

> 原文：<https://towardsdatascience.com/3-keras-design-patterns-every-ml-engineer-should-know-cae87618c7e3?source=collection_archive---------6----------------------->

## 机器学习工程师也有自己的设计模式！

![](img/6bb62657c2fc42fdc4bf47a4909d5437.png)

照片由[爱丽丝·迪特里希](https://unsplash.com/@alicegrace?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/design?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

Keras 是一个建立在 TensorFlow 之上的机器学习(ML)库，使得创建和适应深度学习模型架构变得极其容易。正如他们喜欢说的，“Keras API 是为人类设计的，而不是为机器设计的。”

这是事实！如果你想快速勾画出一个想法，Keras 是这项工作的工具。然而，Keras 可以非常灵活和强大；你可以逐渐剥离复杂的层次，深入定制几乎每一个小细节。

此外，由于其 TensorFlow 支持的后端，Keras 可以利用多 GPU 配置或 TPU 训练。

在这个故事中，我们研究了三种主要的 Keras 设计模式。我们将首先构建一个简单的序列模型。然后我们将扩展我们的实现来支持非顺序用例。最后，我们将更深入地通过子类化 Keras `Model`类来创建和执行我们自己的训练循环。开始吧！

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=keras_design_patterns)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=keras_design_patterns)！

# 像箭一样直

为了展示 Keras 的威力，我们需要一个运行的例子。为此，我们将使用“ *Hello world* ”计算机视觉的例子，即 MNIST 数据集。我知道，我知道……MNIST 的问题现在很无聊，但是我们试图让事情变得简单，这样我们就可以专注于重要的事情:代码。

首先，我们将使用序列模型求解 MNIST。这再简单不过了。从现在开始，遵循本文的最佳方式是启动 Google Colab 笔记本，并将运行时改为支持 GPU 的运行时。

在第一个代码单元格中，提供导入语句:

然后，让我们加载并准备数据:

现在，我们准备构建我们的模型。Keras sequential API 可以毫不费力地构建一个深度学习模型，它的工作方式很像一条开放的高速公路；我们通过一系列直接的层来输入数据:

最后，您所要做的就是编译模型，拟合它，并在测试数据集上评估它。广义编译意味着提供损失函数、优化器和您关心的指标。完成这一步后，你就可以开始训练了:

仅此而已！经过 15 个时期后，我在测试数据集上获得了`0.9912`的准确度。不错，但我相信你不会感到惊讶。毕竟，MNIST 任务被认为是已经解决的。

# 功能 API

但是如果你不能用一系列的层来解决你的问题呢？如果您需要执行单独计算的分支，并且模型将它们的结果合并到一起，那该怎么办？然后，是时候使用 Keras 功能 API 了。

因此，我们将输入通过两个具有不同特性的不同卷积层，然后合并它们的输出。不，对于 MNIST 问题来说，这不是真的有必要，但我们会假装有必要。

为了实现这一切，我们只需要更新模型创建步骤:

经过 15 个时期后，我在测试数据集上获得了`0.9919`的准确度。然而，你不能真的说这个模型更好或更差。这只是一个玩具的例子！

# 子类

现在是时候拿出大枪了。一些工程师需要知道`fit`函数内部发生了什么，并控制一切。Keras 提供了子类化 API，您可以使用它创建自己的模型类，并完全定制训练步骤。

首先，我们需要创建数据集:

然后，创建我们的模型。这与我们在顺序 API 部分中使用的模型相同:

接下来，我们需要创建我们的培训和测试步骤:

最后，让我们将所有东西放在一起，构建我们的培训循环:

经过 15 个时期后，我在测试数据集上获得了`0.9910`的准确度。这是一个总结！

# 结论

Keras 是一个建立在 TensorFlow 之上的机器学习(ML)库，使得创建和适应深度学习模型架构变得极其容易。正如他们喜欢说的，“Keras API 是为人类设计的，而不是为机器设计的。”

Keras 提供了三种主要方法来解决您的 ML 任务:

*   顺序 API
*   功能 API
*   Keras 子类化方法

每一项技术都让我们对低层次张量流更深入一步。通常，顺序的和功能的 API 解决了我们 99%的问题，但是一个好的 ML 工程师应该了解每一个设计模式。

# 关于作者

我的名字是[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=keras_design_patterns)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请在 Twitter 上关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。