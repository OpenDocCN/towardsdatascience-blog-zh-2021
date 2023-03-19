# 随机鹦鹉论文:对未来危害的警告

> 原文：<https://towardsdatascience.com/stochastic-parrots-paper-alarm-for-future-harm-6bc2abc5e327?source=collection_archive---------35----------------------->

## 了解紧迫的问题和当前的缓解措施，以减少 NLP 研究中的偏见和公平性

![](img/0463f45ef04b682384dd14b23416c565.png)

[廷杰伤害律师事务所](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在过去的三年里，在自然语言处理(NLP)领域推出了 [Transformer](https://lilianweng.github.io/lil-log/2020/04/07/the-transformer-family.html) s 之后，已经有了无数的突破。此外，关于 NLP 研究和人工智能技术进步的副产品所造成的危害，有越来越多的意识、讨论和辩论。NLP 伦理领域的最后一年以有争议的解雇而结束，新的一年以最令人期待的[随机鹦鹉](http://faculty.washington.edu/ebender/papers/Stochastic_Parrots.pdf)论文的发表而开始，该论文由 [Timnit Gebru](https://googlewalkout.medium.com/standing-with-dr-timnit-gebru-isupporttimnit-believeblackwomen-6dadc300d382) 等人在 [facct 2021](http://facctconference.org/) 上发表。我感谢作者们为触发警报所做的努力。

文章发人深省。我看过很多关于这篇论文的正反讨论[ [1](https://algorithmicfairness.wordpress.com/2021/01/23/on-stochastic-parrots/) 、 [2](https://gist.github.com/yoavg/9fc9be2f98b47c189a513573d902fb27) 、 [3](https://tusharc.dev/papers/stochastic_parrots_bender.html) 、 [4](https://www.youtube.com/watch?v=bf3KJC1sQ30) 。[这篇有争议的文章中有什么](https://pub.towardsai.net/on-the-dangers-of-stochastic-parrots-summarized-7eb370bc3d7b) post 很好地总结了这篇文章，但没有表明立场。发表偏倚是指倾向于只发表积极的和有意义的结果。我相信这篇论文是克服发表偏倚的一个例子。这引发了学术界和企业界关于技术危害及其对社会影响的讨论。在过去的几个月里，我一直在研究自然语言处理和负责任的人工智能研究中的人工智能伦理、偏见和公平。本文旨在提出本文讨论的一些迫切问题。总结当前的研究成果，以减轻或揭示大型语言模型的问题。

Gebru 等人讨论了大型语言模型(LLM)对环境的影响、财务成本、与从互联网上公开可用的免费文本中获得的大型训练数据相关的风险、对边缘社区造成的危害以及该领域研究方向的问题。本文还提供了一些缓解这些问题的建议。讨论的危害对所有尺寸的 LMs 都是真实的。LLM 展示了人类在技术发展中所能达到的顶峰。自 LLMs 开发以来，在应用开发方面取得了令人印象深刻的工程成就，一些值得注意的应用是 [DALL-E 和 Clip](https://www.technologyreview.com/2021/01/05/1015754/avocado-armchair-future-ai-openai-deep-learning-nlp-gpt3-computer-vision-common-sense/) 。作为一名研究人员，我总是被研究突破所感动。我们正试图打破导致创新应用开发的限制。然而，本文中提出的问题是为了反映底线，即在我们创建 LLM 之前，我们是否可以做更多的研究来了解和减轻当前可用的 LM 所造成的危害和问题？

# 一些个人想法

## 环境和财务成本

LM 模型的训练会导致巨大的财务和[环境成本](https://www.technologyreview.com/2019/06/06/239031/training-a-single-ai-model-can-emit-as-much-carbon-as-five-cars-in-their-lifetimes/)。环境关注点适用于许多新兴技术。作者提到，“*在 GPU 上训练一个单一的 BERT 基本模型(没有超参数调整)估计需要的能量相当于一次穿越美国的飞行。”*也许作者提供的关于环境成本的比较值得商榷。然而，真正要问的问题是:我们正在做什么来解决这些新兴技术带来的环境问题？

世界上存在着经济和技术差距。培训 LLM 会产生巨大的财务成本。谁在训练这些 LLM 模型？谁将从应用程序中获益最多？

## 大型语言模型(LLM)中使用的训练数据

读完这篇论文后，我发现主要的和令人担忧的问题是现代逻辑推理小说如何对霸权文本进行编码。用于训练 GPT-2 的文本是通过抓取 Reddit 出站链接收集的，每个人都知道，Reddit 中属于特定年龄、阶级和种族的男性人数过多。当我们用有偏见的基于网络的训练数据开发应用程序时，会反映出什么类型的意识形态？最近的研究显示，LLMs 中持续存在着[刻板印象](https://arxiv.org/abs/2004.09456)、[性别](https://venturebeat.com/2020/04/22/stereoset-measures-racism-sexism-and-other-forms-of-bias-in-ai-language-models/)和[反穆斯林](https://arxiv.org/pdf/2101.05783.pdf)偏见。我们需要更加关注使用主流文本作为不完全代表世界的训练数据。互联网上用户生成文本的人口统计数据是多少？这就把我们带到了下一个问题。人工智能模型应该对世界进行编码，还是应该更好？谁来决定？

使用公开数据的另一个主要问题是隐私问题，因为数据集可能包含个人信息。研究人员已经表明，可以从 LLMs [ [8](https://arxiv.org/abs/2012.07805) ]中提取训练数据。自从数字营销兴起以来，数据隐私一直是最紧迫的问题。我们将如何解决人工智能隐私问题？与开发新技术相比，目前有多少量化和解决隐私问题的努力正在进行中？

## 静态快照 LLMs

提出的另一个要点是，LLM 是时间的静态快照，因为鉴于成本较高，重新培训 LLM 的机会较少。如论文所述，LLM 是随机鹦鹉。近年来，在*黑人的命也是命*、*模仿运动以及负责任的人工智能和人工智能伦理*方面已经做了大量工作，这些工作将不会在这些 LLM 中反映出来。作者讨论了谁会从进步中受益，谁会受到伤害，因为技术的使用还没有达到社会的大部分。你认为伯特知道新冠肺炎吗？静态快照将如何描绘动态世界？

## 我们如何校准 LLM？

主要问题“语言模型会不会太大？”这让我想到我的孩子掌握语言的能力。他的词汇量有限，因为他还在学习这门语言。对于一个 6 岁的孩子来说，许多事情都没有意义，因为大脑整合正在发展中。他有时会在错误的地方用词，这些错误会慢慢帮助他下次改正。我想到了一个思维实验。如果给一个 5-6 岁的孩子看来自维基百科、Reddit、YouTube 和脸书视频的所有数据。结果会是什么？在我看来，这将是一个更大的语言模型。前几天，他从一个儿童电视节目中学到了一个 F 字。他用了这个词，我必须告诉他这个词在孩子的语言中是什么意思，以及为什么不用它。然而，成人使用被认为是正常的。什么是好什么是坏是很主观的。但成长是一个学习、观察、互动和犯错的过程，最终会在成年后校准我们的系统。

我们在训练这些不同大小的语言模型时，是否理解了这些模型可能犯的错误？我们要如何校准这些 LLMs 和 AI 系统？然而，实践经验(使用 LMs 构建技术)可能会发现隐藏的危害，犯错误和改正错误将有助于我们开发更好更公平的技术。我们可以用现有的 LM 来做这件事吗？

## LLMs 中最近的研究偏差和公平性

去年，来自 OpenAI、斯坦福以人为中心的人工智能研究所和其他大学的研究人员聚集在一起，讨论围绕最大的语言模型“生成性预训练变形金刚 3”(GPT-3)的开放式研究问题。LLMs 开发在推动开始询问[如何创建更符合伦理的语言模型](https://ai.science/l/1cee1278-489f-417f-a272-9801f06b9cc2)的努力中发挥了重要作用？在开发这些算法时，需要在为 LM 开发[受控文本生成、如何评估算法、](/controlling-text-generation-from-language-models-6334935e80cf)[如何评估偏差](https://arxiv.org/abs/2101.11718)、如何[评估数据集](https://arxiv.org/pdf/2102.12382.pdf)、[算法审计、](https://www.aiethicist.org/frameworks-guidelines-toolkits)和全包团队的方向上进行研究，并在追求研究方向时仔细权衡这些风险。这篇论文发出的警报引起了人们对 LLM 危害研究的关注。在最近的一篇论文中， [Creolizing the web](https://arxiv.org/pdf/2102.12382.pdf) 提出了一种理解来自 Reddit 的真实世界数据集的方法，以识别社区和观点的回音室。我们需要像这样的技术来评估大型数据集，然后才能训练更大的模型。最有希望的方法是用[人在回路框架(HITL](https://arxiv.org/abs/2103.04044) )校准人工智能系统。从研究努力的方向来思考是很重要的。通过提问。与新技术的发展相比，目前有多少研究工作是在减轻或减少技术上的偏见和公平？

总之，新技术的影响是不可预测的，我们无法预见它将如何改变世界。我们应该把这篇论文看作是向提出问题和提供解决方案的建议迈出的一步。现在是时候提出更多问题并找到答案，以创造一个公平的技术，创造一个更美好的世界。让我们用所有的镜头，努力寻找有意义的未来方向。

感谢 Suhas Pai 的观点、讨论和对本文写作的帮助。