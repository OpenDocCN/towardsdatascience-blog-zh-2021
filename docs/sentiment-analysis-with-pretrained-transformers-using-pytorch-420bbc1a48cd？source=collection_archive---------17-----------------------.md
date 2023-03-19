# 使用 Pytorch 对预调整变压器进行情感分析

> 原文：<https://towardsdatascience.com/sentiment-analysis-with-pretrained-transformers-using-pytorch-420bbc1a48cd?source=collection_archive---------17----------------------->

## 使用 Huggingface Transformers 中最简单的 API 预测积极或消极的情绪

![](img/d0f882992a7a3dacc42b8b3085f27124.png)

[卓成友](https://unsplash.com/@benjamin_1017?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 介绍

[**情感分析**](https://en.wikipedia.org/wiki/Sentiment_analysis) 自[**【NLP】**](https://en.wikipedia.org/wiki/Natural_language_processing)问世以来，一直是一项非常热门的任务。它属于**文本分类**的子任务或应用，其中从不同文本中提取和识别情感或主观信息。如今，世界各地的许多企业都使用情感分析，通过分析不同目标群体的情感来更深入地了解他们的顾客和客户。它还广泛应用于不同的信息来源，包括产品评论、在线社交媒体、调查反馈等。

**在本文中，我们将向您展示如何通过 Huggingface 使用** [**变形金刚库**](https://github.com/huggingface/transformers) **快速有效地实现情感分析。**我们将使用预调整的变压器，而不是微调我们自己的变压器，所以需要很低的安装成本。

所以，让我们直接进入教程吧！

# 教程概述

*   步骤 1:安装库
*   步骤 2:导入库
*   步骤 3:建立情感分析管道
*   步骤 4:输入文本
*   步骤 5:执行语义分析

# 步骤 1:安装库

我们需要安装的库是 Huggingface [变形金刚库](https://github.com/huggingface/transformers)。要安装变压器，您只需运行:

```
pip install transformers
```

*注意:Huggingface Transformers 需要安装*[*py torch*](https://pytorch.org/)*或*[*tensor flow*](https://www.tensorflow.org/)*中的一个，因为它依赖其中一个作为后端，因此在安装 Transformers 之前请确保有一个工作版本。*

# 步骤 2:导入库

在您成功地将 Transformers 安装到本地环境之后，您可以创建一个新的 Python 脚本并导入 Transformers 库。我们没有导入整个库，而是在库中引入了`[pipeline](https://huggingface.co/transformers/main_classes/pipelines.html)`模块，它提供了一个简单的 API 来执行各种 NLP 任务，并将所有代码复杂性隐藏在其抽象层之后。要导入`pipeline`模块，我们可以简单地做:

```
from transformers import pipeline
```

# 步骤 3:建立情感分析管道

现在，在您导入`pipeline`模块后，我们可以开始使用该模块构建情感分析模型和分词器。为了建造它，我们可以做:

```
sentiment_analysis = pipeline(“sentiment-analysis”)
```

这将创建一个适合情感分析任务的管道。等等，你可能会问，这里用的是什么模型和记号化器。默认情况下，变形金刚库使用一个 [**DistilBERT**](https://arxiv.org/abs/1910.01108) 模型，该模型在来自 [GLUE 数据集](https://gluebenchmark.com/)的[斯坦福情感树库 v2 (SST2)](https://www.kaggle.com/atulanandjha/stanford-sentiment-treebank-v2-sst2) 任务上进行了微调。

如果您想使用另一个模型或标记器，您可以在实例化管道时将它们作为`model`和`tokenizer`参数传递。

# 步骤 4:输入文本

我们已经建立了管道，现在是时候输入我们想要测试其情感的文本了。让我们为两个句子声明两个变量，一个肯定，一个否定:

```
pos_text = “I enjoy studying computational algorithms.”
neg_text = “I dislike sleeping late everyday.”
```

# 第五步:进行情感分析

最后，是时候对我们输入文本的情绪(积极或消极)进行分类了。要执行情感分析，我们可以运行以下程序:

```
result = sentiment_analysis(pos_text)[0]
print("Label:", result['label'])
print("Confidence Score:", result['score'])
print()result = sentiment_analysis(neg_text)[0]
print("Label:", result['label'])
print("Confidence Score:", result['score'])
```

输出:

> 标签:阳性
> 置信度得分:0.9000136363686
> 
> 标签:负的
> 信心分数:0.901038636866

瞧，瞧！我们现在可以看到，该模型正确地将两个文本预测到它们的情感中，具有高的置信度得分。

# 结论

这个帖子到此为止！在这篇短文中，我们回顾了如何使用 Transformers 库提供的简单 API 进行情感分析。如果你想要的话，我在这里附上了这篇文章的 Jupyter 代码:

如果你想知道如何对你自己的自定义数据集进行文本分类而不是情感分析，你可以参考我以前关于 BERT 文本分类的帖子。在那里，您将找到一个文本分类示例的深入演练和解释:

[](/bert-text-classification-using-pytorch-723dfb8b6b5b) [## 使用 Pytorch 的 BERT 文本分类

### 文本分类是自然语言处理中的一项常见任务。我们应用 BERT，一个流行的变压器模型，对假新闻检测使用…

towardsdatascience.com](/bert-text-classification-using-pytorch-723dfb8b6b5b) 

如果你喜欢，请随意看看我的其他帖子，下次见！

[](/text-generation-with-pretrained-gpt2-using-pytorch-563c7c90700) [## 使用 Pytorch 通过预训练的 GPT2 生成文本

### 使用 Huggingface 框架快速轻松地生成任何语言的文本

towardsdatascience.com](/text-generation-with-pretrained-gpt2-using-pytorch-563c7c90700) [](/question-answering-with-pretrained-transformers-using-pytorch-c3e7a44b4012) [## 使用 Pytorch 回答关于预调变压器的问题

### 学会在几分钟内建立一个任何语言的问答系统！

towardsdatascience.com](/question-answering-with-pretrained-transformers-using-pytorch-c3e7a44b4012) [](/machine-translation-with-transformers-using-pytorch-f121fe0ad97b) [## 使用 Pytorch 的变压器机器翻译

### 只需简单的几个步骤就可以将任何语言翻译成另一种语言！

towardsdatascience.com](/machine-translation-with-transformers-using-pytorch-f121fe0ad97b) [](/abstractive-summarization-using-pytorch-f5063e67510) [## 使用 Pytorch 的抽象摘要

### 总结任何文本使用变压器在几个简单的步骤！

towardsdatascience.com](/abstractive-summarization-using-pytorch-f5063e67510) [](/semantic-similarity-using-transformers-8f3cb5bf66d6) [## 使用转换器的语义相似度

### 使用 Pytorch 和 SentenceTransformers 计算两个文本之间的语义文本相似度

towardsdatascience.com](/semantic-similarity-using-transformers-8f3cb5bf66d6) 

# 参考

[1] [变形金刚 Github](https://github.com/huggingface/transformers) ，拥抱脸

[2] [变形金刚官方文档](https://huggingface.co/transformers/)，拥抱脸

[3] [Pytorch 官网](https://pytorch.org/)，艾研究

[4] [Tensorflow 官网](https://www.tensorflow.org/)，谷歌大脑

[5] Sanh，Victor，et al. [“蒸馏伯特，伯特的蒸馏版:更小，更快，更便宜，更轻。”](https://arxiv.org/abs/1910.01108) *arXiv 预印本 arXiv:1910.01108* (2019)。

[6] [斯坦福情感树库 v2 (SST2)](https://www.kaggle.com/atulanandjha/stanford-sentiment-treebank-v2-sst2) ，Kaggle

[7] [通用语言理解评测(GLUE)基准](https://gluebenchmark.com/)，NYU，UW NLP，DeepMind