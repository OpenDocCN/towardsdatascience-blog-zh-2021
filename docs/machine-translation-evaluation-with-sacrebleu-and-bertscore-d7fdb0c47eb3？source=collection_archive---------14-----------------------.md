# 基于 sacreBLEU 和 BERTScore 的机器翻译评估

> 原文：<https://towardsdatascience.com/machine-translation-evaluation-with-sacrebleu-and-bertscore-d7fdb0c47eb3?source=collection_archive---------14----------------------->

## 评估 MT 模型性能的两个有用的软件包

![](img/86c0bfdca912a8d2df5e0036a5c88953.png)

汉娜·赖特在 [Unsplash](https://unsplash.com/s/photos/language?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

通过阅读本文，您将学会使用以下软件包评估您的机器翻译模型:

*   萨克勒布鲁
*   贝特斯科尔

供您参考，BLEU(双语评估替角)是评估机器翻译文本最流行的指标之一。它可以用来评估任何语言的翻译，只要文本中存在某种形式的单词边界。

BLEU 的输出通常是 0 到 100 之间的分数，表示参考文本和假设文本之间的相似度值。值越高，翻译越好。

话虽如此，BLEU 的一个主要缺点是需要适当地标记文本。例如，您可以使用空格作为分隔符轻松标记任何英语句子。然而，对于缅甸语、汉语和泰语等语言来说，标记化可能具有挑战性。

此外，也有人批评 BLEU 分数的提高并不能保证更好的翻译。当翻译的文本包含传达相同信息的相似单词或同义词时，这是相当明显的。例如，下面的句子从人类的角度来看实际上是相同的，但是当用 BLEU 评估它们时不会得到 100 分。

```
# Reference text
Hi, how are you?# Hypothesis text
Hello, how are you?
```

# 萨克勒布鲁

然而，让我们来探索如何使用 [sacreBLEU](https://github.com/mjpost/sacrebleu) 轻松评估 BLEU 分数，它:

> …轻松计算可共享、可比较和可复制的 **BLEU** 分数。

## 装置

强烈建议您在继续之前创建一个新的虚拟环境。激活它并运行以下命令来安装`sacrebleu`:

```
pip install sacrebleu
```

## 导入

创建一个名为`bleu_scorer.py`的新文件，并在文件顶部附加以下导入语句:

```
from sacrebleu.metrics import BLEU
```

## 参考和假设文本

如下定义假设文本和参考文本:

```
refs = [['The dog bit the guy.', 'It was not unexpected.', 'The man bit him first.']]hyps = ['The dog bit the man.', "It wasn't surprising.", 'The man had just bitten him.']
```

`hyps`由一个文本列表组成，而`refs`必须是一个文本列表列表。在实际的用例中，您应该从文件中读取它，如下所示(去掉结尾的换行符):

```
with open('hyps.txt', 'r', encoding='utf8') as f:
    lines = [x.strip() for x in f]
```

对于多重引用，您应该将其定义如下:

```
refs = [['The dog bit the guy.', 'It was not unexpected.', 'The man bit him first.'],
        ['The dog had bit the man.', 'No one was surprised.', 'The man had bitten the dog.']]hyps = ['The dog bit the man.', "It wasn't surprising.", 'The man had just bitten him.']
```

以下代码强调了多重引用的基本结构:

```
refs = [[ref1_for_sent1, ref1_for_sent2 ...,], [ref2_for_sent1, ref2_for_sent2, ...], ...]
```

## 应用程序接口

然后，使用`metrics.BLEU`面向对象的 API 创建一个新实例:

```
bleu = BLEU()
```

它接受以下输入参数:

*   `lowercase` —如果为真，则计算小写 BLEU。
*   `tokenize` —要使用的标记器。如果没有，则默认为特定于语言的标记化器，并将“13a”作为后备默认值。
*   `smooth_method` —要使用的平滑方法(“floor”、“add-k”、“exp”或“none”)。
*   `smooth_value`—“floor”和“add-k”方法的平滑值。`无'返回默认值。
*   `max_ngram_order` —如果给定，它将在计算精度时覆盖最大 n 元语法顺序(默认值:4)。

最大 ngram 默认为 4，因为根据以下[研究论文](http://aclweb.org/anthology/P/P02/P02-1040.pdf)发现它与单语人类判断的相关性最高。

## 语料库得分

调用`corpus_score`函数计算整个语料库的蓝分:

```
result = bleu.corpus_score(hyps, refs)
```

您可以在以下[要点](https://gist.github.com/wfng92/91e596f294c488ce67359e70f945b429)中找到完整的代码:

## 输出

输出如下所示:

```
29.44 82.4/42.9/27.3/12.5 (BP = 0.889 ratio = 0.895 hyp_len = 17 ref_len = 19)
```

*   `29.44`指最终的 BLEU 分数
*   `82.4/42.9/27.3/12.5`表示 1–4 ngram 顺序的精度值
*   `BP`是简洁的惩罚
*   `ratio`表示假设长度与参考长度之比
*   `hyp_len`指假设文本的总字符数
*   `ref_len`是参考文本的总字符数

您可以在下面的[库](https://github.com/mjpost/sacrebleu/blob/master/sacrebleu/metrics/bleu.py)找到关于每个指标的更多信息。

# 贝特斯科尔

另一方面，`BERTScore`有一些与 BLEU 不同的突出之处。它

> …利用来自 BERT 的预训练上下文嵌入，并通过余弦相似度匹配候选句子和参考句子中的单词。
> 
> 它已被证明与人类对句子级和系统级评估的判断相关。此外，BERTScore 还计算精确度、召回率和 F1 值，这对于评估不同的语言生成任务非常有用。

这为使用同义词或相似词的句子提供了更好的评估。例如:

```
# reference text
The weather is cold today.# hypothesis text
It is freezing today.
```

## 装置

运行以下命令通过`pip install`安装`BERTScore`:

```
pip install bert-score
```

## 导入

创建一个名为`bert_scorer.py`的新文件，并在其中添加以下代码:

```
from bert_score import BERTScorer
```

## 参考和假设文本

接下来，您需要定义引用和假设文本。BERTScore 接受文本列表(单个引用)或文本列表列表(多个引用):

```
refs = [['The dog bit the guy.', 'The dog had bit the man.'],                               ['It was not unexpected.', 'No one was surprised.'],                               ['The man bit him first.', 'The man had bitten the dog.']]hyps = ['The dog bit the man.', "It wasn't surprising.", 'The man had just bitten him.']
```

与 sacreBLEU 相比，多引用的结构略有不同。它基于以下语法:

```
refs = [[ref1_for_sent1, ref2_for_sent1 ...,], [ref1_for_sent2, ref2_for_sent2, ...], ...]
```

## 应用程序接口

实例化 BERTScorer 面向对象 API 的新实例，如下所示:

```
scorer = BERTScorer(lang="en", rescale_with_baseline=True)
```

它接受:

*   `model_type` —上下文嵌入模型规范，默认使用目标语言的建议模型；必须指定`model_type`或`lang`中的至少一个
*   `verbose` —打开中间状态更新
*   `device` —如果没有，则模型存在于 cuda 上:如果 cuda 可用，则为 0
*   `batch_size` —加工批量
*   `nthreads` —螺纹数量
*   `lang` —句子的语言。当`rescale_with_baseline`为真时需要指定
*   `rescale_with_baseline` —使用预先计算的基线重新调整伯特得分
*   `baseline_path` —定制的基线文件
*   `use_fast_tokenizer` —使用 HuggingFace 的快速标记器

默认情况下，它基于指定的`lang`使用以下模型:

*   `en` —罗伯塔-大号
*   `en-sci`—allenai/scibert _ scivocab _ uncased
*   `zh`—Bert-base-中文
*   `tr` —dbmdz/Bert-base-Turkish-cased

语言的其余部分将基于 Bert-base-多语言环境。如果您打算定制该型号，请查看以下[谷歌表单](https://docs.google.com/spreadsheets/d/1RKOVpselB98Nnh_EOC4A2BYn8_201tmPODpNWu4w7xI/edit?usp=sharing)了解更多信息。

重新调整基线有助于调整输出分数，使其更具可读性。这仅仅是因为一些上下文嵌入模型倾向于在非常窄的范围内产生分数。你可以在下面的[博客文章](https://github.com/Tiiiger/bert_score/blob/master/journal/rescale_baseline.md)中找到更多信息。

这个存储库还包含一些预先计算好基线文件。只需前往下面的[库](https://github.com/Tiiiger/bert_score/tree/master/bert_score/rescale_baseline)并根据您打算使用的模式下载所需的文件。

它将在初始运行时从 HuggingFace 下载模型和依赖项。随后，它将重用相同的下载模型。在 Linux 操作系统上，该模型位于以下目录中:

```
~/.cache/huggingface/transformers/
```

## 语料库得分

如下调用`score`函数:

```
P, R, F1 = scorer.score(hyps, refs)
```

它将返回以下项的张量:

*   `P` —精度
*   `R` —回忆
*   `F1`—F1-得分

以下是 F1 分数的输出示例:

```
tensor([0.9014, 0.8710, 0.5036, 0.7563, 0.8073, 0.8103, 0.7644, 0.8002, 0.6673, 0.7086])
```

请注意，基础值的范围是从-1 到 1。

通过调用如下的`mean`函数，可以很容易地计算出语料库的分数:

```
print(F1.mean())
# 0.6798698902130217
```

完整代码位于以下[要点](https://gist.github.com/wfng92/c25848d7e22b5b7d86e20469a0d38783):

请注意，与 sacreBLEU 相比，运行 BERTScore 要慢很多。请确保您在评估期间有足够的 GPU 内存。

要了解更多关于 BERTScore 的信息，请查看官方资料库中的[示例笔记本](https://github.com/Tiiiger/bert_score/blob/master/example/Demo.ipynb)。

# 结论

让我们回顾一下你今天所学的内容。

本文首先简要介绍了 BLEU，以及用它来评估译文的优缺点。

接下来，它讲述了如何使用 sacreBLEU 来计算语料库级别的 BLEU 分数。输出还包括 1–4n gram 的精度值。

随后，在 BERTScore 上探讨了用余弦相似度评价译文的方法。`score`函数分别返回精度、召回率和 F1 分数的张量。

感谢你阅读这篇文章。祝你有美好的一天！

# 参考

1.  [Github — sacreBLEU](https://github.com/mjpost/sacrebleu)
2.  [Github — BERTScore](https://github.com/Tiiiger/bert_score)