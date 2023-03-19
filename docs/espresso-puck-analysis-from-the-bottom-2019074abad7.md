# 从底部进行浓缩咖啡圆盘分析

> 原文：<https://towardsdatascience.com/espresso-puck-analysis-from-the-bottom-2019074abad7?source=collection_archive---------43----------------------->

## 咖啡数据科学

## 拍摄后分析以读取浓缩茶叶

我提取浓缩咖啡的方式可以被描述为数据收集的漩涡。最初的几个变量扩展到包括对冰球底部成像。这对于某些类型的局部通灵的镜头诊断很有帮助，但我很好奇图像细节与品味和提取的一致性有多强。

相当一部分人会在拍摄后在网上发布他们的冰球顶部，并寻求反馈。冰球的顶部不是很有用，但我发现冰球的底部可以揭示水是如何流动的。

我从经验中看到，更少的暗点意味着更少的通灵，因为暗点通常是对那些区域的低流量的指示，因此对其他区域和通灵有更高的流量。

一些黑斑在表面很深，一些则一直存在。黑点越多，流经圆盘的水流就越不均匀。

# 数据

几个月来，我一直在拍摄冰球底部的照片，所以我抓取了一些图像，并将其与我的数据表对齐。我最终在数据手册中找到了 175 张图片。

![](img/2e122bbca589a67f01e9b4a01661bda9.png)

所有图片由作者提供

我试图保持灯光一致，但通常会有一些灯光伪像。否则，这是一个很容易捕捉到的信息。

# 绩效指标

我使用两个指标来评估技术之间的差异:最终得分和咖啡萃取。

[**最终得分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6) 是评分卡上 7 个指标(辛辣、浓郁、糖浆、甜味、酸味、苦味和回味)的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难确定。

[](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)**使用折射仪测量总溶解固体量(TDS)，该数值与咖啡的输出重量和输入重量相结合，用于确定提取到杯中的咖啡的百分比。**

# **准备图像**

**首先，我需要准备图像，使它们更容易处理。我用一个蓝色的环手动标注了每张照片。我用这个环来定义感兴趣的区域，其他的都涂掉了。**

**![](img/cb44e95272c1e1876a0c0214ad32fecc.png)**

**然后我就可以扔掉任何丢失的棋子，清楚地看到冰球。**

**![](img/0ebc6c44f29daa0366950ab6f75d75de.png)****![](img/befd52739dfe29aca76159ce7e8048cb.png)**

**我开始通过探索色调、饱和度和亮度(HSI)空间来处理它们。在下图中，这个空间用红色、绿色和蓝色(RGB)表示。在该图像中，暗点在色调平面以及强度平面上有明显的偏移。**

**![](img/9f751732dae27720f12eb3e2c595c23d.png)**

**色调和饱和度本身似乎非常有趣:**

**![](img/a1fe81b2a6c1bdbec39749058737efb2.png)**

**色调和饱和度**

**我用平均强度对图像进行了归一化，然后我观察了这些平面的一些圆形切面。我看了每一枚戒指，也看了它们的组合。**

**![](img/71c64a4af975916a6d9e9537ad3db268.png)****![](img/b8ae06f3c088088e09311ce82e7a10f7.png)**

# **相互关系**

**[相关性](https://en.wikipedia.org/wiki/Correlation_coefficient)是衡量两个变量彼此相似程度的指标。高度相关并不意味着一个变量会引起另一个变量，而是当情况发生变化时，两个变量的涨跌幅度相同。我从一开始就假设一些分级变量会有很高的相关性，因为它们是从不同的时间点来看味道的。**

**查看饱和平面上这些环的均值和标准差的多个度量，我没有发现任何相关性强于-25%的东西，这是相当低的。作为参考，味道和 EY 之间的相关性通常在 70%左右。我也没有发现色调或强度平面有很强的相关性，最好的相关性是在饱和度上。**

**![](img/b3ef9b9b69acb7e605fc38d885ddf1cd.png)****![](img/53301030ccad669c96a6eae936fe34ce.png)**

**为了分解数据，我使用了一个我记录的度量标准来描述照片的样子。我将射束流描述为居中的、偏心的、小环形的、环形的、不均匀的或单侧的。这些照片中的大多数都是居中、偏心或小圆环，下面是一些例子:**

**![](img/e7000635431e0165dc0d2d099e5327b7.png)****![](img/37a6886122770413a2c41c5fc83fbf6f.png)****![](img/fdd09bd513f2c96394ee2049b89f5144.png)**

**居中、偏心、小甜甜圈**

**这个描述有点主观，但我想记录一些简单的东西。我最好的照片是居中或偏心的，但这是我第一次在数据分析中使用这个指标。**

**这里有一些突出的指标，都是 STD 指标。居中和偏心拍摄的相关性非常低(abs < 10%)。然而，小甜甜圈照片有更强的负相关性，尤其是对味道而言。**

**![](img/0e8f484a6b033fa50ca9fd71d30430dd.png)****![](img/e7c7b7db0dcb57d18993f2bacb92159d.png)**

**我绘制了累积外环 3 的散点图，它是三个最大环的组合:**

**![](img/15556348b5cdc3866468696b361a0f9f.png)**

**似乎有一些模式，但没有很强的线性拟合。我怀疑小甜甜圈不同于居中或偏心，因为这些镜头几乎没有明显的慢点(冰球上的黑点)。他们的最终提取有点微妙，因为什么可能导致它比另一个镜头高一点或低一点。微小的圆环照片几乎总是具有较低的提取率和更不均匀的提取率。**

**这项研究没有显示出圆盘底部和性能指标之间的强相关性。我从经验中知道，冰球的底部是调试镜头的有用指南，很可能我没有足够的数据来看到更有趣的东西。**

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