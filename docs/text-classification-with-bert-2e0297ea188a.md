# 用 BERT 升级你的初学者 NLP 项目

> 原文：<https://towardsdatascience.com/text-classification-with-bert-2e0297ea188a?source=collection_archive---------11----------------------->

## 深度学习不一定要复杂

![](img/cf7ee006139dcd9be8676cdc9321f06b.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

当我刚开始学习数据科学和看项目时，我认为你可以做深度学习或常规项目。事实并非如此。

随着强大的模型变得越来越容易获得，我们可以轻松地利用深度学习的一些功能，而不必优化神经网络或使用 GPU。

在这篇文章中，我们将看看**嵌入**。这是深度学习模型将单词表示为向量的方式。我们可以将模型的一部分生成嵌入，并在上面安装一个常规的( *scikit-learn* )模型，以获得一些令人难以置信的结果！

我将分别解释每种方法，用图表来表示它的工作原理，并展示如何在 Python 中实现这些技术。

# 目录

*   [先决条件](#b225)
*   [将单词表示为向量](#ca97)
*   [单词包方法](#9bce)
    - [计数向量器](#924f)-
    -[TF-IDF](#2880)
*   [嵌入文字作为向量](#b612)
    -[word 2 vec](#b525)
    -[手套](#3f73)
    - [Doc2Vec](#1bdd)
*   [基于变压器的型号](#cc62)
    - [通用语句编码器](#51c1)-
    BERT
*   [结论](#f3cb)

# 先决条件

*   你应该了解**机器学习**的基础知识。
*   为了充分利用这一点，您应该知道如何在 *scikit-learn* 中拟合模型，并且已经有了一个适合于 **NLP** 的数据集。
*   本教程对于已经有一个 **NLP** 项目并且希望升级它并尝试深度学习的人来说非常理想。
*   本文中的每个模型都增加了复杂性。本文将解释基本原理以及如何使用该技术，但是您可能希望访问一些提供的链接来完全理解这些概念。

# 资料组

为了说明每个模型，我们将使用 Kaggle *NLP 和灾难推特*数据集。这是大约 10，000 条推文，这些推文是根据关键词(例如*着火*)挑选出来的，然后标记它们是否是关于一场真正的灾难。

您可以在此阅读比赛内容并查看结果:

<https://www.kaggle.com/c/nlp-getting-started>  

**您可以在这里查看或克隆所有代码:**

<https://github.com/AdamShafi92/Exploring-Embeddings>  

# 可视化

我们将使用 2 个**可视化**来探索每个模型。我在下面列举了一些例子。

**将文字形象化……**

一个 [UMAP](https://pair-code.github.io/understanding-umap/) 表示所有的句子。UMAP 是一种降维方法，它允许我们仅在二维空间中查看高维度的单词表示。

> **降维**是将数据从高**维**空间转换到低**维**空间，以便低**维**表示保留原始数据的一些有意义的属性，理想情况下接近其固有**维**。

这对于可视化主题集群非常有用，但是如果你以前没有遇到过降维，这可能会令人困惑。我们本质上只是在寻找我们的词被分成集群，其中具有相似主题的**推文在空间上彼此接近。蓝色(非灾难)和橙色(灾难)文本之间的清晰区分也是很好的，因为这表明我们的模型能够很好地对这些数据进行分类。**

作者图表。关于灾难的推文的 UMAP 表现。鼠标悬停时打开原件显示推文。

**评估模型性能…**

一组 5 张图表。从左至右:

1.  **ROC AUC。这是一个典型的评分系统，允许我们比较模型。它考虑了预测的概率**
2.  **精度/召回**。另一个典型指标是，我们正在寻找一个大而平滑的 AUC。
3.  **特征重要性**。这样我们就可以比较每种方法的效果。对伯特来说，这不会显示太多，但有助于说明可解释性
4.  **预测概率**。这使我们能够直观地看到模型如何区分这两个类别。理想情况下，我们希望看到 0 和 1 的集群只有很少的 50%左右。
5.  **混淆矩阵**。我们可以想象假阳性对假阴性。

作者图表。评估模型性能的 5 幅图。

# 定义

*   **矢量:**矢量的经典描述是一个既有大小又有方向的量(例如向西 5 英里)。在机器学习中，我们经常使用高维向量。
*   **嵌入:**将一个词(或句子)表示为向量的一种方式。
*   **文档:**一个单独的文本。
*   **语料库:**一组文本。

# 将单词表示为向量

为了创建基于单词的模型，我们必须将这些单词转换成数字。最简单的方法是对每个单词进行热编码，并告诉我们的模型:

*   **句子#1** 有单词#1，单词#12，单词#13。
*   **句子#2** 有单词#6，单词#24，单词#35。

单词袋和 TDF-IDF 以这种方式表示单词，并在此基础上增加了一些单词出现频率的度量。

> 单词包方法通过简单地为每个单词创建一个列并用一个数字表示该单词出现的位置来将单词表示为向量。向量的大小与语料库中唯一单词的数量相同。

这对于某些方法来说很好，但是我们丢失了关于在同一个句子中有不同意思的单词的信息，或者上下文如何改变一个单词的意思。

> 将单词转换成数字或向量，称为嵌入单词。我们可以把一组变成向量的单词描述为嵌入。

我们对单词进行矢量化的目的是以一种尽可能获取更多信息的方式来表示单词…

> 我们如何告诉模型一个词和另一个词相似？它是怎么知道完全不同的单词意思是一样的呢？或者另一个单词如何改变它后面的单词的意思呢？或者甚至当一个单词在同一个句子中有多个意思的时候？([水牛水牛水牛水牛水牛水牛水牛水牛水牛](https://en.wikipedia.org/wiki/Buffalo_buffalo_Buffalo_buffalo_buffalo_buffalo_Buffalo_buffalo) —我在看你)

深度学习已经允许开发各种技术，这些技术在回答大多数这些问题方面有很大的帮助。

# 单词袋方法

这是最简单的表示单词的方式。我们将每个句子表示为一个向量，取语料库中的所有单词，根据每个单词是否出现在句子中，给每个单词一个 1 或 0。

你可以看到随着**字数的增加**，它会变得非常大**。这个问题是我们的矢量开始变得**稀疏**。如果我们有很多包含各种单词的短句，我们的数据集中就会有很多 0。稀疏会成倍增加我们的计算时间。**

**我们可以通过对每个单词进行**计数**，而不仅仅是 1 或 0，来“升级”一包单词的表示。当我们进行计数时，我们也可以删除在语料库中不常出现的单词，例如，我们可以删除出现次数少于 5 次的每个单词。**

**另一种提高单词量的方法是使用 n-grams。这只是用了 n 个单词，而不是 1 个。这有助于捕捉句子中更多的上下文。**

## **计数矢量器**

****直觉****

**这是矢量语言最简单的方法。我们简单地计算句子中的每个单词。在大多数情况下，建议删除非常常见的单词和非常罕见的单词。**

****实施****

```
from sklearn.feature_extraction.text import CountVectorizerbow = CountVectorizer(min_df=5,max_df=.99, ngram_range=(1, 2)) #remove rare and common words with df parameter
#include single and 2 word pairsX_train_vec = bow.fit_transform(X_train[‘text’])
X_test_vec = bow.transform(X_test[‘text’])cols = bow.get_feature_names() #if you need feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

****可视化****

**这个 4000 维向量的 2d 表示并不好。我们在中间有一个斑点和许多不同的点。我们的模型没有明确的方法来聚集或分离数据。**

****您可以打开图，将鼠标悬停在上面，查看每个点是什么。****

**作者图表。按“查看原文”打开互动版本，查看原始推文。**

**无论如何，我们的模型表现得相当好，它能够区分一些相当数量的推文。然而，从特性的重要性我们可以看出，它主要是通过使用**URL**来做到这一点的。这是发现灾难微博的有效方法吗？**

**作者图表。评估模型性能的 5 幅图。**

## **TF-IDF**

****直觉****

**使用单词包和计数的一个问题是，频繁出现的单词，如*和*，开始主导特征空间，而不提供任何附加信息。可能有更重要的特定领域的单词，但是由于它们不经常出现而被模型丢失或忽略。**

**TF-IDF 代表**词频—逆文档频率****

*   ****词频**:该词在当前文档中的频率得分。**
*   ****逆文档频率**:对单词在语料库中的稀有程度进行评分。**

**在 TF-IDF 中，我们像在单词包中一样，使用单词的频率对单词进行评分。然后**惩罚**在所有文档中**频繁出现的**单词(比如 the、and、or)。**

**我们也可以将 n-grams 与 TF-IDF 一起使用。**

****实施****

```
from sklearn.feature_extraction.text import TfidfVectorizertfidf= TfidfVectorizer(min_df=5,max_df=.99, ngram_range=(1, 2)) #remove rare and common words with df parameter
#include single and 2 word pairsX_train_vec = tfidf.fit_transform(X_train[‘text’])
X_test_vec = tfidf.transform(X_test[‘text’])cols = tfidf.get_feature_names() #if you need feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

****可视化****

**TF-IDF 与该数据集上的计数矢量器没有太大区别。灾难和非灾难推文之间仍然有很多重叠。**

**作者图表。按“查看原文”打开互动版本，查看原始推文。**

**通过使用 TF-IDF，我们看到了模型性能的小幅提升。一般来说，这确实表现得更好，因为我们降低了通常不会给模型增加任何东西的常见单词的权重。**

**作者图表。评估模型性能的 5 幅图。**

# **将单词作为向量嵌入**

**单词袋模型有 3 个关键问题:**

1.  ****相似的词互不相关。**模型不知道*不好*和*可怕*这两个词是相似的，只知道这两个都和负面情绪有关。**
2.  **单词不在上下文中。讽刺甚至*还不错*可能都没捕捉好。具有双重含义的单词不会被捕获。**
3.  **使用大型语料库会产生非常大的稀疏向量。这使得大规模计算变得困难。**

> **通过深度学习，我们从简单的表示转移到嵌入。与之前的方法不同，深度学习模型通常**输出固定长度的向量**，该向量**不必与语料库**中的字数相同。我们现在为数据集中的每个单词或句子创建一个唯一的向量表示。**

## **Word2Vec**

**Word2Vec 是一种生成嵌入的深度学习方法，发表于 2013 年。它可以相对容易地在你的语料库上进行训练，但本教程的目的是使用预训练的方法。我将简要地解释模型是如何被训练的。**

**这个模型有两种训练方式。**

*   ****Skip-gram:** 模型循环遍历句子中的每个单词，并尝试预测相邻的单词。**
*   ****连续单词包:**模型循环遍历每个单词，并使用周围的 *n 个*单词来预测它。**

**要深入了解这一模式，只需看看杰伊·阿拉姆的这篇精彩文章就够了。**

****实施****

**为了实现 Word2Vec，我们将使用一个在 Gensim 的 **Google News** 数据集上训练的版本。该模型为每个单词输出大小为 300 的向量。理论上，相似的单词应该有相似的向量表示。**

**Word2Vec 和 GLoVe 的一个问题是我们不能轻易生成一个**句子**嵌入。**

**要生成嵌入 Word2Vec 或 GLoVe 的句子，我们必须为每个单词生成一个 300 大小的向量，然后**对它们进行平均**。这样做的问题是，尽管相似的句子应该有相似的句子向量，但我们丢失了任何关于单词的**顺序的信息。****

```
import gensim
import gensim.models as g
import gensim.downloader
from spacy.tokenizer import Tokenizer
from spacy.lang.en import English def vectorize_sentence(sentence,model):
    nlp = English()
    tokenizer = Tokenizer(nlp.vocab)
    a = []
    for i in tokenizer(sentence):
        try:
            a.append(model.get_vector(str(i)))
        except:
            pass

    a=np.array(a).mean(axis=0)
    a = np.zeros(300) if np.all(a!=a) else a
    return aword2vec = gensim.downloader.load('word2vec-google-news-300') #1.66 gb# vectorize the dataX_train_vec = pd.DataFrame(np.vstack(X_train['text'].apply(vectorize_sentence, model=word2vec)))
X_test_vec = pd.DataFrame(np.vstack(X_test['text'].apply(vectorize_sentence, model=word2vec)))# Word2Vec doesn't have feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

****可视化****

**乍一看，Word2Vec 似乎比以前的方法更好地表示了我们的数据。有清晰的蓝色区域和单独的橙色区域。左上角的群集似乎主要是大写字母的单词，在其他地区有关于天气的推文。**

**作者图表。按“查看原文”打开互动版本，查看原始推文。**

**不幸的是，乍一看，这与模型性能无关。精度得分比 TF-IDF 稍差。然而，如果我们看看混淆矩阵，我们可以看到这个模型在识别灾难微博方面做得更好。**

**这里的一个大问题是，我们现在不知道是什么推动了这些更好的预测。有一个特性很明显被模型使用的比其他的多，但是不做额外的工作我们无法发现这代表了什么。**

**作者图表。评估模型性能的 5 幅图。**

## **手套**

****直觉****

**手套代表 Glo bal。**

**GloVe 类似于 Word2Vec，因为它是一种早期的嵌入方法，已于 2014 年发布。然而，GloVe 的关键区别在于，GloVe 不仅仅依赖于附近的单词，而是结合了**全局统计** — **跨语料库**的单词出现，以获得单词向量。**

**训练 GloVe 的方式是通过计算语料库中每个单词的共现矩阵。然后，对这个矩阵进行某种类型的降维，将其缩减到一个固定的大小，为每个句子留下一个向量。我们可以很容易地访问这个模型的预训练版本。如果你想知道更多关于它是如何工作的，请看这里。**

****实施****

**我们使用的是 GloVe ' *Gigaword* '模型，它是在维基百科**语料库上训练的。您会注意到它的大小比 Word2Vec 模型小得多，这表明它可能只训练了较少的单词。这是一个问题，因为 GLoVe 不能识别我们数据集中的一个单词，它将返回一个**错误**(我们用 0 代替……)。****

```
import gensim
import gensim.models as g
import gensim.downloader
from spacy.tokenizer import Tokenizer
from spacy.lang.en import Englishdef vectorize_sentence(sentence,model):
    nlp = English()
    tokenizer = Tokenizer(nlp.vocab)
    a = []
    for i in tokenizer(sentence):
        try:
            a.append(model.get_vector(str(i)))
        except:
            pass

    a=np.array(a).mean(axis=0)
    a = np.zeros(300) if np.all(a!=a) else a
    return agv = gensim.downloader.load('glove-wiki-gigaword-300') #376mb# vectorize the dataX_train_vec = pd.DataFrame(np.vstack(X_train['text'].apply(vectorize_sentence, model=gv)))
X_test_vec = pd.DataFrame(np.vstack(X_test['text'].apply(vectorize_sentence, model=gv)))# GloVe doesn't have feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

****可视化****

**手套向量很有趣，右上角的区域是每个单词首字母大写的推文。这不是我们有兴趣区分的东西。否则蓝色和橙色会有很多重叠。**

**作者图表。按“查看原文”打开互动版本，查看原始推文。**

**到目前为止，我们的手套模型表现明显比其他模型差。最可能的原因是这个模型不理解我们语料库中的许多单词。为了解决这个问题，你必须自己在语料库(或一些 Twitter 数据)上训练这个模型。**

**作者图表。评估模型性能的 5 幅图。**

## ****Doc2Vec****

****直觉****

**GLoVe 和 Word2Vec 的关键问题在于**我们只是对整个句子**进行平均。Doc2Vec 针对句子进行了预训练，应该可以更好地表示我们的句子。**

****实现****

**Doc2Vec 不是 Gensim 库的一部分，所以我在网上找到了一个已经过预训练的版本，但是我不确定是什么版本。**

```
# Model downloaded from [https://ai.intelligentonlinetools.com/ml/text-clustering-doc2vec-word-embedding-machine-learning/](https://ai.intelligentonlinetools.com/ml/text-clustering-doc2vec-word-embedding-machine-learning/)
#[https://ibm.ent.box.com/s/3f160t4xpuya9an935k84ig465gvymm2](https://ibm.ent.box.com/s/3f160t4xpuya9an935k84ig465gvymm2)# Load unzipped model, saved locally
model="../doc2vec/doc2vec.bin"
m = g.Doc2Vec.load(model)# Instantiate SpaCy Tokenizer
nlp = English()
tokenizer = Tokenizer(nlp.vocab)# Loop Through texts and create vectorsa=[]
for text in tqdm(X_train['text']):
    a.append(m.infer_vector([str(word) for word in tokenizer(text)]))

X_train_vec = pd.DataFrame(np.array(a))

a=[]
for text in tqdm(X_test['text']):
    a.append(m.infer_vector([str(word) for word in tokenizer(text)]))

X_test_vec = pd.DataFrame(np.array(a))# Doc2Vec doesn't have feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

****可视化****

**我曾期待这款产品能带来巨大的成功，但它并没有实现。最左边的区域是带有@的推文，而最右边的区域主要是 URL。这个模型很好地处理了这些问题(尽管对完整的句子进行了编码)，但我们要寻找的是比这更细微的差别。**

**作者图表。按“查看原文”打开互动版本，查看原始推文。**

**我的上述意见反映在模型中，这表现得像手套一样糟糕。**

**作者图表。评估模型性能的 5 幅图。**

# **基于变压器的模型**

**我不会在这里谈论太多细节，但理解基于 transformer 的模型是值得的，因为自 2017 年谷歌论文发布以来，这种模型架构已经导致了我们在过去几年中看到的最先进的 NLP 模型的爆炸。**

**即使这些模型是最近才发布的，并且是在大型数据集上训练的，我们仍然可以使用高级 python 库来访问它们。是的，我们可以利用最先进的深度学习模型，只需几行代码。**

## **通用句子编码器**

**<https://amitness.com/2020/06/universal-sentence-encoder/>  

谷歌的**通用句子编码器**包括一个变压器架构和深度平均网络。当发布时，它实现了最先进的结果，因为传统上，句子嵌入是对整个句子进行平均的。在通用句子编码器中，每个单词都有影响。

与 Word2Vec 相比，使用它的主要好处是:

1.  使用 Tensorflow Hub 非常容易。该模型自动为整个句子生成一个嵌入。
2.  该模型比 Word2Vec 更好地捕捉了**词序**和**上下文**。

更多信息，请看 t [他的解释](https://amitness.com/2020/06/universal-sentence-encoder/)和[谷歌的下载页面](https://tfhub.dev/google/universal-sentence-encoder/4)。

**实施**

这是最容易实现的模型之一。

```
import tensorflow_hub as hubdef embed_document(data):
    model = hub.load("../USE/")
    embeddings = np.array([np.array(model([i])) for i in data])
    return pd.DataFrame(np.vstack(embeddings))# vectorize the dataX_train_vec = embed_document(X_train['text'])
X_test_vec = embed_document(X_test['text'])# USE doesn't have feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

**可视化**

现在这个**就是**有意思。橙色和蓝色很容易区分。悬停在推文上，很明显语义相似的推文彼此接近。

如果您运行了代码，您还会注意到这个模型嵌入句子的速度非常快，这是一个很大的好处，因为 NLP 工作会因为数据量大而变慢。

作者图表。按“查看原文”打开互动版本，查看原始推文。

不出所料，这款机型表现非常好。尽管精度仅比 TF-IDF 略高，但我所观察的每一项指标都有所提高。

可解释性仍然是一个问题。一个特性似乎比其他的更重要，但是它对应的是什么呢？

作者图表。评估模型性能的 5 幅图。

## 伯特

<https://github.com/UKPLab/sentence-transformers>  

BERT 代表来自变压器的双向编码器表示。它是一个深度学习模型，具有 **transformer** 架构。该模型以类似于 Word2Vec 的方式进行训练，即在句子中间屏蔽一个单词，并让模型填充空白。给定一个输入句子，它还被训练预测下一个句子。

伯特接受了来自英语维基百科和图书语料库数据集的超过 300 万个单词的训练。

在引擎盖下，有两个关键概念:

**嵌入:**单词的向量表示，其中相似的单词彼此“接近”。BERT 使用“单词块”嵌入(30k 单词)加上句子嵌入来显示单词在哪个句子中，位置嵌入代表每个单词在句子中的位置。然后可以将文本输入到 BERT 中。

**注意:**核心思想是每次模型预测一个输出单词时，它只使用输入中最相关信息集中的部分，而不是整个序列。用更简单的话来说，它只关注一些输入词。

然而，我们真的不需要担心这一点，因为我们有办法用几行代码生成嵌入。

**实施**

伯特的言辞非常有力。当进行微调时，该模型能够很好地捕捉语义差异和词序。

[句子转换包](https://medium.com/r?url=https%3A%2F%2Fgithub.com%2FUKPLab%2Fsentence-transformers)允许我们利用预训练的 BERT 模型，这些模型已经过特定任务的训练，如语义相似性或问题回答。这意味着我们的嵌入是针对特定任务的。这也使得生成完整句子的嵌入非常容易。

在这个例子中，我使用了 RoBERTa，它是脸书的 BERT 的优化版本。

```
from sentence_transformers import SentenceTransformerbert = SentenceTransformer('stsb-roberta-large') #1.3 gb# vectorize the dataX_train_vec = pd.DataFrame(np.vstack(X_train['text'].apply(bert.encode)))
X_test_vec = pd.DataFrame(np.vstack(X_test['text'].apply(bert.encode)))# BERT doesn't have feature namesmodel = RandomForestClassifier(n_estimators=500, n_jobs=8)
model.fit(X_train_vec, y_train)
model.score(X_test_vec, y_test)
```

**观想**

很难说这是不是比通用的句子编码器版本更好。我的直觉是，这个模型在区分灾难和非灾难推文方面做得更差，但在聚类类似主题方面可能做得更好。可惜我目前只能衡量前者！

作者图表。按“查看原文”打开互动版本，查看原始推文。

这个模型客观上比通用的句子编码器差。一个特性比其他的更重要，我希望这对应于 URL，也许模型对这些的权重太大了，但是不能从其他 1023 个向量中提取细节。

作者图表。评估模型性能的 5 幅图。

# 结论

我们探索了多种将单词转化为数字的方法。在这个数据集上，谷歌的通用句子编码器表现最好。对于大多数应用程序来说，这是值得一试的，因为它们的性能非常好。我觉得 Word2Vec 现在有点过时了，USE 之类的方法这么快，这么厉害。

我们很多人第一次学习 NLP 的方法是通过做一个**情感分析**项目，用一个**单词包**表示文本。这是一种很好的学习方式，但我觉得它带走了 NLP 的很多乐趣。一袋单词和一个热编码数据没有太大区别。产生的模型不是特别有效，而且很少能捕捉到文本中的任何细微差别。**我们可以轻松采用 BERT 嵌入，这通常会带来巨大的性能提升。**

作为最后一点，总是值得考虑模型**可解释性**和**可解释性**。使用单词袋方法，我们可以清楚地说出哪些单词影响了模型。在伯特模型中，我们可以很容易地说出向量中的哪个位置影响了模型，但要说出每个向量的确切含义却需要相当大的努力(而且几乎是不可能的)。一个悬而未决的问题是——**伯特使用 URL 预测灾难了吗？还是它对语言理解得更好？**

## 了解更多信息

</simple-logistic-regression-a9735ed23abd>  <https://adam-shafi.medium.com/tech-skills-2021-5805848851c6>  

## 联系我

<https://www.linkedin.com/in/adamshafi/> **