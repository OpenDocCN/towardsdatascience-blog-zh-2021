# 使用 Vespa 从 Python 构建新闻推荐应用程序

> 原文：<https://towardsdatascience.com/build-a-news-recommendation-app-from-python-with-vespa-f59c0a08c9a3?source=collection_archive---------30----------------------->

## 第 2 部分—从新闻搜索到嵌入的新闻推荐

在这一部分中，我们将开始使用本教程中创建的嵌入，将[我们的应用程序](/build-a-news-recommendation-app-from-python-with-vespa-7c5b3ce079be)从新闻搜索转换为新闻推荐。嵌入向量将代表每个用户和新闻文章。我们将提供用于下载的嵌入，以便更容易理解这篇文章。当用户来时，我们检索他的嵌入，并通过近似最近邻(ANN)搜索使用它来检索最近的新闻文章。我们还表明，Vespa 可以联合应用一般过滤和人工神经网络搜索，而不像市场上的竞争对手。

![](img/80a954f9c99c7829d5c87b774008f828.png)

马特·波波维奇在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我们假设你已经阅读了新闻搜索教程。因此，您应该有一个保存新闻搜索应用程序定义的`app_package`变量和一个名为`news`的 Docker 容器来运行一个搜索应用程序，该应用程序从 MIND 数据集的演示版本中获取新闻文章。

## 添加用户模式

我们需要添加另一种文档类型来表示用户。我们设置模式来搜索一个`user_id`，并检索用户的嵌入向量。

我们通过指定`fast-search`属性为属性字段`user_id`构建一个索引。请记住，属性字段保存在内存中，默认情况下不进行索引。

嵌入场是张量场。Vespa 中的张量是灵活的多维数据结构，作为一等公民，可用于查询、文档字段和排名中的常数。张量可以是稠密的，也可以是稀疏的，或者两者兼有，并且可以包含任意数量的维度。更多信息请参见[张量用户指南](https://docs.vespa.ai/en/tensor-user-guide.html)。这里我们定义了一个单维的稠密张量(`d0` -维 0)，代表一个向量。51 是本文中使用的嵌入大小。

我们现在有一个用于`news`的模式和一个用于`user`的模式。

```
['news', 'user']
```

## 索引新闻嵌入

类似于用户模式，我们将使用密集张量来表示新闻嵌入。但与用户嵌入字段不同，我们将通过在`indexing`参数中包含`index`来索引新闻嵌入，并指定我们希望使用 HNSW(分层可导航小世界)算法来构建索引。使用的距离度量是欧几里德。阅读[这篇博文](https://blog.vespa.ai/approximate-nearest-neighbor-search-in-vespa-part-1/)了解更多关于 Vespa 实现 ANN 搜索的历程。

## 使用嵌入的推荐

在这里，我们添加了一个使用紧密度排名特性的排名表达式，它计算欧几里德距离并使用它来对新闻文章进行排名。这个等级配置文件依赖于使用最近邻搜索操作符，我们将在搜索时回到下面。但是现在，这需要查询中的一个张量作为初始搜索点。

## 查询配置文件类型

上面的推荐等级配置文件要求我们随查询一起发送一个张量。为了让 Vespa 绑定正确的类型，它需要知道这个查询参数的预期类型。

当查询参数 ranking . features . query(user _ embedding)被传递时，该查询配置文件类型指示 Vespa 预期一个维数为`d0[51]`的浮点张量。我们将在下面看到它是如何与最近邻搜索操作符一起工作的。

## 重新部署应用程序

我们做了所有必要的改变，将我们的新闻搜索应用变成了新闻推荐应用。我们现在可以将`app_package`重新部署到名为`news`的运行容器中。

```
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Finished deployment.
```

```
["Uploading application '/app/application' using http://localhost:19071/application/v2/tenant/default/session",
 "Session 7 for tenant 'default' created.",
 'Preparing session 7 using http://localhost:19071/application/v2/tenant/default/session/7/prepared',
 "WARNING: Host named 'news' may not receive any config since it is not a canonical hostname. Disregard this warning when testing in a Docker container.",
 "Session 7 for tenant 'default' prepared.",
 'Activating session 7 using http://localhost:19071/application/v2/tenant/default/session/7/active',
 "Session 7 for tenant 'default' activated.",
 'Checksum:   62d964000c4ff4a5280b342cd8d95c80',
 'Timestamp:  1616671116728',
 'Generation: 7',
 '']
```

## 喂养和部分更新:新闻和用户嵌入

为了让本教程易于理解，我们提供了已解析的嵌入供下载。要自己构建它们，请遵循[本教程](https://docs.vespa.ai/en/tutorials/news-4-embeddings.html)。

我们刚刚创建了`user`模式，所以我们需要第一次输入用户数据。

对于新闻文档，我们只需要更新添加到`news`模式中的`embedding`字段。

## 获取用户嵌入

接下来，我们创建一个`query_user_embedding`函数，通过`user_id`检索用户`embedding`。当然，您可以使用 Vespa 搜索器更有效地做到这一点，如这里描述的，但是在这一点上保持 python 中的所有内容会使学习更容易。

该函数将查询 Vespa，检索嵌入内容，并将其解析为一个浮点列表。下面是用户`U63195`嵌入的前五个元素。

```
[0.0,
 -0.1694680005311966,
 -0.0703359991312027,
 -0.03539799898862839,
 0.14579899609088898]
```

## 获取推荐

## 人工神经网络搜索

下面的`yql`指示 Vespa 从最接近用户嵌入的十个新闻文档中选择`title`和`category`。

我们还指定，我们希望通过我们之前定义的`recommendation` rank-profile 对这些文档进行排序，并通过我们在`app_package`中定义的查询配置文件类型`ranking.features.query(user_embedding)`向用户发送嵌入。

这是十个回复中的前两个。

```
[{'id': 'index:news_content/0/aca03f4ba2274dd95b58db9a',
  'relevance': 0.1460561756063909,
  'source': 'news_content',
  'fields': {'category': 'music',
   'title': 'Broadway Star Laurel Griggs Suffered Asthma Attack Before She Died at Age 13'}},
 {'id': 'index:news_content/0/bd02238644c604f3a2d53364',
  'relevance': 0.14591827245062294,
  'source': 'news_content',
  'fields': {'category': 'tv',
   'title': "Rip Taylor's Cause of Death Revealed, Memorial Service Scheduled for Later This Month"}}]
```

## 将人工神经网络搜索与查询过滤器相结合

Vespa ANN 搜索完全集成到 Vespa 查询树中。这种集成意味着我们可以包含查询过滤器，人工神经网络搜索将只应用于满足过滤器的文档。无需进行涉及过滤器的预处理或后处理。

以下`yql`搜索以`sports`为类别的新闻文档。

这是十个回复中的前两个。注意`category`字段。

```
[{'id': 'index:news_content/0/375ea340c21b3138fae1a05c',
  'relevance': 0.14417346200569972,
  'source': 'news_content',
  'fields': {'category': 'sports',
   'title': 'Charles Rogers, former Michigan State football, Detroit Lions star, dead at 38'}},
 {'id': 'index:news_content/0/2b892989020ddf7796dae435',
  'relevance': 0.14404365847394848,
  'source': 'news_content',
  'fields': {'category': 'sports',
   'title': "'Monday Night Football' commentator under fire after belittling criticism of 49ers kicker for missed field goal"}}]
```

# 结论和未来工作

我们通过在 Vespa 中存储用户资料，将我们的新闻搜索应用程序转变为新闻推荐应用程序。在这种情况下，用户简档被选择为由基于关于用户浏览历史的数据训练的 ML 模型生成的嵌入。然后，我们使用这些用户资料，根据近似最近邻搜索推荐新闻文章。未来的工作将集中在评估新闻推荐应用程序获得的结果是否符合用于生成嵌入的 ML 模型的预期。