# 生产用于实验的优质废咖啡渣

> 原文：<https://towardsdatascience.com/producing-good-spent-coffee-grounds-for-experiments-637dcd9f4c53?source=collection_archive---------26----------------------->

## 咖啡数据科学

## 主流之外的探索

之前，我已经用用过的咖啡渣做了实验，给出了一个控制变量，以测试咖啡也能过滤的理论。然而，麻烦的是，用过的地面不像新鲜的地面，我不完全明白如何到达真正好的用过的地面。

我将在这里探索我是如何得到一套相当不错的咖啡渣的，这些磨粒将成为一些实验的基础，以更好地了解咖啡。

# 期望的属性

好的用过的咖啡渣应该能够用在浓缩咖啡篮中，并且与好的咖啡渣有相似的镜头:

1.  没有或几乎没有咖啡萃取物了
2.  水流均匀
3.  与常规注射相比具有相似的流速

用过的咖啡渣有两个挑战需要克服:

1.  二氧化碳消失了。
2.  咖啡被提取出来了。

# 初始配方

我最初的食谱是从一个用过的咖啡球开始的:

1.  混淆
2.  加入篮子
3.  运行 3 或 4 个镜头，直到颜色变得清晰
4.  拉出冰球
5.  放在托盘上
6.  将托盘在 200 华氏度的烤箱中干燥 20 分钟
7.  随时可以使用

干燥后，你需要两个用过的咖啡壶中的咖啡渣才能得到一杯咖啡。所以 20 克的投入将产生大约 10 克的干废咖啡。

![](img/7b88d93534a7e120c82a04d081e88103.png)![](img/66b30716f11fe810e2002a7ae99d8bbe.png)

所有图片由作者提供

我使用了 Flair 的 Bplus 金属过滤器来帮助处理淋浴屏幕。我不知道，我做得还不够。

![](img/ed9d8e967d62ad47ff127d8d1039fdcc.png)![](img/78225320927f928b656fa62ff1f5bede.png)![](img/1f14f093fbd57c10b4713b5d47c03ca7.png)![](img/4891d2ba24da6a81ce326072f7064ad2.png)

# 麻烦

问题是，现在我的镜头跑得超级快。在预灌注时，我的注射花了 5 秒钟装满杯子。

让我们再试一次。把咖啡弄干，然后挤一挤！

![](img/842f9191ae8beb4befdcd16aabc5e7d2.png)

我想可能是咖啡结块了，但这不是问题所在。

# 咖啡分销

所以我做了一些测量。我们可以很容易地重新研磨，但如果不知道研磨后的样子，那就有点盲目了。我测量了分布，并将其与利基市场上的典型 S13 进行了比较。

![](img/074399d496892b9dab3e35f5092cc666.png)

控制转向更精细，但我想可能是咖啡块以奇怪的方式结块。所以我用一个杯子打碎了较大的团块，然后我又拍了一张照片。

我比较了三者的颗粒分布，当粉碎的地面稍微变细时，有一些结块。

![](img/af759dded387af32e85c117bcce12aa5.png)

这仍然没有达到目的，所以我把它磨得更细。首先是 Lume，但是太慢了。于是我转行做了小众(父亲，请原谅)。事实上，我花了一分钟试图想出一个不涉及利基的解决方案，但唉，我知道它会做得最好。

![](img/5c030492230239510db6c61a0c91e4fb.png)![](img/41d29114aebe091bef43a2a37674996d.png)

这有助于稍微改变粒子分布，但没有我希望的那么多。

![](img/5180dd23773fc570feb6b01575fc5b92.png)

在拍摄过程中，它跑得更长了一点，这是一个好迹象。

![](img/7cb57d1cb27be78c9760faeb85fe74c6.png)![](img/688e880f1beb664a4445dbaf80ee63be.png)![](img/afe29e22941d8723797a7c59940b1e1c.png)![](img/a6cd9036c36a39418b0b327ac9bb8cdd.png)![](img/4eeb3fefd33f6b88edd8113cc5aaf40b.png)![](img/ef7aa929bcc6b1f788caed01839ff98f.png)

之后，用过的冰球看起来与正常冰球不同。

![](img/2d050070fa4c6c14f4eab21e20087cd4.png)

# 数据分析

从这些照片中，我看了一些数字，看看这些照片拍摄了多长时间，这些场地里还剩下多少咖啡，尽管它们被认为已经被用完了。我把照片分成两半来帮助理解，它们大概是 1:1 和 2:1 的咖啡投入产出比。所以这些是 20 克进的，前半部分是 20 克出的。这些镜头的问题是，因为它们跑得太快，很难切换杯子，即使下面有刻度。

对于拍摄顺序，这是在拍摄之间使用相同的地面和干燥的顺序:

1.  控制镜头
2.  输出物干燥并捣碎
3.  在壁龛上使用设置 0 (S0)进行更精细的研磨
4.  再送他们一次

对于拍摄时间来说，更好地利用利基市场有所帮助。覆盖过滤器(TCF)的时间增加了一点，击中前半部分和后半部分的时间也增加了。

![](img/a46d1b66ef62310fee8eb99481960020.png)

就温度输出而言，它似乎相当稳定。对于后半部分的“控制干燥和捣碎”来说，输出重量是关闭的，但是除此之外，我能够获得相对的一致性。

![](img/0a2fa934768108819b6242a977ce723a.png)

还剩下一点摘录。这背后的整个想法是了解什么提取物被留下，当使用这些理由进行实验时，我如何估计误差。咖啡萃取物的总量很少，但在重新研磨后迅速增加。这就是再多拉一杆的动机。

![](img/2a2a50fb79f7c06f6369952ff04905ba.png)

这些结果表明，即使地面已经彻底运行，重新研磨和推动水通过他们几次是可取的。我期望从咖啡中完全提取出所有的东西，这需要做更多的工作。

最令人惊讶的结果是，即使地面很好，水还是流得很快。这似乎表明，沟道效应是由圆盘的某些区域被更快地抽出而引起的，直到水以同样快的速度流过它们。这也表明，即使微粒迁移到过滤器的底部，它们也不会阻碍咖啡的流动。我将在另一篇文章中更好地探讨这个问题。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以在[中](https://towardsdatascience.com/@rmckeon/follow)和 [Patreon](https://www.patreon.com/EspressoFun) 上关注我。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影启动页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用图像处理测量咖啡研磨颗粒分布](https://link.medium.com/9Az9gAfWXdb)

[改进浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)

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