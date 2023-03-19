# 探索空间:构建高级 NLP 产品的一站式图书馆

> 原文：<https://towardsdatascience.com/exploring-spacy-your-one-stop-library-to-build-advanced-nlp-products-d242d8d753af?source=collection_archive---------41----------------------->

## *这是一个快速、无缝、先进的自然语言处理库*

![](img/adeccbc3cc2f6491d25da6ae883f0503.png)

由[内森·杜姆劳](https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

随着自然语言处理(NLP)成为构建现代人工智能产品的主要工具，开源库被证明是建筑师的福音，因为它们有助于减少时间，并允许更大的灵活性和无缝集成。spaCy 就是这样一个用流行的 Python 语言编写的高级 NLP 库。今天，我们将探索 spaCy，它的特性，以及如何开始使用免费库无缝构建 NLP 产品。

# **什么是 spaCy？**

spaCy 是一个免费的开源库，适合那些处理大量文本的人。它是为生产使用而设计的，允许您构建必须处理大量文本的应用程序。你可以使用 spaCy 来构建信息提取、自然语言理解或深度学习预处理文本的系统。

# **它提供什么？**

spaCy 提供了许多特性和功能，从语言概念到机器学习功能。它的一些[特性](https://spacy.io/usage/spacy-101#features)包括:

***标记化***

将文本分割成单词、标点符号等。

***词性标注***

将单词类型分配给标记，如动词或名词。

***依存解析***

分配句法依存标签，描述单个标记之间的关系，如主语或宾语。

***词汇化***

指定单词的基本形式。比如“was”的引理是“be”，“rats”的引理是“rat”。

***【SBD】句子边界检测***

寻找和分割单个句子。

***【命名实体识别】(NER)***

标记命名的“真实世界”对象，如人、公司或地点。

***【实体链接(EL)】***

消除文本实体与知识库中唯一标识符的歧义。

***相似度***

比较单词、文本跨度和文档以及它们之间的相似程度。

***文本分类***

为整个文档或部分文档指定类别或标签。

***基于规则的匹配***

根据文本和语言注释查找符号序列，类似于正则表达式。

***训练***

更新和改进统计模型的预测。

***序列化***

将对象保存到文件或字节字符串。

**其他功能包括:**

●支持 61 种以上的语言

●16 种语言的 46 种统计模型

●预训练的单词向量

●一流的速度

●轻松的深度学习集成

●句法驱动的句子分割

●内置语法和 NER 可视化工具

●方便的字符串到哈希映射

●导出到 numpy 数据阵列

●简单的模型打包和部署

●稳健、经过严格评估的精确度

…这还不是完整的列表。然而，作为数据科学家，我们也需要看看 spaCy 没有的所有东西。例如，它不是一个提供 SaaS 或 web 应用的 API 或平台。相反，它的库允许你构建 NLP 应用。它也不是为聊天机器人设计的。然而，它可以作为聊天机器人的底层技术提供文本处理能力。

# **斯帕西的建筑**

spaCy 的核心架构包括 Doc 和 Vocab 对象，Doc 拥有令牌序列及其所有注释，Vocab 对象拥有一组查找表，这些表使得通用信息可以跨文档使用。这允许集中字符串、词向量和词汇属性，从而避免存储这些数据的多个副本，进而节省内存并确保只有一个真实的来源。

类似地，文本注释被设计成允许单一的真实来源。为此，Doc 对象拥有数据，并且 Span 和 Token 指向它。它由记号赋予器构造，然后由管道的组件就地修改。语言对象通过获取原始文本并通过管道发送来协调这些组件，管道返回一个带注释的文档。它还编排培训和序列化。

# **空间入门**

许多人认为 spaCy 是 NLP 的 Ruby on Rails，它易于安装，API 简单高效。您可以通过首先安装 spaCy 及其英语语言模型来开始使用 spaCy。它与 64 位 CPython 2.7 / 3.5+兼容，可以在 Unix/Linux、macOS/OS X 和 Windows 上运行。

接下来，使用一个文本示例。在 spaCy 之后，导入 displaCy，它用于可视化 spaCy 的一些建模，以及一个英文停用词列表。然后，将英语语言模型作为语言对象加载，然后在示例文本中调用它。这将返回一个已处理的 Doc 对象。

这个经过处理的文档被分割成单个的单词并进行注释，但是它包含了原始文本的所有信息。我们可以将一个标记的偏移量放入到原始字符串中，或者通过连接标记和它们后面的空格来重构原始字符串。

您可以在 pip 和 conda 上安装最新的 spaCy 版本。我将试着展示一些用法的例子

## 在 python 中初始化空间

```
import spacy
nlp = spacy.load('en_core_web_sm')nlp("He went to play basketball")
```

## 使用空间的词性标注

```
import spacynlp = spacy.load('en_core_web_sm')doc = nlp("He went to play basketball")for token in doc:
    print(token.text, "-->", token.pos_)
```

输出

```
He –> PRON
went –> VERB
to –> PART
play –> VERB
basketball –> NOUN
```

## 使用空间进行依存解析

```
# dependency parsingfor token in doc:
    print(token.text, "-->", token.dep_)
```

输出

```
He –> nsubj
went –> ROOT
to –> aux
play –> advcl
basketball –> dobj
```

依存标记词根表示句子中的主要动词或动作。其他单词直接或间接地与句子的词根相连。您可以通过执行下面的代码来找出其他标记代表什么:

## 基于空间的命名实体识别

```
doc = nlp("Indians spent over $71 billion on clothes in 2018")for ent in doc.ents:
    print(ent.text, ent.label_)
```

输出

```
Indians NORP
over $71 billion MONEY
2018 DATE
```

# 结论

总的来说，这个像 spaCy 一样的库抽象掉了使用方面的所有复杂性，开始使用它就像写几行代码一样简单。它真的能让像我这样的数据工程师快速上手，用它来处理非结构化的文本数据。

# 在 Linkedin 和 Twitter 上关注我

如果你对类似的内容感兴趣，请在 Twitter 和 Linkedin 上关注我

[Linkedin](https://www.linkedin.com/in/anuj-syal-727736101/)

[推特](https://twitter.com/anuj_syal)