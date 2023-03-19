# 使用 Vespa 从 python 构建新闻推荐应用程序

> 原文：<https://towardsdatascience.com/build-a-news-recommendation-app-from-python-with-vespa-6aa37450eb32?source=collection_archive---------50----------------------->

## 第 3 部分—通过亲子关系有效利用点击率

这部分系列引入了一个新的排名信号:类别点击率(CTR)。这个想法是，我们可以为没有点击历史的用户推荐受欢迎的内容。我们不是仅仅根据文章进行推荐，而是根据类别进行推荐。然而，这些全局 CTR 值经常会不断变化，因此我们需要一种有效的方法来为所有文档更新这个值。我们将通过在 Vespa 中引入文档之间的父子关系来做到这一点。我们也会在排名中直接使用稀疏张量。这篇文章复制了[这篇更详细的 Vespa 教程](https://docs.vespa.ai/en/tutorials/news-7-recommendation-with-parent-child.html)。

![](img/1987d10e070cc36b00252186df8d968b.png)

由 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的[absolute vision](https://unsplash.com/@freegraphictoday?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

我们假设您已经阅读了新闻推荐教程的[第 2 部分。因此，您应该有一个保存新闻应用程序定义的`app_package`变量和一个名为`news`的 Docker 容器来运行应用程序，该应用程序从 MIND 数据集的演示版本获取数据。](https://blog.vespa.ai/build-news-recommendation-app-from-python-with-vespa/)

## 设置全球类别 CTR 文档

如果我们在`news`文档中添加一个`category_ctr`字段，那么每当这项运动的 CTR 统计数据发生变化时，我们就必须更新这项运动的所有文档。如果我们假设类别 CTR 会经常改变，这被证明是低效的。

针对这些情况，Vespa 引入了[亲子关系](https://docs.vespa.ai/en/parent-child.html)。父文档是全局文档，会自动分发到所有内容节点。其他文档可以引用这些父项并“导入”值以用于排名。好处是全局类别 CTR 值只需要写入一个地方:全局文档。

我们通过创建一个新的`category_ctr`模式并设置`global_document=True`来表示我们希望 Vespa 在所有内容节点上保留这些文档的副本，从而实现了这一点。在父子关系中使用文档时，需要将文档设置为全局文档。注意，我们使用一个具有单一稀疏维度的张量来保存`ctrs`数据。

稀疏张量将字符串作为维度地址，而不是数字索引。更具体地说，这种张量的一个例子是(使用[张量文字形式](https://docs.vespa.ai/en/reference/tensor.html#tensor-literal-form)):

```
{
    {category: entertainment}: 0.2 }, 
    {category: news}: 0.3 },
    {category: sports}: 0.5 },
    {category: travel}: 0.4 },
    {category: finance}: 0.1 },
    ...
}
```

这个张量包含所有类别的 CTR 分数。在更新这个张量的时候，我们可以更新单个的细胞，不需要更新整个张量。这个操作叫做[张量修改](https://docs.vespa.ai/en/reference/document-json-format.html#tensor-modify)，当你有大的张量时会很有帮助。

## 在子文档中导入父值

我们需要设置两个东西来使用`category_ctr`张量对`news`文档进行排序。我们需要引用父文档(本例中为`category_ctr`),并从引用的父文档中导入`ctrs`。

字段`category_ctr_ref`是`category_ctr`单据类型的类型引用字段。当输入这个字段时，Vespa 需要完全合格的文档 id。例如，如果我们的全局 CTR 文档有 id `id:category_ctr:category_ctr::global`，那就是我们需要提供给`category_ctr_ref`字段的值。一个文档可以引用许多父文档。

导入的字段定义了我们应该从`category_ctr_ref`字段中引用的文档中导入`ctrs`字段。我们将其命名为`global_category_ctrs`,我们可以在排序时将其引用为`attribute(global_category_ctrs)`。

## 排序中的张量表达式

每个`news`文档都有一个类型为`string`的`category`字段，指示该文档属于哪个类别。我们希望使用这些信息来选择存储在`global_category_ctrs`中的正确 CTR 分数。不幸的是，张量表达式只对张量起作用，所以我们需要添加一个名为`category_tensor`的`tensor`类型的新字段，以在张量表达式中使用的方式保存类别信息:

使用上面定义的`category_tensor`字段，我们可以使用张量表达式`sum(attribute(category_tensor) * attribute(global_category_ctrs))`来选择与被排序的文档类别相关的特定 CTR。我们在下面的等级配置文件中将该表达式实现为`Function`:

在新的 rank-profile 中，我们添加了第一阶段排名表达式，该表达式将最近邻得分与类别 CTR 得分相乘，分别用函数`nearest_neighbor`和`category_ctr`实现。作为第一次尝试，我们将最近邻与类别 CTR 分数相乘，这可能不是组合这两个值的最佳方式。

## 部署

我们可以重用在本教程第一部分中创建的同一个名为`news`的容器。

```
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

## 饲料

接下来，我们将下载全局类别 CTR 数据，该数据已经以具有类别维度的稀疏张量所期望的格式进行了解析。

```
{'ctrs': {'cells': [{'address': {'category': 'entertainment'},
    'value': 0.029266420380943244},
   {'address': {'category': 'autos'}, 'value': 0.028475809103747123},
   {'address': {'category': 'tv'}, 'value': 0.05374837981352176},
   {'address': {'category': 'health'}, 'value': 0.03531784305129329},
   {'address': {'category': 'sports'}, 'value': 0.05611187986670051},
   {'address': {'category': 'music'}, 'value': 0.05471192953054426},
   {'address': {'category': 'news'}, 'value': 0.04420778372641991},
   {'address': {'category': 'foodanddrink'}, 'value': 0.029256852366228187},
   {'address': {'category': 'travel'}, 'value': 0.025144552013730358},
   {'address': {'category': 'finance'}, 'value': 0.03231013195899643},
   {'address': {'category': 'lifestyle'}, 'value': 0.04423279317474416},
   {'address': {'category': 'video'}, 'value': 0.04006693315980292},
   {'address': {'category': 'movies'}, 'value': 0.03335647459420146},
   {'address': {'category': 'weather'}, 'value': 0.04532171803495617},
   {'address': {'category': 'northamerica'}, 'value': 0.0},
   {'address': {'category': 'kids'}, 'value': 0.043478260869565216}]}}
```

我们可以将该数据点输入到`category_ctr`中定义的文档中。我们将为该文档分配`global` id。可以通过使用 Vespa id `id:category_ctr:category_ctr::global`来参考本文档。

我们需要对`news`文档执行部分更新，以包含关于引用字段`category_ctr_ref`和新`category_tensor`的信息，该字段将具有与每个文档相关联的特定类别的值`1.0`。

```
{'id': 'N3112',
 'fields': {'category_ctr_ref': 'id:category_ctr:category_ctr::global',
  'category_tensor': {'cells': [{'address': {'category': 'lifestyle'},
     'value': 1.0}]}}}
```

## 测试新的等级档案

我们将重新定义本教程第二部分中定义的`query_user_embedding`函数，并使用它进行一个涉及用户`U33527`和`recommendation_with_global_category_ctr`等级档案的查询。

下面第一个命中的是一篇体育文章。这里还列出了全球 CTR 文档，体育类的 CTR 得分为`0.0561`。因此，category_ctr 函数的结果是预期的`0.0561`。nearest _ neighborhood 分数是`0.149`，得到的相关性分数是`0.00836`。所以，这和预期的一样有效。

```
{'id': 'id:news:news::N5316',
 'relevance': 0.008369192847921151,
 'source': 'news_content',
 'fields': {'sddocname': 'news',
  'documentid': 'id:news:news::N5316',
  'news_id': 'N5316',
  'category': 'sports',
  'subcategory': 'football_nfl',
  'title': "Matthew Stafford's status vs. Bears uncertain, Sam Martin will play",
  'abstract': "Stafford's start streak could be in jeopardy, according to Ian Rapoport.",
  'url': "https://www.msn.com/en-us/sports/football_nfl/matthew-stafford's-status-vs.-bears-uncertain,-sam-martin-will-play/ar-BBWwcVN?ocid=chopendata",
  'date': 20191112,
  'clicks': 0,
  'impressions': 1,
  'summaryfeatures': {'attribute(category_tensor)': {'type': 'tensor<float>(category{})',
    'cells': [{'address': {'category': 'sports'}, 'value': 1.0}]},
   'attribute(global_category_ctrs)': {'type': 'tensor<float>(category{})',
    'cells': [{'address': {'category': 'entertainment'},
      'value': 0.029266420751810074},
     {'address': {'category': 'autos'}, 'value': 0.0284758098423481},
     {'address': {'category': 'tv'}, 'value': 0.05374838039278984},
     {'address': {'category': 'health'}, 'value': 0.03531784191727638},
     {'address': {'category': 'sports'}, 'value': 0.05611187964677811},
     {'address': {'category': 'music'}, 'value': 0.05471193045377731},
     {'address': {'category': 'news'}, 'value': 0.04420778527855873},
     {'address': {'category': 'foodanddrink'}, 'value': 0.029256852343678474},
     {'address': {'category': 'travel'}, 'value': 0.025144552811980247},
     {'address': {'category': 'finance'}, 'value': 0.032310131937265396},
     {'address': {'category': 'lifestyle'}, 'value': 0.044232793152332306},
     {'address': {'category': 'video'}, 'value': 0.040066931396722794},
     {'address': {'category': 'movies'}, 'value': 0.033356472849845886},
     {'address': {'category': 'weather'}, 'value': 0.045321717858314514},
     {'address': {'category': 'northamerica'}, 'value': 0.0},
     {'address': {'category': 'kids'}, 'value': 0.043478261679410934}]},
   'rankingExpression(category_ctr)': 0.05611187964677811,
   'rankingExpression(nearest_neighbor)': 0.14915188666574342,
   'vespa.summaryFeatures.cached': 0.0}}}
```

## 结论

本教程介绍了父子关系，并通过我们在排名中使用的全局 CTR 特性进行了演示。我们还引入了使用(稀疏)张量表达式的排序。