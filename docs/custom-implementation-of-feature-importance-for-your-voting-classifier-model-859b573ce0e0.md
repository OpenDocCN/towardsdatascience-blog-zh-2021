# 投票分类器模型的特征重要性的自定义实现

> 原文：<https://towardsdatascience.com/custom-implementation-of-feature-importance-for-your-voting-classifier-model-859b573ce0e0?source=collection_archive---------13----------------------->

## 与其他模型不同，Scikit-learn 包缺少投票分类器的功能重要性实现

![](img/52ccdba90a62c5ed163e27deeb5a68e9.png)

图片由 Arek Socha 提供，来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1495858)

机器学习模型越来越多地应用于复杂的高端环境，如金融技术、医学科学等。尽管利用率有所提高，但仍然缺乏解释该模型的技术。模型的可解释性越高，人们就越容易理解结果。有各种先进的技术和算法来解释模型，包括石灰，SHAP 等。

特征重要性是为估计者解释特征重要性的最简单和最有效的技术。特征重要性有助于更好地解释估计量，并通过使用特征选择来改进模型。有多种技术可以计算评估者的特征重要性分数:

*   sci kit-了解特性重要性的内置实现
*   用排列法计算特征重要性
*   使用 SHAP 值计算的要素重要性

在本文中，我们将进一步讨论第一个特性重要性策略。Scikit-Learn 包提供了一个函数`**model.feature_importances_**` 来计算除投票分类器估计器之外的大多数估计器的特征重要性。Scikit-learn 使用不同的算法来计算特征重要性分数。对于随机森林或决策树模型，它使用基尼系数计算重要性分数，对于逻辑回归，它使用向量权重。

在本文中，我们将讨论计算投票分类器模型的重要性分数的实现。

# 什么是投票分类器？

投票分类器是一种集成技术，它将各种模型的预测组合在一起，根据它们的最高概率预测输出(类别)。投票分类器算法只是汇总传递到模型中的每个分类器的结果，并根据投票的最高多数预测输出类。

投票分类器支持两种投票技术:

*   硬投票:投票分类器估计器的预测输出将基于最高多数的投票来计算，即具有被每个分类器预测的最高概率的类别。
*   软投票:输出类是基于该类的平均概率的预测。

> 为了更好地理解投票分类器，请阅读穆巴拉克·加尼尤的一篇文章:

[](/how-voting-classifiers-work-f1c8e41d30ff) [## 投票分类器如何工作！

### 用于增强分类的 scikit-learn 功能

towardsdatascience.com](/how-voting-classifiers-work-f1c8e41d30ff) 

# 功能重要性:

与其他模型估计器不同，Scikit-learn 没有提出投票分类器的特征重要性实现。但是用于投票分类器的每个基本估计器都有其实现方式。其思想是基于权重组合每个估计器的重要性分数。

(作者代码)

计算投票分类器的特征重要性的算法步骤是:

1.  计算每个基本评估者的特征重要性分数
2.  将基本估计值的权重乘以每个特征的重要性分数。
3.  平均每个特性的特性重要性分数(来自步骤 2)。

# 结论:

在本文中，我们讨论了一种技术或方法来计算投票分类器集成模型的特征重要性分数。特征重要性分数有助于理解模型中最重要的特征，并且可以通过使用特征选择来改进模型。

# 参考资料:

[1]来自 Scikit-Learn 的投票分类器文档:[https://sci kit-Learn . org/stable/modules/generated/sk Learn . ensemble . Voting Classifier . html](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)

> 感谢您的阅读