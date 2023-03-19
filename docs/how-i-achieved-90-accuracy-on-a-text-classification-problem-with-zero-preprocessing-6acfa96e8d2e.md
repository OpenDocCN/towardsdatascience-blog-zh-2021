# 我如何在一个没有预处理的文本分类问题上达到 90%的准确率

> 原文：<https://towardsdatascience.com/how-i-achieved-90-accuracy-on-a-text-classification-problem-with-zero-preprocessing-6acfa96e8d2e?source=collection_archive---------7----------------------->

## 使用 Spark NLP 充分利用 BERT 句子嵌入的能力

![](img/4345e3459f2141f08757842324a2ea43.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com/s/photos/symbol?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在之前的[帖子](/glove-elmo-bert-9dbbc9226934)中，我展示了不同的单词嵌入(GloVe，ELMo，BERT)如何用于文本分类任务。我们看到了捕捉上下文对于最大化准确性的重要性。

我们还探索了几种预处理文本以改善结果的方法。与此同时，我们想象了每一步是如何改变我们的原始文本的。

今天我们把一切都扔出窗外！我将演示如何在没有任何预处理的情况下达到 90%的分类准确率。你准备好了吗？我想保持这一个简短和甜蜜，但你可以在这里找到我的完整笔记本。

## 数据集

我选择使用 [AG news](http://groups.di.unipi.it/~gulli/AG_corpus_of_news_articles.html) 基准数据集。我从约翰·斯诺实验室的[中恢复了*训练*和*测试*测试(所有 NLP 的东西都必须看参考)。该数据集分为四个平衡的类别，共有 120，000 行，如下所示。](https://github.com/JohnSnowLabs/spark-nlp-workshop/tree/master/tutorials/Certification_Trainings/Public/data)

数据集被格式化为两列，类别和描述。

## 如何在没有预处理的情况下获得 90%的准确率

因为我希望这是一个简洁的帖子，所以我会让你参考我以前的[文章](/glove-elmo-bert-9dbbc9226934)来了解如何在 Colab 中使用 Spark NLP。这只是几段代码。或者，你可以在这里查看我关于那个项目的笔记本。

作为一个简短的回顾，为了将句子嵌入放在有多酷的环境中，考虑下面我用于 GloVe、ELMo 和 BERT 单词嵌入的预处理步骤的概要:

*   将原始文本转换为文档
*   将文档标记化以将其分解成单词
*   规范化标记以删除标点符号
*   删除停用词
*   将剩余的单词简化为它们的引理
*   然后我可以创建单词嵌入

使用 [BERT](https://nlp.johnsnowlabs.com/2020/08/25/sent_bert_base_cased.html) 句子嵌入，唯一需要的步骤是将原始文本转换成文档。完整的管道可以在下面看到。在第一个块中，您可以看到**描述**列中的文本被使用*文档组装器转换为**文档**。*这个**文档**列然后被用作 **BERT 句子嵌入**的输入。最后，这些嵌入被用作*分类器的输入。*就是这样！

## 结果

下面，您将找到一个示例，展示该模型如何在文本子集上执行。**类别**列有标签，而**结果**列有预测类别。

然后，我们可以使用 scikit-learn 来计算我们的指标。如下图所示，总体**准确率为 90%** ！

## 我们今天学了什么？

我想和你分享一个简单而强大的工具来添加到你的 NLP 工具包中。没有任何预处理，这可能是相当耗时的，BERT 句子嵌入用于获得我们的 4 个类别的优秀分类。

我把这个保持简短，只是为了向你介绍句子嵌入的力量。试试看，让我知道你的想法！