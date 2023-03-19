# 现在是自然语言处理的黄金时代，为什么聊天机器人不能解决更多的问题？

> 原文：<https://towardsdatascience.com/its-the-golden-age-of-natural-language-processing-so-why-can-t-chatbots-solve-more-problems-bada37a427cf?source=collection_archive---------21----------------------->

## [行业笔记](https://towardsdatascience.com/tagged/notes-from-industry)

## 自然语言处理(NLP)正在彻底改变我们的生活方式以及我们与机器和人之间的交互方式。但是聊天机器人仍然存在不足。原因如下。

![](img/b73879b082d66d3df647c09cf765aff3.png)

图片来自 [Pixabay](https://pixabay.com/illustrations/chatbot-chat-application-artificial-3589528/)

人工智能和机器学习(ML)领域的进步，特别是自然语言处理(NLP)，正在[彻底改变我们与机器和彼此互动的方式](https://loris.ai/blog/human-vs-digital-customer-service-how-to-find-the-right-balance/)。亚马逊的 Alexa，苹果的 Siri，虚拟助手和聊天机器人改变了我们请求和接收信息的方式。NLP 管道获取非结构化文本数据，并将其处理为结构化格式，以提取信息、分类并获得洞察力。它意味着计算机可以将人类语言转换成一种可以为 AI 系统精简和处理的形式。最终，它们让人类和机器之间的交流变得更加容易。

**工作原理**

计算机擅长各种自然语言任务，如文本分类、[语音到文本](https://github.com/syhw/wer_are_we)、[语法纠正](http://nlpprogress.com/english/grammatical_error_correction.html)和大规模分析。ML 算法已经被用来帮助在特定问题上取得重大进展，例如面向任务的聊天机器人的[翻译](http://nlpprogress.com/english/machine_translation.html)、[文本摘要](http://nlpprogress.com/english/summarization.html)、[问答](http://nlpprogress.com/english/question_answering.html)系统和[意图检测和槽填充](http://nlpprogress.com/english/intent_detection_slot_filling.html)。

例如，你的文字处理器中内置的语法插件，以及你在开车时用来发送文本的语音笔记应用程序，都要归功于机器学习和自然语言处理。

然而，尽管这些机器人看起来很聪明，但人类现在更聪明。归根结底，人类的语言是微妙的，而且常常是模棱两可的。这对 ML 系统提出了严峻的挑战。在大规模数据集上进行训练——例如:维基百科的每一篇文章——并创建大型语言模型不会导致对语言的理解。

事实上，这种方法可以延续人类的错误和误解。

尽管使用广泛，但仍然不清楚依赖语言模型的应用程序，如生殖聊天机器人，是否可以在没有人类监督的情况下安全有效地释放到野外。还记得 2014 年的电影《T2》前玛奇纳吗？它可能不是那么极端，但这些系统的后果和考虑应该认真对待。

**自然语言处理的最新进展导致了创新的爆发**

就在过去的十年里，技术发生了巨大的变化，并影响着客户支持生态系统。随之而来的是一个有趣的机会，可以在客户体验(CX)过程中增强和帮助人们——使用最新模型中的见解来帮助指导客户对话。

自然语言处理突破和语言模型爆炸的主要驱动力包括:

1.  新技术和算法的开发(单词嵌入和[转换器](https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04))。
2.  对高端硬件(GPU 和[TPU](https://en.wikipedia.org/wiki/Tensor_Processing_Unit))的改进和更多访问。
3.  开源工具和库——如 [SpaCy](https://spacy.io/) 、[humping Face](https://huggingface.co/)和[Rasa](https://rasa.com/)——已经使自然语言处理民主化。

变形金刚，或者说[基于注意力的模型](https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04)，已经在自然语言基准上产生了更高性能的模型，并迅速淹没了这个领域。利用语言模型的文本分类器、摘要器和信息提取器已经超越了先前的[最先进的结果](http://nlpprogress.com/)。高端硬件的更高可用性也允许更快的训练和迭代。开源库及其支持生态系统的发展使从业者能够接触到前沿技术，并允许他们快速创建基于该技术的系统。

这些进步导致了语言模型的雪崩，这些语言模型有能力预测序列中的单词。想想[谷歌的自动填充](https://blog.google/products/search/how-google-autocomplete-predictions-work/)。可以预测序列中下一个单词的模型可以由机器学习实践者进行微调，以执行一系列其他任务。

OpenAI 的 GPT-3——[是一种可以自动编写文本的语言模型](https://venturebeat.com/2021/11/18/openai-makes-gpt-3-generally-available-through-its-api/)——在过去的一年里受到了大量的炒作。北京人工智能研究院的[无刀 2.0](/gpt-3-scared-you-meet-wu-dao-2-0-a-monster-of-1-75-trillion-parameters-832cd83db484) (一种多模态人工智能系统)和谷歌的[开关变压器](https://arxiv.org/abs/2101.03961)都被认为是更强大的模型，包括超过 1.6 万亿个参数，使 GPT-3 微不足道的 1750 亿个参数相形见绌。新的、更大的语言模型以惊人的速度发布。说到人工智能系统，市场上并不缺乏。

**那么为什么聊天机器人没有更好呢？**

> **虽然这些语言模型和利用它们的系统变得更加复杂和强大，但问题仍然存在:为什么这些** [**技术仍然如此令人沮丧并且经常出错**](https://venturebeat.com/2021/09/20/falsehoods-more-likely-with-large-language-models/) **？**

除了令人沮丧的与 Alexa 的交互，一些人工智能系统的错误可能会产生严重的后果。如图 1 所示，在大规模数据集上训练的大型语言模型可能会延续人类的错误和误解:

![](img/bac22696faf2ffc6260e8ab6d856786a.png)

图 1: [TruthfulQA](https://arxiv.org/pdf/2109.07958.pdf) 来自 GPT-3–175 b 的带有默认提示的问题和答案。例子说明了 GPT-3 的错误答案，模仿人类的谎言和误解。更多信息，请查看完整的研究: [TruthfulQA:测量模型如何模仿人类的谎言](https://arxiv.org/pdf/2109.07958.pdf)

答案比你想象的简单:自然语言处理不是自然语言理解。无论计算的复杂性、能源和时间如何致力于创建更大的语言模型，这种方法都不会导致获得意义、把握上下文或理解的能力。要深入了解这一点的更多技术解释，请参考[机器学习不会解决自然语言理解](https://thegradient.pub/machine-learning-wont-solve-the-natural-language-understanding-challenge/)。这是对 NLP 缺点的一个大开眼界的观察。

这对公司、客户服务和聊天机器人意味着什么？

那么这对公司，尤其是那些严重依赖聊天机器人的公司有什么影响呢？很复杂。

自然语言处理的进步导致人们高度期待聊天机器人能够帮助转移和处理过多的客户问题。公司[加快了其数字业务](https://www.gartner.com/smarterwithgartner/why-now-is-the-time-to-accelerate-digital)的发展，将[聊天机器人纳入其客户支持](https://www.tidio.com/blog/chatbot-statistics/#customer-experience-expectations)堆栈。

对一些人来说，是的，聊天机器人可以成为他们 CX 解决方案的一个可行部分。然而，对于大多数人来说，聊天机器人并不是客户服务解决方案的一站式商店。此外，它们甚至会制造自己的盲点和新问题。虽然聊天机器人现在无处不在，但根据技术公司 Tidio 的研究，大约一半的用户仍然喜欢与真人交流，而不是聊天机器人。

在一个日益数字化、自动化和虚拟化的世界中，当客户遇到问题时，他们只是希望由真人来迅速、恰当地解决问题。聊天机器人[供应商可以希望只解决*大约 50%的顾客询问*](https://www.forbes.com/sites/forbestechcouncil/2021/03/04/how-cognitive-virtual-agents-can-revolutionize-the-customer-support-industry/?sh=362c8e8138dd)。虽然聊天机器人有可能减少简单的问题，但仍然有一部分对话需要人工代理的帮助。

![](img/3dd4582fa7a358611a4b4aacad5e5352.png)

作者图片

无法使用聊天机器人解决问题的沮丧客户可能会觉得公司不想处理他们的问题。他们可能会因为自己的经历而感到不满足，也不会被视为客户。对于那些真正致力于自助服务门户并浏览常见问题的人来说，当他们联系到人时，客户通常会更加沮丧。更不用说收集到的信息之间的差距了——例如，一个聊天机器人收集客户信息，然后一个 CX 代表请求相同的信息。在这些时候，代理对这些潜在的有争议的谈话准备得越充分(他们掌握的信息越多)，对客户和代理都越有利。

尽管一些公司押注于完全数字化和自动化的解决方案，但聊天机器人还没有出现在开放域聊天中。如果不检查生成模型，则[会导致有害问题](https://www.vox.com/future-perfect/22672414/ai-artificial-intelligence-gpt-3-bias-muslim)。

坦率地说，聊天机器人无法处理人类查询的多样性和细微差别。在最好的情况下，聊天机器人有能力将未解决的、通常是最复杂的问题交给人工代理。但这可能会引发问题，给 CX 特工带来一连串需要处理的问题，增加他们的工作负担。

**那么，我们是否为人类配备了处理客户问题的最佳工具和支持？**

好消息是 NLP 的进步不必完全自动化和孤立使用。在洛里斯，我们相信来自我们最新模型的洞察力可以用来帮助引导对话和增强人类交流。理解人类和机器如何合作创造最佳体验将会带来有意义的进步。从我们的模型中获得的见解可以用来帮助指导对话，并帮助而不是取代人类交流。

我们的软件利用这些新技术，用于更好地装备代理，以处理最困难的问题——机器人无法单独解决的问题。我们努力通过向用户学习来不断改进我们的系统，以开发更好的技术。

通过实时预测客户满意度和意向，我们使代理能够有效、恰当地处理客户问题。我们的软件可以实时指导代理人做出回应，并简化死记硬背的任务，因此他们有更多的空间来解决最困难的问题，并专注于为客户提供价值。在客户支持职位的人员流动率达到历史最高水平的时候，这一点尤其令人心酸。

Paul R. Daugherty 在他的书《人类+机器:人工智能时代的重新想象》中解释道，

> “简单的事实是，当人类和机器作为盟友一起工作时，公司可以实现业绩的最大提升……以便利用彼此的互补优势。”

![](img/992ad0730c93b1ffcb9598393ca0d6fd.png)

图片来自 [Pixabay](https://pixabay.com/photos/technology-developer-touch-finger-3389904/?download)

虽然语言建模、机器学习和人工智能已经取得了很大的进步，但在处理人类问题的复杂性方面，这些技术仍处于起步阶段。正因为如此，聊天机器人不能放任自流，仍然需要人类的支持。技术驱动的人类可以也应该帮助驱动和引导对话系统，帮助他们随着时间的推移而学习和改进。意识到并实现人类和技术之间平衡的公司将主导客户支持，推动未来更好的对话和体验。

有兴趣了解更多信息吗？

*   联系 LinkedIn 上的[我](https://www.linkedin.com/in/sethplevine/)

[](https://www.linkedin.com/in/sethplevine/) [## Seth Levine -高级机器学习科学家- Loris.ai | LinkedIn

### 数据科学家，ML 研究员，网页设计师，企业家。目前的工作集中在自然语言处理上

www.linkedin.com](https://www.linkedin.com/in/sethplevine/) 

*   我们在招人！查看我们的[开放数据科技角色](https://loris.ai/careers/)

[](https://loris.ai/) [## 洛里斯-面向客户至上团队的对话式人工智能

### 对话式人工智能和洞察力可以在几周内提高 CX 代理商的工作效率并改善客户对话。

loris.ai](https://loris.ai/) 

**资源、参考资料和进一步阅读**

*   [博玛萨尼](https://arxiv.org/search/cs?searchtype=author&query=Bommasani%2C+R)，r .等人，[论基金会模式的机遇与风险。](https://arxiv.org/abs/2108.07258) arXiv 预印本 2021。
*   布伊格，m .，[认知虚拟代理人如何革新客户支持行业](https://www.forbes.com/sites/forbestechcouncil/2021/03/04/how-cognitive-virtual-agents-can-revolutionize-the-customer-support-industry/?sh=6109d6f238dd)。2021 年 3 月 4 日。福布斯。
*   Daugherty，p .，[人类+机器:重新想象人工智能时代的工作](https://amzn.to/3E74ah4)。2018 年 3 月 20 日。哈佛商业评论出版社。
*   Fedus，w .，[开关变压器:通过简单有效的稀疏性扩展到万亿参数模型](https://arxiv.org/abs/2101.03961)。2021 年 1 月 11 日。arXiv 预印本。
*   人类与数字客户服务:如何找到正确的平衡？。2021 年 7 月 12 日。Loris.ai 博客。
*   为什么现在是加速数字化的时候了。2020 年 9 月 11 日。高德纳。
*   拥抱脸。[https://huggingface.co/](https://huggingface.co/)
*   为什么没人愿意在客户服务部工作。2021 年 11 月 10 日。LinkedIn Pulse。
*   林等， [TruthfulQA:测量模型如何模仿人类的错误](https://arxiv.org/pdf/2109.07958.pdf)。2021 年 9 月 8 日。arXiv 预印本。
*   马克西姆。[机器学习的变形金刚和序列对序列学习介绍](https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04)。2019 年 1 月 4 日。中等。
*   拉莎。[https://rasa.com/](https://rasa.com/)
*   罗梅洛，a .， [GPT-3 吓到你了？遇见武道 2.0:1.75 万亿参数的怪物](/gpt-3-scared-you-meet-wu-dao-2-0-a-monster-of-1-75-trillion-parameters-832cd83db484)。2021 年 6 月 5 日。迈向数据科学，中等。
*   跟踪自然语言处理的进展。[http://nlpprogress.com/](http://nlpprogress.com/)。
*   萨缪尔·s .[艾的伊斯兰恐惧症问题](https://www.vox.com/future-perfect/22672414/ai-artificial-intelligence-gpt-3-bias-muslim)。2021 年 9 月 18 日。Vox。
*   2021 年你需要知道的 11 个聊天机器人统计数据和趋势。2021 年 12 月 6 日。蒂迪奥。
*   斯帕西。[https://spacy.io/](https://spacy.io/)
*   Sullivan d .，[谷歌自动完成预测是如何产生的](https://blog.google/products/search/how-google-autocomplete-predictions-work/)。2020 年 10 月 8 日。谷歌博客。
*   Tavva，r .，[自然语言处理管道，解释。](https://www.kdnuggets.com/2021/03/natural-language-processing-pipelines-explained.html) KDNuggets。
*   你所需要的只是关注。第 31 届神经信息处理系统会议(NIPS 2017)，美国加州长滩。
*   Walid S .，[机器学习不会解决自然语言理解](https://thegradient.pub/machine-learning-wont-solve-the-natural-language-understanding-challenge/)。2021 年 8 月 7 日。渐变。
*   Wiggers，k .，[大型语言模型更可能出现错误](https://venturebeat.com/2021/09/20/falsehoods-more-likely-with-large-language-models/)。创业节拍。2021 年 9 月 20 日。
*   OpenAI 通过其 API 使 GPT-3 普遍可用。2021 年 11 月 18 日。