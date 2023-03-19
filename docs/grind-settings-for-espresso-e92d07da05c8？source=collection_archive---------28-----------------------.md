# 浓缩咖啡的研磨设置

> 原文：<https://towardsdatascience.com/grind-settings-for-espresso-e92d07da05c8?source=collection_archive---------28----------------------->

## 咖啡数据科学

## 数据驱动指南

浓缩咖啡是迄今为止制作咖啡最丰富的方法，但也是最困难的方法。每一个镜头都有挑战，因为多个变量汇集在一起形成了一个美丽的东西。研磨设置、夯实压力、水温、压力、预注入、剂量、篮子、研磨机和机器都起作用。对于初学者来说，最具挑战性的障碍是能够理解他们应该根据其他镜头的运行方式做出什么调整。

我一直在看我为我的浓缩咖啡拍摄做失败分析的所有方法，我认为看几个变量会很有趣。在开始，我想分别看看这些变量，并展示我所看到的，希望它能帮助其他人更好地看到他们的投篮输出，并理解从那里去。

本文重点介绍 3 种研磨设置。对于每一个，我把地面放在一个篮子里，用牙签分发，并用自动校平机夯实。我把中心做得不那么密集，以对抗杠杆机器固有的噪音。我也用同样的篮子和同样的豆子拍了三张照片。

![](img/64f6128048c8a5a1a6b1e88d2f301d6e.png)![](img/e4e6ea68a63dacb52000ffb1dd71c68e.png)![](img/db08699d434c6f22a7ab2e435992d142.png)

磨成篮子，分配，夯实。所有图片由作者提供

我使用了三种设置:8，10，12。

![](img/35a04e3164821b5d9b9b40545cccd184.png)![](img/64f6128048c8a5a1a6b1e88d2f301d6e.png)![](img/d4d92ca1204746973e4f71a9a21a4106.png)![](img/c4ac7405b4000ea730e04c5f41e0c2e5.png)![](img/db08699d434c6f22a7ab2e435992d142.png)![](img/8d7eb8fdf0da908a2eb20d04a941f1a2.png)

左栏:设置 8，中间:设置 10，右栏:设置 12

我为每个镜头都拍了视频。每一行都是 5 秒的时间。列设置为 8、10 和 12。

设置 8 ……。设置 10……设置 12

![](img/e96030bb528eb68e176f697e8d8b2018.png)![](img/55c4b85001006b0ca86225b98e777690.png)![](img/5845044b2849d5b113f0a6221af214c2.png)![](img/d3ce803f715a2aff581a1342d643f9ab.png)![](img/a89a84de1bd763b68a445c638d9b1d63.png)![](img/2861b3d6eb5d80f0588268a2ba41dc07.png)![](img/eeec4ecaa7e336f22dcf3a8d7589af01.png)![](img/19a587d08859d0081fb3d4ca3d84eb81.png)![](img/c55cff3938c522c6ca21f382410bb7bf.png)![](img/8efc5f06302d79eed2cdce893f22b6d3.png)![](img/8f5e7551350184aecc84ab44e075a686.png)![](img/3753eb438235d468630ec97630d55084.png)![](img/2aed0a730f199254efff6856c0dbe11f.png)![](img/a9d31f1eba8a73a3741ffe907651968b.png)![](img/6faf94e349223a52a69df2d469803d8b.png)![](img/47aeb97452f980ee6fa92be9ab5caa0d.png)![](img/6f873d18586cd09d8a70da91e5347255.png)![](img/a1baff050a31992813df72ebca51b008.png)![](img/133084d37d5a415cd965f0f469ee2258.png)![](img/441cc90c721280b7c9d84d29e9b4291b.png)![](img/46d57155c346bb71aeec4f883b145ba0.png)![](img/ccf31e1bc4b5426337ad1b44c6fdf5f3.png)![](img/fae76b732071058ce1cfe8f87d132f9c.png)![](img/4285fbec39340445285043415ba297f5.png)![](img/25ee5b554801b7a27f0e1fe436571bf6.png)![](img/6eda11df82d4fbc91c0c935cdc256121.png)![](img/cf6c74ec3028dcb59c884ce79f5c1c74.png)

从图中可以看出，设置 12 拍跑得更快。我们可以观察一段时间内的流量:

![](img/21adcf3ab3cb5e08820f59f79e20b4dd.png)

在拍摄之后，我通常会对冰球的底部进行成像，设置 8 看起来在外侧有一个更暗更宽的环，这表明沟道效应更差。

![](img/8546dc1512e871e0186d5d7e80fb18a6.png)![](img/1cae6d77e3ba35cb87ac5803cc029695.png)![](img/2fe028feee1169a6e55a6414c6c353c3.png)

设置 8，设置 10，设置 12

# 绩效指标

我使用两个指标来评估技术之间的差异:最终得分和咖啡萃取。

[**最终得分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6) 是评分卡上 7 个指标(辛辣、浓郁、糖浆、甜味、酸味、苦味和回味)的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难确定。

# 品味和 EY

就性能而言，设置 10 和 12 在 TDS 和 EY 方面是相似的，但是基于味道，设置 10 大大优于其他 2 个。

![](img/ddb05207312f3a721dbb8c1c2c100827.png)![](img/d59bc982e3a9ca6ce29351e06df3f85d.png)

在拍摄时间方面，我保持预输注恒定，但 TCF(覆盖过滤器的时间)在设置 12 时比其他两个快得多。正如预期的那样，研磨越粗，总时间越短。

![](img/b32da6e6d9f72ed37d8ea9729c7746db.png)

制作好的浓缩咖啡的关键在于实验。尝试几个不同的变量，并试图了解如何进行调整，直到你得到天堂的味道。然后不断调整你所有的变量，直到你到达天堂的下一层。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影启动页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

[改善浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

[断奏生活方式概述](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)

[测量咖啡研磨分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)

[咖啡萃取](https://rmckeon.medium.com/coffee-extraction-splash-page-3e568df003ac?source=your_stories_page-------------------------------------)

[咖啡烘焙](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea?source=your_stories_page-------------------------------------)

[咖啡豆](https://rmckeon.medium.com/coffee-beans-splash-page-e52e1993274f?source=your_stories_page-------------------------------------)

[浓缩咖啡用纸质过滤器](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)

[浓缩咖啡篮及相关主题](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)

[意式咖啡观点](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)

[透明 Portafilter 实验](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)

[杠杆机维护](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)

[咖啡评论和想法](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)

[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)