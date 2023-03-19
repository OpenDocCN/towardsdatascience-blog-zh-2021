# 圣诞节 12 天的 12 个有用算法

> 原文：<https://towardsdatascience.com/12-useful-algorithms-for-12-days-of-christmas-62e4e789f3f6?source=collection_archive---------3----------------------->

## 所有数据科学家都应该知道的非常酷的算法

![](img/485077d54f0a56e9c81c61a8815baf05.png)

由 freepik 创建的背景向量—[www.freepik.com](http://www.freepik.com)

> ***务必*** [***订阅此处***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学的指南、诀窍和技巧、生活经验等文章！***

# 介绍

又到了一年的这个时候了！这一次，不是[圣诞节 12 天的 12 个数据科学项目](/12-data-science-projects-for-12-days-of-christmas-aff693f5ed2b)，我想和你们分享 12 个非常酷、非常强大的算法，我们不只是在谈论监督算法。

我们在同一页上，一个**算法**是一系列用于解决问题的指令。从技术上来说，菜谱是一种算法，但我们将专注于与数据科学相关的算法。

我将更多地关注每个算法背后的逻辑，而不是数学，但我会提供额外的资源以防万一。:)

说到这里，让我们开始吧！

# 1.基于密度的噪声应用空间聚类

![](img/c461dcf51731bbf349c2a6b60dff5a33.png)

作者创建的图像

## 这是什么？

DBSCAN，也称为基于密度的带噪声应用程序空间聚类，是一种聚类算法，它通过查找密集聚集在一起的区域(换句话说，具有许多近邻的点)来识别聚类。

DBSCAN 是最好的聚类算法(优于 k-means 聚类或层次聚类),原因如下:

*   它可以自己确定最佳的集群数量
*   它可以发现异常形状的集群，而不仅仅是圆形的
*   它足够健壮，不会受到离群值的影响

## 更多资源

<https://en.wikipedia.org/wiki/DBSCAN>  <https://www.kdnuggets.com/2020/04/dbscan-clustering-algorithm-machine-learning.html>  </how-dbscan-works-and-why-should-i-use-it-443b4a191c80>  

> ***一定要*** [***订阅这里***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学的指南、诀窍和技巧、生活经验等文章！***

# 2.TF-IDF

![](img/4c205c0518bc366d3b43026c591f3f79.png)

TF-IDF 方程

## 这是什么？

TF-IDF 代表术语频率-逆文档频率，它用于确定一个词在语料库(文档集合)中作为文档的重要性。

具体而言，给定单词的 TD-IDF 值相对于该单词在文档中出现的次数而增加，并随着语料库中也包含该特定单词的文档数量而减少。这是为了说明一般情况下更常用的单词。

TF-IDF 是自然语言处理和信息检索领域的一种流行技术。

## 更多资源

  <https://monkeylearn.com/blog/what-is-tf-idf/>  <https://www.analyticsvidhya.com/blog/2021/11/how-sklearns-tfidfvectorizer-calculates-tf-idf-values/>  

# 3.Apriori 算法

## 这是什么？

Apriori 算法是一种关联规则算法，最常用于确定项目集中相互关联最紧密的项目组。

举个例子，假设我们有一个顾客在杂货店购物的数据库。Apriori 算法可用于确定哪些商品对或商品组最常一起购买。

主要有两个参数:支持度和信心。支持度指的是物品出现的频率，而置信度表示在一个或多个其他物品被购买的情况下，一个物品被购买的条件概率。

## 更多资源

  <https://www.educative.io/edpresso/what-is-the-apriori-algorithm>  

# 4.霍尔特-温特斯指数平滑

## 这是什么？

霍尔特-温特斯指数平滑法，也称为三重指数平滑法，是一种流行的预测技术，用于显示趋势和季节性的时间序列数据。

它被称为三重指数平滑，因为它考虑了数据的水平、数据的趋势和数据的季节性。

这种预测方法相对于其他方法(如 ARIMA)的优势在于:

*   理解和实现起来很简单
*   它相当准确
*   而且它的计算成本很低，也不占用太多资源

## 更多资源

  </holt-winters-exponential-smoothing-d703072c0572>  <https://machinelearningmastery.com/exponential-smoothing-for-time-series-forecasting-in-python/>  

> ***务必*** [***订阅此处***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学指南、诀窍和技巧、生活经验等的文章！***

# 5.矩阵分解

## 这是什么？

矩阵分解算法是一种常用于构建推荐系统的[协同过滤](https://en.wikipedia.org/wiki/Collaborative_filtering)算法。

![](img/660f926a04a9d6771ceb541e8a1c21bb.png)

[摘自维基百科，可以免费分享](https://en.wikipedia.org/wiki/File:Collaborative_filtering.gif)

协同过滤背后的思想是基于其他相似用户的兴趣来预测给定用户的兴趣。这被称为基于记忆的方法，但另一种方法是基于模型的方法，其中使用机器学习算法来预测用户对未评级项目的评级。

## 更多资源

  </recommendation-system-matrix-factorization-d61978660b4b>  <https://developers.google.com/machine-learning/recommendation/collaborative/matrix>  

# 6.莱文斯坦距离

![](img/70c860d9e3b1537ce41bab12ff8aaeed.png)

作者创建的图像

## 这是什么？

Levenshtein 距离是一种简单的算法，用于确定两个字符串之间的相似性。

具体来说，它等于将一个单词更改为另一个单词的最小单字符编辑(替换、添加、删除)次数。

例如，“taco”和“eggs”之间的 Levenshtein 距离是 4。“十字”和“纵横字谜”之间的 Levenshtein 距离也是 4。直觉上，这些对排序相同是很奇怪的，这显示了这种算法的局限性。

所以，我推荐的两个更好的字符串相似性算法是 Trigram 和 Jaro-Winkler 算法。

## 更多资源

  <https://blog.paperspace.com/implementing-levenshtein-distance-word-autocomplete-autocorrect/>  

# 7.页面等级

## 这是什么？

PageRank 是 Google 创建的一种算法，用于对他们的网页进行搜索结果排名。根据谷歌的说法，“ *PageRank 的工作原理是统计一个页面的链接数量和质量，以确定对该网站重要性的粗略估计。潜在的假设是，更重要的网站可能会从其他网站收到更多的链接*。

简化公式如下:

![](img/2a959d5d4f44f0ec82182b752ebe111b.png)

对于链接到页面 u 的所有页面，页面 u 的页面排名等于页面 v 的页面排名之和除以来自页面 v 的链接数量

## 更多资源

<https://en.wikipedia.org/wiki/PageRank>  <https://www.link-assistant.com/news/google-pagerank-algorithm.html>  

# 8.Dijkstra 算法

![](img/4a13c84466c2c1b11fbc4612aa07b6c0.png)

作者创建的图像

## 这是什么？

Dijkstra 算法是一种用于在节点图中寻找从起始顶点(节点)到目标顶点(节点)的最短路径的算法。

Dijkstra 的算法采用贪婪的迭代方法，生成一组信息，其中包括从一个起始顶点到图形中每隔一个顶点的最短路径(见下面的视频)。

Dijkstra 的算法通常用于寻找最短的运输路线，确定最短的石油线路长度，并在一些社交媒体应用程序中使用。

## 更多资源

<https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm>  <https://stackabuse.com/dijkstras-algorithm-in-python/>  

> ***务必*** [***订阅此处***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学的指南、诀窍和技巧、生活经验等文章！***

# 9.ε-贪婪算法

## 这是什么？

ε-贪婪算法是解决[多臂强盗问题](https://en.wikipedia.org/wiki/Multi-armed_bandit)的一种简单方法，该问题代表了探索与开发的两难选择。

这个问题背后的想法是，有 k 个不同的选择，每个选择都有不同的回报，但是你不知道任何一个选择的回报。所以，你从*探索*不同的选择开始，随着时间的推移，在*探索*更多的选择和*开发*报酬最高的选择之间有一个权衡。

使用ε-贪婪算法，在时间的一部分ε中选择一个随机的备选项。其余时间(1-ε)，选择已知最高支出(奖励)的备选项。ε是一个必须设置的参数。

更好的解决方案包括置信上限解决方案和贝叶斯汤普森抽样。

## 更多资源

<https://www.geeksforgeeks.org/epsilon-greedy-algorithm-in-reinforcement-learning/>  <https://en.wikipedia.org/wiki/Multi-armed_bandit#Optimal_solutions>  

# 10.高斯朴素贝叶斯

## 这是什么？

朴素贝叶斯是一种基于贝叶斯定理的分类最大似然算法。

更具体地说，高斯朴素贝叶斯是一种朴素贝叶斯，它假设连续变量遵循正态分布，并由以下等式表示:

![](img/92518031ebcba437a56a017fc57c39cf.png)

虽然朴素贝叶斯持有非常不切实际的假设，但它速度快，易于实现，并且适用于多类预测。这使得它在实时预测、多类预测、文本分类，甚至补充推荐系统方面很受欢迎。

## 更多资源

</a-mathematical-explanation-of-naive-bayes-in-5-minutes-44adebcdb5f8>  <https://scikit-learn.org/stable/modules/naive_bayes.html>  

# 11.随机森林分类器

## 这是什么？

随机森林算法是一种集成学习算法，它涉及创建多个决策树，然后选择每个决策树的所有预测的模式。通过使用投票系统，或“多数获胜”模型，它减少了个别树的可能性误差。结果是一个更准确、更稳健、偏差更小的模型。

![](img/6d9957bd84d2bb166d0b1a0c4c658434.png)

作者创建的图像

用这个图像，如果我们创建一个决策树，第三个，它会预测 0。但是通过使用所有四个决策树，预测值将是 1。

## 更多资源

</understanding-random-forest-58381e0602d2>  

# 12.哈里斯角探测器

## 这是什么？

Harris 角点检测器是一种在计算机视觉算法中用来识别图像中的角点的算子。这对于图像处理和计算机视觉是很重要的，因为已知角点是图像中的重要特征。

基本直觉如下:

![](img/b14a56d198b1d70976364a235a88e93a.png)

作者创建的图像

*   在**平坦的**区域，任何方向都没有渐变(颜色变化)。
*   在**边缘**区域，边缘方向没有梯度变化。
*   只有在**角**区域中，在所有方向上都有梯度变化

因此，在整个图像中使用这种方法来确定图像中的角点在哪里。

## 更多资源

<https://medium.com/data-breach/introduction-to-harris-corner-detector-32a88850b3f6>    

# 感谢阅读！

> 被 ***确定要*** [***订阅此处***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学指南、诀窍和技巧、生活经验等的文章！***

不确定接下来要读什么？我为你选了另一篇文章:

</all-machine-learning-algorithms-you-should-know-in-2022-db5b4ccdf32f>  

**还有一个:**

</the-10-best-data-visualizations-of-2021-fec4c5cf6cdb>  

# 特伦斯·申

*   ***如果你喜欢这个，*** [***订阅我的媒介***](https://terenceshin.medium.com/membership) ***获取独家内容！***
*   ***同样，你也可以*** [***关注我上媒***](https://medium.com/@terenceshin)
*   [***报名我的个人简讯***](https://terenceshin.substack.com/embed)
*   ***跟我上***[***LinkedIn***](https://www.linkedin.com/in/terenceshin/)***其他内容***