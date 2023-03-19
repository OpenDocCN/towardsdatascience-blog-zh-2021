# 五分钟了解 GPT-3

> 原文：<https://towardsdatascience.com/understanding-gpt-3-in-5-minutes-7fe35c3a1e52?source=collection_archive---------3----------------------->

## 人工智能

## 有数百篇关于 GPT 3 号的文章。这里有一个 5 分钟的关于它的所有信息的汇编。

![](img/2221264b09805c0ec47cac2436847d52.png)

[TT 先生](https://unsplash.com/@mrtt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

一个月前，我发表了[这篇长达 35 分钟的 GPT 3 号概述。但我很看重你作为读者的时间，所以我决定写一篇超浓缩的 5 分钟文章。我总结了这篇较长文章的主要观点:GPT-3 是什么，它能做什么，以及它现在和未来对世界的影响。尽情享受吧！](/gpt-3-a-complete-overview-190232eb25fd)

# GPT 3 号是什么

[GPT-3](https://arxiv.org/abs/2005.14165) 是 OpenAI 的生成式预训练模型家族的第三个版本。 [GPT-1](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) 和 [GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) 为 GPT-3 奠定了基础，证明了两个关键假设的成功:变形金刚+无监督预训练工作正常(GPT-1)和语言模型可以多任务处理(GPT-2)。

GPT-3 是一种基于 transformer 架构的语言模型，以一种生成式、无监督的方式进行预训练，在零/一/少量多任务设置中显示出良好的性能。它的工作原理是在给定一系列标记的情况下预测下一个标记，并且可以对尚未训练过的 NLP 任务进行预测。在看过几个例子后，它在一些基准测试中达到了最先进的水平，如机器翻译、问答和完形填空任务。

GPT-3 是用巨大的互联网文本数据集训练的——总共 570GB。当它发布时，它是最大的神经网络，有 1750 亿个参数(100 倍 GPT-2)。即使在今天，它也是最大的*密集*神经网络，仅次于像[开关变压器](https://arxiv.org/abs/2101.03961)或[悟道 2.0](/gpt-3-scared-you-meet-wu-dao-2-0-a-monster-of-1-75-trillion-parameters-832cd83db484) 这样的稀疏模型。

GPT-3 最令人印象深刻的特点是它是一个元学习者；它学会了学习。你可以用自然语言要求它执行一项新任务，它“理解”它必须做什么，就像人类一样(保持距离)。

# GPT 3 号能做什么

在论文中，OpenAI 的研究人员使用标准基准将 GPT-3 与之前的模型进行了比较。它表现出比以前的类似系统更好的性能，但对于一些任务来说，受监督的系统——为特定任务而训练——要好得多。与监督系统相比，GPT-3 的主要贡献是普及了一条通往 AGI 的新的有前途的道路。

除了枯燥的基准测试，OpenAI 还提供了另一种测试 GPT 3 语言技能的方法。他们发布了[测试版 API](https://beta.openai.com/) ，鼓励开发者寻找新的令人兴奋的用例。该 API 的工作原理是向基线 GPT-3 输入文本，即所谓的提示，使其专注于特定的任务。如果你输入:*这个女人正在遛狗→ La mujer está paseando a su perro。孩子在公园里玩耍→ ____* ，GPT-3 会知道你在要求英语-西班牙语翻译，并将能够做到这一点。

GPT-3 的这个特殊版本将不同于任何其他用户的 GPT-3。那就是提示+元学习的力量；在不改变原始模型的情况下，用户可以让 GPT-3 成为不同任务的专家。基线 GPT-3 不知道如何执行任何任务，它知道如何学习去做，这使它更加强大和多才多艺。

这里有一个 GPT-3 能做什么的列表，还有链接(特别提到 [Gwern Branwen](https://www.gwern.net/GPT-3) ，他做了一个很棒的例子汇编):

*   **非小说类:** [对话](https://www.gwern.net/GPT-3#dialogue)，[模仿类](https://arr.am/2020/08/17/ai-tim-ferriss-interviews-ai-marcus-aurelius-gpt-3/)，[杂文类](https://adolos.substack.com/p/feeling-unproductive-maybe-you-should)，[新闻类文章](https://arxiv.org/abs/2005.14165)，[剧情总结](https://www.gwern.net/GPT-3-nonfiction#moviebook-plot-summaries)，[推文类](https://thoughts.sushant-kumar.com/)，[教学类](https://twitter.com/yehoshzl/status/1288180481960415232?ref=gptcrushdemosofopenaisgpt)。
*   **专业:** [广告](https://www.trypencil.com/)，[邮件](https://www.flowrite.com/)，[文案](https://copysmith.ai/)， [CV 生成](https://urspace.io/)，[团队管理](https://www.thinkconfluent.com/)，[内容营销](https://www.usebroca.com/)，[记笔记](https://totallib.com/)。
*   **代码:** [Python](https://www.youtube.com/watch?v=LVOqlz_vSeo) ， [SQL](https://twitter.com/FaraazNishtar/status/1285934622891667457) ， [JSX](https://twitter.com/sharifshameem/status/1282676454690451457) ， [React app](https://twitter.com/sharifshameem/status/1284421499915403264?ref=gptcrushdemosofopenaisgpt) ， [Figma](https://twitter.com/jsngr/status/1284511080715362304) ， [javascript](https://twitter.com/Antonio_GomezM/status/1287969287110443008) ， [CSS](https://twitter.com/zoltanszogyenyi/status/1286349416530620416) ， [HTML](https://twitter.com/Bryandsouza90/status/1284827138277941251?ref=gptcrushdemosofopenaisgpt) ， [LaTeX](https://twitter.com/sh_reya/status/1284746918959239168)
*   **创意:** [小说](https://gpt3.substack.com/)，[诗歌](https://www.gwern.net/GPT-3#poetry)，[歌曲](https://twitter.com/arram/status/1281259921892237312?lang=es)，[幽默](https://www.gwern.net/GPT-3#humor)，[网游](https://play.aidungeon.io/main/landing)，[桌游](https://www.gwern.net/GPT-3-nonfiction#board-games)，[迷因](https://twitter.com/wowitsmrinal/status/1287175391040290816?ref=gptcrushdemosofopenaisgpt)，[烹饪菜谱](https://twitter.com/notsleepingturk/status/1286112191083696128)，[吉他标签](https://twitter.com/AmandaAskell/status/1283900372281511937)，[写出你独特的风格](https://www.shortlyai.com/)。
*   **理性技巧:** [逻辑](https://www.gwern.net/GPT-3-nonfiction#logic)，[不确定性](https://www.gwern.net/GPT-3-nonfiction#expressing-uncertainty)，[常识](https://www.gwern.net/GPT-3-nonfiction#common-sense-knowledge)，[类比](https://medium.com/@melaniemitchell.me/can-gpt-3-make-analogies-16436605c446)，[概念交融](https://www.gwern.net/GPT-3-nonfiction#concept-blending)，[计数](https://www.gwern.net/GPT-3-nonfiction#verbal-counting)，[字谜](https://www.gwern.net/GPT-3-nonfiction#anagrams)，[预测](https://arr.am/2020/08/08/gpt-3-predicts-the-rest-of-2020/)。
*   **哲学:** [人生意义](https://messagink.com/story/5f14b5c8de14c8a40bd5a9e5/meaning-of-life-by-open-ai-gpt-3)，[数字 42](https://muellerberndt.medium.com/i-asked-gpt-3-for-the-question-to-42-i-didnt-like-its-answer-and-neither-will-you-33f425a4d60f) ，[回应哲学家](https://dailynous.com/2020/07/30/philosophers-gpt-3/#gpt3replies)。

# GPT 3 号现在和将来对世界的影响

## 疯狂炒作

GPT-3 从根本上改变了工业和学术界的人工智能格局，人们为之疯狂。一些人将人类特征归因于该系统，另一些人在此基础上建立产品和公司，它成为各地的头条新闻，一批批研究人员开始构建类似的人工智能。以下是一些炒作的例子:

*   **归因:**人们说 GPT-3 是"[有自我意识的](https://twitter.com/sonyasupposedly/status/1284188369631629312?s=20)，"一个"[一般智力的](https://twitter.com/rauchg/status/1282449154107600897?s=20)"(或者至少是一个"[无脑路径的](https://dailynous.com/2020/07/30/philosophers-gpt-3/#chalmers)"对它来说)，并且能够"[理解【和】推理](https://www.lesswrong.com/posts/L5JSMZQvkBAx9MD5A/to-what-extent-is-gpt-3-capable-of-reasoning#eq6FTwG2yWuBdPofs)"
*   **杂志:**上了[纽约时报](https://www.nytimes.com/2020/11/24/science/artificial-intelligence-ai-gpt3.html)，[福布斯](https://www.forbes.com/sites/bernardmarr/2020/10/05/what-is-gpt-3-and-why-is-it-revolutionizing-artificial-intelligence/)，[麻省理工科技评论](https://www.technologyreview.com/2020/07/20/1005454/openai-machine-learning-language-generator-gpt-3-nlp/)，[连线](https://www.wired.com/story/ai-text-generator-gpt-3-learning-language-fitfully/)，[卫报](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)，[财富](https://fortune.com/2020/06/11/openai-artificial-intelligence-commercial-product/)， [The Verge](https://www.theverge.com/21346343/gpt-3-explainer-openai-examples-errors-agi-potential) ，[科技危机](https://techcrunch.com/2021/03/17/okay-the-gpt-3-hype-seems-pretty-reasonable/)，[数字趋势](https://www.digitaltrends.com/features/openai-gpt-3-text-generation-ai/)。
*   **创业公司:** [可行的](https://askviable.com/)，[寓言工作室](https://fable-studio.com/)，[阿哥利亚](https://www.algolia.com/)，[复制匠](https://copysmith.ai/)，[纬度](https://latitude.io/)，[他方](https://www.othersideai.com/)，[调试](https://debuild.co/)，[热兹](https://www.rezi.ai/)，[总库](https://totallib.com/)，[布罗卡](https://www.usebroca.com/)，[认为合流](https://www.thinkconfluent.com/)
*   **AI 后继者:** [开关变压器](https://arxiv.org/abs/2101.03961)， [DALL E](https://openai.com/blog/dall-e/) ，[夹子](https://openai.com/blog/clip/)， [UC](https://arxiv.org/pdf/2104.00332.pdf) ， [LaMDA](/googles-lamda-the-next-generation-of-chatbots-62294be58426) ， [MUM](/will-googles-mum-kill-seo-d283927f0fde) ，[武道 2.0](/gpt-3-scared-you-meet-wu-dao-2-0-a-monster-of-1-75-trillion-parameters-832cd83db484) 。

## 潜在的危险

GPT-3 是一个令人惊叹的人工智能，但像任何其他强大的技术一样，可以被恶意使用。这是它造成伤害的许多方式中的一些:

*   **偏见:** OpenAI 发现 GPT-3 存在种族、性别和宗教偏见，这可能反映了训练数据中的偏见，因此该系统受到了[强烈批评](https://twitter.com/an_open_mind/status/1284487376312709120)。OpenAI 最近发表了 [PALMS](/openai-palms-adapting-gpt-3-to-society-49c16ae5e039) ，这是一种使用小型精选数据集来减少 GPT-3 偏差的方法。
*   **假新闻:**因为 GPT-3 写得太好了，它可以写出可以被当做人造的虚假文章，正如博主[利亚姆·波尔](https://adolos.substack.com/p/feeling-unproductive-maybe-you-should)和[卫报](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)所证明的那样。OpenAI 在论文中强调，人类法官只能识别 52%的 GPT-3 文章，略高于纯粹的概率。
*   **环境成本:**训练 GPT 3 号产生的碳足迹与“[驾驶汽车往返月球](https://www.theregister.com/2020/11/04/gpt3_carbon_footprint_estimate/)产生的碳足迹大致相同“越大越好”的趋势只有在环境没有受到威胁的情况下才会继续。
*   无用的信息:GPT 3 号不对它吐出的文字负责。这些低质量的数据在互联网上积累，用哲学家香农·瓦勒的话说，“人们越来越无法使用，甚至有害。”
*   工作损失:像 GPT-3 [这样的系统威胁到](/ai-will-impact-computer-based-jobs-first-and-hard-11af4d47487)基于计算机的、非常规的认知工作，甚至比机器人威胁蓝领工作更甚。[目前的报告](https://futurism.com/the-automation-upheaval-wont-be-limited-to-blue-collar-jobs)估计大约 40-50%的工作会在 15-20 年内被取代。

## 批评和辩论

*   在疯狂的宣传之后，人们开始寻找 GPT-3 的弱点。发现系统缺少[逻辑](https://lacker.io/ai/2020/07/06/giving-gpt-3-a-turing-test.html)、[常识、](https://twitter.com/hc_mty/status/1284154884074483713)和[文本理解](https://twitter.com/yoavgo/status/1284557072118550532)。加里·马库斯写了一篇对这一体系的很有说服力的评论:“[GPT-3]对世界的理解经常严重偏离，这意味着你永远不能真正相信它所说的。”
*   **反批评:**格温根据一个前提对大多数批评者提出了很好的论点:“抽样可以证明知识的存在，但不能证明知识的缺失。”他发现大多数 GPT 3 号的失败是由于人们有缺陷的提示。
*   **实践与哲学的争论:**实践的争论关系到 GPT-3 的局限性:我们无法确定它什么时候会正确回答，什么时候不会，这使得它不可靠。哲学辩论涉及通往 AGI 的道路:GPT-3——或任何其他基于计算机的人工智能——可能永远无法到达 AGI，除非它们“T22 长大，属于一种文化，并在世界上行动 T23。”

如果你想了解更多关于 GPT 3 号的信息，我推荐你阅读完整的概述

</gpt-3-a-complete-overview-190232eb25fd> [## GPT-3 —全面概述

towardsdatascience.com](/gpt-3-a-complete-overview-190232eb25fd) 

**其他推荐阅读**

</4-things-gpt-4-will-improve-from-gpt-3-2b1e7a6da49f>  

[***跟我一起旅行到未来***](https://mindsoftomorrow.ck.page/) ***了解更多关于人工智能、哲学和认知科学的内容！如有任何疑问，欢迎在评论中提问或联系***[***LinkedIn***](https://www.linkedin.com/in/alberromgar/)***或***[***Twitter***](https://twitter.com/Alber_RomGar)***！:)***