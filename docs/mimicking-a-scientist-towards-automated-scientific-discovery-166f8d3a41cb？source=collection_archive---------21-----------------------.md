# 模仿科学家:走向自动化科学发现

> 原文：<https://towardsdatascience.com/mimicking-a-scientist-towards-automated-scientific-discovery-166f8d3a41cb?source=collection_archive---------21----------------------->

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

## 如何通过三个简单的步骤构建科学发现引擎

![](img/bbdf5d5651c7a06c0354de51161d99d3.png)

由[帕维尔·内兹南诺夫](https://unsplash.com/@npi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

在本文中，我演示了如何构建一个简单的自动化科学发现引擎。 [**检查演示完毕**](https://aliaksandrkazlou.github.io/2021/10/31/paper-generator-post/) 。

当前的科学实践受到我们认知和社会学局限的束缚。机器比我们更快、更高效、更耐用。最重要的是，机器不受人类偏见的影响。将人工智能融入科学过程可能会引发下一场科学革命。

但是，有可能实现科学自动化吗？尽管今天它很微弱，但许多人认为它不仅是可能的，而且是我们力所能及的。艾伦·图灵研究所提出了一个挑战，那就是为科学发现建造一个与顶级人类科学家没有区别的引擎。这个想法是，一个明确的使命声明，以获得诺贝尔奖为目标，是加速挑战的最佳方式。

机器是否会与顶级人类科学家无法区分只是拼图的一部分。著名的中文房间论点认为，仅仅使用句法规则来操纵符号不会产生真正的理解。类似的担忧也出现在人类对科学过程的理解上( [Gigerenzer 2004](https://pure.mpg.de/rest/items/item_2101336/component/file_2101335/content) ， [Altman 1994](https://www.bmj.com/content/308/6924/283) )。然而，通过图灵测试的变化是必要的第一步。

# **指定研究区域**

最初的提议集中在生物系统上。我认为不是生物系统，而是社会系统最不适合自动化。他们代表了最复杂的，认识论上错综复杂的研究领域。由于社会系统过于复杂，人类无法进行推理，在经验假设的广阔空间中进行自动搜索是向前迈出的关键一步。解决社会科学面临的这一挑战将为其他学科的自动化提供有力的证据。

关注社会系统的另一个原因是社会科学研究者所展示的成熟程度。社会研究的重要性不可估量。这将推动社会科学家处于可靠、诚实和透明的前沿( [Camerer et al .，2018](https://www.nature.com/articles/s41562-018-0399-z) ， [Open Science Collaboration，2015](https://www.science.org/doi/10.1126/science.aac4716) ，【常和李，2015 )。达到那样的学术水平是一项艰巨的挑战。

这里展示的原型集中在民主和经济增长的问题上。以这种方式缩小关注范围很有吸引力，原因有二。首先，关于该主题的文献数量庞大且高度确定( [Acemoglu、Naidu、Restrepo 和 Robinson，2019](https://www.nber.org/system/files/working_papers/w20004/w20004.pdf) 、[波苏埃洛、Slipowitz 和 Vuletin，2016](https://www.econstor.eu/bitstream/10419/146477/1/IDB-WP-694.pdf) )。第二，政治经济宏观系统是高阶系统。这一层次的研究是方法论复杂性、想象力和创造力的巅峰([曼昆 1995 年，第 307 页](https://d1wqtxts1xzle7.cloudfront.net/63529523/growth_of_nations20200604-36712-14v9qkq-with-cover-page-v2.pdf?Expires=1636198448&Signature=X9WCnD0HpnlQoc551hNPSn~9lOhwHyTeSkrc0BIcMT3UpnR8LaKmIaXJ5B8rz4AHEenWGD70KvbYQqW5AH9EbqbB8wZX3Qawrif16kaBXrYdZri54~WH~tvB4GPaFEm8VBdyWa7AgfyPgbtNQC-DGnzSJfh6i0jJD~CfZqBv34DvK43KhRO2vwPBamNJfMeirSENqIg21eJz0udEn3FRq9ETXJqucn2USFIpaxJ7ufrjn-6DaHdMMvKaw8cN4y0lAA18CS-Hz7VkEzf28h4JW3xI6whZ-XCOrserXFvDu9hVqIOARpAvE~M3wqDfJqFBTA8rbzEjj-35158TjOu6uA__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA)，[林道尔和普里切特，2002 年，第 18 页](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.217.9005&rep=rep1&type=pdf))。

# **自动化科学发现的三大支柱**

## 获取大数据

我们生活在大数据时代。大数据是灵活生成假设的沃土。这种灵活性是实现人类水平的科学发现能力的关键组成部分( [Gelman 和 Loken，2014](http://www.psychology.mcmaster.ca/bennett/psy710/readings/gelman-loken-2014.pdf) 、 [Simmons、Nelson 和 Simonsohn，2011](https://journals.sagepub.com/doi/10.1177/0956797611417632) )。

在这项研究中，我使用了 WDI 数据库(CC BY 4.0)。[世界发展指标](https://datatopics.worldbank.org/world-development-indicators/)是最大的国家级数据库。它包含 217 个经济体和 40 多个国家集团的 1400 个时间序列指标。

## **搜索假设空间**

我们能让我们的探索范围更大吗？答案是肯定的，通过应用成千上万的模型而不是单一的模型。每个模型代表一个假设。人类的认知限制使我们无法同时处理多个假设。AI 的优势在于它赋予了机器无需显式编程就能学习的能力( [Gründler 和 Krieger，2016](https://www.sciencedirect.com/science/article/pii/S0176268016300222) )。这确保了发现的过程是全面的，没有人为的偏见。

在假设空间上搜索也需要一个快速和可扩展的算法。在这项研究中，我依赖于一个现代人工智能框架— [具有线性激活函数和时空连续体约束的感知器](https://www.econometrics-with-r.org/10-3-fixed-effects-regression.html)。

## **公式化理论**

人们应该意识到强烈的批评意见，即当前的大数据分析是由理论调查驱动的( [Monroe，Pan，Roberts，Sen 和 Sinclair，2015](https://scholar.harvard.edu/files/msen/files/big-data.pdf) )。对纯统计估计背后的潜在机制进行理论化是一个安全网。一个单独的估计或一个单独的理论可能被认为是弱证据。然而，当很好地结合在一起时，它们会相互加强。这种方法是人机共生的第一块基石，不应该被轻视。因此，所有的估计都必须有坚实的理论支持。

# 结论

我们正处于人工智能革命的中期，我们是分裂的。有些人认为这是一种生存威胁，是文明的夕阳，在它的照耀下，人类策划了自己的灭亡。还有一些人认为这个特性已经存在了( [Bem，2011](https://d1wqtxts1xzle7.cloudfront.net/54466465/FeelingFuture-with-cover-page-v2.pdf?Expires=1636450670&Signature=T2DgO3908UdXeD1Yea3d~V8Qh2TjDblgYkCKcZx6whw0lhAxSGeFvk2Ok1kECQelSDyCQXz4eboZBM~uXPXqC75D9jr41RLlsEChLG3e-0-q-74yoSfmwJf3qWs3lyqKqAnLOBcRBj4FvqrGa5i3uW6Pj1KWvl6IF4tS-Vt8A7EyrHEqga~C7yhe-1euVra4Nji5SmE0OgaKB0cYympBUKT0rjRwchH~th~Gmd9-oNddGb6cm1k3678Us8vRHlXhnHMKuIaCsdJaQtU12yA0nLAiMfGEhngXAXdpMokuoWWWnQ24EuB~dEGDPJax77Etz3gDuWQytpYuNXvrDKhmmA__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA) )。这个原型是乐观主义者向前迈出的一小步，也是“方法论恶霸”的又一场败仗( [Singal，2018)](https://www.thecut.com/2016/10/inside-psychologys-methodological-terrorism-debate.html) 。

# 参考

1.  Acemoglu，d .，Naidu，s .，Restrepo，p .，& Robinson，J. A. (2019 年)。民主确实会导致增长。*政治经济学杂志*， *127 期* (1)，第 47–100 页。
2.  奥尔特曼博士(1994 年)。糟糕的医学研究丑闻。
3.  贝姆博士(2011 年)。感知未来:对认知和情感异常追溯影响的实验证据。*《人格与社会心理学杂志》*， *100 期* (3)，407 期。
4.  卡默勒，C. F .，德雷伯，a .，霍尔茨迈斯特，f .，何，T. H .，胡贝尔，j .，约翰内松，m .，…，吴，H. (2018)。评估 2010 年至 2015 年间自然和科学中社会科学实验的可复制性。*自然人类行为*， *2* (9)，637–644。
5.  张阿昌，李，p(2015)。经济学研究可复制吗？来自 13 种期刊的 60 篇发表的论文说“通常不会”。
6.  Gelman，a .，& Loken，E. (2014 年)。科学中的统计危机:数据依赖分析——一个“分叉路径的花园”——解释了为什么许多具有统计意义的比较站不住脚。*美国科学家*， *102* (6)，460–466。
7.  Gigerenzer，G. (2004 年)。愚蠢的统计数据。*《社会经济学杂志》*， *33* (5)，587–606 页。
8.  k . gründler 和 t . Krieger(2016 年)。民主与增长:来自机器学习指标的证据。*欧洲政治经济学杂志*， *45* ，85–107。
9.  Hanck，c .，Arnold，m .，Gerber，a .，& Schmelzer，M. (2019)。与 r*杜伊斯堡-埃森大学*一起介绍计量经济学。
10.  北野，H. (2021)。诺贝尔图灵挑战:创造科学发现的引擎。 *npj 系统生物学与应用*， *7* (1)，1–12。
11.  林道尔博士、普里切特博士、罗德里克博士和埃考斯博士(2002 年)。有什么大想法？第三代经济增长政策[附评论]。*经济学家*， *3* (1)，1–39。
12.  N. G .曼昆、E. S .费尔普斯和 P. M .罗默(1995 年)。国家的发展。*布鲁金斯经济活动论文*， *1995* (1)，275–326 页。
13.  Monroe，B. L .，Pan，j .，Roberts，M. E .，Sen，m .，和 Sinclair，B. (2015)。不要！形式理论、因果推理、大数据在政治学中并不是相互矛盾的趋势。 *PS:政治学&政治学*， *48* (1)，71–74。
14.  j . singal(2018 年)。心理学内部的“方法论恐怖主义”争论。伤口。十月十二日。[https://www . the cut . com/2016/10/inside-psychologys-methodology-terrorism-debate . html](https://www.thecut.com/2016/10/inside-psychologys-methodological-terrorism-debate.html)
15.  j .波苏埃洛、a .斯利波维茨和 g .武莱廷(2016 年)。民主不会导致增长:内生性论点的重要性。
16.  开放科学合作。(2015).评估心理科学的再现性。*理科*， *349* (6251)。
17.  西蒙斯，J. P .，尼尔森，L. D .，&西蒙森，U. (2011 年)。假阳性心理学:数据收集和分析中未公开的灵活性允许呈现任何有意义的东西。*心理科学*， *22* (11)，1359–1366。
18.  《图灵人工智能科学家大挑战》[https://www . Turing . AC . uk/research/research-projects/Turing-AI-scientist-grand-challenge](https://www.turing.ac.uk/research/research-projects/turing-ai-scientist-grand-challenge)
19.  世界银行，世界发展指标。[https://data topics . world bank . org/world-development-indicators/](https://datatopics.worldbank.org/world-development-indicators/)