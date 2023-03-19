# 在 Docker 上使用 Node.js 和 ElasticSearch 进行全文搜索

> 原文：<https://towardsdatascience.com/full-text-search-with-node-js-and-elasticsearch-on-docker-edcea23612fd?source=collection_archive---------4----------------------->

## 让我们基于 Node.js、ElasticSearch 和 Docker 构建一个真实世界的应用程序

![](img/9e2e1c9d2a642b2fe177203c3b6f5109.png)

照片由[张诗钟·维诺](https://unsplash.com/@johnyvino)在 [Unsplash](https://unsplash.com) 上拍摄

全文搜索既令人害怕又令人兴奋。一些流行的数据库，如 MySql 和 Postgres，是存储数据的惊人解决方案…但当谈到全文搜索性能时，没有与 ElasticSearch 竞争。

对于那些不知道的人来说， **ElasticSearch** 是一个建立在 **Lucene** 之上的搜索引擎服务器，具有惊人的分布式架构支持。根据 db-engines.com 的说法，它是目前使用最多的搜索引擎。

在这篇文章中，我们将构建一个简单的 REST 应用程序，称为报价数据库，它将允许我们存储和搜索尽可能多的报价！

我准备了一个 [JSON 文件](https://github.com/micheleriva/the-quotes-database/blob/master/src/data/quotes.json)，其中包含 5000 多条作者引用；我们将用它作为填充 ElasticSearch 的初始数据。

您可以在这里找到这个项目[的资源库。](https://github.com/micheleriva/the-quotes-database)

# 设置 Docker

首先，我们不想在我们的机器上安装 ElasticSearch。我们将使用 Docker 在一个容器上编排 Node.js 服务器和 ES 实例，这将允许我们部署一个生产就绪的应用程序以及它需要的所有依赖项！

让我们在项目根文件夹中创建一个`Dockerfile`:

如你所见，我们告诉 Docker 我们将运行 Node.js 10.15.3-alpine 运行时。我们还将在`/usr/src/app`下创建一个新的工作目录，在那里我们将复制`package.json`和`package-lock.json`文件。这样，Docker 将能够在我们的`WORKDIR`中运行`npm install`，安装我们需要的依赖项。

我们还将通过运行`RUN npm install -g pm2`在全球范围内安装 [PM2](https://pm2.keymetrics.io/) 。Node.js 运行时是单线程的，因此如果一个进程崩溃，整个应用程序都需要重启...PM2 检查 Node.js 进程状态，并在应用程序因任何原因关闭时重新启动它。

安装 PM2 后，我们将在我们的`WORKDIR` ( `COPY . ./`)中复制我们的代码库，我们告诉 Docker 公开两个端口:`3000`，这将公开我们的 RESTful 服务，和`9200`，这将公开 ElasticSearch 服务(`EXPOSE 3000`和`EXPOSE 9200`)。

最后，我们告诉 Docker 哪个命令将启动 Node.js 应用程序:`npm run start`。

# 设置 docker 撰写

现在你可能在想，“太好了，我明白了！但是我如何在 Docker 中处理 ElasticSearch 实例呢？我在我的文档里找不到！_“…你说得对！这就是 docker-compose 派上用场的地方。它允许我们编排多个 Docker 容器，并在它们之间创建连接。因此，让我们写下`docker-compose.yml`文件，它将存储在我们的项目根目录中:

这比我们的 docker 文件要复杂一些，但是让我们来分析一下:

*   我们声明我们使用的是哪个版本的文件(`3.6`)
*   我们声明我们的服务:`api`这是我们的 Node.js 应用程序。就像在我们的 docker 文件中一样，它需要`node:10.15.3-alpine`图像。我们还为这个容器指定了一个名字:`tqd-node`，在这里，我们使用`build .`命令调用之前创建的 Dockerfile。然后，我们需要公开`3000`端口:如您所见，我们将这些语句编写如下:`3000:3000`。这意味着我们从端口`3000`(在我们的容器内)映射到端口`3000`(可以从我们的机器访问)。然后我们将设置一些环境变量。值`elasticsearch`是一个变量，它引用我们的`docker-compose.yml`文件中的`elasticsearch`服务。我们还想挂载一个卷:`/usr/src/app/quotes`。这样，一旦我们重启我们的容器，我们将维护我们的数据而不丢失它。我们再次告诉 Docker，一旦容器启动，我们需要执行哪个命令，然后我们设置一个到`elasticsearch`服务的链接。我们还告诉 Docker 在`elasticsearch`服务启动后启动`api`服务(使用`depends_on`指令)。最后，我们告诉 Docker 在`esnet`网络下连接`api`服务。这是因为每个容器都有自己的网络:这样，我们说`api`和`elasticsearch`服务共享同一个网络，所以它们将能够用相同的端口相互调用。这是(你可能已经猜到了)我们的 ES 服务。它的配置与`api`服务非常相似。我们将把`logging`指令设置为`driver: none`来删除它的详细日志。
*   我们声明存储 es 数据的卷
*   我们宣布我们的网络，`esnet`

# 引导 Node.js 应用程序

现在我们需要创建 Node.js 应用程序，所以让我们开始设置我们的`package.json`文件:

```
npm init -y
```

现在我们需要安装一些依赖项:

```
npm i -s @elastic/elasticsearch body-parser cors dotenv express
```

太好了！我们的`package.json`文件应该是这样的:

让我们在 Node.js 中实现我们的 ElasticSearch 连接器。首先，我们需要创建一个新的`/src/elastic.js`文件:

如你所见，这里我们设置了一些非常有用的常量。首先，我们使用其官方 Node.js SDK 创建一个到 ElasticSearch 的新连接；然后，我们定义一个索引(`"quotes"`)和一个索引类型(`"quotes"`)，我们稍后会看到它们的含义。

现在我们需要在 ElasticSearch 上创建一个索引。您可以将“索引”视为 SQL“数据库”的等价物。ElasticSearch 是一个 NoSQL 数据库，这意味着它没有表——它只存储 JSON 文档。索引是映射到一个或多个主碎片的逻辑名称空间，可以有零个或多个副本碎片。你可以在这里阅读更多关于弹性搜索指数的信息。

现在让我们定义一个创建索引的函数:

现在我们需要另一个函数来为我们的报价创建映射。映射定义了我们文档的模式和类型:

如您所见，我们正在为文档定义模式，并将它插入到我们的`index`中。

现在让我们考虑一下，ElasticSearch 是一个庞大的系统，可能需要几秒钟才能启动。在 ES 准备好之前，我们无法连接到 ES，因此我们需要一个函数来检查 ES 服务器何时准备好:

如你所见，我们正在回报一个承诺。这是因为通过使用，`async/await`,我们能够停止整个 Node.js 进程，直到这个承诺得到解决，并且它不会这样做，直到它连接到 es。这样，我们强制 Node.js 在启动前等待 ES。

我们已经完成了 ElasticSearch！现在，让我们导出我们的函数:

太好了！让我们看看整个`elastic.js`档案:

# 用报价填充弹性搜索

现在我们需要用我们的报价填充我们的 ES 实例。这听起来很容易，但是相信我，这可能是我们应用程序中很棘手的一部分！

让我们在`/src/data/index.js`中创建新文件:

正如您所看到的，我们正在导入刚刚创建的`elastic`模块和来自存储在`/src/data/quotes.json`中的 JSON 文件的报价。我们还创建了一个名为`esAction`的对象，一旦我们插入一个文档，它将告诉 ES 如何索引它。

现在我们需要一个脚本来填充我们的数据库。我们还需要创建一个具有以下结构的对象数组:

如您所见，对于我们将要插入的每个报价，我们需要将其映射设置为 ElasticSeaech。这就是我们要做的:

太好了！现在让我们创建我们的主文件`/src/main.js`，看看我们将如何组织我们到目前为止所写的所有内容:

我们来分析一下上面的代码。我们创建一个自动执行的 main 函数来检查 ES 连接。在 ES 连接之前，代码不会执行。当 ES 准备好时，我们将检查`quotes`索引是否存在:如果不存在，我们将创建它，我们将设置它的映射，并将填充数据库。显然，我们只有在第一次启动服务器时才会这样做！

# 创建 RESTful API

现在我们需要创建 RESTful 服务器。我们将使用 Express.js，它是构建服务器最流行的 Node.js 框架。

我们将从`/src/server/index.js`文件开始:

如你所见，它只是一个标准的 Express.js 服务器；我们不会在那上面花太多时间。让我们看看我们的`/src/server/routes/index.js`档案:

我们创建两个端点:

*   `GET /`将返回与我们的查询字符串参数匹配的报价列表。
*   `POST /new/`将允许我们发布一个新的报价存储在弹性搜索。

现在让我们看看我们的`/src/server/controllers/index.js`文件:

这里我们基本上定义了两个函数:

*   `getQuotes`，需要至少一个查询字符串参数:`text`
*   `addQuote`，需要两个参数:`author`和`quote`

ElasticSearch 接口委托给我们的`/src/server/models/index.js`。这种结构有助于我们维护一个类似 MVC 的架构。让我们看看我们的模型:

如您所见，我们通过选择包含给定单词或短语的每个报价来构建我们的 ElasticSearch 查询。然后，我们生成查询，设置`page`和`limit`值:我们可以在查询字符串中传递它们，例如:`http://localhost:3000/quotes?text=love&page=1&limit=100`。如果这些值没有通过查询字符串传递，我们将使用它们的默认值。

ElasticSearch 返回大量数据，但我们需要四样东西:

*   报价 ID
*   引用本身
*   引用作者
*   得分

分数代表报价与我们的搜索词的接近程度；一旦我们有了这些值，我们就将它们和总结果数一起返回，这在前端对结果进行分页时可能会很有用。

现在我们需要为模型创建最后一个函数:`insertNewQuote`:

这个函数很简单:我们将引文和作者发布到我们的索引中，并将查询结果返回给控制器。现在，完整的`/src/server/models/index.js`文件应该如下所示:

我们完事了。我们需要从里到外设置我们的启动脚本`package.json`文件，我们已经准备好了:

一旦连接了 ElasticSearch，我们还需要更新我们的`/src/main.js`脚本来启动我们的 Express.js 服务器:

# 启动应用程序

我们现在准备使用 docker-compose 启动我们的应用程序！只需运行以下命令:

```
$ docker-compose up
```

您需要等到 Docker 下载了 ElasticSearch 和 Node.js 图像，然后它将启动您的服务器，您就可以对 REST 端点进行查询了！

让我们用几个 cURL 调用进行测试:

```
$ curl localhost:3000/quotes?text=love&limit=3

{
  "success": true,
  "data": {
    "results": 716,
    "values": [
      {
        "id": "JDE3kGwBuLHMiUvv1itT",
        "quote": "There is only one happiness in life, to love and be loved.",
        "author": "George Sand",
        "score": 6.7102118
      },
      {
        "id": "JjE3kGwBuLHMiUvv1itT",
        "quote": "Live through feeling and you will live through love. For feeling is the language of the soul, and feeling is truth.",
        "author": "Matt Zotti",
        "score": 6.2868223
      },
      {
        "id": "NTE3kGwBuLHMiUvv1iFO",
        "quote": "Genuine love should first be directed at oneself if we do not love ourselves, how can we love others?",
        "author": "Dalai Lama",
        "score": 5.236455
      }
    ]
  }
}
```

如你所见，我们决定将结果限制在`3`，但是还有其他 713 个引用！我们可以通过调用以下命令轻松获得接下来的三个报价:

```
$ curl localhost:3000/quotes?text=love&limit=3&page=2{
  "success": true,
  "data": {
    "results": 716,
    "values": [
      {
        "id": "SsyHkGwBrOFNsaVmePwE",
        "quote": "Forgiveness is choosing to love. It is the first skill of self-giving love.",
        "author": "Mohandas Gandhi",
        "score": 4.93597
      },
      {
        "id": "rDE3kGwBuLHMiUvv1idS",
        "quote": "Neither a lofty degree of intelligence nor imagination nor both together go to the making of genius. Love, love, love, that is the soul of genius.",
        "author": "Wolfgang Amadeus Mozart",
        "score": 4.7821507
      },
      {
        "id": "TjE3kGwBuLHMiUvv1h9K",
        "quote": "Speak low, if you speak love.",
        "author": "William Shakespeare",
        "score": 4.6697206
      }
    ]
  }
}
```

如果您需要插入新的报价呢？就叫`/quotes/new`端点吧！

```
$ curl --request POST \
     --url http://localhost:3000/quotes/new \
     --header 'content-type: application/json' \
     --data '{
        "author": "Michele Riva",
        "quote": "Using Docker and ElasticSearch is challenging, but totally worth it."
}'
```

答案会是:

```
{
  "success": true,
  "data": {
    "id": "is2QkGwBrOFNsaVmFAi8",
    "author": "Michele Riva",
    "quote": "Using Docker and ElasticSearch is challenging, but totally worth it."
  }
}
```

# 结论

Docker 使得管理我们的依赖项和它们的部署变得非常容易。从那时起，我们可以轻松地在 [Heroku](https://web.archive.org/web/20210213000221/https://heroku.com/) 、 [AWS ECS](https://web.archive.org/web/20210213000221/https://aws.amazon.com/ecs/) 、 [Google Cloud Container](https://web.archive.org/web/20210213000221/https://cloud.google.com/containers/?hl=it) 或任何其他基于 Docker 的服务上托管我们的应用程序，而无需费力地用它们的超级复杂的配置来设置我们的服务器。

下一步？

*   了解如何使用 [Kubernetes](https://web.archive.org/web/20210213000221/https://kubernetes.io/) 来扩展您的容器并编排更多的弹性搜索实例！
*   创建一个允许您更新现有报价的新端点。错误是会发生的！
*   那么删除一个报价呢？您将如何实现该端点？
*   用标签保存你的引语会很棒(例如，关于爱情、健康、艺术的引语)…试着更新你的`quotes`索引！

软件开发很有趣。有了 Docker，Node，和 ElasticSearch，就更好了！