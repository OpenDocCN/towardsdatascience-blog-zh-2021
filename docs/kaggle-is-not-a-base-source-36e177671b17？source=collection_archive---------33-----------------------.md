# Kaggle 不是一个基地来源

> 原文：<https://towardsdatascience.com/kaggle-is-not-a-base-source-36e177671b17?source=collection_archive---------33----------------------->

## 作为数据科学家，我们的工作就是用真实、可操作的数据来讲述故事。基于错误数据的故事只是童话。

![](img/30e1ebdb6268ad9e7cf4146254193c9e.png)

[娜塔莉亚 Y](https://unsplash.com/@foxfox?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/fairy-tale?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

上一次您不得不为学校项目、商业项目或个人项目寻找和收集数据时，您会去哪里找呢？你是直接从你公司的某个人那里得到数据的吗？你有没有自己出去追踪一个有趣的数据集？

**最重要的是——你拿到数据后，验证了数据的合法性吗？**

我需要一个项目的分类数据集。这些数据可以是任何东西，所以我很自然地去 Kaggle 寻找一些很酷的数据集。我在[航空公司乘客满意度](https://www.kaggle.com/teejmahal20/airline-passenger-satisfaction)上找到了这个数据集，这是一个非常丰富的分类数据集。

![](img/0fa3fb49e5b255c995afdd4e83bd2725.png)

Kaggle 上的航空公司乘客满意度数据集—图片由作者拍摄

这是一个很棒的数据集。它有各种客户服务指标评级、一个二元目标变量和超过 130，000 行响应。这是 Kaggle 上的一个银牌数据集，理由很充分。它检查所有的盒子是否是一个有趣的数据集。

然而，我立即注意到，除了承认该数据是从位于[这里](https://www.kaggle.com/johndddddd/customer-satisfaction)的不同 Kaggle 数据集修改的之外，没有任何关于该数据的信息。

![](img/c7b4488f979a3cf90799fa3db7df124a.png)

Kaggle 上的乘客满意度数据集—作者拍摄的图像

我去了约翰·d 上传的这个“原始”数据集。值得注意的是，这个数据集仍然没有归属，所以现在我去了谷歌。

在我的搜索中，我发现了两个潜在的数据来源。第一个是美国客户满意度指数，在其[航空公司客户体验基准](https://www.theacsi.org/industries/travel/airline)调查中有一个类似的独立变量列表。然而，他们的声明是“每年，ACSI 都会采访数百名乘客，询问他们最近的飞行经历”。几百是很好，但不是 130k。这是数据的来源吗？如果是这样，这些数据可能是从几十年的回复中收集的，因此不一定有用。

另一个潜在的数据来源是国际航空运输协会的 [Airsat 乘客满意度基准](https://www.iata.org/en/services/statistics/intelligence/passenger-satisfaction-benchmark/)。他们列出了类似的变量分组，但没有具体说明他们进行了多少次调查。

除了无法找到数据来源——**最令人不安的是，有多少人使用这些没有来源的数据集作为重要项目的基础**。我在一个同行评审的出版物中找到了一篇已发表文章的实例，该文章公开使用原始的“John D”数据集作为他们工作的基础，将其称为“Kaggle repository”。虽然这篇文章的目的更多的是关于方法而不是商业答案，但是底层数据仍然应该被验证和获得。《约翰·D·论卡格尔》不是资料来源。另一篇为《零售和消费者服务杂志》撰写的文章(我只能阅读摘要)提到了“一个包含超过 13.3 万名顾客反馈的数据集”。在不确定的情况下，这个特定的响应数字可疑地接近于清理过的银牌数据集，这个数据集引发了整个奇怪的兔子洞。

对航空公司满意度的任何搜索都会产生几个项目、文章和参考资料，这些项目、文章和参考资料都基于现在臭名昭著的(如果只是我自己认为的)航空公司乘客满意度数据集— **一个没有已知来源的数据集**。

据我们所知，约翰·D 在一个下午心血来潮写了这个数据集。我们永远也不会知道，因为尽管在他的数据集的评论中有要求，他从来没有说明他的来源。现在这个数据集在互联网上永久存在，被合法出版物引用。

这正是错误信息传播的方式。没有来源的信息被当作合法的信息传播出去。当足够多的时候，它被认为是合法的，不需要确认。也许这个数据集是合法的——但如果不是呢？如果这个数据集向企业和世界提供了错误的，或者最多是可疑的信息，那该怎么办？

不要增加这个问题。了解你的数据来源。验证您的数据。仅使用来自已知来源的数据，并尽可能获得“第二意见”。为您的数据提供归属，并增加信心。

**作为数据科学家，我们的工作就是用真实、可操作的数据来讲述故事。基于错误数据的故事只是童话。**

参考资料:

1.[TJ Klein 整理的 Kaggle 上的航空公司乘客满意度](https://www.kaggle.com/teejmahal20/airline-passenger-satisfaction)数据集

2.[乘客满意度](https://www.kaggle.com/johndddddd/customer-satisfaction)由 John D .上传的 Kaggle 数据集

3. [ACSI 航空公司体验基准计划](https://www.theacsi.org/industries/travel/airline)

4. [Airsat 乘客满意度基准](https://www.iata.org/en/services/statistics/intelligence/passenger-satisfaction-benchmark/)计划