# 逻辑回归，解释清楚(加上九个方便的小抄)

> 原文：<https://towardsdatascience.com/logistic-regression-clearly-explained-plus-nine-handy-cheat-sheets-5c7f31441a05?source=collection_archive---------25----------------------->

如果你觉得三月是一个永无止境的苦旅，你并不孤单。无论是疫情的一周年纪念，还是仅仅是一个奇怪的季节转换，我们都不确定；然而，没有什么比学习新事物更能让我们从昏睡中清醒过来。一个吸引人的、充满活力的帖子你今天会用到吗？阅读[卡罗琳娜·本托](https://medium.com/u/e960c0367546?source=post_page-----5c7f31441a05--------------------------------)的[逻辑回归实用介绍](/logistic-regression-in-real-life-building-a-daily-productivity-classification-model-a0fc2c70584e)，在那里她非常清晰地展示了模型的现实生活用例。

![](img/cced8869068a58cb0b1deb2c71644822.png)

照片由 [Sneha Chekuri](https://unsplash.com/@snehachekuri93?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

既然我们已经迎头跨入了新的一个月，也是时候为接下来的几周制定日程了。[罗伯特·兰格](https://medium.com/u/638b9cae9933?source=post_page-----5c7f31441a05--------------------------------)带着四篇深度学习论文的[新选集回来了，将在四月份阅读，这是一种保持最新研究*和*与成千上万其他 TDS 读者一起这样做的有益方式。](/four-deep-learning-papers-to-read-in-april-2021-77f6b0e42b9b)

在实际操作方面，我们最关注的最新资源和指南包括…

*   [Sara A. Metwalli](https://medium.com/u/7938431b336a?source=post_page-----5c7f31441a05--------------------------------) 的九张备忘单的便利[集合，涵盖了可视化和自然语言处理等流行数据科学主题的要点。](/9-comprehensive-cheat-sheets-for-data-science-46005d72b485)
*   [使用标签传播算法的指南](/how-to-get-away-with-few-labels-label-propagation-f891782ada5c)，作者[罗伯特·库伯勒](https://medium.com/u/6d6b5fb431bf?source=post_page-----5c7f31441a05--------------------------------)博士，当你遇到部分标记或未标记的数据集时。
*   [尤娜·申](https://medium.com/u/ec5df6268466?source=post_page-----5c7f31441a05--------------------------------)的[装置作品基于纽约现代艺术博物馆](/using-momas-collection-dataset-to-visualize-whose-stories-are-missing-76a8960a33c2)的数据，突显出该系列缺乏来自边缘化群体的艺术家的代表性。

## 未来，无论是近的还是远的

在不确定的时期(经济和其他方面)，我们很多人都想知道我们是否已经为事业成功做好了准备；有些人可能会决定换工作，甚至换职业。 [Kendric Ng](https://medium.com/u/a02f9c809bc8?source=post_page-----5c7f31441a05--------------------------------) 从一个特定且及时的角度探讨了这个话题，[询问对于想在科技公司找到工作的人来说，数据科学硕士学位是否是必要的](/do-i-have-to-get-a-masters-to-break-into-data-science-27ae217dfb81)。受肯德里克的启发，TDS 团队从我们的档案中收集了其他几篇相关的帖子，[就学位](/masters-of-the-house-on-degrees-and-credentials-in-data-science-9769b9d829b4)和证书在数据科学和相邻领域的重要性发表了广泛的意见。

在求职和学位之外，还有更远的人工智能领域，有一天我们构建的人工智能可能会比我们的最佳意图更聪明、更具说服力。在最新一期的 TDS 播客中， [Jeremie Harris](https://medium.com/u/59564831d1eb?source=post_page-----5c7f31441a05--------------------------------) 采访了牛津人类未来研究所的研究员斯图亚特·阿姆斯特朗，关于[我们今天应该做些什么来避免不久前还属于科幻小说领域的场景](/ai-humanitys-endgame-e3d93e0f9969)。这是一次有趣的谈话，我们希望你能听一听。

一如既往，我们感谢你们所有人推动我们发表数据科学方面的最佳作品，并为这个社区带来新的令人兴奋的声音。你的支持真的很重要。

直到下一个变量，
TDS 编辑器

## 我们策划主题的最新内容:

## 入门指南

*   你的朋友比你有更多的朋友
*   [如何轻松将多个 Jupyter 笔记本合并成一个](/how-to-easily-merge-multiple-jupyter-notebooks-into-one-e464a22d2dc4)作者[阿玛尔·哈斯尼](https://medium.com/u/d38873cbc5aa?source=post_page-----5c7f31441a05--------------------------------)
*   [如何通过](/7-ways-to-measure-the-value-of-data-208314bba3be) [Borna Almasi](https://medium.com/u/7ad3e1b0b177?source=post_page-----5c7f31441a05--------------------------------) 测量数据的值

## 实践教程

*   [由](/feature-generation-with-gradient-boosted-decision-trees-21d4946d6ab5) [Carlos Mougan](https://medium.com/u/d5344df58d03?source=post_page-----5c7f31441a05--------------------------------) 利用梯度增强决策树进行特征生成
*   [加速弹性搜索中的 BERT 搜索](/speeding-up-bert-search-in-elasticsearch-750f1f34f455)由 [Dmitry Kan](https://medium.com/u/95a0ba753977?source=post_page-----5c7f31441a05--------------------------------)
*   [在 Python Jupyter 笔记本中运行线性混合效果模型的三种方法](/how-to-run-linear-mixed-effects-models-in-python-jupyter-notebooks-4f8079c4b589)作者 [Jin Hyun Cheong](https://medium.com/u/ed25f2e73793?source=post_page-----5c7f31441a05--------------------------------)

## 深潜

*   [NMF——一个可视化解释器和 Python 实现](/nmf-a-visual-explainer-and-python-implementation-7ecdd73491f8)作者[阿努帕玛·加拉](https://medium.com/u/df96a905cd52?source=post_page-----5c7f31441a05--------------------------------)
*   [斑马医疗视觉如何开发临床人工智能解决方案](/how-zebra-medical-vision-developed-clinical-ai-solutions-34b385617b65)作者 [Markus Schmitt](https://medium.com/u/902b7c650a38?source=post_page-----5c7f31441a05--------------------------------)
*   [音频深度学习变得简单:自动语音识别(ASR)，它是如何工作的](/audio-deep-learning-made-simple-automatic-speech-recognition-asr-how-it-works-716cfce4c706)作者 [Ketan Doshi](https://medium.com/u/54f9ca55ed47?source=post_page-----5c7f31441a05--------------------------------)

## 思想和理论

*   [现实生活元学习:教与学](/real-life-meta-learning-teaching-and-learning-to-learn-186877376709)作者[瑞安·桑德](https://medium.com/u/dca93f60cd11?source=post_page-----5c7f31441a05--------------------------------)
*   [进入 j(r)VAE:除法，(旋转)，和排序…卡](/enter-the-j-r-vae-divide-rotate-and-order-the-cards-9d10c6633726)由[马克西姆·齐亚丁诺夫](https://medium.com/u/eb4c5879edd?source=post_page-----5c7f31441a05--------------------------------)
*   [质量多样性算法:MAP-Polar](/quality-diversity-algorithms-a-new-approach-based-on-map-elites-applied-to-robot-navigation-f51380deec5d) 作者 [Ouaguenouni Mohamed](https://medium.com/u/6c5dbf6956c8?source=post_page-----5c7f31441a05--------------------------------)