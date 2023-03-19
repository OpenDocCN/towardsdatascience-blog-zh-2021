# 我是如何通过 GCP 专业 ML 工程师认证的

> 原文：<https://towardsdatascience.com/how-i-passed-the-gcp-professional-ml-engineer-certification-47104f40bec5?source=collection_archive---------4----------------------->

![](img/3d111be38bd349fa4946b1b12f4b44bd.png)

Billy Huynh 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 如果你遵循这个学习计划，你也能做到

# 介绍

2020 年，谷歌云平台发布了其最新认证:**专业 ML 工程师认证**。

当 2021 年开始时，我决定试一试，尽管谷歌建议在参加考试前有 3 年以上的实践经验，而且我实际上从未在现实生活中与 GCP 共事过。但是为什么要这么做呢？嗯，不考虑证书本身，我真的认为准备工作本身会让我了解 GCP 如何工作以及如何在其上进行机器学习的许多细节。此外，由于我每天都在使用 ML，我认为这只是一个让它适应 Google 平台的问题。

尽管我花了近 3 个月的时间来准备，我可能可以在更短的时间内完成，但我最终比考试所需的时间更长，做了大量的 MOOCs 和 Qwiklabs，还通过一本书进行了学习。

我只是告诉你我个人的故事来表明，即使你没有太多的 GCP 经验，仍然有可能在合理的时间内学会它，至少足以通过认证考试。我会建议一个短得多的学习路线，然后展示我做的额外的东西，我认为这些东西对获得认证是不必要的，但如果你只是对学习 GCP 感兴趣，那可能会有用。

# 学习轨迹

## 基础知识

1.  快速浏览一下[考试指南](https://cloud.google.com/certification/guides/machine-learning-engineer)和[样题](https://docs.google.com/forms/d/e/1FAIpQLSeYmkCANE81qSBqLW0g2X7RoskBX9yGYQu-m1TtsjMvHabGqg/viewform)，以便在学习的时候知道要找什么。
2.  [这个关于机器学习的速成班](https://developers.google.com/machine-learning/crash-course/ml-intro)，如果你需要复习的话
3.  参加[这个课程](https://www.coursera.org/learn/gcp-big-data-ml-fundamentals)是为了理解主要的 GCP 工具以及如何将它们应用到 ML 问题中(如果你已经熟悉 GCP，跳过这个)
4.  参加[本课程](https://www.coursera.org/specializations/machine-learning-tensorflow-gcp)以了解张量流的基础知识(如果您熟悉张量流，请跳过本课程)

## 进入细节

1.  阅读文档(不要关注代码，而是更多地关注何时使用哪个工具)。阅读整个 GCP 文档可能需要很长时间，因此，请关注以下特定领域:

*   ML API—[视觉](https://cloud.google.com/vision)、[自然语言](https://cloud.google.com/natural-language/)、[视频](https://cloud.google.com/video-intelligence)和[语音转文本](https://cloud.google.com/speech-to-text)(了解他们每个人能做什么，也了解他们不能做什么)
*   [AutoML](https://cloud.google.com/automl/docs) (了解何时应该使用 AutoML 而不是 ML APIs)
*   [AI 平台](https://cloud.google.com/ai-platform/training/docs)(这是最重要的部分。重点关注如何提高性能、如何使用 TPU 和 GPU 等加速器、如何进行分布式培训和服务以及不同的可用工具，如假设工具)
*   [推荐 AI](https://cloud.google.com/recommendations-ai/docs/placements) (有 3 种型号类型。了解何时使用它们)
*   [TPU](https://cloud.google.com/tpu/docs/how-to)(知道何时以及如何使用它们)
*   [TensorFlow](https://www.tensorflow.org/guide) (不背代码，但是如何提高性能)
*   [BigQuery ML](https://cloud.google.com/bigquery/?tab=tab2#key-features) (了解所有可用的算法)

## 额外的东西

所有这些都是可选的，特别是如果你已经熟悉 GCP 的话。如果你不是，而且你有时间，它可能会帮助你学习一些额外的细节，但我不会说这是至关重要的。

1.  课程 [MLOps 基础](https://www.coursera.org/learn/mlops-fundamentals)学习更多关于人工智能平台管道和 Kubeflows 的知识
2.  课程[GCP tensor flow 端到端机器学习](https://www.coursera.org/learn/end-to-end-ml-tensorflow-gcp#syllabus)
3.  本书[谷歌云平台上的数据科学](https://www.amazon.com/Data-Science-Google-Cloud-Platform/dp/1491974567)提供一些动手编程的经验

如果你经历了所有这些，你可能已经涵盖了大部分考试内容，应该可以开始了。现在，让我们来看一些额外的提示，它们可能会帮助你专注于正确的事情。

# 额外提示和注意事项

## 一般

*   用于**流**数据的典型大数据管道:

*发布/订阅- >数据流- >大查询或云存储*

*   **批量**数据的典型大数据管道:

*发布/订阅- >云运行或云功能- >数据流- >大查询或云存储*

*   默认使用通用 API(视觉、视频智能、自然语言……)。仅当您有自定义需求(自定义标签等)时才使用 AutoML。)
*   要去除敏感数据，您可以使用 BigQuery、云存储、数据存储或数据丢失保护(DLP)进行编辑、标记或哈希处理
*   TensorBoard 和 TensorFlow 模型分析之间的差异:前者在训练期间基于小批量进行评估，而后者在训练之后进行评估，可以在数据切片中进行，并且基于全部数据
*   AI 解释:有了表格数据，你可以对大的特征空间使用有形状的或综合的成分；对于图像，可以使用集成渐变进行像素级解释，或者使用 XRAI 进行区域级解释。
*   什么时候在 TFX 上空使用库伯气流？当您需要 PyTorch、XGBoost 或者您想要对流程的每一步进行 dockerize 时
*   Keras:默认使用顺序 API。如果您有多个输入或输出、图层共享或非线性拓扑，请更改为函数式 API，除非您有 RNN。如果是这种情况，Keras 子类代替
*   优化张量流流水线的 3 种方法:预取、交错和缓存
*   写这篇文章的时候发现[这个网站](https://www.passcert.com/news_Google-Professional-Machine-Learning-Engineer-Exam-Dumps_756.html)有 5 个例题，有些是我考试的时候看到的。由于这是一个相当新的认证，除了谷歌提供的问题之外，不容易找到其他问题的例子，但随着时间的推移，应该会变得更容易

## 大查询 ML

*   它支持以下类型的模型:线性回归、二元和多元逻辑回归、k 均值、矩阵分解、时间序列、提升树、深度神经网络、AutoML 模型和导入的张量流模型
*   使用它进行快速简单的建模、原型制作等。

## 储存；储备

选择用于分析的存储:

*   结构化数据:毫秒级延迟的 Bigtable，秒级延迟的 BigQuery
*   非结构化:默认情况下使用云存储，移动设备使用 Firebase 存储

## 催速剂

*   在 CPU、TPU 和 GPU 之间选择:

> 对于快速原型、简单/小型模型或如果您有许多 C++自定义操作，请使用 CPUs 如果你有一些定制的 C++操作和/或中大型模型，使用 GPU 使用 TPU 进行大型矩阵计算，无需定制张量流运算和/或训练数周或数月的超大型模型

*   要提高 TPU 上的性能:如果数据预处理是一个瓶颈，则作为一次性成本离线进行；选择适合内存的最大批量；保持每个内核的批处理大小不变

## 神经网络

反向传播中的常见陷阱及其解决方案:

*   消失渐变->使用 ReLu
*   分解渐变->使用批量标准化
*   ReLu 层正在消亡->学习率降低

对于多类分类，如果:

*   标签和概率是互斥的，使用*soft max _ cross _ entropy _ with _ logits _ v2*
*   标签是互斥的，但不是概率，使用*sparse _ soft max _ cross _ entropy _ with _ logits*
*   标签不是互斥的，使用*sigmoid _ cross _ entropy _ with _ logits*

# 结论

我希望这篇文章能帮助你以更有效的方式准备认证，重点放在什么是重要的。显然，只有认证是不够的，你实际上必须知道如何在实践中使用 GCP 的 ML。因此，一旦你获得了你的认证，我建议你尝试一些 Qwiklabs 或平台上的一些个人兼职项目，以获得实践经验(但我向你保证，准备好考试将会产生巨大的差异)。

如果你正在准备 GCP ML 工程师认证，你可能会发现阅读这篇关于特征工程的文章很有用:

</feature-engineering-3a380ad1aa36>  

你只需花 7 分钟就能读完整本书，最终，你会对特性工程的基础有一个清晰的理解，这真的能帮助你获得认证。

> 如果你想进一步讨论，请随时在 LinkedIn 上联系我，这将是我的荣幸(老实说)。