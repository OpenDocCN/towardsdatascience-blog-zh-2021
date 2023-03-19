# 你会相信一种算法来选择你的下一个度假目的地吗？

> 原文：<https://towardsdatascience.com/would-you-trust-an-algorithm-to-choose-your-next-vacation-destination-447ae3877730?source=collection_archive---------32----------------------->

我们有时会想:如果我们让一个人工智能来策划这份简讯，它会和 TDS 的 100%人类团队一样吗？它会依赖于观点、掌声和社交分享吗，或者它会以某种方式检测一篇文章不太有效的品质——作者的声音、原创性或清晰度？ [Carolina Bento](https://medium.com/u/e960c0367546?source=post_page-----447ae3877730--------------------------------) 在她的[关于决策树算法的精彩解释](/decision-tree-classifier-explained-in-real-life-picking-a-vacation-destination-6226b2b60575)中提出了类似的问题。以选择度假目的地的过程为例，她展示了这样一个系统将如何工作，以及它将面临的限制。

![](img/687e723b29068d9893f96f62f2d4b245.png)

照片由 [Ricardo Gomez Angel](https://unsplash.com/@ripato?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

许多专家认为，理解一个模型为什么会产生某种结果比满足于它的输出更重要。在某些方面，一个人为的决定是一样的；幸运的是，通过我们的每周精选，解释我们的选择非常容易。[罗伯特·兰格](https://medium.com/u/638b9cae9933?source=post_page-----447ae3877730--------------------------------)的每月[深度学习研究论文综述](/four-deep-learning-papers-to-read-in-july-2021-e91c546d112d)是 TDS 上的常年最爱，读者不断涌向它，因为它不仅列出并总结了该领域的重要发展，还添加了围绕它们的背景和分析。在文章的另一端， [Elena Etter](https://medium.com/u/54ef08b94813?source=post_page-----447ae3877730--------------------------------) 深入探讨了一个很少讨论但至关重要的话题:[融入数据可视化的各层主观性](/data-strata-6bc0d8484381)，以及它们如何影响媒体的所谓中立性和透明性。

正如你现在可能知道的，我们对执行良好的教程和解释者情有独钟。当作者成功地将一个复杂的话题引入一个吸引人的帖子，激励其他人学习并采取行动时，这总是一种享受。本周，我们特别欣赏了 [CJ Sullivan](https://medium.com/u/a9bc11f7a61b?source=post_page-----447ae3877730--------------------------------) 的动手演示:它专注于[注入在 Neo4j](/visualizing-graph-embeddings-with-t-sne-in-python-10227e7876aa) 中创建的图形嵌入，并用 Streamlit 仪表板可视化它们。 [Pierre Blanchart](https://medium.com/u/21ac52b39efa?source=post_page-----447ae3877730--------------------------------) 转向模型可解释性，并展示了[我们如何在 XGBoost](/explaining-the-decisions-of-xgboost-models-using-counterfactual-examples-fd9c57c83062) 这样的树集合模型中使用反事实解释方法。从理论到实践，[博尔哈·维拉斯科](https://medium.com/u/cd162631459a?source=post_page-----447ae3877730--------------------------------)(和合著者)向我们介绍了[双机器学习的新兴方法](/double-machine-learning-for-causal-inference-78e0c6111f9d)，并解释了它在因果推理环境中的应用。对于任何对计算智能越来越好奇的人来说，[布兰登·摩根](https://medium.com/u/422735e4a376?source=post_page-----447ae3877730--------------------------------)刚刚启动了一个令人兴奋的新项目:一个关于进化计算的[完整课程](/evolutionary-computation-full-course-overview-f4e421e945d9)。(如果你已经看过布兰登的介绍，单位[一](/unit-1-optimization-theory-e416dcf30ba8)和[二](/unit-2-introduction-to-evolutionary-computation-85764137c05a)已经有了！)

我们对坚实、实用的指南的欣赏，只能与我们在了解一些不经常出现在我们视野中的问题和对话时的喜悦相匹配。TDS 播客正是这种讨论的场所，[Jeremie Harris](https://medium.com/u/59564831d1eb?source=post_page-----447ae3877730--------------------------------)[最近与 Jeffrey Ding 的关于中国蓬勃发展的人工智能生态系统](/chinas-ai-ambitions-and-why-they-matter-a7075ba993dc)的一集也不例外。[丹尼尔·安杰洛夫](https://medium.com/u/b47135f530bb?source=post_page-----447ae3877730--------------------------------)提出了一个[发人深省的问题给在工业领域工作的人工智能从业者](/why-dont-we-test-machine-learning-as-we-test-software-43f5720903d):“你怎么知道你开发的系统足够可靠，可以在现实世界中部署？”他继续探索软件开发的测试实践，并检验它们在机器学习中是否同样有用。最后，我们在过去的一周主持了一场关于 TDS 的热烈辩论，帖子权衡了数学技能对数据科学家的重要性。我们留给你 [Sarem Seitz](https://medium.com/u/8f6d033b1a40?source=post_page-----447ae3877730--------------------------------) 的[慷慨激昂的案例，他们称之为“ML 中最不受重视的技能](/you-do-need-math-for-machine-learning-cf934a607960)”，以及为什么学习一个职业的理论基础与一个人发布好的、干净的代码的能力一样重要。

感谢您接受我们的阅读推荐——我们希望您和我们一样喜欢它们。并且，一如既往地感谢你们[让我们的工作成为可能](https://medium.com/membership)。

直到下一个变量，
TDS 编辑器

# 我们策划主题的最新内容:

## [入门](https://towardsdatascience.com/tagged/getting-started)

*   [提高你作为数据科学家影响力的 10 个策略](/10-strategies-to-boost-your-impact-as-a-data-scientist-590f1398ed37)作者[丹尼斯·艾勒斯](https://medium.com/u/7383a58c0e3e?source=post_page-----447ae3877730--------------------------------)
*   分位数是理解概率分布的关键
*   [数据科学如何掌握熊猫](/how-to-master-pandas-for-data-science-b8ab0a9b1042)作者 [Chanin Nantasenamat](https://medium.com/u/f94b47c3cfca?source=post_page-----447ae3877730--------------------------------)

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

*   [语义搜索:由](/semantic-search-measuring-meaning-from-jaccard-to-bert-a5aca61fc325)[詹姆斯·布里格斯](https://medium.com/u/b9d77a4ca1d1?source=post_page-----447ae3877730--------------------------------)测量从 Jaccard 到 BERT 的意义
*   [结构方程建模](/structural-equation-modeling-dca298798f4d)由 [Joos Korstanje](https://medium.com/u/8fa2918bdae8?source=post_page-----447ae3877730--------------------------------)
*   [用这三个工具加速你的命令行导航](/speed-up-your-command-line-navigation-with-these-3-tools-f90105c9aa2b)由 [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----447ae3877730--------------------------------)

## [深潜](https://towardsdatascience.com/tagged/deep-dives)

*   [把你的钱投在你的 ML 上:建立对商业关键人工智能的信任](/put-your-money-where-your-ml-is-building-trust-in-business-critical-ai-f57963dc5109)
*   [线性回归的置信区间从何而来——以魏毅](/where-do-confidence-interval-in-linear-regression-come-from-the-case-of-least-square-formulation-78f3d3ac7117)[的最小二乘公式](https://medium.com/u/1b4bd5317a6e?source=post_page-----447ae3877730--------------------------------)为例
*   [企业 ML——为什么将你的模型投入生产比构建它需要更长的时间](/enterprise-ml-why-getting-your-model-to-production-takes-longer-than-building-it-e44ef80f8969)作者 [Ketan Doshi](https://medium.com/u/54f9ca55ed47?source=post_page-----447ae3877730--------------------------------)

## [思想与理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

*   [通过代理正常化激活消除 CNN 中的批次依赖性](/removing-batch-dependence-in-cnns-by-proxy-normalising-activations-bf4824eb0ba4)Antoine Labatie
*   [节点件:由](/nodepiece-tokenizing-knowledge-graphs-6dd2b91847aa)[迈克尔·高尔金](https://medium.com/u/4d4f8ddd1e68?source=post_page-----447ae3877730--------------------------------)标记知识图
*   [机器学习中的多任务学习](/multi-task-learning-in-machine-learning-20a37c796c9c)由 [Devin Soni](https://medium.com/u/5f4d2b8b896d?source=post_page-----447ae3877730--------------------------------)