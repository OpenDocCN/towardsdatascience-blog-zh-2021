# 一个 50 毫秒以下的神经搜索

> 原文：<https://towardsdatascience.com/a-sub-50ms-neural-search-with-distilbert-and-weaviate-4857ae390154?source=collection_archive---------16----------------------->

## 如何使用 Transformers、DistilBERT 和 vector search engine Weaviate 构建一个快速且可投入生产的神经搜索

当利用现代 NLP 模型(如 BERT)的功能时，获得准确的结果通常不是最大的问题。以 UX 为中心的库已经出现，这使得使用 NLP 模型变得更加容易。最值得注意的是拥抱脸变形金刚使伯特和类似模型的实验和调整变得相当容易。

然而，将 Jupyter 笔记本的实验投入生产则是另一回事。主要的挑战是如此沉重的神经模型的速度以及在生产中操作它们，[被称为 MLOps](https://en.wikipedia.org/wiki/MLOps) 。

在本教程中，我们将利用 vector search engine Weaviate 的能力来运行生产中的任何模型。Weaviate 还带有各种各样的内置模型，可以通过 Weaviate 的模块功能进行配置。这也包括对基于变压器的模型的支持。然而，对于本教程，我们决定在 Weaviate 之外进行所有的矢量化，以演示 Weaviate 如何用于任何模型。

![](img/f7f1614d0e4a9bb5cd98ee27c3df4022.png)

高速和低延迟将是我们在本教程中将要构建的神经搜索的关键特征。由[马修·施瓦茨](https://unsplash.com/@cadop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 我们如何为这个实验选择技术

主要的技术选择包括:

*   拥抱脸变形金刚
    变形金刚已经迅速成为与基于神经网络的 NLP 模型(如 BERT)一起工作的事实标准。PyTorch 和 Tensorflow 的深度集成提供了最大的灵活性和易用性。
*   **DistilBERT**
    本文的一个关键方面是在生产中实现高速度和极低的延迟。DistilBERT 的精度是普通 BERT 的 97%，同时重量更轻，速度快了大约 60%。这使得它成为一个伟大的选择。如果你想用不同的型号，你可以很容易地换成其他型号。
*   **Weaviate 矢量搜索引擎** Weaviate 是一个[实时矢量搜索引擎](https://www.semi.technology/developers/weaviate/current/#what-is-weaviate)，它不仅在查询时非常快，而且适合生产使用。它内置于 Go 中，具有云原生思维。这使得它既可靠又容易在 Kubernetes 和类似的容器编排器上运行。还有一个 [python 客户端](https://github.com/semi-technologies/weaviate-python-client)可用，它将使我们的变形金刚代码和 Weaviate 之间的连接变得容易。在引擎盖下，Weaviate 使用了一个 [HNSW index](https://arxiv.org/abs/1603.09320) 的变体，该变体被定制为提供您期望从适当的数据库中获得的所有特性[。](https://db-engines.com/en/blog_post/87)

# 我应该用 CPU 还是 GPU 来运行这个？

基于神经网络的模型在 GPU 上往往比在 CPU 上快得多，然而，当需要 GPU 时，进入的障碍更高。幸运的是，下面的所有代码在没有 GPU 的情况下也能工作。我们将强调在 CPU 和 GPU 上运行时需要做最小改动的地方。Weaviate 本身不需要任何 GPU，因为它在 CPU 上运行得相当快。使用 CUDA 支持的 CPU 可以极大地提高 BERT 和 BERT 衍生转换器的性能，这也是我们在本例中选择使用 GPU 的原因。

# 我从哪里得到一个 GPU？

如果你想在 GPU 支持下运行，至少有两个方便的选择:

1.  您可能已经在使用配有 CUDA 兼容 GPU 的计算机。
2.  你可以在云中旋转一个。对于这个例子，我们使用的是 Google Cloud 上可用的最小也是最便宜的 GPU 一辆英伟达特斯拉 T4。在我写这篇文章的时候[每月花费大约 180 美元](https://cloud.google.com/compute/gpus-pricing)。我们甚至不需要超过一个小时，使成本可以忽略不计。

# 我们的路线图:我们将建设什么？

在本教程中，我们将介绍以下步骤:

1.  首先，我们将使用 docker-compose 在本地启动 **Weaviate Vector 搜索引擎**。
2.  我们将**下载并预处理一个免费的基于文本的数据集**。如果您愿意，也可以用自己的数据替换数据集中的数据。
3.  然后我们将使用**转换器和 DistilBERT 将我们的文本编码成矢量**。您可以轻松地切换到您选择的另一个基于 transformer 的模型。
4.  我们现在可以**将文本对象和它们的向量导入到 Weaviate** 中。Weaviate 将自动建立一个矢量索引和一个倒排索引，使各种搜索查询成为可能。
5.  最后，我们将再次使用 DistilBERT 模型对搜索查询进行矢量化，然后**使用这个**执行矢量搜索。

# 先决条件

对于本教程，您需要:

*   bash 兼容的 shell
*   python >= 3.8 & pip 已安装
*   Docker 和 Docker Compose 已安装

# 可选择的

*   与 CUDA 兼容的 GPU，为您的操作系统安装了相应的驱动程序

# 步骤 1-旋转 Weaviate

在本地运行 Weaviate 最简单的方法是下载一个 docker-compose 示例文件。如果您愿意，您可以调整配置，但是对于本教程，我们将保留所有默认设置:

要下载 docker-compose 文件，请运行:

然后，您可以使用以下命令在后台启动它:

```
$ docker-compose up -d
```

Weaviate 现在应该正在运行，并在您的主机端口`8080`上公开。您可以使用`docker ps`进行验证，或者简单地向 Weaviate 发送一个测试查询，它应该会返回如下内容:

```
$ curl localhost:8080/v1/schema
# {"classes":[]}
```

它显示了一个空的模式，因为我们还没有导入任何东西。

您还可以查看文档，了解运行 Weaviate 的其他方式，如 Kubernetes、Google Cloud Marketplace 或 Weaviate cloud service。

# 步骤 2-下载示例数据集

对于本教程，我们选择使用 [20 个新闻组](http://qwone.com/~jason/20Newsgroups/)数据集，这是一个通常用于 NLP 相关任务的数据集。

我们将创建一个名为`data`的文件夹，在其中下载并提取数据集:

请注意，我们将不得不做一些预处理，例如删除每个文件的标题，这样我们只剩下帖子本身。我们将在接下来编写的 python 脚本中实现这一点。

# 步骤 3-安装 python 依赖项

我们将需要`torch`和`transformers`用于 BERT 模型，需要`nltk`用于标记化，需要`weaviate-client`将所有东西导入 Weaviate。

```
$ pip3 install torch transformers nltk weaviate-client
```

# 步骤 4 —开始构建我们的 python 脚本

我们现在将构建我们的 python 脚本，它将加载和预处理我们的数据，将我们的数据编码为向量，将它们导入到 Weaviate 中，最后用 Weaviate 运行向量搜索。

下面的代码片段被分成了更小的块，以便于阅读和解释，你也可以在这里下载完整的 python 脚本。

# 初始化一切

首先，让我们初始化我们将要使用的所有库。请注意，如果您没有运行 GPU 支持，您必须删除行`model.to('cuda')`。

# 加载和预处理数据集

接下来，我们将构建两个助手函数来从磁盘读取数据集。第一个函数将得到一个随机选择的文件名。第二个函数读取这些文件名并做一些简单的预处理。我们将通过识别两个换行符的第一次出现来从 newgroup 帖子中去除标题。此外，我们将用常规空格替换所有换行符和制表符。然后，我们跳过每个少于 10 个单词的帖子，以删除非常嘈杂的帖子。最后，我们将截断所有帖子，最多不超过 1000 个字符。您可以随意调整这些参数。

# 使用蒸馏模型进行矢量化

接下来，我们构建另外两个助手函数，它们用于对我们之前从数据集中读取的帖子进行矢量化。注意，我们将`text2vec`流程提取到一个单独的函数中。这意味着我们可以在查询时重用它。

注意，如果你运行时没有 GPU 支持，你必须删除线`tokens_pt.to('cuda')`。

# 初始化弱模式

Weaviate 有一个非常简单的模式模型。对于您创建的每个类，Weaviate 将在内部创建一个向量索引。类可以有属性。对于我们的`Post`类，类型为`text`的单个属性`content`就足够了。注意，我们还明确地告诉 Weaviate 使用`none`矢量器，这意味着 Weaviate 本身不会对任何东西进行矢量化——我们提供了用上面的 DistilBERT 创建的矢量。

# 将我们的数据导入 Weaviate

我们现在可以将所有数据导入到 Weviate 中。注意，对于每篇 20 条新闻的文章，我们将导入文本和向量。这将使我们的结果非常容易阅读，甚至允许混合基于文本和基于矢量的搜索。

我们可以使用 Weaviate 的批量导入特性，该特性将利用内部并行化来加速导入过程。根据您的资源选择批量大小，我们的测试 VM 应该能够轻松地一次处理 256 个对象。

# 用 DistilBERT 对搜索词进行矢量化，用 Weaviate 进行矢量搜索

我们现在可以重用上面定义的`text2vec`函数来矢量化我们的搜索查询。一旦我们从 DistilBERT 获得了向量，我们就可以使用客户端的`with_near_vector`方法将它传递给 Weaviate 的`nearVector` API。

让我们也测量一下需要多长时间，因为速度是这篇文章的主要动机之一。

# 全部运行

终于到了运行我们所有方法的时候了。让我们首先初始化模式，读取并矢量化一些帖子，然后将它们导入 Weaviate。如果你在没有 GPU 支持的情况下运行，减少帖子的数量可能是有意义的。

您应该会看到如下内容:

```
So far 100 objects vectorized in 2.073981523513794s
So far 200 objects vectorized in 4.021450519561768s
So far 300 objects vectorized in 6.142252206802368s
...
So far 3800 objects vectorized in 79.79721140861511s
So far 3900 objects vectorized in 81.93943810462952s
Vectorized 3969 items in 83.24402093887329s
```

现在让我们执行一些搜索:

它应该打印类似于以下内容的内容:

```
Query "the best camera lens" with 1 results took 0.018s (0.008s to vectorize and 0.010s to search)
0.8837:    Nikon L35 Af camera. 35/2.8 lens and camera case. Package $50  Send e-mail
---Query "which software do i need to view jpeg files" with 1 results took 0.022s (0.007s to vectorize and 0.015s to search)
0.9486:   How do I view .eps files on X? I have an image in color encapsulated postscript, and need to view it on my screen.  Are there any utilities that will let me convert between encapsulated postscript and plain postscript?  Joseph Sirosh
---Query "windows vs mac" with 1 results took 0.019s (0.007s to vectorize and 0.011s to search)
0.8491:   Appsoft Image is available for NeXTStep. It is a image processing program similar to Adobe Photoshop. It is reviewed in the April '93 issue of Publish! Magazine.   Richardt
```

请注意，这三个响应时间都低于 25 毫秒，我们成功实现了 50%的延迟目标！

请随意尝试不同的查询和限制。如果有些帖子的内容看起来有点过时，不要感到惊讶，20 个新闻组数据集确实很老了。当时肯定没有 iPhone，但有大量的麦金塔内容。

# 有更简单的方法吗？

上面练习的一个要点是表明 Weaviate 与任何 ML 模型都是兼容的，只要它能产生向量。结果，我们不得不自己采取一些措施。幸运的是，Weaviate 带有可选模块，可以帮助您对数据进行矢量化。例如，`[text2vec-contextionary](https://www.semi.technology/developers/weaviate/current/modules/text2vec-contextionary.html)`模块可以在导入时使用基于 fasttext 的算法对所有数据进行矢量化。如果你想使用伯特和朋友，看看即将发布的`text2vec-transformers`模块。

# 一个回顾和从这里去哪里。

我们已经证明，我们可以将 Weaviate、Hugging Face Transformers 和(提取)BERT 结合起来，产生一种生产质量的神经搜索，它能够在大约 25 毫秒内搜索数千个对象。对于这个演示来说，这是一个很好的例子，但是在实际应用中，您的数据集可能要大得多。幸运的是，Weaviate 的扩展性非常好，甚至可以在 50 毫秒内搜索数百万甚至数十亿个对象。要了解 Weaviate 如何处理大规模矢量搜索，请阅读[Weaviate](https://www.semi.technology/developers/weaviate/current/vector-index-plugins/hnsw.html)内部如何利用 HNSW。