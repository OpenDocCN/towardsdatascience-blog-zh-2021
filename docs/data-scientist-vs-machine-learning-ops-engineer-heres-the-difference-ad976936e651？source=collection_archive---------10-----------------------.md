# 数据科学家 vs 机器学习运营工程师。区别就在这里。

> 原文：<https://towardsdatascience.com/data-scientist-vs-machine-learning-ops-engineer-heres-the-difference-ad976936e651?source=collection_archive---------10----------------------->

## 意见

## 深入探讨 Data Scientist 和 MLOps 的异同。

![](img/87ac61cbe5b84cd21975aa49100d9eb0.png)

[LinkedIn 销售导航员](https://unsplash.com/@linkedinsalesnavigator?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/two-people-working?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  数据科学家
3.  机器学习运营工程师
4.  异同
5.  摘要
6.  参考

# 介绍

虽然我写过关于数据科学和机器学习工程角色的文章，但我想比较数据科学家和机器学习运营工程师(通常被称为 MLOps 工程师)的具体职位。机器学习本身可以非常广泛，因此，一个新的职业出现了，它只专注于操作，而不是算法本身背后的研究。具有讽刺意味的是，数据科学家比 MLOps 工程师更关注机器学习算法。你甚至可以说 MLOps 工程师是传统意义上的软件工程师，他在整个数据科学流程中加入了部署和生产部分的专业化。我将更深入地探讨这两个职位，所以如果你想了解这两个杰出职业之间的共同差异和相似之处，请继续阅读。

# 数据科学家

![](img/05c5b18327f623153b7f9c3dc2e5b917.png)

由[Á·阿尔瓦罗·伯纳尔](https://unsplash.com/@abn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/white-board?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄的照片。

随着数据科学领域出现更多专业化，数据科学家的角色也变得越来越广泛。因此，你可以期待我的经历包含一些与你或你的期望不同的东西。然而，我研究了各种形式的数据科学，并希望给出该职位的最佳概述，以及将其与 MLOps 角色区分开来的关键点。数据科学家最好被描述为专注于业务的科学家，他用机器学习算法研究、发现和解决公司内部的问题。你的主要目标通常是使一个过程与以前相比更加准确和高效。这个定义听起来非常类似于软件工程的角色，然而，这个职位当然更关注算法和它们如何作为解决方案工作，而不是更多的手工制作和面向对象的编程代码解决方案。

> 以下是数据科学家在他们公司可能会经历的几天到几个月的一般流程:

1.  探索贵公司的数据和产品
2.  会见之前已经确定了企业内部或外部棘手问题的利益相关者
3.  想出一个业务问题陈述，突出手头的问题
4.  以某种方式获取数据，通常是使用 SQL 或者与数据工程师一起从其他新的来源中提取新的数据
5.  对您选择的数据集执行探索性数据分析
6.  将几个模型与基准模型进行比较
7.  选择您的主要算法
8.  识别关键特性—特性工程
9.  删除多余和不必要的功能
10.  可能创建整体或逐步算法过程
11.  考虑异常值
12.  保存您的模型并在开发环境中测试
13.  提供准确性或误差指标的详细信息
14.  展示你能为公司节省多少，以及你如何能让产品变得更好

有时，我在上面列出的数据科学过程中的这些步骤可能会不时发生变化。你要记住，所有的业务都是不同的，但如果你遵循一个数据创建、算法比较、测试和结果呈现的流程，你将成为你公司的伟大数据科学家。

现在，第 14 步之后会发生什么？这就是下一个角色发挥作用的地方。但是，也请记住，不是每个公司都能够负担得起或认为有必要在数据科学家旁边配备一名 MLOps 工程师。但是，如果您足够幸运，有一个更完整的数据科学团队，您可以专注于上面的那些步骤，而 MLOps 工程师可以专注于我将在下面描述的后续步骤。当然，如果你能两者兼而有之，那就太好了，很多人都身兼两职。虽然有时，如果数据科学家更专注于机器学习算法，而 MLOps 工程师专注于您的数据科学模型的部署、管道和生产化，这可能会更有益。当您不必担心将实际模型实现到您的业务、电话应用程序和公司软件的其他部分的软件工程繁重方面时，它可以帮助您提高模型的准确性或减少其错误度量。

# 机器学习运营工程师

***物流工程师***

![](img/9c1cc69bc074de5d350151663c6c9c7e.png)

照片由[杰佛森·桑多斯](https://unsplash.com/@jefflssantos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄。

机器学习运营工程师这一角色，通常被称为 MLOps 工程师，对于您的数据科学团队来说非常重要且有益。如果你目前是一名软件工程师，并且希望跨职能工作，特别是机器学习算法，或者你可能是一名了解算法如何工作的数据科学家，但希望更多地关注软件工程、数据工程和模型部署，你可能会发现自己正在转换到这一角色。当你作为一名 MLOps 工程师工作时，你可能首先会得到一个由数据科学家开发的数据科学模型。你还将研究如何优化一些数据科学代码，因为你将更加专注于软件工程，具有更加面向对象的编程经验(*同样，这种经验对你或你的公司来说可能是不同的，但总体而言，我已经看到 MLOps 更加精通编程)*。

> 因此，总的来说，作为一名 MLOps 工程师，您可以期望与数据科学家合作，通过数据工程和 DevOps 工具的实践来连接您公司软件中从测试到生产的差距。

> 下面是一个 MLOps 工程师在他们公司可能会经历几天到几个月的一般过程:

1.  研究所使用的机器学习算法的一般概念( *s*
2.  了解业务问题和数据科学解决方案
3.  了解模型需要多长时间训练、测试和部署一次
4.  你会做多少次预测，什么时候做？
5.  但更重要的是(*数据科学家也可以处理第 2-4 步*)，看看你如何利用你的专业知识来自动化整个工作流程
6.  此外，在您公司的应用程序或软件中实现模型(*)例如，每小时将模型结果插入一个表，该表显示在消费者看到的用户界面中*
7.  最终使用数据存储和 OOP 改进等数据工程技术优化模型本身
8.  致力于版本控制(*如 Git/GitHub* )和训练/预测的监控
9.  代码库创建或效率

如您所见，整个数据科学流程的一些步骤在数据科学家和 MLOps 工程师之间有所重叠。他们经常紧密合作，以实现公司运行有益的机器学习算法或数据科学模型的最终目标。

# 异同

![](img/6f02e848d02422446db9d38e59acbf85.png)

由[维罗妮卡·贝纳维德斯](https://unsplash.com/@vivalaveronica?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/similar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄的照片。

你可以预期这些角色会因公司而异，但是，总的来说，这两个职位之间有一些关键的相似之处和不同之处，它们几乎可以应用于任何地方。如果您能够同时执行这两项任务，那就太好了，但在某种程度上，可扩展的方法需要数据科学和 MLOps 工程两者协同工作，而不是一个人来完成。

> 这是两个角色之间的相似之处

*   双方都需要了解业务、问题和解决方案(*至少是高层次的概述*)
*   两者都需要很好地了解公司的数据，如果需要的话，还需要去哪里寻找更多的数据
*   两人通常都精通 SQL 和 Python
*   两者通常都使用 Git 和 GitHub ( *版本控制和存储库*)
*   两者都需要了解培训和测试的概念

> 以下是这两种角色之间的不同之处

*   数据科学家通常在他们的 Jupyter 笔记本或类似的东西上工作或开发
*   数据科学家往往更注重研究，而…
*   MLOps 专注于生产就绪代码和编程
*   MLOps 与 Docker 和 CircleCi 等 DevOp 工具一起工作
*   以及 AWS/EC2、Google Cloud 或 Kubeflow
*   MLOps 更倾向于面向对象
*   数据科学家必须知道实际的机器学习算法是如何工作的(*，例如梯度下降、正则化、参数调整等。)*
*   数据科学家专注于选择和创建算法(*)，例如，它是有监督的、无监督的、回归的还是分类的？*)
*   学校教育/教育是不同的。通常，为数据科学家提供数据科学硕士学位，为 MLOps 工程师提供软件工程学士学位— *越来越多的本科学校正在开设数据科学学士学位。*
*   在认证或其他形式的短期教育经历方面，为数据科学家提供软件工程专业，为 MLOps 提供机器学习专业也是有益的，这样两种角色都更加全面，可以更好地合作。

以上只是数据科学家和机器学习运营工程师的一些关键异同。还有更多，如果你目前正处于这些角色中的一个，你可能会经历一些与我描述的相似或不同的事情。总的来说，如果你处于一个不同的角色，并且在这两者之间做出选择，你就不会出错。这最终取决于你的喜好和你擅长什么。例如，我会将数据科学总结为统计学、机器学习和业务分析，对于机器学习运营工程，我会将角色总结为软件工程、机器学习部署专业知识、数据工程和 DevOps 的组合。数据科学家专注于手头的算法，而 MLOps 致力于算法的部署和自动化。

# 摘要

如您所见，这两个角色需要一些不同的技能，并有不同的目标，但同时，他们共享大量的技能，最终主要目标是相同的。与 MLOps 相比，我个人更喜欢数据科学家的角色，但是，我发现自己每天都在学习越来越多的 MLOps 工具和实践。我确实努力去了解一个 MLOps 人员所知道的一切，这样我就可以慢慢变得更有效率(*而且很有趣*)。这两个职位对公司都非常重要，所以如果你想对公司产生积极的影响，那么追求其中一个职位将是一个好主意。

> 总的来说，这里总结了两种角色

```
**Data Scientists:** business analysis, research, data, statistics, and Machine Learning algorithms**MLOps Engineer:** programming, Software Engineering, productionaliztion, DevOps, and automation
```

*我希望你觉得这篇文章既有趣又有用。*请记住，这篇文章是基于我对这两个角色的看法和个人经历。如果你不同意或同意，请在下面随意评论为什么以及你想补充的具体内容。你更喜欢做数据科学家，还是更喜欢做 MLOps 工程师？你认为他们应该合并成一个角色吗？真的有区别吗？从其他人那里获得一些见解将是有趣的，这样每个人都可以向其他人学习，以便找出数据科学和机器学习运营工程之间的异同的最佳代表。T *感谢您的阅读，欢迎随时查看我的个人资料或阅读其他文章，如果您对其中任何一篇有任何问题，请联系我。*

> 这是我写的另一篇文章，涉及[数据科学与商业分析](/data-scientist-vs-business-analyst-heres-the-difference-f0a0a1f51c9) [5]:

[](/data-scientist-vs-business-analyst-heres-the-difference-f0a0a1f51c9) [## 数据科学家与业务分析师。区别就在这里。

### 他们有多大不同？他们有什么共同的特点？

towardsdatascience.com](/data-scientist-vs-business-analyst-heres-the-difference-f0a0a1f51c9) 

# 参考

[1]照片由 [LinkedIn 销售导航员](https://unsplash.com/@linkedinsalesnavigator?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/two-people-working?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄

[2]照片由[Á·阿尔瓦罗·贝尔纳尔](https://unsplash.com/@abn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/white-board?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[3]Jefferson Santos 在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)

[4]照片由[维罗妮卡·贝纳维德斯](https://unsplash.com/@vivalaveronica?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/similar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2017)拍摄

[5] M.Przybyla，[数据科学家与业务分析师。区别就在这里](/data-scientist-vs-business-analyst-heres-the-difference-f0a0a1f51c9)，(2020