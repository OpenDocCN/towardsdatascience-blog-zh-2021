# 浓缩咖啡的剂量设置

> 原文：<https://towardsdatascience.com/dose-settings-for-espresso-77d4bdb699e5?source=collection_archive---------30----------------------->

## 咖啡数据科学

## 探索一些剂量设置

浓缩咖啡是迄今为止制作咖啡最丰富的方法，但也是最困难的方法。每一个镜头都有挑战，因为多个变量汇集在一起形成了一个美丽的东西。研磨设置、夯实压力、水温、压力、预注入、剂量、篮子、研磨机和机器都起作用。对于初学者来说，最具挑战性的障碍是能够理解他们应该根据其他镜头的运行方式做出什么调整。

我一直在看我为我的浓缩咖啡拍摄做失败分析的所有方法，我认为看几个变量会很有趣。在开始，我想分别看看这些变量，并展示我所看到的，希望它能帮助其他人更好地看到他们的投篮输出，并理解从那里去。

在这篇文章中，我看看剂量，特别是 18g，19g 和 20g。对于每一个，我把地面放在一个篮子里，用牙签分发，并用自动校平机夯实。我把中心做得不那么密集，以对抗杠杆机器固有的噪音。我也用同样的篮子和同样的豆子拍了三张照片。

![](img/1f19018f9bcc0a341c8a22ff0fa83578.png)![](img/4d276383a4f2c2f1b6a9cf3b02cefe5d.png)![](img/f2c3b1b71037676f07e092f3b88f3f69.png)

18g、19g、20g，所有图片均由作者提供

我为每个镜头都拍了视频。每一行都是 5 秒的时间。这些列是 18 克、19 克和 21 克的剂量。

18g……..……..19 克 20 克

![](img/7e4931833365e5f9f9a0672ff98b4a37.png)![](img/75b88ecc52e1939405f7372417cef54b.png)![](img/7f5dd6f7fc3a6248b66ea9f9bb486bfe.png)![](img/77f977099463707be6c027b9c76347e9.png)![](img/e64b57c61ce13f99dfe976c4aa2b5fb6.png)![](img/d172c7b180b669cd1d48c64fb18deb62.png)![](img/1705b34069aa6c566e27a67cfa0543af.png)![](img/6a43c6738085e0e098f6fd04fbc218d4.png)![](img/e1565638ab4781e26fedfd2ccf2017fa.png)![](img/d2c15c9037641fb5771a97752cff9cb3.png)![](img/660771497895fcaa30a8433fddac428e.png)![](img/3ecaaae1ed99f60d86ebdf67e270e808.png)![](img/1ef8ce53501c55ac3c6c7040bd10132d.png)![](img/80318cddf4ef4b269c7a1d106b31c926.png)![](img/c7fd980e6508d60f6cb57715fa2290a4.png)![](img/ca1bbfd075e1a8ca0d901113d68f3e1e.png)![](img/d39e7eb864a4c6f39fef07a5006ac002.png)![](img/1429b32a7de16e5a848ca05204902c88.png)![](img/a70b32e8357bde30715586678b1ef928.png)![](img/cf9d5bf10941942d948c5e2e22478ec3.png)![](img/290de6456f1b7b8c7596628f8b62ce07.png)![](img/f83582a6c1ad5077636e5aef4a4b2cbb.png)![](img/d41159602bbe3e79ea4bd33e70f55176.png)![](img/bdaf8332a97047ca2b39f316ca7fb3d9.png)![](img/0cfc86d377dcff2e3b9283a64a7c66ef.png)![](img/6e15c49c8a942c8741c538ad9758ba81.png)![](img/ae0d99122c8664c38c5e8058e139afc0.png)

这是流量日志。

![](img/5ca8e063e72f3d59c4c20f7219f04f1b.png)

它们都有相似的流动剖面，但它们并不完全一致。因此，让我们将 18g 和 19g 向后移动一两秒钟，使流动中的拐点对齐。

![](img/22bf80332732593a2fd73476ad20f255.png)

从这里开始，18g 真的很快达到峰值，18g 和 19g 的最大流量高于 20g。

我们可以看看圆盘的底部，看起来 18g 的边缘有更多的黑点，但它们看起来相对相似。

![](img/bb2e95096dabc2baf65dc109e05cb9d2.png)![](img/8af85f28d9f06130e34ad64be97754cc.png)![](img/42876bce9dc817488f77ad6c7d5e76a5.png)

18g，19g，20g

# 绩效指标

我使用两个指标来评估技术之间的差异:最终得分和咖啡萃取。

[**最终得分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6) 是记分卡 7 个指标(尖锐、浓郁、糖浆、甜味、酸味、苦味和余味)的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难确定。

[](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)**使用折射仪测量总溶解固体量(TDS)，这个数字结合咖啡的输出重量和输入重量用于确定提取到杯中的咖啡的百分比，称为**提取率(EY)** 。**

# **品味和 EY**

**从口味和提取方面来说，19g 是最好的，EY 也达到了顶峰。**

**![](img/edfb3b48fcc0477231fa3211e2ff86b3.png)****![](img/3ff4cf623f538e6f32b37b978c21ef3c.png)**

**正如预期的那样，20g 覆盖过滤器(TCF)的时间最长，但总时间保持不变。**

**![](img/ae108d98df1917ad7da608dcbaf94662.png)**

**剂量设置的关键是尝试一些设置。耐心会有回报的，因为你已经习惯了你的机器、研磨机和咖啡豆。**

**如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)。**

# **[我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):**

**[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)**

**[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)**

**[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)**

**[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)**

**[摄影启动页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)**

**[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)**

**[改善浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)**

**[断奏生活方式概述](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)**

**[测量咖啡磨粒分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)**

**[咖啡萃取](https://rmckeon.medium.com/coffee-extraction-splash-page-3e568df003ac?source=your_stories_page-------------------------------------)**

**[咖啡烘焙](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea?source=your_stories_page-------------------------------------)**

**[咖啡豆](https://rmckeon.medium.com/coffee-beans-splash-page-e52e1993274f?source=your_stories_page-------------------------------------)**

**[浓缩咖啡用纸质过滤器](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)**

**[浓缩咖啡篮及相关主题](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)**

**[意式咖啡观点](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)**

**[透明 Portafilter 实验](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)**

**[杠杆机维护](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)**

**[咖啡评论和想法](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)**

**[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)**