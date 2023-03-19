# 在成为数据科学家之前，你应该先掌握数据分析

> 原文：<https://towardsdatascience.com/you-should-master-data-analytics-first-before-becoming-a-data-scientist-5dbceaea9d3d?source=collection_archive---------4----------------------->

## 意见

## 以下是为什么…的 4 个原因

![](img/f1c6fa8c750db18ff1da5a57ae096c6e.png)

由[Unsplash](https://unsplash.com/s/photos/data-analyst?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的[新数据服务](https://unsplash.com/@new_data_services?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片。

# 目录

1.  介绍
2.  探索性数据分析
3.  利益相关者协作
4.  特征创建
5.  精通可视化
6.  摘要
7.  参考

# 介绍

虽然在学习数据科学之前了解数据分析是关键，这一点乍一看似乎很明显，但你可能会惊讶地发现，有多少人在没有分析和呈现数据的正确基础的情况下就直接进入了数据科学。无论是实习、入门级职位，还是任何真正的数据分析职位，事先都有一定的好处。还需要注意的是，这种形式的经验可以通过完成在线课程和数据分析专业来获得。也就是说，如果你已经接受了数据科学的正规教育，你可能已经只在一门课程中学习了数据分析的基础知识，这是最有可能的，这就是为什么有必要在你的投资组合中添加一些以数据分析为重点的知识。然而，最好的方法是与其他人一起练习某种数据分析，正如我在下面讨论在学习数据科学之前掌握数据分析的四大好处时所看到的那样。

# 探索性数据分析

![](img/ab57e1afde9a261b36e0dc45a5304d93.png)

由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

由于你专攻数据分析，你在探索数据方面变得高效就不足为奇了。作为一名数据科学家，这通常是数据科学过程的第一步，因此如果您跳过这一步，您的模型可能会导致错误、混乱和误导性结果。你必须记住，垃圾进来会产生垃圾出去。仅仅因为你向机器学习算法扔了一个数据集，并不意味着它会回答手边的业务问题。

您将不得不发现数据中的异常、聚合、缺失值、转换、预处理等等。首先理解数据当然很重要，因此精通数据分析至关重要。有几个 Python ( *和 R 以及* l)库可以帮助自动完成这项工作。然而，我经常发现，对于大型数据集，它们花费的时间太长，会导致内核崩溃，你不得不重新启动。这就是为什么手动观察数据也很重要。也就是说，我将在下面介绍的库有一个大型数据集模式，它可以跳过一些昂贵且耗时较长的计算。这种情况的参数在 Pandas Profiling 库的 profile 报告中:`minimal=True`。

> 这里有一个非常容易使用的库:

```
from pandas_profiling import ProfileReportprofile = ProfileReport(df, title="Pandas Profiling Report")profile.to_widgets()# or you an do the followingdf.profile_report()
```

[熊猫简介](https://github.com/pandas-profiling/pandas-profiling)【3】，可以在你的 Jupyter 笔记本上查看。该库的一些独特功能包括但不限于类型推断、唯一值、缺失值、描述性统计、频繁值、直方图、文本分析以及文件和图像分析。

总的来说，除了这个库之外，还有无数种方法可以练习探索性的数据分析，所以如果你还没有，找一门课程，掌握分析数据。

# 利益相关者协作

![](img/9621e731883b930bcf806de01d30d049.png)

照片由 [DocuSign](https://unsplash.com/@docusign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄。

数据科学家通常可以在他们的教育中很快学会复杂的机器学习算法，跳过与利益相关者沟通以实现目标和阐明数据科学过程的重要部分。如果您还没有注意到，那么您必须成为将业务用例转化为数据科学模型的大师。产品经理或其他利益相关者不会走到你面前，要求你创建一个有 80%准确率的监督机器学习算法。他们会告诉你一些数据，以及他们一直看到的问题，你对数据科学的指导很少，这当然是意料之中的，因为这是你的工作。你必须提出回归、分类、聚类、提升、打包等概念。您还必须与他们合作，以建立成功标准，例如，100 RMSE 意味着什么，以及您如何解决并将其转化为对利益相关者有意义的业务问题。

那么，怎样才能学会协作呢？与数据科学家相比，预先担任数据分析师通常需要更多的协作。作为一名数据分析师，你将几乎每天或至少每周与他人一起创建指标、进行可视化并开发分析洞察力。正如我们从上面学到的，这种实践对于成为更好的数据科学家至关重要。

> 通过数据分析角色实现利益相关方协作实践的优势:

*   商业理解
*   问题定义
*   成功标准创建

如您所见，与利益相关方合作是数据分析师和数据科学家职位的重要组成部分。

# 特征创建

![](img/e9d5463bf7944135bd43f0e461bd54a9.png)

照片由[米利安·杰西耶](https://unsplash.com/@mjessier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/insights?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】上拍摄。

作为一名数据科学家，您将不得不执行要素工程，在该工程中，您将隔离有助于模型预测的关键要素。在学校或任何您学习数据科学的地方，您可能已经有了一个完美的数据集，但在现实世界中，您将不得不使用 SQL 查询您的数据库来开始查找必要的数据。除了表中已经有的列之外，您还必须创建新的列——通常，这些新特性可以是聚合的指标，例如`clicks per user`。作为一名数据分析师，您将最常使用 SQL，而作为一名数据科学家，如果您只知道 Python 或 R，这可能会令人沮丧——而且您不能一直依赖 Pandas，因此，如果不知道如何高效地查询数据库，您甚至无法开始模型构建过程。类似地，对分析的关注可以让你练习创建子查询和指标，就像上面所说的那样，这样你就可以添加一些到至少 100 个，完全由你创建的新特性，这些新特性可能比你现在拥有的基础数据更重要。

> 特征创建的好处:

*   能够执行任何 SQL 查询
*   提高模型精度和误差
*   寻找关于数据的新见解

# 精通可视化

![](img/b97161918c08146ae8efb7797c633854.png)

照片由[威廉·艾文](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/chart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上拍摄。

数据分析师通常会掌握可视化，因为他们必须以公司其他人易于理解的方式呈现调查结果。拥有一个充满值的复杂表格可能会令人困惑和沮丧，因此作为一名数据科学家，能够突出重要的指标、见解和结果也是非常有益的。同样，当你完成了你用来构建最终模型的复杂的机器学习算法时，你会兴奋地分享你的结果；然而，利益相关者只需要知道重点和关键要点。

> 通过可视化实现这一过程的最佳方式是，以下是创建这些可视化的一些关键方法:

*   （舞台上由人扮的）静态画面
*   谷歌数据工作室
*   检查员
*   Seaborn 图书馆
*   MatPlotLib

当然还有更多，但下面是我经常看到用的最多的。通过可视化表达见解和结果，你也可以帮助自己更好地学习过程和要点。

# 摘要

*那么问题来了，在成为数据科学家之前，是否应该先成为一名数据分析师？*我说是——或者至少是某种形式的，无论是实习、工作、类似业务分析师的工作，还是在数据分析课程中获得认证。除了我上面讨论的四个好处之外，另一个要强调的是，如果你在简历中有数据分析的头衔或经验，它肯定会帮助你找到一份数据科学家的工作。

> 总而言之，在成为数据科学家之前先成为数据分析大师有一些重要的好处:

```
Exploratory Data AnalysisStakeholder CollaborationFeature CreationMastered Visualizations
```

我希望你觉得我的文章既有趣又有用。如果你在成为数据科学家之前已经以某种方式成为了数据分析师，请在下面随意评论。这对你现在的数据科学事业有帮助吗？你同意还是不同意，为什么？

*请随时查看我的个人资料和其他文章，也可以通过 LinkedIn 联系我。*

# 参考

[1]照片由[新数据服务](https://unsplash.com/@new_data_services?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)于[Unsplash](https://unsplash.com/s/photos/data-analyst?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)拍摄

[2]由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)

[3]熊猫，[熊猫简介](https://pandas-profiling.github.io/pandas-profiling/docs/master/index.html)，(2021)

[4]照片由[医生](https://unsplash.com/@docusign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2021)上拍摄

[5]照片由 [Myriam Jessier](https://unsplash.com/@mjessier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/insights?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄

[6]照片由 [William Iven](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/chart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2015)