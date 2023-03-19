# 全球 ML 社区的数据管理平台

> 原文：<https://towardsdatascience.com/a-data-management-platform-for-the-global-ml-community-57a15da01bb?source=collection_archive---------38----------------------->

## 走出数据工程，为混乱的数据提供系统的解决方案

![](img/3e62821909715e05f7ec89c84101496c.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

**简介**

如果有一个机器学习(ML)社区需要立即采取的行动，那就是有效地处理数据危机。我们面临的数据危机是由数据工程的成本和复杂性以及新出现的工具和方法的大杂烩推动的。

关于“民主化人工智能”的讨论已经持续了太长时间，各种倡议和努力跨越了所有的想象空间。然而，关键的 ML 成分，数据，似乎已经从讨论中缺席。冒着指出显而易见的风险，一个正常运行的 ML 管道需要在 ML 民主化之前实现数据的民主化。

很少有组织的 ML 工作没有受到开发和维护数据集所需资源的严重阻碍。从学术界到工业界，从材料科学到法律，存储、管理、共享、查找和使用数据用于 ML 研究和应用的需求是很少有人能够克服的障碍。这严重阻碍了 ML 在学术界和工业界的发展，产生了重大的社会影响，包括阻碍科学研究。

明确地说，数据民主化不仅仅是获取数据，而是赋予需要使用数据的人权力。我们给所需的技能和工具起了个名字——数据工程——丝毫无助于解决问题。虽然完全消除数据工程是不现实的，但必须努力显著减轻它。为了做到这一点，ML 数据管理平台的主要开源设计需要 1)对全球社区(commons)可用，以及 2)可由非编程专家使用。

少数几家世界领先的技术公司拥有资源和专业知识来开发满足其内部需求的数据管理平台。然而，这只会加剧贫富不均的状况。源自云的主流开源设计不太可能，因为主要的云提供商都在构建自己的专有平台。然而，没有什么可以阻止云提供商实现他们自己版本的完全集成到他们堆栈中的通用平台。

**要求**

当提到平台时，我指的是用于数据管理的一体化解决方案。从高层次来看，数据管理平台需要以下列内容为目标:

*   通过将工程从数据工程中分离出来，为世界各地的数据科学和 ML 用户提供支持。
*   支持管理 ML 中的整个数据生命周期。
*   通过集成整个 ML 工作流程来支持 ML 生命周期。
*   支持主流的 ML 框架。
*   支持各种 ML 数据，包括传感器和仪器数据。

**需要识别的内容**

虽然这一切听起来很棒，但问题是没有这样的平台存在，所以问题是我们如何到达那里？最近，我一直在与一些个人和组织讨论这个问题，并得出结论认为，需要认识到以下几点:

*不是工程问题*

必须认识到，数据管理平台缺乏主导设计不是一个工程问题，而是一个超越工程的战略问题。

*专业知识和资源*

Big tech 拥有开发和维护这样一个平台的专业知识和资源，并且正在进行可行的内部开发工作，以满足他们自己的内部需求。相比之下，初创公司没有资源、经验和用户基础来启动它。为了启动这项工作，应该通过 big tech 内部目前正在开发的一个或多个内部平台的贡献来“资本化”。

*初始用户群和采用情况*

如果这个平台有任何未来，它需要有一个广泛采用的路线图——包括大技术的内部要求。获得这种采用的最佳方式是通过平台本身的集体协作。由于庞大的内部和外部用户群体，一个大的技术起源将给予该平台必要的采用。庞大的用户群将有助于该平台经得起未来考验，证明开发和维护成本的合理性，并推动对该平台的持续采用和投资。

它还将向更广泛的社区发出信号，表明从 1)人的角度来看，投资学习和采用该平台是值得的——考虑将与该平台相关的培训项目纳入其课程的大学，以及 2)从技术角度来看，创业公司和技术提供支持集成。

*协作经济学*

数据工程挑战甚至对大型科技公司来说也是一项成本，而合作开发一个通用平台的主导设计将降低大型科技公司的开发成本，对其他所有人来说也是如此。该平台不会成为直接的收入驱动因素，而是通过增加 ML 的应用和采用，为创收生态系统做出有益贡献。这一层次的合作具有经济意义——竞合。

*等待中的建筑辩论*

面向数据的架构与微服务架构是讨论的一个关键点，至少在短期内可能不会解决。只要有几家公司追求，追求其中一个或两个都是可行的。

**接下来的步骤**

为了超越空谈，ML 社区内越来越多的团体正在审查发起以下活动的选项:

1.  建立参与机制，促进行业在开发和发布数据管理平台方面的协调和合作。
2.  认识需求，并提出平台主导设计的要求。
3.  审查当前的开发工作和数据库技术，并分享建立此类平台的经验。
4.  设计新的解决方案和发展路线图，以应对未来的挑战。
5.  协调公共平台的开发和发布。
6.  协调平台的维护和未来发展。

**总结**

数据科学和机器学习社区与领先的技术公司合作，需要整合现有的开发工作，以协调最终发布的公共数据管理平台。

一个用于 ML 的数据管理平台，确保在整个 ML 生命周期中的一致和高质量的数据流，将使构建高质量的数据集和 ML 系统的构建更具可重复性和系统性。

现在是联合起来让数据工程再次变得不酷的时候了。首先要认识到，一组组织在一个平台上合作可以实现更多的目标，因为需求大体相似，如果组内有差异，这些差异会被世界各地的其他组织共享。

**相关阅读:**

面向大数据的数据清洗方法综述。
https://www . science direct . com/science/article/pii/s 1877050919318885

面向数据编程的演变
[https://tborchertblog.wordpress.com/2020/02/13/28/](https://tborchertblog.wordpress.com/2020/02/13/28/)。

分析和缓解 https://vldb.org/pvldb/vol14/p771-mohan.pdf DNN 培训
[中的数据停滞](https://vldb.org/pvldb/vol14/p771-mohan.pdf)

数据科学中的基准和过程管理:我们会走出困境吗？
[https://dl.acm.org/doi/10.1145/3097983.3120998](https://dl.acm.org/doi/10.1145/3097983.3120998)

部署机器学习的挑战:案例研究调查
[https://arxiv.org/abs/2011.09926v2](https://arxiv.org/abs/2011.09926v2)

DBMS 大概率阴天:企业级 ML
[https://arxiv.org/abs/1909.00084](https://arxiv.org/abs/1909.00084)10 年预测

面向所有人的数据工程
https://www.sigarch.org/data-engineering-for-everyone/ T4

生产机器学习中的数据生命周期挑战
[https://sigmodRecord . org/publications/sigmodRecord/1806/pdf/04 _ Surveys _ polyzotis . pdf](https://sigmodrecord.org/publications/sigmodRecord/1806/pdfs/04_Surveys_Polyzotis.pdf)

生产机器学习中的数据管理挑战
[https://dl.acm.org/doi/10.1145/3035918.3054782](https://dl.acm.org/doi/10.1145/3035918.3054782)

机器学习的数据平台
[https://dl.acm.org/citation.cfm?doid=3299869.3314050](https://dl.acm.org/citation.cfm?doid=3299869.3314050)

软件团队中的数据科学家:最新技术和挑战
[https://ieeexplore.ieee.org/document/8046093](https://ieeexplore.ieee.org/document/8046093)

机器学习的数据验证
[https://mlsys.org/Conferences/2019/doc/2019/167.pdf](https://mlsys.org/Conferences/2019/doc/2019/167.pdf)

检测数据错误:我们在哪里，需要做什么？[https://dl.acm.org/doi/10.14778/2994509.2994518](https://dl.acm.org/doi/10.14778/2994509.2994518)T21

用 ML 推理扩展关系查询处理
[https://arxiv.org/abs/1911.00231](https://arxiv.org/abs/1911.00231)

火鸟:亚特兰大预测火灾风险和优先消防检查。
[https://www.cc.gatech.edu/~dchau/papers/16-kdd-firebird.pdf](https://www.cc.gatech.edu/~dchau/papers/16-kdd-firebird.pdf)

对于大数据科学家来说，“看门人工作”是 Insights 的关键障碍
[https://www . nytimes . com/2014/08/18/technology/for-the-Big-Data-Scientists-Hurdle-to-Insights-Is-guardian-Work . html](https://www.nytimes.com/2014/08/18/technology/for-big-data-scientists-hurdle-to-insights-is-janitor-work.html)

朦胧研究:以数据为中心的人工智能
[https://github.com/hazyresearch/data-centric-ai](https://github.com/hazyresearch/data-centric-ai)

项目建议:机器学习排名服务的数据管理平台
[https://research.google/pubs/pub47850/](https://research.google/pubs/pub47850/)

现代面向数据编程
[http://inverse probability . com/talks/notes/modern-data-oriented-programming . html](http://inverseprobability.com/talks/notes/modern-data-oriented-programming.html)。

数据准备水平。
[https://arxiv.org/abs/1705.02245](https://arxiv.org/abs/1705.02245)

人、计算机和热得一塌糊涂的真实数据
[https://dl.acm.org/doi/10.1145/2939672.2945356](https://dl.acm.org/doi/10.1145/2939672.2945356)

重新思考用于 ML 的数据存储和预处理
[https://www . sigarch . org/re thinking-Data-Storage-and-Preprocessing-for-ML/](https://www.sigarch.org/rethinking-data-storage-and-preprocessing-for-ml/)

机器学习的规则:ML 工程的最佳实践
[https://developers . Google . com/machine-learning/guides/rules-of-ML](https://developers.google.com/machine-learning/guides/rules-of-ml)

机器学习系统的技术准备水平
[https://arxiv.org/abs/2101.03989](https://arxiv.org/abs/2101.03989)

tf.data:一个机器学习数据处理框架【https://arxiv.org/pdf/2101.12127.pdf
T22

https://eng.uber.com/uber-big-data-platform/大数据平台:100+Pb，分钟延迟

Zipline: Airbnb 的机器学习数据管理平台
[https://Data bricks . com/session/zip line-airbnbs-Machine-Learning-Data-Management-Platform](https://databricks.com/session/zipline-airbnbs-machine-learning-data-management-platform)