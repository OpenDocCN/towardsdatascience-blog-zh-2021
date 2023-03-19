# 数据科学基础课程

> 原文：<https://towardsdatascience.com/data-science-101-91b9f852a01c?source=collection_archive---------24----------------------->

## 意见

## 每个用例的一步一步的过程

![](img/e68dd8500d76d11220d9d722173bf764.png)

[绿色变色龙](https://unsplash.com/@craftedbygc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  问题陈述
3.  数据收集
4.  探索性数据分析
5.  特征工程
6.  模型比较
7.  结果讨论
8.  摘要
9.  参考

# 介绍

所有的技术流程都有一定的趋势，数据科学也不例外。随着你在任何工作中获得越来越多的经验，你开始注意到一种趋势，这种趋势会使工作变得容易一些。本文的目标是使您的数据科学工作更加简化，因为我将在下面概述的过程适用于每一个数据科学用例(*或至少大多数*)，对于那些不是 100%适用的用例，它仍然有希望对您有用。正如您将看到的，这个过程有六个主要步骤——主要针对模型的开发部分。它不一定用于将模型部署到生产环境中。这个过程强调了从您试图解决的问题到使用数据科学技术解决该问题的步骤。如果您想了解更多关于数据科学过程的六个步骤，请继续阅读下面的内容。

# 问题陈述

![](img/ae5e9d94cffdf159967b7a8f1545f6f2.png)

照片由[艾米丽·莫特](https://unsplash.com/@emilymorter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/question?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

要构建数据科学模型或利用机器学习算法，您需要了解问题是什么。这一步也可以称为更符合“*业务用例*”的东西。在这一步，你最有可能体验到与利益相关者一起工作，从数据分析师、业务分析师、产品经理到公司的高级管理人员。

> 下面是一个糟糕的问题陈述的例子:

*   *“我们想预测 2022 年有多少人会购买我们的产品”*

> 下面是一个很好的问题陈述示例:

*   *“目前预测销量的方式不准确”*

虽然第一个例子有道理，但它没有突出问题，而是突出了一个可能的解决方案。重点首先应该是以最简单的形式理解问题。在此基础上，我们可以使用数据科学技术和模型提出可能的解决方案。

问题陈述的另一部分可以是定义目标的过程。例如，询问当前的销售预测准确度是多少，目标准确度是多少，以及模型是否能够达到目标准确度，以及不能完全达到目标准确度意味着什么，这将非常有用。

# 数据收集

![](img/9ee10d34aec537faf5a52cc8a4455ca5.png)

托拜厄斯·菲舍尔在[Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄的照片。

关于本文中描述的整体数据科学过程，数据收集过程可能是从学术界到专业环境最远的一步。举个例子，在教育课程中，你可能马上得到一个已经被处理和研究过的数据集。对于工作环境或专业设置，您必须学习如何从外部来源或数据表中的内部来源获取数据。这一步可能需要相当长的时间，因为您需要浏览数据库中或跨数据库的几乎所有数据表。您最终获得的数据可能会使用不同来源的不同数据。最终数据最终将被读入数据帧，以便对其进行分析、训练和预测。

> 以下是一些获取数据的可能方法:

*   来自 Google Sheets
*   从 CSV 文件
*   来自 Salesforce
*   JSON 文件
*   数据库表
*   从其他网站
*   还有更多

# 探索性数据分析

![](img/b97161918c08146ae8efb7797c633854.png)

照片由[Firmbee.com](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄。

数据科学过程中的这一步通常可以遵循相同的格式。此时，您将拥有主要的单一数据框架。出于数据科学问题的考虑，你需要将你的`X`特征与你的`y`目标变量——你试图预测的东西——分开。这些要素可以从一个到数百个，甚至更多，但最好从简单开始，首先分析数据集的主要要素(*您直觉上认为对模型预测有重要意义的要素*)，然后大致了解所有要素。

您可以查看各种有助于定义您的数据的描述性统计数据，以下是一些描述您的数据的更简单、更常用的方法——通常使用`pandas`库:

*   `df[[‘feature_1’, ‘feature_n’]].head()` —数据的前 5 行
*   `df[[‘feature_1’, ‘feature_n’]].tail()` —数据的最后 5 行
*   `df[[‘feature_1’, ‘feature_n’]].describe()`-计数、平均值、标准差、最小值、25%、50%、75%、最大值-让您对数据和特定特征的分布有一个很好的了解
*   分析缺失的数据—有时这是意料之中的
*   数据异常
*   错误数据—不应为负值的负值，等等。

如果您想要一个大的快捷方式，您可以使用`pandas profiling`，它显示了关于您的数据框架的所有这些描述性统计数据，并且在短短几行代码中显示了更多内容[5]:

[](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/) [## 简介-熊猫-简介 2.12.0 文档

### 从熊猫生成档案报告。熊猫的 df.describe()函数很棒，但是对于严肃的……

pandas-profiling.github.io](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/) 

# 特征工程

![](img/f6c3205eeb0c3222da0a6c00e2347030.png)

由[杰斯温·托马斯](https://unsplash.com/@jeswinthomas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/math?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上拍摄的照片。

既然您已经浏览了数据，您可能想要设计您的要素。对我来说，该过程的这一步应该被称为“模型特征工程之前的*”——因为你不用模型来编辑你的特征。也就是说，您可以通过多种方式来使用您的功能。其中一些方法包括通过简单地将两个要素分割在一起来创建新要素，以及将要素组合在一起以创建聚合要素。*

> 以下是特征工程的一些例子:

*   加法/减法/除法等。创建新的功能
*   对要素进行分组以创建聚合要素
*   一个热编码
*   分类变量的目标编码，以减少数据框架的维数

# 模型比较

![](img/953ab85813c6f69300be9bfa9884d8eb.png)

罗伯特·阿纳施在[Unsplash](https://unsplash.com/s/photos/compare?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【7】上拍摄的照片。

如您所见，在开始讨论主要的'*数据科学*'部分之前，我们已经执行了几个步骤。在这一部分中，无论您是执行回归还是分类，在选择一个模型作为您的最终模型进行更新和增强之前，最好对几个模型进行比较。

例如，尽管为您的用例选择特定的机器学习算法似乎是显而易见的，但最好消除您的偏见，并获得一个基线，比如说 5 到 10 个常用算法。从那里，你可以比较每一个的好处，而不仅仅是准确性。例如，您可能希望将训练模型所需的`time`或它可能达到的`expensive`与数据的`transforming`要求进行比较。

> 以下是一些常见的数据科学模型比较方法:

*   手工编写几个算法，并创建一个表格或数据框架，并排显示彼此的优缺点
*   PyCaret
*   我倾向于用最基本的方法来比较模型，这样我就不会过多地使用某个特定的算法，以防我最终不使用它

# 结果讨论

![](img/46e02fa1faec94121192d9f60464d843.png)

照片由[阿里·萨阿达特](https://unsplash.com/@camsaadat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/error?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【9】上拍摄。

在将您的模型实现到产品中之前，您需要与您的利益相关者讨论结果。你会看到你的精度意味着什么，或者你的损失度量，比如 RMSE——均方根误差。这些结果经常让不学习或不使用数据科学的人感到困惑，所以你的工作是使它们尽可能简单，以便利益相关者可以从你的结果中做出决定-例如，继续或不继续(*有时复杂的机器学习算法不是问题的答案*)。

> 在讨论结果时，请记住以下几点:

*   除了原始结果之外，使用可视化工具显示结果——使用 Tableau、Google Data Studio 等工具，或者在 Python 中创建自己的结果
*   按某些要素进行聚合，以突出显示误差较高或较低的地方，或者精度较高或较低的地方
*   解释如果有更多的数据、更多的时间或不同的算法，你会怎么做
*   解释这些结果对公司的业务和财务方面意味着什么——这种模式是省钱了，还是只是创建和运行需要钱？
*   你的模型使过程更快，更好吗？
*   它仅仅帮助内部用户还是帮助你公司产品的客户？

将数据科学模型纳入公司的生态系统时，有很多问题需要讨论。也就是说，最后一步是通过将模型投入生产来实现自动化。

# 摘要

这些主要步骤对大多数数据科学项目都很重要。之后发生的步骤通常涉及更多的机器学习操作——这意味着，现在你的模型被批准了，你可以通过让软件工程师、UX/UI 研究人员、更多的产品经理和统计学家参与 A/B 测试来将其实现到产品中。既然模型使用起来很有意义，那么你应该问的一个重要问题是，你将如何使用它？

*   *例如，你的模式是否会引起客户的负面或正面反应？*

正如您所看到的，在公司整合数据科学时，有许多事情需要考虑，但遵循这六个主要步骤可以为使用数据科学有效解决问题设定一个简单的大纲和攻击计划。

*总结一下，以下是你可以应用于每个数据科学用例的六个步骤:*

```
* Problem Statement* Data Collection* Exploratory Data Analysis* Feature Engineering* Model Comparison* Results Discussion
```

我希望你觉得我的文章既有趣又有用。如果您遵循数据科学实施的主要流程，请随时在下面发表评论，您同意还是不同意，您将删除或添加哪些步骤？你认为如果你在将来实施这个过程会有好处吗？

*请随时查看我的个人资料和其他文章，也可以通过 LinkedIn 联系我。我不隶属于上述任何公司。*

# 参考

[1]照片由[绿色变色龙](https://unsplash.com/@craftedbygc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2015)上拍摄

[2]照片由 [Emily Morter](https://unsplash.com/@emilymorter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/question?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2017)

[3]Tobias Fischer 在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)

[4]照片由[Firmbee.com](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2015)上拍摄

[5]熊猫简介—西蒙·布里格曼，[熊猫简介主页](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/)，(2021)

[6]照片由[杰斯温·托马斯](https://unsplash.com/@jeswinthomas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/math?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄

[7]罗伯特·阿纳施在 [Unsplash](https://unsplash.com/s/photos/compare?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)

[8] [Moez Ali](https://medium.com/u/fba05660b60f?source=post_page-----91b9f852a01c--------------------------------) ， [PyCaret 主页](https://pycaret.org/)，(2021)

[9]照片由[阿里·萨阿达特](https://unsplash.com/@camsaadat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/error?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)拍摄