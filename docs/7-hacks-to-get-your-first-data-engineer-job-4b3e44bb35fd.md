# 如果你想成为一名数据工程师，你需要知道的 7 件事☄

> 原文：<https://towardsdatascience.com/7-hacks-to-get-your-first-data-engineer-job-4b3e44bb35fd?source=collection_archive---------7----------------------->

## 帮助你获得第一份数据工程师工作的策略

![](img/1cdcb34a14483ccfd7e618eadd9162d0.png)

婆罗摩山的神奇日出[数字图像]作者:伊尔哈姆·巴赫蒂亚[https://unsplash.com/photos/Z1A2U0vo8uY](https://unsplash.com/photos/Z1A2U0vo8uY)

关于如何成为一名 10 倍数据工程师，有大量的知识，但是什么值得你去获得你的第一份数据工程师工作呢？需要学习的工装和概念实在太多:*很吓人*😱。在这篇文章中，我会给你一些关于如何开始你的学习之旅的**建议**和**为你提供额外的资源**📌。这不是一条捷径，但它会帮助你分清事情的轻重缓急，不会陷入困境。

# 如果门槛太高，请尝试另一个数据角色。👷

根据你的经验，获得数据工程师的第一份工作可能具有挑战性。如果你是软件工程的新手，那么技术门槛会很高。数据工程师所需的技术技能范围比数据分析师要广得多。

因此，Data Analyst 是一个良好的开端，因为它需要较少的硬技能(但有时需要更多的领域知识)。了解 SQL 并掌握一个 dashboarding 工具([Tableau](https://www.tableau.com/trial/tableau-software?utm_campaign_id=2017049&utm_campaign=Prospecting-CORE-ALL-ALL-ALL-ALL&utm_medium=Paid+Search&utm_source=Google+Search&utm_language=EN&utm_country=DACH&kw=tableau&adgroup=CTX-Brand-Core-EN-E&adused=543201874518&matchtype=e&placement=&gclsrc=ds&gclsrc=ds)/[power bi](https://powerbi.microsoft.com/en-au/)/[Metabase](https://www.metabase.com/))应该会让你处于一个很好的位置。

最重要的是，数据分析师经常与数据工程师一起工作。因此，你有机会了解他们做什么，当你觉得准备好了，你可以申请内部调动。这样总比走正门容易。据我所知，目前有许多数据工程师走的是这条路。

📌如果你想听一个关于这种转移的故事和一些关于成为数据分析师的见解，请点击这里查看这篇文章。

# 不要从流媒体和机器学习开始🌊

根据公司的数据成熟度，其中一些概念不是必须具备的。请不要被你在他们的工作邀请中能找到多少时髦词汇所迷惑。《连线》提到去年只有 9%的公司使用机器学习这样的工具。虽然人工智能的采用正在快速增长，但仍有大量公司在基础数据工程方面苦苦挣扎。

作为一名大三学生，在知识方面有一个基线，它将涵盖许多用例，并让你走得很远。如果你学会了如何使用经典的管道框架( **Pandas，Spark，dbt** )来编写 **Python 和 SQL** ，你将涵盖大多数分析性批量用例。获得使用分析数据库的经验，例如 **BigQuery** (或者红移/雪花，但是没有任何免费层用于 playground)并选择一个编排工具。那一侧的气流目前是行业标准。

📌看看这些数据工程师路线图，这应该会给你一个合适的学习路径:

*   来自 [datastack.tv](http://datastack.tv/) 的[数据工程师路线图](https://github.com/datastacktv/data-engineer-roadmap)
*   [数据工程路线图 2021](https://medium.com/coriers/data-engineering-roadmap-for-2021-eac7898f0641) 来自 [SeattleDataGuy](https://medium.com/u/41cd8f154e82?source=post_page-----4b3e44bb35fd--------------------------------)

您是否注意到这些路线图是从软件工程基础开始的？

# 软件工程基础很重要。很多。💾

我们经常忘记，从本质上讲，数据工程师只是另一种类型的软件工程师。

我认为这种疏忽的原因是因为工作已经发生了变化，今天的许多数据工程师来自非软件工程背景(BI 开发人员、数据分析师)。然而，如果你掌握了这些基础知识，你将在软件工程师同行中脱颖而出，并且你将在理解如何交付一个生产就绪的项目上获得优势。

这些包括(并非详尽无遗的清单) :

*   CICD 概念和工具(Github Actions / Jenkins / Circle CI)
*   Git (Github / Gitlab)
*   测试(单元/集成/系统测试)
*   作为代码的基础设施(Terraform，Pulumi)
*   Devops (k8s、Docker 等)

📌这里有一篇很好的文章[来理解 DevOps 与数据工程师的关系，以及你能从中获得什么。](https://k21academy.com/microsoft-azure/data-engineer/devops-for-data-engineering/)

# 学习用一个附带项目来构建端到端的东西🗺

拿起一支笔，设计如何从 A 点获取数据、转换数据、使用数据(使用仪表板工具)并基于此做出决策。试着回答这些问题:

*   我的数据来自哪里？我如何得到它？API？数据库？刮痧？
*   我如何协调管道？
*   我将如何消费它？我可以使用哪个仪表板工具？连接是如何工作的？这背后的局限/成本是什么？我如何为我的数据建模？
*   如果我想更改数据管线中的特征，会发生什么情况？我如何管理访问权限？我如何管理版本？

对高层设计有一个很好的了解，并理解每个组件如何相互交流，是学习如何将你的技能组合转化为可操作的价值的一个很好的开始。

📌查看[这篇文章](https://medium.com/coriers/5-data-engineering-projects-to-add-to-your-resume-32984d86fd37),为数据工程师获取关于副项目想法的灵感。

# 关注一家云提供商，了解其与其他☁️提供商的相似之处

所有的云提供商在工具方面都有很多相似之处。那些花哨的名字只会让你迷失方向。关注一家提供商，在网上查找你在另一家云提供商上使用的同等服务。虽然有时可能会有显著的功能差异，但即使没有工作经验，您也能掌握该工具如何适合您的端到端管道。

**AWS** 占据市场主导地位，据[统计](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/)超过 32 %的市场份额。因此，能够获得第一份工作绝对是一个不错的选择。

📌谷歌与他们的竞争对手保持着一个最新的对照表[在这里](https://cloud.google.com/free/docs/aws-azure-gcp-service-comparison)。

# 瞄准没有太多遗产和合理数据成熟度的年轻公司🏢

基于上一点，你可能想关注一家云公司。作为大三学生这么做有很多原因。

首先，你可能已经花了相当多的时间关注云服务。如果公司有很多旧框架或本地集群，这是您需要掌握的额外知识。

除此之外，您还需要确保您在学习数据现代化堆栈上投入的时间至少能持续几年，然后才会过时。

📌Crunchbase 是快速了解一家公司规模/历史的绝佳资源。查看他们的工程博客和 GitHub 组织也会让你对他们的成熟有另一种感觉。

# 软技能和硬技能一样重要，甚至更重要。👨‍🏫

> “工程很容易，难的是人的问题。”谷歌副总裁比尔·考夫兰

数据工程师不是住在地下室的技术大师。他们被许多利益相关者包围着:商业、软件工程师、数据科学家、数据分析师等等。因此，**团队合作**和**沟通**是打破这些孤岛的关键。

一旦你进入这个行业，良好的软技能(或者更确切地说是人际技能，因为其中没有任何软肋)将会给你带来强大的优势。

📌以下是一些值得一读的文章，从中可以获得关于数据角色软技能的实用技巧:

*   [https://medium . com/better-programming/soft-skills-every-a-programmer-or-data-scientist-should-master-e 09742 b 34 f 38](https://medium.com/better-programming/soft-skills-every-programmer-or-data-scientist-should-master-e09742b34f38)
*   [https://medium . com/forward-data-science/5-soft-skills-you-need-as-a-machine-learning-engineer-and-why-41ef 6854 cef 6](https://medium.com/towards-data-science/5-soft-skills-you-need-as-a-machine-learning-engineer-and-why-41ef6854cef6)

# 结论🚀

不要专注于成为下一个技术巨星。退一步，看到更大的画面，并根据市场趋势和您的经验，专注于您需要加强的方面，以获得您在数据世界中的第一个角色。

第一次失败不要放弃。继续前进，祝你好运！❤️

# 迈赫迪·瓦扎又名迈赫迪欧·🧢

感谢阅读！🤗 🙌如果你喜欢这个，**跟着我上**🎥 [**Youtube**](https://www.youtube.com/channel/UCiZxJB0xWfPBE2omVZeWPpQ) ，✍️ [**中型**](https://medium.com/@mehdio) ，或者🔗 [**LinkedIn**](https://linkedin.com/in/mehd-io/) 获取更多数据/代码内容！

**支持我写作** ✍️通过加入媒体通过这个[**链接**](https://mehdio.medium.com/membership)