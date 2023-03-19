# 我在 2021 年作为应用数据科学家探索的 10 项技术

> 原文：<https://towardsdatascience.com/10-technologies-i-explored-as-an-applied-data-scientist-in-2021-b0cd04fcfa9d?source=collection_archive---------20----------------------->

![](img/5a5f9d15a723dbe55abae2ccf86ac8e7.png)

来源:https://unsplash.com/photos/uCMKx2H1Y38

## 广告技术和深度学习的 ML

2021 年是我数据科学职业生涯技术技能发展的延续，重点是构建实时机器学习系统。虽然我去年开始构建生产级数据产品，正如我在 [2020 年回顾](https://medium.com/r?url=https%3A%2F%2Ftowardsdatascience.com%2F8-new-tools-i-learned-as-a-data-scientist-in-2020-6dea2f847c32)中概述的那样，但今年我更加关注机器学习，重点是在生产工作流程中使用深度学习。今年，我的职称也发生了变化，这反映了我向应用科学家角色的转变，我将亲自动手构建高通量 ML 系统。

在深入研究我今年探索的技术列表之前，我想确定我在过去几年中在数据科学领域看到的两个趋势:

1.  新领域中使用的自然语言处理方法
2.  应用数据科学不仅仅使用 Python

我专注于在 2021 年建立广告技术，使用机器学习在这个领域建立高性能系统，并发现这个领域的系统借鉴了 NLP 研究的许多方法，我将在我的技术列表中提供更多细节。第二个趋势是，为了发布 ML 系统，应用数据科学家通常需要使用与服务于模型预测的部署相同的编程语言。虽然 Python 可以用来创作可以扩展到大量请求的 web 端点，但是工程团队可能使用不同的编程语言来服务 ML 模型。对 2021 年的我来说，这意味着学习 Golang，以便更紧密地与我们的 ML 工程团队合作，推出数据产品。

鉴于这些趋势，我专注于以下技术，以在 2021 年继续发展我的应用数据科学职业生涯:

1.  Go 编程语言
2.  AWS 上的 NoSQL
3.  谷歌通用表达式语言(CEL)
4.  流畅位
5.  库贝特尔
6.  DataProc
7.  分布式深度学习
8.  线性模型的深度学习
9.  特征嵌入
10.  Word2vec

对于列表中的每一项，我将讨论学习该技术的动机，并为建立熟练程度提供指导。

## Go 编程语言

对我来说，2021 年最大的技术转变是学习 Golang，以便构建能够处理大量请求的 web 服务。我以前依赖 Java 编程语言来构建这些类型的系统，但是遇到了 Java 中使用的大规模线程模型的限制。对我来说，使用 Java 的最大问题是处理异步调用，比如给 NoSQL 的一家商店打电话。虽然现在 Java 对异步调用有了一些支持，但是用 Golang 编写这些类型的系统并让编程语言处理许多关于并发性的问题要容易得多。我还想学习 Golang，因为 Zynga 的许多团队现在都在使用 Go 来开发以前用 Java 编写的系统，接触 Golang 使我们的数据科学家能够直接与我们的工程团队合作来发布 ML 产品。

虽然学习 Golang 对于数据科学家来说是一项艰巨的任务，但有几种方法可以提高这门语言的熟练程度。我的方法是从在 Go 中构建小型 rest 端点开始，学习使用编程语言的基础知识。它还提供了使用`struct`数据类型解析 JSON 字符串的机会。一旦我能够启动并运行一个基本的端点，我就从 Redis 开始向端点添加一个 NoSQL 概要查找。我还利用了我过去用过的跨平台库，包括[协议缓冲区](https://developers.google.com/protocol-buffers)，它可以用于 Java、Python 和 Go。接下来，我探索了用`expvar`进行日志记录，以使用我们现有的日志堆栈和 DataDog。然后我探索了一些用于 ML 模型推理的 Golang 模块，包括用于 LightGBM 实现的[叶库](https://github.com/dmitryikh/leaves)。最终，我对编程语言有了足够的了解，可以开始用 Golang 设计 ML 系统。

对于初创公司的数据科学家和大型团队中的应用数据科学家来说，学习 Go 是非常好的，因为它可以帮助他们掌握生产系统的代码库。

## AWS 上的 NoSQL

虽然 2020 年我的重点是 GCP，但 2021 年我主要使用 AWS 来部署系统，因为许多 Zynga 服务都在那里运行。我继续使用 Redis，但是从 GCP 管理的 MemoryStore 版本切换到 AWS 管理的 ElastiCache 版本。Redis 非常适合需要亚毫秒级读写的情况，但是缺乏对列管理和并发写的支持。在许多情况下，为 NoSQL 的商店探索诸如 DynamoDB 这样的产品是有用的，它比简单的键值存储提供了更多的灵活性。在一个项目中，我们使用 Redis 的协议缓冲区，并使用 Dynamo 为系统存储元数据。如果我们对项目有不同的延迟需求，我们可能会在系统的更多方面利用 Dynamo。

我对使用这些系统的建议是使用 Docker 来运行这些服务的本地实例。例如， [dynamodb-local](https://hub.docker.com/r/amazon/dynamodb-local) 使得运行本地实例进行开发变得很容易，这种方法可以用于 Python 和 Golang。

## 谷歌通用表达式语言(CEL)

我在 2021 年面临的挑战之一是在不同的运行时环境中处理训练模型和服务模型。在这种情况下，我们使用 PySpark 进行模型训练，使用 Golang 进行模型服务。虽然我们能够在两个运行时中使用 LightGBM 进行模型训练和模型推断，但是我们需要一种方法来确保在这些不同的环境中以一致的方式执行特性工程。

我们的方法是在 PySpark 和 Golang 中使用谷歌的[通用表达式语言](https://opensource.google/projects/cel) (CEL)进行特征工程。CEL 程序是在运行时被翻译成高效程序表达式的小脚本。该脚本可以返回单个结果或结果列表。您可以将变量作为输入传递给脚本，输入也可以是协议缓冲区对象。因为我们已经在 PySpark 和 Golang 管道中使用协议缓冲区来存储配置文件信息，所以我们利用 CEL 程序将这些对象翻译成可以传递给 ML 库的特征向量。这种方法为探索不同的特性集提供了很大的灵活性，但是当我们的特性集增长到超过成千上万个特性时，这种方法就成了问题。

如果你对查看 CEL 感兴趣，我推荐你首先查看一下 [CEL 规范](https://github.com/google/cel-spec)，然后运行一个简单的脚本，使用一个语言实现，比如 [cel-go](https://github.com/google/cel-go) 。

## 流畅位

流利位是我在 2021 年首次探索的另一项技术。它是一个日志处理器，用于将数据和指标传递到各个目的地，并且可以以各种新颖的方式使用。我们最终使用它将事件数据记录到 S3，与我过去所做的相比，这是完成这项任务的一种简单得多的方法。例如，为了记录 GCP 上的事件数据，我通常会写一个 PubSub 主题，然后在数据流管道中使用该主题，该管道将事件传输到 BigQuery 或云存储。有了 Fluent Bit，就有了一个直接将消息记录到 [S3](https://docs.fluentbit.io/manual/pipeline/outputs/s3) 的处理器，使得这个过程更加直接。一个警告是，消息将被写成 JSON 字符串，并且需要额外的 ETL 步骤来使数据在数据库中以结构化格式可用。

## 库贝特尔

当我在 2020 年使用 Kubernetes 部署 web 服务时，我依赖 GCP 控制台来执行许多部署任务，比如滚动更新。当我在 2021 年将重点转移到 AWS 时，我仍然使用 Kubernetes 进行部署，但开始使用 EKS 代替 GKE。这意味着重新学习如何执行基本任务，例如查看 pod 的日志输出，使用 Kubernetes control CLI (kubectl)而不是依赖于云提供商的控制台 UI。学习 kubectl 的一些基本命令是很有用的，因为它是跨 Kubernetes 的不同云产品的标准化 CLI。

我对使用这个工具的建议是在 GCP 上运行一个小型 GKE 集群，并尝试复制 kubectl 中的命令，您也可以通过 UI 执行这些命令。随着时间的推移，您可能会发现自己越来越不依赖控制台来检查 pod 和管理部署。

## DataProc

虽然自 2018 年加入 Zynga 以来，我一直在使用 PySpark，但我一直使用 Databricks 平台来编写和部署 PySpark 工作流。2021 年，我需要在 GCP 上运行 PySpark 作业，并且没有 Databricks 作为运行作业的选项。我最终使用了 GCP 的 PySpark 集群专用解决方案 Cloud DataProc，类似于 AWS 上的 EMR。虽然 DataProc 为运行 PySpark 作业提供了一个健壮的平台，但是通过命令行界面而不是笔记本环境使用 PySpark 还有一点学习曲线。在集群上设置依赖关系还需要更多的手动步骤，比如 python 库。

对于数据科学家，我建议在使用 DataProc 或 EMR 之前，先在笔记本环境中体验一下 PySpark。我采用了一种渐进的方法来运行 spark 工作流的 DataProc。我的第一步是建立一个小型 DataProc 集群，并使用内置的 Jupyter 端点在笔记本环境中工作，并在笔记本中建立依赖关系。下一步是学习如何编写初始化动作脚本，以便在新集群上设置 Jar 依赖项和 Python 库。最后一步是安排一个工作流来初始化集群，并运行一系列 PySpark 脚本。

## 分布式深度学习

今年我尝试了 Horovod 图书馆。它是用于 TensorFlow 和 PyTorch 模型的分布式训练的框架。Horovod 还提供了 [spark 绑定](https://horovod.readthedocs.io/en/stable/spark_include.html)用于 PySpark 框架。结果是一个类似于 MLlib 的接口，使数据科学家能够在跨机器集群的大型数据集上训练深度学习模型。

如果您已经在使用 PySpark 进行分布式 ML 模型训练，并希望探索在管道中使用深度学习，我建议您探索 Horovod。如果你已经在使用 Databricks，这里有一个[快速启动](https://docs.databricks.com/applications/machine-learning/train-model/distributed-training/horovod-runner.html)指南，帮助你使用 Horovod。

## 线性模型的深度学习

由于实时数据产品的延迟要求，我在 2021 年广泛使用线性模型。虽然我们的系统基于可以实时服务的 ML 模型受到限制，但我们探索了训练深度学习模型来学习简单线性模型(如逻辑回归)的系数。使用这种方法，您可以用一个信号神经元训练一个神经网络，并将训练好的权重用作线性模型中的系数。当您希望训练具有自定义损失函数的模型时，或者在训练逻辑回归模型时需要支持连续标签时，这非常有用。

您可以通过使用现有的线性模型(如使用波士顿住房数据集来预测房价)并使用自定义损失函数将结果与 Keras 模型进行比较来掌握这种方法。例如，在测量预测误差时，您可能希望使用对数标度。

## 特征嵌入

我今年在生产中使用的一种深度学习方法是 Keras 嵌入层，以训练用作浅层模型输入的特征嵌入。当您希望在 XGBoost 模型中使用高维度的分类特征时，这种方法非常有用，例如用户所在的国家。您可以使用嵌入图层将输入表示为少量要素，而不是为每个国家创建一个要素。例如，您可以为用户的国家/地区训练 2D 嵌入，然后用两个数字特征替换国家/地区类别。这些特征嵌入可用于深度和浅层学习模型。[这个博客](/why-you-should-always-use-feature-embeddings-with-structured-datasets-7f280b40e716)为模型训练提供了一个很好的特征嵌入介绍。

## Word2vec

我今年探索的另一种深度学习方法是 Word2Vec，用于将分类数据转换为数字特征。虽然 Word2Vec 是为文本分析开发的，但它在 NLP 之外还有许多应用。例如，您可以将用户看过的电影表示为文本文档，其中每部电影都是文档中代表用户的一个单词。然后，您可以使用 Word2Vec 将这个文档表示转换成数字表示，并在这个新的空间中找到对电影有相似兴趣的用户。[本帖](https://medium.com/square-corner-blog/using-word2vec-to-power-a-recommendation-engine-1c18b93fbb30)很好的介绍了 Word2Rec，求推荐。

## 结论

2021 年是我开始将深度学习用于生产系统的一年，为了建立更高吞吐量的 ML 服务，我学习了 Golang。作为一名应用数据科学家，我开始与 ML 工程师密切合作，开发实时 ML 系统。虽然学习 Golang 对于数据科学家来说是一项艰巨的任务，但是如果您对构建实时系统感兴趣，那么学习 Golang 确实非常有用。我 2022 年的目标是继续从深度学习中学习新方法，并在生产系统中使用分布式深度学习。

[本·韦伯](https://www.linkedin.com/in/ben-weber-3b87482/)是 Zynga 应用科学总监。我们正在[招聘](https://www.zynga.com/job-listing-category/data-analytics-user-research/)！