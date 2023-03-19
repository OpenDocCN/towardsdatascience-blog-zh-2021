# 浓缩咖啡的预浸泡

> 原文：<https://towardsdatascience.com/pre-infusion-for-espresso-dab5185b8094?source=collection_archive---------13----------------------->

## 咖啡数据科学

## 暴风雨(或大动荡、激烈辩论)前的平静

直到我发现泵机没有预注入功能，至少对于便宜的泵机来说是这样，我才意识到我对杠杆机是多么的宠坏了。预浸是一个关键参数，它允许你将咖啡磨得更细，因为它有助于减少通灵。

对于那些不知道的人来说，预冲泡是让水以低于正常的压力进入咖啡球。通常，预注入压力在 0.5 至 4 巴之间。通常为 1 至 2 巴。

刚开始定期使用金快线的时候对预输注了解不多，最后还是用了 10 秒预输注的套路。在过去的两年里，我开始更多地探索这一变量，因为我发现更长的预灌输导致更好的品尝。

在这篇文章中，我使用了三个预输注参数来显示它们如何影响拍摄。这不是对某个特定参数的建议，而是让人们探索变量，找到最适合他们的方法。对于这三个镜头，他们都是在金快车上拍摄的，有着相同的咖啡烘焙、相同的研磨机和相同的冰球准备。所有注射都使用[压力脉冲](/pressure-pulsing-for-better-espresso-62f09362211d)进行注射。

每列图像是一个参数，而每行是 5 秒钟的时间，预输注时间为:

30 秒……20 秒……..十秒钟。

![](img/6b101dc4aebdb4008fb851ba339f43e5.png)![](img/2d6840f0053a6e6baff04e79bb8862c0.png)![](img/3b692e6b89f38c10ac77792ba9188924.png)![](img/096f746f53b2defb16948af7a44635f5.png)![](img/a22dafaa9e38cdef517dcabfe797a27d.png)![](img/8d4400e44df444e5600cf23bc2171f90.png)![](img/f6561dbb237ceeb27468059b911cba20.png)![](img/f6e7b0d63c1d169d74c7c4b66ad7c50d.png)![](img/0a481937c3229e27c4cbd51ed00afd74.png)![](img/4e3c8320835f78bc01ede73c94447176.png)![](img/a92456226db6a19197ebce85d3bd9597.png)![](img/62371c858cc4805eed3911c8b3895d8a.png)![](img/1004878814eec592baf01652a60187d5.png)![](img/393fe2d73f8c6a8e0963cbbb1e2d73b1.png)![](img/24a43f4bf9af6a2ee54c4165ce21de9e.png)![](img/85b7297ceb8f616b49052e553644462b.png)![](img/606c3ed79703375664d3ab5dc4828b6f.png)![](img/3bfca8ad958f4e5babcd7ec53bc19fb0.png)

首先清楚的是，较低的预输注注射产生更多的克莉玛。让我们来看看一些性能指标。

# 绩效指标

我使用两个指标来评估技术之间的差异:最终得分和咖啡萃取。

[**最终得分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6) 是记分卡 7 个指标(尖锐、浓郁、糖浆、甜味、酸味、苦味和回味)的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难确定。

</coffee-solubility-in-espresso-an-initial-study-88f78a432e2c>**用折射仪测量总溶解固体量(TDS)，这个数字结合咖啡的输出重量和输入重量用于确定提取到杯中的咖啡的百分比，称为**提取率(EY)** 。**

# **表演**

**对于 TDS 和 EY 来说，较短的预浸时间胜出，但奇怪的是，较长的预浸时间只会稍微改善味道。最大的变化是甜味增加，糖浆(或口感)减少。**

**![](img/aa98161bcbbd220628f00db7125acc2e.png)****![](img/b5c7c6c558b7802b79ccd26a73db438f.png)**

**时间指标主要由预输注时间驱动，因为输注时间大致保持不变。**

**![](img/dff6a374465ebbd20150040d7cd158e6.png)**

**预浸泡时间越短，产生的克莉玛越多。这可以从达到特定体积所需的时间中看出。**

**![](img/df9e047b6c98db7bde45e10d93e54624.png)**

**我们可以把它们画在一起，看一看总的趋势。**

**![](img/50c3a0ad3d7d585dd1b438756869033d.png)**

**我不知道这说明了克莉玛什么。人们通常认为克莉玛是浓缩咖啡性能的一个很好的定性指标，但由于长时间的预浸泡，我的大多数照片都没有多少克莉玛。**

**虽然我更喜欢更长时间的预输注，但数据表明仍有探索的空间。通常，我的预输注时间与覆盖过滤器的[时间(TCF)](/pre-infusion-for-espresso-visual-cues-for-better-espresso-c23b2542152e) 相关，因为我之前发现最佳预输注时间大约为 3*TCF。**

**如果你愿意，请在 [Twitter](https://mobile.twitter.com/espressofun?source=post_page---------------------------) 和 [YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw?source=post_page---------------------------) 上关注我，我会在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以在[中](https://towardsdatascience.com/@rmckeon/follow)关注我，在[订阅](https://rmckeon.medium.com/subscribe)。**

# **[我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):**

**[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)**

**[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)**

**个人故事和关注点**

**[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)**

**[摄影飞溅页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)**

**[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)**

**[改良浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)**

**[对断奏生活方式的总结](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)**

**[测量咖啡研磨分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)**

**[咖啡萃取](https://rmckeon.medium.com/coffee-extraction-splash-page-3e568df003ac?source=your_stories_page-------------------------------------)**

**[咖啡烘焙](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea?source=your_stories_page-------------------------------------)**

**[咖啡豆](https://rmckeon.medium.com/coffee-beans-splash-page-e52e1993274f?source=your_stories_page-------------------------------------)**

**[浓缩咖啡用纸质过滤器](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)**

**[浓缩咖啡篮及相关主题](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)**

**[意式咖啡观点](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)**

**[透明 Portafilter 实验](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)**

**[杠杆机维修](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)**

**[咖啡评论和想法](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)**

**[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)**