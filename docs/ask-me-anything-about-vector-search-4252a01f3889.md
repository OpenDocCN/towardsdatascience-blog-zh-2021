# 问我任何关于矢量搜索的问题

> 原文：<https://towardsdatascience.com/ask-me-anything-about-vector-search-4252a01f3889?source=collection_archive---------20----------------------->

![](img/e1c8375caf9678786950698e1540504c.png)

本杰明·苏特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今年的[柏林流行语](https://2021.berlinbuzzwords.de/)特别关注搜索的未来——向量搜索，以及其他真正酷的话题，如[扩展卡夫卡](https://2021.berlinbuzzwords.de/session/should-you-read-kafka-stream-or-batch-should-you-even-care)，使用 [Opentelemetry](https://2021.berlinbuzzwords.de/session/building-observable-streaming-systems-opentelemetry) 的分布式系统跟踪以及[提高工作满意度](https://2021.berlinbuzzwords.de/session/scale-your-job-satisfaction-not-your-software)。一些致力于向量搜索主题的会议包括一些令人印象深刻的密集检索技术的[演示](https://twitter.com/DmitryKan/status/1405592168639651849)，以支持在你的文本湖中回答问题。

在问我任何问题:矢量搜索！[会议](https://2021.berlinbuzzwords.de/session/ask-me-anything-vector-search) [Max Irwin](https://medium.com/u/ef0b7261dd17?source=post_page-----4252a01f3889--------------------------------) 和我讨论了矢量搜索的主要话题，从它的[适用性](https://2021.berlinbuzzwords.de/session/text-search-and-recommendation-ads-and-online-dating-approximate-nearest-neighbors-real)到它与优秀的 ol' sparse search (TF-IDF/BM25)的比较，到它在黄金时间的准备情况以及在向用户提供它之前哪些具体的工程元素需要进一步调整。如果你对矢量搜索感兴趣，你可以从阅读我写的关于这个主题的一系列博客文章开始:在 [Solr](https://dmitry-kan.medium.com/neural-search-with-bert-and-solr-ea5ead060b28) 、 [Lucene](https://medium.com/swlh/fun-with-apache-lucene-and-bert-embeddings-c2c496baa559) 和 [Elasticsearch](/speeding-up-bert-search-in-elasticsearch-750f1f34f455) 中，跳转到 GitHub [repo](https://github.com/DmitryKey/bert-solr-search) (在会议期间，我有与会者联系我，分享他们使用它运行内部演示——所以你也可以这样做)。

更新:AMA 会议的录音在这里:

![](img/c813a18e292e36f6a6cba42ca376abf6.png)

链接:[https://youtu.be/blFe2yOD1WA](https://youtu.be/blFe2yOD1WA)

[https://www.youtube.com/watch?v=blFe2yOD1WA](https://www.youtube.com/watch?v=blFe2yOD1WA)

对于这篇博文，我决定从友好的互联网观众中挑选 3 个最重要的问题(我们在活动前圈了一份在线表格，以收集一组关于矢量搜索的非常有趣和深刻的问题)，并给出我的部分答案，稍微扩展一下(并在可能的情况下增加论文和代码)。

> 我们从研究和工业两方面看到了密集检索领域出现的一些模式。你对密集检索的下一步有什么想法？事情将走向何方，人们需要做些什么准备？

这里有一篇来自谷歌研究的最近的[论文](https://arxiv.org/abs/2105.13626)，关于在[字节级](https://github.com/google-research/byt5)上训练嵌入模型，这将有助于解决拼写错误查询的各种令人生畏的问题。另一篇论文应用[傅立叶变换](https://syncedreview.com/2021/05/14/deepmind-podracer-tpu-based-rl-frameworks-deliver-exceptional-performance-at-low-cost-19/amp/)来提高 BERT 的速度:快 7 倍，准确率 92%。因此，社区正在解决嵌入带来的各种问题，这将推动密集检索/重新排序和矢量搜索的进一步发展。需要注意的一点是，这些模型是否通用化，根据 BEIR 的基准测试论文，密集检索方法不能很好地通用化。只有当模型已经为相同的领域训练时，它们才会击败 BM25。相比之下，最快的方法是基于重排序的，如 ColBERT，但有一个条件:准备多分配 10 倍的磁盘空间来存储索引，而不是 BM25 索引(具体数字:900 GB 对 18 GB)。当谈到将前沿研究产品化时，除了考虑神经搜索如何与当前的搜索解决方案共存之外，您还需要整体评估特定搜索方法对可伸缩性、搜索速度、索引足迹的影响。你还允许预过滤吗？用户对他们在屏幕上看到的结果有发言权吗？你的 UX 会平稳地支持这种搜索引擎模式的转变，并在转变过程中保持用户效率与当前水平相当吗？

此外，当您考虑为您的域构建矢量搜索块时，请仔细选择相似性度量:余弦度量在排名中倾向于较短的文档，而点积则倾向于较长的文档，因此可能需要这些度量的组合，甚至是动态度量选择过程。这可以追溯到仔细设计整个搜索栈和/或选择搜索供应商。矢量搜索的总体性能是一个尚未解决的问题，所以你需要寻找最适合你的搜索引擎的模型配置，不要太在意大玩家报告的误差幅度。我也可以推荐阅读一些好的调查论文，比如 https://arxiv.org/abs/2106.04554 的文章。对于那些想在十亿规模数据集上练习并了解现有人工神经网络算法的能力和局限性的人来说，作为 NeurIPS 2021 的一部分，有一个出色的[大型人工神经网络竞赛](http://big-ann-benchmarks.com/index.html#call)宣布。

> 许多 ML 应用程序在简单的 web 服务后面使用 faiss/airy/NMS lib 进行人工神经网络检索，例如在推荐系统中。这对于简单的应用程序很有效，但是当需要有效的过滤时，你似乎需要跳跃到一个成熟的搜索系统(elastic，vespa 等)。你认为“faiss plus filter”工具有没有用武之地，或者你认为像 vespa 这样的搜索系统带来的额外好处能够弥补它带来的额外复杂性吗

D 不同的供应商提供了不同的构建人工神经网络指数的方法。在 Elasticsearch 世界里，你有两个选择:

1.  实现 LSH 的 Elastiknn 插件。
2.  用 HNSW 方法实现堆外图搜索。

Elastiknn 支持使用字段过滤器对结果进行预过滤，如颜色:蓝色。OpenDistro 通过重用 Elasticsearch 熟悉的功能来实现[预过滤](https://opendistro.github.io/for-elasticsearch-docs/docs/knn/)，比如脚本评分和无痛扩展。顺便说一句，我在以前的博客文章中试验了这两种方法(在开头提到过)，并实现了索引和搜索组件来演示这两种实现。

无论您选择哪种方法，您都需要仔细选择超参数，以便在索引速度、召回率和消耗的内存方面获得最佳性能。HNSW 可以很好地扩展到多核架构，它有一系列启发式算法来避免局部极小值，并且它可以构建一个连接良好的图。但是在 Lucene 中为每个段构建一个图在 RAM 和磁盘使用方面可能会变得非常昂贵，所以您应该考虑在服务查询之前将段合并成一个(所以考虑一下为这样的优化分配比纯 BM25 索引更多的时间)。我认为将过滤和人工神经网络结合起来作为搜索的一个单一阶段是一个明智的决定，因为多步检索可能会遭遇速度慢或召回率低或两者兼而有之的问题。此外，当用户预先知道该特定搜索将可能产生非常大量的文档作为回报时，他们可能想要控制文档空间的边界(例如，像工业研究或专利现有技术调查)。

> 这是一个内容长度“最佳点”吗？密集向量比稀疏向量(普通 tf*idf)有明显优势。

在你的领域专家团队的帮助下，你可以通过更加关注长文档中的内容来解决这个问题。第一段和第二段最重要吗？文档中的特定章节对于特定的信息需求是否重要？如果是的话，让它们被注释掉重要性和语义角色，并加载到多字段倒排索引中，使用 BM25 作为基线。顺便说一下，在衡量搜索质量时，你可以重复使用行业标准，如 DCG@P、NDCG@P、AP @ T——选择合适的评分者来优化搜索质量本身就是一门艺术，但如果你想开始，请前往 Quepid(使用[托管的](https://quepid.com/)或[本地](https://github.com/o19s/quepid)部署——纯开源),将其连接到 Solr / Elasticsearch，并开始使用评分查询来了解你当前的搜索质量。相信我，这项投资将会有回报，并产生大量改进的想法，从而使你的搜索引擎更具结构性。这是我今年用 Quepid 为学生做的电影搜索的现场演示:[https://www.youtube.com/watch?v=OOYsWn3LWsM&t = 1068s](https://www.youtube.com/watch?v=OOYsWn3LWsM&t=1068s)

密集检索有一个 512 个单词的自然限制，超过这个限制，无论是在索引速度方面，还是在一个长文本可以压缩成什么语义方面，该模型的性能都不会很好。"所有的神经方法都有文档长度的限制，因为它们有 512 个单词的限制."——来自 BEIR 纸业。

在 AMA 会议期间，观众提出了更多的问题。请[观看](https://www.youtube.com/watch?v=blFe2yOD1WA)录像，了解更多信息，享受矢量搜索的乐趣！