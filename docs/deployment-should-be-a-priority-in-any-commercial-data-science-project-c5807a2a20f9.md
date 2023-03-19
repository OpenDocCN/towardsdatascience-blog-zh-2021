# 部署应该是任何商业数据科学项目中的优先事项

> 原文：<https://towardsdatascience.com/deployment-should-be-a-priority-in-any-commercial-data-science-project-c5807a2a20f9?source=collection_archive---------33----------------------->

## [意见](https://towardsdatascience.com/tagged/opinion)

## *为什么你应该关心数据工程，即使是作为一名数据科学家*

# 介绍

我开始我的职业生涯时是一名核心科学家，所以将我的研究应用于生产从来不是我的主要关注点。幸运的是，我作为自然地理学家的背景确实迫使我永远不要将数据科学视为一项孤立的努力，而总是作为解决地理问题的一种手段。当我转换到一个行业职位时，我越来越相信将我的模型投入生产应该是我工作流程中不可或缺的一部分。在本文中，我想给出一些我变得确信每个数据科学家都应该学习一些数据工程技能(或者与一些数据工程师成为朋友)的原因。我想从两个角度来阐述我的观点:一个更技术性的观点和一个关注用户体验的观点。

![](img/0d46ea614e679c951310e8cc73d882bb.png)

由[卡洛斯·穆扎](https://unsplash.com/@kmuza?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 技术性

从技术的角度来看，我不认为在我自己的笔记本电脑上安装一个好的模型就一定能保证这个解决方案可以扩展到生产。生产中的数据量可能太多、太频繁甚至太少，以至于您的小型研究模型无法扩展到生产中。此外，数据的漂移可能很大，以至于几周后你的模型就过时了。只有当你积极地将你的模型尽早地、经常地投入生产时，你才需要面对这种偏差。此外，您所面临的数据工程挑战(如扩展数据存储、模型的版本控制和计算扩展)最好不要留到项目的最后。

# 用户体验

我想关注的第二个角度是用户体验。我认为普通的数据科学家很难代表最终用户。因此，让最终用户参与到你的项目中是真正获得用户体验的关键。即使有一个真正了解最终用户的好的产品负责人，也不等同于让用户使用你的解决方案。例如，在我参与的一个项目中，我们有一个我合作过的最好的产品负责人。即便如此，在我们与实际最终用户的第一次试验中，他们还是拒绝使用我们的产品。当他们忙于修理机器故障时，我们请他们花时间研究我们的产品。这不是一个选项，机器应该首先被修理。甚至我们的 rockstar 产品负责人也没有预见到这一点。

用户参与最好通过让实际用户使用该解决方案来完成，而不仅仅是让他们在房间里进行演示。即使他们在演示期间很热情，这也不一定意味着如果他们不得不使用产品本身，他们会很高兴。能够尽快将您的模型部署到最终用户是至关重要的。

# 生产价值

总之，我认为任何商业数据科学项目都应该优先部署他们的解决方案。它迫使您面对只在生产中出现的技术和 UX 问题，并可能严重限制您的解决方案的附加值。这当然不是一个简单的任务，但如果你的重点是创造现实世界的价值，这是值得的。

# 我是谁？

我叫 Paul Hiemstra，是荷兰的一名教师和数据科学家。我是科学家和软件工程师的混合体，对与数据科学相关的一切都有广泛的兴趣。你可以在 medium 上关注我，或者在 LinkedIn 上关注我。

如果你喜欢这篇文章，你可能也会喜欢我的其他一些文章:

*   [掌握数据科学并不是学习一系列技巧](/mastering-data-science-is-not-learning-a-series-of-tricks-df66d8529c29)
*   [牛郎星剧情解构:可视化气象数据的关联结构](/altair-plot-deconstruction-visualizing-the-correlation-structure-of-weather-data-38fb5668c5b1)
*   [面向数据科学的高级函数式编程:用函数运算符构建代码架构](/advanced-functional-programming-for-data-science-building-code-architectures-with-function-dd989cc3b0da)
*   [通过规范化扩展您的回归曲目](/expanding-your-regression-repertoire-with-regularisation-903d2c9f7b28)