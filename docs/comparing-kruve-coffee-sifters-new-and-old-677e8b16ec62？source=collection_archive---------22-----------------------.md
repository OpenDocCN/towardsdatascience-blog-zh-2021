# 比较克鲁夫咖啡筛:新的和旧的

> 原文：<https://towardsdatascience.com/comparing-kruve-coffee-sifters-new-and-old-677e8b16ec62?source=collection_archive---------22----------------------->

## 咖啡数据科学

## 惊人的发现

[在之前的](https://medium.com/overthinking-life/kruve-coffee-sifter-an-analysis-c6bd4f843124)中，我已经使用孔分析检查了 [Kruve](https://www.kruveinc.com/) 过滤器。我发现它们比目前市场上的任何网格都要好，我怀疑这仍然是真的。当时，我只有几个屏幕。我最近买了整套，我决定对整套做同样的研究。

我也有一个独特的机会来比较 400 微米和 500 微米的筛网，从原来的 Kruve 到目前正在制作的筛子。

很快，我注意到他们在屏幕的质地上感觉有点不同。我对比了几张照片，发现分布立即发生了变化。

![](img/62e6d48eda479848c72e38983ffd4b9b.png)

所有图片由作者提供

所以我首先用显微镜来观察屏幕。

# 400um

![](img/acebeb8d6042cb194a344cd1c277902f.png)

400um，顶部/底部:宽/放大；左/右:旧/新

# 200um

![](img/4d48a4cf15359dc9ed1130926864fe1c.png)

200um，顶部/底部:宽/放大；左/右:旧/新

新的孔看起来有锥度，这很好，因为咖啡更容易通过。似乎新屏幕功能更好。

# 孔洞测量

我用图像处理来测量这些洞。我把每个屏幕放在一个明亮的屏幕上，然后我拍了一张照片。然后，我隔离屏幕，测量所有的洞，了解地面的真相，将像素转换成现实世界的坐标。

![](img/ab086edf96da5742a5149f2a00f86166.png)![](img/63f14731fbcb1bc533327ee23cc0a40a.png)

我用两种类型的图来表示这些数据:箱线图和彩色分布图。以下是箱线图在点数分布方面的含义:

![](img/280c40679ba3ca65be8f8218b67009a5.png)

数据有明显的变化。这可能是由几个因素造成的，但考虑到这种趋势的线性程度，我怀疑这要么是阈值，要么是金属以一种奇怪的方式反射的光。

![](img/01940306a2c1c2f2480ab3350e3758f4.png)![](img/6b604dc2db04db378ef10c3e08e8c258.png)

所以我用这个最佳拟合来调整数值。过去的每个屏幕仍然有一点重叠。大部分的洞都有相同的大小，这种变化的一部分可能是由于相机的角度。

![](img/902d2c16e23de36c3e23687661093d84.png)![](img/f226edfadd03ca95ab6985ecd08aa203.png)

所以我用代表孔大小的颜色制作了过滤器的图像。这个假颜色显示了最大和最小的孔尺寸。许多屏幕上都有奇怪的图案，我不知道为什么。但它确实存在，我拍了多张照片来验证它。我不认为它影响性能，它可能是封装中屏幕上的一些胶带在表面上的反射。

![](img/cf451bd9707cd51b503864a855692e62.png)

黄色表示最大尺寸，蓝色表示最小尺寸

最后 5 个屏幕有奇怪的效果。某些影响不是由图像的缩减采样引起的。我把 600um 和 200um 的屏幕放在这里做参考。我怀疑是制造过程的一部分造成的。

![](img/f6de5c7805cfebff14e4ab923c5f4d1a.png)![](img/bb096e93a98d6dcfc7ced1fbfe3177a0.png)

左:600 微米，右:200 微米

# 旧与新

我只是对比了新旧屏幕。旧屏幕是克鲁夫第一次筹资时的。这些图在测量上有线性调整。对于 400 微米的屏幕，有向较大孔的轻微偏移，而对于 500 微米的屏幕，有向较大孔的较大偏移。

![](img/27452c3851b6b948a849b8aa30d29305.png)![](img/0e56cb053074ed5460c558dfb6d46bc7.png)

# 只有孔尺寸的变化

我只看了孔尺寸的变化，因为孔尺寸的变化随着孔尺寸的变大而变小。

![](img/5cc8a52cd889ce61c4c1cda35a02d418.png)

我们可以跟踪标准差，除了最初的几个筛子之外，还可以看到一个线性趋势。这是一个很好的迹象，表明筛子是在严格的质量规范下制造的。

![](img/e1970294e7f38b7042173d574734665e.png)![](img/8b09775fa33ef72cbb82738b9a5ee16f.png)

Kruve 最初的目标是帮助分离特定的研磨尺寸，并允许人们修改研磨分布。然而，这个工具改变了我的浓缩咖啡体验。首先，它在制作[断续](https://medium.com/overthinking-life/staccato-espresso-leveling-up-espresso-70b68144f94)的镜头，但后来我开始用筛子做咖啡实验。克鲁夫筛对于理解咖啡非常有用。我曾多次用他们的筛子来试验和[了解](/fines-in-coffee-grinds-searching-for-a-source-326c3eba2bb4)咖啡。

从旧筛到新筛的分布有一些变化，但筛子仍然工作。

主要的收获是，这些孔的分布可以让你更好地了解任何两个滤网之间有多少重叠。

我确实在[克鲁夫](https://www.kruveinc.com/)提供的 30%折扣上买了克鲁夫。公平地说，几年来，我一直是筛选咖啡的大力提倡者，所以这不应该是一个很大的惊喜，但我想确保提前。

如果你愿意，可以在推特、 [YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw?source=post_page---------------------------) 和 [Instagram](https://www.instagram.com/espressofun/) 上关注我，我会在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我在[中](https://towardsdatascience.com/@rmckeon/follow)和[订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[我未来的书](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

个人故事和关注点

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影飞溅页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

[改进浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

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