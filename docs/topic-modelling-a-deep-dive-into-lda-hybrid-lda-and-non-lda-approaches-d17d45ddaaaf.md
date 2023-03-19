# 主题建模:深入研究 LDA、混合 LDA 和非 LDA 方法

> 原文：<https://towardsdatascience.com/topic-modelling-a-deep-dive-into-lda-hybrid-lda-and-non-lda-approaches-d17d45ddaaaf?source=collection_archive---------28----------------------->

## 在你写一行代码之前，你需要知道的是

![](img/de4a16b4930d6213d243bff284224b96.png)

由 [Raphael Schaller](https://unsplash.com/@raphaelphotoch?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

主题建模是我发现在我的角色中最需要的自然语言处理技术之一。最近，我看到它作为实体提取而流行起来，然而，它的目的仍然是一样的——在文本语料库中进行模式识别。

在本文中，我将对可用于对短格式文本进行主题建模的技术进行深入的回顾。短格式文本通常是用户生成的，由缺乏结构、存在噪声和缺乏上下文来定义，导致机器学习建模困难。

这篇文章是我的[情感分析深度探索](/sentiment-analysis-a-deep-dive-into-the-theory-methods-and-applications-322f984f2b48)的第二部分，是我做的关于主题建模和情感分析的[系统文献综述的结果。](https://local.cis.strath.ac.uk/wp/extras/msctheses/papers/strath_cis_publication_2733.pdf)

# 概观

主题建模是一种[文本处理技术](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf?TB_iframe=true&width=370.8&height=658.8)，旨在通过找出并展示文本数据中被识别为主题的模式来克服信息过载。它实现了[改进的用户体验](https://projecteuclid.org/journals/annals-of-applied-statistics/volume-1/issue-1/A-correlated-topic-model-of-Science/10.1214/07-AOAS114.full)，允许分析师在已识别主题的引导下快速浏览文本语料库或集合。

主题建模[通常通过无监督学习](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0167404817300603&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=16150203952534417719&ei=pUTcYPbkKtyLy9YPx5K8qAM&scisig=AAGBfm3pKQ2rYCPj7vq25qyjN6b2jla1ew)进行，运行模型的输出是所发现主题的概述。

检测话题可以在[在线和离线模式](http://ceur-ws.org/Vol-2311/paper_4.pdf)下完成。当在线完成时，它的目标是随着时间的推移发现出现的动态话题。离线时，它是回顾性的，将语料库中的文档视为一批，一次检测一个主题。

主题检测和建模有四种主要方法:

*   基于键盘的方法
*   概率主题建模
*   衰老理论
*   基于图表的方法。

方法也可以通过用于主题识别的技术进行[分类，这产生了三个组:](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0957417416301038&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=5259186730874098934&ei=MUvcYLudDtyDy9YPheGKmAE&scisig=AAGBfm0_96cGJAj0XjYeSimNrGRqQJybMw)

*   使聚集
*   分类
*   概率技术。

# 1.潜在狄利克雷分配

LDA(潜在狄利克雷分配)是一个[贝叶斯分层概率生成模型](https://scholar.google.com/scholar_url?url=https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf%3FTB_iframe%3Dtrue%26width%3D370.8%26height%3D658.8&hl=en&sa=T&oi=gsb-ggp&ct=res&cd=0&d=17756175773309118945&ei=C03cYNimGYjCmwG3s6r4Cg&scisig=AAGBfm0y2e28PKlP3viZcQEbvTwTTHlHJQ)，用于收集离散数据。它基于文档中单词和主题的可交换性假设进行操作。它[将](https://scholar.google.com/scholar_url?url=http://proceedings.mlr.press/v84/wang18a.html&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=260075222128407753&ei=ME3cYLPSHNC5mAHv2o2QBA&scisig=AAGBfm2rHNvY0l9wYMjz3m9c5TjygJkazA)文档建模为主题的离散分布，后面的主题被视为文档中术语的离散分布。

最初的 LDA 方法使用变分期望最大化(VEM)算法来为 LDA 推断主题。后来[基于 Gibbs 抽样的随机抽样推断被引入](https://scholar.google.com/scholar_url?url=http://proceedings.mlr.press/v84/wang18a.html&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=260075222128407753&ei=XU3cYPnGHIOHy9YPvfyVwAo&scisig=AAGBfm2rHNvY0l9wYMjz3m9c5TjygJkazA)。这改进了实验中的性能，并且已经被更频繁地用作模型的一部分。

首次引入 LDA 的 Blei 和他们的同事们展示了它相对于概率 LSI 模型的优越性。 [LSI(潜在语义索引)使用线性代数和单词袋表示法](https://scholar.google.com/scholar_url?url=https://www.taylorfrancis.com/chapters/edit/10.4324/9780203936399-10/meaning-context-walter-kintsch&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=4005447354759878017&ei=zU_cYN_KKcvhmQG794zoBg&scisig=AAGBfm23vQNpOLti-d4vqgSfwgrxzMPZhA)来提取意思相似的单词。

## LDA 的好处——LDA 有什么好处？

1.  **战略业务优化**

在所有审查的技术中，LDA 最常被列为模型的一部分，并被认为对战略业务优化有价值。

**2。通过更好地理解用户生成的文本来提高竞争优势。**

一项 [2018 研究](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/8614146/&hl=en&sa=T&oi=gsb&ct=res&cd=2&d=15741880252452956632&ei=qVDcYPSRMsvhmQG794zoBg&scisig=AAGBfm3wGYF00Q3FDtK9ngeI5Az62L9BhQ)证明了 LDA 作为一种通过从用户在线评论中提取信息并随后根据情感对主题进行分类来提高公司竞争优势的方法的价值。

**3。提高公司对用户的了解。**

基于 LDA 的主题建模也被用于[描述用户的个性特征](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S092523121630604X&hl=en&sa=T&oi=gsb&ct=res&cd=2&d=8328834636591876070&ei=F1HcYMe-K8vhmQG794zoBg&scisig=AAGBfm1bCO2t0aWzokbN5pKeC5LZJBCsQA)，基于他们的在线文本出版物。

在[我自己的研究](https://local.cis.strath.ac.uk/wp/extras/msctheses/papers/strath_cis_publication_2733.pdf)中，我使用 LDA 主题建模，根据用户在社交媒体上发布的与产品或公司相关的简短的用户生成文本，在用户的客户旅程阶段对用户进行分类。

**4。了解客户投诉，提高客户服务效率。**

在一项 [2019 年的研究中，](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S095741741930154X&hl=en&sa=T&oi=gsb&ct=res&cd=2&d=5697469705519407606&ei=fVTcYJmkOIjCmwG3s6r4Cg&scisig=AAGBfm0X_vugU_Z6Cteo2qvVA9x3RiWg_g) LDA 主题建模用于分析消费者金融保护局的消费者投诉。预先确定的标签用于分类，这通过任务自动化提高了投诉处理部门的效率。

## LDA 的局限性——LDA 被批评了什么？

1.  **无力伸缩。**

LDA 因其所基于的技术的线性而被批评为不能缩放。

其他变体，如 pLSI，LSI 的概率变体，[通过使用统计基础](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0957417416301464&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=17965918767627044979&ei=zFfcYKzrJK3CsQKW_J7gDA&scisig=AAGBfm2beLSNGietxuQH1UH6E7UMSadMOw)和生成数据模型来解决这一挑战。

**2。假设文件可交换性**

虽然 LDA 很有效并且经常使用，但是它对文档可交换性的假设受到了批评。在主题随时间演变的情况下，这可能是限制性的。

**3。通常忽略共现关系。**

基于 LDA 的模型因通常忽略所分析文档之间的共现关系而受到批评[。这导致检测到不完整的信息，并且无法通过上下文或其他桥接术语发现潜在的共现关系。](https://scholar.google.com/scholar_url?url=https://www.researchgate.net/profile/Solomia-Fedushko/publication/331276764_Proceedings_of_the_Sixth_International_Conference_on_Computer_Science_Engineering_and_Information_Technology_CCSEIT_2016_Vienna_Austria_May_2122_2016/links/5c6fcd63299bf1268d1bc2b0/Proceedings-of-the-Sixth-International-Conference-on-Computer-Science-Engineering-and-Information-Technology-CCSEIT-2016-Vienna-Austria-May-2122-2016.pdf%23page%3D212&hl=en&sa=T&oi=gsb-ggp&ct=res&cd=0&d=8926787121599265886&ei=PlncYLGBAa3CsQKW_J7gDA&scisig=AAGBfm0h1_4gOYZMJNFjwqf9dIblCE5zBg)

为什么这很重要？这可以防止重要但罕见的主题被检测到。

这种批评也出现在对后来一项研究的分析中，作者提出了一个专门为在线社交网络主题建模定制的模型。他们证明，与 LDA 相比，即使应用于神经嵌入特征表示的浅层机器学习聚类技术也能提供更有效的性能。

**4。不适合用户生成的简短文本。**

[Hajjem 和 Latiri (2017)](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S1877050917315235&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=656547531701379292&ei=t1ncYOLQA9yDy9YPheGKmAE&scisig=AAGBfm2XoXN1TusTewLlFSI1hF8AIFW3Vg) 批评 LDA 方法不适合短格式文本。他们提出了一个混合模型，该模型利用了信息检索领域的典型机制。

[LDA-混合方法也被提出](https://scholar.google.com/scholar_url?url=https://link.springer.com/content/pdf/10.1007/978-3-319-50496-4_59.pdf&hl=en&sa=T&oi=gsb&ct=res&cd=1&d=12584422342234601625&ei=5lncYMzIBtC5mAHv2o2QBA&scisig=AAGBfm1LdH_lfLmUw7ON3ippk_hdqC2UNQ)来解决这些限制。然而，即使它们在短格式文本上的表现也不是最优的，这使得 LDA 在嘈杂、非结构化的社交媒体数据中的效率受到质疑。

# 2.混合 LDA 方法

为了解决 LDA 的一些突出的局限性，引入了学习单词的向量表示的模型。

通过学习单词和隐藏主题的向量表示，它们被证明在短格式文本上具有更有效的分类性能。

Yu 和 Qiu 提出了一种混合模型，其中使用 Dirichlet 多项式混合和词向量工具扩展了用户-LDA 主题模型，当在微博(即短)文本数据上与其他混合模型或单独的 LDA 模型相比时，产生了最佳的性能。

另一个概念上类似的方法可以应用于 Twitter 数据。这就是[层次潜在狄利克雷分配(hLDA)](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/8607040/&hl=en&sa=T&oi=gsb&ct=res&cd=1&d=11520210757019649284&ei=o27cYL_uJNC5mAHv2o2QBA&scisig=AAGBfm3LKvoBP4St24SaqqxlwIHFxyRcKg) ，旨在通过使用 word2vec(即向量表示技术)自动挖掘推文主题的层次维度。通过这样做，它提取数据中单词的语义关系，以获得更有效的维度。

其他方法，如[非负矩阵分解(NMF)](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0950705118304076&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=13654315554949747121&ei=9W7cYPypIoXSmAHGkZCwCQ&scisig=AAGBfm1tsEQtbcxiUtod_CxwVaAPdych2Q) 模型也被认为在类似配置下对短文本的性能优于 LDA。

# 3.LDA 替代方案

除了 LDA，在主题发现领域还有许多其他的发展。考虑到他们缺乏学术关注，他们似乎有一些关键的局限性，仍然没有得到解决。

在 2017 年的一项研究中，提出了一种用于主题检测的[分层方法](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0004370217300735&hl=en&sa=T&oi=gsb&ct=res&cd=2&d=13965082245195253395&ei=4nPcYOT_IYXSmAHGkZCwCQ&scisig=AAGBfm23mROSJXWPy2LuJmafZM_iBjoOZA)，其中单词被视为二元变量，只允许出现在层次结构的一个分支中。尽管与 LDA 相比是有效的，但是由于表征数据的语言模糊性，这种方法不适合在 UGC 或社交媒体短文本上应用。

一个[高斯混合模型](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/8318929/&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=7717772120239817391&ei=eHTcYNPkCYOHy9YPvfyVwAo&scisig=AAGBfm3EKNwMg1QNMzuyEkDCnOt1rVI3Aw)也可以用于新闻文章的主题建模。该模型旨在将文本表示为概率分布，作为发现主题的手段。虽然比 LDA 好，但它在 UGC 短文本的主题发现中可能再次表现得不太一致。原因是短格式文本缺乏结构和数据稀疏性。

另一个基于[形式概念分析(FCA](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0957417416301038&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=5259186730874098934&ei=Z3XcYITLH9yLy9YPx5K8qAM&scisig=AAGBfm0_96cGJAj0XjYeSimNrGRqQJybMw) )的模型被提出用于 Twitter 数据的主题建模。这种方法显示了基于来自先前主题的信息的新主题检测的便利性。然而，它不能很好地概括，这意味着它不可靠，对主题敏感，这是它没有受过训练的。

其他模型，如 TG-MDP(主题图马尔可夫决策过程)，考虑了文本数据的语义特征，并以较低的时间复杂度自动选择最优主题集。这种方法仅适用于离线主题检测(如前所述，这种方法不太常见)。尽管如此，当与基于 LDA 的算法(GAC，LDA-GS，KG)进行基准测试时，它提供了有希望的结果。

这些模型可以用来做什么？—你可能想知道。

*   [在 Twitter、Reddit 和其他网站等微博社区中发现新兴话题](https://scholar.google.com/scholar_url?url=https://www.sciencedirect.com/science/article/pii/S0957417416301567&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=1190096410935930586&ei=WXbcYKzhLK3CsQKW_J7gDA&scisig=AAGBfm3aftTLdkXWZcc2Xw5jCB_SWu2boA)
*   观察[话题在一段时间内的演变](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/8513750/&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=4295391264651091058&ei=hnbcYL_lL9yLy9YPx5K8qAM&scisig=AAGBfm3pApLB6HHUzQYFdX3loVfCiiu5_A)

## 最后的想法

总的来说，尽管有许多方法可以进行主题建模，但是 LDA 已经发展成为最常用的方法。

考虑到它的局限性，许多混合方法后来被开发出来，以提高主题的准确性和相关性。这些方法经常挑战 LDA 的概率层次结构。

也开发了非 LDA 方法，然而，它们不太适合于简短形式的文本分析。

在以后的一篇文章(本系列的第三部分)中，我将分析用于短文本的最佳主题建模和情感分析算法。