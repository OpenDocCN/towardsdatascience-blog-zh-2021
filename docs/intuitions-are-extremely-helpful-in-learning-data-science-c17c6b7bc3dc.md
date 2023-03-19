# 直觉对学习数据科学非常有帮助

> 原文：<https://towardsdatascience.com/intuitions-are-extremely-helpful-in-learning-data-science-c17c6b7bc3dc?source=collection_archive---------26----------------------->

## 意见

## 我非常喜欢 Francois Chollet 的 Python 深度学习

# 介绍

根据我的经验，在数学方面有两种人。例如，考虑针对这两类受众的主成分分析的两种解释:

*   **喜欢故事的人**:在 PCA 中，你可以旋转数据集的轴，以便更有效地描述你的数据

![](img/6919af6cace3a14433034602e638c7aa.png)

使用 PCA 旋转轴。图片作者。

*   **喜欢数学的人** : PCA 被定义为一种正交线性变换，它将数据转换到一个新的坐标系，这样数据的一些标量投影的最大方差就位于第一个坐标上([维基百科](https://en.wikipedia.org/wiki/Principal_component_analysis))

后者更符合传统的大学数学教育。你花了很多时间学习基本的数学技能，这些技能最终会叠加成更复杂、更有用的数学概念，比如深度神经网络。这种方法的优点是，它允许你真正深入地理解神经网络内部发生了什么。如果你的目标是研究新类型的架构，这种深刻的知识肯定是需要的。然而，这种学习数据科学的方法非常耗时，并且不适合每个学生。

和我一起工作的学生往往不太倾向于数学。他们的目标不是真正深入深度学习的本质，而是学会应用它来解决实际问题。根据我的经验，他们从基于故事和直觉的数学演示中受益匪浅。这些直觉更加模糊，但是为学生提供了很好的定位点来吸收新知识。此外，他们往往足以能够实际应用深度学习。

# 我为什么喜欢巧克力

一本提供一些极好的直觉的书是 Francois Chollet[写的](https://www.linkedin.com/in/fchollet/)[用 Python 进行深度学习](https://www.manning.com/books/deep-learning-with-python-second-edition)。我很喜欢他对深度学习的实用和平易近人的解释。您可能无法根据这本书从头开始编写 tensorflow，但它为您提供了使用 Keras 构建实用深度学习模型的知识。

让这本书与众不同的两点是经验法则和直觉。经验法则的例子是哪种损失函数用于哪种类型的数据，或者在隐藏层中使用多少个神经元。关于直觉，考虑一下书中的以下引文和我写的一个更数学化的句子:

> (Chollet)深度学习是一个从数据中学习表示的数学框架
> 
> 通过计算损失向量函数相对于网络权重的偏导数，并选择使损失函数最小化的权重变化，来训练神经网络的连续层

后一句要精确很多，但也需要你有很多先验知识(偏导数，损失函数)。Chollet 的前一段引用提供了神经网络如何试图解决问题的更多直觉。Chollet 将这些令人敬畏的直觉塑造成连贯的叙述的方式使得这本书读起来很有乐趣。

# 这对学习意味着什么？

对于初学深度学习的爱好者来说，直觉是建立知识库的好方法。直觉为你的学习过程提供了基础，更详细的知识有了嵌入的空间。特别是对于一个更注重实践的学生来说，这种更自上而下的学习方式被证明是非常有效的。

但我相信，这些直觉也可以被证明对那些采用更多数学方法进行深度学习的学生有益。与更注重实践的学生一样，直觉让你将数学概念嵌入到更广阔的背景中。这让你看到深度学习少了一系列的数学运算，更多的是作为一个整体的问题解决机器。

# 我是谁？

我叫 Paul Hiemstra，是荷兰的一名教师和数据科学家。我是科学家和软件工程师的混合体，对与数据科学相关的一切都有广泛的兴趣。你可以在 medium 上关注我，或者在 LinkedIn 上关注我。

如果你喜欢这篇文章，你可能也会喜欢我的其他一些文章:

*   [掌握数据科学并不是学习一系列技巧](/mastering-data-science-is-not-learning-a-series-of-tricks-df66d8529c29)
*   [学习 AI 机器人玩井字游戏系列文章](https://towardsdatascience.com/tagged/rl-series-paul)
*   [牛郎星图解构:可视化气象数据的关联结构](/altair-plot-deconstruction-visualizing-the-correlation-structure-of-weather-data-38fb5668c5b1)
*   [面向数据科学的高级函数式编程:使用函数运算符构建代码架构](/advanced-functional-programming-for-data-science-building-code-architectures-with-function-dd989cc3b0da)
*   [通过规范化扩展您的回归曲目](/expanding-your-regression-repertoire-with-regularisation-903d2c9f7b28)