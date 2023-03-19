# 小众零到底有多零？

> 原文：<https://towardsdatascience.com/how-zero-is-the-niche-zero-d823101f4829?source=collection_archive---------22----------------------->

## 咖啡数据科学

## 理由保留的数据综述

我买了一个[小众零](https://www.nichecoffee.co.uk/)，零代表零留存。这样做的目的是，如果你将 20g 咖啡豆放入研磨机中，你将得到 20g 咖啡渣(不是 19.9 或 20.1，而是 20)。我很好奇这台研磨机能达到多接近零。

![](img/ec1a216ab371b90649887c2167e3f035.png)

我喜欢数据，我发现数据对我的咖啡体验有很大的帮助。所以我开始收集一些数据。如果我们定义 0 到 1 个小数位，小生境 0 实际上是零保留。损失量在 0.05g 左右，这已经很不错了。

![](img/797da157f6937008baf60b332aa59ce1.png)

所有图片由作者提供

# 数据分析

我们可以一起看看研磨设置，因为我已经运行了不少。似乎研磨设置对研磨保持力没有影响。有时，输出的重量会增加，这是由于之前运行的研磨物加入到新运行的研磨物中造成的。我试图在每次研磨后清洗，但这并不总是抓住一切。

![](img/f2257588c1eebf4bce8ea4b9ac8f131e.png)![](img/7b5eb71a7f58d9cbfe8af731ce61acc3.png)

我也看了研磨率。这与研磨设置成线性关系，如预期那样，有一些异常值。

![](img/930aae1ee731c1e325707dad22837ac2.png)

主要的异常值来自两次烘烤。在这两种情况下，研磨机都卡住并完全停止。我在设置 5 时研磨 40 克，然后在设置 15 时研磨 40 克。第五集有些问题，速度慢了一点，我不得不加快一点。然而，对于设置 15，它很快就卡住了。

我很好奇粒子分布中是否有一些有趣的东西，但看起来相对正常，如下所示:

![](img/bc68584525a61b5fdc6d93662a03dd0a.png)

我还查看了几周内的留存率，以及每周的平均损失，大多数损失低于 0.1 克。

![](img/d8593ab453901597995720e4311d0288.png)

# 清洁

至于清洁，在写这篇文章的时候，我决定要清洁我的机器。我有一个习惯，该打扫的时候不打扫。这只花了 5 分钟，在过去的几个月里，我已经磨了将近 4 公斤的咖啡。

![](img/d1452020f7a18ba6e6757b780693169b.png)![](img/9f303d18e354a015a80a036ea80cdd52.png)

我把找到的咖啡量了一下，一共是 0.86 克咖啡。我用的是纸巾，里面残留了一些，所以最多 1 克咖啡。一些这种滞留可能是由于使用壁龛研磨用过的/干的粉末，但大多数看起来是正常的。

![](img/e6244522d8bb24fabb42a1e9a04dc783.png)![](img/4016dc18193534fb77a7ea995f4fece3.png)![](img/0f27313184cb2a6b188add7542fd6f97.png)![](img/acfbb43d68c24c595cdbd99b34c08c38.png)

它看起来很乱，但它是非常简单的清洁过程。

![](img/51cbb7ef2273093471aa4f7f3c1e379f.png)![](img/dcae8dc6555b1e40b4451b3ec6b753ae.png)

后来，我的校准改变了一个点，这可能是因为咖啡将毛刺相互推开了。

![](img/fce6e4ffdebf6a201ca431bde4462cf6.png)

在没有空气吹过系统的情况下，生态位零点是人们可以预期的零点。在各种研磨设置下，平均保持力为 0.05 克，其表现比我想象的要好。我非常喜欢它，我希望他们能推出平毛刺版本。

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