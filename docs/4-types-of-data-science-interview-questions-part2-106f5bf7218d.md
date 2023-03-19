# 征服数据科学面试—第二部分

> 原文：<https://towardsdatascience.com/4-types-of-data-science-interview-questions-part2-106f5bf7218d?source=collection_archive---------24----------------------->

## **具有强烈商业焦点的真题**

![](img/a7e50da71b222f122756c6f81b4401bf.png)

谁不喜欢一个谈论数据和商业的人[图片来自 [GraphicMama-team](https://pixabay.com/users/graphicmama-team-2641041/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1454403) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1454403)

DS 面试可以是一连串的多轮面试。对于大多数公司来说，进行 2-3 轮是很常见的。这之后有时会有董事轮/招聘经理轮。多轮意味着您可以放心地在 ds 项目管道的所有方面接受测试。

大体上有以下类型的问题:

**1。基于简历/基于项目**

**2。ML 熟练检查—算法细节**

**3。指标**

**4。基于案例研究的问题**

我们已经在[**上一篇**](https://learnwithdivya.medium.com/4-types-of-data-science-interview-questions-part1-d17db4253c69) 中讨论了类型 1 和类型 2，在那里我们看到了具有强烈技术焦点的问题**。**当前岗位目标类型 3 和类型 4。

![](img/28ce29ba7119875f69b654b656a2ebab.png)

案例研究是对你思维过程的测试[照片由[克里斯蒂娜@ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/white-board?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄]

# **基于案例研究的问题**

在这类问题中，面试官分享一个业务用例，并要求你解决它。采访者提供最低限度的护栏。

> *这是一种检验候选人业务敏锐度的方法，看看他们是否能接受模糊的业务陈述(是的，很多时候业务需求确实是模糊的)，询问相关问题，陈述他们的假设，将其与合适的数据科学解决方案联系起来，最后将 DS 解决方案翻译回对业务有意义的语言。*

在这类问题中，重要的是提出澄清性问题，并将问题分成小块。

假设你是 Big Basket 的分析师。您可以查看 BB 目录中所有产品的价格。您还可以访问交易详情。BB 公司正计划推出一种内部开发的优质蜂蜜。你需要为这种新的内部优质蜂蜜提出一个建议价位。你的方法是什么？

**经过测试的概念** —商业敏锐度、数据意识

**Eg2 :** 你是一家生产疫苗小瓶的公司质量控制团队的一员。小瓶的壁应该至少有 2 毫米厚。该公司保证质量标准的最大缺陷率为 3%。作为一名质量检测员，你需要检测多少瓶才能确定是否达到质量要求。

**概念测试** —统计(样本量估计)

你是一家生产各种服装和配饰的精品店的顾问。他们通过自己的网站销售产品。精品店有不同的部门，比如男装、男式配饰、男式鞋，以及类似的女装部门。是时候推出新的春夏系列了。然而，由于过去几个月的低销售量，仓库里有积压的存货。你的工作是想出一个策略，清空旧股票，卖出新股票。你可以给折扣，但你需要对每个部门的成本敏感。

陈述你所做的任何假设。你会考虑哪些数据点来制定你的策略？你会对数据进行什么样的分析？最后，你将如何说服个别部门主管使用你的策略？

**概念测试:**商业敏锐度，将业务转化为 DS，反之亦然

**一个可能的解决方案可以谈以下几点:**

**数据** —库存管理成本、商品成本、过去价格、过去需求、客户交易历史

**分析** —需求预测、优惠效果、客户购买倾向、产品捆绑

**与业务沟通** —显示没有任何策略的销售预测与有建议策略的销售预测的图表。同样，有无策略的库存成本也是如此。

你是一家金融科技初创公司的数据科学家。这家公司有一种产品提供 500 美元的信用额度(无担保贷款)。目标人群是没有固定收入的人。大多数银行在提供贷款前坚持要求信用检查、工资单、担保/保证。但是你的公司提供无担保贷款，没有任何信用检查，甚至不收利息。对于任何接近你的人，你收集他们在 KYC 的详细信息，并有权查看他们银行账户过去 24 个月的交易。想出一个策略来确定共享的银行账户是否是他们的主要银行账户，以及他们是否有资格获得信贷额度。

**概念测试** —商业敏锐度，将问题分解成大块的能力，领域意识，将业务转化为 DS

你是一家数字营销公司的数据科学家。你目前的项目是分析产品评论，找出人们谈论的话题。你有大量的输入数据流。你将如何实时分配主题？你需要能够适应实时出现的新话题？

**可能的解决方案**可以利用单词嵌入。

![](img/366cff4c12abeadcafe582e7b81ea99a.png)

这是正确的措施吗？[图片由[达林·阿里亚斯](https://unsplash.com/@darlingarias?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/metric?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄]

# **指标(KPI)**

> "如果你不能衡量它，你就不能改进它."
> 
> 彼得·德鲁克

对于任何商业问题来说，重要的是要知道你试图改善什么——降低成本，增加参与度，提高盈利能力。这就是度量变得重要的地方。我认为衡量标准是将 DS 解决方案与业务目标联系起来的标准。

**Eg1:** 你是 Twitter 的数据科学家。你想增加新用户对平台的参与度。你将使用什么标准来判断一个新用户是否会变得无所事事？

**提示—** 跟踪每日指标的趋势—如登录和交互。

我们是一家手机制造商。我们有我们手机和其他公司手机的历史价格表，我们想向管理层报告我们的价格在市场上的竞争力。你会怎么做？

**提示—** 比例

# **结论**

随着一个人从初级数据科学家向高级数据科学家的晋升，对工作的期望也发生了变化，面试问题也反映了这一点。最初，人们只是期望解决一个问题，只要给出了解决策略。然而，后来人们期望拥有一个端到端的项目，这包括 CRISP-DM 方法的所有方面——定义策略、执行策略、检查策略的有效性、部署和监控解决方案。

本系列中提到的问题的多样性涵盖了这些方面的大部分，将有助于你更好地准备。

**您可能还喜欢:**

[](/conquer-the-python-coding-round-in-data-science-interviews-5e27c4513be3) [## 征服数据科学面试中的 Python 编码回合

### 我 2021 年在印度班加罗尔作为数据科学家面试的经历。

towardsdatascience.com](/conquer-the-python-coding-round-in-data-science-interviews-5e27c4513be3)