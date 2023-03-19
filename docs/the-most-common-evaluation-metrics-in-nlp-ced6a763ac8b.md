# NLP 中最常见的评估指标

> 原文：<https://towardsdatascience.com/the-most-common-evaluation-metrics-in-nlp-ced6a763ac8b?source=collection_archive---------5----------------------->

## [自然语言处理笔记](https://towardsdatascience.com/tagged/nlpnotes)

## 了解这些指标

![](img/7d325710c70e9ad3aacdcda5a012d5ee.png)

由[弗勒](https://unsplash.com/@yer_a_wizard?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 介绍

每当我们建立机器学习模型时，我们都需要某种形式的度量来衡量模型的好坏。请记住，模型的“好”可能有多种解释，但通常当我们在机器学习上下文中谈论它时，我们谈论的是模型在不属于训练数据的新实例上的性能度量。

确定用于特定任务的模型是否成功取决于两个关键因素:

1.  对于我们的问题，我们选择的评估标准是否正确
2.  如果我们遵循正确的评估流程

在本文中，我将只关注第一个因素——选择正确的评估指标。

## 不同类型的评估指标

我们决定使用的评估标准取决于我们正在进行的 NLP 任务的类型。进一步补充，项目所处的阶段也会影响我们使用的评估标准。例如，在模型构建和部署阶段，我们通常会使用不同的评估标准来评估产品中的模型。在前两个场景中，ML 指标就足够了，但是在生产中，我们关心业务影响，因此我们更愿意使用业务指标来衡量我们模型的好坏。

也就是说，我们可以将评估指标分为两类。

*   内在评估—关注中间目标(即 NLP 组件在定义的子任务上的性能)
*   外部评估—关注最终目标的性能(即组件在整个应用中的性能)

利益相关者通常关心外在的评估，因为他们想知道模型在解决手边的业务问题方面有多好。然而，为了让人工智能团队衡量他们做得如何，拥有内在的评估指标仍然很重要。在本文的剩余部分，我们将更加关注内在指标。

## 定义指标

评估 NLP 系统的一些常见内在指标如下:

**精度**

无论何时使用精度指标，我们的目标都是了解测量值与已知值的接近程度。因此，它通常用于输出变量为分类变量或离散变量的情况，即分类任务。

**精度**

在我们关心模型预测有多精确的情况下，我们会使用 Precision。精度度量将告知我们对应于分类器标记为阳性的情况，实际标记为阳性的标记的数量。

**回忆**

召回衡量模型能够召回阳性类别的程度(即，模型识别为阳性的阳性标签的数量)。

**F1 比分**

精确度和召回率是具有相反关系的互补指标。如果两者都是我们感兴趣的，那么我们将使用 F1 分数将精确度和召回率结合成一个单一的指标。

要更深入地研究这 4 个指标，请阅读 [*混淆矩阵【未混淆】*](/confusion-matrix-un-confused-1ba98dee0d7f) 。

</confusion-matrix-un-confused-1ba98dee0d7f>  

**曲线下面积(AUC)**

AUC 通过捕捉在不同阈值下正确的阳性预测数和不正确的阳性预测数，帮助我们量化模型的分类能力。

要更深入地了解这一指标，请查看对 AUC-ROC 曲线 的理解 [*。*](/comprehension-of-the-auc-roc-curve-e876191280f9)

</comprehension-of-the-auc-roc-curve-e876191280f9>  

**平均倒数排名(MRR)**

平均倒数排名(MRR)评估检索到的响应，对应于一个查询，给定其正确的概率。这种评估标准通常经常在信息检索任务中使用。

阅读更多关于平均倒数排名的信息。

**平均精度(MAP)**

与 MRR 相似，平均精度(MAP)计算每个检索结果的平均精度。它还被大量用于信息检索，以对检索结果进行排序。

[谭](https://medium.com/u/fdf264797c2a?source=post_page-----ced6a763ac8b--------------------------------)写了一篇名为 [*分解平均精度(图)*](/breaking-down-mean-average-precision-map-ae462f623a52) 的非常好的文章，推荐阅读。

**均方根误差(RMSE)**

当预测结果是真实值时，我们使用 RMSE。这通常与 MAPE(我们将在接下来介绍)结合使用，用于从温度预测到股票市场价格预测等任务的回归问题。

阅读更多关于[均方根误差](https://en.wikipedia.org/wiki/Root-mean-square_deviation)的信息

**平均绝对百分比误差(MAPE)**

当预测结果是连续的时，MAPE 是每个数据点的平均绝对百分比误差。因此，我们用它来测试评估一个回归模型的性能。

阅读更多关于 [MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) 的信息。

**双语评估替角(BLEU)**

BLEU 评分评估由机器从一种自然语言翻译成另一种自然语言的文本的质量。因此，它通常用于机器翻译任务，但是，它也用于其他任务，如文本生成、释义生成和文本摘要。

Jason Brownlee(机器学习大师)写了一篇关于 BLEU 分数的很棒的文章，标题是 [*用 Python 计算文本 BLEU 分数的温和介绍*](https://machinelearningmastery.com/calculate-bleu-score-for-text-python/#:~:text=The%20Bilingual%20Evaluation%20Understudy%20Score,in%20a%20score%20of%200.0.) *。*

**流星**

使用显式排序评估翻译的*指标* (METEOR)是基于精度的机器翻译输出评估指标。它克服了 BLEU 评分的一些缺陷，例如在计算精度的同时进行精确的单词匹配 METEOR 评分允许同义词和词干词与参考单词匹配。该指标通常用于机器翻译任务。

阅读更多关于[流星](https://en.wikipedia.org/wiki/METEOR)的内容。

**胭脂**

与 BLEU 评分相反，*面向回忆的替角用于 Gisting 评估* (ROUGE)评估指标测量回忆。它通常用于评估生成文本的质量和机器翻译任务，但是，由于它测量召回率，因此它主要用于摘要任务，因为评估模型在这些类型的任务中能够召回的单词数更重要。

阅读更多关于[胭脂](https://en.wikipedia.org/wiki/ROUGE_(metric))的内容。

**困惑**

有时我们的 NLP 模型会混淆。困惑是一个很好的概率度量，用来评估我们的模型到底有多困惑。它通常用于评估语言模型，但也可以用于对话生成任务。

Chiara Campagnola 写了一篇非常好的关于困惑评估指标的文章，我推荐阅读。它被命名为 [*语言模型*](/perplexity-in-language-models-87a196019a94) 中的困惑。

## 最后的想法

在本文中，我提供了许多在自然语言处理任务中使用的常见评估指标。这绝不是一个详尽的指标列表，因为在解决 NLP 任务时还会用到更多的指标和可视化。如果你想让我更深入地研究任何一个指标，请留下你的评论，我会努力做到这一点。

感谢您的阅读！通过 [LinkedIn](https://www.linkedin.com/in/kurtispykes/) 和 [Twitter](https://twitter.com/KurtisPykes) 与我联系，了解我关于数据科学、人工智能和自由职业的最新帖子。

## 相关文章

</deep-learning-may-not-be-the-silver-bullet-for-all-nlp-tasks-just-yet-7e83405b8359>  </never-forget-these-8-nlp-terms-a9716b4cccda>  </5-ideas-for-your-next-nlp-project-c6bf5b86935c> 