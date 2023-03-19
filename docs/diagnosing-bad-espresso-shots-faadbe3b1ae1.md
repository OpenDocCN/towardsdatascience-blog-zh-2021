# 诊断糟糕的浓缩咖啡

> 原文：<https://towardsdatascience.com/diagnosing-bad-espresso-shots-faadbe3b1ae1?source=collection_archive---------21----------------------->

## 咖啡数据科学

## 数据样本会告诉你什么

休斯顿，我们有麻烦了。我的提取率突然下降。我不知道为什么，但我需要调查。从神射到沉射(我说的沉，是指我的嘴)，有一种相当绝望的感觉。我想我最好做一个快速的记录，以防其他人对我如何诊断问题感兴趣。

# 镜头设置

在我说发生了什么之前，我们先来谈谈那次枪击。我不拍普通镜头，也不用普通工具。我进行断奏击球、断奏夯实击球或断奏击球。这些都是分层的镜头。

此外，我在镜头的底部和中间使用了一个[布过滤器](https://rmckeon.medium.com/paper-filters-for-espresso-splash-page-f55fc553e98)，这改善了味道和提取。这也让我有更多的信息点进行故障分析。

除了其他变量之外，我还跟踪每一次拍摄的提取率和口味评分。

# 发射失败

我注意到提取率(EY)和味道下降，我开始修改不同的参数来改善 EY。我的大部分镜头都没用，无论我做什么都没用。

我开始给冰球底部拍照，拍了几张之后，一个清晰的图案出现了。这表明底部滤布过滤器出现故障。我对布过滤器没有年龄方面的问题，当我发现这一点时，过滤器已经使用了 50 次。我之前用了 100 次布过滤器，拍摄质量没有下降。

![](img/a2ff2f07f1d70a7c29e1c70e131256e4.png)![](img/ee3fb0c73ae0ffb1fa5d935dcce41a2c.png)![](img/47d10b6ecdc0470f8565441ddd5e52ab.png)![](img/c51d413b43129d18b2ea15fe6e988042.png)

我甚至试过在沸水和异丙醇中清洗过滤器，但没有用。

![](img/ab3b57f0e5b4667c80bcf1ba2b6a4e2c.png)![](img/5eeae04d84545332c8c6b7aff7afe5f3.png)

左:将滤布过滤器放入篮中的常规方向。右图:以常规方式上下颠倒。

我让水以常规方式和倒置方式通过过滤器，结果差别很大。

## 常规取向

![](img/8c7e2f5c94ab73d688dbd7e8c22c9354.png)![](img/62ff004c51439e1d7de9f4126facf353.png)![](img/ca6b14f7049a44b3b09ce0020db5c9c6.png)![](img/ed9179555ed1749e5dd3d58f9f7278f9.png)![](img/9c897e94c8a00957b14bab63226c4c31.png)![](img/d3e692d41bf3ea6452979fa9fb5fd645.png)

## 颠倒方向

![](img/47494194557a52b4cfe45e73d5c35e92.png)![](img/9e41bd7653905c77c6cd1ebdfde5ed15.png)![](img/0deb660d36ebe339c6acbf43d90e65fb.png)![](img/85d6611ada6028384f93104ef4949530.png)![](img/6d90b42a06fa93f7440cdbc17e388130.png)![](img/2802bc04c1e0bc0a666265d53fa9516f.png)

# 查看数据

我回去查看了 Extraction Yield (EY)，因为我想弄清楚哪里出了问题，因为它并不是突然出了问题。我很容易看出下降的趋势。

![](img/606c33931f3d99c446362a5a2764993d.png)

我仔细看了看照片，发现有一个镜头是关着的。EY 是 17.9%，但味道评分是前几个镜头的一半。这是多次烘烤的混合镜头。我喝四杯一种的烤肉，然后换成另一种，我积累的量足够一周一次混合一杯。这是一张有趣的百搭牌。

我调出了下面的图片，显示咖啡从一边流出的速度比另一边快得多。

![](img/b6fa7e640db9b916afbfef8a03e8e96d.png)![](img/85cdbaf6c8d3c523f89b0bff22364ad5.png)![](img/65efc709cbb9bf0ab721421a6dc7849e.png)![](img/bccbe177a86460dc96668fe28f97d257.png)![](img/712fed1e789ac0e87b5e39e92471a15b.png)![](img/38f1e477218e5efa3add93636314b614.png)

这种模式似乎与我之前看到的有问题的模式一致

![](img/7301d48d7ae538c263699996a4d1343c.png)![](img/dadb681c9f9e0364da383cae6fbc5c28.png)

对于混合拍摄，我拍了滤镜的照片，但我没有拍滤镜下面的照片，这可能会混淆问题。

![](img/819497a944d215a70b1e5196f9114681.png)

我在这个故事中看到了两个寓意:

1.  不要使用干布或纸质过滤器；仅使用潮湿的过滤器。
2.  数据在导致咖啡问题的根源方面非常有效

随着我对这个潜在问题的关注，我注意到过滤器上多一点水有很大的不同。我想知道这是否是为什么在冰球顶部喷水有助于流动的部分原因。

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