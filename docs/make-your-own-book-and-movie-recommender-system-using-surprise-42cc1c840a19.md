# 使用 Surprise 制作您自己的书籍和电影推荐系统

> 原文：<https://towardsdatascience.com/make-your-own-book-and-movie-recommender-system-using-surprise-42cc1c840a19?source=collection_archive---------23----------------------->

![](img/03faf47e1ba093f175070fa17622227f.png)

Juraj Gabriel 在 [Unsplash](https://unsplash.com/s/photos/book-movie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我花了几年的博士时间来构建推荐系统，并从头开始对它们进行基准测试，只有当我完成它时，我才听说了 sikit 图书馆的惊喜。

[GIF](https://gph.is/g/4V6JjGO) by [GIFY](https://gph.is/g/4V6JjGO)

这是我以前就想知道的一个惊喜。这将节省我的时间和精力来实现许多推荐系统基线。

所以我想在这里分享关于这个库的信息，并给出一些关于如何使用它的代码，因为它可能会帮助其中一个读者。让我们开始吧！

# 什么是惊喜？

> Surprise(代表**简单 Python 推荐系统引擎**)是一个 Python 库，用于构建和分析处理显式评级数据的推荐系统。它提供了各种现成的预测算法，如基线算法，邻域法，基于矩阵分解(奇异值分解，PMF，SVD++，NMF)，以及许多其他算法。此外，各种相似性措施(余弦，MSD，皮尔逊…)是内置的。来源:[http://surpriselib.com/](http://surpriselib.com/)

我将在两个不同的数据集上使用 Surprise:

1.  建立一个电影推荐系统。
2.  建立图书推荐系统。

在这两种情况下，我将使用协作过滤技术和基于内容的技术来过滤项目，不用担心我会解释它们之间的区别。

# 使用 Surprise 构建电影推荐系统

## 安装惊喜

```
pip install surprise
```

## 读取数据集

我在这里使用的是 MovieLens 数据集。它包含 2500 万用户评级。数据在。/data/raw 文件夹。我们可以直接加载。csv 文件，具有内置的惊喜功能，但为了以后的灵活性目的，通过 Pandas 数据框加载它会更方便。

出于复杂性考虑，我只使用了这个数据集的一个子集，但是你可以全部使用。然后，我将评级上传到一个惊喜数据集对象，该对象按顺序包含以下字段:

1.  使用者辩证码
2.  电影 Id
3.  相应的评级(通常在 1-5 的范围内)

# 训练和交叉验证协同过滤模型

> 协同过滤方法依赖于用户的偏好以及系统中其他用户的偏好。这个想法是，如果两个用户是志同道合的(在过去就项目的一些偏好达成一致)，与一个用户相关的项目被推荐给另一个用户，反之亦然。他们仅从用户项目评级矩阵中获得推荐。来源:[我的博士论文报告](https://boa.unimib.it › phd_unimib_799490)

Surprise 实现了一些协作算法，如奇异值分解、NMF 等。

```
0it [00:00, ?it/s]RMSE: 0.9265
RMSE: 1.0959
Computing the msd similarity matrix...
Done computing similarity matrix.1it [00:12, 12.72s/it]RMSE: 0.9919
RMSE: 0.9299
RMSE: 1.1025
Computing the msd similarity matrix...
Done computing similarity matrix.2it [00:26, 13.54s/it]RMSE: 0.9915
RMSE: 0.9270
RMSE: 1.0996
Computing the msd similarity matrix...
Done computing similarity matrix.3it [00:42, 14.14s/it]RMSE: 0.9905
```

# 训练基于内容的过滤模型

> 基于内容的过滤方法分析与用户相关的项目的一组特征，并基于这些特征学习用户简档。过滤过程基本上包括将用户简档的特征与项目内容(即项目简档)的特征进行匹配。来源:[我的博士论文报告](https://boa.unimib.it › phd_unimib_799490)

因此，这里我将直接依赖于项目属性，即电影标题。首先，我必须用属性向量描述一个用户概要文件。然后，我将使用这些向量来生成基于相似性度量的推荐。

大多数基于内容的过滤方法是基于启发式模型，该模型在向量空间模型中将用户和项目表示为 TF-IDF(或 BM25)的向量。

我在这段代码中使用了 TF-IDF，它考虑了在一个文档中频繁出现的术语(TF =term-frequency)，但在语料库的其余部分很少出现的术语(IDF = inverse-document-frequency)。

由于项目和用户简档被表示为 TF-IDF 的向量，所以项目以与用户相似度递减的顺序被推荐
(用户简档以项目的相同形式表示)。最常见的度量是余弦相似性，我将在下面使用它来计算两个配置文件之间的相似性。

假设用户想要电影《山达基和信仰的监狱(2015)》中最‘相似’的 10 部电影。

```
Recommending 10 products similar to Going Clear: Scientology and the Prison of Belief (2015)...
-------
	Recommended: Soulless 2 (2015) (score:0.09111541613391295)
	Recommended: Víkend (2015) (score:0.09111541613391295)
	Recommended: No Longer Heroine (2015) (score:0.06179864475000222)
	Recommended: In the Shadow of Women (2015) (score:0.06179864475000222)
	Recommended: Marco Polo: One Hundred Eyes (2015) (score:0.05026525144746273)
	Recommended: Ugly American, The (1963) (score:0.0)
	Recommended: Snitch Cartel, The (El cartel de los sapos) (2011) (score:0.0)
	Recommended: Drone (2014) (score:0.0)
	Recommended: Kenny Rogers as The Gambler (1980) (score:0.0)
	Recommended: Le Rossignol (2005) (score:0.0)
```

# 构建图书推荐系统

这将是一个类似于上面的代码，但应用于不同的数据集。

我使用了 Kaggle 上可用的 [goodbooks-10k](https://www.kaggle.com/zygmunt/goodbooks-10k) 数据集。它包含评级和图书 csv 文件。第一个包含超过 53，000 个用户对 10，000 本书的评级数据。第二个文件包含 10，000 本书的元数据(标题、作者、ISBN 等。).

我通过 [Github](https://github.com/amamimaha/surprise_recommender_systems) 分享了书籍和电影推荐系统。

# 结论

就是这样！您可以将此推荐系统代码模板应用于任何您想要的数据。在接下来的几天里，我还将分享关于上下文推荐系统以及如何实现它。

所以，敬请期待！

[GIF](http://gph.is/2dUaDW7) 由 [Gify](https://giphy.com/)