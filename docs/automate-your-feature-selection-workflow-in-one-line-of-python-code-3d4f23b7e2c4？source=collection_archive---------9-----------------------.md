# 在一行 Python 代码中实现要素选择工作流的自动化

> 原文：<https://towardsdatascience.com/automate-your-feature-selection-workflow-in-one-line-of-python-code-3d4f23b7e2c4?source=collection_archive---------9----------------------->

## 使用 Featurewiz 快速选择特征

![](img/f9a7791db9600fea7b83f79ae501df13.png)

图片来自[阿雷克索查](https://pixabay.com/users/qimono-1962238/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1690423)来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1690423)

就实例数量而言，更多的训练数据会产生更好的数据科学模型，但这不适用于特征数量。真实世界的数据集有许多要素，其中一些对于训练稳健的数据科学模型非常有用，而其他一些则是会影响模型性能的冗余要素。

特征选择是数据科学模型开发工作流的一个重要元素。选择所有可能的特征组合是多项式解决方案。数据科学家使用各种特征选择技术和技巧来移除冗余特征。

> 阅读[这篇文章](/top-7-feature-selection-techniques-in-machine-learning-94e08730cd09)，了解 [7 个特征选择技巧](/top-7-feature-selection-techniques-in-machine-learning-94e08730cd09)。

在本文中，我们将重点介绍如何使用开源 Python 包 Featurewiz 实现特征选择工作流的自动化。

# Featurewiz:

**Featurewiz** 是一个开源库，用于从数据集中创建和选择最佳特征，这些特征可进一步用于训练稳健的数据科学模型。Featurewiz 还提供了功能工程功能。只需点击一下代码，它就可以创建数百个新功能。Featurewiz API 有一个参数`**‘feature_engg’**`，可以设置为`**‘interactions’**`、`**‘group by’**`、`**‘target’**`，它会一口气创建上百个特性。

功能工程或创建新功能不仅仅是 featurewiz 的功能。它可以减少特征的数量，并选择最佳的特征集合来训练鲁棒的模型。

## Featurewiz 是如何工作的？

Featurewiz 使用两种算法从数据集中选择最佳要素。

*   苏洛夫
*   递归 XGBoost

## 苏洛夫:

SULOV 代表 ***搜索不相关的变量列表*** ，非常类似于 [mRMR 算法](https://en.wikipedia.org/wiki/Minimum_redundancy_feature_selection)。SULOV 算法遵循的步骤如下:

1.  计算所有超过阈值的相关变量对。
2.  相对于目标变量计算 MIS(交互信息得分)。
3.  比较每一对相关变量，并移除具有低 MIS 的特征。
4.  其余特征具有高的 MIS 和低的相关性。

## 递归 XGBoost:

在 SULOV 算法选择具有高 MIS 和对数相关性的最佳特征集之后，使用重复 XGBoost 算法来计算剩余变量中的最佳特征。步骤如下:

1.  为剩余的要素集构建数据集，并将其分为训练和验证两部分。
2.  使用验证计算列车上的前 10 个特征。
3.  每次使用不同的功能集重复步骤 1 和 2。
4.  组合所有 10 个特征的集合，并对它们进行去重复，这将产生最佳的特征集合。

Featurewiz 使用上面讨论的两种算法来寻找最佳的特征集，该特征集可以进一步用于训练健壮的机器学习模型。

# 安装和使用:

Featurewiz 可以使用 Pypl 安装

```
**pip install featurewiz**
```

安装后，可以导入 featurewiz:

```
**from featurewiz import featurewiz**
```

现在，开发人员只需要编写一行代码，就可以从数据集中获得最佳的功能集。

```
**out1, out2 = featurewiz(dataname, target, corr_limit=0.7, verbose=0,   sep=",", header=0, test_data="", feature_engg="", category_encoders="")**
```

Featurewiz 不仅可以处理具有一个目标变量的数据集，还可以处理具有多标签目标变量的数据集。从 featurewiz 返回的数据框包含最佳的要素集，可用于模型训练。开发者不需要指定问题的类型，如果是回归或分类，特性可以自动决定它。

# 结论:

在本文中，我们讨论了一个开源库 featurewiz，它可以自动选择数据集的特性。除了功能选择，featurewiz 还具有执行功能工程和生成数百个功能的能力，只需点击一下代码。
Featurewiz 使用两种算法(SULOV 和递归 XGBoost)来选择最佳的特性集。Featurewiz 只需点击一行代码即可完成整个功能选择，从而加快了数据科学家的工作流程。数据科学家可以使用几种特性选择技术来筛选出最佳特性，您可以在下面提到的文章中阅读其中的 7 种特性选择技术。

[](/top-7-feature-selection-techniques-in-machine-learning-94e08730cd09) [## 机器学习中的 7 大特征选择技术

### 选择最佳功能的流行策略

towardsdatascience.com](/top-7-feature-selection-techniques-in-machine-learning-94e08730cd09) 

# 参考资料:

[1] Featurewiz GitHub 回购:[https://github.com/AutoViML/featurewiz](https://github.com/AutoViML/featurewiz)

> 感谢您的阅读