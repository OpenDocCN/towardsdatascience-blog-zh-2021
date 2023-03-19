# 浓缩咖啡篮:精确与常规

> 原文：<https://towardsdatascience.com/espresso-baskets-precision-vs-regular-de9548e72bd0?source=collection_archive---------15----------------------->

## 咖啡数据科学

## 探索大多数机器的简单升级

许多浓缩咖啡机最简单的升级之一就是过滤篮。在过去的二十年里，精密过滤器改变了浓缩咖啡的领域。在这篇文章中，我看了我的杠杆机(Kim Express)的原料过滤篮和一个 [Pesado](/espresso-filter-comparison-pesado-vs-vst-18a1321e62d) 精密过滤篮。

![](img/ecb502869e0984d5798226dc05611be4.png)![](img/01a343979c75fce3fd71ac4b95c37dc6.png)![](img/f0180979f468a6657721f7c42c3e945b.png)![](img/cf5b1ecd145f6574ea3148dc02b4cca4.png)

左:金快递篮，右:佩萨多精密篮

测试本身很棘手，因为普通篮子(16g)比 Pesado (20g)小。所以我做了一些调整:

常规:16g 输入

Pesado: 21g 输入

然后，我的目标是将两者的投入产出比为 1.25，作为一个公平的比较点。

# 过滤孔

我在之前的工作中检查了[过滤篮的孔，结果显示精密篮的孔可变性比标准篮小得多。我根据洞的大小给洞着色，深蓝色的洞较小，浅黄色的洞较大。这两个图像根据各自的平均孔尺寸以相同的比例着色。所以色差代表相对于平均值的差异，这里的 STD 是 Kim Express 的标准差，因为它比 Pesado 大。](/espresso-filters-an-analysis-7672899ce4c0)

![](img/a5379a3a29bb0cb02d5eb8e8b2cf1350.png)

首先，Kim Express 篮子有大约 480 个孔，而 Pesado 篮子有 715 个孔，因此 Kim 篮子上的每个孔对性能的影响比 Pesado 上的大。Kim 篮的球洞尺寸和标准偏差比 Pesado 大得多。

![](img/ccce6af00ccb2e5985d9797c5da8e6c0.png)

Kim Express 中间有一大块不均匀的洞。对于 Pesado 过滤器，局部变化较小，但当您到达边缘时，似乎边缘上的孔变得稍大。这可能是由篮筐底部的滑动曲线引起的，但这些变化远小于金篮筐的空间变化。

# 镜头对比

我们可以先看看视频中的图片。每张图片时间为 5 秒。此外，我做了 30 秒的预输注，并在输注过程中使用压力脉冲。常规过滤器图像在左侧，精密过滤器图像在右侧。

常规的..精确

![](img/e93a23272a32a0cd930ec5958c55f0b0.png)![](img/8278f18c205775bbdca0aaa52a4b0b77.png)![](img/ef2e70600bca9b558b55d0895dab1ff0.png)![](img/9b241357c41bf50d8e554f8310bf5bae.png)![](img/c66cd7baeb5bf1983e997c4866657572.png)![](img/2e1b77389041714a7fcdd34f1e9d2a56.png)![](img/625179db72003f98f9e0c3ef7b634dea.png)![](img/f3fd1fbd0746775b9774db0cf5d9c121.png)![](img/6e5c5c6e16318233c556e185a66a600c.png)![](img/f558dbeaa3fb5054007a9fd695746bea.png)![](img/93722f5b94218186f9ad8e5b364a488f.png)![](img/4e1328b8adb851237b98b7040a2bcaf1.png)

输液开始:

![](img/1497a7589e5eae9dd895cb0a9ae378b4.png)![](img/af3c4b5d4f3d224bf2f9e61d1d3798af.png)![](img/dd85b83fb94d72703ac4f5cf0221f88a.png)![](img/29078d747ff575d5f106412ba19e3902.png)

常规篮子的水流似乎更慢，但它没有精确的油炸圈饼效果那么大。然而，在输液过程中，常规篮的流量似乎更不均匀。

我们也可以看看圆盘的底部，看看暗点能说明什么关于通灵。

![](img/a8fc110a79005c2761b7191c6cb2c3ab.png)![](img/346f7c9a7381cc1efbd5d2d637d2af61.png)

左:常规，右:精确

常规的篮子在右侧有很多暗点，除了左上象限(就在中心的西北方)以外，其他地方都是黑暗的。精密篮子有一些奇怪的地方，但它看起来有点干净。对于常规的篮子来说，这些孔更加明显。

# 绩效指标

我使用两个指标来评估技术之间的差异:最终得分和咖啡萃取。

[**最终得分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6) 是评分卡上 7 个指标(辛辣、浓郁、糖浆、甜味、酸味、苦味和回味)的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难确定。

# 表演

从口味上看，precision 篮子显然是赢家。回想几年前我做这个改变的时候，我的品味有了很大的提高。

![](img/75c95f34a81a63b3fc42effc41eaac9f.png)![](img/1e271d1d06da66957615636af395a981.png)

令人惊讶的是，精确篮的 TDS 和 EY 更低。常规篮子的最终投入产出比为 1.25，精确篮子的最终投入产出比为 1.24。这种比较的一个问题是，由于篮子的大小，剂量差异很大。

就时间而言，普通的篮子需要更长的时间来达到 10 毫升的液体。考虑到剂量差异如此之大，这很有意思。

![](img/e576f4de3235f07de05c148ad9b77c69.png)

# 附加数据

前段时间 Sprometheus 贴了一个[关于精准篮筐的视频](https://youtu.be/XgGWl7UmXZw)，他收集了一点数据。他把它显示在一个表格中，所以我把它扔进一个图表中，看能从小样本量中看出什么。我知道，当品味已经告诉你精确的篮子更好时，很难为 TDS/EY 制作更大的样品尺寸。

第一个图表是比较标准类型和精确类型的散点图。第二张是 TDS 和 EY 的对比图，有助于了解力量和提取的对比。IMS 显然做得不好。就 TDS 与 EY 而言，标准仍然与精确篮子混合在一起。我很好奇，在意大利腊肠拍摄的不同阶段，这些测量值会是怎样的。

![](img/a97bda5345518abaa19e1ac12d968c5a.png)![](img/fe32f76452bda5c77bdfed444ccfa35f.png)

这些数据中有一部分让我不太满意，那就是 IMS 的性能。我之前[在多次拍摄和多次烘烤的情况下，比较了](/espresso-filter-comparison-pesado-vs-vst-18a1321e62d?source=your_stories_page-------------------------------------)IMS made 篮子(Pesado)和 VST 篮子，我没有看到味道或 EY 方面的表现差异。

精确的篮子完全改变了我的浓缩咖啡体验。我会把它们推荐给任何想在品味上有大提升的人。当然，另一面是他们向你展示了你做浓缩咖啡的水平。精确的篮子隐藏不了什么，所以如果你想充分利用它们，你必须提高你的工艺水平。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)和[订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影飞溅页](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

[改善浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

[断奏生活方式概述](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)

[测量咖啡磨粒分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)

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