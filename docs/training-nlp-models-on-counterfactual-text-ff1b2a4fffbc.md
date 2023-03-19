# 在反事实文本上训练自然语言处理模型

> 原文：<https://towardsdatascience.com/training-nlp-models-on-counterfactual-text-ff1b2a4fffbc?source=collection_archive---------32----------------------->

## 西班牙语和阿拉伯语的再反射实验

[今年年初](https://medium.com/codex/counterfactual-text-with-seq2seq-5ca11bef342)，我上传了一个 seq2seq 模型来生成西班牙语的性别反事实(*El professor viejo*<->*la profesora vieja*)。从那以后，我调试了一些问题，做了一个通用库，并为阿拉伯语创建了一个初始的 seq2seq 模型。

目标是看看通过流程传递数据是否会创建更一般化的训练数据(数据扩充),从而提高准确性和公平性。

**这篇文章和笔记本在 2021 年 7 月更新了一个新的、固定的** `**random_state**` **和一个更精确的 SimpleTransformers 版本。**

# 训练阿拉伯语 seq2seq 模型

当我提出这个项目的阿拉伯语版本时， [Wissam Antoun](https://twitter.com/wissam_antoun) 推荐了一篇论文“[自动性别识别和阿拉伯语再输入](https://www.aclweb.org/anthology/W19-3822/)”。阿布扎比 NYU 大学的作者发布了他们改编自 OpenSubtitles 的平行性别语料库。这是少数几篇 NLP 和语言学论文中的一篇，这些论文称这种反事实构建实践为“再反射”。

对数据集的两大警告是:

*   它只有第一人称陈述，这意味着没有翻转一些名词和代词的训练数据(“你见过我的朋友吗？”)和有限的兴趣去翻动其他的词，比如‘母亲’或者‘经理’。
*   阿拉伯语电视和电影通常是埃及阿拉伯语，但其中一些字幕是志愿者翻译的。我没有用方言分类器测试数据集。

我[基于最近的阿拉伯模型训练了几个 seq2seq 模型](https://colab.research.google.com/drive/1TuDfnV2gQ-WsDtHkF52jbn699bk6vJZV):来自 UBC NLP(温哥华)的 MARBERT 和来自 AUB MindLab(贝鲁特)的 AraBERT。我测试了一个方向的准确性(男性-女性或女性-男性句子翻转)，然后是双向任务。我包括了一组有限的第三人称训练示例，但是没有进一步扩展它和编辑其他训练数据，没有一个模型能很好地处理这些。

最终，我在 HuggingFace 上发布了一个基于 MARBERT 模型的解码器和编码器。

# 数据扩充和基准

## 西班牙语

在西班牙语笔记本中，我过滤了一个现有的亚马逊评论数据集。我发现 BETO(一个单语西班牙模型)在预测评论的星级数方面优于 mBERT。

我有两个策略来使用这些性别再反应模型来增加和消除我的训练数据(数据扩充)。对我来说，哪种策略更好并不明显:

*   *将*翻转的示例添加到训练数据中——顺序稍后将被随机化，因此它们将与原始数据混合在一起
*   *用反事实替换训练数据中的*例子

对于 mBERT 来说，当作为一个分类器时，附加数据对问题的改善微不足道；替换数据取得了微小但更可衡量的改进(54.8%比 56.2%)。

## 重新思考我的方法

我对这个实验最大的担心是*花费数小时通过 seq2seq* 运行数千个例子。该库没有使用 GPU 来转换这些示例，因此您可以提前在本地运行它，但它对实验进行了甚至很小的调整和更改，这令人沮丧。

当我对阿拉伯语 seq2seq 模型和笔记本感到困惑时，我离开了这个问题，回来时没有理解我自己的代码。我决定写一个脚本( [GitHub link](https://github.com/MonsoonNLP/seq2seq-for-data-augmentation) )让这两种方法的实验变得更容易，并阅读我的[阿拉伯语笔记本](https://colab.research.google.com/drive/194ITDA1AjxAx_4ZLjoRFQI1aWzsl7xU8?usp=sharing)。

```
append_sequenced(
    "monsoon-nlp/es-seq2seq-gender-encoder",
    "monsoon-nlp/es-seq2seq-gender-decoder",
    [[txt1, label1], ...],
    seq_length=512,     # cap length of input for model
    random_state=404,   # random state for scikit-learn
    frequency=0.5,      # fraction of examples to edit
    always_append=False # append example even if unchanged by flip
)
replace_sequenced(
    "monsoon-nlp/es-seq2seq-gender-encoder",
    "monsoon-nlp/es-seq2seq-gender-decoder",
    [[txt1, label1], ...],
    seq_length=512,
    random_state=404,
    frequency=0.5
)
```

## 阿拉伯实验

我尝试的第一个数据集( [ajgt_twitter_ar](https://huggingface.co/datasets/ajgt_twitter_ar) ，Jordanian Tweets)相当小，但所有分类器都得分很高，不可能显示数据增强的效果。

我最终使用的数据集( [HARD](https://huggingface.co/datasets/hard) ，Hotel Arabic-Reviews 数据集)包含评论和评级(0–3)。

将此视为一个回归问题，当附加示例时，新模型具有*明显更小的误差*(RMSE = 0.2442 至 0.1977)。

在替换时，误差介于前两次运行之间(0.2285)。

我在 Koc 大学(伊斯坦布尔)的 bert-base-arabic 模型上重新运行了这些测试。用一个模型及其嵌入生成示例，并将它们覆盖到另一个模型的嵌入上，可能会有好处[除非 seq2seq 模型基于每个模型会有相同的输出？].

未编辑时，追加时误差为 0.2694
，替换时误差略微降低至 0.2643
，误差也降低至 0.2523

# 此 seq2seq 步骤的用途

替换方法对西班牙分类模型的整体准确性有一个小的积极影响，而附加方法对阿拉伯回归模型总是有一个小的积极影响。当我们考虑整体准确性时，选择特定的模型或随机种子可能是一个更重要的选择。不幸的是，我在 2020 年测试中看到的积极结果很可能是错误的。
阿拉伯模型仍有很大的空间，可以通过更多的例子进行训练，并在翻转方面更加全面。

## 数据集的适用性

评论以第一人称编写，这有助于阿拉伯语 seq2seq 模型在第一人称对话文本上训练。因为评论是关于产品和地点的，所以评论文本可能较少提及性别和其他人。

当涉及到数据来源(员工评论、个人经历、艺术描述)时，更好的是有更多的多样性，或者更好的是，一个专门围绕这些语言的公平性或健壮性的数据集。

对于较小的少数群体来说，一个微小的准确度变化，甚至是对多数群体的负变化，都可能隐藏了显著的准确度变化，我们希望更好地打包和标记数据，以便我们可以衡量和了解如何为每个人提供最佳选择。

## 未来是菲利克斯

展望未来，我会对[谷歌的新 FELIX 架构](https://ai.googleblog.com/2021/05/introducing-felix-flexible-text-editing.html)感兴趣:

> 与 seq2seq 方法相比，速度提高了 90 倍，同时在四个单语生成任务上取得了令人印象深刻的结果

# 更新？

这篇文章发表于 2021 年 5 月。有关任何更新，请参见[GitHub 自述文件。](https://github.com/mapmeld/use-this-now/blob/main/README.md#gender-re-inflection)