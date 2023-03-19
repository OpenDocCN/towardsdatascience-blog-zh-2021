# 进行分析与扩展分析

> 原文：<https://towardsdatascience.com/doing-analytics-vs-scaling-analytics-b20795984ce2?source=collection_archive---------31----------------------->

## [意见](https://towardsdatascience.com/tagged/opinion)

## 进行分析在很大程度上是一个数据科学家可以解决的问题，而扩展分析需要的不仅仅是一个数据科学家团队

> *本文中的所有观点都是个人观点，是* ***而非*** ***可归因于我现在(或过去)所代表的任何组织。***

分析、数据科学和人工智能已经成为每个高级管理人员的主流词汇，并且成为“数据驱动”是大多数组织幻灯片中最常见的术语。然而，要在这些计划中取得成功还需要不断的努力，根据 Gartner 的预测，这些项目中有近 80%将永远无法部署。如果你问自己，“为什么？”—最常引用的原因之一是“文化”(下面引用了 HBR 的文章)，这是事实，但还有其他因素也有影响。

[](https://hbr.org/2019/07/building-the-ai-powered-organization) [## 构建人工智能驱动的组织

### 简单来说，问题是许多公司扩大人工智能规模的努力都失败了。那是因为只有…

hbr.org](https://hbr.org/2019/07/building-the-ai-powered-organization) 

作为一名数据科学从业者，我认为这些计划失败的原因之一是因为“进行分析与扩展分析”是两码事。如果你询问任何大型组织中“分析团队”的起源，你会听到一个熟悉的故事，即 3-4 名数据科学家/数据工程师在一个孤岛式团队中工作，试图构建一个数据湖，探索数据科学的用例，并开发概念证明(PoCs)以向企业展示数据的价值。虽然这种探索性的工作方式是一个很好的开始，但当涉及到将这些算法嵌入到这些组织的工作流程中时，大多数团队都犹豫了。这是因为进行分析在很大程度上是一个数据科学家可以解决的分析问题，而扩展分析需要的不仅仅是一个数据科学家团队，我将在下面详细阐述。

![](img/b98c5ec85b43cdfb5bcaa2df1c6e3f3f.png)

照片由[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> 人工智能/人工智能确实可以用大量数据做非凡的事情，但要进行规模分析，第一个问题是，‘你有‘正确的’数据吗？’

当你开始谈论“正确的”数据时，你会在数据科学家中听到一个常见的缩写词是 GIGO(垃圾输入，垃圾输出)。大多数组织都坐在来自不同来源的大量数据之上，他们通常会吹嘘自己收集了多少字节的数据。然而，数十亿字节的数据并不能转化为数据科学的成功，大多数公司只有在开始调查规模分析后才意识到这一点。当您试图合并不同来源的数据时，比如合并销售数据和营销数据，或者合并供应链数据和销售数据，就会出现问题；这时猫从袋子里出来了。然而，这些用于销售、营销等的 IT 系统。当你试图构建管道将它们结合起来时，孤立地工作就像一种魅力——**数据质量/完整性**问题会阻碍你进行规模化分析。

当涉及到数据质量问题时，一个经常被建议的解决方法是在组织中建立一个“数据治理”模型。虽然拥有这一点很重要，但你必须再问自己两个问题—

**i)你的业务有多“数字化”,你的数据收集过程有多少是自动化的？**

如果你拿优步、脸书、亚马逊和 Airbnb 这样的企业来说，它们的整个业务流程都是数字化的，并且在某种程度上是可追踪的。在这样的公司中扩展分析比在更传统的企业中更容易，如制造业、汽车、航空航天等。原因在于，业务流程中仍有某些元素没有完全数字化(或者有一些元素依赖于手动数据输入)。因此，当更多像上面这样的传统公司想要加入使用数据科学的行列时，结果并不好，因为数据收集过程还没有标准化(或)简化。

**ii)您从项目一开始就让数据科学家参与进来了吗？**

在大多数公司，数据科学家只在开发模型/算法时才会参与进来。理想情况下，数据科学家应该从业务需求/理解阶段开始参与，在这个阶段，您构思问题陈述，并开始查看要解决的数据。如果数据科学家无法参与，那么让具备分析知识的人参与进来非常重要，这样他/她可以确保“正确的数据”可用于构建模型。许多公司在让“技术”人员参与商业讨论时会三思，最终会选择那些从分析角度来看你更有可能失败的项目。麦肯锡已经发现了这一差距，并就“分析翻译”这一中介角色发表了文章。

[](https://www.mckinsey.com/business-functions/mckinsey-analytics/our-insights/analytics-translator) [## 分析翻译:新的必备角色

### 众所周知，组织越来越多地转向高级分析和人工智能(AI)…

www.mckinsey.com](https://www.mckinsey.com/business-functions/mckinsey-analytics/our-insights/analytics-translator) 

> **假设你有高质量的数据来开发突破性的 ML 解决方案，扩展分析的下一个问题是，“你有合适的基础设施吗？”**

在功能强大的笔记本电脑上使用 Python/R 进行大量预测对于进行分析来说是非常好的，但是当涉及到扩展时，云是一种方式。虽然云计算已经存在了十多年，并且大多数组织都在接受它，但是关于云的讨论通常仅限于 it 部门。这不被视为一种战略优势，也很少出现在董事会的对话中。IT 只是您业务的推动者的日子已经一去不复返了，在这个后疫情时代，IT/分析/技术几乎就是您的业务。[正如高盛首席执行官劳埃德·布兰克费恩在 2017 年初所说，“我们是一家技术公司。我们是一个平台。”](https://digital.hbs.edu/platform-digit/submission/goldman-sachs-a-technology-company/)这种业务平台思维对于扩展分析至关重要，因为合适的云基础设施可以帮助您快速实现除其他优势之外的三个目标——

i) **跨 IT 系统的数据集成**，支持您的业务和分析模型的嵌入

ii) **使用云自动化工具跟踪您的业务工作流程，协调各种任务/模型**

iii) **模型管理和维护**，这一领域最近出现了 MLOps，可以帮助您随着时间的推移对模型进行微调

> **希望您在数据和基础架构上打勾，下一个关于扩展分析的问题是，“您是否有一个适合在整个价值链中使用分析的运营模式？”**

每个组织都有一个决策框架，他们依赖这个框架来运行他们的业务操作。虽然大多数公司的决策都会用到数据，但如果你想利用在整个价值链中扩展分析的复合优势，你的思考和运营方式必须改变。业内专家称之为 [**【最后一英里挑战】**，这被定义为分析的最后阶段，其中*洞察力被转化为推动价值*的变化或结果。](https://www.forbes.com/sites/brentdykes/2019/12/18/two-keys-to-conquering-the-last-mile-in-analytics/?sh=385107a594fb)我个人曾有过这样的经历:我们以极高的准确性/度量标准构建了令人惊叹的模型，但企业还没有做好使用该模型的准备。这又回到了我们之前讨论的“文化”方面，这就是你需要在 3p 中进行根本转变的地方——人、过程和目的。

i) **People** —分析行业多年来一直在努力应对这一挑战，解决方案非常简单；提升数据科学和分析技能。当我与想使用人工智能的人交谈时，他们发现模型很神秘，可以为你解决任何问题，但忘记了实际上它们是协同工作的数学代理。人们观念的转变不会在一夜之间发生，因此组织必须优先考虑数据素养，并确保每个人都了解它，以使用这些模型。

ii) **流程**——你作为一家企业的运作方式需要改变，如果这种思维方式的转变在任何组织中都是“自上而下”的，那会更好。领导团队必须在跨地区、市场和所有职能领域的数据&云战略上保持一致。另一个重要的部分是过程是关于你的分析模型的“可解释性”(这是最近的一个大趋势),需要集成到你的业务过程的结构中，这样人们可以更好地信任这些模型。

iii) **目的**——这确实触及了“为什么”的哲学层面你甚至应该使用分析。答案是“**效率和优化**”。当你有一家小公司时，你可能会根据专家的意见/直觉来运作，但仍然保持竞争力。但是，随着业务的增长，做出正确的决策变得更加困难，每一个错误的决策都会造成浪费。因此，在整个公司部署分析的最终目的是更有效地运作，并找到进一步优化的领域。

摘自[http://gph.is/2fuCtXw](http://gph.is/2fuCtXw)

**结论:**

总之，如果你想在你的组织中更好地扩展分析，你将需要这些关键要素—

1.  不要在数据科学上草率行事。尝试首先在整个 IT 环境中修复您的数据，然后进入科学部分
2.  拥抱云，并在您的业务价值链中积极使用它
3.  首先考虑您将在哪里使用分析模型来推动您的业务，然后相应地在人员和流程方面做出改变。

参考文献—

1][https://www . tlnt . com/purpose-people-and-process-the-3-PS-of-effective-performance-management/](https://www.tlnt.com/purpose-people-and-process-the-3-ps-of-effective-performance-management/)

2][https://digital . HBS . edu/platform-digital/submission/Goldman-Sachs-a-technology-company/](https://digital.hbs.edu/platform-digit/submission/goldman-sachs-a-technology-company/)

3][https://HBR . org/2021/02/为什么很难成为数据驱动型公司](https://hbr.org/2021/02/why-is-it-so-hard-to-become-a-data-driven-company)

4][https://www . McKinsey . com/business-functions/McKinsey-analytics/our-insights/breaking-away-the-secrets-to-scaling-analytics](https://www.mckinsey.com/business-functions/mckinsey-analytics/our-insights/breaking-away-the-secrets-to-scaling-analytics)

5][https://www . Forbes . com/sites/Brent dykes/2019/12/18/two-keys-to-converting-the-last-mile-analytics/？sh=385107a594fb](https://www.forbes.com/sites/brentdykes/2019/12/18/two-keys-to-conquering-the-last-mile-in-analytics/?sh=385107a594fb)

6][https://www . Gartner . com/en/news room/press-releases/2018-02-13-Gartner-says-近一半的首席信息官正在计划部署人工智能](https://www.gartner.com/en/newsroom/press-releases/2018-02-13-gartner-says-nearly-half-of-cios-are-planning-to-deploy-artificial-intelligence)