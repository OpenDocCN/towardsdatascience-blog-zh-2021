# 建立一个异常检测系统？它必须满足的 5 个要求

> 原文：<https://towardsdatascience.com/building-an-anomaly-detection-system-5-requirements-it-must-meet-23c9527e7760?source=collection_archive---------33----------------------->

## 成功解决方案的建议——从信号分析到大规模分享见解

![](img/51521166c1b8dc661bf42e9e4968764e.png)

[阿里·哈坚](https://unsplash.com/@alisvisuals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/detective?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

如果您已经找到了自己的路，那么您的企业很可能已经将异常检测确定为大幅降低成本或寻找新增长机会的一种方式，并且您正处于规划解决方案的早期阶段。

构建自己的异常检测系统是完全可能的——但是要想成功，你需要满足这五个要求。

这五个测试不仅仅会节省你的时间和精力，它们是人们实际使用你的系统和看一次就把它和其他听起来不错但在现实生活中不起作用的技术解决方案一起扔进垃圾箱的区别。

因此，如果你只是为了乐趣而从事一个数据科学项目，并且你享受这个过程，就像达到最终目标一样——无论如何，请随意跳过这篇文章，继续探索你能做什么。

但是，如果你真的想建立一个使用时间序列数据的最先进的系统——从信号分析到大规模呈现输出——请继续阅读。

(还不熟悉异常检测？这里有几个初级读本):

*   【https://avora.com/what-is-anomaly-detection/ 
*   [https://medium . com/Pinterest-engineering/building-a-real-time-anomaly-detection-system-for-time-series-at-Pinterest-a833e 6856 DDD](https://medium.com/pinterest-engineering/building-a-real-time-anomaly-detection-system-for-time-series-at-pinterest-a833e6856ddd)
*   [https://towards data science . com/effective-approach-for-time-series-anomaly-detection-9485 b 40077 f1](/effective-approaches-for-time-series-anomaly-detection-9485b40077f1)
*   [https://towards data science . com/anomaly-detection-with-time-series-forecasting-c 34c 6d 04 b 24 a](/anomaly-detection-with-time-series-forecasting-c34c6d04b24a)
*   [https://blog . statsbot . co/time-series-anomaly-detection-algorithms-1 cef 5519 AEF 2](https://blog.statsbot.co/time-series-anomaly-detection-algorithms-1cef5519aef2)
*   [https://otexts.com/fpp3/](https://otexts.com/fpp3/)

# **要求 1——您的系统需要能够处理现实生活数据的复杂性**

![](img/27534561d6637d809c5961ea46ef2a99.png)

克里斯·利维拉尼在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

如果用户没有通过“嗅觉测试”,他们会发现很难相信系统的输出。直觉对用户很重要。如果他们看不到某些东西是如何组合在一起的，他们就不会相信这些东西——例如，为什么会标记异常，趋势和季节性是否被正确计算？特别是对于被认为是异常的数据点，这有助于建立对系统能力的信心。

一个好的方法是将信号分解成关键部分:趋势、季节性、残差(参见 Hyndman 等人的快速提示)。对残差执行检测。这就是“噪音”。这不一定是特定的分布。现实生活并不总是那么简单！

此外，很少有商业数据集预先标记了异常，历史异常不太可能已经被识别出来！

**推荐:**

1.  总是分解你的信号成分并验证它们——这样做是为了安心，但也是为了对照你的直觉和你的用户的直觉。给他们看一些他们能解释的东西，并确保当你拆开它时，它仍然有意义。
2.  为非参数检测方法设置一些合理的阈值。(注意—您需要为研发投入额外的时间/资源。让用户选择阈值—对一个人来说是异常的，对另一个人来说可能不是。
3.  防止出现问题，因为您假设数据是预先标记的，而不是假设您的数据是未标记的。没有人有时间回去用他们认为异常的点来标记数百甚至数千个时间序列图表

# **要求 2——您的用户不会停留在一个指标上。它需要在规模上可访问**

![](img/3e2f67ffbd25397425612acbdc7a40e2.png)

Miguel Henriques 在 [Unsplash](https://unsplash.com/s/photos/audience?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

你从头开始——通过构建核心算法。但是用例几乎总是很快变得更加复杂，因为人们看到了它在一个特定例子中工作的价值。例如，一个分析团队可能正在跟踪整体销售指标的收入，发现一些有趣的异常情况。消息传到其他利益相关者那里，很快，人们开始要求在更精细的层面(例如，按国家、产品类别或收购渠道划分的收入)进行跟踪，而不仅仅是跟踪一个顶线指标，从而迅速产生数百或数千个新的时间序列。

拥有一个他们无法解释的异常检测系统是没有意义的。这些用户不会费力地浏览那么多时间序列图表。即使他们知道怎么做，他们也不可能有时间。

**推荐:**

*   理论上来说，运行这些是可能的(技术/基础设施资源除外)，但是以一种不会给用户带来过多信息的方式呈现输出是绝对必要的。
*   仔细想想你的系统如何只共享与某个用户相关的异常。否则，任何潜在的有价值的信号都将淹没在噪声中。

# **要求 3——你需要尽量减少误报。比你想象的要快**

无论你的算法有多好，任何异常检测器都无法提供 100%正确的是/否答案。假阳性和假阴性永远存在，两者之间有取舍。

人类操作员将不得不做出决定，即使在同一个团队中，对于什么构成异常也可能有不同意见。

在最初的构建过程中很容易忽略这一点，但不预先解决这一问题会带来一些风险，异常检测系统可能会向用户发出警报“风暴”,对于没有数据流背景的业务用户来说，这可能会感觉系统没有充分管理通知。

误报有两种形式:

1.数据在一段时间内意外或莫名其妙地丢失/不完整。

2.数据是完整的，但检测错误地标记了异常。

**推荐:**

1.  如果数据缺失/不完整/迟交，请等待数据完整。但是您需要等待多长时间将取决于该指标刷新的频率——有不同的方法可以解决这个问题，可以是手动的，也可以是编程的。前者靠的是“直觉”和历史知识，不太可能规模化。
2.  如果数据是完整的，但你仍然得到假阳性——检查分解结果和残差计算——这对人类有意义吗？否则，这可能会导致用户不信任输出。(我们在需求 1 中谈到了通过嗅探测试的重要性)。在这里，确保有一种方法可以根据您的发现来调整/调整您的算法，或者更好的是，允许用户挑出这些数据点供您研究！

**警告:**在极端情况下，用户将简单地关闭异常检测系统，以避免被虚假警报淹没，所有构建该系统的良好工作都将付诸东流

# **要求 4——您的系统需要考虑事件和“已知”异常**

![](img/c34518a525ee70bdf1e8c7d1e59c1445.png)

照片由[贝内迪克特·盖耶](https://unsplash.com/@b_g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

商业数据上的异常检测算法通常会发现提前已知的“大”事件，例如黑色星期五、圣诞节、复活节、企业促销。这些对用户来说并不奇怪，他们会预料到，并希望异常检测也能这样做。

**推荐:**

考虑这些事件有两种方式:

1.  如果异常与已知事件一致，则抑制任何通知，或提供背景叙述；
2.  使用基于历史观察的估计值，修改具有已知事件的时间范围的期望值。例如，如果我们知道黑色星期五通常会出现 80%的销售额激增，那么下次我们遇到黑色星期五并看到销售额激增时，在考虑异常现象是否值得注意时，应该考虑预期的峰值。

# **要求 5——让用户能够轻松地与其他利益相关者分享见解，并找出原因**

![](img/b544d308407d6d66d871502b82795806.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

经过所有这些艰苦的工作，终于到了在 IT / BI 团队之外分享这些异常现象的时候了！这是你一直努力的方向。

这一部分需要仔细计划和执行。有一些重要的驱动因素:

1.  多个用户想要访问同一个被跟踪的指标(并且他们不想单独创建它们)；
2.  根据用户的要求，需要跟踪成百上千的指标；
3.  并不是每个指标都会引发异常——您需要引导用户只关注最近发生的异常活动，以避免他们淹没在数据中；
4.  用户将希望得到一个易于理解的、简明的消息，显示已经识别出了什么异常情况(如果可能的话，还有为什么！)

**推荐:**

所有这些都需要前端和后端协同工作，而不仅仅是一个数据库(对于大多数用户来说，从可访问性的角度来看，数据库至少要走几步)。像 PowerBI、Tableau 或 Looker 这样的传统仪表板可能不会在这里出现——它们不是为这种类型的用例设计的。

**房间里的大象:**

在发现异常(例如，英国的收入异常低)后，用户会很快想找出原因——这可能很难回答。这就是根本原因分析可以发挥作用的地方——请在以后的文章中注意这一点！

# **总而言之**

让一个生产就绪的异常检测系统为业务用户正常工作不仅仅是在数据集上设置一个松散的算法。如果你是一个动手的数据科学家，我鼓励你尝试一下上面的技巧。