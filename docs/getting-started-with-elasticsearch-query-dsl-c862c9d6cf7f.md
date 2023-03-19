# Elasticsearch 查询 DSL 入门

> 原文：<https://towardsdatascience.com/getting-started-with-elasticsearch-query-dsl-c862c9d6cf7f?source=collection_archive---------1----------------------->

## 使用 Python Elasticsearch 客户端，用特定领域语言编写 Elasticsearch 查询的实践指南

![](img/add53927e8dd5923787932e48b4d3005.png)

照片由[克里斯多佛·伯恩斯](https://unsplash.com/@christopher__burns?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在这篇文章中，我将介绍弹性搜索中查询的基础知识。我们将看看如何在 [Elasticsearch 领域特定语言](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html) (DSL)中构建查询(例如，过滤器与查询上下文，以及相关性评分)，并在 [Python Elasticsearch 客户端](https://elasticsearch-py.readthedocs.io/en/6.8.2/)中应用它们。(而且，如果 DSL 让您头晕目眩，请跳到本文的最后一节，在这里我们将介绍针对 es 运行 SQL 查询的基础知识)。这篇博文中使用的所有代码都可以在 [this GitHub repo](https://github.com/ngoet/es_demo) 中找到。

# 入门指南

对于这篇文章，我假设你熟悉 ES 的基础知识。我还假设您已经提供并部署了自己的 Elasticsearch 集群(要么“从头开始”，要么使用托管服务)，并且您已经将数据写入了索引。

*要快速了解 ES 的基础知识以及如何使用 Python 提供集群和设置索引的说明，请查看我之前的文章* [*使用 Python 创建和管理弹性搜索索引*](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) 。

[](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [## 使用 Python 创建和管理弹性搜索索引

### 从 CSV 文件创建 ES 索引以及使用 Python Elasticsearch 管理数据的实践指南…

towardsdatascience.com](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) 

对于下面的例子，我将使用我在之前的文章中建立的`neflix_movies`索引。这个指数是根据 Kaggle 上的 7787 场网飞演出的数据构建的。原始数据是 CSV 格式的，包含网飞上可用的电影和电视剧的信息，包括元数据，如上映日期、片名和演员。我们的索引的映射定义如下:

这种映射可以进一步改进，并没有针对磁盘使用进行优化，也没有针对搜索速度进行[调整。然而，对于我们将在下面运行的查询来说，这已经足够了。](https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-search-speed.html)

*如果你想学习如何创建更好的针对磁盘使用优化的 ES 索引，请查看我之前关于* [*优化弹性搜索中的磁盘使用*](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) *的帖子。*

[](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) [## 优化弹性搜索中的磁盘使用

towardsdatascience.com](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) 

# DSL 快速入门

在我们开始这篇博文的实践部分之前，让我们回顾一下从 Elasticsearch 查询数据的一些基础知识。ES search API 接受使用基于 JSON 的 [Elasticsearch 领域特定语言(DSL)](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html) 的查询。ES 文档将 DSL 描述为查询的抽象语法树(AST ),由两种类型的子句组成:

1.  **叶查询子句**，查找特定字段中的特定值(如`match`或`range`)；和
2.  **复合查询子句**用于逻辑组合多个查询(如多叶或复合查询)或改变这些查询的行为。

当你运行一个针对你的索引(或多个索引)的查询时，ES [通过一个代表匹配质量的**相关性分数**(一个浮点值)](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html#relevance-scores)(`_score`字段显示其每次“命中”的值)。一个 ES 查询[有一个查询和一个过滤上下文。](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html)过滤器上下文——顾名思义——简单地过滤掉不符合语法条件的文档。然而，与 bool 上下文中的匹配不同，它不会影响相关性分数。

让我们来看一个简单的查询示例，它有一个**查询**和一个**过滤器**上下文。下面的例子过滤发行年份(使用一个[范围查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html)，并在查询上下文中针对电影类型运行一个[匹配查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html)。

```
{
    "query": {
        "bool": {
            "must": [
                {"match": {"type": "TV Movie"}},
            ],
            "filter": [
                {"range": {"release_year": {"gte": 2021}}}
            ]
        }
    }
}
```

我们可以直接使用我们的 ES 控制台(假设您使用的是基于云的平台，如[Elastic.co](https://www.elastic.co)、 [Bonsai](https://bonsai.io) 或[亚马逊 Elasticsearch 服务](https://aws.amazon.com/elasticsearch-service/))或通过 [Python Elasticsearch 客户端](https://elasticsearch-py.readthedocs.io/en/6.8.2/)在我们的索引上运行这个查询。无论您使用客户端还是控制台，查询本身看起来都是一样的。在本文的剩余部分，我的例子将使用 Python ES 客户端和 DSL。

为了开始，我们首先设置 ES 客户端连接(更多细节，请参见[我之前的博文](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113)和[这个 GitHub repo](https://github.com/ngoet/es_demo) ):

第二步，我们定义并运行针对`netflix_movies`索引的查询:

代码末尾的 print 语句将标题和相关的相关性分数打印为元组。输出如下所示:

```
[
 ('Bling Empire', 0.96465576),
 ('Carmen Sandiego', 0.96465576),
 ('Cobra Kai', 0.96465576),
 ('Disenchantment', 0.96465576),
 ('Dream Home Makeover', 0.96465576),
 ("Gabby's Dollhouse", 0.96465576),
 ('Headspace Guide to Meditation', 0.96465576),
 ('Hilda', 0.96465576),
 ('History of Swear Words', 0.96465576),
 ('Inside the World’s Toughest Prisons', 0.96465576)
]
```

在这种情况下，所有条目的相关性分数都是相同的:这是有意义的，因为所有这些条目对于`type` (即“电视电影”)都具有完全相同的值。既然我们知道“TV Movie”是该字段可以取的值，我们也可以使用过滤器上下文，因为我们正在寻找那个*确切的值*。

那么什么时候应该使用过滤器或查询上下文呢？一般来说，过滤器更便宜，应该尽可能在任何时候使用( [ES 自动缓存经常使用的过滤器以提高性能](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/query-filter-context.html))。它们用于搜索二进制案例(答案[明确为“是”或“否”](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html))和精确值(例如在搜索数值、特定范围或关键字时)。反过来，当结果可能不明确或用于全文搜索时(即当[搜索已分析的文本字段](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html)时)，应使用查询上下文。

让我们带着何时使用过滤器和查询上下文的信息重新看看上面显示的查询:该查询适用于一系列短字段类型(`release_year`)和关键字字段类型(`type`)。对于后者，我们知道自己想要的确切价值(“电视剧”)。因此，编写查询的更好方法如下，重用范围过滤器，并对后者应用[术语查询](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/query-dsl-term-query.html)(下面将详细介绍这种类型的查询)，并将两者放在“过滤器”上下文中:

# 常见查询

现在我们已经介绍了搜索和 DSL 的基础知识，让我们先来看看几个基本的查询。

## 比赛

首先是“匹配”查询，这是全文搜索的[默认选项。假设我们正在寻找“纸牌屋”系列的元数据。因为 ES 在使用匹配查询时分析文本，所以如果我们使用默认值(DSL 默认为“or”操作符)，它将返回标题中有“House”或“Cards”的任何节目。因此，我们将使用“and”运算符:](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/query-dsl-match-query.html)

该查询产生一个匹配项(一个文档):

```
{
  "total": {
    "value": 1,
    "relation": "eq"
  },
  "max_score": 15.991642,
  "hits": [
    {
      "_index": "netflix_movies",
      "_type": "_doc",
      "_id": "rWHgLHgBIp2yHscf16v-",
      "_score": 15.991642,
      "_source": {
        "show_id": "s2833",
        "type": "TV Show",
        "title": "House of Cards",
        "director": null,
        "cast": "Kevin Spacey, Robin Wright, Kate Mara, Corey Stoll, Sakina Jaffrey, Kristen Connolly, Constance Zimmer, Sebastian Arcelus, Nathan Darrow, Sandrine Holt, Michel Gill, Elizabeth Norment, Mahershala Ali, Reg E. Cathey, Molly Parker, Derek Cecil, Elizabeth Marvel, Kim Dickens, Lars Mikkelsen, Michael Kelly, Joel Kinnaman, Campbell Scott, Patricia Clarkson, Neve Campbell",
        "country": "United States",
        "date_added": "November 2, 2018",
        "release_year": 2018,
        "rating": "TV-MA",
        "duration": "6 Seasons",
        "listed_in": "TV Dramas, TV Thrillers",
        "description": "A ruthless politician will stop at nothing to conquer Washington, D.C., in this Emmy and Golden Globe-winning political drama."
      }
    }
  ]
}
```

这个点击的相关性分数是`15.991642`。乍一看，这似乎很奇怪。但是，请记住，相关性分数是没有限制的，它用于评估一个文档相对于匹配查询的所有其他文档的相关性。换句话说:分数表示在相同的搜索中，与相同查询的其他“命中”(文档)相比，文档与查询的相关程度。

## 术语查询

术语查询用于“查找”字段与精确值匹配的文档[。我们的索引中有许多`keyword`字段，我们可以对其应用术语查询。下面的例子提取了五个文档，其中`type`字段等于`TV Show`(注意`size`参数控制 ES 返回多少条记录，默认为 10 条):](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)

文本字段应该使用*而不是*术语查询。ES 在分析过程中改变文本字段，[标记文本，去掉标点符号，并将其转换成小写字母](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)。如果`type`是一个`text`字段而不是一个`keyword`字段，上面显示的查询将不会返回结果，因为作为分析的一部分,`“TV Show”`的任何实例都将被转换为`["tv", "show"]`。

## 范围

在我们的“常见查询”系列中，最后但同样重要的是“范围查询”。顾名思义，范围查询用于[查找特定字段的值落在定义的范围](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html)内的任何文档。它可用于`date`和数字字段类型。它也可以用于`text`和关键字字段，只要您的集群设置[允许昂贵的查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html#query-dsl-allow-expensive-queries)(默认)。下面显示的查询对发行年份运行简单的范围查询，搜索 2012 年或更早发行的 100 部电影或电视节目:

# (略多)复杂的查询

我倾向于使用几个更复杂的查询。在这一节中，我们将简要了解正则表达式、度量聚合和定制过滤器。

## 使用正则表达式

[regexp 查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-regexp-query.html)设计用于查找特定字段中的术语与正则表达式匹配的文档。下面的示例在我们的`netflix_movies`索引中查找任何文档，其中`title`字段包含部分由“war”组成的单词(例如 war、warror、war、warm 等)。):

Regexp 查询可以区分大小写(使用`case_insensitive`选项，默认设置为`false`)。然而，`title`是一个文本字段，其分析*未*被禁用。因此，我们可以预期`title`字段中的术语已经被小写化了(除了别的以外，请参考前面的章节以获得更多关于“分析”的细节)。因此，考虑到我们当前的映射，启用或禁用区分大小写没有什么区别。

## 使用指标聚合检查数据

ES 聚合是探索数据的一种很好的方式，它们有三种风格[:度量、桶和管道。在这里，我们将看到一个来自指标，一个来自存储桶类别。](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)

我们首先来看一下 [stats 查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics-stats-aggregation.html)(一个[度量聚合](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics.html))，当您想要查看数字值的汇总统计信息(min、max、mean、sum)时，这个查询会很方便。例如，如果我们想快速查看`release_year`在我们的数据中是如何分布的，我们可以运行以下查询:

对于聚合，我们在查询中将`size`参数设置为 0。这意味着搜索 API 不会返回任何文档(如果您忽略这一点，`hits`字段将包含文档)。该查询的响应为我们提供了整个文档中`release_year`值的简单汇总统计数据(例如，最小值、最大值、平均值):

```
'{
  "release_year_stats": {
    "count": 7787,
    "min": 1925.0,
    "max": 2021.0,
    "avg": 2013.932579940927,
    "sum": 15682493.0
  }
}'
```

现在让我们转向第二个聚合查询，这一次来自 [bucket 聚合](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket.html)家族。[术语聚合](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html)有助于快速了解不同类别的观察结果的可用性。例如，在我们的`netflix_movies`索引中，您可能想要查看在任何给定年份有多少部电影上映，或者每种类型有多少部电影出现在数据集中。在 DSL 中，我们对前者的查询(每个发行年份的电影数量)如下所示:

在这个查询中有两件事值得强调。首先，我们在`terms`对象中指定第二个`size`参数，它控制返回多少个“术语桶”(在本例中是:5)。第二，我们按关键字(即发行年份)以升序对输出进行排序。该查询的输出应该如下所示:

```
{
  "release_years": {
    "doc_count_error_upper_bound": 0,
    "sum_other_doc_count": 7775,
    "buckets": [
      {
        "key": 1925,
        "doc_count": 1
      },
      {
        "key": 1942,
        "doc_count": 2
      },
      {
        "key": 1943,
        "doc_count": 3
      },
      {
        "key": 1944,
        "doc_count": 3
      },
      {
        "key": 1945,
        "doc_count": 3
      }
    ]
  }
}
```

让我们快速回顾一下这个输出。`buckets`字段包括五个术语桶中每一个的文档计数(每个关键字一个对象)。反过来，`sum_other_doc_count`是从输出中排除的桶的所有计数的总和。最后，`doc_count_error_upper_bound`值反映了[与桶聚集](http://doc_count_error_upper_bound)相关联的误差。此时你可能会想:“错误，什么错误？”。ES 在响应中报告了一个错误，因为术语聚合与近似。这源于这样一个事实，即文档分布在多个分片中，每个分片都有自己的有序术语列表，这些列表在聚合中组合在一起。

协调搜索过程的[节点请求等于](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html#_shard_size_3) `[size](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html#_shard_size_3)`的多个术语桶。随后，来自这些碎片的结果被组合以产生最终的响应。因此，如果指定的`size`小于唯一项的数量，我们可以预期出现错误的可能性会更大。在上面的例子中，我们有 73 个不同的`release_year`值，所以我们可能想要更改`size`参数(对于大量的术语桶，`shard_size`也是如此，以解决[因增加](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html#_shard_size_3) `[size](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html#_shard_size_3)`而产生的一些额外开销)。

## 使用脚本查询自定义过滤器

我们最后的“复杂”查询:“脚本查询”。如果您想对索引中的文档使用自己设计的过滤器，可以使用[脚本查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-script-query.html)。过滤器上下文中提供了脚本查询。在下面的例子中，我编写了一个简单的脚本来提取`release_year`大于 2018 的文档(我们可以使用范围查询获得相同的结果，见上文)。

`source`字段包含脚本，`params`字段包含脚本使用的`start_year`值。脚本查询的默认语言是“[无痛](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting.html)”(在`lang`字段中指定)，但是其他语言也是可用的，包括 [Lucene 的表达式语言](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting-expression.html)和 [Mustache 语言](https://mustache.github.io/mustache.5.html)。如果您有经常使用的定制脚本，您可以使用[端点在集群状态](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting-using.html)上存储脚本，这样您可以检索它们以备后用。存储脚本[加快了搜索速度，因为它通常会减少编译所需的时间](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting-using.html)。

# 使用 SQL 语法查询 ES

对于 SQL 爱好者:您可以使用 SQL 语法来查询您的 ES 索引。该功能是 [X-Pack](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-xpack.html) 的一部分，您可以使用它在基于云的 es 服务(如 [AWS](https://aws.amazon.com/about-aws/whats-new/2019/05/amazon-elasticsearch-service-sql-support/) 或[Elastic.co](https://www.elastic.co/what-is/elasticsearch-sql))的控制台中直接对您的 ES 索引执行 SQL 查询，或者使用 Python Elasticsearch 客户端。(注意这个[不能在 Bonsai 上运行，因为 X-Pack 插件不包括在内，因为它需要一个用于商业目的的许可证](https://medium.com/r?url=https%3A%2F%2Fdocs.bonsai.io%2Farticle%2F135-plugins-on-bonsai)。因此，我在[Elastic.co](https://www.elastic.co)上运行了 SQL translate API 的以下 API 调用，它提供了 14 天的免费试用。

## 将您的 SQL 语法转换成 DSL

如果您在 Elasticsearch 上工作，您可以使用 SQL translate API 来[将 SQL 语法翻译成本地的 Elasticsearch 查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/sql-translate.html)。在控制台上，这就像在 JSON 文档中使用带有 SQL 语法的 POST 请求一样简单。例如，获取 2015 年或以后上映的网飞电影的标题列表的查询如下所示:

```
{"query": "SELECT title FROM netflix_movies WHERE release_year >= 2015"}
```

如果您在 API 控制台上的翻译端点(`POST _sql/translate`)上使用 POST 请求发送该文档，它将返回本地 ES 查询(DSL)中的相应查询。对于我们的示例，产生的 ES 查询是:

```
{
  "sort": [
    {
      "_doc": {
        "order": "asc"
      }
    }
  ],
  "query": {
    "range": {
      "release_year": {
        "to": null,
        "include_upper": false,
        "boost": 1,
        "from": 2015,
        "include_lower": true
      }
    }
  },
  "_source": false,
  "fields": [
    {
      "field": "title"
    }
  ],
  "size": 1000
}
```

## 直接运行 SQL 查询

您也可以直接获得输出，完全跳过“翻译步骤”。如果您使用带有`_sql?format=txt`的 JSON 文档发出 POST 请求，API 会以一种格式良好的文本格式返回响应数据(对于 JSON 格式，使用`_sql?format=json`):

```
title                                
------------------------------------- 
Black Crows                                                                     Black Earth Rising                                                              Black Lightning                                                                 Black Man White Skin                                                            Black Mirror                                                                    Black Mirror: Bandersnatch                                                      Black Panther                                                                   Black Sea                                                                       Black Site Delta                                                                Black Snow
...........
```

类似地，您可以运行简单的`GROUP BY`查询(假设您执行聚合的字段不是`text`类型)。例如，以下查询将返回每个发行年份的电影数量(这里我将输出限制为前五年):

```
POST _sql?format=txt
{"query": "SELECT release_year, COUNT(*) as n_entries FROM netflix_movies GROUP BY release_year LIMIT 5"}
```

同样，输出的格式也很好:

```
release_year  |   n_entries    
---------------+--------------- 
1925           |1               
1942           |2               
1943           |3               
1944           |3               
1945           |3
```

就是这样！我们现在已经回顾了 DSL 的基础知识，并运行了几个基本的和更复杂的查询。

感谢您的阅读！

[](https://medium.com/@ndgoet/membership) [## 用我的推荐链接加入媒体。

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@ndgoet/membership) 

**如果你喜欢这篇文章，这里还有一些你可能喜欢的文章:**

[](/four-things-you-need-to-master-to-get-started-with-elasticsearch-c51bed6ae99d) [## 开始使用 Elasticsearch 需要掌握的四件事

### 弹性研究概念和原则的简明概述

towardsdatascience.com](/four-things-you-need-to-master-to-get-started-with-elasticsearch-c51bed6ae99d) [](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [## 使用 Python 创建和管理弹性搜索索引

### 从 CSV 文件创建 ES 索引以及使用 Python Elasticsearch 客户端管理数据的实践指南

towardsdatascience.com](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) [## 优化弹性搜索中的磁盘使用

towardsdatascience.com](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) 

***免责声明****:“elastic search”是 Elasticsearch BV 的商标，在美国和其他国家注册。本文中对任何第三方服务和/或商标的描述和/或使用不应被视为对其各自权利持有人的认可。*

*请仔细阅读* [*本免责声明*](https://medium.com/@ndgoet/disclaimer-5ad928afc841) *中的任何内容后再依托* [*我的 Medium.com 文章*](/@ndgoet) *。*