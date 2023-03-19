# 道德数据分析——每个企业都需要知道的

> 原文：<https://towardsdatascience.com/ethical-data-analytics-what-every-business-needs-to-know-e772bf8f670c?source=collection_archive---------19----------------------->

![](img/9c1a68b67e24efe49670fb476d14514a.png)

照片由 [**埃维拉尔多·科埃略**](https://unsplash.com/@_everaldo) 在 [Unsplash](https://unsplash.com/) 上拍摄

每个公司都有客户数据。问题是，他们能用它做什么？哪些行为符合法律和道德规范，哪些不符合？如果说公司在历史上没有注意过如何利用客户数据，那么现在它们肯定注意到了。许多新的全球立法(如欧洲的 GDPR)已经将这个问题列为每一家存储客户数据并使用它来发送营销电子邮件的公司的首要考虑因素。

无论您的公司是否足够大，拥有一支使用自学算法的数据科学家团队，还是只有一名分析师在密室中切割数据，数据分析都可以帮助您做出关于关键业务流程的数据驱动型决策，从而提高业务绩效。但无论大小，如果你的公司利用客户数据做出面向客户的决策，就有伦理问题需要考虑。

**当“了解你的客户”走得太远**

基于客户数据的任何决策或行动都可能导致潜在的破坏性个人后果，从仅仅是不舒服到改变生活。到 2012 年，美国零售巨头塔吉特(Target)已经找到了如何通过数据挖掘进入女性子宫的方法，了解到一位顾客在需要开始购买尿布之前很久就已经怀孕了。

如果这种见解 1)对一个人的生活有深远的影响(比如银行业)，或者 2)对大多数人有很浅的影响(比如社交媒体)，那么这种见解的影响可能特别有害。首先，银行使用数据分析来做出决定，例如，谁可以获得抵押贷款以及利率如何，可能会对某人的生活产生持久且潜在的有害影响。在第二个场景中，2016 年的[脸书/剑桥分析公司政治影响丑闻为应用数据分析如何影响投票箱中的大众行为提供了一个清晰的路线图。](https://www.nytimes.com/2018/04/04/us/politics/cambridge-analytica-scandal-fallout.html)

无论决策是否会对另一个人的生活产生直接影响，每个公司都需要考虑他们的算法中可能出现的无意识偏见，因为这可能会对品牌产生负面影响。几年前，一个谷歌图像识别人工智能错误地将[人类表示为大猩猩](https://www.theguardian.com/technology/2018/jan/12/google-racism-ban-gorilla-black-people)，微软的聊天机器人 Tay 在 24 小时内变得[对 Twitter 太无礼](https://www.theguardian.com/technology/2016/mar/24/tay-microsofts-ai-chatbot-gets-a-crash-course-in-racism-from-twitter)，亚马逊的招聘算法[产生了明显的性别偏见。](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)

出于所有这些原因，一份最近在网上流传的[泄露提案](https://www.theverge.com/2021/4/14/22383301/eu-ai-regulation-draft-leak-surveillance-social-credit)透露，欧盟正在考虑禁止出于多种目的使用人工智能，包括大规模监控和社会信用评分。然而，值得注意的是，GDPR 已经强烈坚持透明度，因为消费者对于强加给他们的自动决策有“解释的权利”。

**要考虑的道德框架**

无论存在(或最终出现)什么样的法规来减轻不公平的算法行为，数据科学家都站在关于数据分析的道德思考的前沿。幸运的是，有许多公共框架可供数据科学家或公司中希望避免任何法律或道德纠纷的任何人参考，这些纠纷可能因使用自动化和/或数据驱动的决策而产生。

最重要的是，行为合乎道德意味着不直接或间接地伤害他人或对他人造成不利。正如上面无意识偏见的例子一样，即使公司试图以客观的方式做好事，也可能在道德问题上站在错误的一边，并受到公开谴责。因此，在开始任何数据分析使用案例之前，建议后退一步，考虑案例的道德相关因素。

大型(政府和非政府)组织，如[谷歌](https://ai.google/principles/)、[欧盟](https://ec.europa.eu/digital-single-market/en/news/ethics-guidelines-trustworthy-ai)和[联合国](https://www.un.org/en/chronicle/article/towards-ethics-artificial-intelligence)已经发布了使用数据/AI/ML(机器学习)的指南，可以在处理数据和分析时提供道德指导。但是为了避免你不得不查阅所有这些，一项针对 39 位不同指南作者的元研究显示了以下主题的强烈重叠:1)隐私；2)安全和安保；3)透明度、问责制和可解释性；4)公平和不歧视。

**避免道德失误的实用指南**

在我们公司，我们已经将这些指南提炼为道德风险快速扫描，突出需要特别关注的领域，并可以帮助您评估特定用例可能产生的风险。扫描由一系列问题组成，这些问题涉及诸如谁将受到所做决策的影响、这些决策将如何受到影响以及影响的程度如何等。

例如:您是否会使用这些数据来做出可能会对被视为弱势群体的财务状况产生影响的决策？如果是，他们的个人行为会受到什么影响？在你能够衡量算法是否会导致不良影响之前，需要多长时间？

*这是伦理数据分析系列的第一篇文章。在下一部分*</how-to-make-your-data-project-ethical-by-design-99629dcd2443>**中，我们将更深入地探讨道德风险快速扫描，并描述 IG & H(我工作的公司)用来帮助企业减轻扫描结果中出现的潜在问题的框架。**

**这篇文章也被发表到 IG & H 的网站上。这是三篇系列文章中的第一篇，你可以在这里找到这些文章:* [*第二部分*](/how-to-make-your-data-project-ethical-by-design-99629dcd2443) *和* [*第三部分*](/the-importance-of-ethics-centricity-in-data-projects-ea2a60e6b67)*