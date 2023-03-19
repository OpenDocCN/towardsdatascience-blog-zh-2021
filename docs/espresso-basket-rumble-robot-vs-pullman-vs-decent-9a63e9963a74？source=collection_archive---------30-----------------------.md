# 咖啡篮隆隆声:机器人 vs 普尔曼 vs 体面

> 原文：<https://towardsdatascience.com/espresso-basket-rumble-robot-vs-pullman-vs-decent-9a63e9963a74?source=collection_archive---------30----------------------->

## 咖啡数据科学

## 一投命中的精准篮下优势！

我有几个还没有分析的浓缩咖啡过滤篮，我也更新了我的分析代码，所以这是一个好机会。

我要看看机器人、普尔曼和像样的过滤篮的孔径分布。

# 分析

我使用[标准图像处理技术](/espresso-filters-an-analysis-7672899ce4c0)进行滤孔分析。我以前注意到，大多数对篮子进行的分析都是计算阈值以上的像素数量，但我的技术更先进，使用实际的光量来获得一些亚像素精度。

另外，我更新了算法，把所有的洞都改成了圆形。有时它们是椭圆形的。然后我应用了一个全局标准化来解决相机的任何倾斜。这种新算法考虑了大量的相机误差和镜头失真。

# 从底部

这是底部的视图，带有一个手绘的蓝色圆圈，用于孔洞检测算法。

![](img/b6d85b9c91999c2a01338f25ad10fbc2.png)![](img/7b00bbb55d7fedfdca0f90bd48b1637b.png)![](img/414698e3c0706558a615aecc6bf57114.png)![](img/14ce55fbb9ec9d55d5e67f3f99cfbc14.png)

上图(左:机器人；右:普尔曼)、下(左:DE 18g 右:DE 15g)，所有图片作者

# 从头

![](img/19f72eaf626ca6f85bc82451c52705e4.png)![](img/3d558914179242dcb29c8b80d95a37e0.png)![](img/baf6c3140a8c47ac89fab764ab9cee2a.png)![](img/12fcf4bf0be10722eb2b6e226f7fa7b9.png)

上图(左:机器人；右:普尔曼)、下(左:DE 18g 右:德 15g)

我计算了每个过滤器的孔径。

![](img/631f63e2c5cd6f42b3ed0c34a179c57c.png)

虽然箱线图很好，但汇总表显示了一些有趣的内容。我也有孔距。就球洞大小而言，所有的篮筐都有相似的标准偏差，这很棒，因为它们都应该是精确的篮筐。

![](img/756d98a4b5cc1127ba2261d00af15a15.png)

这两个像样的篮子有一个稍微不同的洞大小，特别是从顶部。我不确定这是如何影响流动的，因为孔的数量也不同。由于阈值和原始图像，孔的数量略有不同。成像过滤篮比看起来更棘手。

就有洞的总面积而言，除了机器人之外，它们都有非常相似的面积。这意味着机器人过滤器的流量会受到更多限制，但普尔曼和体面将类似地执行。

我们也可以看一下孔的平均尺寸。对于机器人来说，顶部的孔比底部的大，但对于其他三个机器人来说，顶部的孔更大。这意味着甚至咖啡出口的方式也是不同的。问题是机器人的面积比其他地方小，所以很难知道会有什么影响。

![](img/939cc138419e2c4c8ff699a377289054.png)

我们可以分别看所有的底值和顶值。DE 滤波器的差异很奇怪。我认为洞的大小会更接近。普尔曼有较小的孔直径，但有更多的孔。

![](img/e463e82bc023ac4fd9b9bdeff2ed70be.png)

# 空间分析

我用错误的颜色绘制了每张图片上的洞的大小。这是为了了解孔洞大小的分布是否是随机的，或者某些区域是否存在会导致沟道效应的或大或小的孔洞群？

为了理解下面的假色，颜色根据特定过滤器的最小和最大孔径从 0 调整到 1。

![](img/2cb3b05de60985c53d88c7b47852df7d.png)

示例图像

# 从底部

![](img/1e6c343fafe1a3085ed4dcd02509edc1.png)![](img/17ee5bcfcb82d7b8825ad5db97e590bb.png)![](img/598d6c9f8875e86174662cf0d0b03c1c.png)![](img/375df0838f17aa8118e7fe0ba20f4f51.png)

上图(左:机器人；右:普尔曼)、下(左:DE 18g 右:德 15g)

机器人的中心有一组孔，这可能会导致中心形成更快的流动通道。普尔曼也有类似的问题。对于两个 DE 过滤器，它们都有一些簇，但是孔的大小似乎更随机。

# 从头

![](img/c0a126df882ac7307627a1e9ef8c6340.png)![](img/f500213fb2a13cc37cbfc9a9e4d566b7.png)![](img/009dfb0df0578542e43acb3b8c019ebe.png)![](img/f09d81c3cf621aed6081680aa4a363ad.png)

上图(左:机器人；右:普尔曼)、下(左:DE 18g 右:德 15g)

从底部来看，在空穴分布方面没有任何重大问题。DE 18g 有一些孔在顶部合并。这是由于对特定图像进行阈值处理。过滤篮是金属的，所以它们能反射很多光。因此，摄影的挑战。

![](img/8be99b9a71bdf9941e8d833e56bf3778.png)

总面积似乎是真正区分过滤篮的统计数据。这并不是说机器人篮子更好或更差，而是不同。它将有一个较低的流量，所以必须在两个篮子之间进行调整，以获得类似的流量。

我喜欢看这四个过滤器。这一分析的关键是有非常好的图片。我很高兴过滤器上的孔尺寸模式不是太局限。该试验的主要结论是对流量的预测。似乎机器人过滤篮的总面积较低，与流量成正比。如果同样数量的咖啡以同样的研磨速度进入篮子，那么水通过咖啡的速度就会变慢。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事首页](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影飞溅页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[浓缩咖啡过滤器:分析](/espresso-filters-an-analysis-7672899ce4c0?source=your_stories_page-------------------------------------)

[浓缩咖啡过滤篮:可视化](https://medium.com/@rmckeon/espresso-filter-baskets-visualized-189043a8929d?source=your_stories_page-------------------------------------)

[造型咖啡怪咖](/modeling-coffee-gringers-afb7c4949d6b?source=your_stories_page-------------------------------------)

[浓缩咖啡过滤器对比:佩萨多 vs VST](/espresso-filter-comparison-pesado-vs-vst-18a1321e62d?source=your_stories_page-------------------------------------)

[浓缩咖啡篮(VST):有脊与无脊](https://medium.com/swlh/espresso-baskets-vst-ridged-vs-ridgeless-89ac52767f13?source=your_stories_page-------------------------------------)

[IMS 超细 vs VST:小样浓缩咖啡过滤器对比](/ims-superfine-vs-vst-a-small-sample-espresso-filter-comparison-4c9233e194?source=your_stories_page-------------------------------------)

[浓缩咖啡模拟:计算机模型的第一步](https://medium.com/@rmckeon/espresso-simulation-first-steps-in-computer-models-56e06fc9a13c?source=your_stories_page-------------------------------------)

克鲁夫:对技术状态的进一步分析

克鲁夫咖啡筛:一项分析