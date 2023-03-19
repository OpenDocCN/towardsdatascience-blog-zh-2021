# 使用 Vespa 从 python 构建新闻推荐应用程序

> 原文：<https://towardsdatascience.com/build-a-news-recommendation-app-from-python-with-vespa-7c5b3ce079be?source=collection_archive---------26----------------------->

## 第 1 部分—新闻搜索功能

我们将在 Vespa 中构建一个新闻推荐应用程序，而不会离开 python 环境。在本系列的第一部分中，我们希望开发一个具有基本搜索功能的应用程序。未来的帖子将增加基于嵌入和其他 ML 模型的推荐功能。

![](img/0cf8c9cafdb6dd84654f934d9744df3a.png)

照片由[菲利普·米舍夫斯基](https://unsplash.com/@filipthedesigner?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

这个系列是 Vespa 的[新闻搜索推荐教程](https://docs.vespa.ai/en/tutorials/news-2-basic-feeding-and-query.html)的简化版。我们还将使用[微软新闻数据集(MIND)](https://msnews.github.io/) 的演示版本，这样任何人都可以在他们的笔记本电脑上跟随。

## 资料组

最初的 Vespa 新闻搜索教程提供了一个脚本来下载、解析和转换 MIND 数据集为 Vespa 格式。为了方便您，我们提供了本教程所需的最终解析数据供下载:

```
{'abstract': "Shop the notebooks, jackets, and more that the royals can't live without.",
 'title': 'The Brands Queen Elizabeth, Prince Charles, and Prince Philip Swear By',
 'subcategory': 'lifestyleroyals',
 'news_id': 'N3112',
 'category': 'lifestyle',
 'url': 'https://www.msn.com/en-us/lifestyle/lifestyleroyals/the-brands-queen-elizabeth,-prince-charles,-and-prince-philip-swear-by/ss-AAGH0ET?ocid=chopendata',
 'date': 20191103,
 'clicks': 0,
 'impressions': 0}
```

这里使用的最终解析数据是一个列表，其中每个元素都是一个字典，包含一篇新闻文章的相关字段，比如`title`和`category`。我们也有文章收到的`impressions`和`clicks`的数量信息。mind 数据集的演示版包含了 28.603 篇新闻文章。

```
28603
```

## 安装 pyvespa

## 创建搜索应用程序

创建应用程序包。`app_package`将保存与您的应用规范相关的所有相关数据。

向架构中添加字段。下面是对下面使用的非显而易见的参数的简短描述:

*   索引参数:为字段配置[索引管道](https://docs.vespa.ai/en/reference/advanced-indexing-language.html)，它定义了 Vespa 在索引期间如何处理输入。
*   “索引”:为此字段创建搜索索引。
*   “summary”:让该字段成为结果集中[文档摘要](https://docs.vespa.ai/en/document-summaries.html)的一部分。
*   “属性”:将该字段作为[属性](https://docs.vespa.ai/en/attributes.html)存储在内存中——用于[排序](https://docs.vespa.ai/en/reference/sorting.html)、[查询](https://docs.vespa.ai/en/query-api.html)和[分组](https://docs.vespa.ai/en/grouping.html)。
*   索引参数:[配置](https://docs.vespa.ai/en/reference/schema-reference.html#index)Vespa 应该如何创建搜索索引。
*   “enable-bm25”:设置一个兼容 [bm25 排名](https://docs.vespa.ai/en/reference/rank-features.html#bm25)的索引进行文本搜索。
*   属性参数:[配置](https://docs.vespa.ai/en/attributes.html)Vespa 应该如何对待一个属性字段。
*   “快速搜索”:为属性字段建立索引。默认情况下，不会为属性生成索引，搜索这些属性默认为线性扫描。

向架构中添加字段集。字段集允许我们轻松地搜索多个字段。在这种情况下，搜索`default`字段集等同于搜索`title`和`abstract`。

我们有足够的资源来部署应用程序的第一个版本。在本教程的后面，我们将把一篇文章的受欢迎程度包括在相关性分数中，用于对匹配我们查询的新闻进行排名。

## 在 Docker 上部署应用程序

如果您的机器上安装了 Docker，您可以在本地 Docker 容器中部署`app_package`:

```
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for application status.
Waiting for application status.
Finished deployment.
```

`vespa_docker`将解析`app_package`并将所有必要的 Vespa 配置文件写入`disk_folder`。然后它将创建 docker 容器，并使用 Vespa 配置文件来部署 Vespa 应用程序。然后，我们可以使用`app`实例与部署的应用程序进行交互，比如用于提要和查询。如果你想知道更多幕后发生的事情，我们建议你浏览[Docker 入门教程](https://docs.vespa.ai/en/tutorials/news-1-getting-started.html)。

## 向应用程序提供数据

我们可以使用`feed_data_point`方法。我们需要具体说明:

*   `data_id`:识别数据点的唯一 id
*   `fields`:字典中的键与我们的应用程序包模式中定义的字段名相匹配。
*   `schema`:我们要向其馈送数据的模式的名称。当我们创建应用程序包时，我们默认创建了一个与应用程序名同名的模式，在我们的例子中是`news`。

## 查询应用程序

我们可以使用 [Vespa 查询 API](https://docs.vespa.ai/en/query-api.html) 到`app.query`来释放 Vespa 所能提供的全部查询灵活性。

## 使用关键字搜索索引字段

从文档中选择`default`(标题或摘要)包含关键字“音乐”的所有字段。

```
{'id': 'index:news_content/0/5f1b30d14d4a15050dae9f7f',
 'relevance': 0.25641557752127125,
 'source': 'news_content'}
```

选择`title`和`abstract`，其中`title`包含‘音乐’，`default`包含‘节日’。

```
{'id': 'index:news_content/0/988f76793a855e48b16dc5d3',
 'relevance': 0.19587240022210403,
 'source': 'news_content',
 'fields': {'title': "At Least 3 Injured In Stampede At Travis Scott's Astroworld Music Festival",
  'abstract': "A stampede Saturday outside rapper Travis Scott's Astroworld musical festival in Houston, left three people injured. Minutes before the gates were scheduled to open at noon, fans began climbing over metal barricades and surged toward the entrance, according to local news reports."}}
```

## 按文档类型搜索

选择文档类型等于`news`的所有文档的标题。我们的应用程序只有一种文档类型，所以下面的查询检索我们所有的文档。

```
{'id': 'index:news_content/0/698f73a87a936f1c773f2161',
 'relevance': 0.0,
 'source': 'news_content',
 'fields': {'title': 'The Brands Queen Elizabeth, Prince Charles, and Prince Philip Swear By'}}
```

## 搜索属性字段，如日期

因为`date`没有用`attribute=["fast-search"]`指定，所以没有为它建立索引。因此，搜索它相当于对字段的值进行线性扫描。

```
{'id': 'index:news_content/0/debbdfe653c6d11f71cc2353',
 'relevance': 0.0017429193899782135,
 'source': 'news_content',
 'fields': {'title': 'These Cranberry Sauce Recipes Are Perfect for Thanksgiving Dinner',
  'date': 20191110}}
```

由于`default`字段集是由索引字段组成的，Vespa 将首先过滤所有在`title`或`abstract`中包含关键字“天气”的文档，然后在`date`字段中扫描“20191110”。

```
{'id': 'index:news_content/0/bb88325ae94d888c46538d0b',
 'relevance': 0.27025156546141466,
 'source': 'news_content',
 'fields': {'title': 'Weather forecast in St. Louis',
  'abstract': "What's the weather today? What's the weather for the week? Here's your forecast.",
  'date': 20191110}}
```

我们还可以执行范围搜索:

```
{'id': 'index:news_content/0/c41a873213fdcffbb74987c0',
 'relevance': 0.0017429193899782135,
 'source': 'news_content',
 'fields': {'date': 20191109}}
```

## 整理

默认情况下，Vespa 会按相关性分数降序对点击进行排序。相关性分数是由 [nativeRank](https://docs.vespa.ai/en/nativerank.html) 给出的，除非另有说明，我们将在本文后面做些说明。

```
[{'id': 'index:news_content/0/5f1b30d14d4a15050dae9f7f',
  'relevance': 0.25641557752127125,
  'source': 'news_content',
  'fields': {'title': 'Music is hot in Nashville this week',
   'date': 20191101}},
 {'id': 'index:news_content/0/6a031d5eff95264c54daf56d',
  'relevance': 0.23351089409559303,
  'source': 'news_content',
  'fields': {'title': 'Apple Music Replay highlights your favorite tunes of the year',
   'date': 20191105}}]
```

然而，我们可以使用关键字`order`通过给定的字段显式排序。

```
[{'id': 'index:news_content/0/d0d7e1c080f0faf5989046d8',
  'relevance': 0.0,
  'source': 'news_content',
  'fields': {'title': "Elton John's second farewell tour stop in Cleveland shows why he's still standing after all these years",
   'date': 20191031}},
 {'id': 'index:news_content/0/abf7f6f46ff2a96862075155',
  'relevance': 0.0,
  'source': 'news_content',
  'fields': {'title': 'The best hair metal bands', 'date': 20191101}}]
```

`order`默认按升序排序，我们可以用`desc`关键字覆盖:

```
[{'id': 'index:news_content/0/934a8d976ff8694772009362',
  'relevance': 0.0,
  'source': 'news_content',
  'fields': {'title': 'Korg Minilogue XD update adds key triggers for synth sequences',
   'date': 20191113}},
 {'id': 'index:news_content/0/4feca287fdfa1d027f61e7bf',
  'relevance': 0.0,
  'source': 'news_content',
  'fields': {'title': 'Tom Draper, Black Music Industry Pioneer, Dies at 79',
   'date': 20191113}}]
```

## 分组

我们可以使用 Vespa 的[分组](https://docs.vespa.ai/en/grouping.html)功能来计算文档数量最多的三个新闻类别:

*   有 9115 篇文章的新闻
*   体育 6765 篇
*   金融 1886 篇

```
{'id': 'group:root:0',
 'relevance': 1.0,
 'continuation': {'this': ''},
 'children': [{'id': 'grouplist:category',
   'relevance': 1.0,
   'label': 'category',
   'continuation': {'next': 'BGAAABEBGBC'},
   'children': [{'id': 'group:string:news',
     'relevance': 1.0,
     'value': 'news',
     'fields': {'count()': 9115}},
    {'id': 'group:string:sports',
     'relevance': 0.6666666666666666,
     'value': 'sports',
     'fields': {'count()': 6765}},
    {'id': 'group:string:finance',
     'relevance': 0.3333333333333333,
     'value': 'finance',
     'fields': {'count()': 1886}}]}]}
```

## 使用新闻流行度信号进行排名

默认情况下，Vespa 使用 [nativeRank](https://docs.vespa.ai/en/nativerank.html) 来计算相关性分数。我们将创建一个新的等级简档，它在我们的相关性分数计算中包括一个流行度信号。

我们新的等级档案将被命名为`popularity`。以下是对上述内容的细分:

*   inherits="default "

这会将 Vespa 配置为创建一个名为 popularity 的新等级配置文件，它继承了所有默认的等级配置文件属性；只有显式定义或覆盖的属性才会与默认等级配置文件的属性不同。

*   功能流行度

这就建立了一个可以从其他表达式中调用的函数。该函数计算点击数除以印象数，以表示受欢迎程度。然而，这并不是最好的计算方法，因为即使不确定性很高，一篇印象数少的文章也可以在这个值上得分很高。但这是一个开始:)

*   第一期的

Vespa 中的相关性计算分为两个阶段。第一阶段中完成的计算是对匹配查询的每个文档执行的。相比之下，第二阶段的计算仅在由第一阶段中完成的计算所确定的前 n 个文档上完成。我们现在只使用第一阶段。

*   表达式:nativeRank + 10 *人气

该表达式用于对文档进行排序。在这里，默认的排名表达式—默认字段集的 nativeRank 被包含进来以使查询相关，而第二个术语调用 popularity 函数。这两个术语的加权和是每个文档的最终相关性。注意，这里的权重 10 是通过观察设定的。更好的方法是使用机器学习来学习这些值，我们将在未来的帖子中回到这个话题。

## 重新部署应用程序

因为我们已经更改了应用程序包，所以我们需要重新部署我们的应用程序:

```
Waiting for configuration server.
Waiting for application status.
Waiting for application status.
Finished deployment.
```

## 使用新的流行度信号进行查询

当重新部署完成时，我们可以通过使用`ranking`参数来对匹配的文档进行排序。

```
{'id': 'id:news:news::N5870',
 'relevance': 5.156596018746151,
 'source': 'news_content',
 'fields': {'sddocname': 'news',
  'documentid': 'id:news:news::N5870',
  'news_id': 'N5870',
  'category': 'music',
  'subcategory': 'musicnews',
  'title': 'Country music group Alabama reschedules their Indy show until next October 2020',
  'abstract': 'INDIANAPOLIS, Ind.   Fans of the highly acclaimed country music group Alabama, scheduled to play Bankers Life Fieldhouse Saturday night, will have to wait until next year to see the group. The group famous for such notable songs like "If You\'re Gonna Play in Texas", "Love In The First Degree", and "She and I", made the announcement that their 50th Anniversary Tour is being rescheduled till ...',
  'url': 'https://www.msn.com/en-us/music/musicnews/country-music-group-alabama-reschedules-their-indy-show-until-next-october-2020/ar-BBWB0d7?ocid=chopendata',
  'date': 20191108,
  'clicks': 1,
  'impressions': 2}}
```

# 结论和未来工作

这篇文章提供了使用 Vespa 从 python 构建新闻搜索应用程序的一步一步的方法。我们还展示了如何部署、馈送和查询应用程序，重点介绍了排序和分组等查询功能。未来的帖子将添加用户信息，以展示我们如何将这个搜索应用程序变成由 ML 模型支持的推荐应用程序。