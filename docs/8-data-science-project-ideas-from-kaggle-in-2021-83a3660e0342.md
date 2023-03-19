# 2021 年 Kaggle 的 8 个数据科学项目创意

> 原文：<https://towardsdatascience.com/8-data-science-project-ideas-from-kaggle-in-2021-83a3660e0342?source=collection_archive---------26----------------------->

![](img/103ff1f2526b45256bddc027ffd5ee97.png)

蒂娜·万霍夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

最后，我们来到了 2021 年🎉

这是人生的新篇章🐣

对我来说，作为一名数据科学家，我想利用这个机会总结一份我在 2021 年在 Kaggle 上发现的有趣数据集的列表。我也希望这个列表可以对那些正在寻找数据科学项目来建立自己的投资组合的人有用。

# 动机

在尝试了许多不同的途径来学习数据科学之后，我发现迄今为止最有效的途径是从真实数据集着手进行项目。然而，这听起来很简单，但实际上从零开始构建数据科学投资组合非常具有挑战性。

数据科学是一门宽泛的学科。即使有这么多免费的在线资源，新手也很容易感到迷失。被动地学习新概念并不能保证你下次面对类似的问题时能够解决。

最后，我觉得设计自己的学习地图的能力很重要，以确保你处于主动学习模式。它需要你的热情、逻辑、勤奋和对数据科学的全面理解。

要成为一名积极的学习者，在任何科目中，**兴趣**是你最好的老师。😼

因此，我总结了一些最近从 Kaggle 更新的数据集。这些任务从情感分析到建立预测器不等。我也尝试增加一些延伸阅读，作为更多探索的选择。

(我根据日期和投票选择了数据集。)

# 项目创意

## 人力资源分析:

<https://www.kaggle.com/arashnic/hr-analytics-job-change-of-data-scientists>  

*   预测候选人为公司工作的可能性。
*   解释模型，说明哪些特征影响候选人决策。

数据集不平衡。需要一些策略来修复不平衡的数据集。

</having-an-imbalanced-dataset-here-is-how-you-can-solve-it-1640568947eb>  </comparing-different-classification-machine-learning-models-for-an-imbalanced-dataset-fdae1af3677f>  

## 疫苗推文:

<https://www.kaggle.com/gpreda/pfizer-vaccine-tweets>  

*   浏览关于辉瑞和 BioNTech 新疫苗的推文，了解公众的反应、讨论的话题以及与疫苗相关的积极和消极情绪。

我们可以通过使用拥抱脸包来简化 NLP 过程。

<https://github.com/huggingface/transformers>  

## 信用卡客户:

<https://www.kaggle.com/sakshigoyal7/credit-card-customers>  

*   提高预测流失客户的绩效。
*   想象一下频繁客户和不频繁客户之间的区别。

一个很好的数据集来理解精度和召回。

> 在这个商业问题中，最重要的是识别那些被搅动的顾客。即使我们预测不活跃的客户会变得活跃，这也不会损害我们的业务。但是预测客户变得不活跃就可以了。所以召回率(TP/TP+FN)需要更高。

</beyond-accuracy-precision-and-recall-3da06bea9f6c>  </accuracy-precision-recall-or-f1-331fb37c5cb9>  

## Spotify 音乐数据集:

<https://www.kaggle.com/yamaerenay/spotify-dataset-19212020-160k-tracks>  

*   分析一个世纪以来歌曲的发展趋势。
*   建立一个基于内容的推荐引擎，推荐艺术家。
*   寻找不同流派中有影响力的艺术家。
*   基于其他特征预测歌曲流行度。
*   根据音频特征将不同的流派进行分类。

这是一个很有潜力的数据集。如任务中所列，该数据集适用于推荐引擎、趋势分析、流行度预测器和无监督聚类。

</k-means-clustering-and-pca-to-categorize-music-by-similar-audio-features-df09c93e8b64>  

## 美国大选:

<https://www.kaggle.com/manchunhui/us-election-2020-tweets>  

*   对两位总统候选人的推文进行情感分析。

虽然这个任务要求我们执行情感分析，但我觉得基于文本数据构建词云也是合适的。

</sentiment-classification-in-python-da31833da01b>  </simple-wordcloud-in-python-2ae54a9f58e5>  </introduction-to-nlp-part-5a-unsupervised-topic-model-in-python-733f76b3dc2d>  

## 比特币价格:

<https://www.kaggle.com/mczielinski/bitcoin-historical-data>  

*   从历史数据预测比特币价格。

这可以是一个时间序列分析任务。

</an-end-to-end-project-on-time-series-analysis-and-forecasting-with-python-4835e6bf050b>  </time-series-analysis-in-python-an-introduction-70d5a5b1d52a>  

## Yelp:

<https://www.kaggle.com/yelp-dataset/yelp-dataset>  

*   建立餐厅推荐模型。
*   对 Yelp 评论进行情感分析。
*   根据评论，预测餐厅评级。

对于餐馆推荐，热图也可能有所帮助。

</data-101s-spatial-visualizations-and-analysis-in-python-with-folium-39730da2adf>  

## Airbnb:

<https://www.kaggle.com/dgomonov/new-york-city-airbnb-open-data>  

*   预测纽约 Airbnb 租赁价格。
*   通过查看纽约市地图上的 Airbnb 来比较不同地区的价格。
*   按房型分析价格是否有差异。

我可能想把重点放在这个经典项目的一些创造性的数据可视化上。

<https://medium.com/mobgen/airbnb-and-house-prices-in-amsterdam-part-1-9dc0cbffc136>  <https://medium.com/typeme/lets-code-a-neural-network-from-scratch-part-2-87e209661638>  

# 结论

当我把上述资源总结成一个阅读清单时，我真的很享受自己的学习。有些项目可能具有挑战性，但努力总会有回报。对我来说，一个困难的项目想法比一个简单的想法让我更愿意学习。同时，一个复杂的数据集通常包含更多的特征，使我们能够深入地完成一个项目。

努力总会有回报的。🏆

我希望你在这篇文章中找到一些真正让你感兴趣的项目想法！

新年快乐

快乐学习

下次见