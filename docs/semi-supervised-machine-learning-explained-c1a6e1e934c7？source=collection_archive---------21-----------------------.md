# 半监督机器学习解释

> 原文：<https://towardsdatascience.com/semi-supervised-machine-learning-explained-c1a6e1e934c7?source=collection_archive---------21----------------------->

## 机器学习的另一种方式

![](img/a8323120504b116f42bd59b3417fd95f.png)

约翰·汤纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

机器可以通过各种方式学习。监督学习是一种机器学习问题，涉及基于示例输入-输出对学习输入到输出的映射函数。无监督学习涉及从未标记数据中学习模式。半监督学习可以被视为监督学习和非监督学习的混合。

本质上，当我们在训练的时候把少量的有标签的数据和大量的无标签的数据结合起来，我们就有了一个半监督的机器学习问题。根据维基百科，半监督学习可能被描述为弱监督的特例[ **来源** : [维基百科](https://en.wikipedia.org/wiki/Semi-supervised_learning) ]。

> 弱监督是机器学习的一个分支，它使用有噪声、有限或不精确的源来提供监督信号，以便在监督学习环境中标记大量训练数据— [维基百科](https://en.wikipedia.org/wiki/Weak_supervision)。

## 数据问题

监督学习模型和技术在商业中很常见。然而，建立有效的模型高度依赖于获得高质量的标记训练数据——我们都听说过“垃圾进，垃圾出”的说法。当企业试图使用机器学习来解决问题时，对高质量标记数据的需求通常会导致一个主要障碍。这个问题表现在几个方面:

*   **标记数据数量不足** —当提出新产品或新行业出现时，他们面临的一个常见问题是缺乏标记训练数据来应用传统的监督学习方法。通常，数据科学家会获得更多数据，但这种情况下的问题是，如果不等待时间的流逝来积累数据，这样做可能不切实际、成本高昂或不可能。
*   **没有足够的领域专业知识来标记数据** —有些问题可以被任何人标记。例如，大多数人可以标记猫和狗的图像，但当标记图像中猫或狗的品种时，这个问题变得更具挑战性。让领域专家来标记训练数据可能会很快变得昂贵，因此它通常不是一个可行的解决方案。
*   **没有足够的时间来标记和准备数据** —众所周知，60%或更多的时间花在机器学习问题上，专门用于准备数据集。当在处理快速发展的问题的领域中工作时，收集和准备数据集以足够快地构建有用的解决方案可能是不切实际的。

总的来说，收集高质量的标记数据会很快对一个人的资源提出沉重的要求，一些公司可能没有这些资源来满足需求。这就是半监督学习发挥作用的地方。如果我们有少量的标记数据和大量的未标记数据，那么我们可以将我们的问题框架为半监督机器学习问题。

## 半监督学习的类型

当我们在处理一个半监督问题时，目标会根据我们希望执行的学习类型而变化。我们可以将半监督学习用于归纳学习、直推式学习或两者的对比。

**归纳学习**

归纳学习的目标是归纳新数据。因此，归纳学习指的是建立一种学习算法，从标记的训练集学习并推广到新数据。

**直推式学习**

直推式学习的目标是将来自已标记训练数据集的信息转换成可用的未标记(训练)数据。

## 最后的想法

半监督学习介于监督学习和非监督学习之间。我们可以将半监督学习作为归纳或直推学习来进行，并且这两种学习都可以使用半监督学习算法来执行。其实现方式超出了本文的范围，但是感兴趣的读者可能想阅读:

*   [半监督学习](http://www.acad.bg/ebook/ml/MITPress-%20SemiSupervised%20Learning.pdf)
*   [半监督学习简介](https://www.amazon.co.uk/Introduction-Semi-Supervised-Synthesis-Artificial-Intelligence/dp/1598295470)

感谢阅读！

如果你喜欢这篇文章，请通过订阅我的免费**[每周简讯](https://mailchi.mp/ef1f7700a873/sign-up)与我联系。不要错过我写的关于人工智能、数据科学和自由职业的帖子。**

## **相关文章**

**[](/the-difference-between-classification-and-regression-in-machine-learning-4ccdb5b18fd3) [## 机器学习中分类和回归的区别

### 函数逼近问题

towardsdatascience.com](/the-difference-between-classification-and-regression-in-machine-learning-4ccdb5b18fd3) [](/unsupervised-machine-learning-explained-1ccc5f20ca29) [## 无监督机器学习解释

### 它是什么，方法和应用

towardsdatascience.com](/unsupervised-machine-learning-explained-1ccc5f20ca29) [](/5-books-you-can-read-to-learn-about-artificial-intelligence-477b5a26277d) [## 你可以阅读的关于人工智能的 5 本书

### 跟上时代

towardsdatascience.com](/5-books-you-can-read-to-learn-about-artificial-intelligence-477b5a26277d)**