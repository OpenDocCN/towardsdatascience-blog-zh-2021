# 罚款迁移的丧钟

> 原文：<https://towardsdatascience.com/the-death-knell-to-fines-migration-8909c1deb2b4?source=collection_archive---------50----------------------->

## 咖啡数据科学

## 寻找答案

在 espresso 中，微粒迁移是一个没有太多支持证据的概念，但这个想法仍然存在。主要概念是小于 100um 的非常细小的颗粒在喷射过程中迁移到圆盘的底部。在这个过程中，它们不仅会从圆盘中出来进入咖啡，还会堵塞过滤篮的孔。

我之前已经用粉笔展示了[数据，以表明微粒不会迁移很远，我还展示了一些数据，表明](/fines-dont-migrate-during-an-espresso-shot-1bb77e3252ca)[咖啡膨胀](/debunking-fines-migration-in-espresso-989f486eef0e?source=your_stories_page-------------------------------------)会阻碍微粒的迁移。在这项工作中，我想看看如何罚款可能堵塞过滤器使用筛选和废咖啡。

> 理论:细料将首先被完全提取，因此水将在没有大阻力的情况下流过它们。

我的一般理论是，细颗粒比大颗粒提取快，脱气快，因此，水流过它们时没有阻抗。水流过它们的结果是它们不太可能在圆盘中迁移。我第一次看到这个证据是在做实验用的废咖啡渣的时候。不管颗粒分布有多细，水都会流动得很快。

# 实验设计

为了完全理解这些微粒，我需要分离它们。我是用筛子做的。具体来说，我用了一个 [Kruve](https://www.kruveinc.com/) 筛，有两个筛:400 微米和 200 微米。目标是生产两批咖啡渣:

1.  400 微米>咖啡渣> 200 微米
2.  咖啡渣< 200 微米

![](img/69fabada1f8bd3fa23293212a7ee07cf.png)

所有图片由作者提供

对于任何尝试过更细筛的人来说，他们都知道很细筛的困难。咖啡，甚至是用过的咖啡，都是结块的。通常，我用 400 微米和 500 微米的筛子筛选断奏镜头。我可以以 3g/分钟的速度筛选。要去 200um，我只能在 15 分钟内勉强得到 1g，所以我必须随机应变。

![](img/501b54544f5869471a6a591af36027cb.png)![](img/ff3618140d4b64a46f5742b865b33cb5.png)

即使在用勺子筛选之后，我也不认为我能从大于 400 微米的较大颗粒中分离出所有较细的颗粒，但主要目的是得到小于 400 微米和小于 200 微米的颗粒。这也说明了测量技术中的一些误差，因为当筛分时，两个最小的轴定义了颗粒是否通过筛子。同样清楚的是，我没有把所有小于 200 微米的颗粒从 200 微米到 400 微米的颗粒中分离出来。

我可以在大于 400 微米的粒子分布中看到这一点，其中有许多小于 400 微米甚至 200 微米的粒子。

![](img/bf8c8ccc12b18a26b20e37d94432e664.png)

不管是什么情况，我已经筛选了足够的材料来进行测试。

![](img/e37e1305ed1817de3e9305a1a4b6d437.png)![](img/24869907219b8675e00d65572501f4c8.png)

左:200 微米

# 200 微米到 400 微米之间

对于这两个镜头，我使用了超过 1 公斤的压力硬夯。地面不太容易压缩。

![](img/d027d47c499a2aaf8174813c1d6c8773.png)![](img/9ce57fa5e823abfe1e5298c3f07352d0.png)![](img/199d1dd2935b845eb5c25dae3873ac78.png)![](img/03a5faacbbde686b9ce17e4d49114aa3.png)![](img/d660e70e3c21e4012ef889f1e07c9a5f.png)![](img/4df68738ea2dc3133ac0fdd0529d069b.png)

球拉得比没拉慢一点，冰球漂亮又均匀。

![](img/4f02bc5f66dc1e6961a8748cb52eff35.png)

# Twitter 和 [YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw?source=post_page---------------------------) ，我在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以在[中](https://towardsdatascience.com/@rmckeon/follow)和 [Patreon](https://www.patreon.com/EspressoFun) 上关注我。

![](img/c4263741394d5ae828a1515e63666d0f.png)![](img/92cd83776e35f14f561ebe14f6c76a60.png)![](img/1b101185de0a88452142c47755fdecce.png)![](img/1ffbfc0c0846e3fc3a619c0eeed4620a.png)![](img/b67ebb8fdf50a63d26711b58456ac7d0.png)![](img/8b80ea8e3a5ab707468317e4c1107bb4.png)

[我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

# [浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

![](img/9ae95d7cb2c22a5b95eb316266d23020.png)![](img/e9f42d8004d3443d72e4a1a1cd499f9b.png)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事首页](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影飞溅页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

# [改进浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

[断奏生活方式概述](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)

[测量咖啡磨粒分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)

[咖啡萃取](https://rmckeon.medium.com/coffee-extraction-splash-page-3e568df003ac?source=your_stories_page-------------------------------------)

[咖啡烘焙](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea?source=your_stories_page-------------------------------------)

[咖啡豆](https://rmckeon.medium.com/coffee-beans-splash-page-e52e1993274f?source=your_stories_page-------------------------------------)

[浓缩咖啡滤纸](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)

[浓缩咖啡篮及相关主题](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)

[意式咖啡观点](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)

[透明 Portafilter 实验](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)

[杠杆机维修](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)

[咖啡评论和想法](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)

[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)

[Paper Filters for Espresso](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)

[Espresso Baskets and Related Topics](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)

[Espresso Opinions](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)

[Transparent Portafilter Experiments](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)

[Lever Machine Maintenance](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)

[Coffee Reviews and Thoughts](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)

[Coffee Experiments](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)