# 每个数据科学家都应该知道的 4 种使用情形

> 原文：<https://towardsdatascience.com/4-use-cases-every-data-scientist-should-know-49dccd4d533f?source=collection_archive---------29----------------------->

## 意见

## 如何处理常见机器学习算法问题的示例

![](img/2857a65174bfc73294e5c3c2d038c3b3.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business-person?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  信用卡欺诈检测
3.  客户细分
4.  客户流失预测
5.  销售预测
6.  摘要
7.  参考

# 介绍

如果您是一名资深的数据科学家，您可能已经看到了其中的一些用例，但是，如果您是相当新的，这些用例可以让您实践各种数据科学概念，这些概念可以进一步应用于多个行业。不幸的是，数据科学用例通常不会在公司中很快得到很好的开发，相反，用例将根据项目的需求和期望经过几次会议来构建。重要的是要有一种通用用例的感觉，这种感觉可以被调整并应用到更新的用例中，因为有时，你会遇到文章中没有提到或大学没有研究过的全新场景。然而，数据科学的美妙之处在于，它是可扩展的，并且只需要相对较少的工作量就可以应用于多个问题。话虽如此，让我们检查四个用例，您可以直接应用到您的工作中，也可以调整用于未来的应用程序，包括模型的可能功能，以及所使用的算法本身。

# 信用卡欺诈检测

![](img/7a639f5bc57e2a782da1725b1def09da.png)

埃弗里·埃文斯在[Unsplash](https://unsplash.com/s/photos/credit-card?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍照。

在本例中，我们将构建一个受监督的模型，其分类为`fraud`或`no fraud`。理想情况下，在你的数据集中，你会有很多欺诈**看起来像什么**而**看起来不像**的例子。下一步是获取或创建几个描述欺诈行为以及正常行为的特征，这样算法就可以轻松区分这两种标签。

> 以下是您可以在**随机森林**算法中使用的功能:

*   货币金额
*   频率
*   位置
*   日期
*   交易描述
*   交易类别

> 以下是在获得训练和测试数据集后可以使用的示例代码:

```
RF = RandomForestClassifier()RF.fit(X_train, y_train)predictions = RF.predict(X_test)
```

你可以从几个功能开始，然后努力构建新的功能，例如，`aggregates`或`per`功能(*例如:每天花费的钱，等等。*)

# 客户细分

![](img/4942edfa912496ef64afd96b24778ea9.png)

照片由[粘土堤](https://unsplash.com/@claybanks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[un splash](https://unsplash.com/s/photos/customer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄。

与上面的例子相反，这个场景将使用无监督学习，而不是分类，使用聚类。典型的聚类算法是 **K-Means** 。这个问题不受监督的原因是因为您没有标签，并且您不知道要对什么进行分组，但是您会希望根据新组的共享特征来找到新组的模式。在本例中，使用该模型的具体原因是，您想要找到购买某些产品的人的模式。这样，你就可以为这些客户设计一个有针对性的营销活动。

> 以下是您可以在 K 均值算法中使用的可能特征:

*   购买的产品
*   他们的位置
*   产品或商家位置
*   消费频率
*   产品工业
*   教育
*   收入
*   年龄

> 以下是获得数据和要素后可以使用的示例代码:

```
kmeans = KMeans(
         init="random",
         n_clusters=6
         )kmeans.fit(X)predictions = kmeans.fit_predict(X)
```

这种算法通常用于电子商务、商品销售以及任何有客户数据和营销的行业。

# 客户流失预测

![](img/6531eb3f8e4d883b3b3c3259d9d3b2ae.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/angry-customer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上的照片。

这种情况可以受益于各种机器学习算法。这个问题也类似于信用卡欺诈检测问题，我们希望收集带有预定义标签的客户的特征，特别是`churn`或`no-churn`。您可以再次使用**随机森林**，或者不同的算法，例如 **XGBoost** 。因此，这个场景是一个使用监督学习的分类问题。我们将预测网站上购买了一种或多种产品的用户的客户流失。

> 以下是您可以在 XGBoost 算法中使用的功能:

*   登录金额
*   日期功能(*月、周等。*
*   位置
*   年龄
*   产品历史
*   产品多样性
*   产品使用时间长度
*   产品使用频率
*   登录时间
*   客户通过电子邮件发送给客户服务的金额
*   客户与聊天机器人聊天的数量
*   如果他们提到产品

这些特征可以表明某人是长期用户还是短期用户。一些功能，如*推荐*，将清楚地表明他们是否喜欢该产品，而*产品多样性*在分类中可能是任何一种方式，例如，如果他们购买了四种不同的产品，但确实多次使用它们。

> 以下是获得数据和要素后可以使用的示例代码:

```
model = XGBClassifier()model.fit(X_train, y_train)predictions = model.predict(X_test)
```

# 销售预测

![](img/0ea0d8334311ca8440fcfa8749dde6a7.png)

照片由 [M. B. M.](https://unsplash.com/@m_b_m?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】拍摄。

也许与前三个用例最不同的是预测销售。在这个例子中，我们可以使用 ***深度学习*** 来预测某个产品的未来销量。使用的算法被称为 **LSTM** ，代表长短期记忆。

> 以下是您可以在 LSTM 算法中使用的功能:

*   日期
*   制品
*   商人
*   销售额

> 以下是获得数据和要素后可以使用的示例代码:

```
model = Sequential()model.add(LSTM(4, batch_input_shape=(1, X_train.shape[1], X_train.shape[2])))model.add(Dense(1))model.compile(loss='mean_squared_error')model.fit(X_train, y_train)predictions = model.predict(X_test)
```

# 摘要

本文展示了常见算法的常见用例，还涵盖了您可以使用数据科学解决的各种特定类型的问题。例如，我们研究了监督和非监督学习，以及另一种算法亚型，它使用深度学习进行预测。尽管这些都是特定的用例，但无论您的行业是医疗保健还是金融，您都可以将这些特性(*或*中的子不同特性)和 Python 代码应用于您业务中的大多数数据科学问题。

> 总而言之，我们比较了涵盖各种算法的数据科学的四个用例:

```
* Credit Card Fraud Detection — using Random Forest* Customer Segmentation — using K-Means* Customer Churn Prediction — using XGBoost* Sales Forecasting — using LSTM
```

我希望你觉得我的文章既有趣又有用。如果您在这些用例中使用了机器学习算法，请随时在下面发表评论。你使用了不同于我举例的算法吗？为什么或为什么不？您还可以将其他哪些可以从示例算法中受益的用例组合在一起？

请随时查看我的个人资料和其他文章，也可以通过 LinkedIn 联系我。感谢您的阅读！

# 参考

[1]图片由 [Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/business-person?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2018)

[2]埃弗里·埃文斯在 [Unsplash](https://unsplash.com/s/photos/credit-card?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2020)

[3]照片由[粘土银行](https://unsplash.com/@claybanks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[un splash](https://unsplash.com/s/photos/customer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)拍摄

[4]图片由 [Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/angry-customer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2018)

[5]照片由 [M. B. M.](https://unsplash.com/@m_b_m?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)上拍摄