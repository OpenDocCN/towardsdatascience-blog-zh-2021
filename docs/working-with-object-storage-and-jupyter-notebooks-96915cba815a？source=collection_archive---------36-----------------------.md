# 使用对象存储和 Jupyter 笔记本

> 原文：<https://towardsdatascience.com/working-with-object-storage-and-jupyter-notebooks-96915cba815a?source=collection_archive---------36----------------------->

## 理解对象存储和 Jupyter 笔记本的概念。了解在 LakeFS 中集成它们的最佳平台。

面向通用用途的对象存储和 Jupyter 笔记本正在兴起。对大数据和数据元素的需求不断增加。虽然需要对象存储来存储持续增长的大量数据元素，但 Jupyter 笔记本电脑可以计算和分析这些数据集和数据。

在本文中，我们将介绍对象存储的概念，并承认它的好处。然后，我们将了解 Jupyter 笔记本电脑及其在现代计算中的应用范围。最后，我们将了解 LakeFS，它是集成对象存储和 Jupyter 笔记本的最佳平台之一。

# 了解对象存储:

![](img/f7ebe63800a6f9e55bd148652cef7b10.png)

[图像来源](https://commons.wikimedia.org/wiki/File:Vieww.png)

数据是一种宝贵的资源，互联网上有大量的数据(通常是非结构化的)和数据集。以高度可伸缩的方式存储这些数据元素的最佳方式是使用[对象存储](https://lakefs.io/object-storage/)。它具有很高的耐用性，因为存储的信息是安全的，即使某个磁盘出现故障，数据也不会丢失。

对象存储将体系结构中的数据元素作为对象进行管理。它不同于其他存储体系结构，如文件系统或块存储系统。前者利用文件存储数据，而后者使用扇区和磁道内的块。

因此，它是安全、灵活和可扩展的。这是数据恢复或备份系统的最佳选择之一。对象存储的功能允许其用户存储大量数据，这在人工智能、分析和云计算中尤其有用。当与 Jupyter 笔记本结合使用时，它会非常有价值。

大公司和科技巨头利用对象存储来保留大量数据。对象存储在特定的公司中有其独特的使用案例，如在脸书上存储照片、图片和图像，在网飞上收集电影，在 Spotify 上收集歌曲。在线协作服务 Dropbox 利用对象存储来存储多个文件。

# 使用 Jupyter 笔记本:

![](img/407a6620754cf9233efd36944ef7a5a3.png)

照片由 [Ashley West Edwards](https://unsplash.com/@ashleywedwards?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Project Jupyter 对程序员来说是一个强大的开源环境，因为它具有高度的交互性，并支持动态范围的编程语言，主要是 Python、R 和 Julia。

Jupyter 项目最重要的特征之一是 Jupyter 笔记本。这些笔记本可以让你开发各种编程项目，分析数据，创建许多机器学习和深度学习模型，等等。

Jupyter Book 允许用户从计算材料中构建书籍、内容和文档。Jupyter 笔记本最好的部分是它为用户提供的众多选项和独特性。您可以混合使用降价来构建代码块，以解释您的代码并使其更具代表性。您还可以在 Jupyter 笔记本上添加额外的数学公式，以及重组文本。最后，您可以下载各种格式的笔记本，包括 PDF 文件、HTML 网页、Python 文件或笔记本文件等输出格式。

如果你不想在你的系统上安装 Jupyter 笔记本的版本，你也可以使用这些笔记本的免费版本，可以从[谷歌联合实验室](https://colab.research.google.com)获得。Colaboratory(也称为 Colab)允许用户在免费 Jupyter 笔记本的帮助下开发 Python、数据科学和可视化项目。这些笔记本运行在云上，有免费的资源，如 GPU 和 TPU 可用。你可以将这些笔记本保存在谷歌硬盘上。

# 将 Jupyter 和对象存储与 LakeFS 集成:

![](img/efa0587e4787ad39e6373428fcabb348.png)

照片由[克劳迪娅·恰瓦扎](https://unsplash.com/@chiclaudia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

对象存储与 Jupyter 笔记本电脑相结合产生了极佳的效果。对象存储和 Jupyter Notebook 可以具有显著的互连性，以实现更好、更高效的结果。

由于对象存储中的数据是按照对象来管理的，因此它们可以存储大量的信息，这些信息可以在 Jupyter 笔记本中使用，以创建建设性的高质量项目。但是，集成 Jupyter 笔记本和对象存储组件的最佳平台是什么？

LakeFS[公司提供了一个最好的平台来整合对象存储的质量，并将其与 Jupyter 笔记本结合使用。](https://lakefs.io/)

LakeFS 是一个开源平台，可以创建一个定制系统来有效地管理您的数据元素。它提供对象存储的功能，并允许您访问各种资源，以利用其功能来执行复杂的任务，包括数据科学和分析。下图是其示意图。

![](img/26d243145eaafe6d122d0e67d1af5397.png)

[图像来源](https://docs.lakefs.io/)

LakeFS 旨在解决 GitHub 的一些与更广泛的数据存储相关的问题。比起 GitHub 这样的网站，你可以使用云来存储更多的对象。支持的云平台主要有两个，分别是 Amazon Web Services (AWS)和 Google 对象存储云平台，这些依次集成到 Spark、ML flow、Jupyter notebook 等。

借助这种架构，您可以在 Jupyter 笔记本电脑上利用对象存储的属性。这些有各种好处，包括为各种计算问题存储更大的数据集和文档，包括大数据、机器学习、深度学习、计算机视觉、自然语言处理等等。

由于 Jupyter 笔记本支持 Python，我们将研究 LakeFS 的一些 Python 代码。要开始安装过程，您可以通过在命令提示符下键入以下行来安装必要的依赖项。

```
pip install bravado==10.6.2
```

一旦安装了所有必需的依赖项，就可以开始处理各种任务了。借助 Bravado 包，您可以在运行时生成特定的动态客户端。这种生成是由 OpenAPI 定义通过 lakeFS 服务器完成的，它为您提供了指定 URL 的 JSON 扩展。如果您选择在您的系统上本地运行该平台，您可以在[http://localhost:8000/swagger . JSON](http://localhost:8000/swagger.json)上这样做。

让我们看几个特定于任务的代码块来理解它们的用法。首先，让我们试着用 Bravado 生成一个 Python 客户机。随着 Bravado 包的安装，唯一的其他要求是从 OpenAPI 获得一个 URL。在我们达到这两个要求之后，您可以使用下面的代码块来生成一个客户端:

```
from bravado.requests_client import RequestsClientfrom bravado.client import SwaggerClienthttp_client = RequestsClient()http_client.set_basic_auth('localhost:8000', 'AKIAIOSFODNN7EXAMPLE', 'wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY')client =  SwaggerClient.from_url('http://localhost:8000/swagger.json', http_client=http_client, config={"validate_swagger_spec": False})
```

一旦准备好了客户机对象，就可以用它来与 API 进行交互。仅举一个创建清单和存储库的简单例子，您可以使用下面的代码块。

```
client.repositories.createRepository(repository={ 'id': 'example-repo', 'storage_namespace': 's3://storage-bucket/repos/example-repo', 'default_branch':'main' }).result()# output:# repository(creation_date=1599560048, default_branch='main',            id='example-repo', storage_namespace='s3://storage-   bucket/repos/example-repo')
```

在本文中，我们已经讨论了一些通用代码。为了更详细地了解如何在 LakeFS 中使用 Jupyter 笔记本上支持的 Python API，我强烈推荐查看下面的[网站](https://docs.lakefs.io/using/python.html)。

# 结论:

在本文中，我们理解了对象存储和 Jupyter 笔记本的概念。然后，我们研究了将这两个方面集成到一个组件中的最佳方法之一，以便在 LakeFS 的帮助下实现更好的功能和更快的计算。

我强烈推荐查看下面的网站指南，通过他们的文档了解更多关于 LakeFS [的理论和技术方面的内容。对于进一步解释使用 LakeFS 平台可以实现的目标，这应该是一个很好的起点。](https://docs.lakefs.io/)

如果你对这篇文章中提到的各点有任何疑问，请在下面的评论中告诉我。我会尽快给你回复。

看看我的其他一些文章，你可能会喜欢读！

[](/how-to-read-and-understand-python-code-faster-180ba1ba9445) [## 如何更快地阅读和理解 Python 代码

### 如何更快地为数据科学和人工智能解释 Python 代码的快速指南

towardsdatascience.com](/how-to-read-and-understand-python-code-faster-180ba1ba9445) [](/5-reasons-why-you-should-code-daily-as-a-data-scientist-fa7fc6dc92c4) [## 作为数据科学家，你应该每天编码的 5 个理由

### 即使它只是每天一点点编码，你也必须考虑它

towardsdatascience.com](/5-reasons-why-you-should-code-daily-as-a-data-scientist-fa7fc6dc92c4) [](/answering-10-most-commonly-asked-questions-about-artificial-intelligence-f847e63b5837) [## 回答关于人工智能的 10 个最常见的问题

### 提供人工智能问题的解决方案，澄清误解，并讨论关于人工智能的各种概念

towardsdatascience.com](/answering-10-most-commonly-asked-questions-about-artificial-intelligence-f847e63b5837) [](/10-best-free-websites-to-learn-more-about-data-science-and-machine-learning-f2c6d7387b8d) [## 了解数据科学和机器学习更多信息的 10 个最佳免费网站！

### 分析 10 个最好的免费网站，以获得更多关于机器学习和数据科学的知识

towardsdatascience.com](/10-best-free-websites-to-learn-more-about-data-science-and-machine-learning-f2c6d7387b8d) 

谢谢你们坚持到最后。我希望你们喜欢阅读这篇文章。我希望你们都有美好的一天！