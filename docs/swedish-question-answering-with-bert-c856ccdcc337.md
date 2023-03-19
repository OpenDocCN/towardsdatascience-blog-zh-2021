# 用伯特回答瑞典语问题

> 原文：<https://towardsdatascience.com/swedish-question-answering-with-bert-c856ccdcc337?source=collection_archive---------27----------------------->

## 如何微调非英语 BERT

![](img/b594c20e07fa4f49ad4de519ecc942ac.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

问题回答是自然语言处理领域中研究得最多的任务之一，过去几年中的一个亮点是 BERT 的出现。

BERT 和其他基于 Transformer 的模型现在可用于多种语言。然而，对于相对次要的语言，例如瑞典语，可用的模型是有限的或者根本不存在。当我们试图将瑞典语 BERT 微调到一个问题回答任务时，我们面临了几个挑战。这篇文章的目的是记录我们是如何解决这些问题的。

这篇文章的主要重点将是使用 SQuAD 2.0 的问答任务。

**S**tanford**Qu**estion**A**nswering**D**ataset(SQuAD)是一个阅读理解数据集，由一组维基百科文章上的问题组成，其中每个问题的答案都是相应阅读文章中的一段文字或跨度，或者问题可能无法回答。[squad 2.0](https://rajpurkar.github.io/SQuAD-explorer/)【Pranav Rajpurkar，Robin Jia & Percy Liang，2018】将 SQuAD1.1 中的 10 万个问题与超过 5 万个无法回答的问题结合在一起。

# 用 BERT-family 构建瑞典问答系统的三种方法

如果你想用 BERT-family 做一个瑞典语(或者你首选的非英语语言)的问答任务，你可以想出三种方法。

1.  将瑞典语问题翻译成英语，用英语 BERT 处理，并将英语答案翻译回瑞典语
    👍有许多英文资源，包括微调模型。
    👎对每个推理都要进行翻译，这增加了计算成本。有误译的可能。
2.  使用多语言模型处理瑞典语问题
    👍多语言模型也是许多研究的主题，因此有许多可用的资源。
    👎多语言模型比单语模型具有更大的模型大小，这使得当您只想处理瑞典语时，它们的效率很低。
3.  将英语数据集翻译成瑞典语，微调预训练的瑞典语 BERT，并将微调的瑞典语 BERT 用于瑞典语 QA
    👍可以获得相对较小的模型以进行有效的处理。
    👎数据集的误译可能会导致意外的偏差。

在本文中，我们将重点讨论第三个选项，即使用瑞典语 SQuAD 构建一个瑞典语 QA BERT。

# 小队 2.0 的翻译

我们已经使用谷歌翻译 API 自动翻译了 SQuAD 2.0。这听起来像是一项简单的任务，但事实并非如此，原因如下。

*   上下文中决定答案起止的跨度，翻译后可能会发生变化。
*   如果上下文和答案是独立翻译的，则翻译的答案可能不包括在翻译的上下文中。

让我更详细地解释一下第一点。比如《小队 2.0 dev》中的*氧*文字有以下语境(摘自一整句话)、问题、答案。

> ***上下文*** *氧是一种化学元素，符号 O，原子序数 8。*
> 
> ***问题***
> *元素周期表中氧的原子序数？*
> 
> ***回答*** *8*
> 
> *——摘自* [*小队 2.0 dev、*氧气](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/Oxygen.html)

在这种情况下，答案的跨度是从 60 到 61，这表明了摘录的上下文中答案的开始和结束的位置。

这个上下文可以翻译成瑞典语如下。

```
Syre är ett kemiskt grundämne med symbol O och atomnummer 8.
```

现在，答案的正确跨度是从 53 到 54。因此，我们不仅需要翻译上下文和答案，我们还需要正确地跟踪它们在上下文中的位置。

例如，关于第二点，我们在*氧气*文本*中有以下上下文、问题和答案。*

> 关于燃烧和空气之间关系的第一个已知实验是由公元前 2 世纪的希腊力学作家拜占庭的菲洛进行的。
> 
> ***问题***
> *已知的第一个关于燃烧和空气的实验是在哪一年进行的？*
> 
> ***答案*** *公元前二世纪*
> 
> *——摘自* [*小队 2.0 dev、*氧气](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/Oxygen.html)

由 Google 翻译这个上下文，至少现在，会产生以下输出。

```
Ett av de första kända experimenten om förhållandet mellan förbränning och luft utfördes av den grekiska författaren om mekanik under 2000-talet f.Kr. Philo of Byzantium.
```

然而，如果我们独立翻译答案*公元前二世纪*，我们得到如下。

```
2: a århundradet fvt.
```

因此，如果你独立地翻译上下文和答案，在上下文中可能找不到基本事实答案。我们需要连贯地翻译上下文和答案。

# 克服问题的简单策略

为了克服上述困难，我们采用了以下策略。

1.  在翻译之前，在上下文中的答案周围插入特殊标记。比如《小队 2.0 dev》中的*氧*文字有以下语境(摘自一整句话)、问题、答案。

> ***上下文*** *双原子氧气构成了地球大气的 20.8%。然而，对大气含氧量的监测显示，由于化石燃料的燃烧，全球含氧量呈下降趋势。*
> 
> ***问题***
> *哪种气体占地球大气的 20.8%？*
> 
> ***答案*** *双原子氧*
> 
> *——摘自* [*小队 2.0 dev、*氧气](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/Oxygen.html)

在这种情况下，我们在答案*双原子氧*周围插入特殊标记*【0】*，得到

```
[0] Diatomic oxygen [0] gas constitutes 20.8% of the Earth's atmosphere. However, monitoring of atmospheric oxygen levels show a global downward trend, because of fossil-fuel burning.
```

注意，特殊标记不限于*【0】*，我们也可以使用其他标记。

2.翻译标记的上下文。结果会是这样的；

```
[0] Diatomiskt syre [0] gas utgör 20,8% av jordens atmosfär. Övervakning av syrehalten i atmosfären visar dock en global nedåtgående trend på grund av förbränning av fossila bränslen.
```

3.从翻译的上下文中提取标记的句子。这将是翻译的答案。标记的句子的开始和结束将是答案的跨度。

产生的数据集在[github repo](https://github.com/susumu2357/SQuAD_v2_sv)和[拥抱脸数据集](https://huggingface.co/datasets/susumu2357/squad_v2_sv)中可用。

我承认这个策略并不完美。有些答案在上下文中找不到，所以这些不合适的例子必须删除。因此，转换后数据集的大小约为原始数据集的 90%。

# 评估班 2.0 开发

我们微调了由瑞典国家图书馆(KB 实验室)预先训练的[瑞典语 BERT](https://github.com/Kungbib/swedish-bert-models) ，并在我们的 SQuAD 2.0 dev 数据集的瑞典语翻译版上评估了三个模型。

第一个型号是由 deepset GmbH 公司在班培训的[多语种 XLM-罗伯塔。这是上面列出的第二个选项的示例。](https://huggingface.co/deepset/xlm-roberta-large-squad2)

第二个模型是[在小队](https://huggingface.co/KB/bert-base-swedish-cased-squad-experimental)上训练的 KB 实验室模型。这种模式被贴上了“实验性”的标签，但效果很好。

第三个模型是我们的 BERT，它在我们的瑞典版 SQuAD 2.0 训练数据集上进行了微调。

评估结果总结在下表中。

```
╔═════════════════════════════════╦═════════════╦═══════╗
║              **Model              ║ Exact Match ║  F1 **  ║
╠═════════════════════════════════╬═════════════╬═══════╣
║ Multilingual XLM-RoBERTa(large) ║    56.96    ║ **70.78** ║
║ Swedish BERT (base, KB Lab)     ║    65.65    ║ 68.89 ║
║ Swedish BERT (base, Ours)       ║    **66.73**    ║ 70.11 ║
╚═════════════════════════════════╩═════════════╩═══════╝
```

我们的模型具有大约 110M 的参数，获得了比 KB 实验室模型更好的分数，并且与具有大约 550M 参数的 XLM-罗伯塔相比，建立了接近的 F1 分数。

如果你有兴趣使用微调过的模型，该模型可在 [HuggingFace 模型中枢](https://huggingface.co/susumu2357/bert-base-swedish-squad2)中获得。

# 诺贝尔奖数据集

出于评估目的，我们在内部创建了瑞典语的诺贝尔奖数据集。该数据集包含最近诺贝尔物理学奖的描述作为上下文和手动创建的问答对。该数据集包含 91 个问答对，因此大小很小，但对评估很有价值。

由于该数据集是独立创建的，并且该模型以前没有见过该数据集，因此如果我们使用微调模型而不进行进一步训练，则该结果是真实的。

评估结果总结在下表中。我们的模型在精确匹配和 F1 分数上都取得了更好的结果。

```
╔═════════════════════════════════╦═════════════╦═══════╗
║              **Model              ║ Exact Match ║  F1 **  ║
╠═════════════════════════════════╬═════════════╬═══════╣
║ Multilingual XLM-RoBERTa(large) ║    13.19    ║ 60.00 ║
║ Swedish BERT (base, KB Lab)     ║    32.97    ║ 52.41 ║
║ Swedish BERT (base, Ours)       ║    **46.15**    ║ **61.54** ║
╚═════════════════════════════════╩═════════════╩═══════╝
```

# 摘要

提出了一种正确翻译抽取式问答任务数据集的方法。我们在翻译数据集上将我们的微调模型与其他微调模型的性能进行了比较，并确认我们的模型表现相对较好。

# 关于我们

![](img/3571df9004c1e74aea0e879a629a27a6.png)

图片由 Savantic AB 提供

我们是斯德哥尔摩的 [**博学 AB**](https://www.savantic.se/en/startsida-english/)**安艾公司。我们喜欢解决不可能的问题！**

# **参考**

*   **[https://github.com/Kungbib/swedish-bert-models](https://github.com/Kungbib/swedish-bert-models)**
*   **[https://huggingface.co/KB/bert-base-swedish-cased](https://huggingface.co/KB/bert-base-swedish-cased)**
*   **[https://hugging face . co/KB/Bert-base-Swedish-cased-squad-experimental](https://huggingface.co/KB/bert-base-swedish-cased-squad-experimental)**
*   **[https://huggingface.co/deepset/xlm-roberta-large-squad2](https://huggingface.co/deepset/xlm-roberta-large-squad2)**
*   **[https://github.com/susumu2357/SQuAD_v2_sv](https://github.com/susumu2357/SQuAD_v2_sv)**
*   **[https://huggingface.co/datasets/susumu2357/squad_v2_sv](https://huggingface.co/datasets/susumu2357/squad_v2_sv)**
*   **[https://huggingface.co/susumu2357/bert-base-swedish-squad2](https://huggingface.co/susumu2357/bert-base-swedish-squad2)**