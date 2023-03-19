# Weaviate Python 库入门

> 原文：<https://towardsdatascience.com/getting-started-with-weaviate-python-client-e85d14f19e4f?source=collection_archive---------14----------------------->

## 关于新的基于机器学习的矢量搜索引擎的完整教程

![](img/d677ed6e8a8a45e493925de6b9412ad4.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3501528)的[艾哈迈德·加德](https://pixabay.com/users/ahmedgad-9403351/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3501528)拍摄

0.新的更新。

1.  什么是 Weaviate？
2.  可以用在哪里？
3.  有什么优势？
4.  什么是 Weaviate Python 客户端？
5.  如何在 Weaviate 集群中使用 Weaviate Python 客户端？

*   5.0.创建一个弱实例/集群。
*   5.1.连接到集群。
*   5.2.获取数据并进行分析。
*   5.3.创建适当的数据类型。
*   5.4.加载数据。
*   5.5.查询数据。

## 0.新的更新。

Weaviate-client 版本 3.0.0 带来了一些新的变化，查看官方文档了解所有信息[这里](https://weaviate-python-client.readthedocs.io/en/latest/changelog.html#version-3-0-0)。

整篇文章现在包含了旧版本和新版本的例子(只有那些被改变的)。

## 1.什么是 Weaviate？

Weaviate 是一个开源、云原生、模块化的实时矢量搜索引擎。它是为扩展你的机器学习模型而构建的。因为 Weaviate 是模块化的，所以你可以将其用于任何进行数据编码的机器学习模型。Weaviate 带有可选的文本、图像和其他媒体类型的模块，可以根据您的任务和数据进行选择。此外，根据数据的种类，可以使用多个模块。更多信息[点击这里](https://www.semi.technology/developers/weaviate/current/)。

在本文中，我们将使用*文本*模块来了解 Weaviate 最重要的功能和能力。文本模块，也称为 *text2vec-contextionary* ，捕捉文本对象的语义，并将其置于一个概念超空间中。这允许进行语义搜索，与其他搜索引擎的“单词匹配搜索”形成对比。

欲了解更多关于 Weaviate 和 SeMI 技术公司的信息，请访问官方[网站](https://www.semi.technology/)。

## 2.可以用在哪里？

目前，Weaviate 用于以下情况:

*   语义搜索，
*   相似性搜索，
*   图片搜索，
*   电力推荐引擎，
*   电商搜索，
*   网络安全威胁分析，
*   自动化数据协调，
*   异常检测，
*   ERP 系统中的数据分类，

以及更多的案例。

## 3.有什么优势？

要了解什么是我们的优势，你应该问自己这些问题:

*   你目前的搜索引擎给你的搜索结果质量够好吗？
*   让你的机器学习模型规模化是不是工作量太大了？
*   您是否需要快速且近乎实时地对大型数据集进行分类？
*   你需要将你的机器学习模型扩展到生产规模吗？

我们可以解决所有这些问题。

## 4.什么是 Weaviate Python 客户端？

Weaviate Python 客户端是一个 Python 包，允许您与 Weaviate 实例进行连接和交互。python 客户端不是一个 Weaviate 实例，但是你可以用它在 [Weaviate 云服务](https://console.semi.technology/)上创建一个。它提供了用于导入数据、创建模式、进行分类、查询数据的 API 我们将浏览其中的大部分，并解释如何以及何时可以使用它们。

这个包被发布到 PyPI ( [链接](https://pypi.org/project/weaviate-client/))。另外，PyPI 上有一个 CLI 工具([链接](https://pypi.org/project/weaviate-cli/))。

## 5.如何在一个脆弱的集群中使用 python-client？

在这一节中，我们将经历创建一个 Weaviate 实例的过程，连接到它并探索一些功能。

(jupyter-notebook 可以在这里找到[。)](https://github.com/semi-technologies/Getting-Started-With-Weaviate-Python-Client/blob/main/Getting-Started-With-Weaviate-Python-Client.ipynb)

## 5.0.创建一个弱实例/集群。

创建一个 Weaviate 实例有多种方式。这可以通过使用一个`docker-compose.yaml`文件来完成，该文件可以在这里[生成](https://www.semi.technology/developers/weaviate/current/getting-started/installation.html#customize-your-weaviate-setup)。对于此选项，您必须安装`docker`和`docker-compose`，并在您的驱动器上留出空间。

另一个选择是在 [Weaviate 云服务控制台](https://console.semi.technology/) (WCS 控制台)上创建一个帐户，并在那里创建一个集群。有不同的集群选项可供选择。如果您没有帐户，请创建一个。

在本教程中，我们将直接从 python 在 WCS 上创建一个集群(您只需要您的 WCS 凭证)。

我们现在要做的第一件事是安装 Weaviate Python 客户端。这可以使用 pip 命令来完成。

```
>>> import sys
>>> !{sys.executable} -m pip install weaviate-client==2.5.0
```

**为版本 **3.0.0** 更新**。

```
>>> import sys
>>> !{sys.executable} -m pip install weaviate-client==3.0.0
```

现在，让我们导入软件包，并在 WCS 上创建一个集群。

```
>>> from getpass import getpass # hide password
>>> import weaviate # to communicate to the Weaviate instance
>>> from weaviate.tools import WCS
```

**将**更新为 **3.0.0** 版本。

```
>>> from getpass import getpass # hide password
>>> import weaviate # to communicate to the Weaviate instance
>>> from weaviate.wcs import WCS
```

为了对 WCS 或 Weaviate 实例进行身份验证(如果 Weaviate 实例启用了身份验证)，我们需要创建一个身份验证对象。目前，它支持两种类型的认证凭证:

*   密码凭证:`weaviate.auth.AuthClientPassword(username='WCS_ACCOUNT_EMAIL', password='WCS_ACCOUNT_PASSWORD')`
*   令牌凭证`weaviate.auth.AuthClientCredentials(client_secret=YOUR_SECRET_TOKEN)`

对于 WCS，我们将使用密码凭据。

```
>>> my_credentials = weaviate.auth.AuthClientPassword(username=input("User name: "), password=getpass('Password: '))User name: WCS_ACCOUNT_EMAIL
Password: ········
```

`my_credentials`对象包含你的凭证，所以小心不要公开它。

```
>>> my_wcs = WCS(my_credentials)
```

现在我们已经连接到 WCS，我们可以使用`create`、`delete`、`get_clusters`、`get_cluster_config`和`is_ready`方法检查集群的状态。

下面是`create`方法的原型:

```
my_wcs.create(cluster_name:str=None, 
    cluster_type:str='sandbox',
    config:dict=None,
    wait_for_completion:bool=True) -> str
```

返回值是创建的集群的 URL。

**注意:** *WCS 名称必须是全局唯一的，因为它们用于创建公共 URL，以便以后访问实例。因此，请确保为* `*cluster_name*` *选择一个唯一的名称。*

*如果你想检查笔记本中任何方法的原型和 docstring，运行这个命令:* `*object.method?*` *。您也可以使用* `*help()*` *功能。*
例如:`WCS.is_ready?`或`my_wcs.is_ready?`或`help(WCS.is_ready)`。

```
>>> cluster_name = 'my-first-weaviate-instance'
>>> weaviate_url = my_wcs.create(cluster_name=cluster_name)
>>> weaviate_url100%|██████████| 100.0/100 [00:56<00:00,  1.78it/s]            
'https://my-first-weaviate-instance.semi.network'>>> my_wcs.is_ready(cluster_name)True
```

## 5.1.连接到集群。

现在我们可以用`Client`对象连接到创建的 weaviate 实例。构造函数如下所示:

```
weaviate.Client(
    url:str,
    auth_client_secret:weaviate.auth.AuthCredentials=None,
    timeout_config:Union[Tuple[int, int], NoneType]=None,
)
```

构造函数只有一个必需的参数`url`，和两个可选的参数:`auth_client_secret`——如果 weaviate 实例启用了身份验证，则使用`timeout_config`——设置 REST 超时配置，并且是一个元组(重试次数，超时秒数)。有关参数的更多信息，请查看 docstring。

```
>>> client = weaviate.Client(weaviate_url)
```

现在我们连接到了 Weavite，但这并不意味着它已经设置好了。它可能仍会在后台执行一些设置过程。我们可以通过调用`.is_live`方法来检查 Weaviate 实例的健康状况，并通过调用`.is_ready`来检查 Weaviate 是否为请求做好了准备。

```
>>> client.is_ready()True
```

## 5.2.获取数据并进行分析。

我们设置了 Weaviate 实例，连接到它并为请求做好准备，现在我们可以后退一步，获取一些数据并对其进行分析。

这一步，对于所有的机器学习模型来说，是最重要的一步。在这里，我们必须决定什么是相关的，什么是重要的，以及使用什么数据结构/类型。

在这个例子中，我们将使用新闻文章来构建数据。为此，我们需要`newspaper3k`包。

```
>>> !{sys.executable} -m pip install newspaper3k
```

**注意:如果没有下载任何文章，可能是因为没有下载 *nltk punkt* 工具。要修复它，请运行下面的单元格。**

```
>>> import nltk *# it is a dependency of newspaper3k*
>>> nltk.download('punkt')
```

> 感谢 GitHub 用户@gosha1128 在下面的`get_articles_from_newspaper`函数中发现了这个被抑制在`try/except`块中的 bug。

```
>>> import newspaper
>>> import uuid
>>> import json
>>> from tqdm import tqdm

>>> def get_articles_from_newspaper(
...         news_url: str, 
...         max_articles: int=100
...     ) -> None:
...     """
...     Download and save newspaper articles as weaviate schemas.
...     Parameters
...     ----------
...     newspaper_url : str
...         Newspaper title.
...     """
...     
...     objects = []
...     
...     # Build the actual newspaper    
...     news_builder = newspaper.build(news_url, memoize_articles=False)
...     
...     if max_articles > news_builder.size():
...         max_articles = news_builder.size()
...     pbar = tqdm(total=max_articles)
...     pbar.set_description(f"{news_url}")
...     i = 0
...     while len(objects) < max_articles and i < news_builder.size():
...         article = news_builder.articles[i]
...         try:
...             article.download()
...             article.parse()
...             article.nlp()

...             if (article.title != '' and \
...                 article.title is not None and \
...                 article.summary != '' and \
...                 article.summary is not None and\
...                 article.authors):
... 
...                 # create an UUID for the article using its URL
...                 article_id = uuid.uuid3(uuid.NAMESPACE_DNS, article.url)
... 
...                 # create the object
...                 objects.append({
...                     'id': str(article_id),
...                     'title': article.title,
...                     'summary': article.summary,
...                     'authors': article.authors
...                 })
...                 
...                 pbar.update(1)
... 
...         except:
...             # something went wrong with getting the article, ignore it
...             pass
...         i += 1
...     pbar.close()
...     return objects>>> data = []
>>> data += get_articles_from_newspaper('https://www.theguardian.com/international')
>>> data += get_articles_from_newspaper('http://cnn.com')https://www.theguardian.com/international: 100%|██████████| 100/100 [00:34<00:00,  2.90it/s]
http://cnn.com: 100%|██████████| 100/100 [02:11<00:00,  1.32s/it]
```

## 5.3.创建适当的数据类型。

在函数`get_articles_from_newspaper`中，我们保留文章的*标题、摘要*和*作者*。我们还为每篇文章计算一个 UUID(通用唯一标识符)。所有这些字段都可以在上面的单元格中看到。

有了这些信息，我们就可以定义一个模式，即每种对象类型的数据结构以及它们之间的关系。该模式是一个嵌套字典。

所以让我们创建`Article`类模式。我们知道文章有 ***标题，摘要有******作者有*** 。

关于模式以及如何创建它们的更多信息可以在[这里](https://www.semi.technology/developers/weaviate/current/data-schema/schema-configuration.html)和[这里](https://www.semi.technology/developers/weaviate/current/restful-api-references/schema.html#parameters)找到。

```
>>> article_class_schema = {
...     # name of the class
...     "class": "Article",
...     # a description of what this class represents
...     "description": "An Article class to store the article summary and its authors",
...     # class properties
...     "properties": [
...         {
...             "name": "title",
...             "dataType": ["string"],
...             "description": "The title of the article", 
...         },
...         {
...             "name": "summary",
...             "dataType": ["text"],
...             "description": "The summary of the article",
...         },
...         {
...             "name": "hasAuthors",
...             "dataType": ["Author"],
...             "description": "The authors this article has",
...         }
...     ]
... }
```

在上面的类模式中，我们创建了一个名为`Article`的类，描述为`An Article class to store the article summary and its authors`。描述是为了向用户解释这个类是关于什么的。

我们还定义了 3 个属性:`title` -文章的标题，类型为`string`(区分大小写)，`summary` -文章的摘要，数据类型为`text`(不区分大小写)，`hasAuthor` -文章的作者，数据类型为`Author`。`Author`不是原始数据类型，它是我们应该定义的另一个类。原始数据类型的列表可以在这里找到[。](https://www.semi.technology/developers/weaviate/current/data-schema/datatypes.html)

**注 1:** 属性应该总是 cameCase 格式，并且以小写字母开头。
**注 2:** 属性数据类型始终是列表，因为它可以接受多种数据类型。

将另一个类指定为数据类型称为交叉引用。这样，您可以将数据对象链接在它们之间，并创建一个关系图。

现在让我们以同样的方式创建`Author`类模式，但是使用属性`name`和`wroteArticles`。

```
>>> author_class_schema = {
...     "class": "Author",
...     "description": "An Author class to store the author information",
...     "properties": [
...         {
...             "name": "name",
...             "dataType": ["string"],
...             "description": "The name of the author", 
...         },
...         {
...             "name": "wroteArticles",
...             "dataType": ["Article"],
...             "description": "The articles of the author", 
...         }
...     ]
... }
```

既然我们决定了数据结构，我们可以告诉 Weaviate 我们将导入什么类型的数据。这可以通过访问客户端的`schema`属性来完成。

可以通过两种不同的方式创建模式:

1.  使用`.create_class()`方法，这个选项每次调用只创建一个类。
2.  使用`.create()`方法，这个选项一次创建多个类(如果您有完整的模式，这很有用)

我们还可以用`.contains()`方法检查模式是否存在，或者特定的类模式是否存在。

更多关于模式方法的信息，点击[这里](https://www.semi.technology/developers/weaviate/current/restful-api-references/schema.html)。

因为我们分别定义了每个类，所以我们应该使用`.create_class()`方法。

```
client.schema.create_class(schema_class:Union[dict, str]) -> None
```

它还接受指向类定义文件的文件路径或 URL。

```
>>> client.schema.create_class(article_class_schema)---------------------------------------------------------------------------

UnexpectedStatusCodeException             Traceback (most recent call last)

<ipython-input-12-6d56a74d9293> in <module>
----> 1 client.schema.create_class(article_class_schema)

~/miniconda3/envs/test/lib/python3.6/site-packages/weaviate/schema/crud_schema.py in create_class(self, schema_class)
    138         check_class(loaded_schema_class)
    139         self._create_class_with_premitives(loaded_schema_class)
--> 140         self._create_complex_properties_from_class(loaded_schema_class)
    141 
    142     def delete_class(self, class_name: str) -> None:

~/miniconda3/envs/test/lib/python3.6/site-packages/weaviate/schema/crud_schema.py in _create_complex_properties_from_class(self, schema_class)
    352                 raise type(conn_err)(message).with_traceback(sys.exc_info()[2])
    353             if response.status_code != 200:
--> 354                 raise UnexpectedStatusCodeException("Add properties to classes", response)
    355 
    356     def _create_complex_properties_from_classes(self, schema_classes_list: list) -> None:

UnexpectedStatusCodeException: Add properties to classes! Unexpected status code: 422, with response body: {'error': [{'message': "Data type of property 'hasAuthors' is invalid; SingleRef class name 'Author' does not exist"}]}
```

正如我们所看到的，我们不能创建引用不存在的数据类型的 class 属性。这并不意味着根本没有创建类`Article`。让我们从 weaviate 获取模式，看看创建了什么。

```
>>> # helper function
>>> def prettify(json_dict): 
...     print(json.dumps(json_dict, indent=2))>>> prettify(client.schema.get()){
  "classes": [
    {
      "class": "Article",
      "description": "An Article class to store the article summary and its authors",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The title of the article",
          "name": "title"
        },
        {
          "dataType": [
            "text"
          ],
          "description": "The summary of the article",
          "name": "summary"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    }
  ]
}
```

我们没有指定的配置不是强制性的，而是被设置为默认值。

正如我们所看到的，只有`hasAuthor`属性没有被创建。所以让我们创建`Author`类。

```
>>> client.schema.create_class(author_class_schema)
>>> prettify(client.schema.get()){
  "classes": [
    {
      "class": "Article",
      "description": "An Article class to store the article summary and its authors",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The title of the article",
          "name": "title"
        },
        {
          "dataType": [
            "text"
          ],
          "description": "The summary of the article",
          "name": "summary"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    },
    {
      "class": "Author",
      "description": "An Author class to store the author information",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The name of the author",
          "name": "name"
        },
        {
          "dataType": [
            "Article"
          ],
          "description": "The articles of the author",
          "name": "wroteArticles"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    }
  ]
}
```

现在我们已经创建了两个类，但是我们仍然没有`hasAuthor`属性。不用担心，它可以在任何时候创建，使用模式的属性`property`和方法`create`。

```
client.schema.property.create(schema_class_name:str, schema_property:dict) -> None>>> client.schema.property.create('Article', article_class_schema['properties'][2])
```

现在让我们得到模式，看看它是否是我们所期望的。

```
>>> prettify(client.schema.get()){
  "classes": [
    {
      "class": "Article",
      "description": "An Article class to store the article summary and its authors",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The title of the article",
          "name": "title"
        },
        {
          "dataType": [
            "text"
          ],
          "description": "The summary of the article",
          "name": "summary"
        },
        {
          "dataType": [
            "Author"
          ],
          "description": "The authors this article has",
          "name": "hasAuthors"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    },
    {
      "class": "Author",
      "description": "An Author class to store the author information",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The name of the author",
          "name": "name"
        },
        {
          "dataType": [
            "Article"
          ],
          "description": "The articles of the author",
          "name": "wroteArticles"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    }
  ]
}
```

一切都如我们所愿。

如果您不想考虑哪个类是在何时创建的，以及哪些属性可能失败或不失败(由于尚不存在的类)，有一个解决方案。解决方案是用`create`方法创建整个模式。所以让我们从 weaviate 中删除这个模式，看看它是如何工作的。

```
>>> schema = client.schema.get() # save schema
>>> client.schema.delete_all() # delete all classes
>>> prettify(client.schema.get()){
  "classes": []
}
```

*注意，如果我们删除一个模式或一个类，我们会删除与之相关的所有对象。*

现在让我们从保存的模式中创建它。

```
>>> client.schema.create(schema)
>>> prettify(client.schema.get()){
  "classes": [
    {
      "class": "Article",
      "description": "An Article class to store the article summary and its authors",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The title of the article",
          "name": "title"
        },
        {
          "dataType": [
            "text"
          ],
          "description": "The summary of the article",
          "name": "summary"
        },
        {
          "dataType": [
            "Author"
          ],
          "description": "The authors this article has",
          "name": "hasAuthors"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    },
    {
      "class": "Author",
      "description": "An Author class to store the author information",
      "invertedIndexConfig": {
        "cleanupIntervalSeconds": 60
      },
      "properties": [
        {
          "dataType": [
            "string"
          ],
          "description": "The name of the author",
          "name": "name"
        },
        {
          "dataType": [
            "Article"
          ],
          "description": "The articles of the author",
          "name": "wroteArticles"
        }
      ],
      "vectorIndexConfig": {
        "cleanupIntervalSeconds": 300,
        "maxConnections": 64,
        "efConstruction": 128,
        "vectorCacheMaxObjects": 500000
      },
      "vectorIndexType": "hnsw",
      "vectorizer": "text2vec-contextionary"
    }
  ]
}
```

这看起来和我们一个类一个类一个属性地创建的模式一模一样。这样，我们现在可以将模式保存在一个文件中，并在下一个会话中通过提供文件路径直接导入它。

```
# save schema to file
with open('schema.json', 'w') as outfile: 
    json.dump(schema, outfile)
# remove current schema from Weaviate, removes all the data too
client.schema.delete_all()
# import schema using file path
client.schema.create('schema.json')
# print schema
print(json.dumps(client.schema.get(), indent=2))
```

## 5.4.加载数据。

现在我们已经准备好了数据，Weaviate 也知道我们拥有什么样的数据，我们可以将`Articles`和`Authors`添加到 Weaviate 实例中。

可以通过三种不同的方式导入数据。

1.  逐个对象地迭代添加对象。这可以使用客户端的`data_object`对象属性来完成。
2.  分批。这可以通过创建一个适当的批处理请求对象并使用客户机的`batch`对象属性提交它来完成。(**仅在** `weaviate-client` **版本< 3.0.0 中。**
3.  使用来自`weaviate.tools`模块的`Batcher`对象。(**仅在** `weaviate-client` **版本< 3.0.0 中出现。**)
4.  **新的** `Batch` **类在 weaviate-client 版本 3.0.0 中引入。**

我们将看到它们的实际应用，但首先让我们强调一下它们之间的区别。

*   备选方案 1。是添加数据对象和创建引用的最安全的方法，因为它在创建对象之前进行对象验证，而批量导入数据会跳过大部分验证以提高速度。这个选项要求每个对象有一个 REST 请求，因此比成批导入数据要慢。如果您不确定您的数据是否有效，建议使用此选项。
*   选项 2。如上所述，跳过了大部分数据验证，每批只需要一个 REST 请求。对于这种方法，您只需向一个批处理请求中添加尽可能多的数据(有两种类型:`ReferenceBatchRequest`和`ObjectsBatchRequest`)，然后使用客户端的`batch`对象属性提交它。此选项要求您首先导入数据对象，然后导入引用(确保引用中使用的对象在创建引用之前已经导入)。(**仅在** `weaviate-client` **版本< 3.0.0 中。**)
*   选项 3。依赖于来自 2 的批处理请求。但是对于一个`Batcher`，你不需要提交任何批量请求，当它满了的时候，它会自动为你做。(**只出现在** `weaviate-client` **版本< 3.0.0 中。**)
*   选项 4: **新的** `Batch` **类在 weaviate-client 版本 3.0.0 中引入。**新的`Batch`对象不需要来自 2 的`BatchRequests`。而是在内部使用。新类还支持 3 种不同的批量加载数据的情况:a)手动——用户可以完全控制何时以及如何添加和创建批量；b)满时自动创建批次；c)使用动态批处理自动创建批次，即每次创建批次时调整批次大小，以避免任何`Timeout`错误。

## 5.4.1 使用`data_object`属性加载数据

在这种情况下，我们只取一篇文章(`data[0]`)并使用`data_object`属性将其导入 Weaviate。

方法是首先创建对象，然后创建链接它们的引用。

在笔记本中运行`client.data_object.create?`以获得关于该方法的更多信息。或`help(client.data_object.create)`处于空转状态。

每个数据对象都应该具有模式中定义的相同格式。

```
>>> prettify(data[0]){
  "id": "df2a2d1c-9c87-3b4b-9df3-d7aed6bb6a27",
  "title": "Coronavirus live news: Pfizer says jab 100% effective in 12- to 15-year-olds; Macron could announce lockdown tonight",
  "summary": "11:08Surge testing is being deployed in Bolton, Greater Manchester after one case of the South African variant of coronavirus has been identified.\nDr Helen Lowey, Bolton\u2019s director of public health, said that \u201cthe risk of any onward spread is low\u201d and there was no evidence that the variant caused more severe illness.\nPublic Health England identified the case in the area of Wingates Industrial Estate.\nDr Matthieu Pegorie from Public Health England North West said that there was no link to international travel, therefore suggesting that there are some cases in the community.\nThe Department of Health says that enhanced contact tracing will be deployed where a positive case of a variant is found.",
  "authors": [
    "Helen Sullivan",
    "Yohannes Lowe",
    "Martin Belam",
    "Maya Wolfe-Robinson",
    "Melissa Davey",
    "Jessica Glenza",
    "Jon Henley",
    "Peter Beaumont"
  ]
}>>> article_object = {
...     'title': data[0]['title'],
...     'summary': data[0]['summary'].replace('\n', '') # remove newline character
...     # we leave out the `hasAuthors` because it is a reference and will be created after we create the Authors
... }
>>> article_id = data[0]['id']

>>> # validated the object
>>> result = client.data_object.validate(
...     data_object=article_object,
...     class_name='Article',
...     uuid=article_id
... )

>>> prettify(result){
  "error": null,
  "valid": true
}
```

对象通过了验证测试，现在可以安全地创建/导入它了。

```
>>> # create the object
>>> client.data_object.create(
...     data_object=article_object,
...     class_name='Article',
...     uuid=article_id # if not specified, weaviate is going to create an UUID for you.
...)'df2a2d1c-9c87-3b4b-9df3-d7aed6bb6a27'
```

`client.data_object.create`返回对象的 UUID，如果你指定了一个，它也将被返回。如果你没有指定，Weaviate 会为你生成一个并返回。

恭喜我们已经添加了第一个 weaviate 对象！！！

现在我们实际上可以使用`get_by_id`或`get`方法通过 UUID 从 Weaviate 中“获取”这个对象。(`get`未指定和 UUID 返回前 100 个对象)

```
>>> prettify(client.data_object.get(article_id, with_vector=False)){
  "additional": {},
  "class": "Article",
  "creationTimeUnix": 1617191563170,
  "id": "df2a2d1c-9c87-3b4b-9df3-d7aed6bb6a27",
  "lastUpdateTimeUnix": 1617191563170,
  "properties": {
    "summary": "11:08Surge testing is being deployed in Bolton, Greater Manchester after one case of the South African variant of coronavirus has been identified.Dr Helen Lowey, Bolton\u2019s director of public health, said that \u201cthe risk of any onward spread is low\u201d and there was no evidence that the variant caused more severe illness.Public Health England identified the case in the area of Wingates Industrial Estate.Dr Matthieu Pegorie from Public Health England North West said that there was no link to international travel, therefore suggesting that there are some cases in the community.The Department of Health says that enhanced contact tracing will be deployed where a positive case of a variant is found.",
    "title": "Coronavirus live news: Pfizer says jab 100% effective in 12- to 15-year-olds; Macron could announce lockdown tonight"
  },
  "vectorWeights": null
}
```

现在让我们创建作者以及`Article`和`Authors`之间的交叉引用。

添加参考的方式相同，但添加参考时使用`client.data_object.reference.add`方法。

```
>>> # keep track of the authors already imported/created and their respective UUID
>>> # because same author can write more than one paper.
>>> created_authors = {}

>>> for author in data[0]['authors']:
...     # create Author
...     author_object = {
...         'name': author,
...         # we leave out the `wroteArticles` because it is a reference and will be created after we create the Author
...     }
...     author_id = client.data_object.create(
...         data_object=author_object,
...         class_name='Author'
...     )
...     
...     # add author to the created_authors
...     created_authors[author] = author_id
...     
...     # add references
...     ## Author -> Article
...     client.data_object.reference.add(
...         from_uuid=author_id,
...         from_property_name='wroteArticles',
...         to_uuid=article_id
...     )
...     ## Article -> Author 
...     client.data_object.reference.add(
...         from_uuid=article_id,
...         from_property_name='hasAuthors',
...         to_uuid=author_id
...     )
```

在上面的单元格中，我们遍历了文章的所有作者。对于每次迭代，我们首先创建`Author`，然后添加引用:从`Author`到`Article`的引用——通过`Author`的`wroteArticles`属性链接，以及从`Article`到`Author`的引用——通过`Article`的`hasAuthors`属性。

请注意，双向引用不是必需的。

现在让我们得到这个物体，并看看它。

```
>>> prettify(client.data_object.get(article_id, with_vector=False)){
  "additional": {},
  "class": "Article",
  "creationTimeUnix": 1617191563170,
  "id": "df2a2d1c-9c87-3b4b-9df3-d7aed6bb6a27",
  "lastUpdateTimeUnix": 1617191563170,
  "properties": {
    "hasAuthors": [
      {
        "beacon": "weaviate://localhost/1d0d3242-1fc2-4bba-adbe-ab9ef9a97dfe",
        "href": "/v1/objects/1d0d3242-1fc2-4bba-adbe-ab9ef9a97dfe"
      },
      {
        "beacon": "weaviate://localhost/c1c8afce-adb6-4b3c-bbe0-2414d55b0c8e",
        "href": "/v1/objects/c1c8afce-adb6-4b3c-bbe0-2414d55b0c8e"
      },
      {
        "beacon": "weaviate://localhost/b851f6fc-a02b-4a63-9b53-c3a8764c82c1",
        "href": "/v1/objects/b851f6fc-a02b-4a63-9b53-c3a8764c82c1"
      },
      {
        "beacon": "weaviate://localhost/e6b6c991-5d7a-447f-89c8-e6e01730f88f",
        "href": "/v1/objects/e6b6c991-5d7a-447f-89c8-e6e01730f88f"
      },
      {
        "beacon": "weaviate://localhost/d03f9353-d4fc-465d-babe-f116a29ccaf5",
        "href": "/v1/objects/d03f9353-d4fc-465d-babe-f116a29ccaf5"
      },
      {
        "beacon": "weaviate://localhost/8ab84df5-c92b-49ac-95dd-bf65f53a38cc",
        "href": "/v1/objects/8ab84df5-c92b-49ac-95dd-bf65f53a38cc"
      },
      {
        "beacon": "weaviate://localhost/e667d7c9-0c9b-48fe-b671-864cbfc84962",
        "href": "/v1/objects/e667d7c9-0c9b-48fe-b671-864cbfc84962"
      },
      {
        "beacon": "weaviate://localhost/9d094f60-3f58-46dc-b7fd-40495be2dd69",
        "href": "/v1/objects/9d094f60-3f58-46dc-b7fd-40495be2dd69"
      }
    ],
    "summary": "11:08Surge testing is being deployed in Bolton, Greater Manchester after one case of the South African variant of coronavirus has been identified.Dr Helen Lowey, Bolton\u2019s director of public health, said that \u201cthe risk of any onward spread is low\u201d and there was no evidence that the variant caused more severe illness.Public Health England identified the case in the area of Wingates Industrial Estate.Dr Matthieu Pegorie from Public Health England North West said that there was no link to international travel, therefore suggesting that there are some cases in the community.The Department of Health says that enhanced contact tracing will be deployed where a positive case of a variant is found.",
    "title": "Coronavirus live news: Pfizer says jab 100% effective in 12- to 15-year-olds; Macron could announce lockdown tonight"
  },
  "vectorWeights": null
}
```

如我们所见，我们将参考设置为`beacon`和`href`。我们无法通过从 weaviate 获取对象来查看数据。我们可以通过*查询*数据来完成(参见 **5.5 节查询数据)。**)或通过 UUID(或`beacon`，或`href`)获取对象。

```
>> from weaviate.util import get_valid_uuid # extract UUID from URL (beacon or href)

>>> # extract authors references, lets take only the first one as an example (the article might have only one)
>>> author = client.data_object.get(article_id, with_vector=False)['properties']['hasAuthors'][0]

>>> # get and print data object by providing the 'beacon'
>>> author_uuid = get_valid_uuid(author['beacon']) # can be 'href' too
>>> prettify(client.data_object.get(author_uuid, with_vector=False)){
  "additional": {},
  "class": "Author",
  "creationTimeUnix": 1617191569894,
  "id": "1d0d3242-1fc2-4bba-adbe-ab9ef9a97dfe",
  "lastUpdateTimeUnix": 1617191569894,
  "properties": {
    "name": "Helen Sullivan",
    "wroteArticles": [
      {
        "beacon": "weaviate://localhost/df2a2d1c-9c87-3b4b-9df3-d7aed6bb6a27",
        "href": "/v1/objects/df2a2d1c-9c87-3b4b-9df3-d7aed6bb6a27"
      }
    ]
  },
  "vectorWeights": null
}
```

到目前为止，还不错(…那又怎样！)

数据对象(`client.data_object`)的方法更多:`.delete`、`.exists`、`.replace`、`.update`。

也有一些引用的方法(`client.data_object.reference` ): `.add`、`.delete`、`.update`。

## 5.4.2 使用批处理加载数据

(**只在** `weaviate-client` **版本< 3.0.0 中。**)

批量导入数据与逐个对象添加非常相似。

我们要做的第一件事是为每个对象类型创建一个`BatchRequest`对象:`DataObject`和`Reference`。它们被相应地命名为:`ObjectsBatchRequest`和`ReferenceBatchRequest`。

让我们为每一批创建一个对象，并导入接下来的 99 篇文章。

**注意:**我想再次提醒您，批量导入/创建数据会跳过一些验证步骤，可能会导致图形损坏。

```
>>> from weaviate import ObjectsBatchRequest, ReferenceBatchRequest
```

让我们创建一个向批处理请求添加单个文章的函数。

```
>>> def add_article(batch: ObjectsBatchRequest, article_data: dict) -> str:
...     
...     article_object = {
...         'title': article_data['title'],
...         'summary': article_data['summary'].replace('\n', '') # remove newline character
...     }
...     article_id = article_data['id']
...     
...     # add article to the object batch request
...     batch.add(
...         data_object=article_object,
...         class_name='Article',
...         uuid=article_id
...     )
...     
...     return article_id
```

现在让我们创建一个函数，向批处理请求添加一个作者，如果还没有创建作者的话。

```
>>> def add_author(batch: ObjectsBatchRequest, author_name: str, created_authors: dict) -> str:
...     
...     if author_name in created_authors:
...         # return author UUID
...         return created_authors[author_name]
...     
...     # generate an UUID for the Author
...     author_id = generate_uuid(author)
...     
...     # add author to the object batch request
...     batch.add(
...         data_object={'name': author_name},
...         class_name='Author',
...         uuid=author_id
...     )
...     
...     created_authors[author_name] = author_id
...     return author_id
```

最后一个功能是添加交叉引用。

```
>>> def add_references(batch: ReferenceBatchRequest, article_id: str, author_id: str)-> None:
...     # add references to the reference batch request
...     ## Author -> Article
...     batch.add(
...         from_object_uuid=author_id,
...         from_object_class_name='Author',
...         from_property_name='wroteArticles',
...         to_object_uuid=article_id
...     )
...     
...     ## Article -> Author 
...     batch.add(
...         from_object_uuid=article_id,
...         from_object_class_name='Article',
...         from_property_name='hasAuthors',
...         to_object_uuid=author_id
...     )
```

现在我们可以遍历数据并使用批处理导入数据。

```
>>> from weaviate.tools import generate_uuid
>>> from tqdm import trange

>>> objects_batch = ObjectsBatchRequest()
>>> reference_batch = ReferenceBatchRequest()

>>> for i in trange(1, 100):
...    
...     # add article to batch request
...     article_id = add_article(objects_batch, data[i])
...     
...     for author in data[i]['authors']:
...         
...         # add author to batch request
...         author_id = add_author(objects_batch, author, created_authors)
...         
...         # add cross references to the reference batch
...         add_references(reference_batch, article_id=article_id, author_id=author_id)
...     
...     if i % 20 == 0:
...         # submit the object batch request to weaviate, can be done with method '.create_objects'
...         client.batch.create(objects_batch)
...        
...         # submit the reference batch request to weaviate, can be done with method '.create_references'
...         client.batch.create(reference_batch)
...        
...         # batch requests are not reusable, so we create new ones
...         objects_batch = ObjectsBatchRequest()
...         reference_batch = ReferenceBatchRequest()

>>> # submit the any object that are left
>>> status_objects = client.batch.create(objects_batch)
>>> status_references = client.batch.create(reference_batch)0%|          | 0/99 [00:00<?, ?it/s]
```

为了批量导入数据，我们应该为想要导入的数据对象类型创建一个`BatchRequest`对象。批处理请求对象没有大小限制，因此您应该在需要多少对象时提交它。(请记住，如果您将使用包含太多对象的批处理，可能会导致超时错误，因此请将它保持在合理的大小，以便您的 Weaviate 实例可以处理它。)此外，我们会跟踪已经创建的作者，这样我们就不会一遍又一遍地创建同一个作者。

对`client.batch.create`的调用返回每个被创建对象的状态。如果你想确定一切正常，就检查一下。另外**注意**即使 Weaviate 未能创建对象，也不意味着批量提交也失败，更多信息请阅读`client.batch.create`的文档。

## 5.4.3 使用批处理程序对象加载数据。

(**仅在** `weaviate-client` **版本< 3.0.0 中出现。**)

`Batcher`是一个自动提交对象 weaviate 的类，包括`DataObject`和`Reference`s。`Batcher`可以在`weaviate.tools`模块中找到，它有以下构造器原型:

```
Batcher(
    client : weaviate.client.Client,
    batch_size : int=512,
    verbose : bool=False,
    auto_commit_timeout : float=-1.0,
    max_backoff_time : int=300,
    max_request_retries : int=4,
    return_values_callback : Callable=None,
)
```

有关每个参数的解释，请参见文档。

让我们看看它是如何对我们从`data`中提取的其余对象起作用的。

```
>>> from weaviate.tools import Batcher
```

对于 a `Batcher`,我们只需要将我们想要导入的对象添加到 Weaviate。`Batcher`有添加`objects` ( `batcher.add_data_object`)和添加`references` ( `batcher.add_reference`)的特殊方法。此外，它还提供了一个带有关键字参数的`batcher.add`方法，该方法可以检测您试图添加的数据类型。`batcher.add`方法使得重用我们上面定义的`add_article`、`add_author`和`add_references`函数成为可能。

**注:**`Batcher.add`是在`weaviate-client`版本 2.3.0 中引入的。

让我们使用批处理程序添加来自`data`的剩余文章和作者。因为`Batcher`自动提交对象进行 weaviate，所以我们需要在我们完成之后总是`.close()`它，以确保我们提交的是`Batcher`中剩余的内容。

如果你像我一样，有时忘记关闭对象，`Bather`可以在上下文管理器中使用，即与`with`一起使用。让我们看看它是如何与上下文管理器一起工作的。

```
>>> # we still need the 'created_authors' so we do not add the same author twice
>>> with Batcher(client, 30, True) as batcher:
...     for i in trange(100, 200):
...         
...         # add article to batcher
...         article_id = add_article(batcher, data[i]) # NOTE the 'bather' object instead of 'objects_batch'
... 
...         for author in data[i]['authors']:
...
...             # add author to batcher
...             author_id = add_author(batcher, author, created_authors) # NOTE the 'bather' object instead of 'objects_batch'
...
...             # add cross references to the batcher
...             add_references(batcher, article_id=article_id, author_id=author_id) # NOTE the 'bather' object instead of 'reference_batch'Batcher object created!

  0%|          | 0/100 [00:00<?, ?it/s]

Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Updated object batch successfully
Updated reference batch successfully
Batcher object closed!
```

这就是分批配料器。

这是导入数据的三种方式。选择一个适合你和你的项目。

## 5.4.4 新增`Batch`对象

(**只在** `weaviate-client` **版本> =3.0.0。**)

新的`Batch`对象可以用同样的方式访问:`client.batch`。如 5.4 节所述，这个类有 3 种不同的用法，所以让我们来看看我们到底是怎么做的，以及它们之间的区别。

我们首先要做的是重新定义以下函数:`add_article`、`add_author`和`add_references`。

```
>>> from weaviate.batch import Batch # for the typing purposes
>>> from weaviate.util import generate_uuid5 # old way was from weaviate.tools import generate_uuid>>> def add_article(batch: Batch, article_data: dict) -> str:
...    
...     article_object = {
...         'title': article_data['title'],
...         'summary': article_data['summary'].replace('\n', '') # remove newline character
...     }
...     article_id = article_data['id']
...    
...    # add article to the object batch request
...   batch.add_data_object(  # old way was batch.add(...)
...        data_object=article_object,
...        class_name='Article',
...        uuid=article_id
...    )
...    
...    return article_id>>> def add_author(batch: Batch, author_name: str, created_authors: dict) -> str:
...    
...    if author_name in created_authors:
...        # return author UUID
...        return created_authors[author_name]
...    
...    # generate an UUID for the Author
...    author_id = generate_uuid5(author)
...    
...    # add author to the object batch request
...    batch.add_data_object(  # old way was batch.add(...)
...        data_object={'name': author_name},
...        class_name='Author',
...        uuid=author_id
...    )
...    
...    created_authors[author_name] = author_id
...    return author_id>>> def add_references(batch: Batch, article_id: str, author_id: str)-> None:
...    # add references to the reference batch request
...    ## Author -> Article
...    batch.add_reference(  # old way was batch.add(...)
...        from_object_uuid=author_id,
...        from_object_class_name='Author',
...        from_property_name='wroteArticles',
...        to_object_uuid=article_id
...    )
...    
...    ## Article -> Author 
...    batch.add_reference(  # old way was batch.add(...)
...        from_object_uuid=article_id,
...        from_object_class_name='Article',
...        from_property_name='hasAuthors',
...        to_object_uuid=author_id
...    )
```

现在我们已经修改了上面的函数，使之与新的`Batch`对象兼容，让我们来看看它们的运行情况。

## a)手动

这种方法使用户可以完全控制何时以及如何添加和创建批处理。它非常类似于`weaviate-client`版本< 3.0.0 的`BatchRequests`方法，所以让我们来看看我们到底如何使用它(或者从旧版本迁移到它)。

参见下面代码单元中相同方法的上下文管理器方式。(**不要同时运行两个单元！**

```
>>> from tqdm import trange>>> for i in trange(1, 100):
...     
...    # add article to the batch
...    article_id = add_article(client.batch, data[i])
...    
...    for author in data[i]['authors']:
...        
...        # add author to the batch
...        author_id = add_author(client.batch, author, created_authors)
...        
...        # add cross references to the batch
...        add_references(client.batch, article_id=article_id, author_id=author_id)
...    
...    if i % 20 == 0:
...        # submit the objects from the batch to weaviate
...        client.batch.create_objects()
...        
...        # submit the references from the batch to weaviate
...        client.batch.create_references()>>> # submit any objects that are left
>>> status_objects = client.batch.create_objects()
>>> status_references = client.batch.create_references()
>>> # if there is no need for the output from batch creation, one could flush both
>>> # object and references with one call
>>> client.batch.flush()
```

或者，我们可以使用带有上下文管理器的`Batch`实例，当它存在于上下文中时，就会调用`flush()`方法。

```
>>> from tqdm import trange>>> with client.batch as batch:
...     for i in trange(1, 100):
...        
...        # add article to the batch
...        article_id = add_article(batch, data[i])
...        
...        for author in data[i]['authors']:
...            
...            # add author to the batch
...            author_id = add_author(batch, author, created_authors)
...            
...            # add cross references to the batch
...            add_references(batch, article_id=article_id, author_id=author_id)
...        
...        if i % 20 == 0:
...            # submit the objects from the batch to weaviate
...            batch.create_objects()
...            
...            # submit the reference from the batch to weaviate
...            batch.create_references()
```

## b)满时自动创建批次

该方法类似于`weaviate-client`版本< 3.0.0 的`Batcher`对象。让我们看看它是如何工作的。

```
>>> # we still need the 'created_authors' so we do not add the same author twice
>>> client.batch.configure(
...     batch_size=30,
...    callback=None, # use this argument to set a callback function on the batch creation results
...)
>>> for i in trange(100, 200):
...    
...    # add article to the batch
...    article_id = add_article(client.batch, data[i])...    for author in data[i]['authors']:...        # add author to the batch
...        author_id = add_author(client.batch, author,                             created_authors)...        # add cross references to the batch
...        add_references(client.batch, article_id=article_id, author_id=author_id)
>>> client.batch.flush()
```

当然，我们也可以在这里使用上下文管理器。

```
>>> # we still need the 'created_authors' so we do not add the same author twice
>>> client.batch.configure(
...     batch_size=30,
...    callback=None, # use this argument to set a callback function on the batch creation results
... )
>>> with client.batch(batch_size=30) as batch: # the client.batch(batch_size=30) is the same as client.batch.configure(batch_size=30)
...     for i in trange(100, 200):...        # add article to the batch
...        article_id = add_article(batch, data[i])...        for author in data[i]['authors']:...            # add author to the batch
...            author_id = add_author(batch, author, created_authors)...            # add cross references to the batch
...            add_references(batch, article_id=article_id, author_id=author_id)
```

## c)使用动态批处理自动创建批次，即每次创建批次时都会调整批次大小，以避免任何`Timeout`错误。

该方法的工作方式与 b)中描述的方法相同。我们不会用它来运行任何单元格，但是我要提到的是，要启用动态批处理，我们需要做的就是为`configure` / `__call__`方法提供另一个参数。

示例:

```
client.batch.configure(
    batch_size**=**30,
    dynamic**=True**
)
```

要查看这个新的`Batch`对象的全部功能，请参见完整的文档[这里的](https://weaviate-python-client.readthedocs.io/en/latest/weaviate.batch.html#weaviate.batch.Batch)或者对`Batch`或/和任何`Batch`方法执行`help`功能，就像这样:`help(Batch)`

## 5.5.查询数据。

现在我们已经导入了数据，并准备好进行查询。可以使用客户端对象的`query`属性(`client.query`)来查询数据。

使用 GraphQL 语法查询数据，可以通过三种不同的方式完成:

*   **GET** :从 Weaviate 获取对象的查询。更多信息[此处](https://www.semi.technology/developers/weaviate/current/graphql-references/get.html)使用`client.query.get(class_name, properties).OTHER_OPTIONAL_FILTERS.do()`
*   **聚合**:聚合数据的查询。更多信息[此处](https://www.semi.technology/developers/weaviate/current/graphql-references/aggregate.html)使用`client.query.aggregate(class_name, properties).OTHER_OPTIONAL_FILTERS.do()`
*   或者使用表示为`str`的 GraphQL 查询。
    使用`client.query.raw()`

**注意:**`.get`和`.aggregate`都需要调用`.do()`方法来运行查询。`.raw()`不会。

现在让我们只获取文章对象及其相应的标题。

## 获得

```
>>> result = client.query.get(class_name='Article', properties="title")\
...     .do()
>>> print(f"Number of articles returned: {len(result['data']['Get']['Article'])}")
>>> resultNumber of articles returned: 100

{'data': {'Get': {'Article': [{'title': "The soft power impact of Ruth Bader Ginsburg's decorative collars"},
    {'title': 'After centuries in the ground, these French oaks will soon form part of the new spire at Notre Dame'},
    {'title': 'With tradition and new tech, these Japanese designers are crafting more sustainably made clothing'},
    {'title': "LEGO won't make modern war machines, but others are picking up the pieces"},
    {'title': 'Remember when Jane Fonda revolutionized exercise in a leotard and leg warmers?'},
    {'title': 'Be brave, Taylor tells Manchester City Women before Barcelona return leg'},
    {'title': "'In the middle of a war zone': thousands flee as Venezuela troops and Colombia rebels clash"},
    {'title': "Destruction of world's forests increased sharply in 2020"},
    {'title': "Climate crisis 'likely cause' of early cherry blossom in Japan"},
    {'title': "What's in a vaccine and what does it do to your body?"},
    {'title': 'Zunar and Fahmi Reza: the cartoonists who helped take down Najib Razak'},
    {'title': 'Downing Street suggests UK should be seen as model of racial equality'},
    {'title': "'It's hard, we're neighbours': the coalmine polluting friendships on Poland's borders"},
    {'title': 'Why we are all attracted to conspiracy theories – video'},
    {'title': 'UK criticised for ignoring Paris climate goals in infrastructure decisions'},
    {'title': 'GameStop raids Amazon for another heavy-hitter exec'},
    {'title': 'The World Economic Forum says it will take an extra 36 years to close the gender gap'},
    {'title': 'Share a story with the Guardian'},
    {'title': "Paddleboarding and a released alligator: Tuesday's best photos"},
    {'title': "Ballerina Chloé Lopes Gomes alleged racism at her company. Now she says it's time for change"},
    {'title': 'How ancient Egyptian cosmetics influenced our beauty rituals'},
    {'title': 'Why is Australia trying to regulate Google and Facebook – video explainer'},
    {'title': 'Back in the swing and the swim: England returns to outdoor sport – in pictures'},
    {'title': "Biden's tariffs threat shows how far Brexit Britain is from controlling its own destiny"},
    {'title': 'Our cities may never look the same again after the pandemic'},
    {'title': "World's first digital NFT house sells for $500,000"},
    {'title': "The untold story of Ann Lowe, the Black designer behind Jackie Kennedy's wedding dress"},
    {'title': 'Deliveroo shares slump on stock market debut'},
    {'title': 'Hundreds of people missing after fire in Rohingya refugee camp in Bangladesh – video'},
    {'title': "Why Beijing's Serpentine Pavilion signals a new age for Chinese architecture"},
    {'title': 'My Brother’s Keeper: a former Guantánamo detainee, his guard and their unlikely friendship - video'},
    {'title': 'Has your family been affected by the Brazilian or South African variants of Covid-19?'},
    {'title': 'New Zealand raises minimum wage and increases taxes on the rich'},
    {'title': 'Deliveroo shares plunge on market debut - business live'},
    {'title': "Seoul's burgeoning drag scene confronts conservative attitudes"},
    {'title': "David Hockney at 80: An encounter with the world's most popular artist"},
    {'title': 'Una avanzada licuadora Nutribullet al mejor precio'},
    {'title': "The 'fox eye' beauty trend continues to spread online. But critics insist it's racist"},
    {'title': 'Lupita: the indigenous activist leading a new generation of Mexican women – video'},
    {'title': "Hong Kong's vast $3.8 billion rain-tunnel network"},
    {'title': 'See how tattoo art has changed since the 18th century'},
    {'title': "Hong Kong Disneyland's new castle is an architectural vision of diversity"},
    {'title': 'Rosamund Pike in "I Care a Lot" and six more recommendations if you love an antiheroine'},
    {'title': 'How NFTs are fueling a digital art boom'},
    {'title': "'We’re not little kids': leading agents ready for war with Fifa over new rules"},
    {'title': 'A photographic history of men in love'},
    {'title': "Hedge fund meltdown: Elizabeth Warren suggests regulators should've seen it coming"},
    {'title': 'Palau to welcome first tourists in a year with presidential escort'},
    {'title': 'Coronavirus: how wealthy nations are creating a ‘vaccine apartheid’'},
    {'title': 'UK economy poised to recover after Covid-19 second wave'},
    {'title': 'Missed it by that much: Australia falls 3.4m doses short of 4m vaccination target by end of March'},
    {'title': "Meet North Korea's art dealer to the West"},
    {'title': 'Why Australia remains confident in AstraZeneca vaccine as two countries put rollout on ice'},
    {'title': 'Exclusive: Jamie Dimon speaks out on voting rights even as many CEOs remain silent'},
    {'title': "Green investing 'is definitely not going to work’, says ex-BlackRock executive"},
    {'title': 'European commission says AstraZeneca not obliged to prioritise vaccines for UK'},
    {'title': '‘Honey, I forgot to duck’: the attempt to assassinate Ronald Reagan, 40 years on'},
    {'title': 'Graba tus próximas aventuras con esta GoPro de oferta'},
    {'title': "Europe’s 'baby bust': can paying for pregnancies save Greece? - video"},
    {'title': "After the deluge: NSW's flood disaster victims begin cleanup – in pictures"},
    {'title': "Wolfsburg v Chelsea: Women's Champions League quarter-final – live!"},
    {'title': "'A parallel universe': the rickety pleasures of America's backroads - in pictures"},
    {'title': "'Nomadland': Chloé Zhao and crew reveal how they made one of the year's best films"},
    {'title': 'Seaspiracy: Netflix documentary accused of misrepresentation by participants'},
    {'title': 'Unblocking the Suez canal – podcast'},
    {'title': 'About half of people in UK now have antibodies against coronavirus'},
    {'title': 'Top 10 books about New York | Craig Taylor'},
    {'title': 'Is Moldova ready to embrace an unmarried, childfree president? | Europe’s baby bust – video'},
    {'title': 'This woman left North Korea 70 years ago. Now virtual reality has helped her return'},
    {'title': "'Immediate and drastic.' The climate crisis is seriously spooking economists"},
    {'title': "Under Xi's rule, what is China's image of the 'ideal' man?"},
    {'title': "'Hamlet' in the skies? The story behind Taiwan's newest airline, STARLUX"},
    {'title': 'Pokémon at 25: How 151 fictional species took over the world'},
    {'title': '‘Similar to having a baby, the euphoria’: rediscovery of rare gecko delights experts'},
    {'title': 'How Budapest became a fine dining force to be reckoned with'},
    {'title': "Teen who filmed killing tells court George Floyd was 'begging for his life'"},
    {'title': 'Empowering, alluring, degenerate? The evolution of red lipstick'},
    {'title': 'Real-world locations straight out of a Wes Anderson movie'},
    {'title': 'The club kid designer dressing the most powerful women in US politics'},
    {'title': "Fashion gaffes are a reflection of the industry's diversity problem"},
    {'title': 'Our colorful clothes are killing the environment'},
    {'title': 'Hazte con un Roku SE a mitad de precio'},
    {'title': 'Why does Bollywood use the offensive practice of brownface in movies?'},
    {'title': "Why Washington Football Team may stick with their 'so bad it’s good' name"},
    {'title': "Graphic novel on the Tiananmen Massacre shows medium's power to capture history"},
    {'title': 'Could a Norway boycott of the Qatar World Cup change the future of football?'},
    {'title': 'Las 5 cosas que debes saber este 31 de marzo: Así es una instalación que alberga a menores migrantes'},
    {'title': 'Multiplica el alcance de tu Wi-Fi con este repetidor rebajado un 50%'},
    {'title': "'Lack of perspective': why Ursula von der Leyen's EU vaccine strategy is failing"},
    {'title': 'Redder Days by Sue Rainsford review – waiting for the end of the world'},
    {'title': 'Dita Von Teese and Winnie Harlow star in a star-studded fashion film'},
    {'title': 'The Suez fiasco shows why ever bigger container ships are a problem'},
    {'title': 'The most anticipated buildings set to shape the world in 2020'},
    {'title': "Elite minority of frequent flyers 'cause most of aviation's climate damage'"},
    {'title': "I love my boyfriend – but I really don't want to have sex with him"},
    {'title': 'Amazon-backed Deliveroo crashes in London IPO'},
    {'title': 'People are calling for museums to be abolished. Can whitewashed American history be rewritten?'},
    {'title': 'Ruby Rose on gender, bullying and breaking free: ‘I had a problem with authority’'},
    {'title': 'Merkel, Macron and Putin in talks on using Sputnik V jab in Europe, says Kremlin'},
    {'title': 'After fighting cancer, Tracey Emin returns to the art world with raw, emotional works'}]}},
 'errors': None}
```

因此我们可以看到`result`只包含 100 篇文章，这是由于默认的限制 100。让我们改变它。

```
>>> result = client.query.get(class_name='Article', properties="title")\
...     .with_limit(200)\
...     .do()
>>> print(f"Number of articles returned: {len(result['data']['Get']['Article'])}")Number of articles returned: 200
```

通过堆叠多种方法，我们可以做得更多。`.get`可用的方法有:

*   `.with_limit` -设置返回对象的另一个限制。
*   `.with_near_object` -获取与传递给该方法的对象相似的对象。
*   `.with_near_text` -获取与传递给该方法的文本相似的对象。
*   `.with_near_vector` -获取与传递给该方法的 vector 相似的对象。
*   `.with_where` -获取使用`Where`过滤器过滤的对象，参见此[链接](https://www.semi.technology/developers/weaviate/current/graphql-references/filters.html#where-filter)获取示例和解释。

此外，可以使用将 GraphQL 查询作为字符串返回的`.build()`方法来代替`.do()`。这个字符串可以传递给`.raw()`方法。

**注意:**每个查询只能使用一个`.with_near_*`。

```
>>> client.query.get(class_name='Article', properties="title")\
...     .with_limit(5)\
...     .with_near_text({'concepts': ['Fashion']})\
...     .do(){'data': {'Get': {'Article': [{'title': 'Dita Von Teese and Winnie Harlow star in a star-studded fashion film'},
    {'title': "Fashion gaffes are a reflection of the industry's diversity problem"},
    {'title': "Bottega Veneta ditches Instagram to set up 'digital journal'"},
    {'title': 'Heir to O? Drew Barrymore launches lifestyle magazine'},
    {'title': 'Our colorful clothes are killing the environment'}]}},
 'errors': None}
```

使用`Get`我们可以看到每个对象的交叉引用。我们将为此使用`.raw()`方法，因为用任何现有的`.with_*`方法都不可能。

```
>>> query = """
... {
...   Get {
...     Article(limit: 2) {
...       title
... 
...       hasAuthors {         # the reference
...         ... on Author {    # you always set the destination class
...           name             # the property related to target class
...         }
...       }
...     }
...   }
... }
... """

>>> prettify(client.query.raw(query)['data']['Get']['Article'])[
  {
    "hasAuthors": [
      {
        "name": "Rhonda Garelick"
      }
    ],
    "title": "The soft power impact of Ruth Bader Ginsburg's decorative collars"
  },
  {
    "hasAuthors": [
      {
        "name": "Saskya Vandoorne"
      }
    ],
    "title": "After centuries in the ground, these French oaks will soon form part of the new spire at Notre Dame"
  }
]
```

## 骨料

我们可以使用`.aggregate`来计算满足特定条件的对象数量。

```
>>> # no filter, count all objects of class Article
>>> client.query.aggregate(class_name='Article')\
...     .with_meta_count()\
...     .do(){'data': {'Aggregate': {'Article': [{'meta': {'count': 200}}]}},
 'errors': None}>>> # no filter, count all objects of class Author
>>> client.query.aggregate(class_name='Author')\
...     .with_meta_count()\
...     .do(){'data': {'Aggregate': {'Author': [{'meta': {'count': 258}}]}}, 'errors': None}
```

以下是`.aggregate`支持的方法。

*   `.with_meta_count`将元计数设置为真。用于计算每个过滤组的对象数。
*   `.with_fields` -聚合查询返回的字段。
*   `.with_group_by_filter` -设置一个`GroupBy`滤镜。查看此[链接](https://www.semi.technology/developers/weaviate/current/graphql-references/aggregate.html#groupby-filter)了解更多关于过滤器的信息。
*   `.with_where` -使用`Where`过滤器聚集物体。参见此[链接](https://www.semi.technology/developers/weaviate/current/graphql-references/filters.html#where-filter)获取示例和解释。

当然，在查询数据时，可能性是无穷无尽的。体验这些功能的乐趣。

(jupyter-notebook 可以在这里找到[。)](https://github.com/semi-technologies/Getting-Started-With-Weaviate-Python-Client/blob/main/Getting-Started-With-Weaviate-Python-Client.ipynb)

请随时在 [GitHub](https://github.com/semi-technologies/weaviate-python-client) 上查看并为 weaviate-client 投稿。