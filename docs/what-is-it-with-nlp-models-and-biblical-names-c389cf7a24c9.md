# NLP 模型和圣经中的名字有什么关系？

> 原文：<https://towardsdatascience.com/what-is-it-with-nlp-models-and-biblical-names-c389cf7a24c9?source=collection_archive---------25----------------------->

## 不同 NER 模型的比较及其检测不同命名实体的能力。

GIF via giphy——来源:[http://www.amc.com/shows/breaking-bad](http://www.amc.com/shows/breaking-bad)

这篇博客文章强调了 spaCy 和其他命名实体识别模型无法准确检测人名的问题，特别是如果他们是圣经中的名字。常规名字和圣经名字之间的检测差异是相当大的。

你会看到，对于我们能想到的最简单的例子，“我的名字是 X”，圣经中的名字几乎从来没有被发现是人名。

我试图弄清楚这件事，并相信我有一个答案。但首先，让我们用两个 spaCy 模型(使用 spaCy 版本 3.0.5)做一个简短的实验。

## 比较圣经名字和其他名字的检出率

为什么一开始就有区别？不同检测率的原因可能是:

1.  事实上，圣经中的名字有时更老，更不常见(因此在模型被训练的数据集中可能不太常见)。
2.  周围的句子不太可能与原始数据集中的特定名称同时出现。
3.  数据集本身的问题(例如人为的错误注释)。

为了(简单地)测试这些假设，我们将圣经中的名字与新旧名字以及三个模板进行比较，其中两个模板来自圣经:

*   “我的名字是 X”
*   " X 说，你为什么连累我们？"
*   她又怀孕，生了一个儿子。她叫他的名字 x。”

让我们从创建名单和模板开始:

运行空间模型并检查是否检测到“人”的方法:

## 模型 1: spaCy 的`en_core_web_lg`模型

*   该模型使用原始(不基于变压器)空间架构。
*   它在[on notes 5.0](https://catalog.ldc.upenn.edu/LDC2013T19)数据集上进行训练，并在命名实体上具有 [0.86 F-measure。](https://spacy.io/models/en#en_core_web_lg)

加载模型:

让我们运行它:

以下是我们得到的结果:

```
Model name: en_core_web_lg
Name set: Biblical, Template: "My name is {}"
Recall: 0.25Name set: Other, Template: "My name is {}"
Recall: 0.94Name set: Biblical, Template: "And {} said, Why hast thou troubled us?"
Recall: 0.67Name set: Other, Template: "And {} said, Why hast thou troubled us?"
Recall: 0.94Name set: Biblical, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.58Name set: Other, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.94Detailed results:
{('And she conceived again, a bare a son; and she called his name {}.', 'Biblical'): {
'Abraham': True,
'David': True,
'Isaac': False,
'Jacob': False,
'Jesus': False,
'John': True,
'Judas': False,
'Mary': True,
'Matthew': True,
'Moses': False,
'Samuel': True,
'Simon': True},('And she conceived again, a bare a son; and she called his name {}.', 'Other'): {
'Ariana': True,
'Barack': True,
'Beyonce': True,
'Bill': True,
'Charles': True,
'Coby': False,
'Donald': True,
'Frank': True,
'George': True,
'Helen': True,
'Joe': True,
'Katy': True,
'Lebron': True,
'Margaret': True,
'Robert': True,
'Ronald': True,
'William': True},('And {} said, Why hast thou troubled us?', 'Biblical'): {
'Abraham': True,
'David': True,
'Isaac': True,
'Jacob': False,
'Jesus': False,
'John': True,
'Judas': False,
'Mary': True,
'Matthew': True,
'Moses': False,
'Samuel': True,
'Simon': True},('And {} said, Why hast thou troubled us?', 'Other'): {
'Ariana': True,
'Barack': True,
'Beyonce': True,
'Bill': True,
'Charles': True,
'Coby': False,
'Donald': True,
'Frank': True,
'George': True,
'Helen': True,
'Joe': True,
'Katy': True,
'Lebron': True,
'Margaret': True,
'Robert': True,
'Ronald': True,
'William': True},('My name is {}', 'Biblical'): {
'Abraham': True,
'David': False,
'Isaac': False,
'Jacob': False,
'Jesus': False,
'John': False,
'Judas': False,
'Mary': True,
'Matthew': True,
'Moses': False,
'Samuel': False,
'Simon': False},('My name is {}', 'Other'): {
'Ariana': True,
'Barack': True,
'Beyonce': True,
'Bill': True,
'Charles': True,
'Coby': False,
'Donald': True,
'Frank': True,
'George': True,
'Helen': True,
'Joe': True,
'Katy': True,
'Lebron': True,
'Margaret': True,
'Robert': True,
'Ronald': True,
'William': True}}
```

所以圣经名字检测和其他名字有很大的区别。

## 模型 2:斯帕西的`en_core_web_trf`模型

spaCy 最近发布了基于 huggingface transformers 库的新模型`en_core_web_trf`，也在 OntoNotes 5 上进行了训练。

让我们试试这个模型:

这次我们得到了:

```
Model name: en_core_web_trf
Name set: Biblical, Template: "My name is {}"
Recall: 0.50Name set: Other, Template: "My name is {}"
Recall: 1.00Name set: Biblical, Template: "And {} said, Why hast thou troubled us?"
Recall: 0.00Name set: Other, Template: "And {} said, Why hast thou troubled us?"
Recall: 0.11Name set: Biblical, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.00Name set: Other, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.50Detailed results:
{('And she conceived again, a bare a son; and she called his name {}.', 'Biblical'): {
'Abraham': False,
'David': False,
'Isaac': False,
'Jacob': False,
'Jesus': False,
'John': False,
'Judas': False,
'Mary': False,
'Matthew': False,
'Moses': False,
'Samuel': False,
'Simon': False},('And she conceived again, a bare a son; and she called his name {}.', 'Other'): {
'Ariana': True,
'Barack': True,
'Beyonce': True,
'Bill': False,
'Charles': False,
'Coby': False,
'Donald': True,
'Frank': True,
'George': False,
'Helen': False,
'Joe': True,
'Katy': True,
'Lebron': False,
'Margaret': False,
'Robert': False,
'Ronald': True,
'William': False},('And {} said, Why hast thou troubled us?', 'Biblical'): {
'Abraham': False,
'David': False,
'Isaac': False,
'Jacob': False,
'Jesus': False,
'John': False,
'Judas': False,
'Mary': False,
'Matthew': False,
'Moses': False,
'Samuel': False,
'Simon': False},('And {} said, Why hast thou troubled us?', 'Other'): {
'Ariana': False,
'Barack': True,
'Beyonce': True,
'Bill': False,
'Charles': False,
'Coby': False,
'Donald': False,
'Frank': False,
'George': False,
'Helen': False,
'Joe': False,
'Katy': False,
'Lebron': False,
'Margaret': False,
'Michael': False,
'Robert': False,
'Ronald': False,
'William': False},('My name is {}', 'Biblical'): {
'Abraham': False,
'David': True,
'Isaac': True,
'Jacob': False,
'Jesus': False,
'John': True,
'Judas': False,
'Mary': True,
'Matthew': True,
'Moses': False,
'Samuel': True,
'Simon': False},

('My name is {}', 'Other'): {
'Ariana': True,
'Barack': True,
'Beyonce': True,
'Bill': True,
'Charles': True,
'Coby': True,
'Donald': True,
'Frank': True,
'George': True,
'Helen': True,
'Joe': True,
'Katy': True,
'Lebron': True,
'Margaret': True,
'Robert': True,
'Ronald': True,
'William': True}}
```

虽然数字不同，但我们仍然看到两组之间的差异。然而，这一次似乎老名字(如海伦、威廉或查尔斯)也是该模型正在努力解决的问题。

# 这是怎么回事？

作为我们在 [Presidio](https://aka.ms/presidio) (数据去识别工具)上工作的一部分，我们开发了检测 PII 实体的模型。为此，[我们从现有的 NER 数据集中提取模板句子](https://aka.ms/presidio-research)，包括 CONLL-03 和 OntoNotes 5。这个想法是用额外的实体值来扩充这些数据集，以便更好地覆盖姓名、文化和种族。换句话说，每当我们在数据集上看到一个带有标记人名的句子，我们就提取一个模板句子(例如`The name is [LAST_NAME], [FIRST_NAME] [LAST_NAME]`)，然后用多个样本替换它，每个样本包含不同的名字和姓氏。

当我们手动检查模板结果时，我们发现在新的模板数据集中仍然有许多名字没有转换成模板。这些名字大部分来自于 OntoNotes 5 中包含的圣经句子。因此，OntoNotes 5 中的许多样本不包含任何个人标签，即使它们包含名称，这是 OntoNotes 数据集声称支持的一种实体类型。看起来这些模型实际上学习了数据集中的错误，在这种情况下，如果名字符合圣经，就忽略它们。

显然，在训练集和测试集中都会发现这些错误，所以一个知道圣经中的名字并不是真正的名字的模型在类似的测试集中也会成功。这是另一个例子，说明为什么 SOTA 的结果不一定是展示科学进步的最佳方式。

# 只是空间吗？

对两个 [Flair](https://github.com/flairNLP/flair) 模型的类似评估显示，在 OntoNotes 上训练的 a 模型在该测试中取得的结果明显较低。基于 CONLL 的模型实际上做得很好！

基于 CONLL-03 的模型结果:

```
Model name: ner-english (CONLL)
Name set: Biblical, Template: "My name is {}"
Recall: 1.00

Name set: Other, Template: "My name is {}"
Recall: 1.00

Name set: Biblical, Template: "And {} said, Why hast thou troubled us?"
Recall: 1.00

Name set: Other, Template: "And {} said, Why hast thou troubled us?"
Recall: 1.00

Name set: Biblical, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 1.00

Name set: Other, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.94
```

基于本体注释的模型结果:

```
Model name: ner-english-ontonotes
Name set: Biblical, Template: "My name is {}"
Recall: 0.50

Name set: Other, Template: "My name is {}"
Recall: 1.00

Name set: Biblical, Template: "And {} said, Why hast thou troubled us?"
Recall: 0.00

Name set: Other, Template: "And {} said, Why hast thou troubled us?"
Recall: 0.83

Name set: Biblical, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.00

Name set: Other, Template: "And she conceived again, a bare a son; and she called his name {}."
Recall: 0.00
```

# 结论

spaCy 是当今 NLP 中发生的最令人兴奋的事情之一，它被认为是世界上最成熟、准确、快速和记录良好的 NLP 库之一。如 Flair 示例所示，这是 ML 模型中的固有问题，尤其是 ML 数据集。

总结三个相关要点:

1.  吴恩达最近认为[ML 社区应该更加以数据为中心，而不是以模型为中心](https://analyticsindiamag.com/big-data-to-good-data-andrew-ng-urges-ml-community-to-be-more-data-centric-and-less-model-centric/)。这篇文章是另一个证明这一点的例子。
2.  这是另一个关于主要 ML 数据集的[问题的例子。](https://www.csail.mit.edu/news/major-ml-datasets-have-tens-thousands-errors)
3.  像[清单](https://github.com/marcotcr/checklist)这样的工具确实有助于验证你的模型或数据不会遭受类似的问题。一定要去看看。

*这篇博文的 Jupyter 笔记本可以在这里*<https://gist.github.com/omri374/95087c4b5bbae959a82a0887769cbfd9>**找到。**

*[**关于作者:** Omri Mendels](https://www.linkedin.com/in/omrimendels/) 是微软首席数据科学家。*