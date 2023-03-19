# 横断面浓缩咖啡圆盘切片

> 原文：<https://towardsdatascience.com/cross-sectional-espresso-puck-slicing-4caa57366e3c?source=collection_archive---------22----------------------->

## 咖啡数据科学

## 探索拍摄后的粒子分布

从准备到扔掉用过的冰球之间的内部情况来看，浓缩冰球是一个相当神秘的东西。虽然理论上粒子分布在圆盘内移动，但是还需要对圆盘进行更多的解剖来检查粒子移动。我的目的是观察一个浓缩咖啡圆盘的横截面，以帮助确定颗粒从上到下的不同。

我从金快车上的普通镜头开始。一个根本性的挑战是水如何进入淋浴屏幕的固有问题。另一个是我想尽可能多地提取。我通常将 1.3 比 1 的输出与输入比例拉至大约 22%的提取率，所以我通过圆盘推动了更多的镜头，以获得与大多数人更相似的输出率。

# 圆盘切片

浓缩咖啡圆盘坚硬但易碎。我最终做了一些尝试，尝试简单地切冰球的可行性。我开始用一把非常锋利的刀，但这无助于脆弱的本性。我尝试了对角线切割，但切割仍然让事情以不受控制的方式分崩离析。

然后，当我从一个摩卡壶里倒空一个湿冰球时，我有了一个启示；干咖啡渣易碎，但湿咖啡渣粘在一起。在拍摄我真正感兴趣的照片之前，我试着切了一些湿冰球。

我慢慢地从上到下弄湿冰球，以确保它是湿的，但不是湿透的。

![](img/987b240f6f759f795a88c0842e810f69.png)![](img/3d353766755eeb4f87935fc25a9d3348.png)![](img/55b7edf60bf18d50736134206d6cd18d.png)![](img/456d6505f4a2d1b40914fdf797c99473.png)![](img/2db1332d4994d9def5e70a3f6d733f2f.png)![](img/03226773cafd454f2a6b23915dfcbc77.png)![](img/70229f49b3d963618164da43356e4c3b.png)![](img/101816c44ea998f1d7c38ee87c318c63.png)![](img/fe6389f2288565a4c73476dc6a8ffc6b.png)

然后我斜着切。

![](img/3e48b1429f7bb162ff8a12461f933ec7.png)![](img/756ca8ccb976f4e316e560e921e09853.png)![](img/b9cca043f38093c8909ae48a1108ee64.png)

我的目标是尽可能多的切割冰球。我最后做了 7 层。

![](img/1c8423e6634c41f94be1ace570235996.png)![](img/18f2c4c937c0c8b0c0cd220496feb103.png)![](img/c9de3c13f7075d22c913f3aa5b3cec4b.png)

我让这些伤口干了一夜，它们很快就干了。

# 考试日

我决定分别收集圆盘外部和内部的数据，所以我将外部的数据从内部的数据中切掉。此外，我测量了 gTDS，以了解还有多少可溶物留在地上。

![](img/1a77f6bba268c959618e83e4b8e3dec0.png)![](img/4fef53d3a17215c03a636147ecea072a.png)

从 gTDS 的角度来看，大多数值都很低。唯一的上升趋势是，当你接近底部时，内部 GTD 上升，但幅度不大。

![](img/af2cd7f1170079ce4104db06fe8c11f4.png)

# 粒子分布

我查看了[原始粒子数](https://medium.com/nerd-for-tech/measuring-coffee-grind-distribution-d37a39ffc215)，而不是估计它所占的体积。结果，更细的颗粒占更高的百分比，只是因为有更多的颗粒。这是内外圆盘的综合结果:

![](img/c057c8d92e1281889113a46867a5e989.png)

有一些细微的差别，所以让我们仔细看看，从外层测量开始，只有顶层、中层和底层:

![](img/f49d63e1445f1369f0b71579239cdcf2.png)

同样，这些也遵循相同的趋势，L7 或圆盘底部的较细颗粒略有增加。但是，L4 并不在顶部和底部的中间。

查看内部测量，我们看到一个类似的模式。分布中似乎有很大的重叠，除了 L7 在 90 微米处有一个轻微的凸起外，没有明显的趋势。

![](img/c14c90a0afa07ea9379aee52f4958bf7.png)

我们可以看看组合测量，模式是相似的。

![](img/a06145269c61f5d6065ea71f73792cb4.png)

# 关注其他层

处理测量误差和了解趋势的一种方法是查看不同的层，看看分布中是否有趋势和变化。我们可以从顶部(L1)到 L4 开始，把 L4 到 L7 放在下面。

![](img/97a9fc0b4e22767a1712db2070dc6c37.png)

这两者都没有显示出任何趋势上的层与层之间的转移。

我们可以用下面几幅提供类似结论的图来分割这些数据。

![](img/ccdd296eeb306efee5a445df0cbf337e.png)![](img/6b17e4f62119d7ff05580c29a6d48f90.png)

总的来说，这个实验表明粒子在整个圆盘中的分布是相对相同的，并且似乎没有因为浓缩咖啡而在两个方向上迁移细粒子或粗粒子。细颗粒迁移应该导致底部细颗粒的增加和顶部细颗粒的减少，但是没有观察到这种类型的模式。

很有可能存在测量误差，我更希望对每个样品进行多次测量，但我时间有限。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)和[订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影飞溅页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

[改善浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

[断奏生活方式概述](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)

[测量咖啡磨粒分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)

[咖啡萃取](https://rmckeon.medium.com/coffee-extraction-splash-page-3e568df003ac?source=your_stories_page-------------------------------------)

[咖啡烘焙](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea?source=your_stories_page-------------------------------------)

[咖啡豆](https://rmckeon.medium.com/coffee-beans-splash-page-e52e1993274f?source=your_stories_page-------------------------------------)

[浓缩咖啡用纸质过滤器](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)

[浓缩咖啡篮及相关话题](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)

[意式咖啡观点](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)

[透明 Portafilter 实验](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)

[杠杆机维修](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)

[咖啡评论和想法](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)

[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)