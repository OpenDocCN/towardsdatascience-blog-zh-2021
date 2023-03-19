# 在马登扩展审查不平等

> 原文：<https://towardsdatascience.com/expanding-review-inequality-in-madden-301cb0b438e5?source=collection_archive---------30----------------------->

## 在独家NFL 足球模拟游戏中，用户和评论家的分数是如何产生分歧的

![](img/4fff073667168afa916a4aec3f6c5a67.png)

由[戴夫·阿达姆松](https://unsplash.com/@aussiedave?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

2004 年，艺电公司(Electronic Arts)签署了一份为期 10 年的独家协议，获得了在游戏机上制作 NFL 和 NFLPA 授权视频游戏的权利。这笔交易消除了 2K 等其他发行商的模拟风格竞争。自 2015 年以来，该交易已被永久续约，最近一次续约发生在 2020 年 5 月。2015 年续签独家授权时，SB Nation 的《英里高报告》写道，[“EA 体育的*好消息，游戏玩家的*](https://www.milehighreport.com/2015/6/20/8806841/ea-sports-retains-exclusive-rights-to-make-madden-nfl-video-game) *坏消息”。对于那些寻求沉浸式足球模拟体验的人来说，这句话可能低估了悲伤的现实。*

我坚信大众的智慧能够判断许多事物的价值。为了了解每年发布的 Madden 游戏的质量，我利用了来自 [Metacritic](https://www.metacritic.com/) 的数据。本文中使用的数据是直接从 Metacritic 获得许可使用的。Metacritic 对专业游戏评论家和个人用户都进行评分。数据中的偏差包括缺乏用户侧的验证购买，以及由于用户和评论家分数上的“影响者”或“购买评论”而导致的潜在分数膨胀。该数据集并不完美，但足以描绘出一幅画面，说明专业评论家认为是一款完美游戏的东西与用户想要的东西之间的鸿沟越来越大。生成这些数字的数据和代码可以在 [GitHub](https://github.com/glickmac/Madden_Scores) 上获得。

## 数据

![](img/97a96ef32d1383d354d462cb27bde56f.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了获取数据，我访问了从 [2001](https://www.metacritic.com/game/playstation-2/madden-nfl-2001) 到 [2022](https://www.metacritic.com/game/playstation-5/madden-nfl-22) 的《疯狂游戏》的 [Metacritic](https://www.metacritic.com/) 。我选择 PlayStation 游戏机在我的分析中保持一致，因为它们往往比 Xbox 有更多的评论。如果新一代游戏机在 2006 年末(发布年)上市，我会等到下一年(在这种情况下是发布年)。我这样做是为了让开发者有时间赶上新系统的性能。从 PlayStation 2 到今天的 PlayStation 5，经历了四代游戏机。生成的数据表示例如下所示，完整的表格可从 [GitHub](https://github.com/glickmac/Madden_Scores) 下载:

![](img/939e8bbb8e314675c68069d9fe53a72c.png)

作者提供的资料图片截图

Metacritic 颜色代码游戏分为三层:好(绿色)，meh(黄色)，坏(红色)。一个好的游戏被定义为得分在 75-100 之间。一般的游戏得分在 50-75 之间，糟糕的游戏得分低于 50。

## 这些年来马登

虽然 Madden 游戏自 90 年代中期就已经发布，但 Playstation 2 的年度发布分数从 2001 年[开始(Metacritic 始于 1999 年)。](https://www.metacritic.com/game/playstation-2/madden-nfl-2001)

![](img/98a1793c803acf23b2eb1bb4c9d3ab4c.png)

从 Madden 2001 年到 Madden 22 年的平均用户和评论家分数。彩色水平线代表元符号颜色代码。作者创建的图像。

对于 PlayStation 2 上的游戏，用户分数和评论家分数都在良好范围内。许多在 PS2 时代让游戏如此受欢迎的功能在过渡到 PS3 时代时被删除了。由于没有竞争推动创新，EA 将逐步添加回这些 PS2 功能，并将其作为前一年发布的新功能进行宣传。这是推动销售的方法，直到麦登终极团队(MUT)成熟。在过去的几年里，MUT 一直是 EA 的主要赚钱机器。MUT 在 [2010](https://www.metacritic.com/game/playstation-3/madden-nfl-10) 首次引入 Madden。era pre-MUT 和 with MUT 的用户和评论家评分对比如何？

![](img/9f2ebfb0bc62e1687c0cf716880b7b49.png)

箱线图比较了有和没有 Madden Ultimate Team 的时代的平均用户和评论家分数。MUT 前后评分差异有显著性(p=0.001，H=10.72，Kruskall-Wallace)。作者创建的图像。

此外，每一代新游戏机的用户评分都在下降。PS2 时代存在于一个竞争和创新的时代。PS3 时代见证了从 PS2 时代移除的功能的重新引入，成为销售的主要推动力。当 Madden 来到 PS4 时，MUT 是开发者的主要焦点，这可以通过复制/粘贴元素来证明，比如连续 5 年的完全相同的冠军庆典。PS5 上的 Madden 22 在 MUT 上遭受了同样的重视(PS5 上 n=1)。

![](img/1c71efd7f4e0377447ccaa291570fb3b.png)

4 代游戏机的评论家和用户评分。每一代控制台的得分差异显著(p=0.002，H=14.73，Kruskall-Wallace)。图片作者。

用户分数下降和糟糕的游戏体验的另一个因素可能归因于更加动画驱动的游戏引擎，这导致了一些非常[有趣的错误和故障](https://www.youtube.com/watch?v=Jmo0mFXkLZw)。在 [Madden 2018](https://www.metacritic.com/game/playstation-4/madden-nfl-18) 中引入冻伤引擎是因为全公司要求使用相同的游戏引擎。冻伤最初是为第一人称射击游戏设计的。冻伤引擎的引入导致了用户分数的大幅下降。

![](img/743d8fdc134a4c7b406cd70d1fc4e138.png)

采用 frosty 引擎之前和实施 frosty 引擎之后的用户和评论家分数。发动机之间的差别是显著不同的(p=0.0039，H=8.34，克鲁斯卡尔-华莱士)

**用户和评论家之间的差距**

![](img/2a380e267f7c4dd38233150d525d3e42.png)

[克里斯托夫辊](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

近年来，用户对《疯狂游戏》的评价大幅下降。评论家评论保持相对稳定，略微下降到一般的元评论类别。Reddit 用户 [lyac8](https://www.reddit.com/user/lyac8/) 的一项[伟大研究](https://www.reddit.com/r/gaming/comments/hcprl4/analysis_of_games_metacritic_scores/)显示，用户和评论家之间的分数差异在过去几年一直在增加，在 2019 年和 2020 年，评论家平均比平时高出约 10 分。lyac8 表示，评论家通常会给糟糕的游戏更高的分数，因为他们不想失去从工作室获得未来发展的机会。我理解这种情绪，但我认为马登分数有些可疑。2019 年，GameRant 发表了一篇报道，强调了 Metacritic 上评论家和玩家之间差距最大的 [10 款游戏。一些游戏之所以会出现在这个列表中，是因为评论家的分数很高，而用户的分数虽然仍然很高，但却远远低于按比例调整后的数值。](https://gamerant.com/game-review-player-critic-gap-metacritic/)[红色死亡救赎 2](https://www.metacritic.com/game/xbox-one/red-dead-redemption-2) 就是一个很好的例子，Xbox One 上的评论家分数为 97 分，用户分数为 80 分，相差 17 分(它在 PS4 上的[用户分数为 85 分](https://www.metacritic.com/game/playstation-4/red-dead-redemption-2))。GameRant 的文章中最大的差异是[命运 2](https://www.metacritic.com/game/xbox-one/destiny-2) ，评论家得分为 87，用户得分为 43，结果相差 44 分。看起来很多，但是等等，这是马登！

![](img/f611e5d4c73161d326c6502eb5d4b276.png)

在 Madden 中评论家分数和用户分数之间的差异。图片作者。

GameRant 的文章写于 2019 年。我很惊讶这篇文章没有提到[马登 17](https://www.metacritic.com/game/playstation-4/madden-nfl-17) 或[马登 18](https://www.metacritic.com/game/playstation-4/madden-nfl-18)42 和 48 的惊人差异！？！分别是。然后情况变得更糟，糟糕得多。从 [Madden 19](https://www.metacritic.com/game/playstation-4/madden-nfl-19) 到 [Madden 22](https://www.metacritic.com/game/playstation-5/madden-nfl-22) 的 Madden 游戏的评论家和用户之间的得分差异已经大于 **60！！！！！**对于最近 4 个版本【60，62，61，63】。如果这些巨大的差异(> 40)是零星的，就像我们在 PS3 时代看到的那样，我们可以将其填补到一个变得陈旧的专营权或堆积效应(鉴于用户评论的巨大数量，我们确实看到了 [Madden 21](https://www.metacritic.com/game/playstation-4/madden-nfl-21) )，然而这种一致的差异让我猜测，EA 要求元批评或“购买”评论的“好类别”级别评论略高于其他情况。

**结论**

![](img/98a1793c803acf23b2eb1bb4c9d3ab4c.png)

从 Madden 2001 年到 Madden 22 年的平均用户和评论家分数。彩色水平线代表元符号颜色代码。作者创建的图像。

在这里，我们展示了用户和评论家之间的差距从未如此之大。代码和数据都可以在 [GitHub](https://github.com/glickmac/Madden_Scores) 上找到。马登不再是每个人的足球模拟，然而，由于独家许可，它是唯一可用的 AAA 足球模拟。我个人仍然觉得每年秋天的几个小时里马登是令人愉快的；虽然由于我的人生阶段，功能的下降，或者两者的结合，我发现自己不像以前在专营权中那样投入。我的名字是[科迪·格利克曼](https://codyglickman.com/)，可以在 [LinkedIn](https://www.linkedin.com/in/codyglickman/) 上找到我。一定要看看我下面的其他文章:

<https://medium.com/swlh/exploring-college-football-salaries-dc472448684d>  </a-fantasy-draft-order-lottery-using-r-shiny-6668a5b275d2>  </college-football-travel-during-covid-1ced0164840e> 