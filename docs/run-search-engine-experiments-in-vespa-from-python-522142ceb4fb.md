# 在 python 的 Vespa 中运行搜索引擎实验

> 原文：<https://towardsdatascience.com/run-search-engine-experiments-in-vespa-from-python-522142ceb4fb?source=collection_archive---------47----------------------->

## pyvespa 的三种入门方式

[pyvespa](https://pyvespa.readthedocs.io/en/latest/index.html) 给 vespa 提供了一个 python API。该库的主要目标是允许更快的原型开发，并促进 Vespa 应用程序的机器学习实验。

![](img/c9576c73749e26e91498adbd7c759364.png)

克里斯汀·希勒里在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

有三种方法可以让你从`pyvespa`中获得价值:

1.  您可以连接到正在运行的 Vespa 应用程序。
2.  您可以使用 pyvespa API 构建和部署 Vespa 应用程序。
3.  您可以从存储在磁盘上的 Vespa 配置文件部署应用程序。

我们将逐一回顾这些方法。

## 连接到正在运行的 Vespa 应用程序

如果您已经在某个地方运行了一个 Vespa 应用程序，您可以使用适当的端点直接实例化 [Vespa](https://pyvespa.readthedocs.io/en/latest/reference-api.html#vespa.application.Vespa) 类。以下示例连接到 [cord19.vespa.ai](https://cord19.vespa.ai/) 应用程序:

```
from vespa.application import Vespa

app = Vespa(url = "https://api.cord19.vespa.ai")
```

然后我们准备好通过`pyvespa`与应用程序交互:

```
app.query(body = {
  'yql': 'select title from sources * where userQuery();',
  'hits': 1,
  'summary': 'short',
  'timeout': '1.0s',
  'query': 'coronavirus temperature sensitivity',
  'type': 'all',
  'ranking': 'default'
}).hits[{'id': 'index:content/1/ad8f0a6204288c0d497399a2',
  'relevance': 0.36920467353113595,
  'source': 'content',
  'fields': {'title': '<hi>Temperature</hi> <hi>Sensitivity</hi>: A Potential Method for the Generation of Vaccines against the Avian <hi>Coronavirus</hi> Infectious Bronchitis Virus'}}]
```

## 使用 pyvespa API 进行构建和部署

您还可以使用 pyvespa API 从头开始构建您的 Vespa 应用程序。这里有一个简单的例子:

```
from vespa.package import ApplicationPackage, Field, RankProfile

app_package = ApplicationPackage(name = "sampleapp")
app_package.schema.add_fields(
    Field(
        name="title", 
        type="string", 
        indexing=["index", "summary"], 
        index="enable-bm25")
)
app_package.schema.add_rank_profile(
    RankProfile(
        name="bm25", 
        inherits="default", 
        first_phase="bm25(title)"
    )
)
```

然后我们可以将`app_package`部署到 Docker 容器(或者直接部署到 [VespaCloud](https://pyvespa.readthedocs.io/en/latest/create-and-deploy-vespa-cloud.html) ):

```
from vespa.package import VespaDocker

vespa_docker = VespaDocker(
    disk_folder="/Users/username/sample_app", # absolute folder
    container_memory="8G",
    port=8080
)
app = vespa_docker.deploy(application_package=app_package)Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for application status.
Waiting for application status.
Finished deployment.
```

`app`保存了一个 Vespa 类的实例，就像我们的第一个例子一样，我们可以用它来提供和查询刚刚部署的应用程序。我们还可以访问存储在`disk_folder`中的 Vespa 配置文件，修改它们，并使用下一节讨论的方法直接从磁盘部署它们。当我们想要根据通过`pyvespa` API 无法获得的 Vespa 特性来微调我们的应用程序时，这是非常有用的。

还可以通过`export_application_package`方法将`app_package`显式导出到 Vespa 配置文件(无需部署它们):

```
vespa_docker.export_application_package(
    application_package=app_package
)
```

## 从 Vespa 配置文件部署

`pyvespa` API 提供了`Vespa`中可用功能的子集。原因是`pyvespa`旨在用作信息检索(IR)的实验工具，而不是用于构建生产就绪的应用程序。因此，python API 基于我们复制经常需要 IR 实验的常见用例的需求而扩展。

如果您的应用需要`pyvespa`中没有的功能或微调，您可以直接通过 Vespa 配置文件来构建，如 Vespa 文档中的[许多示例](https://docs.vespa.ai/en/getting-started.html)所示。但是即使在这种情况下，您仍然可以通过基于存储在磁盘上的 Vespa 配置文件从 python 部署`pyvespa`来获得价值。为了说明这一点，我们可以复制并部署本 [Vespa 教程](https://docs.vespa.ai/en/tutorials/news-3-searching.html)中介绍的新闻搜索应用程序:

```
!git clone [https://github.com/vespa-engine/sample-apps.git](https://github.com/vespa-engine/sample-apps.git)
```

新闻搜索应用程序的 Vespa 配置文件存储在`sample-apps/news/app-3-searching/`文件夹中:

```
!tree sample-apps/news/app-3-searching/[01;34msample-apps/news/app-3-searching/[00m
├── hosts.xml
├── [01;34mschemas[00m
│   └── news.sd
└── services.xml

1 directory, 3 files
```

然后，我们可以从磁盘部署到 Docker 容器:

```
from vespa.package import VespaDocker

vespa_docker_news = VespaDocker(
    disk_folder="/Users/username/sample-apps/news/app-3-searching/", 
    container_memory="8G", 
    port=8081
)
app = vespa_docker_news.deploy_from_disk(application_name="news")Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for configuration server.
Waiting for application status.
Waiting for application status.
Finished deployment.
```

同样，`app`保存了一个 Vespa 类的实例，就像我们的第一个例子一样，我们可以用它来提供和查询刚刚部署的应用程序。

## 最后的想法

我们讨论了使用`pyvespa`库从 python 连接到`Vespa`应用程序的三种不同方式。这些方法提供了极大的工作流灵活性。它们允许您快速开始 pyvespa 试验，同时允许您修改 vespa 配置文件以包括 pyvespa API 中不可用的功能，而不会失去试验添加功能的能力。