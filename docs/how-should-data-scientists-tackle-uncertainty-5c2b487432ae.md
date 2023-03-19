# 数据科学家应该如何应对不确定性？

> 原文：<https://towardsdatascience.com/how-should-data-scientists-tackle-uncertainty-5c2b487432ae?source=collection_archive---------19----------------------->

商业利益相关者、决策者和外行人都向数据科学家寻求清晰的、基于数据的建议。事实上，数据往往是杂乱的，科学和艺术之间的分界线可能(也确实)变得模糊不清。

本周，我们分享了一系列优秀的帖子，这些帖子以这样或那样的方式处理了不确定性问题，以及如何以清醒的头脑、开放的心态和足够的好奇心来处理这个问题。

*   [**学习如何在神经网络中建模不确定性**](/modeling-uncertainty-in-neural-networks-with-tensorflow-probability-a706c2274d12) 。我们从卷起袖子开始:当你看到“当我们用完全相同的设置多次重复相同的实验时，结果会出现不可预测的差异”时，你会怎么做？[亚历山大·莫拉克](https://medium.com/u/f390f1bdd353?source=post_page-----5c2b487432ae--------------------------------)的教程向我们介绍了任意不确定性，并提出了一种使用张量流概率来解决它的方法。
*   我们如何减轻气候对经济的影响？恶劣天气对供应链和经济行为的影响可能是极端的，随着气候变化，这种影响变得越来越明显。[王博士](https://medium.com/u/4351cf1c9413?source=post_page-----5c2b487432ae--------------------------------)向我们介绍了她的团队发起的“将恶劣天气影响的昂贵混乱转化为准确的影响数据”的过程，重点是快餐和零售需求。

![](img/9b1508f076c77d9dae8576d35e32e8c6.png)

[杰西卡·鲁斯切洛](https://unsplash.com/@jruscello?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*   [**AI 能否适应快速增长的环境担忧？**“人工智能灵活性”可能是答案吗？](/what-would-a-green-ai-look-like-28d91aaff3be)
*   [**仔细看看不确定性的本质**](/ditch-p-values-use-bootstrap-confidence-intervals-instead-bba56322b522) 。在行业数据科学家的日常工作中，建议和发现经常围绕着概率。正如[弗洛伦特·比森](https://medium.com/u/4252c9e9ceb5?source=post_page-----5c2b487432ae--------------------------------)建议的，这些对话应该有更多的细微差别；他认为过度依赖 p 值会产生次优决策，并建议使用 Bootstrap 置信区间。
*   [**在选择模型特征时，我们应该考虑哪些因素？**](/characteristics-of-a-good-feature-4f1ac7a90a42) 根据他丰富的行业经验， [Conor O'Sullivan](https://medium.com/u/4ae48256fb37?source=post_page-----5c2b487432ae--------------------------------) 深入探讨了特性选择的过程。他坚持认为，我们应该超越预测能力——消除不确定性的关键因素——而采取更全面的方法，考虑可解释性、稳定性和道德等因素。
*   [**用 AI 伦理塑造用户体验**](/ai-ethics-as-a-user-experience-challenge-ceb7abb3cd38) 。还是在工业领域，最近的 TDS 播客节目邀请了主持人 Jeremie Harris 和 Shopify 的工程和数据科学总监 Wendy Foster。Wendy 的人工智能伦理和产品设计方法不是引导用户走上预定的道路，而是依靠不确定性，为用户提供清晰的选择和权衡，并使他们能够做出明智的决定。

感谢您加入我们又一周的大数据科学写作。感到鼓舞支持我们作者的工作？[考虑成为中等会员](https://medium.com/membership)。

直到下一个变量，
TDS 编辑

# 我们策划主题的最新内容:

## [入门](https://towardsdatascience.com/tagged/getting-started)

*   [商务人士所说的细分](https://medium.com/towards-data-science/what-business-people-really-mean-when-they-say-segmentation-82f2a815b7b1)实际上寻找的是什么[姚文玲](https://medium.com/u/dea357b44e49?source=post_page-----5c2b487432ae--------------------------------)
*   [在将 AI 引入您的项目之前要问的七个问题](/seven-questions-to-ask-before-introducing-ai-to-your-project-b969d591c98b)作者 [Aliaksei Mikhailiuk](https://medium.com/u/30bef13bba71?source=post_page-----5c2b487432ae--------------------------------)
*   [Irene Chang](/build-a-flask-heroku-mood-tracker-web-app-using-the-spotify-api-14b3b5c92ac9)[使用 Spotify API](https://medium.com/u/6d381bb66361?source=post_page-----5c2b487432ae--------------------------------) 创建一个 Flask-Heroku 情绪跟踪器网络应用

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

*   [由](/orchestrate-a-data-science-project-in-python-with-prefect-e69c61a49074) [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----5c2b487432ae--------------------------------) 与提督一起用 Python 编排一个数据科学项目
*   [英语到印地语的神经机器翻译](/english-to-hindi-neural-machine-translation-7cb3a426491f)作者[马哈尔什·罗伊](https://medium.com/u/37f16212df3c?source=post_page-----5c2b487432ae--------------------------------)
*   [如何在谷歌分析中衡量用户互动](/how-to-measure-user-interactions-in-google-analytics-cc5f5a32b02b)克洛伊·摩根
*   [用 Spark + R 分析巴西的紧急财政援助](/using-spark-r-to-analyze-emergency-financial-assistance-data-in-brazil-92957e0e25a7)作者 [Cleiton Rocha](https://medium.com/u/450e8c22f706?source=post_page-----5c2b487432ae--------------------------------)

## [深潜](https://towardsdatascience.com/tagged/deep-dives)

*   [1 毫秒延迟下的抱脸变压器推断](/hugging-face-transformer-inference-under-1-millisecond-latency-e1be0057a51c)作者[Michal benesy](https://medium.com/u/9515e0e75a23?source=post_page-----5c2b487432ae--------------------------------)
*   [AI 状态报告:2021 年摘要](/state-of-ai-report-2021-summary-6c16f4eb72a6)作者[丹尼尔·伯克](https://medium.com/u/dbc019e228f5?source=post_page-----5c2b487432ae--------------------------------)
*   [通过](/training-provably-robust-neural-networks-1e15f2d80be2) [Klas Leino](https://medium.com/u/d1ed71c899ca?source=post_page-----5c2b487432ae--------------------------------) 训练可证明健壮的神经网络

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

*   [Asset2Vec:将 3D 物体转化为矢量并返回](/asset2vec-turning-3d-objects-into-vectors-and-back-8335496b756d)作者 [Jonathan Laserson 博士](https://medium.com/u/56d1c8006910?source=post_page-----5c2b487432ae--------------------------------)
*   [可解释的人工智能(XAI)。但是，为谁？](/explainable-artificial-intelligence-xai-but-for-who-696aa1e65c67)作者[卡洛斯·穆根](https://medium.com/u/d5344df58d03?source=post_page-----5c2b487432ae--------------------------------)
*   [时间序列的傅立叶变换](/fourier-transform-for-time-series-292eb887b101)作者[约奥斯·科尔斯坦杰](https://medium.com/u/8fa2918bdae8?source=post_page-----5c2b487432ae--------------------------------)
*   [机器学习时代的稳健决策](/robust-decision-making-in-the-era-of-machine-learning-2dc43fd571a6)作者[傅鹏程](https://medium.com/u/b3ea629b1d17?source=post_page-----5c2b487432ae--------------------------------)