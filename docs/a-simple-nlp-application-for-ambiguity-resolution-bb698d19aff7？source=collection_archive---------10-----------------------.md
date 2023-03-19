# 一个简单的用于歧义消解的 NLP 应用

> 原文：<https://towardsdatascience.com/a-simple-nlp-application-for-ambiguity-resolution-bb698d19aff7?source=collection_archive---------10----------------------->

## 如何利用 expert.ai 自然语言处理技术解决同形词和多义词的歧义问题

![](img/00e4fa87daf532c3b76e4dd03ed26fb7.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

歧义是自然语言处理中最大的挑战之一。当我们试图理解一个词的意思时，我们会考虑几个不同的方面，例如使用它的上下文，我们自己对世界的了解，以及一个给定的词在社会中通常是如何使用的。词汇会随着时间的推移而改变意思，在某个领域可能是一个意思，在另一个领域可能是另一个意思。这种现象可以在同形词和多义词中观察到，同形词是指两个单词恰好以相同的方式书写，通常来自不同的词源，而多义词是指一个单词有不同的意思。
在本教程中，我们将看到如何使用 expert.ai 技术解决词性标注和语义标注中的歧义。

# 开始之前

请查看如何安装 expert.ai NL API python SDK，要么在这篇[关于数据科学的文章](/visualizing-what-docs-are-really-about-with-expert-ai-cd537e7a2798?showDomainSetup=true&gi=300077e01aa3)上，要么在官方文档上，[这里](https://github.com/therealexpertai/nlapi-python#expertai-natural-language-api-for-python)。

# 词性标注

语言是模糊的:不仅一个句子可以用不同的方式来表达相同的意思，就连词条——一个被认为不那么模糊的概念——也可以表达不同的意思。

例如，单词 *play* 可以指几种不同的事物。让我们来看看下面的例子:
*我真的很喜欢这部戏。我在一个乐队里，我弹吉他。*

不仅同一个词可以有不同的意思，而且它可以用在不同的角色中:在第一句中， *play* 是名词，而在第二句中它是动词。给每个单词分配正确的语法标签被称为词性标注，这并不容易。

让我们看看如何用 expert.ai 解决 PoS 歧义性—首先，让我们导入库并创建客户端:

我们将看到两个句子的词性标注——注意，在两个句子中，词条*键*是相同的，但其词性发生了变化:

为了分析每个句子，我们需要创建一个对 NL API 的请求:最重要的参数——也显示在下面的代码中——是要分析的文本、语言和我们请求的分析，由资源参数表示。
请注意，expert.ai NL API 目前支持五种语言(en、it、es、fr、de)。我们使用的资源是*消歧*，它作为 expert.ai NLP 管道的产品，执行多级标注。
事不宜迟，让我们创建第一个请求:

现在我们需要迭代文本的位置，并检查哪个被分配给了词条*键*:

```
Part of speech for "The key broke in the lock."

The            	POS: DET
key            	POS: NOUN
broke in       	POS: VERB
the            	POS: DET
lock           	POS: NOUN
.              	POS: PUNCT
```

上面打印的是跟随 [UD 标签](https://universaldependencies.org/u/pos/)的 PoS 列表，其中*名词*表示词条*关键字*在这里用作名词。我们在第二个句子中看到的同形异义词不应该是这种情况，其中*键*用作形容词:

```
Part of speech for "The key problem was not one of quality but of quantity."

The            	POS: DET
key            	POS: ADJ
problem        	POS: NOUN
was            	POS: AUX
not            	POS: PART
one            	POS: NUM
of             	POS: ADP
quality        	POS: NOUN
but            	POS: CCONJ
of             	POS: ADP
quantity       	POS: NOUN
.              	POS: PUNCT
```

正如您在上面看到的，在这个句子中，词条*键*被正确地识别为一个形容词。

# 语义标注

一个单词也可以有相同的语法标签，也可以有不同的意思。这种现象被称为一词多义。能够推断每个单词的正确含义就是执行语义标记。

更常见的单词往往有更多的及时添加的含义。比如，引理纸可以有多种含义，这里看到:
*我喜欢在纸上做笔记。每天早上，我丈夫都会阅读当地报纸上的新闻。*

指出每个词条的正确含义是一项重要的任务，因为一个文档可能会基于此改变含义或焦点。要做到这一点，我们必须依靠发达和强大的技术，因为语义标记严重依赖于来自文本的许多信息。

对于语义标记，通常使用 ID:这些 ID 是概念的标识符，每个概念都有自己的 ID。对于同一个引理，比如*论文*，我们会用某个 id *x* 表示其作为材料的意义，用另一个 *y* 表示其作为报纸的意义。
这些 id 通常存储在知识图中，在知识图中，每个节点是一个概念，而拱形是遵循特定逻辑的概念之间的连接(例如，如果一个概念是另一个的下位词，则拱形可以链接两个概念)。现在让我们看看 expert.ai 是如何执行语义标记的。我们首先选择句子来比较两个引理*解*:

现在是对第一句话的请求—使用与上一个示例相同的参数:

语义信息可以在每个令牌的 *syncon* 属性中找到:syncon 是一个概念，存储在 expert.ai 的知识图中；每个概念由一个或多个引理构成，这些引理是同义词。
让我们看看信息是如何在文档对象中呈现的:

```
Semantic tagging for "Work out the solution in your head."

Work out       	CONCEPT_ID: 63784
the            	CONCEPT_ID: -1
solution       	CONCEPT_ID: 25789
in             	CONCEPT_ID: -1
your           	CONCEPT_ID: -1
head           	CONCEPT_ID: 104906
.              	CONCEPT_ID: -1
```

每个令牌都有自己的 syncon，而其中一些表示为-1 作为概念 id:这是分配给没有任何概念的令牌的默认 ID，比如标点符号或文章。
因此，如果对于前一个句子，我们获得了引理*解*的概念 id 25789，那么对于第二个句子，我们应该获得另一个概念 id，因为这两个引理在两个句子中具有不同的含义:

```
Semantic tagging for "Heat the chlorine solution to 75° Celsius."

Heat           	CONCEPT_ID: 64278
the            	CONCEPT_ID: -1
chlorine       	CONCEPT_ID: 59954
solution       	CONCEPT_ID: 59795
to             	CONCEPT_ID: -1
75             	CONCEPT_ID: -1
° Celsius      	CONCEPT_ID: 56389
.              	CONCEPT_ID: -1
```

不出所料，引理*解*对应的是不同的概念 id，说明使用的引理与上一句的意思不同。

请在 [GitHub](https://github.com/coprinus-comatus/ambiguity-resolution-expert.ai) 上找到这篇文章作为笔记本。

# 结论

NLP 很难，因为语言是模糊的:一个单词、一个短语或一个句子根据上下文可能有不同的意思。利用 expert.ai 等技术，我们可以解决歧义，并在处理词义时建立更准确的解决方案。