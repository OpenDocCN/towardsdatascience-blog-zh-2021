# 又一个最大的神经网络——但是为什么呢？

> 原文：<https://towardsdatascience.com/yet-another-largest-neural-network-but-why-f48d231972a9?source=collection_archive---------13----------------------->

## 人工智能|新闻|观点

## GPT 3 号的成功不断带来回报，但是我们真的还需要这样做吗？

![](img/a668ce83bcce1c555ab3bf2a8e3cb6f4.png)

纳丁·沙巴纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

它又发生了。微软和英伟达已经建立了一个新的最大的密集语言模型——是前冠军 GPT-3 的三倍。然而，与 GPT-3 相比，这个新模型没有引起任何骚动，无论是在媒体还是在人工智能社区。这是有原因的。

我以前多次谈到过大型语言模型(LLM)，这是一个通用术语，用于用大量数据预先训练的基于大规模转换器的神经网络。我以前的文章有明确的技术语气，回答了像它们为什么存在，它们有什么能力，或者它们有什么局限性这样的问题。直到现在，我还没有努力后退一步，尝试在我们社会的大背景下理解 LLM，以及它们如何与人工智能和技术的其他方面相互关联。

今天，我将从另一个角度来解决一些我们需要分析以避免盲目行走的关键问题:我们真的需要另一个最大的神经网络吗？这些模型的目的是什么，人工智能社区期望从中获得什么？无止尽地遵循缩放模型的道路会有什么后果——特别是对于人工智能社区，以及整个世界？如果我们给人工智能和人工智能的其他方法更多的空间，不是更好吗(AGI)？

# 大型语言模型——划分人工智能社区

自从 2017 年[的变形金刚架构](https://arxiv.org/abs/1706.03762)的流行，以及 2018 年的 BERT [和 2020 年](https://arxiv.org/abs/1810.04805)的 GPT-3 [的成功，LLM 的趋势已经无处不在。在 Google、OpenAI 和其他公司证明了这些模型的有效性和实用性之后，世界各地的研究人员和开发人员开始复制他们的方法。LLM 现在如此普及，以至于我们几乎把它当成了深度学习的同义词。](https://arxiv.org/abs/2005.14165)

最近，几十名斯坦福大学的研究人员将法学硕士命名为“基础模型”。他们认为，这些越来越大的神经网络构成了“构建人工智能系统的新兴范式。”但并不是所有的 AI 专家都认同这个花哨的标题。

伯克利的计算机科学教授 Jitendra Malik 说“基金会这个词是非常错误的。”他补充说，“我们在这些模型中使用的语言是没有根据的，有这种虚假，没有真正的理解。”佐治亚理工学院[的教授 Mark Riedl 在 Twitter 上说](https://twitter.com/mark_riedl/status/1428398136939192328)“将非常大的预训练神经语言模型作为‘基础’模型是一个辉煌的……公关噱头。”

虽然他们承认这些模型的有效性，但他们批评这些模型缺乏理论背景。事实证明，LLM 在语言甚至[视觉任务](https://arxiv.org/abs/2010.11929)方面比以前的模型做得更好，但他们缺少对语言和世界的深刻理解，而这是任何被称为“基础”的东西所需要的。

长期深度学习评论家加里·马库斯(Gary Marcus)在[中就该主题进行了彻底的论证](https://thegradient.pub/has-ai-found-a-new-foundation/#:~:text=In%20the%20final,models%20at%20all%3F)，称“将‘预训练语言模型’重新称为基础模型是一种误导……该报告称，毫无疑问，‘我们并不完全了解基础模型所提供的基础的性质或质量’，但为什么要大言不惭地称它们为基础模型呢？”

但是“基础模型”并不是人工智能社区给予 LLM 的唯一头衔。在一篇日期为 2021 年 3 月的论文中，艾米丽·m·本德、蒂姆尼特·格布鲁和其他人将这些模型称为“随机鹦鹉”。他们分析了与 LLM 相关的不同风险和成本，并得出结论，研究人员在从事这些研究时应该小心谨慎。

这一段最好地抓住了他们的立场:“从互联网摄取的训练数据编码霸权世界观的趋势，LMs 放大训练数据中的偏见和其他问题的趋势，以及研究人员和其他人将 LM 驱动的性能提升误认为实际自然语言理解的趋势……呈现了现实世界的伤害风险。”

无论是“基础模型”还是“随机鹦鹉”，很明显 LLM 在人工智能的当前状态中发挥着巨大的作用。专家们持有不同的观点，也没有明确的前进方向。我们不知道他们的技术极限，我们不知道我们是否能够保持目前的进展速度，我们也不知道他们对社会和环境影响的潜在程度。

# 威震天-图灵 NGL 530 b-另一个 GPT-3

几天前 Nvidia [在其博客中发布了](https://developer.nvidia.com/blog/using-deepspeed-and-megatron-to-train-megatron-turing-nlg-530b-the-worlds-largest-and-most-powerful-generative-language-model/)消息。它与微软联手带来了“世界上最大和最强大的生成语言模型。”威震天-图灵 NGL 530B (MT-NGL)现在是最大的密集神经网络(最大的稀疏神经网络仍然是[武道 2.0](/gpt-3-scared-you-meet-wu-dao-2-0-a-monster-of-1-75-trillion-parameters-832cd83db484) )，拥有 5300 亿个参数——比 GPT-3 大三倍——被创建来“推进自然语言生成的人工智能的艺术状态。”

根据这篇博文，MT-NGL 在零镜头、一镜头和少量镜头设置方面超过了 GPT-3——这表明了该模型在面临新任务之前看到的例子数量。它还提高了一些语言生成任务的准确性，如补全、阅读理解或常识推理。

为了训练如此庞大的模型，这些公司不得不寻找新的解决方案来解决老问题，即硬件-软件限制和与长训练时间相关的成本。他们将超级计算机英伟达塞勒涅和微软 Azure NDv4 的能力与最先进的人工智能训练软件结合起来——Megatron-ML 和 DeepSpeed 。

# 为什么我们一直在建造 LLM？

MT-NGL 是一个梦幻般的模型，但它听起来并不令人印象深刻，不是吗？GPT 3 号是一个新奇的东西，一个意想不到的突破。现在，我们已经厌倦了听到 LLM 打破了下一个障碍——当这个障碍只是向前迈出的一小步，却带来了一连串的后果。微软和 Nvidia [似乎担心](https://developer.nvidia.com/blog/using-deepspeed-and-megatron-to-train-megatron-turing-nlg-530b-the-worlds-largest-and-most-powerful-generative-language-model/#:~:text=Our%20observations%20with%20MT-NLG%20are%20that%20the%20model%20picks%20up%20stereotypes%20and%20biases%20from%20the%20data%20on%20which%20it%20is%20trained.%20Microsoft%20and%20NVIDIA%20are%20committed%20to%20working%20on%20addressing%20this%20problem.%20We%20encourage%20continued%20research%20to%20help%20in%20quantifying%20the%20bias%20of%20the%20model.)他们系统的潜在问题:

“我们对 MT-NLG 的观察是，该模型从其训练的数据中提取了刻板印象和偏见。微软和 NVIDIA 致力于解决这个问题。我们鼓励继续研究，以帮助量化模型的偏差。”那很好，但是他们把它当作下游要做的事情。他们强调的事实是，模型是最大和最强大的语言生成器。偏见可以事后解决。

这不仅仅是偏见。MT-NGL 的训练方式与 GPT-3 类似——从互联网上获取大量公开文本。后者被证明能够[传播误传](https://www.wired.com/story/ai-write-disinformation-dupe-human-readers/)。如果 MT-NGL 的语言专长更强，预计它制造假新闻的能力也会更强。此外，尽管软件和硬件都有所突破，但培训这些 LLM 的成本仍然非常高。微软和英伟达是大型科技公司，所以他们买得起，但其他公司的准入门槛不断提高。让我们不要忘记训练人工智能系统对环境造成的损害t。有倡议用可再生能源替代能源，但[这还不够](https://theconversation.com/it-takes-a-lot-of-energy-for-machines-to-learn-heres-why-ai-is-so-power-hungry-151825#:~:text=Unless%20we%20switch%20to%20100%25%20renewable%20energy%20sources%2C%20AI%20progress%20may%20stand%20at%20odds%20with%20the%20goals%20of%20cutting%20greenhouse%20emissions%20and%20slowing%20down%20climate%20change.)。

LLM 是目前照亮人工智能研究的明星，但人们开始更多地关注硬币的黑暗面。这些模型真的有必要吗？它们值得付出社会、环境和经济成本吗？如果有的话，技术的目的是什么？它们是通往 AGI 的最明智的途径吗？还是仅仅是我们仅有的一条途径？

不管后果如何，只有一个合理的动机来继续构建 LLMs 相信比例假说。这个假设说，如果我们继续训练越来越大的神经网络，这些网络被证明工作得相当好，更复杂的行为将会自然地出现这意味着没有必要在系统中硬编码复杂的认知功能，比如推理。因此，那些相信缩放假说的人在其中看到了一条通往人类级别的人工智能的直接路径和解决我们所有问题的方法。

信徒们的思路是这样的:“如果我们沿着伸缩假说的路径，我们将到达 AGI，它可以解决我们的大部分问题——如果不是全部的话。它会造成多种形式的损害(社会、环境、经济、政治)，但最终产生的好处将远远超过附带的后果。目的证明手段是正当的。"

然而，没有多少人工智能专家认为扩展神经网络是通往 AGI 的合理途径——至少在没有[理论](https://www.technologyreview.com/2021/03/03/1020247/artificial-intelligence-brain-neuroscience-jeff-hawkins/)、[算法](https://www.gwern.net/Scaling-hypothesis#:~:text=DeepMind21%20holds%20what%20we%20might%20call%20the%20%E2%80%9Cweak%20scaling%20hypothesis%E2%80%9D%3A%20they%20believe%20that%20AGI%20will%20require%20us%20to%20%E2%80%9Cfind%20the%20right%20algorithms%E2%80%9D)或[硬件](https://www.nature.com/articles/s41928-020-0457-1)突破的情况下。我们不需要盲目地走一条看起来(部分)可行的路，因为我们只有这条路。如果我们继续研究，我们可以找到一种方法来推动人工智能的进步，而不需要哀叹伤害——而不是像大技术公司那样认为请求原谅比获得许可更容易。

我们需要改变我们做一些事情的方式——比如直接在神经形态芯片中建立神经网络，而不是虚拟地创建它们。而且，更关键的是，我们需要改变我们在更普遍的意义上对待人工智能的方式——包容、安全、负责、可解释、有道德的人工智能才是重要的，即使它会减缓进展。正如 Peter Norvig [最近写的](https://hai.stanford.edu/news/peter-norvig-todays-most-pressing-questions-ai-are-human-centered)，“今天人工智能最紧迫的问题是以人为中心的。”

我们有一些人工智能系统工作得很好。让我们对此感到满意，暂时把注意力放在人类身上。最终，人工智能和我们所做的一切最终都是为了改善人类的整体福祉。还能为什么？

*如果你喜欢这篇文章，可以考虑订阅我的免费每周简讯*[](https://mindsoftomorrow.ck.page)**！每周都有关于人工智能的新闻、研究和见解！**

**您也可以直接支持我的工作，使用我的推荐链接* [*这里*](https://albertoromgar.medium.com/membership) *成为中级会员，获得无限权限！:)**