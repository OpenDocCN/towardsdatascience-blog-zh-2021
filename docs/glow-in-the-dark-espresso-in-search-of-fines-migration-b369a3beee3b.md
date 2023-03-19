# 在黑暗的浓咖啡中发光，寻找微粒迁移

> 原文：<https://towardsdatascience.com/glow-in-the-dark-espresso-in-search-of-fines-migration-b369a3beee3b?source=collection_archive---------23----------------------->

## 咖啡数据科学

## 寻找另一种方式来看待微粒迁移理论

这是我写的第四篇关于微粒迁移的文章。我假设有证据表明，微粒不会通过粉笔实验(T0)和 T2 实验(T3)以及 T4 实验(T5)迁移。

我不打算重新做粉笔实验，直到我的儿子带着黑粉笔回家。我想这可以成为一个有趣的实验，结合用过的咖啡。如果更细小的粒子在整个冰球中运动，黑暗部分的发光会有助于使它们更容易被看到。白垩粉本身的尺寸比 100 微米小得多，所以它是细粉的一个很好的替代品。

目的是在黑暗的粉笔黄昏中，在用过的咖啡渣(直径为 400 微米)上发光。如果细粒迁移发生，它肯定会发生在大的咖啡颗粒上，颗粒之间有更多的空间供细粒迁移。

![](img/59331d8f5c1239d3587ffd67c237e921.png)![](img/a23d7077c1166900b1edd98c008c4733.png)

所有图片由作者提供

回顾一下，浓缩咖啡中的微粒迁移理论是，非常细小的颗粒(微粒)在一次喷射中迁移，然后可能会堵塞过滤器或进入杯中。这种迁移被认为是沟道效应的根本原因之一。

![](img/b2f7c2dd700da5ce6de7d57e25bfa47d.png)![](img/27ab97c69613b26d9b6cf507a013097d.png)

来展示黑暗粉笔在黑暗中的发光效果。

粉笔不容易溶解。有一些研究表明，升高温度会降低溶解度，而升高压力会降低溶解度，但在浓缩咖啡范围内并不多。

在黑暗中发光的颜色通常也不溶于水。

在这两种情况下，干燥地面和随后的液体仍然会让我们看到黑暗粉笔发光。

# 数据收集

我从 18g 经过筛选的废咖啡渣开始，因此大多数咖啡渣的直径大于 400um。然后我在上面加了 0.62g 的粉笔，我还加了一个[金属网筛](https://flairespresso.com/product/flair-58-puck-screen/)减少粉笔回流到我的机器里。

我做了粉笔灰，让它在阳光下晒几个小时。

![](img/a8723159d9b7d170122cc5647dd4cebd.png)![](img/2d3fc4ebd3f9354d9629c5c1baa90f2b.png)![](img/e69c6ad9f730457118a53fa291353620.png)![](img/16fb68bf18828633f70d1e79c7353b61.png)

我启动了金快车，然后开枪了。这是一场泥泞的灾难，很可能是因为提取了所有剩下的东西。我怀疑，由于高流量，这很可能是微粒迁移的最坏情况。最初，我想在黑暗中进行这个测试，这样我就可以看到如果微粒真的迁移了，它们是否会发光。

![](img/d414cf0febc149bdbe8cb6a9676a690c.png)![](img/c2b5f2676ba91ce0a43ff136a0035c0c.png)![](img/ea87f9c1cb968df9afca2d739a118763.png)![](img/4eee4fb88e6dd6d47019097dae2d67a6.png)![](img/0f1ca9a3ce0b7da1a4821f178fb04333.png)![](img/44dde44aeb197a00f2b4c302dd5edd25.png)![](img/b96c4195ac631de3d312a0ab6dfe9be4.png)![](img/5d2871570a25cd66a1106cd43dfa8a8f.png)![](img/badf8cae82af41f5dd2ed7cf6d5bfc5f.png)

经过一番清理，我开始着手分析冰球。

![](img/de43cc1412ea26a6946a548b7ed90718.png)![](img/f0537cf2cc8890ea15f54b8c92664686.png)

# 圆盘分析

出于理智，我想检查一下顶部在黑暗中是否还会发光。因此，黑暗的图像是在一个没有灯光或窗户的房间里用长曝光相机拍摄的。

![](img/633881d437210039dd8e73084375102c.png)![](img/124ada1388a2e48f044aaf717320d621.png)

当我试图把整个冰球取出来时，只有最上面的部分露了出来。完美！冰球的顶部暴露得很好，我在黑暗中拍了张照片。视觉上，没有白色斑点，也没有任何东西在黑暗中出现。

![](img/a7d0cc46f71286af5c907ccea7363995.png)![](img/35d3d527725dcddcafa9ed183a7b5516.png)

我仔细检查了顶部，除了最顶部的颗粒外，我仍然没有发现任何白色颗粒。

![](img/2cb4f7c9b8a3a75155fa5c7aeaeded16.png)![](img/5096a0665c03380219e2c0674a1824dc.png)![](img/7304e4d0c750df743ba0465869e625f9.png)![](img/15ca1e0741834a382a0b685c653c570a.png)

然后我切开那一部分，我仍然没有发现白色斑点，表明没有粉笔穿透冰球。

![](img/19611fa2f34bc1f58e011cd3a03d5941.png)![](img/d7749b962fe7e8093cf3ffe85bb6dd2d.png)

作为一个健全的检查，我弹出了冰球的另一部分，我没有发现白色斑点的证据。在黑暗中，我没有发现发光的东西。

![](img/f7f91bca720068f77218839510967e0a.png)![](img/c28c992dffba3b34d0bd8fa03fb7ff65.png)![](img/393cf1b2506d789edaeb49a516995274.png)![](img/5368c449245dc95d545fc15bec2663bd.png)

# 筛分分析

我拿起最上面的部分，用 400 微米的筛子筛选。有一些粉笔块留在上面，但是很多细小的粉笔颗粒在下面。

![](img/94804cb7db81c5fd65b81ab89e243d30.png)![](img/63f2219f23c8e5f958d65b273e1010e9.png)![](img/71335f7ef7c37b7acd44daffd3ca7a4f.png)![](img/c626936bd3d0272ff8e08cb34fc16659.png)

我用显微镜观察一些粒子只是为了好玩。

![](img/f16bfd2ad28938fb2b53de91a1723bd5.png)![](img/1d1b9b0a71631f91613d29d4c9b50693.png)

另外，我用烤箱烘干了所有的液体。我在黑暗中也观察了这个，我没有在黑粉笔中看到任何发光的迹象。我也没有看到托盘里有任何白色颗粒。

![](img/d9e2fc6c1242daa129811abe61767957.png)![](img/7665182981d921e422197cda64543ef4.png)![](img/e8cfe42d3784a0e9277f138385de8b55.png)![](img/3e027338742b0f12a33557d26247d6d5.png)

我刮去残渣，有 0.5 克残渣，相当于 2.77%的提取率。通常在用过的磨碎物中仍有少量残余物需要提取。

# 超暗测试

我把冰球的底部放在托盘上，我寻找任何白色颗粒，但我没有找到。

![](img/2c02bd2dd9dd1430ee71ab2f4363d82b.png)![](img/25b85c90e69f0573b4f046d91dc127c8.png)![](img/884193e82b8c66f53ec730cfadae5388.png)

我试图用我的厨房灯在黑暗的粉笔中充电发光，但我发现了一个紫外线灯，所以我能够充电并在黑暗中前进。

![](img/4afa209a8162b4beb02c008a0c7dc760.png)![](img/819a7465925d55d54710bf363c7bd2e8.png)![](img/9695221f22e25d0eb73bc973f995e3a8.png)![](img/db3e216665c20629d157d96f47eb0661.png)

对于顶部来说，黑暗粉笔中的光芒仍然闪耀，但对于底部，在托盘上展开，什么也没有。我试着拿着灯走了很多距离，但是什么都没有出现。

然后我对残留物做了同样的检查，什么也没发现。

![](img/67b06fac4e80be7c0ba1d810c962d0aa.png)![](img/e134de2d64ff9a5c039e40386e461274.png)

我做了一个实验，试图用黑色粉笔和粗筛过的废咖啡找到微粒迁移的证据，但我没有找到证据。我尝试了多种方法来检测证据，在常规光线和紫外线下进行视觉检查，但我还是没有发现粉笔穿透冰球的任何证据。

> 结论:没有足够的证据表明微粒会迁移。

很有可能我错了，做了一个糟糕的实验，但是这个实验很简单，其他人可以重复甚至改进这个实验方案。我欢迎这样的测试。

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

[咖啡评论与思考](https://rmckeon.medium.com/coffee-reviews-and-thoughts-splash-page-ca6840eb04f7?source=your_stories_page-------------------------------------)

[咖啡实验](https://rmckeon.medium.com/coffee-experiments-splash-page-671a77ba4d42?source=your_stories_page-------------------------------------)