# 创建电影分级模型第 1 部分:数据收集！

> 原文：<https://towardsdatascience.com/creating-a-movie-rating-model-part-1-data-gathering-944bee6167c0?source=collection_archive---------14----------------------->

![](img/5bc682a7569e19265c98e0bd0ca21917.png)

## 电影分级模型

## 通过收集我们需要的数据，开始创建电影分级预测模型的过程

你好，朋友们！欢迎来到一个新的系列，我从这篇关于创建电影分级预测模型的文章开始。在开始写这篇文章之前，我想快速提一下，我绝对没有放弃 Terraform + SageMaker 系列。我刚刚度过了一个超级忙碌的夏天，有很多事情要做，所以我暂时停止了写博客。这方面还有更多的内容，敬请关注！

我开始这个新系列的原因是一个有趣的原因。我是播客的忠实听众，我最喜欢的播客之一叫做[](https://www.podcastone.com/the-watercooler)*。这是一个轻松的播客，五个同事在一天结束后聚在一起谈论，嗯，几乎是天底下的一切！他们也有一些重复出现的片段，包括主持人之一卡兰·比恩(Caelan Biehn)将为不同的电影提供评级。他的评级实际上由两个分数组成:一个是简单的“赞成/不赞成”，另一个是一个介于 0 和 10 之间的十进制浮动分数，他称之为“Biehn 标度”。(顺便说一下，卡兰是演员[迈克尔·比恩](https://en.wikipedia.org/wiki/Michael_Biehn)的儿子，他曾主演过《终结者**外星人*等电影。)**

***水冷器*这些家伙真的很吸引他们的观众，所以有一次我在他们的脸书小组里开玩笑说，我应该围绕卡兰的电影评级创建一个电影评级预测模型。他们都觉得很好玩，我也觉得会很好玩，所以我们就来了！**

**在深入探讨这篇文章的主要目的——数据收集——之前，让我们在继续构建我们的模型的过程中，为我们将在整个系列中做的事情打下基础。和往常一样，非常欢迎您关注[我的 GitHub 库](https://github.com/dkhundley/movie-ratings-model)中的代码。**

# **项目范围和流程**

**![](img/a9a0a4355c2cbb9d617f3bb818decd59.png)**

**作者创作的流动艺术品**

**如上所述，Caelan 为每个电影评级提供了两个分数:一个二元的“同意/不同意”和一个 0 到 10 的浮动分数。当然，这意味着我们需要创建两个预测模型:一个处理二元分类，另一个是更基于回归的模型。我有意不坚持一个特定的算法，因为我们将使用未来的帖子来验证一些算法，看看哪一个执行得最好。**

**为此，上面的流程图显示了我们将如何在整个系列中前进。在第一阶段(也就是本文)，我们需要从各种来源收集数据来支持我们的模型。一旦我们收集了我们的数据，我们将需要执行所有适当的特征工程，以准备我们的数据通过我们各自的算法。接下来，我们将尝试几种不同的算法以及超参数调整，看看哪种算法的性能最好。一旦我们决定了要利用哪些算法，我们将创建一个完整的模型培训管道来实际创建预测模型。最后，我们将把训练好的模型包装在一个漂亮的推理 API 中！**

**在继续之前，有一点需要注意:我不一定要在这里创建一个完美的模型。在我的日常工作中，我是一名机器学习工程师，这个角色的范围更侧重于模型部署，而不是模型创建。也就是说，我并不精通模型创建，但我希望通过这个系列在这个领域有所发展！如果你在这个过程中发现可能有一些更好的方法，请在任何博客帖子上留下评论。**

**好了，让我们进入这项工作的第一阶段:数据收集！**

# **收集电影评级**

**当然，如果我们想创建一个电影分级模型，我们必须有电影分级来训练我们的监督模型！据我所知，没有人在任何地方保存卡兰的电影评级，所以我不得不做一些许多人可能会害怕的事情:我必须自己收集它们。这意味着要翻遍《水冷器》的播客的完整目录，并听完它们，记下卡兰给出的任何评级。为了支持这一点，我创建了一个[谷歌工作表电子表格](https://docs.google.com/spreadsheets/d/1-8tdDUtm0iBrCdCRAsYCw2KOimecrHcmsnL-aqG-l0E/edit#gid=0)，在那里我通过我的 iPhone 收集和输入数据。在这篇文章发表的时候，我只看了全部播客的 3/4，但是我认为我们有足够的数据来开始这个系列。**

**(补充说明:除了电影，Caelan 还对其他东西进行评级，所以你会在电子表格中找到比电影更多的东西。我捕捉这些其他的东西只是为了好玩，但它们会被过滤掉，只关注电影。此外，*水冷器*可以是 NSFW，也可以是一些非电影评级，所以……如果你看完整的电子表格，请注意这一点。😂)**

**好消息是，我发现了一种用 Python 编程从 Google 工作表中获取数据的方法，只要你知道`sheet_id`号是什么。在为我自己的 Google Sheet 找到特定的 ID 后，我可以使用下面的脚本在任何需要的时候获取更新的电影评论:**

```
**# Defining the ID of the Google Sheet with the movie ratings
sheet_id = '1-8tdDUtm0iBrCdCRAsYCw2KOimecrHcmsnL-aqG-l0E'# Creating a small function to load the data sheet by ID and sheet name
def load_google_sheet(sheet_id, sheet_name):
    url = f'[https://docs.google.com/spreadsheets/d/{sheet_id}/gviz/tq?tqx=out:csv&sheet={sheet_name}'](https://docs.google.com/spreadsheets/d/{sheet_id}/gviz/tq?tqx=out:csv&sheet={sheet_name}')
    df = pd.read_csv(url)
    return df# Loading all the sheets and joining them together
df_main = load_google_sheet(sheet_id, 'main')
df_patreon = load_google_sheet(sheet_id, 'patreon')
df_mnight = load_google_sheet(sheet_id, 'movie_night')
df = pd.concat([df_main, df_patreon, df_mnight], axis = 0)**
```

**这个小脚本从 Google Sheet 中的三个选项卡收集数据，并将它们全部加载到一个统一的 Pandas 数据框架中。显然，这个熊猫数据框架包含的不仅仅是电影评论。为了方便起见，谷歌表单包含一个名为“类别”的栏目，我们可以用它来过滤电影。下面是要执行的脚本:**

```
**# Keeping only the movies
df_movies = df[df['Category'] == 'Movie']**
```

**最后，我们可以去掉我们的项目不需要的列，包括“集号”、“类别”和“注释”列。做到这一点的代码非常简单:**

```
**# Removing the columns we do not need for the model
df_movies.drop(columns = ['Category', 'Episode Number', 'Notes'], inplace = True)**
```

**我们剩下的就是这个样子的东西！**

**![](img/a83f4ea816c4b0bae2233202b6256d2b.png)**

**作者截图**

**好吧！我们可以用 Pandas DataFrame `to_csv`函数保存下来，并准备继续前进。**

# **收集支持数据**

**对于那些熟悉像臭名昭著的泰坦尼克号项目这样的 Kaggle 数据集的人来说，你会知道 Kaggle 基本上给了你所有你需要的数据。我们的项目显然不是这种情况，收集数据来支持任何这样的模型都是一个挑战。数据科学家在这个职位上必须考虑的一些事情包括如下问题:**

*   **支持这种潜在模型的数据存在吗？**
*   **创建模型时，我需要收集什么样的特征？**
*   **我需要通过什么方式来整理原始形式的数据？**
*   **我需要对原始数据进行大量的特征工程吗？**
*   **原始数据真的代表了模型背后的推论吗？**
*   **数据是否在推论上产生了不公平/不道德的偏向？**

**好消息是我可以很容易地回答其中的一些问题。数据存在吗？嗯，我知道 IMDb 的存在，所以我敢肯定，一些容量的电影数据存在！我也不必担心创建一个不道德的模型，因为这种努力的本质不会给未来带来糟糕的结果。如果你想知道不道德的努力会是什么样子，我总是用创建一个预测模型的例子来预测基于 20 世纪数据的最佳 CEO 候选人。大多数 20 世纪的首席执行官都是中年白人男性，这在很大程度上是因为压制女性和少数族裔的制度。在 20 世纪首席执行官信息的基础上创建一个模型，会使所有的推论偏向于选择一个中年白人男性，这显然是不好的，也是不道德的。**

**这里的挑战是从不同的来源获取数据来支持我们的模型。说实话，我一开始不知道我应该寻找的所有功能。我有一种直觉，将卡兰的分数与其他评论家的分数进行比较可能是一个好主意，因此这可能具体表现为收集每部电影的*烂番茄*分数。但是到了最后，我不知道我会找到什么。此外，我如何确保来自一个源的电影数据与另一个匹配？例如，我如何确保一个给我提供*复仇者联盟 4：终局之战*数据的来源不会意外地从另一个来源给我提供*复仇者联盟 3：无限战争*的数据？**

**幸运的是，由于谷歌的力量，数据收集没有我想象的那么困难！现在让我们来分析我收集电影数据的三个来源，以及第一个来源是如何帮助保持一个来源到另一个来源的一致性的。**

**(在继续之前，请注意，从现在开始，这篇文章中讨论的所有内容都被记录在这个独特的 Jupyter 笔记本中。在我们继续进行的过程中，请随意参考它！)**

## **数据源#1:电影数据库(TMDb)**

**在谷歌上搜索可能对我们的项目有前景的资源，似乎不断出现的首选是这个叫做 [**电影数据库**](https://www.themoviedb.org/?language=en-US) 的资源，通常缩写为 **TMDb** 。虽然它有一个非常好的用户界面，可以通过网络浏览，他们也提供了一个同样伟大的 API 进行交互。唯一的问题是，他们试图限制有多少流量被他们的 API 命中，所以他们要求用户通过他们的网站注册以获得 API 密钥。(注意:请不要问我要我的 TMDb API 密钥。我想确保我可以留在自由层，只有当我为自己保留我的 API 密匙时才有可能。)**

**如果你[按照这个链接](https://developers.themoviedb.org/3/getting-started/introduction)后面的说明去做，注册 TMDb 是非常简单的。注册完成后，您应该会看到这样一个屏幕，其中显示了您的特定 API 密钥。**

**![](img/3157379bda02f935ef0ac9da877613a7.png)**

**作者截图**

**如前所述，这个 API 键是敏感的，所以如果你在自己的代码中使用它，我建议你在把代码上传到 GitHub 的情况下，想办法模糊它。有很多方法可以做到这一点，但为了简单起见，我将我的密钥加载到一个名为`keys.yml`的文件中，然后使用一个`.gitignore`文件来确保该文件不会上传到 GitHub。如果你不知道什么是`.gitignore`文件，它是一个你可以描述任何你不想上传到 GitHub 的东西的地方，无论它是一个单一的特定文件，一个特定的目录，还是任何以特定扩展名结尾的东西。这个`.gitignore`文件本身不存储任何敏感的东西，所以非常欢迎你复制我在这个项目中使用的[。](https://github.com/dkhundley/movie-ratings-model/blob/main/.gitignore)**

**那么，如何创建并使用这个`keys.yml`文件呢？让我创建一个不同的`test.yml`文件，向您展示它如何处理不敏感的信息。首先，让我们创建那个`test.yml`文件，并像这样填充它:**

```
**test_keys:
    key1: s3cretz
    key2: p@ssw0rd!**
```

**现在我们可以使用这个小脚本导入 Python 的默认`yaml`库并解析`test.yml`文件。下面你会看到一个截图，我特意打印了包含“敏感”信息本身的变量，但是请注意，你**并不想用你实际的 API 键**来打印。**

```
**# Importing the default Python YAML library
import yaml# Loading the test keys from the separate, secret YAML file
with open('test.yml', 'r') as f:
    keys_yaml = yaml.safe_load(f)

# Extracting the test keys from the loaded YAML
key1 = keys_yaml['test_keys']['key1']
key2 = keys_yaml['test_keys']['key2']**
```

**![](img/c0643317c31f94b77ae840fe678528f9.png)**

**作者截图**

**好的，这就是我们如何保证 API 密匙的安全，但是我们实际上如何使用它呢？幸运的是，我们可以使用几个 Python 包装器来轻松地使用 Python 与 TMDb 进行交互，我选择使用的是一个名为 **tmdbv3api** 的包装器。tmdbv3api 的文档可以在这个链接中找到[，我们将通过这个 Python 包装器遍历我所做的与 TMDb 的 api 交互的基础。](https://github.com/AnthonyBloomer/tmdbv3api)**

**用`pip3 install tmdbv3api`安装后，我们需要做的第一件事是实例化一些 TMDb Python 对象，这将允许我们获得我们需要的数据。让我先向您展示代码，我将解释这些对象的作用:**

```
**# Instantiating the TMDb objects and setting the API key
tmdb = tmdbv3api.TMDb()
tmdb_search = tmdbv3api.Search()
tmdb_movies = tmdbv3api.Movie()
tmdb.api_key = tmdb_key**
```

**第一个`tmdb`对象是一种“主”对象，它使用我们提供的 API 键实例化我们的 API 连接。`tmdb_search`对象是我们用来将电影名称的字符串文本传入 API 以获得关于电影的一些基本信息，尤其是电影的唯一标识符。然后，我们可以将该电影的唯一标识符传递给`tmdb_movies`对象，这将为我们提供一个数据宝库。**

**让我们通过搜索我最喜欢的电影之一来展示这一点。下面是我们如何使用`tmdb_search`来使用字符串搜索电影:**

```
**# Performing a preliminary search for the movie "The Matrix"
matrix_search_results = tmdb_search.movies({'query': 'The Matrix'})**
```

**如果你查看这些搜索结果，你会发现是一系列搜索结果，每一个都包含每部电影的有限细节。现在，就像任何搜索引擎一样，TMDb 将尝试返回与您的字符串最相关的搜索。作为一个例子，让我们看看当我遍历这些结果并从每个搜索结果条目中取出电影的标题时会发生什么:**

**![](img/9558f6f0a7407fbbae83c16b5218ff6f.png)**

**作者截图**

**正如你所看到的，它首先返回了原始的黑客帝国电影，然后是其他后续的黑客帝国电影，然后是其他一些标题中有“黑客帝国”的随机电影。**

**在这里，我们将不得不接受我们的搜索结果的风险…你会在下面看到，我基本上要创建一个 Python `for`循环来迭代通过卡兰评级的每部电影。不幸的是，我无法切实验证我搜索的每部电影在第一次尝试时都会出现正确的一部。例如，我知道 Caelan 评论了旧的恐怖电影*宠物语义*的更新版本，没有看，我不知道 TMDb 是否会给我更新的版本或旧的经典。这只是我们不得不接受的风险。**

**单独做这个搜索并不能解决 TMDb 的问题。虽然这些初步结果确实返回了一些关于这部电影的信息，但实际上还有另一部分 TMDb 将提供我们想要的所有细节。在我们获得这些额外的细节之前，让我们看看原始搜索结果中的关键字，这样我们就可以显示我们将从下一步中提取多少更多的细节。**

**![](img/1e423a1bb7ba85f4d1ca024a55d6e8b4.png)**

**作者截图**

**为了进行更详细的搜索，我们需要将 TMDb ID 传递给`tmdb_movies`对象。我们可以通过查看`matrix_search_results[0]['id']`从原始搜索结果中获得 TMDb ID。这就是我们如何使用 TMDb ID 来提取关于电影的更多细节:**

```
**# Getting details about "The Matrix" by passing in the TMDb ID from the original search results
matrix_details = dict(tmdb_movies.details(matrix_search_results[0]['id']))**
```

**现在，我们来看看与初步结果相比，这个更详细的结果列表的新关键字:**

**![](img/f3d68e1ba6c6aee4edb136b48548fffb.png)**

**作者截图**

**正如你所看到的，我们从这次搜索中获得了比以前更多的细节！现在，为了简洁起见，我不打算在这里花太多时间向您展示我将保留哪些特性。如果你想知道的话，我将创建一个数据字典作为我的 GitHub 的自述文件的一部分。但在进入下一个来源之前，我想指出一件事。还记得我说过我关心的是从一个数据源到另一个数据源保持电影结果的一致性吗？好了，朋友们，我有一些好消息！作为 TMDb 搜索结果的一部分，它们还包括`imdb_id`。这很好，因为像这样的唯一标识符肯定有助于确定正确的 IMDb 搜索结果。说到 IMDb…**

## **数据来源#2: IMDb**

**所以您可能会奇怪，为什么我选择 TMDb 作为主要数据源，而不是 IMDb 呢？答案是因为虽然 IMDb 有官方 API，但我无法使用它。他们似乎需要某种特殊的商业案例来请求访问，老实说，我不想经历这样的麻烦。幸运的是，我确实找到了一个适合我们的替代品: **IMDbPy** 。IMDbPy ( [链接到 docs](https://github.com/alberanid/imdbpy) )是一个 Python 库，有人创建它来与来自 IMDb 的数据交互。现在，我真的不知道作者是如何获得这些数据的。这可能是因为他们可以访问官方的 IMDb API，或者他们正在做一些花哨的屏幕截图。无论是哪种情况，这个 Python 库都提供了我们需要的数据。**

**通过从 TMDb 搜索结果中提取 IMDb ID，我们可以使用它作为 IMDb 搜索的基础。不过有一点需要注意:TMDb 对 IMDb ID 的格式化有点奇怪。具体来说，就是将字符“tt”附加到每个 ID 的开头。不过，不要担心，因为我们可以简单地使用 Python 字符串迭代器，在将这些初始字符放入 IMDb 搜索之前，将其剥离。**

**在通过运行`pip3 install imdbpy`安装 IMDbPy 之后，我们准备通过运行下面的代码实例化我们的`imdb_search`对象:**

```
**# Importing the IMDb Python library
from imdb import IMDb# Instantiating the IMDb search object
imdb_search = IMDb()**
```

**现在，我们可以通过弹出 IMDb ID 来执行搜索，如下所示:**

```
**# Obtaining IMDb search results about the movie "The Matrix"
matrix_imdb_results = dict(imdb_search.get_movie(matrix_imdb_id))**
```

**唯一不幸的缺点是，虽然 IMDb 返回了大量信息，但大多数信息对我们的模型没有好处。IMDb 返回的大部分信息都是关于电影的配角和工作人员的，不幸的是，这些信息在预测模型中并不具有良好的特性。事实上，我们能从这些结果中提取的信息只有`imdb_rating`和`imdb_votes`。我要说这有点令人失望，但既然 TMDb 在第一遍中为我们返回了这么多信息，我也不太失望。**

**完成 IMDb 后，让我们继续第三个也是最后一个数据源。**

## **数据源#3:开放电影数据库(OMDb)**

**从这个项目一开始，我就知道我绝对希望作为支持这些模型的特性的一件事是来自烂番茄的各自的评论家分数和观众分数，到目前为止，我们还没有从 TMDb 或 IMDb 获得这些数据。**

**幸运的是，[开放电影数据库(OMDb)](http://www.omdbapi.com) 在这方面显示了一些希望！实际使用 OMDb 将与我们使用 TMDb 非常相似，除了我们可以使用 IMDb ID 在 TMDb 中搜索电影。与 TMDb 一样，OMDb 要求您注册自己的 API 密钥。你可以通过在这个表格上提交你的电子邮件[来做到这一点](http://www.omdbapi.com/apikey.aspx)，他们会通过电子邮件把 API 密匙发给你。正如您在这个表单中注意到的，您每天只能使用 1000 次对这个 API 的调用，这对我们的目的来说完全没问题。(同样，如果您开始与世界共享这个 API 密匙，您将会遇到问题，所以一定要把这个 API 密匙留给自己！)**

**因为这篇博文已经开始变得很长了，所以我将粘贴我的脚本，它将获得烂番茄评论家评分和 Metacritic metascore。该脚本的行为与 TMDb 非常相似:**

```
**# Instantiating the OMDb client
omdb_client = OMDBClient(apikey = omdb_key)# Iterating through all the movies to extract the proper OMDb information
for index, row in df_all_data.iterrows():
    # Extracting movie name from the row
    movie_name = row['movie_name']

    # Using the OMDb client to search for the movie results using the IMDb ID
    omdb_details = omdb_client.imdbid(row['imdb_id'])

    # Resetting the Rotten Tomatoes critic score variable
    rt_critic_score = None

    # Checking if the movie has any ratings populated under 'ratings'
    omdb_ratings_len = len(omdb_details['ratings'])

    if omdb_ratings_len == 0:
        print(f'{movie_name} has no Rotten Tomatoes critic score.')
    elif omdb_ratings_len >= 0:
        # Extracting out the Rotten Tomatoes score if available
        for rater in omdb_details['ratings']:
            if rater['source'] == 'Rotten Tomatoes':
                rt_critic_score = rater['value']

    # Populating Rotten Tomatoes critic score appropriately
    if rt_critic_score:
        df_all_data.loc[index, 'rt_critic_score'] = rt_critic_score
    else:
        df_all_data.loc[index, 'rt_critic_score'] = np.nan

    # Populating the Metacritic metascore appropriately
    df_all_data.loc[index, 'metascore'] = omdb_details['metascore']**
```

**总而言之，以下是我们所有电影的 18 大特色:**

**![](img/317b50d39b7c6e7dfe9da9e176294e8e.png)**

**作者截图**

**让我们继续将该数据集保存为 CSV 格式，并结束这篇文章！**

# **接下来:特征工程**

**唷，我们在这篇文章中走了很长的路！我们从 3 个不同的数据源中获得了 18 个不同的特征，我希望这将为我们在下一个特征工程部分开始生成新的特征提供一个良好的基础。不幸的是，我们无法获得我所希望的烂番茄观众评分，所以我将向正式的烂番茄 API 提交一个请求，看看我是否能获得它，因为我认为这将是一个非常有用的功能，所以如果我能获得它，我可能会回来修改这篇文章的结尾。**

**感谢大家查看这篇文章！希望您发现它有趣且信息丰富。我也开始在 YouTube 上直播这个项目的编码过程，你可以在我的链接树中找到我所有的事情。实际上，我已经为这篇特别的帖子做了现场直播，重播可以在 YouTube 上看到。再次感谢你的阅读！下一篇帖子见。🎬**