# 自然语言处理任务

> 原文：<https://towardsdatascience.com/natural-language-processing-tasks-3278907702f3?source=collection_archive---------21----------------------->

## NLP 是一组操作，主要是通过各种活动处理文本数据。在这些计算机活动的帮助下，将非结构化的信息转换成明确的数据或从信息中生成文本是可能的。在这篇文章中，我给你列出了这些不同的任务。

![](img/c27ef24e543c06369bc6cd5baebbf087.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=medium&utm_medium=referral) 拍摄

# 介绍

我们与生俱来的对话能力，是从我们的家庭、老师、同事那里获得的……我们在学校学习，在我们的一生中，掌握着写作和说话的词汇和语法。自然语言与计算机语言相反，因为它缺乏明确的逻辑结构、丰富性以及管理它的规则(通常复杂而模糊)。由于计算机还不能自然地理解我们的语言，一系列的活动已经被创造出来来连接两个世界:人类和机器。

信息通常以三种形式提供:

*   一种结构化的表示方法，带有与表格相关的信息的明确标记，以数据库表格或图形的形式呈现。
*   这是一种半结构化表示，具有明确的高级上下文信息，如发票或订单数据，这些数据将始终包含相同的信息。
*   非结构化表示将需要这里指出的提取工作，以将信息转换成显式数据。这些数据具有不同的编辑质量，混合了不同类型的信息，如人名、地名、日期等。主要目的是检测文本元素的结构，识别这些元素的属性以及将它们链接在一起的关系。

语言分析在两个层次上进行:词汇分析，它基于对单个单位的处理，例如[字素](https://en.wikipedia.org/wiki/Grapheme)(记录一个音素的字母或字母组，例如“ewe”、“eau”或“eu”)、[音素](https://en.wikipedia.org/wiki/Phoneme)(音段，例如音/o/)，以及[语素](https://en.wikipedia.org/wiki/Morpheme)(单词的段，例如前缀或后缀)；以及语义分析，侧重于单词和句子的整体意义。

长期以来基于显式规则，NLP 现在充分利用了机器学习提供的自动规则学习功能。这种学习是由语言或相关业务领域的专家进行的标记工作提供的。现在让我们详细说明主要的 NLP 过程。

注意:NLP 任务的这种组织是个人的。

![](img/59b6df4692f6b0044b8a149e19e84541.png)

NLP 任务:主要类别(作者的图表)

![](img/c3c2b75c4d163a9ab85e148c947da525.png)

NLP 任务:基本语言特征(作者的图表)

![](img/357384e84dcaa4ecaf54c79f95d6751c.png)

NLP 任务:增强的语言特性(作者的图表)

![](img/df864f5e45c65b4cf9a7bf0bb7ffd0f3.png)

NLP 任务:语音管理和光学识别(作者的图表)

![](img/b921513c204c8503e21961ba14898723.png)

NLP 任务:文本分类(作者的图表)

![](img/a04ea4f33136809e20c04d48b92c74ec.png)

NLP 任务:语言生成(作者的图表)

![](img/d8066f7d37ff599006942e80b39c9e63.png)

NLP 任务:其他任务(作者的图表)

# 基本语言特征

第一类包括“基本”处理任务，例如:

*   [语言识别](https://en.wikipedia.org/wiki/IETF_language_tag)在于检测文本中使用的语言。它允许定义用于执行其他活动的模型。
*   分句(也叫句界消歧)、[词](https://en.wikipedia.org/wiki/Lexical_analysis)，或者组词。许多 NLP 任务需要在句子或单词级别上工作，因此这种处理非常重要。因此，这一活动在文本处理链中至关重要。
*   [词干](https://en.wikipedia.org/wiki/Stemming)和[词汇化](https://en.wikipedia.org/wiki/Lemmatisation)。词干提取包括提取单词的词根。引理化允许将一个单词简化为它的标准形式(引理)。它也被称为“字典”形式。
*   [单词嵌入](https://en.wikipedia.org/wiki/Word_embedding)和句子嵌入，通过对段落和文档的扩展，将元素转化为数字的向量。矢量化目前是执行相似性搜索、聚类、分类等的基本操作。

# 高级语言特征

第二类包括语义处理任务，例如:

*   [识别命名实体](https://en.wikipedia.org/wiki/Named-entity_recognition) (NER)。一些单词或词组可以被分组到称为实体的类别中。没有这样的标准，但我们通常会找到组织、地点(地址、城市等。)，以及人[ENAMEX]；货币和百分比[NUMEX]；时间和日期[TIMEX]；vehicle…一般来说，特定于被治疗领域的类别被添加到预定义的类别中。确定实体的关系在于将唯一标识符与实体相关联。关系抽取是预测句子中实体的属性和关系的任务。也有可能找到其他类别的提取:特征提取，关键词提取，概念提取…
*   [词性](https://en.wikipedia.org/wiki/Part_of_speech) (POS)也叫语法标注。这项活动包括关联语法信息(名词、动词、冠词、形容词、代词、副词等。)与句子中的单词。
*   [分块](https://en.wikipedia.org/wiki/Shallow_parsing)，也是浅层解析，识别连续的记号间隔，这些记号形成与被处理的领域中的概念相对应的句法单元。这项活动首先识别句子的组成部分(名词、动词、形容词、副词等)。)然后将它们与具有明显语法意义的高阶单位(名词组、动词组等)联系起来。)
*   [统计解析](https://en.wikipedia.org/wiki/Statistical_parsing)包括生成[解析树](https://en.wikipedia.org/wiki/Parse_tree)(也称为派生树)，包含句子词汇单元的结构、依赖性和分类。根树根据[上下文无关语法](https://en.wikipedia.org/wiki/Context-free_grammar)表示句子的结构。
*   语义关系抽取是预测句子中实体和词组的属性和关系的任务。近年来，人们对[链接语法](https://en.wikipedia.org/wiki/Link_grammar)的兴趣有所增加，导致了提取质量的提高，从而对这种技术产生了新的兴趣。如今存在许多类型的依赖，这使得理解哪些活动被链接以及使用哪棵树变得不容易:句法、语义、形态、韵律…
*   单词的词义消歧在于确定单词在句子上下文中的含义。
*   [共指](https://en.wikipedia.org/wiki/Coreference)消解涉及确定与一个或多个表达式相关的事物或人。这种确定可以在句子、段落或文档的上下文中进行。
*   否定检测旨在识别文本中的否定暗示。

# 语音处理

语音处理在语言处理领域有着特殊的地位，因为书面语言和使用声音频率来表达语音之间有着更大的差异。

语音处理涵盖许多活动:

*   所讲语言的标识。这项活动包括识别说话者使用的语言。这是选择语言模型的先决条件。
*   [自动语音识别](https://en.wikipedia.org/wiki/Speech_recognition) (ASR)在于将声音转换成文本或意图。这种处理包括几种语言处理。
*   语音中的情感识别在于识别语音中的情感，而与语义内容无关。
*   [说话人识别](https://en.wikipedia.org/wiki/Speaker_recognition)就是从声音的特征来识别一个人。它包括回答这个问题:“谁在说话？”
*   [说话人验证](https://en.wikipedia.org/wiki/Speaker_recognition)包括检查说话人是否是预期的人。它在于回答这个问题:“是 X 在说话吗？
*   [声音克隆](https://en.wikipedia.org/wiki/Digital_cloning#Voice_cloning)包括复制一个声音，以允许生成任何句子。
*   对话中的语音分离包括分离在单个麦克风中同时说话的几个语音。
*   [语音合成](https://en.wikipedia.org/wiki/Speech_synthesis)是从文本中生成语音。下面是我关于语音合成技术的文章的链接。

[](/state-of-the-art-of-speech-synthesis-at-the-end-of-may-2021-6ace4fd512f2) [## 2021 年 5 月底语音合成的技术水平

### 2021 年 5 月底语音合成研究的最新进展，重点是深度学习…

towardsdatascience.com](/state-of-the-art-of-speech-synthesis-at-the-end-of-may-2021-6ace4fd512f2) 

# 光学识别

光学识别包括检测图像中的书写并将信息转换成文本。这种书写可以来自印刷字符或手写。

*   [光学字符识别](https://en.wikipedia.org/wiki/Optical_character_recognition) (OCR)是将包含印刷或键入信息的图像转换为文本的过程。
*   [手写识别](https://en.wikipedia.org/wiki/Handwriting_recognition) (HWR)在于检测书写文本的模式并将其转换成计算机文本。

# 文本分类

这些活动的目标是成功地检测句子或文档的主题、领域。

*   句子或文档的分类寻求识别与其相关联的一个或多个类别。
*   [主题建模](https://en.wikipedia.org/wiki/Topic_model)在于找到一篇文章、一篇文章或者一个或几个文档的主题。
*   [讽刺](https://en.wikipedia.org/wiki/Sarcasm)或[讽刺](https://en.wikipedia.org/wiki/Irony)的检测。
*   感觉/情绪分析。这是为了确定一个文本是否包含感觉或情绪。下面是我关于情绪/情感分析的文章的链接。

[](/is-your-chatbot-sensitive-575ad0217707) [## 你的聊天机器人敏感吗？

### 对话辅助解决方案越来越多地包括情感分析功能。这是什么意思？是不是…

towardsdatascience.com](/is-your-chatbot-sensitive-575ad0217707) 

*   意见提取类似于情感检测，但是在于检测意见的主题。
*   语义聚类包括在没有监督的情况下生成具有相同语义的单词、句子的集合/组。

# 语言生成

这一类别包括大量允许从可用信息中产生文本的服务。最近，随着具有数十亿个参数的统计模型的产生，这些活动得到了强调，并在接近万亿字节的语料库上进行了训练。

*   [语言建模](https://en.wikipedia.org/wiki/Language_model) (LM)包括创建一个统计模型，对语言中单词序列的分布进行建模。这种模型可用于预测文档中的下一个单词或字符。这种语言建模用于其他语言生成任务。
*   [自然语言生成](https://en.wikipedia.org/wiki/Natural_language_generation) (NLG)。这些多学科包括从内容中生成正确格式的文本。在此活动中，我们发现从令牌(例如实体)或表生成数据描述；面向任务或目标的对话的生成，甚至是自始至终的整个对话的生成；故事、文章的产生。[释义](https://en.wikipedia.org/wiki/Paraphrase)是从一个句子创造新句子的操作。这种技术可以用来扩充现有的语料库。计算机代码的生成也是工作的一个重要轴心。
*   文本校正包括添加标点符号(例如，在语音到文本转换的末尾)如逗号或句号，或者校正拼写或变化错误。
*   增加、替代和减少。文本扩充是通过添加元素或用其他类似元素替换元素来转换句子。缩减是为了给定的目的删除不必要的信息。[文本规范化](https://en.wikipedia.org/wiki/Text_normalization)是将数字、日期、首字母缩写词和缩略语转换成普通文本的过程。
*   [自动文本摘要](https://en.wikipedia.org/wiki/Automatic_summarization)通过提取最具代表性的句子或通过抽象生成新文本，将文本压缩成一组精简的句子。
*   自动机器翻译使用一套技术将文本或语音从一种语言翻译成另一种语言。
*   问答有很多子活动。有开放领域(通常是维基百科)或闭卷问答系统；从知识图中自动提取答案；从答案生成问题(即 Jeopardy 游戏)；问题质量评价；自然语言推理在于从一个“前提”中确定一个“假设”是真(蕴涵)、假(矛盾)还是未确定(中性)。

# 其他任务

存在许多其他活动，但是不太普遍，应用起来更复杂，或者甚至更机密，因为它们在 NLP 平台中不太常见。

例如:

*   回指解析是将一个[回指](https://en.wikipedia.org/wiki/Anaphora_(linguistics))与其关联的单词连接起来的任务。
*   常识包括使用“常识”模式或对世界的了解来做出推论。
*   [维度缩减](https://en.wikipedia.org/wiki/Dimensionality_reduction)用于将单词向量或句子向量的大小缩减为信息的精简版本。
*   域适应使用不需要标记的目标域数据的无监督神经域适应技术。
*   问题质量评估旨在开发主观问题回答算法，以验证问题是否是高质量的，或者是否应该修改或标记。
*   屏蔽语言建模(MLM)是自编码语言建模的一个示例，其中句子中的一个或多个单词被屏蔽，模型根据句子中的其他单词预测这些单词。
*   缺失成分分析旨在确定文本中缺失的成分。这些元素是一组现象，它们与预期的事物相关，但在文本中没有明确提到。省略、连接回指等缺失成分有不同的类型。
*   [语义角色标注](https://en.wikipedia.org/wiki/Semantic_role_labeling)就是给句子中的词赋予语义(一种“意义”)，比如把值赋予名词组，把谓词赋予动词组。语义分析通常在句法和形态句法分析之后进行。
*   [篇章蕴涵](https://en.wikipedia.org/wiki/Textual_entailment)旨在确定两个句子是否矛盾。
*   句子的词汇链接、下一句的预测等。

# 观点

NLP 由许多自动语言处理任务组成，允许从非结构化内容中探索和提取信息。近年来，机器学习技术的贡献推动了该领域的发展，使其有可能达到有时优于人类的质量水平。这是几项任务的集合，将使我们能够取得非凡的成果。

上面没有提到一个活动，因为严格地说，它不是 NLP 任务。该活动涵盖所有活动，因为它包括执行结果的验证/比较活动。它包括通过专用于要执行的任务的语料库来检查算法的性能。这种验证是基本的，因为它将允许我们通过使用一个共同的参考框架来评估治疗而获得一个分数。然而，这一活动受到以下事实的质疑，即算法因此被优化以在被选作参考的语料库上获得最佳可能得分，并且难以概括。