# 咖啡粉中的微粒:寻找源头

> 原文：<https://towardsdatascience.com/fines-in-coffee-grinds-searching-for-a-source-326c3eba2bb4?source=collection_archive---------30----------------------->

## 咖啡数据科学

## 一个更好地了解烤豆子内部的机会

磨咖啡的时候，总是要在细粉和巨砾之间取得平衡。他们是不可避免的，即使是高端磨床。这是为什么呢？典型的解释是，由于咖啡易碎的特性，会产生细小的颗粒，而巨砾是最大的颗粒，仍然可以通过研磨机而不会被压碎。

有一种理论认为，豆子的内部比其余部分更脆。因此，细粒首先由内部产生，而巨砾来自豆的外部。如果这个理论不成立，那么罚款应该同样来自外壳和内部。

我决定设计一个简单快速的实验来测试这个理论。如果这是真的，这可以很好地解释为什么[断续镜头](https://link.medium.com/B0glf09kweb)比常规镜头给人不同的味道。它们是相同的场地，只是按层重新排序。然而，如果细粒层中的颗粒主要由豆子内部控制，而粗粒层中的颗粒主要由外壳控制，那么我可以看出提取过程会有什么不同。

![](img/6a8127c7f03d09f1864376220add1867.png)![](img/7edc35c88dd7e17b0392b9cb35edfa0c.png)![](img/24a3e82c9127b0e3930b2fc9bf1ed774.png)![](img/20ff3750b82a8cd389304ff9c6af94e7.png)

所有图片由作者提供

# 硬度检查

我试着用 [D 型硬度计](https://link.medium.com/q0wkk7klweb)测量豆子的内部，我注意到工具会陷入柔软的部分，通常在工具测量到超过 5 之前，豆子就会破裂。然而，如果我测量豆子的外部，我通常会得到 15 到 20 之间的读数。通常，有一个小压痕或豆裂纹，这意味着不同的硬度计类型将更好地测量硬度。

![](img/cd2445efdf5006ef2f63105275eb550c.png)![](img/497339df5ad62aafdd7effe9918406a4.png)

硬度测量

# 实验设计

为了验证这一理论，我想如果我能把粉末和砾石分开，我就能把砾石磨得更细。然后我可以比较两张照片。

我把我的小生境研磨机设置为 50，这应该会给我一个粗略的分布与一些罚款。我用我的[基于图像的技术](https://link.medium.com/8Aq7RAolweb)来确定粒子的体积分布，并且有一些误差，因为我发现一些筛选，但它给出了一个好主意。这里有两个凸起，一个在 300 微米左右，一个在 1100 微米左右，分别代表细粒和巨砾。

![](img/c7bcb53946b502f09a1d7b8f48bbdda4.png)

我用 800 微米和 500 微米的筛子过滤咖啡。我并不关心 500 微米的屏幕，只是综合了一下结果。我结束了> 800 微米和<800um grinds.

![](img/a1b7185c3a1520d26d1a9653cacb834f.png)![](img/0a2300146ac3c337ae5a2eee64a21d31.png)![](img/68c359c48cfa651812871bee41900617.png)

Above 800um, between 500um and 800um, and less than 500um

I put these two separate piles of grounds of these through the Niche on Setting 13, which is my current starting point for dialing in a shot.

![](img/a88be10ae6b6a95fdd4fb76a2245d6a6.png)![](img/6426f50ae21ba160db2d215473932dce.png)

Left: >800 微米，对不对:> 800 微米重新接地在设置 13

此外，我在场景 13 中研磨了一些相同的咖啡豆作为整粒咖啡豆。

然后我收集了一些粒子分布信息。我应该注意到，在研磨<800um coffee, it went through very quickly, and the distributions showed more fines were created, but it didn’t change too much.

![](img/dff1e1f26dbbdf88104c4722d981483c.png)

Comparing the regrind with the Setting 13 (S13) only, they seem to follow relatively similar distributions. S13 has more fines.

![](img/f54d6a07d06bbd66e24dc69102af7084.png)

# Shot Performance Metrics

I used two metrics for evaluating the differences between shots: [时最后的分数](https://link.medium.com/uzbzVt7Db7)和[咖啡萃取](https://link.medium.com/EhlakB9Db7)。

最终得分是 7 个指标(强烈、浓郁、糖浆、甜味、酸味、苦味和余味)记分卡的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难，会影响最终得分。

使用折射仪测量总溶解固体(TDS ),该数字用于确定提取到杯中的咖啡的百分比，并结合一杯咖啡的输出重量和咖啡的输入重量，称为提取率(EY)。

# 数据分析

我看了几张照片。一个标准镜头在场景 13，一个在场景 13 使用大于 800 微米的粒子，一个使用 800 微米的粒子<800um on Setting 13\. The shots definitely ran faster, but all three extracted similarly. I kept the Pre-infusion time the same because that variable is most tightly correlated to extraction.

![](img/ef114ecf4f9dd5b0a48bcdbfa0073594.png)

Taste is where there was a very noticeable difference. The <800um particles had a much better taste, but something seemed missing. However, for the >，它不甜，有点苦。这是我在开发断奏镜头时的类似经历。

![](img/26ee9989e3d633ba72e987eee57d3a74.png)

# 我们需要更多的数据！

我决定我需要用筛子收集更多的数据，看看豆子较软的部分和外面产生了多少细小颗粒。如果内部更容易产生细屑，那么不管研磨设置如何，都会发生这种情况。

![](img/77f03293063d33dab7e51163e1c8d175.png)![](img/952a24058203b6a1c89ad5cb04d7b9e3.png)

我从设置 50 开始研磨，我用 Kruve 筛筛出大于 800 微米的颗粒。我在设置 30 时重新研磨了> 800 微米的地面。然后我把一些豆子放在 30 度的环境下磨碎来做比较。

![](img/91d2c4444fdf1bd9c299581559111589.png)

然后我比较了这些分布，假设这种情况下的微粒低于 400 微米。如果豆子中的任何地方都有相同的导致细粒的概率，那么 800um S30 应该与 S30 具有相同的< 400um 的百分比，但几乎是一半。其中一些可能是由于以前被研磨过，但是对于更多更大的颗粒，分布肯定是不同的

![](img/5967220cf7ffc45264183b6553494c73.png)

我也用我的成像技术拍摄图像来观察颗粒分布。

![](img/9ef8e07eb728b68947e9b3676465cff8.png)

从这些< 400um 的数据中，不太清楚差异在哪里。成像技术与筛选技术不一致，但它更精细。

让我们只关注一下<400um, throw out all other data, and add a few more data points on the lower end of the particle diameter:

![](img/efbf51f5a3467849205fcdeb0b3afdcc.png)

There is definitely a bigger spike for 800um reground near 100um, but the more interesting piece for me is less than 60um where 800um reground is almost zero. If the fines come from the softer bits inside the bean, and those were removed during the Setting 50 grind, then it would make sense to see such a drop off.

I don’t think these results are definitive, but they point strongly at the theory that fines are the softer parts of the coffee. This really helps explain why coffee sifted to different levels tastes different, and this taste difference is the underlying reason why a staccato shot tastes better than a regular shot.

I also suspect that trying to eliminate the fines from coffee, whatever the brewing technique, is eliminating a flavor or multiple flavors not found in the rest of the bean.

If you like, follow me on [Twitter](https://mobile.twitter.com/espressofun?source=post_page---------------------------) 和 [YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw?source=post_page---------------------------) ，我在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影启动页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

[改良浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

[对断奏生活方式的总结](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)

[测量咖啡研磨分布](https://rmckeon.medium.com/measuring-coffee-grind-distribution-d37a39ffc215?source=your_stories_page-------------------------------------)

[咖啡萃取](https://rmckeon.medium.com/coffee-extraction-splash-page-3e568df003ac?source=your_stories_page-------------------------------------)

[咖啡烘焙](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea?source=your_stories_page-------------------------------------)

[咖啡豆](https://rmckeon.medium.com/coffee-beans-splash-page-e52e1993274f?source=your_stories_page-------------------------------------)

[浓缩咖啡用纸质过滤器](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98?source=your_stories_page-------------------------------------)

[浓缩咖啡篮及相关主题](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738?source=your_stories_page-------------------------------------)

[意式咖啡观点](https://rmckeon.medium.com/espresso-opinions-splash-page-5a89856d74da?source=your_stories_page-------------------------------------)

[透明 Portafilter 实验](https://rmckeon.medium.com/transparent-portafilter-experiments-splash-page-8fd3ae3a286d?source=your_stories_page-------------------------------------)

[杠杆机维修](https://rmckeon.medium.com/lever-machine-maintenance-splash-page-72c1e3102ff?source=your_stories_page-------------------------------------)

[咖啡评论和想法](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)

[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)