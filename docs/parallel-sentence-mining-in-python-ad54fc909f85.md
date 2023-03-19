# Python 中的平行句挖掘

> 原文：<https://towardsdatascience.com/parallel-sentence-mining-in-python-ad54fc909f85?source=collection_archive---------29----------------------->

## 使用语言无关的 BERT 句子嵌入模型的双文本挖掘

![](img/505f7ec3babd4f4b6949133e0e22097b.png)

照片由乔尼·卡斯帕里在 [Unsplash](https://unsplash.com/s/photos/mining?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

今天的主题是关于在两个单语文件(无序的句子)上进行双文本挖掘，以找到平行的句子。在本教程中，您将学习使用一个名为 LaBSE 的语言无关的 BERT 句子嵌入模型。

从 Tensorflow 中心，它表明 [LaBSE 模型](https://tfhub.dev/google/LaBSE/2)

> …经过训练和优化，专门为相互翻译的双语句子对生成相似的表示。因此，它可以用于在更大的语料库中挖掘句子的翻译。

例如，给定以下文本:

```
That is a happy person
```

当使用来自 LaBSE 模型的嵌入时，您将获得以下句子相似度:

```
That is a happy dog
0.741That is a very happy person
0.897Today is a sunny day
0.450
```

事实上，您也可以在翻译文本上识别句子相似性。让我们重复使用前面的句子

```
That is a happy person
```

并与下面的翻译文本进行比较。

```
Questo è un cane felice
0.624Questa è una persona molto felice
0.757Oggi è una giornata di sole
0.422Questa è una persona felice
0.830
```

它表明

```
Questa è una persona felice
```

与原始句子最相似，因此可能是其翻译文本。

因此，您可以使用这个概念来识别在两个不同的语料库上共享高相似度值的翻译对。假设一个英语语料库包含以下句子

```
This is an example sentences.
Good morning, Peter.
Hello World!
```

你得到了另一个意大利语语料库，如下所示:

```
Salve, mondo!
L'esame è dietro l'angolo
Buongiorno, Peter.
```

通过利用 LaBSE 模型，您将能够执行 bitext 挖掘，以便获得以下正确的平行句子:

```
Hello World!    Salve, mondo!
Good morning, Peter.    Buongiorno, Peter.
```

# 设置

强烈建议您在继续安装之前创建一个新的虚拟环境。

在那之前，你需要在你的系统中安装 [FAISS](https://github.com/facebookresearch/faiss) 。有一个库[为 cpu 和 gpu 安装提供 python 轮子。根据您的使用情况，按如下方式安装它:](https://github.com/kyamagu/faiss-wheels)

```
# CPU
pip install faiss-cpu# GPU
pip install faiss-gpu
```

然后，运行下面的命令来安装`sentence-transformer`，这是一个用于多语言句子、段落和图像嵌入的 BERT 模型。

```
pip install sentence-transformers
```

这个 Python 包不仅限于挖掘平行句。它可用于各种应用，例如:

*   `semantic search`:给定一个句子，在大集合中寻找语义相似的句子。
*   `image search`:将图像和文本映射到同一个向量空间。这允许给定用户查询的图像搜索。
*   给定一篇长文档，找出 k 个句子，这些句子对内容进行了简明扼要的总结。
*   `clustering`:根据句子的相似性将句子分组。
*   `cross-encoder`:两个句子同时出现在变压器网络中，得到一个分数(0…1)，表示相似性或标签。

官方存储库包含一些示例脚本，您可以根据您的用例直接使用:

*   `[bitext_mining_utils.py](https://github.com/UKPLab/sentence-transformers/blob/master/examples/applications/parallel-sentence-mining/bitext_mining_utils.py)`:计算分数、执行 knn 聚类和打开文件的实用程序脚本。
*   `[bitext_mining.py](https://github.com/UKPLab/sentence-transformers/blob/master/examples/applications/parallel-sentence-mining/bitext_mining.py)`:读入两个文本文件(每行一句)，输出平行句为 gzip 文件(parallel-sentences-out.tsv.gz)。
*   `[bucc2018.py](https://github.com/UKPLab/sentence-transformers/blob/master/examples/applications/parallel-sentence-mining/bucc2018.py)`:BUCC 2018 共享任务关于寻找平行句的[示例脚本。](https://comparable.limsi.fr/bucc2018/bucc2018-task.html)

## 概念

让我们深入探讨 bitext 挖掘的底层过程。流程如下:

1.  使用 [LaBSE](https://tfhub.dev/google/LaBSE/2) 将所有句子编码成各自的编码，LaBSE 是一种语言无关的 BERT 句子嵌入模型，支持多达 109 种语言。
2.  下一步是找到所有句子的 k 个最近邻句子(基于两个方向)。使用近似最近邻(ANN)算法，k 应介于 4 和 16 之间。
3.  之后，根据 Artetxe 和 Schwenk(第 4.3 节)的论文[中的公式，对所有可能的句子组合打分。分数可以高于 1。](https://arxiv.org/pdf/1812.10464.pdf)
4.  然后对分数进行排序。高分说明最有可能是骈文。大约 1.2-1.3 的阈值表示高质量的翻译。

虽然在大多数运行中转化率较低，但是该方法适合于从开源的大单语语料库中获取附加的平行句。例如，您可以从维基百科 EN-IT 中提取文本，为您的 EN-IT 机器翻译模型获取额外的数据集。

# bucc2018

对于`bucc2018.py`，数据集必须采用以下语法(由制表符分隔的 id 和文本):

```
id    text
```

例如(每个数据点一行):

```
en-000000005 The Bank of England’s balance sheet is also at 20% of GDP.en-000000007 The most important of these was the creation of regional federations of savings banks.en-000000008 Alongside CECA the central or main clearing bank for Spanish savings banks developed.en-000000010 By 1970 the CECA had attained considerable credibility as a savings bank association.
```

只需从 [BUCC2018 任务](https://comparable.limsi.fr/bucc2018/bucc2018-task.html)中获取数据集，并修改以下几行:

```
source_file = "bucc2018/de-en/de-en.training.de"                       target_file = "bucc2018/de-en/de-en.training.en"                       labels_file = "bucc2018/de-en/de-en.training.gold"
```

该脚本将使用`source_file`和`target_file`执行 bitext 挖掘。然后，它将根据`labels_file`评估结果。

按如下方式运行它:

```
python bucc2018.py
```

在初始运行期间，它将从 HuggingFace 下载模型及其依赖项。如果您使用基于 Linux 的系统，它将存储在以下位置:

```
`~/.cache/huggingface/transformers`
```

接下来，它将对 PCA 的训练嵌入进行编码，以减少维数，从而减少内存消耗，代价是性能略有下降。此外，嵌入将被保存在本地，以便以后可以直接从光盘加载。

您应该在终端上得到以下输出:

```
Shape Source: (32593, 128)
Shape Target: (40354, 128)
Perform approx. kNN search
Done: 5.04 sec
Perform approx. kNN search
Done: 5.03 sec
17039
Best Threshold: 1.22599776371784
Recall: 0.9691714836223507
Precision: 0.9853085210577864
F1: 0.9771733851384167
```

一旦这个过程完成，就会生成一个名为`result.txt`的文件:

```
1.7022742387767527 de-000012693 en-000020744
1.7002160776448074 de-000012458 en-000038209
1.6910422179786977 de-000019004 en-000000538
1.6634726798079071 de-000017671 en-000033396
1.6572470385575508 de-000028406 en-000014338
1.6525712862491655 de-000022783 en-000027704
```

每一行由以下项目组成:

*   得分
*   源句子的 id
*   目标句子的 id

分数表示翻译对的质量。越高越好，并按降序排列。高质量的翻译对通常在 1.3 以上。

# 比特矿业公司

另一方面，`bitext_mining.py`接受不带 id 的普通每行格式的句子

```
The Bank of England’s balance sheet is also at 20% of GDP.The most important of these was the creation of regional federations of savings banks.Alongside CECA the central or main clearing bank for Spanish savings banks developed.By 1970 the CECA had attained considerable credibility as a savings bank association.
```

在以下几行修改数据集的路径:

```
source_file = "data/so.txt.xz"
target_file = "data/yi.txt.xz"
```

这个脚本还过滤掉长度不在`min_sent_len`和`max_sent_len`字符之间的句子。根据您自己的用例修改代码中的以下变量:

```
min_sent_len = 10
max_sent_len = 200
```

在您的终端上运行以下命令:

```
python bitext_mining.py
```

您应该会在控制台上看到以下输出:

```
Read source file
Read target file
Shape Source: (6149, 128)
Shape Target: (5579, 128)
Perform approx. kNN search
Done: 0.84 sec
Perform approx. kNN search
Done: 0.68 sec
2222
```

它将在同一个目录中生成一个名为`parallel-sentences-out.tsv.gz`的新 zip 文件。这个脚本将输出句子而不是句子 id 作为输出:

```
score    source_sentence    target_sentence
```

如果您遇到类似`Segmentation fault (core dumped)`的错误，请仔细检查数据集。你需要有足够多的句子才能让它发挥作用。上述结果在 6149 个源句子和 5579 个目标句子上进行了测试。

# 结论

让我们回顾一下你今天所学的内容。

本文首先简要介绍了 LaBSE 模型，以及如何使用它来计算不同语言的两个文本之间的相似性得分。

然后，它进入安装过程。此外，它还解释了示例脚本和 bitext 挖掘背后的关键概念。

稍后，它将逐步介绍如何运行两个示例脚本，即`bucc2018.py`和`bitext_mining.py`。

感谢你阅读这篇文章。祝你有美好的一天！

# 参考

1.  [Tensorflow Hub — LaBSE](https://tfhub.dev/google/LaBSE/2)
2.  [Github —句子转换器](https://github.com/UKPLab/sentence-transformers)