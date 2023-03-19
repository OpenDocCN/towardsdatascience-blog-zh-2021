# 如何创建自己的机器人日记

> 原文：<https://towardsdatascience.com/how-to-create-your-own-robojournalist-36b463c91025?source=collection_archive---------29----------------------->

## 遵循我的一步一步的指导，用 Python 创建你的第一个数据到文本的新闻自动化系统

![](img/9b5a6a0731dcfe74a529e4fbe54b46cd.png)

照片由[雅诺什·迪格尔曼](https://unsplash.com/@janoschphotos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/uefa-soccer-2021?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

最近，我一直在想，我该如何向我的祖父解释我对自动新闻的自然语言生成的研究。我想，我可以创建一个简单的机器人记者来写他最喜欢的运动——足球。不幸的是，他已经在几年前去世了，但我会给你看。

## 期待什么？

在本文中，我将向您展示如何使用 Python 创建基于模板的新闻自动化系统。该方法本身非常简单，但是当您希望将一些重复的数据表示为书面报告，并确保输出如实地表示底层数据时，它确实很有用。

**优点**:

*   简单—不需要 NLP 方面的专业知识，您只需要了解您的数据。
*   流利——你造出的句子语法正确。
*   准确—不包括黑盒解决方案，输出将总是真实的。

**缺点**:

*   不可转移-对于下一个数据集，您需要重复整个过程。
*   费力-设计模板和微调输出需要时间和精力，而且每当您有不同形状的数据或来自不同领域的数据时都需要时间和精力。
*   重复—所有报告都相同。这可以通过添加替代模板或添加单词/替换单词来解决，请查看我的文章[robo journalism——如何解决重复问题](https://miiaramo.medium.com/robojournalism-how-to-tackle-the-repetition-693fcc25cac4)。

# 创建简单新闻自动化系统的分步指南

你是否有一些重复的数据进来，想以文本形式呈现给你的朋友/关注者/诸如此类的人？遵循这 6 到 7 个步骤来实现。

## 第一步:选择要报告的数据

我将使用《体育参考》提供的描述最近欧洲足球锦标赛的数据集。他们提供现成数据的 CSV 导出。这里可以找到[。](https://fbref.com/en/comps/676/schedule/UEFA-Euro-Scores-and-Fixtures)

## 步骤 2:加载数据并替换丢失的值

## 第三步:决定报道什么

我选择了以下数据字段进行报告

*   轮次
*   一天
*   日期
*   家庭(团队)
*   得分
*   离开(团队)
*   举办地点
*   笔记

## 步骤 4:创建模板

我对模板的想法很简单:我列出我想包含的句子。在句子模板中，定义的属性所在的位置被表示为{列名}。

## 步骤 5:将数据嵌入到模板中

我们有个小问题。我们提到 cadence 的第三个模板并不适用于所有活动，仅适用于跑步。这些缺失的值在我们的数据中用两条虚线表示。解决方案:在输出报告之前，删除最后包含两个破折号的任何句子。

## 第六步:输出

现在我们有了:关于 2021 年欧洲足球锦标赛的报道。我只包括了关于最后五场比赛的报道。

## 步骤 7:转换数据以微调输出

使用这种方法，您将获得与模板和输入数据一样好的输出。因此，通过对输入数据进行一些简单的转换，您将获得更好的输出。

感谢您的阅读！如果你学到了新的东西或者喜欢这篇文章，[在 Medium](https://medium.com/@miiaramo/follow) 上关注我。我发表关于数据工程和数据科学的文章。你可以从 be [网页](http://miiaramo.github.io/)了解更多关于我的信息。

我有没有让你疑惑，如何让输出更生动？查看我的文章[robo journalism——如何应对重复](https://miiaramo.medium.com/robojournalism-how-to-tackle-the-repetition-693fcc25cac4),并关注如何在你自己的 NLG 系统上应用我的想法的分步指南😊