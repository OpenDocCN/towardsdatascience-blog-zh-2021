# 机器学习中能否兼顾准确性和公平性？

> 原文：<https://towardsdatascience.com/can-we-balance-accuracy-and-fairness-in-machine-learning-e44ebd9f800e?source=collection_archive---------29----------------------->

## 我们每周精选的必读编辑精选和原创特写

作为每天与代码打交道的人，数据科学家有时会不可避免地默认二进制思维。1 和 0。信号和噪声。统计显著性——或其缺失。

正如 [Jessica Dai](https://medium.com/u/c2db4ee4d10e?source=post_page-----e44ebd9f800e--------------------------------) 在[最近发表的一篇关于算法和公平性的文章](/algorithms-for-fair-machine-learning-an-introduction-2e428b7791f3)中所写的那样，我们*没有*在每次对话中都坚持这种非此即彼的框架，尤其是当问题是建立不会延续偏见的模型时。纵观整个开发生命周期，Jessica 指出了潜在的干预点，数据科学家可以在不牺牲准确性的情况下充当防止偏见的护栏。最重要的是，她认为，“ML 从业者必须与利益相关者合作，如商业领袖、人文专家、合规和法律团队，并制定一个如何最好地对待你的人口的计划。”

![](img/6722869a93f3dd9883156e3caacdf4d7.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Piret Ilver](https://unsplash.com/@saltsup?utm_source=medium&utm_medium=referral) 拍摄

在现实世界的需求和道德实践之间取得正确的平衡是数据科学中许多其他活跃对话的核心。在过去的一周里，TDS 编辑 [Caitlin Kindig](https://medium.com/u/2155e1f99318?source=post_page-----e44ebd9f800e--------------------------------) 收集了几篇令人大开眼界的帖子，解释了[在收集大量数据时，联合学习如何减轻隐私和安全问题](/ai-privacy-and-data-security-continued-a66e5ac3f593)；在 TDS 播客上， [Jeremie Harris](https://medium.com/u/59564831d1eb?source=post_page-----e44ebd9f800e--------------------------------) 与 Andy Jones 聊起了[规模对人工智能的影响](/ai-safety-and-the-scaling-hypothesis-76bfee57f924)——上述海量数据集如何开启了我们几年前无法想象的机遇，但也带来了新的风险。

虽然像这样的挑战听起来往往是理论上的，但它们已经影响并塑造了机器学习工程师和研究人员的工作。Angela Shi 在[解释靶心图中偏差和方差的可视化表示时，着眼于这个难题的实际应用](/what-bias-variance-bulls-eye-diagram-really-represent-ff6fb9670993)。后退几步， [Federico Bianchi](https://medium.com/u/2aff872fe60e?source=post_page-----e44ebd9f800e--------------------------------) 和 Dirk Hovy 的文章确定了[作者和他们的同事在自然学习处理(NLP)领域面临的最紧迫的问题](/on-the-gap-between-adoption-and-understanding-971c3d63f524):“模型发布然后在应用程序中使用的速度可能超过对其风险和局限性的发现。随着它们规模的增长，复制这些模型来发现这些方面变得更加困难。”

费德里科和德克的文章没有提供具体的解决方案——没有一篇论文可以提供——但它强调了学习、提出正确的(通常也是最困难的)问题以及拒绝接受不可持续的现状的重要性。如果激励你采取行动的是扩展你的知识和增长你的技能，本周我们也有一些很棒的选项供你选择。

*   [罗伯特·兰格](https://medium.com/u/638b9cae9933?source=post_page-----e44ebd9f800e--------------------------------)带着他的[永远令人期待的深度学习论文月度集锦](/four-deep-learning-papers-to-read-in-june-2021-5570cc5213bb)回来了，你不想错过——六月份的阵容涵盖了一些令人兴奋的领域，从自我监督学习到深度神经网络中的类选择性。
*   机器学习能帮助全球应对气候变化吗？如果你持怀疑态度，读一读凯瑟琳·拉兰内的帖子。她向我们介绍了她在爱尔兰的风能项目，并展示了她和她的团队如何努力选择能够在该国电网中产生最高效率的模式。
*   Charles Frenzel 和他的合著者着手解决任何行业中最关键的问题之一:客户流失。虽然数据科学家已经制定了相当长一段时间的流失预测管道，[这篇文章聚焦于预测客户何时可能决定放弃某个产品](/retain-customers-with-time-to-event-modeling-driven-intervention-de517a39c6e3)。
*   还是在商业决策的世界里，罗伯特·库伯勒博士研究了每个营销人员最喜欢的分析工具——A/B 测试！—并展示了[如何将少量的“贝叶斯魔法”注入其中](/bayesian-a-b-testing-in-pymc3-54dceb87af74)将会产生更准确的结果。

感谢您本周加入我们，[支持我们发表的作品](https://medium.com/membership)，并信任我们传递更多的信号而不是噪音。为未来更多活跃的对话干杯。

直到下一个变量，
TDS 编辑器

## 我们策划主题的最新内容:

## 入门指南

*   [Julia Kho](/an-easy-beginners-guide-to-git-2d5a99682a4c)[的 Git](https://medium.com/u/75b5f5a46f52?source=post_page-----e44ebd9f800e--------------------------------) 简易入门指南
*   [回答数据科学度量变化面试问题—终极指南](/answering-the-data-science-metric-change-interview-question-the-ultimate-guide-5e18d62d0dc6)作者 [Hani Azam](https://medium.com/u/14c05fc44ff?source=post_page-----e44ebd9f800e--------------------------------)
*   [Jorge martín Lasa OSA](/clustering-on-numerical-and-categorical-features-6e0ebcf1cbad)[根据数字和分类特征](https://medium.com/u/dd62b41dbbf5?source=post_page-----e44ebd9f800e--------------------------------)进行聚类

## 实践教程

*   [通过](/build-a-conversational-assistant-with-rasa-b410a809572d) [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----e44ebd9f800e--------------------------------) 与 Rasa 建立对话助手
*   [你能建立一个机器学习模型来监控另一个模型吗？](/can-you-build-a-machine-learning-model-to-monitor-another-model-15ad561d26df)由[伊梅利德拉](https://medium.com/u/f21493d48f9f?source=post_page-----e44ebd9f800e--------------------------------)
*   [多元自回归模型和脉冲响应分析](/multivariate-autoregressive-models-and-impulse-response-analysis-cb5ead9b2b68)由 [Haaya Naushan](https://medium.com/u/68f801f1b50b?source=post_page-----e44ebd9f800e--------------------------------)

## 深潜

*   [Ketan Doshi](/transformers-explained-visually-not-just-how-but-why-they-work-so-well-d840bd61a9d3)[用视觉解释了变形金刚——不仅解释了它们是如何工作的，还解释了它们为什么工作得这么好](https://medium.com/u/54f9ca55ed47?source=post_page-----e44ebd9f800e--------------------------------)
*   [深度学习阿尔茨海默病诊断:模型实现](/alzheimer-diagnosis-with-deep-learning-model-implementation-5a0fd31f148f)作者 [Oscar Darias Plasencia](https://medium.com/u/d4424e88f9ed?source=post_page-----e44ebd9f800e--------------------------------)
*   [用更少的μc 做几乎同样多的事情:生物医学命名实体识别的案例研究](/doing-almost-as-much-with-much-less-a-case-study-in-biomedical-named-entity-recognition-efa4abe18ed)

## 思想和理论

*   [吉布斯采样解释了](/gibbs-sampling-explained-b271f332ed8d)由[塞斯比劳](https://medium.com/u/3c3fa8c446bb?source=post_page-----e44ebd9f800e--------------------------------)
*   [作为一名生物医学领域的学术研究者，我的开放和可复制科学工作流程](/my-workflow-for-open-and-reproducible-science-as-an-academic-researcher-in-biomedicine-b41eaabcd420)作者 [Ruben Van Paemel](https://medium.com/u/9af3bb47dd8a?source=post_page-----e44ebd9f800e--------------------------------)
*   在《变形金刚》中，注意力是你真正需要的吗？由[大卫球菌](https://medium.com/u/a182c5459e71?source=post_page-----e44ebd9f800e--------------------------------)