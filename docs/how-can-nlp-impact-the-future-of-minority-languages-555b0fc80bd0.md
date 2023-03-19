# NLP 如何影响少数民族语言的未来？

> 原文：<https://towardsdatascience.com/how-can-nlp-impact-the-future-of-minority-languages-555b0fc80bd0?source=collection_archive---------75----------------------->

## 在过去的几年里，自然语言处理领域取得了重大进展，但这对少数民族语言意味着什么呢？

![](img/6dd7f6b0b88786f594c92d1f14263956.png)

[" File:wubi 86 keyboard layout . png "](https://commons.wikimedia.org/w/index.php?curid=80931642)由[仓颉 6](https://commons.wikimedia.org/wiki/User:Cangjie6) 授权于 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0?ref=ccsearch&atype=rich)

OpenAI 的 GPT-3 论文于 2020 年 5 月 28 日问世，成为 AI 自然语言处理领域的热门话题。其 1750 亿参数的 transformer 架构使其在专业和一般新闻中非常受欢迎，这要归功于一些开发人员迅速展示的大量应用程序，其中一些我在我的介绍性文章中列出了:

[](/gpt-3-101-a-brief-introduction-5c9d773a2354) [## GPT-3 101:简介

### 在过去的几周里，几乎不可能避免对 GPT-3 的大肆宣传。这篇文章简要介绍了…

towardsdatascience.com](/gpt-3-101-a-brief-introduction-5c9d773a2354) 

## 最先进的 NLP 模型是如何用不同的语言训练出来的？

让我们以 GPT 3 号为例。根据 [GPT-3 论文](https://arxiv.org/pdf/2005.14165.pdf)，它在大规模数据集上进行了预训练，包括[公共爬行](https://commoncrawl.org/)数据集，它包含自 2008 年以来收集的数 Pb 数据，占 GPT-3 总训练组合的 60%，其中还包括其他数据集，如 WebText2、Books1、Books2 等..根据常见的抓取 github 信息，这是语言在数据集中跨文档的分布:

 [## 常用抓取月度档案统计

### 页面数量、顶级域名分布、抓取重叠等。-每月常见爬网的基本指标…

commoncrawl.github.io](https://commoncrawl.github.io/cc-crawl-statistics/plots/languages) 

英语显然是语料库中的主导语言，占总文档的 40%以上，而第二大语言是俄语，占总文档的不到 10%。就母语人士而言，一些排名前十的口语显然没有被充分代表。以西班牙语为例，拥有 4.6 亿母语者(多于母语为英语者)的前 2 大语言仅占语料库中全部文档的 4%)。在下面的链接中，您可以找到另一个基于 GPT-3 训练数据集上每种语言的字符总数的统计数据:

[](https://github.com/openai/gpt-3/blob/master/dataset_statistics/languages_by_character_count.csv) [## openai/gpt-3

### GPT-3:语言模型是一次性学习者。在 GitHub 上创建一个帐户，为 openai/gpt-3 开发做贡献。

github.com](https://github.com/openai/gpt-3/blob/master/dataset_statistics/languages_by_character_count.csv) 

那么，GPT-3 在管理英语以外的语言方面做得如何呢？在一个有趣的 reddit 帖子中，一位用户似乎对它如何设法学习 Python 等编码语言感到困惑，并收到了一位名叫 [gwern](https://www.reddit.com/r/gwern/) 的用户的有趣回复，指出了另一个问题，即“GPT 的架构&数据处理如何影响它对各种事物的学习”。

同一个 reddit 用户写了一篇关于 GPT-3 的优秀文章，指出 BPE(字节对编码)的使用如何以不同的方式影响不同类型的语言(例如，分析型或合成型)，从而引入了隐性偏见。您可以在下面找到完整的文章:

总之，尽管大量的训练数据使得像 GPT-3 这样的语言模型在几种语言中都很好，即使它们针对英语进行了优化和/或它们使用了一小部分非英语文档，但少数民族语言(甚至非少数民族语言)仍然处于很大的劣势。

## 对非英语语言的潜在影响是什么？

即使像 GPT-3 这样的语言模型不知道他们在说什么，我们的关系也正通过人工智能变得越来越数字化和自动化。NLP 模型在特定语言中的表现如何可能对其未来至关重要，因为在在线服务中，这些模型可能会越来越多地被那些更常用的模型所取代，因此在更新的机器学习解决方案中会得到优化。这就是为什么一些语言学院，如西班牙的 RAE (Real Academia de la Lengua)或当地政府，如加泰罗尼亚自治区政府，正在采取具体措施来解决这个问题:

[](https://www.rae.es/noticia/la-rae-presenta-el-proyecto-lengua-espanola-e-inteligencia-artificial-leia-en-el-xvi) [## RAE 在第十六届大会上介绍了西班牙语和人工智能项目(LEIA)

### 关于西班牙语学术协会(ASALE)第十六届大会条款的一部分

www.rae.es](https://www.rae.es/noticia/la-rae-presenta-el-proyecto-lengua-espanola-e-inteligencia-artificial-leia-en-el-xvi) [](https://politiquesdigitals.gencat.cat/ca/detalls/ActivitatAgenda/20201210-projecte-Aina) [## AINA 项目介绍会

### 巴塞罗那超级计算中心的公共行政数字政策部介绍

politiquesdigitals.gencat.cat](https://politiquesdigitals.gencat.cat/ca/detalls/ActivitatAgenda/20201210-projecte-Aina) 

虽然这些举措显示了意识，并且肯定是改善人工智能在非英语语言中的使用的良好步骤，但很明显，主要研究是用英语进行的。例如，在本文撰写之日，最新的多样化文本数据集之一，名为 The Pile，是一个 815Gb 的英语语料库。

 [## 那堆东西

### Pile 是一个 825 GiB 多样化的开源语言建模数据集，由 22 个较小的、高质量的…

pile.eleuther.ai](https://pile.eleuther.ai/) 

NLP 给小众语言带来了怎样的挑战？下面的文章提供了一个很好的总结，集中在三个方面:

*   社会劣势的强化
*   规范性偏见
*   改进 ML 技术的语言扩展

[](/the-importance-of-natural-language-processing-for-non-english-languages-ada463697b9d) [## 自然语言处理对非英语语言的重要性

### 为什么 NLP 中没有更多的语言，为什么这很重要？

towardsdatascience.com](/the-importance-of-natural-language-processing-for-non-english-languages-ada463697b9d) 

## 结论

历史告诉我们，技术对于保持语言记忆的活力至关重要。也许这种技术最大的例子就是不同形式的书。我们所知的泥板、纸莎草纸和后来的书籍不仅是保存古代语言的关键，也是保存我们记忆的关键。

在另一个可能不太为人所知的故事中，中国在 20 世纪 80 年代面临一个巨大的问题，当时他们意识到它的 70，000 多个字符放不进一个键盘。如果他们没有用[五笔输入法](https://en.wikipedia.org/wiki/Wubi_method#:~:text=The%20Wubi%20method%20is%20based,particular%20spoken%20variety%20of%20Chinese.)解决这个问题，40 年后中国还会是一个科技超级大国吗？没有得到足够 NLP 支持的语言在 40 年后会发生什么？

*如果你喜欢阅读这篇文章，请* [*考虑成为会员*](https://dpereirapaz.medium.com/membership) *以便在支持我和媒体上的其他作者的同时，获得上的所有故事。*