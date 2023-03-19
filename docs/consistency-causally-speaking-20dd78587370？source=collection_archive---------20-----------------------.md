# 因果关系上的一致性

> 原文：<https://towardsdatascience.com/consistency-causally-speaking-20dd78587370?source=collection_archive---------20----------------------->

## 为什么“一致性”在因果推断中很重要？

![](img/a48eec07860825605b7527dcd1069c7d.png)

照片由 [xespri](https://www.flickr.com/photos/51321874@N07/) 在[拍摄 https://www . pexels . com/photo/photo-of-head-bust-print-artwork-724994/](https://www.pexels.com/photo/photo-of-head-bust-print-artwork-724994/)

> 一致性只是说你观察到的结果正是你认为你会观察到的结果。

C [ausal 推论](https://en.wikipedia.org/wiki/Causal_inference)本周成为焦点:Joshua D. Angrist 教授和 Guido W. Imbens 教授因其在该领域的开创性工作刚刚获得诺贝尔奖。

正确进行因果推理所需的关键假设之一被称为[一致性](https://journals.lww.com/epidem/fulltext/2009/01000/The_Consistency_Statement_in_Causal_Inference__A.3.aspx)。(作为一名统计学家，我经常把这称为“因果一致性”，而不是“[统计一致性](https://en.wikipedia.org/wiki/Consistency_(statistics))”——一个非常不同的概念。)

一致性只是说你观察到的结果正是你认为你会观察到的结果。或者更准确地说，“一个人在她观察到的暴露历史下的潜在结果就是她观察到的结果。”([科尔和弗兰基斯，2009 年](https://journals.lww.com/epidem/fulltext/2009/01000/The_Consistency_Statement_in_Causal_Inference__A.3.aspx))

例如，假设 Y 是我一个月偏头痛的次数(即“偏头痛计数”)，如果我服用一种标准药物来减轻偏头痛一个月，X=0，如果我服用一种新药，X=1。如果我服用标准药物，y⁰是我的潜在偏头痛计数，而如果我服用新药，y 是我的潜在偏头痛计数。

形式上，一致性声明 Y = X y + (1-X) y⁰，其中 y 是观察到的结果，y⁰是 X=0 时的[潜在结果](https://en.wikipedia.org/wiki/Rubin_causal_model#Potential_outcomes)，y 是 X=1 时的潜在结果。所以如果我设置 X=1，我观察到的结果(Y)就是我认为我会观察到的结果(Y)。如果我服用新药(X=1)，我观察到的偏头痛计数(Y)正是我认为我会观察到的偏头痛计数(Y)。

> 你想要确定你正在测量你认为你正在测量的东西。

# 我有一个基本问题…

一致性公式强调了一个事实，即在任何给定的时间点，我只能观察到一个或另一个潜在的结果。这就是所谓的[因果推论的基本问题](https://www.sciencedirect.com/topics/social-sciences/causal-inference#:~:text=The%20fundamental%20problem%20for%20causal,potential%20outcome%20under%20the%20other)。我只能在任何给定的时间观察 y⁰或 y——所以仅仅使用一个月很难判断新药是否有效果。

如果我在没有其他偏头痛诱因的特别好的一个月后决定尝试这种新药，而我的偏头痛计数已经很低了，该怎么办？这种新药似乎并不比我的标准药物有更多的帮助，所以我停止使用它，因为它更贵。

我不知道的是，在有许多其他偏头痛触发因素的糟糕几个月后，它比标准药物*更有效——当我不太可能尝试新药时，因为我不能拿我的健康和生活质量冒险一个月。我放弃得太早了，因为我只看到了一个可能的结果。(每月的偏头痛计数在这里被称为[混杂因素](https://en.wikipedia.org/wiki/Confounding)。)*

当然，我可以在两个月或两个月以上的时间里尝试标准药物和新药，一个接一个地交替使用——但这是另一个故事了，其中有一个转折😉

# …但一致性不是其中之一？

这可能出错的一个关键方式(即“一致性”如何被违反)是在一项普通的临床研究中，该研究被称为[随机对照试验](https://en.wikipedia.org/wiki/Randomized_controlled_trial) (RCT)，类似于 [A/B 测试](https://en.wikipedia.org/wiki/A/B_testing)。

标准的 RCT 分析被称为[意向性治疗](https://en.wikipedia.org/wiki/Intention-to-treat_analysis) (ITT)。它说你必须比较每组研究参与者的结果，就像你随机分配他们一样——你“打算治疗”他们的方式。

假设你希望新药比标准药物更能降低平均偏头痛次数。你运行一个 RCT，取所有随机服用新药的人的平均偏头痛计数(X=1)，并将其与所有随机服用标准药物的人的偏头痛计数(X=0)进行比较。这种比较理想地告诉你新药相对于标准药物的可能因果效应(X=1 对 X=0)，通常定义为两组平均值之间的差异。

现在假设 A 医生和一群其他医生决定他们不喜欢根据研究方案他们应该实施的治疗。或者，也许病人 B 和一群其他研究参与者认为他们被分配的治疗可能没有帮助。在这两种情况下，结果都是一些病人接受了相反的治疗。这可能有很好的原因——但是这些治疗上的转变没有被记录下来。

如果你做一个 ITT 比较，你可能不知道，一些 X=1 的人实际上服用了标准药物，反之亦然。对于每一个像这样在治疗中“穿越”的人，他们的新方程实际上是 y = xy⁰+(1-x)y——违反了一致性！

这不仅仅是学术上的。如果新药确实比标准药物效果好得多，进行 ITT 分析将揭示新药(相对于标准药物)的较弱效果，因为尽管进行了随机治疗分配，但每组实际服用的药物是混合的:

1.  由于一些参与者交叉，X=0 组的偏头痛计数实际上低于(即，人为地好于)应有水平，而 X=1 组的偏头痛计数实际上高于(即，人为地差于)应有水平。
2.  这使得两组的偏头痛计数更加接近，从而缩小了实际测量的效果。

> 你可能被开错了药物剂量…[或者]你不小心一天吃了两片而不是一片。

还有一个更加平凡的违反一致性的行为。[用药错误](https://psnet.ahrq.gov/primer/medication-errors-and-adverse-drug-events)是一种临床[不良事件](https://psnet.ahrq.gov/primer/adverse-events-near-misses-and-errors)，在这种事件中，临床医生或医疗服务提供者给患者开出了错误的药物或提供了错误的治疗。患者[不依从](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1661624)是指患者没有按照预期服药或接受治疗。

例如，你可能被开错了治疗偏头痛的药物剂量；这是一个用药错误。或者一个朋友可能被告知如何不正确地使用医疗设备，类似于用药错误。如果你不小心一天吃了两片药，而不是遵医嘱吃了一片，那你就没有坚持治疗。

假设在一个月后你的下一次门诊，你的医生忘记了他们将剂量加倍的用药错误，或者你忘记了你吃了两片而不是一片。然后你讨论你上个月的偏头痛次数，看看新药是否有帮助。

你们两个都认为你们从预期疗法 X=1 中观察到了 y。但是，由于双倍剂量 X=2，你无意中观察到了 y——一个你们都没有预料到的潜在结果。真正的一致性公式是 Y = y I(X=2)+y I(X=1)+y⁰ I(X=0)，其中 I(。)是指标功能。希望你们俩都能发现实际接受治疗的变化。这将帮助你解释正确的潜在结果。

这也发生在 RCT。在我们上面的例子中，A 医生和其他医生故意违反研究方案犯了用药错误。病人 B 和其他参与者没有坚持指定的治疗。

所以是的，知道因果一致性很重要。套用埃莉诺·j·默里教授的话(因果推理中值得信赖的声音，你应该听从)，你要确保你在测量你认为你在测量的东西。

*关于出版后的修改，请参见响应/评论“* [*出版后的文字修改*](https://ericjdaza-drph.medium.com/on-13-oct-2021-wed-9c68c4dbc68d) *”。*

# 承认

我感谢 Jennifer Weuve 教授通过这条推文激发了这些想法:

# 参考

*   老科尔，法国人。因果推理中的一致性陈述:定义还是假设？。流行病学。2009 年 1 月 1 日；20(1):3–5.[journals . lww . com/epidem/full text/2009/01000/The _ Consistency _ Statement _ in _ Causal _ Inference _ _ a . 3 . aspx](https://journals.lww.com/epidem/fulltext/2009/01000/The_Consistency_Statement_in_Causal_Inference__A.3.aspx)

# 关于作者

Eric J. Daza 博士是数字健康领域的数据科学统计学家。他获得了应用统计学硕士学位和生物统计学博士学位，在临床试验、公共卫生和行为改变研究方面拥有 18 年以上的工作经验。Daza 为用于个性化健康建议的个人内(即，n-of-1，单病例，独特的)数字健康研究开发因果推断方法。| ericjdaza.com[🇺🇸🇵🇭](https://www.ericjdaza.com/)@埃里克森 linkedin.com/in/ericjdaza|[statsof1.org](https://statsof1.org/)[@ stats of](https://twitter.com/statsof1)[@ fsbiostats](https://twitter.com/fsbiostats)