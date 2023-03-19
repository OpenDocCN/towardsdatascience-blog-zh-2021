# 商业中机器学习的真实世界课程

> 原文：<https://towardsdatascience.com/real-world-lessons-for-machine-learning-in-business-3144c22b553c?source=collection_archive---------41----------------------->

## 2021 年 AWS 机器学习峰会主题演讲要点

![](img/69646fab8267c01184ec58f04abddf67.png)

马里奥·梅萨利奥在 [Unsplash](https://unsplash.com/s/photos/fractal-tree?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

最近，机器学习似乎得到了所有的关注和炒作，有些人甚至说它将成为主流。甚至还有专门针对 ML 的会议和峰会，就像 [2021 AWS 机器学习峰会](https://aws.amazon.com/events/summits/machine-learning)。在我看来，为了让 ML 成为主流，我们仍然需要现实世界的经验来将 ML 转化为商业产品，我希望从这次峰会中获得一些收获。我在这里列出了一些对我影响最大的部分。希望当您计划应用 ML 时，您会发现这些很有用:

*   比萨饼与适量的奶酪例子与 Swami Sivasubramanian
*   与 Yoelle Maarek 的计算幽默对话
*   弄脏你的手和吴恩达聊天

由于该演讲由 AWS 组织，峰会倾向于使用他们的 ML 服务，但这里的要点也可以应用于其他云计算平台，如 GCP，他们提供自己的 ML 服务。

# 配有适量奶酪的披萨

ML 正在走向主流的一个可能的标志是它在各种行业中的使用，包括比萨饼行业。Swami Sivasubramanian 展示了[dafgrds](https://www.dafgards.com/)作为一个即使在有限的训练数据集下也能应用 ML 的企业的例子。他们希望使用 ML 来提高在每个披萨面团中放置适量奶酪的质量和效率。是的，从我喜欢的东西开始多好啊:ML 和披萨。

![](img/802c2d02a857a95b445b5c5bb7a77914.png)

[大卫·贝内什](https://unsplash.com/@davidbenesph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pizza-and-robot?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

他们使用视觉质量检查与[少数镜头学习](https://research.aimultiple.com/few-shot-learning)可以学习一个特定的任务，只有几个例子。我认为这是一个具有有限训练数据集的 ML 应用程序的相关用例。数据收集——获得正确的种类和数量——通常是 ML 项目开始时的一个难点。更进一步，我对 [AWS 展望愿景](https://aws.amazon.com/lookout-for-vision)很感兴趣，这是从客户反馈和这几个镜头学习想法中概念化出来的。

也有你提到的其他公司和企业，你可能听说过，像宝马和纽约时报，但比萨饼的想法仍然存在。

# 计算幽默

为了让 ML 成为主流并坚持下去，关注用户体验也很重要。而且，非常有趣的是，AWS 正在认真研究计算幽默，并在这项研究中花费了大量时间。我确实喜欢他们将 ML 团队融入产品团队的方法。在这个例子中，计算幽默小组一直与亚马逊 Alexa 购物团队合作。

在寻找取悦客户的方法时，我不禁想起了最近人工智能聊天机器人的一项令人耳目一新的进步。在谷歌[的 LaMDA 项目中与冥王星和纸飞机等无生命物体对话！](https://events.google.com/io)

用谷歌的 LaMDA 程序与冥王星和纸飞机对话

现在，回到计算幽默，团队不只是让人工智能有趣，而是想知道客户是否有趣，以及人工智能应该如何反应。在这种情况下，客户是采取主动的一方。

为了检测和可能的反应，该团队甚至试图查看文化背景、讽刺或顾客是否有心情开玩笑。我喜欢这个演讲还提到了混合主动性理论，救济理论等等。

例如，对于任天堂 Switch·格雷的欢乐大会产品，一位顾客在回复中提到了一些文化背景:“我可以用它来侵入黑客帝国并拯救人类吗？”

![](img/e5d9c392c4d0dd1873a14e51418d7419.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/the-matrix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

另一位顾客在一个非常昂贵的饮水机上讽刺道:“这东西能让我飞起来吗？这似乎是由于价格，必须做一些特殊的事情。”

是的，现在的 ML 状态已经可以做这么多了，但是还有很多可以探索的，还有新奇的使用方法。

# 弄脏你的手

然后，还有吴恩达和斯瓦米·西瓦苏布拉马尼安的炉边谈话。在那次不到 30 分钟的聊天中，有很多有趣的地方，但对我来说，最精彩的其实是最初的几分钟！

第一部分是为公司提供一些说明，针对他们的领导者，如首席执行官和首席技术官，他们希望启动他们的机器学习。当你第一次向大联盟迈进时，你必须把手弄脏。

花太长时间去计划和开始可能是一个错误。对于第一个项目，数据可能很乱，但可能有解决方法，企业应该能够从小的试点项目或概念证明开始，并迅速取得成功。从这些项目中学习成长和扩展项目。

我确实喜欢在形成机器学习的长期战略之前尝试一个试点项目的想法。从 POC 中获得的知识将有助于制定与公司目标和人员文化更加一致的战略。

我在之前的一篇关于[强化应用于商业问题](https://medium.com/nerd-for-tech/reinforcement-learning-applied-to-business-problems-412b478046a7)的文章中提到过，RL 的最大障碍在于问题公式化，接下来的步骤将由公式化的问题决定。所以，在问题的表述上要小心。但即使这样说，也不应该阻止人们弄脏自己的手，仍然尝试。从长远来看，可能会有一个 ML 平台，但总是那些第一次 POC 和快速成功提供了对 ML 旅程方向的见解。

现在，即使对于非领导角色，当开始一个 ML 项目时，对于一个全新的应用程序，可能很难创建目标指标来检查 ML 是否成功，直到 ML 团队完成了概念验证，或者文献中是否有相关项目来帮助定义基本级别的性能。这里的关键很可能是去快速和肮脏的原型系统，迭代，并从这些经验中学习。

# 离别笔记

越来越多的公司采用 ML，但这种增长不会很快放缓。它可能始于科技公司，但 ML 项目现在渗透到其他公司，包括那些披萨公司。峰会的关键部分和反复出现的主题是将 ML 转化为产品。

解决像拥有大型训练数据集这样的棘手问题至关重要，因此拥有像少数镜头学习这样的技术是一个受欢迎的变化。为了这些 ML 项目的成功而不仅仅是炒作，客户体验应该被放在首位。投资于解决客户体验的研究，比如那些具有计算幽默的研究，不仅仅是一个附带项目，而是一个人工智能产品的重要方面。而且在 ML 的世界里时不时的看到不同的视角也只是让人耳目一新。考虑工作方式，例如将 ML 团队嵌入到产品或开发团队中，可能是项目成功的关键部分。

将 ML 转化为产品也意味着超越理论，尤其是对于那些刚刚开始 ML 之旅的公司来说，尝试小规模的试点项目并取得立竿见影的效果有助于为公司的 ML 长期战略提供一些指导。