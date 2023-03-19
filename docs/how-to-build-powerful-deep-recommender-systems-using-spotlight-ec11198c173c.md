# 如何使用 Spotlight 构建强大的深度推荐系统？

> 原文：<https://towardsdatascience.com/how-to-build-powerful-deep-recommender-systems-using-spotlight-ec11198c173c?source=collection_archive---------25----------------------->

## 利用深度学习构建电影推荐系统。

![](img/c09b2a936dc1db788e0d9f1ab7aae0ad.png)

照片由 [Unsplash](https://unsplash.com/s/photos/spotlight?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄

在我的上一篇文章中，我展示了如何使用 Surprise 构建基于矩阵分解等技术的浅层推荐系统。

</how-you-can-build-simple-recommender-systems-with-surprise-b0d32a8e4802>  

但是如果你想建立一个推荐系统，使用比简单的矩阵分解更复杂的技术呢？如果你想用深度学习来构建推荐系统呢？如果你想利用用户的观看历史来预测他们将要观看的下一部电影呢？

这就是使用 PyTorch 创建深度学习推荐系统的 Python 库 Spotlight 发挥作用的地方。Spotlight 的界面类似于 Surprise，但支持基于矩阵分解和顺序深度学习模型的推荐系统。

**在本文中，我将展示如何使用 Spotlight 构建深度推荐系统，通过矩阵分解和序列模型进行电影推荐。**

# 装置

官方文档页面提供了用 Conda 安装 Spotlight 的命令，但我建议使用以下命令直接从 Git 安装库，尤其是如果您有 Python 3.7。

```
git clone [https://github.com/maciejkula/spotlight.git](https://github.com/maciejkula/spotlight.git)
cd spotlight
python setup.py build
python setup.py install
```

# 构建电影推荐系统

在这个实际的例子中，我决定使用 Kaggle 上的电影数据集中的数据。该数据集包含来自 270，000 个用户的 45，000 部电影的 2，600 万个评级的文件。出于本例的目的，我使用了数据集中的 **ratings_small** 文件，该文件包含 700 个用户对 9000 部电影的 100，000 个评级的子集。完整的数据集需要更长的时间来训练，但如果您有一台配备强大 GPU 的机器，您可以尝试使用它。你可以在 [GitHub](https://github.com/AmolMavuduru/SpotlightExamples) 上找到这个实际例子的[完整代码](https://github.com/AmolMavuduru/SpotlightExamples)。

## 导入基本库

在下面的代码中，我简单地导入了我用于大多数数据科学项目的基本库。

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```

## 读取数据

在下面的代码中，我读取了三个不同的数据文件:

*   **ratings_small.csv** —包含不同用户和电影的分级数据。
*   **movies_metadata.csv** —包含数据集中所有 45，000 部电影的元数据。
*   **links.csv** —包含在将该数据与电影元数据结合时可用于查找每部电影的 id。

```
ratings_data = pd.read_csv('./data/ratings_small.csv.zip')
metadata = pd.read_csv('./data/movies_metadata.csv.zip')
links_data = pd.read_csv('./data/links.csv')
```

使用 Pandas 中的信息功能为每个数据框获取的列如下所示。

**收视率 _ 数据**

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 100004 entries, 0 to 100003
Data columns (total 4 columns):
 #   Column     Non-Null Count   Dtype  
---  ------     --------------   -----  
 0   userId     100004 non-null  int64  
 1   movieId    100004 non-null  int64  
 2   rating     100004 non-null  float64
 3   timestamp  100004 non-null  int64  
dtypes: float64(1), int64(3)
memory usage: 3.1 MB
```

**元数据**

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 45466 entries, 0 to 45465
Data columns (total 24 columns):
 #   Column                 Non-Null Count  Dtype  
---  ------                 --------------  -----  
 0   adult                  45466 non-null  object 
 1   belongs_to_collection  4494 non-null   object 
 2   budget                 45466 non-null  object 
 3   genres                 45466 non-null  object 
 4   homepage               7782 non-null   object 
 5   id                     45466 non-null  object 
 6   imdb_id                45449 non-null  object 
 7   original_language      45455 non-null  object 
 8   original_title         45466 non-null  object 
 9   overview               44512 non-null  object 
 10  popularity             45461 non-null  object 
 11  poster_path            45080 non-null  object 
 12  production_companies   45463 non-null  object 
 13  production_countries   45463 non-null  object 
 14  release_date           45379 non-null  object 
 15  revenue                45460 non-null  float64
 16  runtime                45203 non-null  float64
 17  spoken_languages       45460 non-null  object 
 18  status                 45379 non-null  object 
 19  tagline                20412 non-null  object 
 20  title                  45460 non-null  object 
 21  video                  45460 non-null  object 
 22  vote_average           45460 non-null  float64
 23  vote_count             45460 non-null  float64
dtypes: float64(4), object(20)
memory usage: 8.3+ MB
```

**链接 _ 数据**

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 45843 entries, 0 to 45842
Data columns (total 3 columns):
 #   Column   Non-Null Count  Dtype  
---  ------   --------------  -----  
 0   movieId  45843 non-null  int64  
 1   imdbId   45843 non-null  int64  
 2   tmdbId   45624 non-null  float64
dtypes: float64(1), int64(2)
memory usage: 1.0 MB
```

请注意以下映射和列关系:

*   **links_data** 中的 **movieId** 列映射到 **ratings_data** 中的 **movieId** 列。
*   **links_data** 中的 **imdbId** 列映射到**元数据**中的 **imdb_id** 列。

## 预处理元数据

在下一节中，我使用下面列出的步骤对数据进行了预处理。请参考我在 GitHub 上的 Jupyter 笔记本和 Kaggle 上的数据集描述，以获得澄清。

1.  从元数据数据框中移除 imdb_id 为空的行。
2.  通过应用 lambda 函数，将元数据中 imdb_id 列的每个元素转换为 int。
3.  通过分别连接 **imbd_id** 和 **imdbId** 列上的数据帧来合并元数据和 links_data。

```
metadata = metadata[metadata['imdb_id'].notna()]def remove_characters(string):

    return ''.join(filter(str.isdigit, string))metadata['imdb_id'] = metadata['imdb_id'].apply(lambda x: int(remove_characters(str(x))))full_metadata = pd.merge(metadata, links_data, left_on='imdb_id', right_on='imdbId')
```

运行上面的代码会生成一个数据帧，我们可以使用它来检索电影的元数据，只基于电影 ID。

## 创建 Spotlight 交互数据集

像 Surprise 一样，Spotlight 也有一个 dataset 对象，我们需要使用它来根据我们的数据训练模型。在下面的代码中，我通过提供以下参数创建了一个 Interactions 对象，所有这些参数都必须是 Numpy 数组:

*   **用户标识** —评级数据中的用户标识
*   **item _ id**—评分数据中的项目 id
*   **评级** —评级数据中对应的评级。
*   **时间戳(可选)** —每个用户/项目交互的时间戳。

```
from spotlight.interactions import Interactionsdataset = Interactions(user_ids=ratings_data['userId'].values,
                       item_ids=ratings_data['movieId'].values,
                       ratings=ratings_data['rating'].values,
                       timestamps=ratings_data['timestamp'].values)
```

## 训练矩阵分解模型

现在已经创建了数据集，我们可以使用 Spotlight 的 ExplicitFactorizationModel 模块来训练基于深度学习的矩阵分解模型，如下所示。

```
from spotlight.cross_validation import random_train_test_split
from spotlight.evaluation import rmse_score
from spotlight.factorization.explicit import ExplicitFactorizationModeltrain, test = random_train_test_split(dataset)model = ExplicitFactorizationModel(n_iter=10)
model.fit(train, verbose=True)rmse = rmse_score(model, test)
print('RMSE = ', rmse)
```

运行上面的代码会产生以下输出，其中包含每个时期的损失值:

```
Epoch 0: loss 4.494929069874945
Epoch 1: loss 0.8425834600011973
Epoch 2: loss 0.5420750372064997
Epoch 3: loss 0.38652444562064103
Epoch 4: loss 0.30954678428190163
Epoch 5: loss 0.26690390673145314
Epoch 6: loss 0.24580617306721325
Epoch 7: loss 0.23303465699786075
Epoch 8: loss 0.2235499506040965
Epoch 9: loss 0.2163570392770579
RMSE = 1.1101374661355057
```

## 利用因子分解模型生成电影推荐

现在我们已经训练了一个矩阵分解模型，我们可以用它来生成电影推荐。predict 方法采用单个用户 ID 或一组用户 ID，并为数据集中的每个电影项目生成预测评级或“分数”。

```
model.predict(user_ids=1)
```

predict 方法的输出是一个值数组，每个值对应于数据集中某个项目(在本例中为电影)的预测评级或得分。

```
array([0.42891726, 2.2079964 , 1.6789076 , ..., 0.24747998, 0.36188596, 1.658421  ], dtype=float32)
```

我在下面创建了一些实用函数，用于将 predict 方法的输出转换为实际的电影推荐。

我们可以调用 **recommend_movies** 函数为具有特定 ID 的用户生成电影推荐，如下所示。

```
recommend_movies(1, full_metadata, model)
```

调用此函数会产生以下输出，其中包含一个字典列表，其中包含每部推荐电影的元数据。

```
[[{'original_title': '2001: A Space Odyssey',
   'release_date': '1968-04-10',
   'genres': "[{'id': 878, 'name': 'Science Fiction'}, {'id': 9648, 'name': 'Mystery'}, {'id': 12, 'name': 'Adventure'}]"}],
 [{'original_title': 'Rocky',
   'release_date': '1976-11-21',
   'genres': "[{'id': 18, 'name': 'Drama'}]"}],
 [{'original_title': "The Young Poisoner's Handbook",
   'release_date': '1995-01-20',
   'genres': "[{'id': 35, 'name': 'Comedy'}, {'id': 80, 'name': 'Crime'}, {'id': 18, 'name': 'Drama'}]"}],
 [{'original_title': 'Thinner',
   'release_date': '1996-10-25',
   'genres': "[{'id': 27, 'name': 'Horror'}, {'id': 53, 'name': 'Thriller'}]"}],
 [{'original_title': 'Groundhog Day',
   'release_date': '1993-02-11',
   'genres': "[{'id': 10749, 'name': 'Romance'}, {'id': 14, 'name': 'Fantasy'}, {'id': 18, 'name': 'Drama'}, {'id': 35, 'name': 'Comedy'}]"}]]
```

## 训练序列模型

我们可以采用不同的方法，通过使用顺序深度学习模型来构建推荐系统。序列模型不是学习矩阵分解，而是学习使用一系列用户-项目交互来预测用户将与之交互的下一个项目。在这个例子的上下文中，顺序模型将学习使用每个用户的观看/评级历史来预测他们将观看的下一部电影。

**ImplicitSequenceModel** 允许我们通过深度学习来训练序列模型，如下所示。注意，我必须使用 **to_sequence** 方法将训练和测试数据集转换成序列数据集。

```
from spotlight.sequence.implicit import ImplicitSequenceModel
from spotlight.cross_validation import user_based_train_test_splittrain, test = user_based_train_test_split(dataset)train = train.to_sequence()
test = test.to_sequence()model = ImplicitSequenceModel(n_iter=10,
                              representation='cnn',
                              loss='bpr')model.fit(train)
```

我们可以使用**平均倒数等级(MRR)** 度量在测试数据上评估这个模型。倒数排名度量获取项目的排名，并输出正确项目的排名的倒数。例如，假设我们的推荐系统为用户产生以下电影排名:

1.  **蝙蝠侠前传**
2.  **蜘蛛侠**
3.  **超人**

让我们假设用户实际上决定接下来观看蜘蛛侠。在这种情况下，蜘蛛侠在上面的列表中排名第二，这意味着倒数排名是 1/2。MRR 对测试集中所有预测的倒数排名进行平均。我们可以使用 **sequence_mrr_score** 函数计算 Spotlight 中模型的 MRR，如下所示。

```
from spotlight.evaluation import sequence_mrr_scoresequence_mrr_score(model, test)
```

结果是测试集中每部电影的 MRR 分数列表。

```
[0.00100402 0.00277778 0.00194932 ... 0.00110619 0.0005305  0.00471698]
```

## 用序列模型生成电影推荐

序列模型也有一个 predict 方法，它允许我们生成预测，但是这个方法不是使用用户 ID，而是接受一个项目序列，并预测一般用户可能选择的下一个项目。

```
model.predict(sequences=np.array([1, 2, 3, 4, 5]))
```

运行上面的函数会产生一个 Numpy 数组，其中包含测试集中每部电影的分数。较高的分数对应于用户接下来观看的较高概率。

```
array([ 0\.      , 16.237215, 11.529311, ..., -2.713985, -2.403066,
       -3.747315], dtype=float32)
```

和前面的矩阵分解模型的例子一样，我在下面创建了更多的效用函数，从序列模型生成的预测中生成电影推荐。

**推荐 _ 下一部 _ 电影**功能使用电影名称列表来推荐用户接下来可能观看的热门电影。 **n_movies** 参数指定要返回的电影数量。考虑下面的示例代码。

```
movies = ['Shallow Grave', 'Twilight', 'Star Wars', 'Harry Potter']
recommend_next_movies(movies, full_metadata, model, n_movies=5)
```

序列模型为已经观看了《浅坟》、《暮光之城》、《星球大战》和《哈利·波特》的用户产生以下电影推荐:

```
[[{'original_title': 'Azúcar amarga',
   'release_date': '1996-02-10',
   'genres': "[{'id': 18, 'name': 'Drama'}, {'id': 10749, 'name': 'Romance'}]"}],
 [{'original_title': 'The American President',
   'release_date': '1995-11-17',
   'genres': "[{'id': 35, 'name': 'Comedy'}, {'id': 18, 'name': 'Drama'}, {'id': 10749, 'name': 'Romance'}]"}],
 [{'original_title': 'Jaws 2',
   'release_date': '1978-06-16',
   'genres': "[{'id': 27, 'name': 'Horror'}, {'id': 53, 'name': 'Thriller'}]"}],
 [{'original_title': 'Robin Hood',
   'release_date': '1973-11-08',
   'genres': "[{'id': 16, 'name': 'Animation'}, {'id': 10751, 'name': 'Family'}]"}],
 [{'original_title': 'Touch of Evil',
   'release_date': '1958-04-23',
   'genres': "[{'id': 18, 'name': 'Drama'}, {'id': 53, 'name': 'Thriller'}, {'id': 80, 'name': 'Crime'}]"}]]
```

从这个例子可以看出，Spotlight 中提供的序列模型非常强大，可以为训练数据集中不存在的用户生成推荐。Spotlight 最棒的一点是，它使构建这些强大的深度推荐系统变得非常容易，而不必通过实现自己的神经网络来重新发明轮子。

# 摘要

Spotlight 是一个易于使用且功能强大的库，用于使用深度学习构建推荐系统。它支持推荐系统的矩阵分解和序列模型，这使得它非常适合于构建简单和更复杂的推荐系统。

和往常一样，你可以在 [GitHub](https://github.com/AmolMavuduru/SpotlightExamples) 上找到这篇文章的代码。如果你喜欢这篇文章，请随意查看下面我以前写的一些关于深度学习的文章。

</how-to-use-facebooks-neuralprophet-and-why-it-s-so-powerful-136652d2da8b>  </what-are-transformers-and-how-can-you-use-them-f7ccd546071a>  

# 加入我的邮件列表

你想在数据科学和机器学习方面变得更好吗？您想了解数据科学和机器学习社区的最新图书馆、开发和研究吗？

加入我的[邮件列表](https://mailchi.mp/e8dd82679724/amols-data-science-blog)，获取我的数据科学内容的更新。当你[注册](https://mailchi.mp/e8dd82679724/amols-data-science-blog)的时候，你还会得到我免费的**解决机器学习问题的逐步指南**！

# 来源

1.  名词（noun 的缩写）拥抱，惊喜:推荐系统的 Python 库，(2020)，开源软件杂志。
2.  R.Banik，[电影数据集](https://www.kaggle.com/rounakbanik/the-movies-dataset)，(2017)，Kaggle。
3.  米（meter 的缩写））库拉，[聚光灯](https://github.com/maciejkula/spotlight)，(2017)，GitHub。