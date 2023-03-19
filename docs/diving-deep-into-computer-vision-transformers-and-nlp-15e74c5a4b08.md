# 深入研究计算机视觉、变形金刚和自然语言处理

> 原文：<https://towardsdatascience.com/diving-deep-into-computer-vision-transformers-and-nlp-15e74c5a4b08?source=collection_archive---------17----------------------->

你已经有一段时间没有机会深入探讨一个话题了吗？你来对地方了。最近，我们很多人的生活都很忙碌，这意味着花时间真正学习、吸收和反思可能是一个挑战。我们在这里帮助选择最近的帖子，这些帖子涵盖了广泛而多样的主题，但仍然有一个共同点:对复杂的主题采取耐心、仔细和引人入胜的方法。我们开始吧！

*   [**赶上变形金刚**](/advanced-techniques-for-fine-tuning-transformers-82e4e61e16e) 的高级微调技术。在[之前的一篇文章](/transformers-can-you-rate-the-complexity-of-reading-passages-17c76da3403)中向我们介绍了微调的基础知识之后， [Peggy Chang](https://medium.com/u/b08bdbf6e014?source=post_page-----15e74c5a4b08--------------------------------) 将读者带到了下一个层次，解释了如何处理更棘手的情况(如大型模型和较小的数据集)，同时使用包括分层学习率衰减和随机权重平均(SWA)在内的方法来提高性能。

![](img/5061b9db6ff8f3e1ed9b37b79f2f8430.png)

照片由[凯尔西·多迪](https://unsplash.com/@khana_photo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

*   [**建立一个图像分类系统，做出类似人类的决策**](/programming-an-intuitive-image-classifier-part-1-266bd657aa4) 。大多数与计算机视觉相关的教程在粗略地做了一些介绍性笔记后就直接进入了实现阶段。Raveena Jayadev 的帖子与众不同之处在于，我们确实得到了一个全面的、实际操作的指导，来构建一个能够识别数字的机器学习系统，但这是在我们首先了解该系统的“心智模型”如何运作之前。
*   [**在线(机器)学习中打好基础**](/a-broad-and-practical-exposition-of-online-learning-techniques-a4cbc300dcd4) 。作为一个流行的深度学习主题，在线学习在模型“必须从不断可用的新数据中学习”的场景中有许多行业应用，[卡梅隆·沃尔夫](https://medium.com/u/28aa6026c553?source=post_page-----15e74c5a4b08--------------------------------)在他对该主题的基本(全面)介绍中解释道。卡梅伦的帖子继续涵盖了在线学习方法，如正规化、提炼和再平衡(仅举几例)。
*   [**从设计阶段**](/designing-a-fairness-workflow-for-your-ml-models-6518a5fc127e) 就确保你的 ML 模型的公平性。 [Divya Gopinath](https://medium.com/u/2519b46a1fe5?source=post_page-----15e74c5a4b08--------------------------------) 从实用主义的角度探讨了机器学习中的公平问题:仅仅*想要*避免偏见是不够的；这种意图需要转化为可衡量和可持续的行动。她的最新帖子涵盖了公平 ML 模型设计的基本原则。
*   [**学习如何用自然语言处理方法推断因果关系**](/causal-inference-using-natural-language-processing-da0e222b84b) 。Haaya Naushan 的最新文章建立在她之前关于计量经济学的工作的基础上，并研究了 NLP 技术在因果推理中的一些有前途的应用，特别是在社会科学的背景下。
*   [**反思 AI 系统的灵活性**](/thinking-fast-and-slow-ai-edition-ccb44cfff16e) 。经过短暂的夏季休息后，TDS 播客带着新一季和令人兴奋的新嘉宾阵容回归主持人杰瑞米·哈里斯。这一集的主角是 IBM 人工智能伦理全球负责人 Francesca Rossi，他讨论了人类认知和人工智能之间的联系，人工通用智能的潜在出现，以及可解释的人工智能(XAI)研究的前景。

谢谢你花时间和我们一起度过这一周，也谢谢你用各种方式支持我们的出版物和我们的作者。

直到下一个变量，
TDS 编辑器

# 我们策划主题的最新内容:

## [入门](https://towardsdatascience.com/tagged/getting-started)

*   [逻辑回归系数的简单解释](/a-simple-interpretation-of-logistic-regression-coefficients-e3a40a62e8cf)作者[迪娜·扬科维奇](https://medium.com/u/b7b4973f5897?source=post_page-----15e74c5a4b08--------------------------------)
*   [一小时内使用 ggplot2 进行数据可视化的指南](/guide-to-data-visualization-with-ggplot2-in-a-hour-634c7e3bc9dd)作者 [Chi Nguyen](https://medium.com/u/e982f12a6925?source=post_page-----15e74c5a4b08--------------------------------)
*   [基于主题模型的推荐系统](/topic-model-based-recommendation-systems-a02d198408b7)作者[杰米·麦高恩](https://medium.com/u/685229ed4b15?source=post_page-----15e74c5a4b08--------------------------------)

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

*   [多层感知器用现实生活中的例子和 Python 代码解释:情感分析](/multilayer-perceptron-explained-with-a-real-life-example-and-python-code-sentiment-analysis-cb408ee93141)作者[卡罗莱纳便当](https://medium.com/u/e960c0367546?source=post_page-----15e74c5a4b08--------------------------------)
*   [Julia 上带跳转的混合整数规划综合研究(第三部分)](/a-comprehensive-study-of-mixed-integer-programming-with-jump-on-julia-part-3-847ad5b3c625)ouguenouni Mohamed
*   [如何通过](/how-to-generate-professional-api-docs-in-minutes-from-docstrings-aed0341bbda7) [Tirthajyoti Sarkar](https://medium.com/u/cb9d97d4b61a?source=post_page-----15e74c5a4b08--------------------------------) 在几分钟内从文档字符串生成专业 API 文档

## [深潜](https://towardsdatascience.com/tagged/deep-dives)

*   [使用贝叶斯定理设计知识驱动模型的分步指南](/a-step-by-step-guide-in-designing-knowledge-driven-models-using-bayesian-theorem-7433f6fd64be)作者[埃尔多安·塔克森](https://medium.com/u/4e636e2ef813?source=post_page-----15e74c5a4b08--------------------------------)
*   [统计机器学习:内核化广义线性模型(GLMs) &内核化线性回归](/statistical-machine-learning-kernelized-generalized-linear-models-glms-kernelized-linear-876e72a17678)作者[安德鲁·罗斯曼](https://medium.com/u/4688574fc42a?source=post_page-----15e74c5a4b08--------------------------------)
*   [测度论中的概率](/measure-theory-in-probability-c8aaf1dea87c)作者[奚楚张](https://medium.com/u/4bc88b1b8f22?source=post_page-----15e74c5a4b08--------------------------------)

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

*   [由](/a-primer-on-atrous-convolutions-and-depth-wise-separable-convolutions-443b106919f5) [Aadhithya Sankar](https://medium.com/u/82053676fe58?source=post_page-----15e74c5a4b08--------------------------------) 编写的关于阿特鲁(扩张)卷积和深度方向可分卷积的初级读本
*   [反馈校准方法](/feedback-alignment-methods-7e6c41446e36)作者[阿尔伯特·希门尼斯](https://medium.com/u/8ee877dce271?source=post_page-----15e74c5a4b08--------------------------------)
*   [利用时态词嵌入测量语义变化](/measuring-semantic-changes-using-temporal-word-embedding-6fc3f16cfdb4)作者[燕妮君](https://medium.com/u/12ca1ab81192?source=post_page-----15e74c5a4b08--------------------------------)