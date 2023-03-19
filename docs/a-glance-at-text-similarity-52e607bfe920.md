# 文本相似性一瞥

> 原文：<https://towardsdatascience.com/a-glance-at-text-similarity-52e607bfe920?source=collection_archive---------26----------------------->

## [自然语言处理笔记](https://towardsdatascience.com/tagged/nlpnotes)

## 如何计算文档之间的相似度

![](img/f628baa155a2791cfd74f5a05b1931bd.png)

[蒂姆·J](https://unsplash.com/@the_roaming_platypus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 什么是文本相似度？

考虑以下两句话:

*   我的房子是空的
*   我的矿上没有人

尽管这两个句子是以两种完全不同的格式写成的，但是人类可以很容易地确定这两个句子表达了非常相似的意思；这两个句子的交集只有一个共同的词“是”，它不能提供任何关于这两个句子有多相似的见解。尽管如此，我们仍然期望相似性算法返回一个分数，通知我们句子非常相似。

这种现象描述了我们所说的语义文本相似性，我们的目的是根据每个文档的上下文来识别相似的文档。由于自然语言的复杂性，这是一个相当困难的问题。

另一方面，我们还有另一个现象叫做词汇文本相似性。词汇文本相似性旨在识别文档在单词级别上的相似程度。许多传统技术倾向于关注词汇文本相似性，并且它们通常比慢慢崛起的新深度学习技术实现起来快得多。

本质上，我们可以将文本相似性定义为试图确定两个文档在词汇相似性和语义相似性方面有多“接近”。

这是自然语言处理(NLP)领域中的一个常见但棘手的问题。文本相似性的一些示例用例包括对文档与搜索引擎中的查询的相关性进行建模，以及理解各种人工智能系统中的相似查询，以便向用户提供统一的响应。

## 文本相似性的流行评价指标

每当我们执行某种自然语言处理任务时，我们需要一种方法来解释我们正在做的工作的质量。“文档非常相似”是主题，与具有 90%准确性分数的模型相比，信息不多。度量为我们提供了客观的、信息丰富的反馈来评估任务。

流行的指标包括:

*   欧几里得距离
*   余弦相似性
*   雅克卡相似性

我在[向量空间模型](/vector-space-models-48b42a15d86d)中介绍了欧几里德距离和余弦相似性，而[桑克特古普塔](https://medium.com/u/d8b11b8c0c06?source=post_page-----52e607bfe920--------------------------------)关于[文本相似性度量概述的文章](/overview-of-text-similarity-metrics-3397c4601f50)详细介绍了 Jaccard 相似性度量。

为了更好地理解我们评估文本相似性的两种方法，让我们用 python 编写上面的例子。

## Python 中的词汇文本相似性示例

```
# importing libraries
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.feature_extraction.text import TfidfVectorizer# utility function to evaluate jaccard similarity
def jaccard_similarity(doc_1, doc_2):
    a = set(doc_1.split())
    b = set(doc_2.split())
    c = a.intersection(b)
    return float(len(c)) / (len(a) + len(b) - len(c))# defining the corpus
corpus = ["my house is empty", "there is no one at mine"]# to evaluate cosine similarities we need vector representations
tfidf = TfidfVectorizer()
X = tfidf.fit_transform(corpus)# printing results
print(f"Cosine Similarity: {cosine_similarity(df, df)[0][1]}\nJaccard Simiarity: {jaccard_similarity(corpus[0], corpus[1])}")**>>>> Cosine Similarity: 0.11521554337793122
     Jaccard Simiarity: 0.1111111111111111**
```

与我们认为这些文档相似的想法相反，两个评估指标都表明我们的句子一点也不相似。这是意料之中的，因为正如我们之前所说的，文档不包含相似的单词，因此它们不被认为是相似的。

## Python 中的语义文本相似性示例

```
from gensim import corpora
import gensim.downloader as api
from gensim.utils import simple_preprocess
from gensim.matutils import softcossimcorpus = ["my house is empty", "there is no one at mine"]dictionary = corpora.Dictionary([simple_preprocess(doc) for doc in corpus])glove = api.load("glove-wiki-gigaword-50")
sim_matrix = glove.similarity_matrix(dictionary=dictionary)sent_1 = dictionary.doc2bow(simple_preprocess(corpus[0]))
sent_2 = dictionary.doc2bow(simple_preprocess(corpus[1]))print(f"Soft Cosine Similarity: {softcossim(sent_1, sent_2, sim_matrix)}")**>>>> Soft Cosine Similarity: 0.7836213218781843**
```

正如所料，当我们考虑所用句子的上下文时，我们能够识别出我们的文本非常相似，尽管没有很多共同的单词。

## 包裹

读完这篇文章后，你现在知道了什么是文本相似性，以及测量文本相似性的不同方法(即词汇文本相似性和语义文本相似性)。这里有一个给有抱负的数据科学家的想法:如果你是一名求职者，正在寻找进入 NLP 的突破口，一个想法可能是创建一个简历解析器，告诉你你的简历与工作描述有多相似。

感谢阅读！在 [LinkedIn](https://www.linkedin.com/in/kurtispykes/) 和 [Twitter](https://twitter.com/KurtisPykes) 上与我联系，了解我关于人工智能、数据科学和自由职业的最新帖子。

## 相关文章

</5-ideas-for-your-next-nlp-project-c6bf5b86935c>  </sentiment-analysis-predicting-whether-a-tweet-is-about-a-disaster-c004d09d7245>  </introduction-to-machine-translation-5613f834e0be> 