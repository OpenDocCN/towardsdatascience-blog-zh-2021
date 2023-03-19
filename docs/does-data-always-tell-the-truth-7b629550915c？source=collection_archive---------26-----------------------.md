# 数据总是说真话吗？

> 原文：<https://towardsdatascience.com/does-data-always-tell-the-truth-7b629550915c?source=collection_archive---------26----------------------->

## 我们每周精选的必读编辑精选和原创特写

近年来，我们可以收集的数据量呈指数级增长，我们可以用来分析数据的计算能力也是如此。然而，数据科学家仍然不得不每天围绕使用这一新发现的权力做出艰难的决定。在商业、科技和医药领域，以及或多或少的其他人类活动领域，情况都是如此。

例如，大公司和小公司都在投入资金和资源，以确保他们的业务战略与底层数据保持一致。然而，正如斯科特·伦德伯格(Scott Lundberg)所展示的那样，我们仍然必须非常小心[，不要落入令人困惑的相关性和因果性的陷阱](/be-careful-when-interpreting-predictive-models-in-search-of-causal-insights-e68626e664b6)。微调一个模型来预测哪些客户会流失是一回事；确定哪些特征对客户行为有直接的因果影响需要一系列不同的问题。Scott 关于这个复杂现实世界问题的帖子清晰易懂，[你绝对应该读一下](/be-careful-when-interpreting-predictive-models-in-search-of-causal-insights-e68626e664b6)。

![](img/fc0cb700acd74a230a93d5d525c25e73.png)

照片由 [Stefano Marsella](https://unsplash.com/@smars06?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有时候，看似微不足道的事实比我们想象的要复杂得多。在我们的日常生活中，如果有人问我们“你多大了？”即使我们中那些认为这个问题不符合外交辞令的人也能马上给出明确的答案。另一方面，我们的 DNA 拥有自己的真理。在他们最近的项目中，[埃莉诺拉·尚西拉](https://medium.com/u/b3b22bfeb19f?source=post_page-----7b629550915c--------------------------------)和她的合著者研究了实足年龄和生物学年龄之间令人惊讶的差距，[专注于 DNA 甲基化作为前者](/predicting-age-with-dna-methylation-data-99043406084)的预测因子。 [Valerie Carey](https://medium.com/u/1a7c9171898f?source=post_page-----7b629550915c--------------------------------) 从不同的方向处理与年龄相关的数据，[观察年龄如何影响预测模型中的公平措施](/measuring-fairness-when-ages-differ-177d9597dd3b)。她的结论是，“如果不考虑年龄效应，试图纠正或调整模型以均衡指标可能会产生意想不到的后果。”

仔细的数据分析可能是一种强大且必要的缓解工具，可以用来对付试图利用算法驱动平台弱点的恶意行为者。[安娜·雅各布森](https://medium.com/u/a3696d672c34?source=post_page-----7b629550915c--------------------------------)和她的团队[检查了来自疑似俄罗斯巨魔工厂](/the-askeladden-algorithm-10859c349fc9)的近 900 万条推文，试图防止未来美国选举诚信的风险。与此同时，在 TDS 播客上，[杰瑞米·哈里斯](https://medium.com/u/59564831d1eb?source=post_page-----7b629550915c--------------------------------)与罗西·坎贝尔[就自由分享尖端人工智能研究的潜在危险](/should-all-ai-research-be-published-5226ad5145b4)以及该领域更好的出版规范和实践的必要性进行了交谈。

如果你已经做到了这一步，你可能已经得出了合理的结论，即数据可能是——而且经常是——杂乱的，从数据中提取有价值的事实可能会更加混乱。然而，这并不是绝望的原因——我们经常在近处发现困惑和灵感。本周我们的一些最佳指南和教程的作者也是如此，所以如果你正在寻找具体的实践知识来添加到你的工具包中，你就来对地方了:

*   从茱莉亚·尼库尔斯基(Julia Nikulski)[对基于变形金刚的 NLP 模型的介绍](/how-to-use-transformer-based-nlp-models-a42adbc292e5)(想想伯特或 GPT-2)开始，它向读者展示了他们如何将它们应用于下游任务。
*   [马塞尔·海德曼](https://medium.com/u/a1e42c5e8445?source=post_page-----7b629550915c--------------------------------)和他的队友查看了一个巨大的房地产数据集[创建了一个预测市场需求的模型](/our-search-for-demand-rex-real-estate-index-51590c8a7de1)。
*   受全球推动尽可能快速高效地分发新冠肺炎疫苗的启示， [Lila Mullany](https://medium.com/u/d55d0a812c15?source=post_page-----7b629550915c--------------------------------) 探索了几种利用计算机视觉简化现场疫苗接种流程的方法[。](/use-computer-vision-to-capture-real-world-events-57dc890e82d5)
*   最后，[杨](https://medium.com/u/aae603d0709?source=post_page-----7b629550915c--------------------------------)深入探讨了不平衡回归这个“非常实用但很少研究的问题”，而[则带领读者通过一些强有力的策略来避免这个问题](/strategies-and-tactics-for-regression-on-imbalanced-data-61eeb0921fca)。

感谢您阅读、分享和互动我们发布的作品。我们希望本周的选择能激励你在你目前正在处理的任何数据集中找到真理，并在时机成熟时采取行动。如果你喜欢这些帖子，考虑[成为一个媒体成员](https://medium.com/membership)来支持我们的作者和我们的社区。

直到下一个变量，
TDS 编辑器

## 我们策划主题的最新内容:

## 入门指南

*   [追踪你的机器学习进度的清单](/a-checklist-to-track-your-machine-learning-progress-801405f5cf86)作者[帕斯卡·詹尼茨基](https://medium.com/u/672b95fdf976?source=post_page-----7b629550915c--------------------------------)
*   [关于 Vicky Yu](/what-product-analysts-should-know-about-a-b-testing-a7bdc8e9a61) 进行的 A/B 测试，产品分析师应该知道什么
*   [如何让您的公司为数据科学做好准备](/how-to-get-your-company-ready-for-data-science-6bbd94139926)作者[Marvin lü作者](https://medium.com/u/b99261ddee6b?source=post_page-----7b629550915c--------------------------------)

## 实践教程

*   [构建可扩展的高性能大数据系统](/building-a-scalable-and-high-performances-big-data-system-221c6b6893eb)作者[米歇尔·丽娃](https://medium.com/u/d062237dbca7?source=post_page-----7b629550915c--------------------------------)
*   [Parul Pandey](/use-colab-more-efficiently-with-these-hacks-fc89ef1162d8)[利用这些技巧](https://medium.com/u/7053de462a28?source=post_page-----7b629550915c--------------------------------)更有效地使用 Colab】
*   [由](/a-comprehensive-beginners-guide-to-the-diverse-field-of-anomaly-detection-8c818d153995)[张秀坤·波尔泽](https://medium.com/u/3ab8d3143e32?source=post_page-----7b629550915c--------------------------------)撰写的关于异常检测的综合初学者指南

## 深潜

*   [棒球和机器学习:2021 年击球预测的数据科学方法](/baseball-and-machine-learning-a-data-science-approach-to-2021-hitting-projections-4d6eeed01ede#d7ff-de7c8875d501)作者[约翰·佩特](https://medium.com/u/42835e566980?source=post_page-----7b629550915c--------------------------------)
*   [深度分裂 Q-Learning 和吃豆人女士](/deep-split-q-learning-and-ms-pacman-5749791d55c8)作者[卢克·格里斯沃尔德](https://medium.com/u/eeeb494bd3e7?source=post_page-----7b629550915c--------------------------------)
*   [数据科学管理中的 7 项任务](/the-7-tasks-in-data-science-management-b01f2a48c846)作者[马丁·施密特](https://medium.com/u/bbad6dc43b40?source=post_page-----7b629550915c--------------------------------)和[马塞尔·何冰](https://medium.com/u/c02b8fd991fd?source=post_page-----7b629550915c--------------------------------)

## 思想和理论

*   [由](/compression-in-the-imagenet-dataset-34c56d14d463)[马克斯·埃利希](https://medium.com/u/cf880ddfcaf1?source=post_page-----7b629550915c--------------------------------)在 ImageNet 数据集中进行压缩
*   [图表示学习——网络嵌入(上)](/graph-representation-learning-network-embeddings-d1162625c52b)朱塞佩·富蒂亚
*   [Wouter van hees wijk 博士](/the-four-policy-classes-of-reinforcement-learning-38185daa6c8a)[的强化学习](https://medium.com/u/33f45c9ab481?source=post_page-----7b629550915c--------------------------------)的四个政策类