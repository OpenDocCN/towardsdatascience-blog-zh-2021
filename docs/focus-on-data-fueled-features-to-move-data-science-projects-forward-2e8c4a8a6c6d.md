# 关注数据驱动的特性，推动数据科学项目向前发展

> 原文：<https://towardsdatascience.com/focus-on-data-fueled-features-to-move-data-science-projects-forward-2e8c4a8a6c6d?source=collection_archive---------36----------------------->

## 数据驱动并不意味着数据驱动本身

![](img/3ea02d69d0d47f76d344ccf2e263a39e.png)

Julian Hochgesang 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

对于围绕数据和数据连字符术语(如“数据驱动”)的所有宣传，重要的是要记住，数据是一种原始资源，在集成到使用所述数据生成有意义的输出的产品之前，它没有实际价值。虽然数据科学家的具体角色和职责因组织而异(甚至在组织内部)，但数据科学家通常负责将数据从原始资源实际转换为对产品用户有价值的东西。事实上，与其说数据科学是对数据本身的科学研究，不如说它是创造事物的实践，这些事物使用数据来产生推论和/或预测，以满足未满足的需求或希望[3，4]。因此，一个组织在数据科学方面的投资通常旨在创建我所称的“数据驱动的产品功能”，即在输入和输出之间的实际过程中，在最少或没有人为干预的情况下，将数据输入转化为有价值的输出的产品功能。然而，要使数据科学项目成功且高效地运行，仅仅拥有大量数据并期望数据科学能够提供相关的数据驱动特性是不够的。相反，在任何技术工作(例如，数据争论和建模)发生之前，需要对期望的数据驱动特性有一个清楚的理解。

# 产品、功能和优势，天啊！

在继续之前，让我们建立一些定义。**产品**是一种旨在满足顾客需求的物品或服务[1，2]。产品的定义很宽泛，可以包括从电视和汽车等实物到信贷监控和金融建议等客户服务。在产品的上下文中，**特征**是产品的特定功能部分，旨在产生特定的**收益**，其中收益是用户通过使用产品获得的某种价值【7】。一个**数据驱动的特性**，特别是，我用这个术语来描述一个由数据驱动的产品特性，如果没有数据输入，这个特性将毫无用处。数据驱动功能的例子包括电子商务网站/应用程序上的推荐引擎，语音助理设备中的语音识别模型，可以区分热狗和非热狗的图像分类器，信用卡客户服务中的欺诈检测模型。

我之所以如此强调产品功能，而不是产品本身，是因为一个产品的创造，甚至可以被称为“数据产品”[4–6]，是具有互补技能的多个团队的结果，而不仅仅是数据科学家。然而，当焦点转移到产品的特定数据驱动功能时，我相信数据科学的责任会变得更加清晰，组织数据科学项目会变得更加简单。

# 从想法到执行

如果数据科学要产生任何效益，方向是必要的。为数据驱动的特性建立一个清晰的概念提供了这样的方向。为什么？当对数据驱动的特性有了清晰的想法后，数据科学项目的下一步可能会变得更加清晰。

我可以想到至少三种方式来理解需要创建的特性，这有助于指导项目中的其他步骤(如果您有其他方式，请留下评论)。首先，在确定了数据驱动特性的想法后，就更容易确定需要的数据，同样重要的是，确定不需要的数据。现在，这并不意味着所需的数据将立即可用或准备好在机器学习模型中使用(大多数情况下不会)，但能够表达数据需求是朝着正确方向迈出的一大步。其次，理解所需的特征也有助于确定需要对数据应用哪种方法。例如，如果正在讨论的特性旨在预测某种事件是否已经发生(比如欺诈行为)，那么您现在有一种很好的预感，您将需要一个分类模型。此外，通过评估预测生成过程是否有必要具有高度的可解释性，您现在可以进一步细化您的建模策略了？如果是这样的话，那么就需要使用某种可解释的方法或机制来理解模型的决策过程。第三，了解推动数据科学项目的产品特性有助于评估性能指标。例如，如果该功能是一个受监督的机器学习模型，了解该功能对产品的用途有助于确定误报或漏报是否会给产品用户带来更大的代价[8]。

# 结论

如今，许多组织都在投资数据科学。这种努力通常应该受到欢迎(假设这些努力建立在道德和负责任的数据实践的基础上)。然而，我写这篇文章是为了强调，对数据科学的投资假定或者应该假定，一个组织对数据驱动的功能有很强的想法，或者正在积极开发由数据驱动的功能的想法。请记住，数据科学家可以将任何数据放入模型并获得一些结果。然而，有意义的结果是由一个清晰的想法驱动的，这个想法指导数据科学家努力使用数据为产品用户带来某种好处。

1.[https://courses . lumen learning . com/unlimited-marketing/chapter/what-a-product/](https://courses.lumenlearning.com/boundless-marketing/chapter/what-is-a-product/)

2.[https://www . aha . io/road mapping/guide/product-management/what-a-a-product](https://www.aha.io/roadmapping/guide/product-management/what-is-a-product)

3.Kozyrkov C .数据科学到底是什么？in:hacker noon[互联网]。2018 年 8 月 10 日【引用于 2020 年 9 月 7 日】。可用:[https://hackernoon . com/what-the-earth-is-data-science-EB 1237 D8 CB 37](https://hackernoon.com/what-on-earth-is-data-science-eb1237d8cb37)

4.做数据科学:来自前线的直话直说。“奥莱利媒体公司”；2013.

5.[https://www . district data labs . com/the-age-of-the-data-product](https://www.districtdatalabs.com/the-age-of-the-data-product)

6.O'Regan S.《设计数据产品——走向数据科学》。In:走向数据科学[互联网]。2018 年 8 月 16 日【引用于 2021 年 2 月 8 日】。可用:[https://towards data science . com/designing-data-products-b 6 b 93 EDF 3d 23](/designing-data-products-b6b93edf3d23)

7.h[ttps://www . aha . io/road mapping/guide/requirements-management/what-is-product-features](https://www.aha.io/roadmapping/guide/requirements-management/what-are-product-features)

8.Koehrsen W.《超越准确性:精确性和召回——走向数据科学》。In:走向数据科学[互联网]。2018 年 3 月 3 日[引用于 2021 年 2 月 20 日]。可用:[https://towards data science . com/beyond-accuracy-precision-and-recall-3da 06 bea 9 f6c](/beyond-accuracy-precision-and-recall-3da06bea9f6c)