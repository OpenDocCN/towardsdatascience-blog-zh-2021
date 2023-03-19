# 每个初学数据的科学家都应该掌握的 6 个预测模型

> 原文：<https://towardsdatascience.com/6-predictive-models-models-every-beginner-data-scientist-should-master-7a37ec8da76d?source=collection_archive---------2----------------------->

## 数据科学模型有不同的风格和技术——幸运的是，大多数高级模型都基于几个基本原理。当你想开始数据科学家的职业生涯时，你应该学习哪些模型？这篇文章给你带来了 6 个在行业中广泛使用的模型，或者以独立的形式，或者作为其他先进技术的构建模块。

![](img/b5bd677b67e82a9ef132796748d44b50.png)

照片由[@ barn images](https://unsplash.com/@barnimages)—unsplash.com 拍摄

当你陷入机器学习和人工智能的炒作漩涡时，似乎只有先进的技术才能解决你想要建立预测模型的所有问题。但是，当你接触到代码时，你会发现事实是非常非常不同的。作为一名数据科学家，您将面临的许多问题都可以通过几种模型的组合来解决，其中大多数模型已经存在了很长时间。

而且，即使你使用更高级的模型来解决问题，学习基础知识也会让你在大多数讨论中领先一步。特别是，了解更简单模型的优点和缺点将有助于您成功指导数据科学项目。事实是:高级模型能够做两件事——**放大或修正它们所基于的简单模型的一些缺陷。**

话虽如此，**让我们跳进 DS 的世界，了解当你想成为一名数据科学家时应该学习和掌握的 6 个模型**。

# 线性回归

最古老的模型之一(例如，[弗朗西斯·高尔顿在 19 世纪使用术语“回归”](https://en.wikipedia.org/wiki/Regression_analysis´))仍然是使用数据表示线性关系的最有效的模型之一。

研究线性回归是世界各地计量经济学课堂上的一个主要内容，学习这个线性模型会让你对解决回归问题(用 ML 解决的最常见问题之一)有一个很好的直觉，还会让你明白如何使用数学建立一条简单的线来预测现象。

学习线性回归还有其他好处——特别是当您学习了可获得最佳性能的两种方法时:

*   [封闭解](/normal-equation-in-python-the-closed-form-solution-for-linear-regression-13df33f9ad71)，一个近乎神奇的公式，用一个简单的代数方程给出变量的权重。
*   [梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)，一种向最佳权重发展的优化方法，用于优化其他类型的算法。

此外，我们可以在实践中使用一个简单的二维图来可视化线性回归，这一事实使该模型成为理解算法的一个良好开端。

了解它的一些资源:

*   [DataCamp 的线性回归解释](https://www.datacamp.com/community/tutorials/essentials-linear-regression-python)
*   [Sklearn 的回归实现](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
*   [R 为数据科学 Udemy 课程线性回归部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

# 逻辑回归

虽然名为回归，逻辑回归是开始掌握分类问题的最佳模型。

学习逻辑回归有几个好处，即:

*   初步了解分类和多分类问题(ML 任务的很大一部分)。
*   理解函数变换，如 Sigmoid 函数所做的变换。
*   理解梯度下降的其他函数的用法，以及优化函数是如何不可知的。
*   对数损失函数初探。

学习逻辑回归后，你应该期望知道什么？你将能够理解分类问题背后的机制，以及如何使用机器学习来区分类别。属于这一类的一些问题:

*   了解交易是否是欺诈性的。
*   了解客户是否会流失。
*   根据违约概率对贷款进行分类。

就像线性回归一样，逻辑也是一种线性算法——在研究了这两种算法之后，您将了解线性算法背后的主要限制，以及它们为何无法表示许多现实世界的复杂性。

了解它的一些资源:

*   [数据营 R 解释中的逻辑回归](https://www.datacamp.com/community/tutorials/logistic-regression-R)
*   [Sklearn 的逻辑回归实现](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
*   [R 适用于数据科学 Udemy 课程—分类问题部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

# 决策树

首先要研究的非线性算法应该是决策树。决策树是一个基于 if-else 规则的简单易懂的算法，它将让你很好地理解非线性算法及其优缺点。

决策树是所有基于树的模型的构建块——通过学习它们，您还将准备好学习其他技术，如 XGBoost 或 LightGBM(下面将详细介绍)。

最酷的部分是决策树适用于回归和分类问题，两者之间的差异最小-选择影响结果的最佳变量背后的基本原理大致相同，您只需切换标准即可-在这种情况下，是误差度量。

**尽管你有回归的超参数概念(如正则化参数)，但在决策树中它们是极其重要的**，能够在好的和绝对垃圾的模型之间画出一条线。超参数在您的 ML 之旅中将是必不可少的，决策树是测试它们的绝佳机会。

关于决策树的一些资源:

*   [LucidChart 决策树解释](https://www.lucidchart.com/pages/decision-tree)
*   [Sklearn 的决策树讲解](https://scikit-learn.org/stable/modules/tree.html)
*   [我关于分类决策树的博文](/6-things-you-should-learn-to-kickstart-your-natural-language-processing-skills-4e10a1d3d2a?sk=a4231696321577dfcf563f532a69d542)
*   [数据科学 Udemy 课程的 R—基于树的模型部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

# 随机森林

由于它们对超参数的敏感性和相当简单的假设，决策树的结果相当有限。当你研究他们的时候，你会明白他们真的倾向于过度拟合，创造出不适合未来的模型。

**随机森林的概念真的很简单——如果决策树是独裁，那么随机森林就是民主。**它们有助于在不同的决策树之间实现多样化，这有助于为您的算法带来鲁棒性——就像决策树一样，您可以配置大量的超参数来增强这种 Bagging 模型的性能。**什么是装袋？**ML 中为不同模型带来稳定性的一个非常重要的概念——您只需使用平均值或投票机制将不同模型的结果转换为单一方法。

在实践中，随机森林训练固定数量的决策树，并(通常)对所有这些先前模型的结果进行平均——就像决策树一样，我们有分类和回归随机森林。如果你听说过群体智慧的概念，bagging models 将这个概念应用到 ML models 培训中。

了解随机森林算法的一些资源:

*   [饶彤彤关于随机森林的中帖](/understanding-random-forest-58381e0602d2)
*   [Sklearn 的随机森林分类器实现](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
*   [数据科学 Udemy 课程的 R—基于树的模型部分](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?couponCode=MEDIUMREADERS)

# XGBoost/LightGBM

其他给他们带来稳定性的基于决策树的算法是 XGBoost 或 LightGBM。这些模型是助推算法，它们处理以前弱学习者犯下的错误，以找到更鲁棒和更好地概括的模式。

这种关于机器学习模型的思想在 [Michael Kearns 关于弱学习者和假设检验的论文](https://www.cis.upenn.edu/~mkearns/papers/boostnote.pdf)之后获得了关注，它展示了增强模型可能是模型遭受的整体偏差/方差权衡的一个极好的解决方案。此外，这些模型是应用于 [Kaggle 竞赛的一些最受欢迎的选择。](/xgboost-lightgbm-and-other-kaggle-competition-favorites-6212e8b0e835)

XGBoost 和 LightGBM 是 Boosting 算法的两个著名实现。了解它们的一些资源:

*   [微软的 Lightgbm GitHub 页面](https://github.com/microsoft/LightGBM)
*   [Pranjal Khandelwal](https://www.analyticsvidhya.com/blog/author/pranjalk7/) [关于 XGBoost vs. LightGBM 的文章](https://www.analyticsvidhya.com/blog/2017/06/which-algorithm-takes-the-crown-light-gbm-vs-xgboost/)
*   [Vishal Morde 关于 XGBoost 的中帖](/https-medium-com-vishalmorde-xgboost-algorithm-long-she-may-rein-edd9f99be63d)

# 人工神经网络

最后，当前预测模型的圣杯——人工神经网络(ann)。

人工神经网络是目前发现数据中非线性模式并在自变量和因变量之间建立真正复杂关系的最佳模型之一。通过学习它们，你将接触到激活函数、反向传播和神经网络层的概念——这些概念将为你研究深度学习模型提供良好的基础。

此外，就其架构而言，神经网络有许多不同的风格——研究最基本的风格将为跳转到其他类型的模型奠定基础，如递归神经网络(主要用于自然语言处理)和卷积神经网络(主要用于计算机视觉)。

了解它们的一些额外资源:

*   [IBM《什么是神经网络》文章](https://www.ibm.com/cloud/learn/neural-networks)
*   [Keras(神经网络实现和抽象)文档](https://keras.io/)
*   [桑奇特·坦瓦尔关于建立你的第一个神经网络的文章](/building-our-first-neural-network-in-keras-bdc8abbc17f5)

而且，就是这样！这些模型应该会让你在数据科学和机器学习方面有一个良好的开端。通过学习它们，你将为学习更高级的模型做好准备，并轻松掌握这些模型背后的数学。

好的一面是，更高级的东西通常基于我在这里介绍的 6 个模型，所以了解它们的底层数学和机制永远不会有坏处，即使是在你需要带“大枪”的项目中。

你觉得少了点什么吗？写在下面的评论里，我很想听听你的意见。

***我在一个*** [***Udemy 课程***](https://www.udemy.com/course/r-for-data-science-first-step-data-scientist/?referralCode=6D1757B5E619B89FA064) ***里开设了一门学习这些模型的课程——这门课程适合初学者，我希望你能在我身边。***

<https://ivopbernardo.medium.com/membership> 