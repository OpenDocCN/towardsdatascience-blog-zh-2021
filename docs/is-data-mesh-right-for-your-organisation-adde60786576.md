# 数据网格适合您的组织吗？

> 原文：<https://towardsdatascience.com/is-data-mesh-right-for-your-organisation-adde60786576?source=collection_archive---------34----------------------->

## 如何知道您的组织是否真的准备好深入数据网格？

![](img/520cb45046ba1bf75888132771d4cae5.png)

*照片由* [*马里奥·高*](https://unsplash.com/@mariogogh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *上* [*下*](https://unsplash.com/s/photos/office?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在上一篇文章中，我们讨论了像野火一样在数据社区中传播的新企业数据架构— [数据网格架构](https://hyperight.com/what-is-data-mesh-and-should-you-mesh-it-up-too/)。我们提到，它代表了数据架构中的一种范式转变，数据行业也随之转变，从优先考虑集中式、整体式数据湖和数据库的大规模数据团队转变为优先考虑数据域和数据产品的一等公民。

简而言之，它呈现了[分布式领域驱动架构](https://martinfowler.com/articles/data-monolith-to-mesh.html)、自助服务平台设计和数据化产品思维的融合。为了更好地理解这一概念，我们与 Data Edge 的合作伙伴管理顾问 Daniel Tidströ进行了交谈，他至少在相当长的一段时间内一直从事数据网格的部分工作。

丹尼尔解释说，当公司快速扩张时，数据网格变得至关重要。随着数据源和数据消费者的激增，让一个中央团队来管理和掌控数据接收、数据转换以及向所有潜在利益相关方提供数据，将不可避免地导致扩展问题鉴于数据在我们的组织中日益重要，为可扩展团队和可扩展平台进行设计至关重要，”Daniel 解释道。

关于数据网格，他提到的一件重要的事情是开始讨论数据的分布，因为数据创建在所有公司中都是固有分布的。“随着数据源的数量每天都在增长，许多组织可能至少应该考虑一下他们的扩展选项。

对于想知道数据网格是否适合他们的公司，丹尼尔建议，如果你有领域驱动的开发，开始使用微服务，或者如果你进行云迁移，这是一个考虑它的好时机。

Monte Carlo 的联合创始人兼首席执行官 Barr Moses 表示，面向领域的数据架构，如数据网格，为团队提供了两个世界中最好的东西:一个集中式数据库(或分布式数据湖),其中的领域(或业务领域)负责处理他们自己的管道。这样，数据网格通过将数据架构分解成更小的、面向领域的组件，使得数据架构更容易扩展。

![](img/140d3519a8eeabc616b3278bed83ec1d.png)

来自[派克斯](https://www.pexels.com/photo/photo-of-people-near-computers-3183159/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[派克斯](https://www.pexels.com/@fauxels?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

# 数据网格对所有类型的组织都有意义吗？

在采访中，Scling 的创始人 Lars Albertsson 将数据网格描述为扩展大型数据组织的一种方式，在大型数据组织中，由于在数据平台中工作的团队数量众多，数据管理和治理变得非常具有挑战性。

“通过将数据管理和治理联合到拥有数据源和数据管道的团队，数据网格可以让公司进一步扩展，”Lars 解释道。

采用大数据和数据运营的公司在多年的时间里经历了围绕数据的组织结构的几个阶段。在早期阶段，有一个团队或几个紧密协作的团队使用单个数据平台，其中的核心组件通常是与批处理管道相结合的数据湖，潜在地补充了流处理能力。

大多数公司要么处于这些早期阶段，要么还没有建立起他们的第一个数据平台。越来越多的数据成熟公司已经成功地将数据创新扩展到先锋团队之外，并将数据处理能力大众化。为了促进数据民主化并使治理易于管理，数据平台技术和开发流程保持同质。Lars 补充说，通常会有小的变化，但是如果熵没有得到控制，过度的摩擦将会阻止数据民主化。

![](img/90f98ef2f4bac7f238741815cd220ec7.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/software?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

集中式数据平台可以扩展到大型组织。对于其文化难以扩展集中式服务的公司来说，集中式数据治理可能被视为一个瓶颈。在这种情况下，数据网格可以通过分配治理责任来进一步扩展。在实践中，数据网格化身是一个数据平台和湖，它被分成多个池和多个处理环境，由不同的团队控制和操作。Lars 解释说，数据网格的大小取决于公司协调中央数据平台的能力。

最后，Lars 指出，数据网格并不是扩展到大量团队的唯一选择；有足够能力协调数据管理的公司可以保持一个集中的数据平台，避免分散化的开销。

> [阅读 Lars Albertsson 的全部采访内容](https://hyperight.com/dataops-is-the-backbone-of-ai-innovation-interview-with-lars-albertsson/)

然而，如何知道您的组织是否真的准备好深入数据网格？为了帮助公司做出决定，Barr Moses 和她的团队以调查的形式为公司创建了计算，以确定你的组织投资数据网格是否有意义。通过回答有关他们的数据源、数据团队、数据域、数据工程瓶颈、数据治理和数据可观察性的问题，他们会得到一个分数，帮助他们决定是否应该采用数据网格。您可以在蒙特卡洛首席执行官 Barr Moses 和蒙特卡洛首席技术官 Lior Gavish 撰写的[数据网格实施指南](/what-is-a-data-mesh-and-how-not-to-mesh-it-up-210710bb41e0)中找到计算方法。

![](img/27f1be09960d307c17d1ef00b79405c3.png)

照片由[马太·亨利](https://burst.shopify.com/@matthew_henry?utm_campaign=photo_credit&utm_content=Free+Coding+On+Laptop+Image%3A+Browse+1000s+of+Pics&utm_medium=referral&utm_source=credit)从[爆](https://burst.shopify.com/coding?utm_campaign=photo_credit&utm_content=Free+Coding+On+Laptop+Image%3A+Browse+1000s+of+Pics&utm_medium=referral&utm_source=credit)

# 数据网格和数据操作的未来展望

Data Mesh 和 DataOps 都将在未来十年颠覆数据和分析行业。但是他们会进步并改变组织吗？

关于上述内容，Lars 表示，与传统公司相比，完全采用 DataOps 的组织在处理数据方面的效率要高出 10-100 倍。尽管这些特征是基于对成熟度范围内许多公司的观察而做出的主观估计，并且没有针对数据运营的科学测量，但 Lars 表示，它们与不同 DevOps 成熟度水平的公司的科学测量运营指标相匹配，如 DevOps 报告的[状态所示。数据运营在新想法的交付时间和出现故障时的恢复时间方面似乎具有类似的效果。](https://services.google.com/fh/files/misc/state-of-devops-2019.pdf)

"这种效率差距如此之大，以至于具有破坏性."DataOps 实际上是从机器学习技术中获得可持续价值的一个要求。构建和运行机器学习应用程序，保持它们的健康，并进行迭代以不断改进它们是复杂和昂贵的。Lars 说，没有实现高水平数据成熟度的公司可能会碰上一两次运气，但无法以可持续和可重复的方式提供人工智能创新。

与此相反，Lars 表示，数据网格不是一个颠覆性的概念，但它是数据非常成熟的大公司进一步扩展的一种方式。“这些公司已经从数据中获得了颠覆性的价值，并完全采用了机器学习技术。”

拉尔斯认为令人担忧的是，围绕数据网格的大部分讨论是在尚未达到这一成熟度水平的公司之间进行的。

“过早采用数据网格可能是有害的；如果您尚未在数据平台中建立强大、同质的约定和流程，那么分散化将会引入异构性，从而减缓创新并阻止有效的数据民主化。”

Lars 认为，对于大多数组织来说，采用数据网格实际上可能是一种倒退，并重新引入大数据时代之前的数据孤岛。"[对数据网格的描述](https://martinfowler.com/articles/data-monolith-to-mesh.html)倾向于强调拥有数据的团队有责任将干净、高质量的数据作为数据产品发布，这种模式也被称为数据中心。"

他说，将数据质量改进的责任转移给拥有领域专业知识的团队通常是一个好主意，也是迈向数据成熟的一个进化步骤，但基础原始数据也必须可用。

![](img/8714fb4b22d1ebc545950c583f60ea2e.png)

照片由 [Anyrgb](https://www.anyrgb.com/en/free-image-ybnrnl)

为了澄清，Lars 举了无效金融交易的例子，对于财务报告的情况来说，可能需要清除这些交易，但它们可能是欺诈检测信号的金矿。“将原始数据隐藏在筒仓中是数据中心的一个缺点，也是采用数据网格时的一个重大风险，除非公司首先在一个集中式平台中建立强大的数据民主实践。因此，我担心围绕数据网格的讨论将是有害的，并诱使数据不太成熟的公司建立数据孤岛。”

与此相反，DataOps 没有这种风险。“如果您的公司能够让拥有不同技能的人很好地合作，那么只会带来好处，并会加速您的数据成熟之旅。”

…

***您有过实施数据网格的经验吗？请在下面的评论中分享你的想法。***

*原载于 2021 年 1 月 28 日 Hyperight.com*[](https://hyperight.com/is-data-mesh-right-for-your-organisation/)**。**