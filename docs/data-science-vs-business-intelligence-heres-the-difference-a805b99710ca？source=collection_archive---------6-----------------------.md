# 数据科学与商业智能:区别在于

> 原文：<https://towardsdatascience.com/data-science-vs-business-intelligence-heres-the-difference-a805b99710ca?source=collection_archive---------6----------------------->

## 意见

## 这两个受欢迎的技术角色之间的异同

![](img/311a2d610a366dc4269a58241ee07547.png)

照片由[克里斯蒂娜@ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/office?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄。

# 目录

1.  介绍
2.  数据科学
3.  商业智能
4.  摘要
5.  参考

# 介绍

在数据科学工作突出之前是商业智能领域。虽然他们以前可能是非常相似的职位，但随着这两个角色变得越来越受欢迎，角色也变得更加明确。话虽如此，仍然需要注意的是，根据你最终的工作地点，这些职位之间可能会有相当多的重叠。我将讨论我从自己的职业生涯以及真实的工作描述中看到的差异和相似之处。如果你想了解数据科学和商业智能职业的主要定义特征，以及它们之间的比较，请继续阅读。

# 数据科学

![](img/1202e4d44616c7ed16e9475b6bf6e1b9.png)

照片由 [Fitore F](https://unsplash.com/@daseine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/robot?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

从我自己的经历以及在其他公司的面试，到查看工作描述，我汇编了一份对数据科学的总结性解释，我将在下面描述它，以及它与商业智能的不同和相似之处。

> 以下是数据科学职业的主要概念、技能和期望:

还包括一些示例工具——但不仅限于这些技能。

*   与利益相关者一起开发用例及问题陈述
*   从各种来源获取数据
*   使用`SQL`和`Python`创建数据集
*   探索性数据分析(`*with* Pandas *usually*`)和特征工程
*   模型探索和比较(`scikit-learn`、`TensorFlow`、T14、、T5)
*   最终模型
*   部署
*   讨论结果和影响

如您所见，这只是作为一名数据科学家的一小部分期望，然而，它可以很好地代表任何数据科学家将执行的主要过程。如果你只看上面的第一点和最后一点，这些也是商业智能工作的主要开始和结束部分。甚至第二、第三和第四点中的一些也可以成为商业智能分析师日常工作的一部分。

> 然而，这里的主要区别是关注模型探索、比较、最终模型和部署，这也是关注机器学习算法和机器学习操作的数据科学过程的一部分。

这一点可能是数据科学和商业智能之间的最大区别，尽管一些商业智能分析师会执行回归分析、预测和预报。

另一个区别可能是对`Python` / `R`或另一种编程语言的关注，在这种语言中，面向对象的概念被用于数据科学。

*以下是数据科学家**【3】的* [*示例工作描述。*](https://g.co/kgs/aEd4tw)

> **除了上面的工作描述和我自己的个人经验，这里有一个我作为数据科学家会采用的具体流程:**

*   开发一个用例:“*这些产品可以用机器学习算法更好更快地分类。*
*   我们需要来自各种来源的数据来创建这个算法，比如我们的产品目录。
*   我们可以`SQL`查询我们的数据库，现在我们已经有了必要的数据，并使用`Pandas`和`Python`把它拉进来。
*   现在我们有了数据，我们可以识别缺失值、异常值、描述性统计、平均值/最小值/最大值以及其他简单但有用的函数，如`df.describe(), df.head(), df.tail(), and df.column_name.value_counts()`——它根据每个箱中显示的实例数量对数据进行分组。
*   删除不必要的功能，并通过简单地将两列分开来创建一些新的功能，在这个用例中使用的一个特定的好功能将是产品的描述—例如，黑色橙色的 ***颜色*** ，XL 高筒衬衫 ***尺寸*** ， ***条纹*** —这些是模型可以解释为成人万圣节衬衫产品的特征。
*   使用`scikit-learn`库利用`Random Forest`算法创建分类器。
*   使用`Amazon SageMaker`作为您的部署平台来部署模型。
*   创建分类器结果的仪表板，包括模型的准确性。

正如你所看到的，有相当多的数据分析与机器学习结合在一起。话虽如此，如果你喜欢面向对象编程以及机器学习操作，我推荐数据科学家这一角色，因为这一角色可以为公司带来巨大的价值，将自动或不准确的流程转换为自动、更准确和更快速的流程。

现在，让我们更深入地了解商业智能的组成，以及它和数据科学之间的异同。

# 商业智能

![](img/c6a91bb33a6bfe9989f8c59aca6a7728.png)

[DocuSign](https://unsplash.com/@docusign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄的照片。

这个领域已经存在了很长时间，可以看到许多与数据科学的重叠，但是，最大的相似之处是两个角色的目标。两个职位或领域都致力于开发用例并解释结果。最终检索这些结果的方法可能有些不同。例如，商业智能分析师可能更关注`Excel`、`Google Sheets`、`Tableau`和`SQL`。

> 以下是商业智能职业的一些主要概念、技能和期望:

还包括一些示例工具——但不仅限于这些技能。

*   与利益相关者一起开发用例。
*   使用`Excel`、`VLOOKUPs`和`SUMIFs`执行数据分析。
*   使用`SQL`获取数据。
*   用`SQL`/更复杂的查询功能分析数据。
*   有时—在`Excel`或其他工具中预测或预报。
*   在可视化工具中显示结果，如`Tableau`或`Looker`。
*   与利益相关者或高管讨论结果。

两种角色之间的一些工具重叠，但是，与此要点总结不同的主要概念是，商业智能中不涉及机器学习算法或部署。另一个区别是每个职位所需的教育程度。这两种角色都需要硕士学位，当然，每个角色都需要与各自的职位更相关的学位，但是，随着远程工作和教育的变化，有更多的角色以训练营、在线学位和在线认证的形式向教育开放。

*下面是一个* [*商业智能分析师*](https://g.co/kgs/xmME86)*【5】的工作描述示例。*

> **除了上面的工作描述和我自己的个人经验，这里有一个我作为商业智能分析师会采用的具体流程:**

*   确定某拼车公司在晚上 11 点有异常数量的用户。
*   获取用户数据和时间数据，以创建要分析的数据集。
*   用`Excel`和/或`SQL`汇总这些数据。
*   `group by`在`SQL`了解到晚上 11 点有某个统计数据。
*   使用回归预测并测试这种情况是否每天都会发生。
*   发现这一群人也在晚上结束时有艺术家聚会的地方分享活动数据。
*   为艺术家创建一个推广活动，告诉其他艺术家也分享，并给予折扣。
*   用`Tableau`展示促销对该客户群的影响结果。

如你所见，这个角色非常注重`SQL`和业务探索。我向那些寻求将`SQL`技能、可视化以及与非技术利益相关者的协作完美结合的人推荐这个职位。你也可以跳过像`Python`这样可能需要更长时间学习的编程技能，从而更快地申请这个职位。这些职位导致更快的项目带来价值，然而，在数据科学中，项目持续时间超过几个月，而在商业智能中可能只有一周。也就是说，商业智能分析师或开发人员可以为公司提供很多价值，比如可以降低成本的见解和分析，以及与数据科学家合作，后者可能会根据你的发现创建一个模型。

# 摘要

这两个角色起初看起来非常相似，甚至非常不同，但是，分析每个职位的来龙去脉，以及每个角色在日常工作或项目中的预期是很重要的。目标可能是最相似的，因为数据、见解和结果都包含在项目干系人的讨论中。除了数据科学在所有方面都明显侧重于机器学习之外，更多`SQL`与更多`Python` / `R`侧重技能等方法也有所不同。

> 总而言之，以下是对每个角色的一些主要期望:

```
*** Data Science:** data acquisition, Python, as well as machine learning algorithms and deployment*** Business Intelligence:** Excel or Google Sheets, SQL, data analysis, and forecasting
```

感谢您的阅读！如果您有任何问题，请联系我，如果您有任何经验、共识或不同意上面讨论的相似性和/或差异，请在下面评论。在每个单独的角色中，你还有哪些其他的技能、概念或期望？您是业务分析师还是数据分析师，您是否也经常比较或对比这些角色——您认为所有这些角色之间有哪些相似或不同之处？

*我不隶属于上述任何公司。*

# 参考

[1]照片由[克里斯蒂娜@ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/office?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[2]照片由 [Fitore F](https://unsplash.com/@daseine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/robot?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[3]迪厅，[数据科学真实岗位描述示例](https://g.co/kgs/aEd4tw)，(2021)

[4]照片由[文件设计](https://unsplash.com/@docusign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2021)上拍摄

[5]沃尔玛，[商业智能真实工作描述示例](https://g.co/kgs/xmME86)，(2021)