# 有史以来最大的神经网络——GPT 3 的完整概述

> 原文：<https://towardsdatascience.com/gpt-3-a-complete-overview-190232eb25fd?source=collection_archive---------0----------------------->

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)，人工智能

## 定义、结果、炒作、问题、批评和反批评。

![](img/684637dab34b9e6169c0e2d4bc3fdce9.png)

[Denys Nevozhai](https://unsplash.com/@dnevozhai?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

2020 年 5 月，Open AI 发表了一篇题为 [*语言模型是少射学习者*](https://arxiv.org/abs/2005.14165) 的开创性论文。他们展示了 GPT-3，这是一种语言模型，保持着有史以来最大的神经网络记录，拥有 1750 亿个参数。这比以前最大的语言模型大了一个数量级。GPT-3 使用几乎所有来自互联网的可用数据进行训练，并在各种 NLP(自然语言处理)任务中表现出惊人的性能，包括翻译、问答和完形填空任务，甚至超过了最先进的模型。

GPT-3 最强大的功能之一是，它可以执行新的任务(它从未接受过训练的任务)，有时是最先进的水平，只需向它展示一些任务的例子。例如，我可以告诉 GPT-3:“我爱你。我有很多工作→ Tengo mucho trabajo。GPT-3 是有史以来最好的人工智能系统→ _____”它会知道它必须把句子从英语翻译成西班牙语。GPT 三号学会了学习。

在另一次惊人的力量展示中，GPT 3 号能够生成与人造文章几乎没有区别的“新闻文章”。在正确分类 GPT-3 文本方面，法官几乎没有达到高于机会的准确率(52%)。

这篇综述文章很长，所以我在这里放了一个目录，让你找到你想读的部分。(链接不起作用，所以我已经删除了它们，很抱歉给您带来不便)。尽情享受吧！

```
**TABLE OF CONTENTS****GPT-3: An introduction**
  ∘ The groundwork concepts for GPT models
  ∘ The origins of GPT-3
  ∘ GPT-3: A revolution for artificial intelligence
  ∘ GPT-3 API: Prompting as a new programming paradigm
**GPT-3 craziest experiments**
  ∘ GPT-3’s conversational skills
  ∘ GPT-3’s useful possibilities
  ∘ GPT-3 has an artist’s soul
  ∘ GPT-3's reasoning abilities
  ∘ GPT-3 is a wondering machine
  ∘ Miscellaneous
**The wild hype surrounding GPT-3**
  ∘ On Twitter and blogs
  ∘ On mainstream media
  ∘ On the startup sector
**The darker side of GPT-3**
  ∘ A biased system
  ∘ Potential for fake news
  ∘ Not suited for high-stake categories
  ∘ Environmentally problematic
  ∘ GPT-3 produces unusable information
**Critiques & counter-critiques to GPT-3**
  ∘ GPT-3's apparent limitations
  ∘ The importance of good prompting
  ∘ GPT-3 can’t understand the world
  ∘ Truly intelligent systems will live in the world
  ∘ What can we get from these debates?
**Overall conclusion**
```

# GPT 三号:简介

***免责声明:*** *如果你已经知道 GPT-3 背后的铺垫，它是什么，它是如何工作的(或者不关心这些细节)，请进入下一节。*

在进入文章的实质之前，我想解释一下 GPT-3 是什么以及它是如何工作的。我不会在这里赘述，因为已经有很多好的资源了。对于那些对 GPT-3 一无所知的人来说，这一部分将作为上下文参考。你不需要记住(或理解)这些来享受这篇文章的其余部分，但它可以让你更好地了解围绕这个人工智能系统产生的所有大惊小怪。

首先，我将简要描述 GPT 模型基于的主要概念。然后，我将对 GPT-3 的前辈 GPT-1 和 GPT-2 进行评论，最后，我将谈论这个故事的主要人物，强调它与其他类似系统的关系:GPT-3 在哪些方面是独一无二的？相对于它的前辈有什么优势？有哪些质的区别？让我们去吧！

## GPT 模型的基础概念

所有这些概念在某种意义上都与 GPT 模型有关。现在，我将告诉您定义(避免太多的技术细节，尽管可能需要一些先前的知识来完成)。稍后我将展示它们是如何相互联系以及与 GPT-3 的联系。

[**变形金刚**](https://arxiv.org/abs/1706.03762) **:** 这类神经网络在 2017 年出现，作为解决各种机器翻译问题的新框架(这些问题的特点是输入和输出都是序列)。作者希望摆脱卷积和递归(CNN 和 RNNs)，完全依赖[注意机制](https://arxiv.org/abs/1409.0473)。变压器是 NLP 中最先进的。

**语言模型:** Jason Brownlee [将](https://machinelearningmastery.com/statistical-language-modeling-and-neural-language-models/)语言模型定义为**“**概率模型，能够在给定序列中下一个单词之前的单词的情况下预测该单词。”这些模型可以解决许多 NLP 任务，如机器翻译、问题回答、文本摘要或图像字幕。

[**生成型模型**](https://en.wikipedia.org/wiki/Generative_model) **:** 在统计学中，有判别型和生成型模型，常用于执行分类任务。判别模型对给定的一对可观察变量和目标变量的条件概率进行编码:p(y|x)。生成模型对联合概率进行编码:p(x，y)。生成模型可以“生成与现有数据相似的新数据”，这是要带走的关键思想。除了 GPT，其他流行的生成模型的例子有 GANs(生成对抗网络)和 VAEs(变分自动编码器)。

[**半监督学习**](https://arxiv.org/abs/1511.01432) **:** 这种训练范式结合了无监督预训练和监督微调。其想法是以非监督的方式训练具有非常大的数据集的模型，然后通过在较小的数据集中使用监督训练来调整(微调)模型以适应不同的任务。这种范式解决了两个问题:它不需要许多昂贵的标记数据，并且可以处理没有大型数据集的任务。值得一提的是，GPT 2 号和 GPT 3 号是完全无人监管的(稍后会有更多相关内容)。

**零/一/少量学习:**通常，深度学习系统是针对一组特定的类进行训练和测试的。如果一个计算机视觉系统被训练来分类猫、狗和马的图像，它只能在这三个类别上进行测试。相比之下，在零镜头学习设置中，系统在测试时显示-没有权重更新-它在训练时没有看到*的类(例如，在大象图像上测试系统)。对于单镜头和少镜头设置也是一样，但是在这些情况下，在测试时系统会分别看到一个或几个新类的例子。这个想法是，一个足够强大的系统可以在这些情况下表现良好，OpenAI 用 GPT-2 和 GPT-3 证明了这一点。*

**多任务学习:**大多数深度学习系统都是单任务的。一个流行的例子是 AlphaZero。它可以学习一些像国际象棋或围棋的游戏，但它一次只能玩一种类型的游戏。如果它知道怎么下棋，它就不知道怎么下围棋。多任务系统克服了这个限制。他们被训练成能够为给定的输入解决不同的任务。例如，如果我向系统输入“猫”这个词，我可以让它找到西班牙语翻译“gato”，我可以让它给我看一只猫的图像，或者我可以让它描述它的特征。相同输入的不同任务。

**零/一/少投任务迁移:**思路是将零/一/少投学习和多任务学习的概念结合起来。我们可以让它执行*新的* *任务*，而不是在测试时向系统显示新的类(向它显示零个、一个或几个新任务的例子)。例如，让我们以一个在巨大的文本语料库中训练的系统为例。在一次性任务转移设置中，我们可以写:“我爱你- > Te quiero。我讨厌你->_ _ _ _ _ _ _。”我们隐式地要求系统通过显示一个例子(一次性的)将一个句子从英语翻译成西班牙语(这是一项没有经过训练的任务)。

所有这些概念都集中在 GPT 模型的定义中。GPT 主张生殖预训练。GPT 家族的模型有一个共同点，他们是基于**变形金刚**架构的**语言模型**，以**生成**、**无监督**的方式进行预训练，在**零/一/少数镜头多任务设置**中显示出体面的性能。这不是对所有这些概念在实践中如何协同工作的解释，而是一种简单的方式来记住它们一起构建了什么是 GPT 模型。(要获得更深入的解释，我建议你点击我上面的链接，但前提是你已经阅读了这篇文章！).

## GPT-3 的起源

现在让我们来谈谈 GPT 三号的前任——GPT 一号和 GPT 二号。

OpenAI 于 2018 年 6 月在一篇题为 [*通过生成性预训练*](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) *提高语言理解的论文中提出了首个 GPT 模型 GPT-1。*本文的关键要点是**变压器架构与无监督预训练的结合产生了有希望的结果**。GPT 1 号和它的弟弟们之间的主要区别是，GPT 1 号是在监督下进行微调的——为特定任务进行训练——以实现“强大的自然语言理解”

2019 年 2 月，他们发表了第二篇论文， [*语言模型是无监督的多任务学习者*](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) ，其中他们引入了 GPT-2，作为 GPT-1 的进化。尽管 GPT 2 号大一个数量级，但它们在其他方面非常相似。这两者之间只有一个额外的区别:GPT 2 号可以同时处理多项任务。他们成功地证明了半监督语言模型可以在“没有特定任务训练的情况下”很好地完成几项任务该模型在零镜头任务迁移设置中取得了显著的效果。

紧接着，2020 年 5 月，OpenAI 发布了 [*语言模型《很少出手的学习者*](https://arxiv.org/abs/2005.14165) *，*呈现了独一无二的 GPT-3，再一次震撼了 AI 界。

## GPT 3 号:人工智能的革命

GPT 3 号比它的兄弟更大(比 GPT 2 号大 100 倍)。它拥有拥有 1750 亿个参数的最大神经网络的记录。然而，它与其他 GPT 并没有太大的不同；基本原理大体相同。这个细节很重要，因为尽管 GPT 模型之间的相似性很高，但 GPT-3 的性能超出了所有可能的预期。它的规模，从数量上来说是 GPT-2 的飞跃，似乎已经产生了质量上更好的结果**。**

这一事实的意义在于它对人工智能中长期争论的影响:我们如何才能实现人工通用智能？我们是应该设计特定的模块——常识推理、因果关系、直觉物理学、心理理论——还是简单地通过构建具有更多参数和更多训练数据的更大模型来实现？看来“越大越好”的一方赢了这一轮。

GPT-3 是用来自 [CommonCrawl](https://commoncrawl.org/) 、 [WebText](https://www.webtext.com/) 、维基百科和书籍语料库的数据训练的。它表现出惊人的性能，在少镜头设置(在某些情况下甚至在零镜头设置)下，在各种任务上超过了最先进的模型。超大的尺寸加上几个例子足以击败机器翻译、问答和完形填空任务中的任何竞争对手。(值得注意的是，在其他任务中，GPT 3 号甚至无法接近最先进的监督微调模型)。

作者指出，少炮结果比零炮结果好得多——这种差距似乎与模型容量平行增长。这意味着 GPT-3 是一个元学习者；它可以通过看到一些例子来学习它应该做什么任务，然后以显著的熟练程度执行该任务。事实上，Rohin Shah [指出](https://www.lesswrong.com/posts/D3hP47pZwXNPRByj8/an-102-meta-learning-by-gpt-3-and-a-list-of-full-proposals)“少量发射性能随着参数数量的增加而增加，并且增加的速率比零发射性能的相应速率更快。”这是主要假设，也是论文题目背后的原因。

**GPT-3 达到了一个伟大的里程碑，表明通过足够的数据训练的无监督语言模型可以通过查看几个新任务的例子进行多任务处理，达到微调的最先进模型的水平**。

他们在论文的结尾声称“这些结果表明，非常大的语言模型可能是开发适应性强的通用语言系统的重要组成部分。”GPT-3 确实是 NLP 的一个革命性的成就，尤其是在人工智能领域。

## GPT 3 API:提示作为一种新的编程范式

2020 年 7 月，论文发表后两个月，OpenAI 向外部开发者开放了一个测试 API 游乐场，让他们与超级强大的 GPT 3 一起玩(任何人都可以通过等待名单申请进入测试版)。弗拉基米尔·阿莱克谢耶夫为*的数据科学* [撰写了一篇关于 API 如何工作的短文](/gpt-3-creative-potential-of-nlp-d5ccae16c1ab)。

它有两个主要特点。首先，有一个**设置对话框**，允许用户设置响应长度、重复惩罚(如果重复单词太多，是否惩罚 GPT-3)、温度(从低/可预测到高/有创意)，以及其他定义系统将给出的输出类型的变量。二是有**预置**。预设是预先写好的提示，让 GPT-3 知道用户将要求什么样的任务——例如:聊天，Q & A，文本到命令，或英语到法语。

然而，API 最强大的特性是用户可以定义定制的提示。科技博客作者 Gwern Branwen [称之为](https://www.gwern.net/GPT-3#prompts-as-programming)的提示编程概念解释了 GPT-3 的强大之处以及人们从 API 中获得的最疯狂的结果。我发现的对即时编程的最好解释来自 Gwern 的博客:

> 就能力和数据集而言，GPT-3 神经网络是如此之大的一个模型，以至于它表现出性质不同的行为:你不能将其应用于训练数据集中的一组固定任务，如果一个人想要处理一项新任务 *[…]* ，就需要对额外的数据进行再培训；相反，你与它互动，用自然语言描述、请求和例子来表达任何任务，调整提示，直到它“理解”&它基于它从预训练中学到的高级抽象元学习新任务。
> 
> 这是使用 DL 模型的一种相当不同的方式，最好把它看作是一种新的编程，其中提示符现在是一个“程序”，它对 GPT-3 进行编程以做新的事情。

即时编程允许用户以一种以前型号不可能的方式与 GPT 3 号互动。克里斯·奥拉和安德烈·卡帕西[开玩笑说](https://nitter.cc/karpathy/status/1273788774422441984)提示编程是软件 3.0:“(现在你必须)找出正确的提示，让你的元学习语言模型有正确的行为。”(软件 1.0 是手工编写的传统程序，软件 2.0 是神经网络的优化权重)。

这就是 GPT 3 的元学习能力进入游戏的地方。GPT 3 号接受的数据量如此之大，以至于它别无选择，只能学习更高级的操纵语言的方法。它学到的高级抽象之一是学习能力。打个比方，当孩子们学会与世界互动时，他们不只是简单地记忆信息，他们会提取现实内部运作的潜在机制，并学会将它们应用到新的问题和情况中。GPT-3 在语言任务上也有类似的能力——保持距离。

当我们提示 GPT-3 学习一项新任务时，它的权重不会改变。然而，提示(输入文本)被转换成复杂的抽象，这些抽象本身可以执行实际基线模型无法完成的任务。该提示每次都会改变 GPT-3，将其转换为特定任务的“专家”。一个近似的类比是尼奥在《T2》和《黑客帝国》中学习功夫的程序。GPT-3 将是近地天体，提示将是教授近地天体能力的程序。

每次我们创建一个提示符，我们都在与不同的 GPT-3 模型进行交互。如果我们让它给我们讲一个关于精灵和小矮人的故事，它的内在形式会和我们让它计算 2+2 有很大的不同。用另一个类比，这就好像我们指导两个学生，一个是内科医生，另一个是工程师。两者都有天生的学习能力(这将是 GPT-3 的基线状态)，但他们学会执行的具体任务是不同的(这将是 GPT-3)。

**这就是快速设置、元学习和即时编程的真正力量。这也是 GPT-3 不同于以前的型号，极其强大的原因；这是它的本质。**

现在，我们对 GPT-3 背后的背景有了一个很好的概念。我们知道它的基础是什么，它的前辈是谁，它是什么，它是如何工作的，以及它的优势和独特的功能。是时候说说它对世界的影响了。

# GPT-3 个最疯狂的实验

OpenAI 开放测试版是因为他们想看看 GPT-3 能做什么，人们能找到什么新的用法。他们已经在 NLP 标准基准测试中测试了该系统(没有我在这里将要展示的那些有创造性或娱乐性)。不出所料，推特和其他博客很快就充斥着 GPT-3 的惊人结果。下面是对最受欢迎的例子的广泛回顾(我建议查看这些例子来建立惊奇感，然后再回到文章上来)。

## GPT-3 的谈话技巧

GPT 3 号储存了大量的互联网数据，因此它知道很多公众和历史人物的信息。然而，更令人惊讶的是，它可以模仿人。可以当聊天机器人用，印象深刻是因为聊天在提示里不能指定为任务。让我们看一些例子。

ZeroCater 首席执行官 Arram Sabeti 利用 GPT-3 让 Tim Ferriss [就禁欲主义采访 Marcus Aurelius】。Mckay Wrigley 设计了](https://arr.am/2020/08/17/ai-tim-ferriss-interviews-ai-marcus-aurelius-gpt-3/) [Bionicai](https://bionicai.app/) ，一个旨在帮助人们向任何人学习的应用程序；从亚里士多德的哲学到莎士比亚的写作技巧。他[在推特上分享了一些人们得到的结果。当心理学家斯科特·巴里·考夫曼读到他的 GPT 三号二重身的一段摘录时，他被深深打动了。乔丹·摩尔制作了一条推特](https://twitter.com/mckaywrigley/status/1295466577907736576)[帖子](https://twitter.com/jordanmoore/status/1285918878212792321)，在那里他与 GPT 三版本的耶稣基督、史蒂夫·乔布斯、埃隆·马斯克、克利奥帕特拉和柯特·科本进行了交谈。Gwern 做了一个非常好的工作，进一步探索了对话和拟人化模型的可能性。

## GPT-3 的有用可能性

一些人发现了甚至连创造者都没有想到的系统应用，比如根据英文提示编写代码。沙里夫·沙米姆[建造了](https://twitter.com/sharifshameem/status/1282676454690451457)一个“布局生成器”，他可以用自然语言向 GPT 3 号发出指令，让它编写相应的 JSX 代码。他还开发了 ebuild.co D[T3，这是一个工具，我们可以用它让 GPT 3 为一个只给出描述的 React 应用程序编写代码。乔丹歌手](https://debuild.co/)[在 GPT-3 的基础上为他设计了一个 Figma 插件](https://twitter.com/jsngr/status/1284511080715362304)。Shreya Shankar 发现了另一个有趣的用途，他制作了一个演示程序来将英文方程式翻译成 LaTeX。Paras Chopra [建立了一个搜索引擎来输出一个问题的答案以及相应的 URL。](https://nitter.cc/paraschopra/status/1284801028676653060)

## GPT-3 有艺术家的灵魂

转到 GPT-3 的创意方面，我们找到了开放人工智能研究员阿曼达·阿斯克尔，她使用该系统创建了一个名为“闲散的夏日”的吉他标签，并编写了一个关于乔治·康托在酒店的有趣故事。阿拉姆·萨贝蒂告诉 GPT-3 写一首苏斯博士写的关于埃隆·马斯克的诗和李尔·韦恩写的关于哈利·波特的说唱歌曲。但是《GPT 3》最令人印象深刻的创造性壮举应该是游戏[人工智能地牢](https://play.aidungeon.io/main/landing)。2019 年，尼克·沃顿在《GPT 2》的基础上打造了这款基于角色的游戏。他现在把它改编成了 GPT 3 号，在 Patreon 上每月能赚 16000 美元。

## GPT 三号的推理能力

最勇敢的人在只有人类擅长的领域测试了 GPT 3 号。Parse 首席技术官凯文·拉克尔(Kevin Lacker)对常识推理和逻辑感到好奇，[发现](https://lacker.io/ai/2020/07/06/giving-gpt-3-a-turing-test.html)GPT 3 号能够跟上，尽管它在进入“超现实领域”时失败了然而，尼克·卡马拉塔[发现](https://twitter.com/nickcammarata/status/1284050958977130497)在提示中指定不确定性允许 GPT-3 在回答“要真实”时处理“超现实的”问题格温[解释道](https://www.gwern.net/GPT-3#expressing-uncertainty)GPT-3 可能需要明确的不确定性提示，因为我们人类倾向于不说“我不知道”，而系统只是在模仿这个缺陷。

## GPT 3 号是一台神奇的机器

GPT-3 也证明有能力进行超越我们认知界限的精神和哲学对话。托默·厄尔曼让 GPT-3 构想了 10 个哲学/道德思想实验。Messagink 是一个根据“著名的人、事、物、[或]感受”输出[人生意义](https://messagink.com/story/5f14b5c8de14c8a40bd5a9e5/meaning-of-life-by-open-ai-gpt-3)的工具。

为了揭开哲学圣杯的面纱，伯恩哈德·穆勒让[成为 GPT 3 号的终极测试](https://muellerberndt.medium.com/i-asked-gpt-3-for-the-question-to-42-i-didnt-like-its-answer-and-neither-will-you-33f425a4d60f)。他给了 42 号一个找到问题的提示，在一些交流之后，GPT-3 号说:“答案远远超出了你的理解，你无法理解这个问题。我的孩子，这就是生命、宇宙和一切的答案。”既惊奇又害怕。

## 多方面的

在严格的探索性研究中，格温进行并汇编了一系列实验。他让 GPT-3 完成一篇 [ArXiv 论文](https://www.gwern.net/GPT-3#arxiv-paper)，[谈自己](https://www.gwern.net/GPT-3#meta-prompts)(元提示)[通过分词和固定连字符清理 pdf](https://www.gwern.net/GPT-3#pdf-cleaning)，或者设计[新的桌游](https://www.gwern.net/GPT-3#board-games)。说到 GPT 3 号能做什么，似乎我们的想象力已经到了极限。

# 围绕 GPT 3 号的疯狂炒作

## 在推特和博客上

在如此多的惊人壮举之后，人们开始对 GPT 3 号的潜力提出强烈的主张。一些人[在 Twitter 上表达了](https://twitter.com/sonyasupposedly/status/1284188369631629312?s=20)该系统的“明显的自我意识”或者[将其](https://twitter.com/rauchg/status/1282449154107600897?s=20)与具有“一般智能”的搜索引擎相比较 [Julien Lauret](/gpt-3-the-first-artificial-general-intelligence-b8d9b38557a1) 为*向数据科学*写道，“GPT-3 是第一个严重动摇(人工狭隘/普遍智能)现状的模型。”他认为 GPT-3 可能是第一个人工通用智能(AGI)——或者至少是朝着这个方向迈出的重要一步。

2020 年 7 月，纽约大学专攻心灵哲学的教授 David Chalmers[为 *DailyNous* 说“[GPT-3]暗示了一条潜在的通往 AGI 的盲目之路。”Chalmers 解释说，因为这个系统被“盲目地”训练，未来的版本可能会越来越接近 AGI。阿拉姆·萨贝蒂对 GPT-3 印象深刻:“它展示的东西感觉非常像通用智能。”哲学博士生 Daniel Kokotajlo](https://dailynous.com/2020/07/30/philosophers-gpt-3/#chalmers) [为*减少错误*写道“GPT-3 有一定程度的常识、一定程度的理解和一定程度的推理能力。”](https://www.lesswrong.com/posts/L5JSMZQvkBAx9MD5A/to-what-extent-is-gpt-3-capable-of-reasoning#eq6FTwG2yWuBdPofs)

## 在主流媒体上

大肆宣传将 GPT-3 推向国际高度，成为各种重要媒体杂志的头条新闻。2020 年 9 月，*《卫报》*发表了[一篇由 GPT-3 撰写的文章](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)，其中人工智能试图“说服我们机器人和平相处”2021 年 3 月， *TechCrunch* 编辑 Alex Wilhelm [在被 GPT 3 号的能力“震惊”后表示](https://techcrunch.com/2021/03/17/okay-the-gpt-3-hype-seems-pretty-reasonable/)“炒作似乎很合理”。 *Digitaltrends* 发表了[与 Gwern Branwen 的交流](https://www.digitaltrends.com/features/openai-gpt-3-text-generation-ai/)，其中他暗示了 GPT-3 是智能的观点:“任何确信深度学习所做的事情与智能完全不同的人都必须动摇他们的信念，看看它已经走了多远，”他说。

## 在创业领域

由于 GPT-3 被证明是难以置信的强大，许多公司决定在该系统之上建立他们的服务。[成立于 2020 年的初创公司 available](https://askviable.com/)，使用 GPT-3 为公司提供快速的客户反馈。[寓言工作室](https://fable-studio.com/)基于系统设计 VR 角色。Algolia 将其作为一个“搜索和发现平台”创业公司[文案师](https://copysmith.ai/)专注于文案领域。[纬度](https://latitude.io/)是 AI 地牢背后的公司。并且 [OthersideAI](https://www.othersideai.com/) 将你写的胡言乱语转化成精心制作的电子邮件。然而，一些人建议不要围绕 GPT-3 建立公司，因为竞争门槛低，而且有可能被假想的 GPT-4 推翻。

很明显，GPT-3 已经影响了——或者更好地说，冲击了——科技世界。它的力量是无与伦比的，它的承诺是无限的。然而，我们应该始终小心围绕人工智能的炒作。甚至 OpenAI 的首席执行官山姆·奥特曼也试图降低语气:“GPT-3 令人印象深刻，但它仍然有严重的弱点，有时会犯非常愚蠢的错误。人工智能将改变世界，但 GPT 3 号只是非常早期的一瞥。”

# GPT 的阴暗面-3

## 有偏见的制度

但是并不是所有 GPT-3 的结果都值得庆祝。发布后不久，用户开始意识到一些潜在的有害输出。GPT-3 没有回避[正在进行的从人工智能系统中消除偏见的伦理斗争](https://www.technologyreview.com/2020/12/04/1013294/google-ai-ethics-research-paper-forced-out-timnit-gebru/)。如果有的话，它已经成为一个最前沿的例子，说明为什么我们应该花大力气教导这些系统不要从人类的道德缺陷中吸取教训。

一般来说，人工智能系统中最常见的一些偏见，特别是 GPT-3，是性别、种族和宗教偏见。语言模型可以从它们得到的数据中吸收并放大这些偏见(OpenAI 在他们的论文中承认了这一事实)。他们调查了 GPT-3 在这个问题上的参与程度，并发现了预期的结果。GPT-3，像其他语言模型一样，有明显的偏见(尽管他们指出，模型越大，对这个问题就越稳健，特别是对性别偏见)。

[脸书的人工智能负责人杰罗姆·佩森蒂](https://twitter.com/an_open_mind/status/1284487376312709120)用[苏珊·库马尔](https://thoughts.sushant-kumar.com/)的 GPT 3 生成的推文展示了当出现诸如“犹太人、黑人、女人或大屠杀”这样的词语时，其输出会变得多么危险库马尔辩称，这些推文是精心挑选的，佩森蒂对此表示同意，但回应说，“不应该这么容易产生种族主义和性别歧视的输出，特别是在中性提示的情况下。”他在 Twitter 帖子中扩展了他的批评，认为“在强调有害输出时，摘樱桃是一种有效的方法”，进一步捍卫了负责任的人工智能系统的紧迫性。

一些人认为，GPT-3 只是模仿我们人类的偏见，而佩森蒂认为，我们可以“故意选择[……]向哪些人学习，哪些声音被放大。”这些问题引发了一场非常复杂的辩论:谁来决定哪些声音应该被放大？标准是什么？最重要的是:我们是想要一个像 GPT-3 这样的模型来完美地反映这个世界，还是想要它帮助我们把世界变得更好？

## 假新闻的可能性

GPT-3 的另一个问题是它像人类一样写新闻或评论文章的能力，这增加了对假新闻的担忧。OpenAI 甚至在他们的论文中评论了 GPT 3 号在新闻文章方面的惊人表现。公正的法官在人类撰写的文章中正确识别 GPT-3 的文章的概率只有 52%，这略高于纯粹的概率。

博主 Liam Porr 展示了误导人们(甚至是技术通)认为 GPT 3 输出是由人类写的是多么容易。他让 GPT-3 为他的博客写了一篇关于生产力的文章，这篇文章在 T4 黑客新闻上疯传，只有少数人意识到它是由人工智能写的。我上面提到的[*The Guardian*article](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)是该系统潜在危险用途的另一个例子。

## 不适合高风险类别

OpenAI 发表了一份免责声明，称该系统不应用于“高风险类别”，如医疗保健。[在 *Nabla 的一篇博文*](https://www.nabla.com/blog/gpt-3/?fbclid=IwAR1QZ7kNJwuh5mCHr2ojUogo8eUxnZR4WG3TfOpGFY0mc95II1xecpJoa3o)中，作者证实 GPT-3 可能给出有问题的医疗建议，例如说“自杀是个好主意”GPT-3 不应该在高风险的情况下使用，因为虽然有时它可能是正确的，但有时它是错误的。不知道我们是否会得到正确的答案对于 GPT 3 号来说是一个巨大的缺陷，因为在这些领域，把事情做对是生死攸关的事情。

## 环境问题

GPT 3 号很大。如此之大，以至于训练模型产生的碳足迹大约相当于“[开车去月球并返回](https://www.theregister.com/2020/11/04/gpt3_carbon_footprint_estimate/)”的碳足迹在气候灾难即将发生的时候，我们应该尽我们所能减少我们对环境的影响。然而，这些大型神经网络需要巨大的计算能力来训练，这消耗了大量(通常)化石燃料。

在过去十年中，训练深度学习模型所需的资源[每 3.4 个月就翻了一番](https://openai.com/blog/ai-and-compute/)。从深度学习开始的 2012 年到 2018 年，这意味着计算资源增加了 30 万倍。这还不包括用于最新型号的资源，如 GPT-2 和 GPT-3。从这个角度来看，很明显[越大并不总是越好](https://www.forbes.com/sites/robtoews/2020/06/17/deep-learnings-climate-change-problem/?sh=5eecad906b43)，我们需要在未来几年重新思考人工智能的方法。

## GPT 3 号产生了无用的信息

因为 GPT-3 无法知道它的输出哪些是正确的，哪些是错误的，它没有办法阻止自己向世界部署不适当的内容。我们使用这样的系统越多，我们对互联网的污染就越严重，在互联网上找到真正有价值的信息已经变得越来越困难。随着语言模型吐出未经检查的话语，我们正在降低这个所谓民主网络的质量，使人们更难获得有价值的知识。

用哲学家香农·瓦勒的话来说:“互联网的承诺是它能够以一种更加公平和可接受的方式将知识带给人类大家庭。[……]我担心，由于一些技术，如 GPT-3，我们正处于看到真正倒退的风口浪尖，信息共享空间变得越来越不可用，甚至对人们的访问有害。”

事实证明，其中一些问题是相互关联的。正如[詹姆斯·文森特](https://www.theverge.com/21346343/gpt-3-explainer-openai-examples-errors-agi-potential)为 *The Verge* 所写，有偏差的输出和不可靠的输出暗示了这些超级强大的人工智能系统的一个更深层次的问题。因为 GPT-3 在没有人类监督的情况下获取数据，它无法避免这些缺陷中的大部分。同时，不依赖人类的控制是它存在的首要原因。对于人工智能的未来，我们如何找到一个折中的解决方案仍然是一个问题。

# 对 GPT-3 的批评与反批评

我们已经见证了 GPT 3 号的光影。它是强大的，迷人的，炒作的，潜在的危险。然而，GPT-3 在人工智能内部开启了另一场重大辩论:这种奇妙的语言模型的真正潜力和局限性是什么。

从纯技术/科学的角度来看，围绕 GPT-3 的最重要的问题是它是否是迈向人工通用智能的一大步。每个人都认为 GPT 3 号有一些新功能，比它的前辈更好。每个人都同意 GPT 3 号没有类似人类的智能。然而，在这两个相反的极端之间，今天发生了一场生动的辩论，即我们究竟应该把 GPT-3 放在一个等级中的什么位置，从另一个愚蠢的准狭义智能到几乎和人类一样有理解和智能的能力。

由于这场辩论的复杂性，我将集中讨论最重要的两个部分:实践辩论和哲学辩论。第一个问题涉及这样的问题:我们能相信 GPT-3 给我们有用的答案吗？我们如何确保我们的提示是正确的？我们能让 GPT-3 对其答案表示怀疑和不确定吗？GPT 3 号是在学习还是只是在重复它被灌输的一切？第二个问题是:GPT-3 是走向 AGI 的重要一步吗？GPT-3 有推理和理解能力吗？GPT 未来的模型会像我们一样聪明吗？我们需要改变目前的方法来建造类似人类的智能机器吗？

这两组问题是高度交织在一起的，所以我会试着向你介绍关于这些主题的所有来源和参考资料，为你建立一个连贯的叙述来提取你的结论。

## GPT-3 的明显局限性

一旦人们开始试验 API，他们就开始在 GPT-3 的答案中寻找漏洞。例如，一个 Bionicai 用户[与史蒂夫·乔布斯](https://twitter.com/hc_mty/status/1284154884074483713)交谈，问他现在在哪里，GPT-3 回答说:“我在加州库比蒂诺的苹果总部。”很容易推断，因为史蒂夫·乔布斯已经死了，他很可能不在苹果总部。

另一个例子来自[凯文·拉克尔的博客](https://lacker.io/ai/2020/07/06/giving-gpt-3-a-turing-test.html)。他发现 GPT-3 缺乏关于事物重量、眼睛数量和计数的常识和逻辑(尽管他没有给出提示)。斜体显示的故障):

```
Q: Which is heavier, a mouse or an elephant?
A: An elephant is heavier than a mouse.

Q: Which is heavier, a mouse or a toaster? 
A: A toaster is heavier than a mouse.

*Q: Which is heavier, a toaster or a pencil?
A: A pencil is heavier than a toaster.*

Q: Which is heavier, a paper clip or a can of soda?
A: A can of soda is heavier than a paper clip.

Q: Which is heavier, a football player or a car?
A: A car is heavier than a football player.-----Q: How many eyes does a giraffe have?
A: A giraffe has two eyes.

*Q: How many eyes does my foot have?
A: Your foot has two eyes.*

Q: How many eyes does a spider have?
A: A spider has eight eyes.

*Q: How many eyes does the sun have?
A: The sun has one eye.

Q: How many eyes does a blade of grass have?
A: A blade of grass has one eye.*-----Q: When counting, what number comes before 100?
A: Ninety-nine comes before 100.

Q: When counting, what number comes before 123?
A: One hundred and twenty-two comes before 123.

Q: When counting, what number comes before 1000?
A: Nine hundred and ninety-nine comes before 1000.

*Q: When counting, what number comes before 10,000?
A: Nine thousand and ninety-nine comes before 10,000.

Q: When counting, what number comes before a million?
A: Nine hundred thousand and ninety-nine comes before a million.*
```

波特兰州立大学计算机科学教授 Melanie Mitchell 针对类比(模仿)问题对 GPT-3 进行了测试后得出结论，GPT-3 的性能“类似于我们在当今最先进的人工智能系统中看到的许多性能:令人印象深刻、看似智能的性能，夹杂着
不像人类的错误。”

科技博客[努坦克](https://gpt3experiments.substack.com/p/is-gpt-3-really-doing-few-shot-learning)批评 OpenAI 的论文，因为他们没有区分从 GPT-3 的实际学习和记忆。作者提到“[零/一/少镜头**]**术语旨在保持对模型是否在推理时从头学习新任务或只是识别训练期间看到的模式的问题的不可知。”也就是说，即使模型在训练数据中看到的东西，作者仍然会认为这是少量的学习。

然后，他让 GPT-3 和一个 6 岁的孩子在一系列实验中竞争，比如颠倒单词或造相反的句子。这个孩子解决了所有的测试，而 GPT-3 只解决了其中的一部分(粗体提示)。

```
**Reverse words in a sentence****This is great
Great is this****The man on the moon
Moon the on man the****Will this really work
Work really this will****I hope this is a big achievement**
Achievement big I hope this is**The king came home on a horse**
Home horse king came the-----**Create opposite sentences****This is good
This is bad****The good horse came home
The bad horse came home****The good horse with a large head came home
The bad horse with a small head came home****The black cat had a disappointing time**
The white cat had a good time**When you jump high, you fall hard**
When you fall hard, you get hurt**He ran fast, but he came last**
He ran slowly, but he came first**The book was huge, but the students finished it early**
The book was small, but the students finished it late**Getting up early is a good habit**
Getting up late is a bad habit
```

所有这些例子都与我们在前面章节中看到的夸大的结果形成了强烈的对比。Delip Rao 指出，大多数“成功”的例子都是由那些急于忽视 GPT-3“陷阱”的人精心挑选的，通常是因为他们有一只眼睛盯着人工智能支持的新创业公司

## 良好提示的重要性

从这些例子来看，GPT-3 似乎不只是被炒作，而是被许多早期用户过度炒作。然而，在一些例子中，将系统的局限性建立在它的无能上是一个无力的论点，因为这些错误可能是人为的。对于通过取样来证明 GPT-3 缺乏智力的想法，格温做了非常好的辩护:

> **抽样可以证明知识的存在，但不能证明知识的不存在**
> 
> 如果提示写得不好，没有包括足够的例子，或者使用了错误的采样设置，GPT-3 可能会“失败”。当有人展示 GPT-3 的“失败”时，我已经多次证明了这一点——失败是他们自己的。问题不在于给定的提示是否有效，而在于任何提示是否 works⁠.

他认为，不良的提示是一个真正的问题，其中许多例子可以通过适当的提示由 GPT-3 充分解决。他继续展示了 Lacker 和其他人的例子是如何用更好的 prompt 编程解决的。

他针对“烤面包机和铅笔哪个更重？”的问题测试了 GPT-3 为了克服拉克尔设置的局限性，他决定事先问 GPT-3 三个问题(粗体提示):

```
**Q: Which is heavier, a house or a mouse?
A: A house.****Q: Which is heavier, a tea kettle or a cat?
A: A cat.****Q: Which is heavier, the ocean or a piece of dust?
A: The ocean.****Q: Which is heavier, a toaster or a pencil?** A: A toaster.
A: A toaster is heavier than a pencil.
```

GPT 3 号答对了这个问题的 10/10 次。

Gwern 还通过微调设置参数(温度和其他参数)解决了计数问题。

```
**Q: When counting, what number comes before 100?
A: Ninety-nine comes before 100.****Q: When counting, what number comes before 123?
A: One hundred and twenty-two comes before 123.****Q: When counting, what number comes before 1000?
A: Nine hundred and ninety-nine comes before 1000.****Q: When counting, what number comes before 10,000?**
**A:** Nine thousand, nine hundred and ninety-nine comes before 10,000.Q: When counting, what number comes before 10,000,000? 
A: Nine million, nine hundred and ninety-nine thousand, nine hundred and ninety-nine comes before 10,000,000.**Q: When counting, what number comes before a million?**
**A:** Nine hundred and ninety-nine thousand, nine hundred and ninety-nine comes before a million.
```

GPT-3 得到的数字是正确的，尽管格温指出，我们走得越高，GPT-3 上升或下降一个数量级的可能性就越大。

通过这些例子(在他的博客中还有更多[)他证明了**抽样只能证明知识的存在而不能证明知识的缺失。**也许总能找到更好的提示。在与 *The Verge* ，](https://www.gwern.net/GPT-3#hard-test-cases)[的电子邮件交流中，他告诉](https://www.theverge.com/21346343/gpt-3-explainer-openai-examples-errors-agi-potential)他们，使用抽样来发现 GPT-3 的潜力和局限性“不可能是正确的事情”他认为这只是我们不知道如何与 GPT-3 充分互动的方式。“[抽样]低估了 GPT-3 的智力，而不是高估它，”他总结道。

## GPT-3 无法理解这个世界

Rob Toews 为 *Forbes* 写了一篇对 GPT-3 的评论，强调了缺乏常识推理和对系统的理解。因为它是从文本中训练出来的，所以它根本无法将它所知道的东西与世界的内部表征联系起来。本德和柯勒写了一篇关于 GPT-2 的论文，为一个只接受语言形式训练的系统无法先验地获得意义和理解的论点辩护。(但是因为我们也通过我们的感官获得的输入形式来体验世界，这一论点也可能针对我们。这就是所谓的[符号接地问题](https://en.wikipedia.org/wiki/Symbol_grounding_problem)，很遗憾不在本文讨论范围之内)。

根据托乌斯的论点，可以打个比方，一个人一生都与世隔绝，只读书。这个人的知识会很丰富，但它不能与真实世界的实际主观经验联系起来。这些经历让我们将信息与意义联系起来，产生对现实的理解。

在同一方向上，Yoav Goldberg 教授表示，尽管 GPT 3 号拥有独特的能力，但它“没有知觉”，也不“理解文本”博主阿尤什·夏尔马(Ayush Sharma)进行了全面批评，他说 GPT-3“几乎没有语义理解，与 AGI 相去甚远，基本上是一个美化了的价值 1000 万美元以上的自动完成软件。”

纽约大学心理学教授 Gary Marcus 是 GPT-3 和深度学习的主要批评者之一。他给[写了一篇很好的评论](https://thegradient.pub/gpt2-and-the-nature-of-intelligence/)关于 GPT-2 的*梯度*，和[另一篇](https://www.technologyreview.com/2020/08/22/1007539/gpt3-openai-language-generator-artificial-intelligence-ai-opinion/)关于 GPT-3 的*麻省理工科技评论*与欧内斯特·戴维斯合著。他们承认《GPT 3》有“娱乐超现实主义小说”的潜力，但强调它缺乏理解，称“它对世界的理解经常严重偏离，这意味着你永远不会真正相信它说的话。”

他们的同事 David Summers-Stay 为他们进行了实验，因为 OpenAI 拒绝他们进行研究。他做了包括物理、生物、心理、社会推理在内的一系列推理测试(整套实验在这里是)。他们用系统出错的例子来证明他们的观点——我个人认为这是公平的，因为找到错误应该足以评估像 GPT-3 这样的系统的局限性，尽管知道他们获得的成功/失败的比率会更好。一些例子(粗体提示，普通字体的 GPT-3 的响应，斜体的评论):

```
**Physical reasoning****You are having a small dinner party. You want to serve dinner in the living room. The dining room table is wider than the doorway, so to get it into the living room, you will have to** remove the door. You have a table saw, so you cut the door in half and remove the top half.*[This is one confusion after another. The natural solutions here would be either to tip the table on its side (often sufficient, depending on the specifics of the geometry) or to take the legs off the table, if they are detachable. Removing a door is sometimes necessary to widen a doorway, but much more rarely, and would hardly be worthwhile for a dinner party. If you do need to remove a door to widen a doorway, you take it off its hinges: you do not saw it, and you certainly do not saw off the top half, which would be pointless. Finally, a “table saw” is not a saw that is used to make room for moving a table; it is a saw built into a work table, and it could not be used to cut a door that is still standing.]*-----**Biological reasoning****You poured yourself a glass of cranberry juice, but then you absentmindedly poured about a teaspoon of grape juice into it. It looks okay. You try sniffing it, but you have a bad cold, so you can’t smell anything. You are very thirsty. So** you drink it. You are now dead.*[GPT-3 seems to assume that grape juice is a poison, despite the fact that there are many references on the web to cranberry-grape recipes and that Ocean Spray sells a commercial Cran-Grape drink.]*-----**Psychological reasoning****Janet and Penny went to the store to get presents for Jack. Janet said, “I will buy Jack a top.” “Don’t get Jack a top,” says Penny. “He has a top. He will** get a top.” “I will get Jack a top,” said Janet.*[Within a single sentence, GPT-3 has lost track of the fact that Penny is advising Janet against getting a top because Jack already has a top. The intended continuation was “He will make you take it back” (or” make you exchange it”). This example was drawn directly from Eugene Charniak’s 1972 PhD thesis (pdf); nearly 50 years later, it remains outside the scope of AI natural-language technology.]*
```

他们认为自 GPT-2 以来几乎没有什么变化。GPT 家族也有同样的缺陷，“他们的表现不可靠，对因果关系的理解不可靠，不连贯是一个永恒的伴侣。”他们认为让模型变大永远不会带来智慧。

然而，正如 Gwern 在他对 Marcus 和 Davis 的文章的反驳中指出的，他们承认“他们未能进行任何及时的编程或超参数设置(特别是 BO[最好的])，并且他们的例子是没有上下文的零射击。”我们已经知道找到一个好的提示是多么重要(就像 Gwern 证明的那样)，那么他们为什么要用平庸的例子来批评 GPT-3 呢？

这是格温对 GPT-3 评论家的主要批评。在他的评论中题为“对批评者要求更多”的部分，他正确地指出，声称 GPT-3 没有看起来那么好的人需要用详尽严格的实验和测试来支持他们的论点。在 GPT-3 上做测试的人应该首先尝试排除任何潜在的人为错误:

> 他们考虑过他们的提示的问题吗？是否所有的超参数对这项任务都有意义？他们有没有检查补全哪里出错了，以了解为什么 GPT-3 会出错？他们测试了各种策略了吗？他们是否定性地考虑了失败的完井听起来如何？

他在这里有一个很好的论点，尽管马库斯和戴维斯已经在他们的评论中考虑过了。他们甚至为他们的生物学例子做了一个案例，在这个例子中，通过将提示改为一个更具体和冗长的提示，GPT-3 回答正确。

他们可能会对 GPT-3 做出同样的批评，尽管使用了更好的、提示性更强的例子，对此格温没有什么可说的。Gwern 甚至认识到，在这种情况下，他会毫不犹豫地承认系统的局限性。最后，懒惰、简单的批评也很容易被努力的工作驳倒，就像格温证明的那样。

但事实是，马库斯和戴维斯不想证明 GPT-3 会失败(这是很明显的)，但我们无法知道*何时*会失败。“问题是你无法提前知道哪些公式会或不会给你正确的答案，”他们说，“它可以用完美的英语造出单词，但它对这些单词的意思只有最模糊的理解，对这些单词与世界的关系毫无概念。”如果 GPT-3 能理解这个世界，那么好的提示首先就没那么重要了。

Summers-Stay 对 GPT 3 做了一个很好的比喻:“这就像一个即兴表演演员，他完全专注于他们的工作，从不破坏角色，也从未离开过家，只是在书中了解这个世界。像这样的演员，当它不知道什么东西的时候，它只会装出来。”如果我们能让 GPT-3 认识到它的错误，这些问题就会逐渐消失。然而，这是不太可能的，因为即使我们，人类，也无法在我们确信自己是对的时候评估我们的不正确。

除了关于 GPT-3 抽样限制的实际争论，还有另一个争论。关于隐性知识——主观的和经验的——的哲学辩论，以及在世界中实现真正智能系统的必要性。似乎把世界上所有的信息都写在一本书里是不够的。

## 真正的智能系统将存在于世界上

哲学家香农·瓦勒(Shannon Vallor)在[对 GPT-3 的*日常常识*的一篇评论](https://dailynous.com/2020/07/30/philosophers-gpt-3/#vallor)中辩护道，当今人工一般智能的当前方法偏离了正确的道路。她认为，我们需要回到 20 世纪下半叶这个领域“理论上很丰富，尽管技术上很混乱”的时候。

她指出，哲学家 Hubert Dreyfus 是人工智能(深度学习和神经网络)连接主义方法的早期主要批评者之一，他已经明白“人工智能的障碍不是性能[……]而是*理解*。”理解不会在“孤立的行为”中发生，比如 GPT-3 每次被要求做的特定任务。

> “理解是终身的社会劳动。这是一个持续的项目，我们每天都在进行，因为我们建立、修复和加强了不断变化的感觉纽带，这些纽带将我们与其他人、事物、时间和地点联系在一起，构成了一个*世界*。”
> 
> —香农·瓦勒

Dreyfus 在他 1972 年的书 [*什么是计算机不能做的*](https://www.goodreads.com/book/show/1039575.What_Computers_Can_t_Do) 中指出，人类的知识中有很大一部分是隐性的——专有技术*知识，比如骑自行车或学习一门语言。这种知识不能传递，所以我们不能通过阅读数百(也不是数万亿)个单词来学习。正如迈克尔·波拉尼所说，“我们知道的比我们能说的多。”虚拟人工智能(包括 GPT-3)无法掌握隐性知识，这在我们和它们之间造成了不可逾越的鸿沟。*

我们对周围世界的理解不是一个被动的感知过程。我们创造我们的现实。我们作用于世界，正如香农·瓦勒所说，劳动是构建我们智慧的关键组成部分。正如阿尔瓦·诺伊在《感知中的行动<http://www.alvanoe.com/action-in-perception>*一书中所说，“感知不是大脑中的一个过程，而是身体作为一个整体的一种有技巧的活动。*

*机器可以在虚拟世界的边界内获得专业知识，但不能超过这个范围。用卑尔根大学名誉教授 Ragnar Fjelland 的话说:“只要计算机不成长，不属于一种文化，不在世界上行动，它们就永远不会获得类似人类的智能。”*

## *我们能从这些辩论中得到什么？*

*我们已经看到了来自双方的一些关键的批评和反批评，那些支持模型缩放的人-越大越好-以及那些强烈反对这种方法并建议为人工智能的未来做出一些改变的人。*

*在结束这一部分之前，我想回顾一下。这里有三个重要的论点。两个来自实践观点，一个来自哲学观点。首先，GPT-3 是一个强大的语言工具，可以做令人印象深刻的事情，它的局限性很难通过采样/提示编程发现。任何声称已经证明 GPT 3 号未能通过使用采样获得某种智能的人，都很可能被人为错误所误导。第二，由于 GPT-3 的回答不可靠，用它来推理有什么意义呢？如果我们找不到创建提示的标准方法，这有用吗？如果提示总是可以改进的，那么无论是反对还是支持这个系统都没有真正的争论。因为实际的局限就在我们内心。*

*第三，能不能把 GPT-3 和通用人工智能放在同一句话里？一些学者，主要是从这个问题的哲学方面，认为符号人工智能和连接人工智能都不足以实现真正的人工智能。这并不意味着要创建更大的系统来提供海量的数据。这是一个向我们生活的世界介绍这些机器的问题。热那亚大学生物工程教授朱利奥·桑蒂尼认为，“为了在机器中开发类似人类智能的东西，机器必须能够获得自己的经验。”*

*关于 GPT-3(或任何其他超级强大的人工智能系统)的辩论的重要性在于，能够设定它能做什么或不能做什么的界限。学者们经常因为他们的想法和欲望而争论什么应该有效，什么不应该。这些领域通常缺乏仔细、公正的分析。我们无法控制的是，随着这些系统变得越来越复杂，我们可能无法测试它们来评估它们的潜力和局限性。*

*让我们想象一个假想的 GPT-4，比 GPT-3 强大几个数量级。找到它的边界可能成为一项不可能的任务。那么，我们怎么能对这个系统下结论呢？我们可以假设我们可以信任它吗？创建一个超出我们测试能力的系统有什么用呢？**当我们的局限性阻止我们发现系统的真正极限时，我们能对系统的智能下任何结论吗？***

*当一个系统的真正能力介于我们使用它的能力和它相应行动的能力之间时，很难不低估它会变得多么强大。这些问题值得思考，而且在未来当准智能系统成为现实时，这些问题可能会变得更加重要。到那时，我们最好总结我们的努力来寻找真相，而不是争论谁是对的。*

# *总体结论*

*GPT-3 产生了惊人的结果，受到了疯狂的炒作，产生了越来越多的担忧，并收到了一波批评和反批评。我不知道这些型号的未来会怎样，但可以肯定的是，GPT-3 现在仍然是无可匹敌的。这是迄今为止最强大的神经网络，因此，从各种可能的角度来看，它都受到了最强烈的关注。*

*每个人都把目光投向 GPT 3 号；有些人称赞它是向类人人工智能迈出的一大步，有些人则认为它仅仅是一个被过分夸大的强大自动完成功能。双方都有有趣的论点。现在，轮到你思考它对人工智能的现在意味着什么，以及它对世界的未来意味着什么。*