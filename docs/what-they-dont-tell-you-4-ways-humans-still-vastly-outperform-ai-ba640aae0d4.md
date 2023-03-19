# 他们没有告诉你的:人类仍然远远胜过人工智能的 4 种方式

> 原文：<https://towardsdatascience.com/what-they-dont-tell-you-4-ways-humans-still-vastly-outperform-ai-ba640aae0d4?source=collection_archive---------18----------------------->

## 人工智能

## 媒体报道经常把人工智能描绘得比它实际上更聪明。

![](img/aeb9a7c8da19673c2c4e15d2ee14fe4d.png)

安德烈·亨特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

人工智能系统看起来如此智能，因为它们更多地暴露了反映它的成就。现实告诉我们并非如此。

每当人工智能有显著突破时，我们只会听到系统变得多么智能和熟练。2012 年[辛顿的团队](https://papers.nips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)在 ImageNet 挑战赛中获得了 63%的最高准确率。几年后，一个系统以惊人的+90%的最高准确率超越了人类的表现。新闻:*“AI 能比人类更好地识别物体。”*嗯，不当他们在真实世界的对象数据集上测试这个精确的模型时，它的性能[下降了 40–45%](https://objectnet.dev/objectnet-a-large-scale-bias-controlled-dataset-for-pushing-the-limits-of-object-recognition-models.pdf)。

去年，人们为 GPT 3 号疯狂。《纽约时报》、[、《卫报》](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)、[、《连线》](https://www.wired.com/story/ai-text-generator-gpt-3-learning-language-fitfully/)、 [TechCrunch](https://techcrunch.com/2021/03/17/okay-the-gpt-3-hype-seems-pretty-reasonable/?guccounter=1&guce_referrer=aHR0cHM6Ly90b3dhcmRzZGF0YXNjaWVuY2UuY29tL3VuZGVyc3RhbmRpbmctZ3B0LTMtaW4tNS1taW51dGVzLTdmZTM1YzNhMWU1Mg&guce_referrer_sig=AQAAAGdVHFC7GKadd8zS8d_I8mFDC7amZXt0t3EefHemPQNI612MyNnHHPh-Px9husi4k7E_LaffuzY-N3WvoiLtoYMZhntKwRpZNgT7L2uGea1CUCdUjSHLnhp-ZPdi-eweYl3rS9ScmsJg_g04t5DtgludKVyFa-a4DVsAqvu7AUs8) 、 [DigitalTrends](https://www.digitaltrends.com/features/openai-gpt-3-text-generation-ai/) 以及许多其他享有盛誉的新闻杂志都在传播它惊人的能力。炒作过头了，OpenAI 的首席执行官 Sam Altman 不得不降低语气:

> “[GPT-3]令人印象深刻[……]但它仍然有严重的弱点，有时会犯非常愚蠢的错误。人工智能将改变世界，但 GPT 3 号只是非常早期的一瞥。”

相比之下，几乎没有人谈论 AI 仍然非常愚蠢的所有任务。人工智能不太聪明不会成为吸引人的标题。尽管深度学习系统取得了所有的成功，但人工智能仍然在一些关键方面与“超人”相反。加里·马库斯是纽约大学的心理学教授，也是著名的人工智能专家，他在自己的书《重启人工智能 中提到了一些例子。在这篇文章中，我将解释我们在哪些方面仍然远远胜过人工智能——并且在可预见的未来仍将如此。

# 理解语言——语用学

生成性预训练语言模型是人工智能的最新趋势。2017 年，谷歌发明了[变压器](https://arxiv.org/abs/1706.03762)架构，这已经成为大多数 NLP 系统的首选。鼎盛时期出现在去年，当时由 OpenAI 开发的基于变形金刚的模型 GPT-3 显示出在学习和生成语言方面无与伦比的能力。从令人难以置信的结果中可以得出结论，GPT 3 号掌握了语言。但是，它只擅长句法和语义:语言的形式和结构。它缺乏理解。它无法将形式与潜在的意义联系起来，也无法触及语言的语用维度。

我最近为数据科学*写了一篇文章，声称“人工智能不会很快掌握人类语言。”像 GPT-3 这样强大的艺术级模型可以生成人类级别的语言，但它们不知道为什么输出给定的句子。虽然 GPT-3 可以说:“我今天早上吃了一个苹果”，但这并不意味着它知道闻、摸或吃苹果是什么感觉。它缺乏实际去做的主观体验。*

语言的目的是将现实与对现实的心理表征联系起来。AI 无法访问我们共享的物理现实，因为它被困在虚拟世界中。如果我不说:“我今天早上吃了一个苹果”，而是说:“我今天早上吃了一个苹果。我去了商店，拿了它，吃了它，然后离开，“只有人类才能推断我偷了它——以及这么做的社会/法律含义。今天的人工智能无法获取这种类型的实用信息。

</ai-wont-master-human-language-anytime-soon-3e7e3561f943> [## 人工智能不会很快掌握人类语言

towardsdatascience.com](/ai-wont-master-human-language-anytime-soon-3e7e3561f943) 

# 了解世界——虚拟陷阱

反对机器学习是通向 AGI 的正确道路的最有力的论据之一是，智能需要具体化。联结主义人工智能——机器学习是其中最重要的范式——松散地基于[笛卡尔二元性](https://en.wikipedia.org/wiki/Meditations_on_First_Philosophy):思想和身体是不同的、分离的物质。因此，智慧可以在没有身体的情况下产生。哲学家[休伯特·德雷福斯](https://www.goodreads.com/book/show/1039575.What_Computers_Can_t_Do)是第一个批评这种观点的人。今天，有一个完整的子领域叫做[发展机器人](http://www.scholarpedia.org/article/Developmental_robotics)基于相反的想法:智能来自于大脑和身体与世界的互动。

除了具身认知，其他论点将连接主义者 AI 推向了绝境: [Judea Pearl](http://bayes.cs.ucla.edu/WHY/) 认为机器学习和深度学习是只能利用数据相关性的技术，而我们通过识别事件的因果来学习。约书亚·特南鲍姆(Joshua tenen Baum)希望给人工智能注入我们在进化过程中发展起来的直觉物理概念，而这正是机器所缺乏的。 [Gary Marcus](https://arxiv.org/abs/2002.06177) 捍卫了将连接主义人工智能与基于符号知识的旧范式相结合，向机器灌输常识的想法。

现在的 AI 没有具体化，不懂因果关系，不懂物理，也不懂语言，不会表现出常识性的推理。然而，我们每天都用这些能力在世界上导航。只要机器在计算机中生存和发展，被限制在虚拟现实的范围内，它们就不能理解世界。用 [Ragnar Fjelland](https://www.nature.com/articles/s41599-020-0494-4) 教授的话来说:“只要计算机不长大，不属于一种文化，不在世界上活动，它们就永远不会获得类似人类的智能。”

</artificial-intelligence-and-robotics-will-inevitably-merge-4d4cd64c3b02>  

# 适应新环境——外推

[Andre Ye](https://medium.com/u/be743a65b006?source=post_page-----ba640aae0d4--------------------------------) 给[写了一篇精彩的文章](https://medium.com/analytics-vidhya/you-dont-understand-neural-networks-until-you-understand-the-universal-approximation-theorem-85b3e7677126)深入神经网络的理论基础。在他的文章中——T4·史蒂芬·平克和加里·马库斯在推特上称赞了他的文章——他解释了一般化(内插法)和外推法之间的区别。他认为神经网络只不过是“伟大的近似器”。他说，神经网络的目标是在训练范围内进行归纳，而不是超出训练范围。本质上，神经网络是一个高度熟练的多维插值器。

但是人类可以推断。我们可以在给定的训练范围之外做出“合理的预测”外推包括将一个问题的结论转化为另一个不同的——通常更复杂的——性质的结论。机器学习系统无法执行这一过程，很大程度上是由于“独立同分布(iid)”假设。它声明真实世界的数据和训练数据具有相同的分布，这是错误的。深度学习革命的三位领先先驱 Geoff Hinton、Yoshua Bengio 和 Yann LeCun 在最近的一篇论文中解释了为什么这是一个关键缺陷:

> “人类可以以一种不同的方式进行归纳，这种方式比普通的 iid 归纳更强大:我们可以正确地解释现有概念的新组合，即使这些组合在我们的训练分布下极不可能出现。”

没有什么能保证机器学习系统能够像我们一样进行推断。这也是为什么 Elon Musk 不得不在本周早些时候收回他在推特上的话:

> “广义的自动驾驶是一个困难的问题，因为它需要解决现实世界人工智能的很大一部分。没想到这么难，但回想起来难度是显而易见的。没有什么比现实拥有更多的自由度。”

# 快速学习新事物——进化适应性

机器学习系统——尤其是深度学习系统——需要大量数据(已标记或未标记)来学习，需要大量计算能力来训练。三种主要的培训框架——监督学习、强化学习和自我监督学习——都受到这些限制。在击败 Stockfish 之前，AlphaZero 与自己进行了 4400 万场比赛。GPT-3 是用互联网的大部分文本数据训练出来的。国际象棋专家和世界级作家不需要这么多的练习。

总的来说，相比之下，人类儿童学得相当快。在 1 个月大的时候，语前婴儿能够辨别音素。1 岁前，他们可以推断因果关系。孩子们在 5 岁的时候就能通过稀疏的数据学会流利地说话。成年人也没什么不同。经过几个小时的练习，我们可以学会驾驶的基本知识。正如我们之前看到的，经过多年的模拟，自动驾驶汽车仍然远远低于我们的水平。

正如辛顿、本吉奥和勒村在他们的论文中提到的[，“人类似乎能够学习大量关于世界的背景知识，主要是通过观察，以独立于任务的方式。”我们从经验和观察中快速学习的能力是无与伦比的。这个难题的关键是进化。我们开发了很好的工具来导航世界，而人工智能是由我们直接设计的。人工智能必须通过学习来弥补进化优势的缺乏。我们来到这个世界上，大部分时间都在准备应付它。人工智能必须从零开始学习。](https://cacm.acm.org/magazines/2021/7/253464-deep-learning-for-ai/fulltext)

# 总结

AI 很擅长解决一些问题。机器学习系统开始在感知任务中表现出色，也称为系统 1 任务——借用心理学家丹尼尔·卡内曼的概念。但它们似乎无法主导高级认知能力。

人类也很擅长解决一些问题。与人工智能有一些重叠，但我们在更高的认知任务方面仍然没有挑战。推理、规划、决策、理解因果关系、语言或直觉物理……所有这些大脑过程仍然在人工智能的范围之外。而且看起来也不会很快改变。

## [跟我一起去未来旅行](https://mindsoftomorrow.ck.page/)了解更多关于人工智能、哲学和认知科学的内容！此外，欢迎在评论中提问或在 LinkedIn 或 Twitter 上联系！:)

# 推荐阅读

</unpopular-opinion-well-abandon-machine-learning-as-main-ai-paradigm-7d11e6773d46>  </the-hard-truth-data-science-isnt-for-everyone-1689b7c05e62> 