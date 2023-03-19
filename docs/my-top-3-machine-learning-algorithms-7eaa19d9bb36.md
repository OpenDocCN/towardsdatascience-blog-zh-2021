# 我最喜欢的 3 种机器学习算法

> 原文：<https://towardsdatascience.com/my-top-3-machine-learning-algorithms-7eaa19d9bb36?source=collection_archive---------44----------------------->

## 意见

## 数据科学家现在和 2021 年最喜欢的算法

![](img/a18924e3543917c9c991d75e7ec81058.png)

戴维·冯迪马尔在[Unsplash](https://unsplash.com/s/photos/tesla?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄的照片。

# 目录

1.  介绍
2.  随机森林
3.  XGBoost
4.  CatBoost
5.  摘要
6.  参考

# 介绍

随着 2021 年的到来，我想讨论一下我最喜欢的三种机器学习算法的更新列表以及原因。在过去的一年里，我在业余时间通过自己研究和使用不同的算法获得了更多的专业经验和实践经验。新的用例、Kaggle 示例、视频和其他文章让我关注我最喜欢的三种算法，包括 Random Forest、XGBoost 和 CatBoost。这三者都有好处，你当然可以用这三者产生令人印象深刻的结果。虽然一个是老的和可靠的，另一个是强大的和有竞争力的，最后一个是新的和令人印象深刻的，这三个算法在我的列表中名列前茅，看看哪三个在你的列表中名列前茅将是有趣的。如果你想了解这三种著名的机器学习算法的更多信息，请继续阅读下面的内容。

# 随机森林

![](img/b75f8ac208cacf55a46b060eae1618f8.png)

由 [Sebastian Unrau](https://unsplash.com/@sebastian_unrau?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄的照片。

一种经过检验的真正的机器学习算法是[随机森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)【3】算法。它已被证明在各种情况和用例中表现良好。这个库的一个独特的好处是，因为它比较老，所以它比 XGBoost 和 CatBoost 拥有更多的文档和更多的例子。随机森林工作的一般方法是在子样本上拟合一些决策树，然后对它们进行平均。这有助于提高精度和防止过度拟合。当我第一次调查我的数据，执行特征工程，并希望有一个总体的准确性时，我倾向于使用这种算法。即使您使用随机森林作为最终数据科学模型的一部分，您仍然会看到一些令人印象深刻的结果，并且 XGBoost 或 CatBoost 实现可能没有必要。当然，您可以随时对所有三种算法进行测试，以查看哪种算法最适合您、您的数据和您的用例。

我特别喜欢用随机森林做分类问题，也叫监督学习。这种类型的学习是当你的数据上有标签。标签的一个例子是商店或电子商务平台的特定类型的服装(*，例如，裤子、t 恤、毛衣*)。你可以使用特征(*袖长、可能的尺寸、颜色等。*)描述服装，最终对新装进行训练和分类。在预测时，很容易使用`predict_proba`函数，该函数很容易显示某事物对其分类的可能性。

> 以下是随机森林的一些好处:

*   通用(*回归和分类*)
*   处理不平衡的数据
*   适用于异常值
*   并行处理(`n_jobs = -1`)
*   易于使用
*   学习、练习和理解算法的几种方法

总的来说，随机森林是数据科学和机器学习过程的一个很好的起点，也可能是一个很好的终点。但是，下面可能会有其他某些算法对于其他某些情况更好，或者出于其他原因(*基于你的限制*)。

# XGBoost

![](img/9409bbfb4163a00cc70603864ef3ce5b.png)

[罗马法师](https://unsplash.com/@roman_lazygeek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/math?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄的照片。

卡格尔赢家。

我不会深究这个问题，因为它比下一个算法讨论得更广泛。

[XG boost](https://xgboost.readthedocs.io/en/latest/)【5】经常被用在比赛中，在这些比赛中，最微小的精确度都是至关重要的。然而，这种强大的机器学习算法也可以在学校，在你的时间练习，或在工作中使用。这是一个梯度增强库，已被证明是非常准确和快速的。XGBoost 可能比 Random Forest 需要更长的时间，但是在我所看到的情况下，它在大多数时候都更准确。

> 以下是 XGBoost 的一些优点:

*   正规化
*   缺失值处理
*   交叉验证
*   并行处理
*   质量很好
*   特征重要性

# CatBoost

![](img/249a3dbe6c28c8504beb537949a65d5d.png)

照片由迪米特里·科艾曼斯在[Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上拍摄

CatBoost 机器学习算法是由[Yandex](https://catboost.ai/)【7】创建的，它在决策树上使用梯度推进。该算法是最新的算法之一，如果不是最新的*算法的话。它强大、健壮、准确，总体来说令人印象深刻。组成这个算法的主要类包括分类器和回归器，分别是`CatBoostClassifer`和`CatBoostRegreessor`。当然，CatBoost 的主要区别在于它强调分类变量的重要性。它使用一种目标编码的形式，其他包或算法也包括这种形式；然而，这种特定类型的目标编码要健壮得多。使用这种转换的好处是不仅可以加快训练和预测数据所需的时间，还可以获得更准确和有益的值。例如，使用 CatBoost，您不需要采用一次热编码，也不需要担心数百个稀疏列，而是可以专注于一个编码特性，该特性也可以防止目标泄漏。*

*我发现对 CatBoost 有用的两个主要特性重要性类型是`PredictionValuesChange`和`LossFunctionChange`类型。对于排名指标，您需要坚持使用 LossFunctionChange 方法。您还可以使用非常流行且易于使用的 SHAP 库来直观地解释特性的重要性。*

> *以下是 CatBoost 的优势:*

1.  *更快的计算*
2.  *快速预测*
3.  *防止过度拟合*
4.  *防止目标泄漏*
5.  *最适用于分类特征*
6.  *提高培训质量*
7.  *减少参数调整的时间*
8.  *更多关于可解释性的时间*
9.  *不同类型的特征重要性*
10.  *也利用 SHAP 价值观*
11.  *嵌入了大量有益且令人印象深刻的情节*

*CatBoost 本身讨论的主要特性是无需参数调整的高质量、分类特性支持、快速和可扩展的 GPU 版本、提高的准确性和快速预测。他们还提到了它在搜索、推荐系统、天气预报等方面的广泛应用。到目前为止，我真的相信这个机器学习算法是最好的。看看一年或几年后会出现什么来与强大而准确的机器学习算法 CatBoost 竞争，这将是一件有趣的事情。*

# *摘要*

*总的来说，有几个机器学习算法提供了很大的好处。它们都比较容易实现，同时还能产生有竞争力和高质量的结果。我知道这些算法中的一些可能对你来说并不陌生，所以我希望我还是增加了对至少一种算法的认识。虽然 Random Forest 是最老的，但 XGBoost 一直在主导 Kaggle 竞争，我相信 CatBoost 是最好的——对于分类数据科学用例来说。*

> *以下是我现在和 2021 年最喜欢的三种机器学习算法:*

```
*Random ForestXGBoostCatBoost*
```

**我希望你觉得这篇文章既有用又有趣，请随时讨论你使用的机器学习算法。哪些是你最喜欢的？你同意还是不同意我的观点？为什么或为什么不？**

**感谢您的阅读！**

*请随意查看我在'`The Top 5 Machine Learning Algorthims` ' [8]中的另一篇文章，这篇文章深入探讨了这些算法中的两个以及其他三个算法的用例:*

*</the-top-5-machine-learning-algorithms-53bc471a2e92>  

# 参考

[1]照片由 [David von Diemar](https://unsplash.com/@davidvondiemar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/tesla?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[2]Sebastian Unrau 在 [Unsplash](https://unsplash.com/s/photos/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2015)

[3] scikit-learn 开发者，[随机森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)，(2007-2020)

[4]罗马法师在 [Unsplash](https://unsplash.com/s/photos/math?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

[5] xgboost 开发商， [XGBoost](https://xgboost.readthedocs.io/en/latest/) ，(2020)

[6]Dimitry Kooijmans 在 [Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)

[7] Yandex， [CatBoost](https://catboost.ai/) ，(2021)

[8 ] M.Przybyla，[五大机器学习算法](/the-top-5-machine-learning-algorithms-53bc471a2e92)，(2020)*