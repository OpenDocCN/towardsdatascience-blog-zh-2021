# 合成数据—主要优势、类型、生成方法和挑战

> 原文：<https://towardsdatascience.com/synthetic-data-key-benefits-types-generation-methods-and-challenges-11b0ad304b55?source=collection_archive---------8----------------------->

## 这里有一个初学者指南，告诉你关于合成数据应该知道些什么

![](img/1163a7338875479e486e58606aca9bb6.png)

图片来源: [Unsplash](https://unsplash.com/photos/bN_TkedaBuQ)

研究人员和数据科学家经常遇到这样的情况，他们要么没有真实的数据，要么由于保密或隐私问题而无法利用这些数据。为了克服这个问题，进行合成数据生成以创建真实数据的替代。为了算法的正确运行，需要进行真实数据的正确替换，这在本质上应该是现实的。本文介绍的研究是关于人工智能中对合成数据日益增长的需求以及我们如何生成这些数据。

# **简介**

合成数据是指除了真实世界事件产生的数据之外，手动或人为创建的数据**。**各种算法和工具可以帮助我们生成综合数据，这些数据有多种用途。这通常需要验证模型，并将真实数据的行为方面与模型生成的数据进行比较。合成数据的起源可以追溯到 20 世纪 90 年代，但真正的使用是在过去几年，人们开始了解数据科学中的风险，这些风险可以通过使用合成数据来消除。

# **合成数据的重要性**

合成数据的重要性在于它能够生成满足特定需求或条件的要素，而这些要素在真实世界的数据中是不可用的。当缺乏测试数据时，或者当隐私是您最优先考虑的问题时，合成数据就能派上用场。

人工智能商业世界非常依赖合成数据——

*   在医疗保健领域，合成数据用于测试某些不存在真实数据的情况和案例。
*   基于 ML 的优步和谷歌的自动驾驶汽车是使用合成数据进行训练的。
*   在金融领域，欺诈检测和保护非常重要。新的欺诈案件可以在合成数据的帮助下进行审查。
*   合成数据使数据专业人员能够使用集中记录的数据，同时仍然保持数据的机密性。合成数据具有复制真实数据的重要特征而不暴露其真实意义的能力，从而保持隐私完整。
*   在研究部门，合成数据帮助您开发和交付创新产品，否则可能无法获得必要的数据。

# **方法论**

主要有两种方法生成合成数据

1.  从分布中提取数字:主要思想是观察真实世界数据的统计分布，然后复制相同的分布以产生具有简单数字的相似数据。
2.  基于主体的建模:主要思想是创建一个观察到的真实世界数据统计分布的物理模型，然后使用相同的模型再现随机数据。它侧重于理解直接影响整个系统的代理之间的交互的影响。

# **利用合成数据的机器学习**

机器学习算法需要处理大量的数据，以便创建一个稳健可靠的模型。否则，生成如此大量的数据会很困难，但有了合成数据，就变得容易多了。它对于计算机视觉或图像处理等领域非常重要，在这些领域中，一旦开发出初始合成数据，模型创建就变得更加容易。

生成对抗网络(GANs)是最近提出的，是图像识别领域的一个突破。一般由两个网络组成:一个鉴别器和一个发生器。生成器网络的功能是生成更接近真实世界图像的合成图像，而鉴别器网络的目标是从合成图像中识别真实图像。gan 是机器学习中神经网络家族的一部分，其中两个网络都通过建立新的节点和层来保持学习和改进。

生成合成数据可以根据需要灵活地调整其性质和环境，以提高模型的性能。标记实时数据的准确性有时非常昂贵，而合成数据的准确性可以很容易地获得高分。

# **合成数据的类型**

合成数据是随机生成的，目的是隐藏敏感的私人信息并保留原始数据中特征的统计信息。合成数据大致分为三类:

*   **全合成数据** —此数据纯属合成，与原始数据无关。这种类型的数据生成器通常会识别真实数据中特征的密度函数，并估计这些特征的参数。随后，对于每个特征，从估计的密度函数中随机生成隐私保护系列。如果只有真实数据的几个特征被选择用于用合成数据替换，那么这些特征的受保护系列被映射到真实数据的其他特征，以便以相同的顺序排列受保护系列和真实系列。用于生成完全合成数据的经典技术很少是自助法和多重插补。由于数据完全是合成的，不存在真实的数据，因此这种技术具有强大的隐私保护功能，并且可以依靠数据的真实性。
*   **部分合成数据** —该数据仅用合成值替换某些选定敏感特性的值。在这种情况下，实际值只有在包含高泄露风险时才会被替换。这样做是为了保护新生成数据的隐私。用于生成部分合成数据的技术是多重插补和基于模型的技术。这些技术也有助于输入真实数据中的缺失值。
*   **混合合成数据** —该数据由真实数据和合成数据生成。对于真实数据的每个随机记录，在合成数据中选择一个接近的记录，然后将两者组合以形成混合数据。它提供了完全和部分合成数据的优点。因此，众所周知，与其他两种方法相比，它提供了良好的隐私保护，具有较高的实用性，但需要更多的内存和处理时间。

# **挑战**

合成数据在人工智能中有很强的根基，具有许多好处，但在处理合成数据时仍有一些挑战需要解决。这些措施如下:

*   生成合成数据的困难。
*   在将复杂的数据从真实数据复制到合成数据时，会遇到许多不一致的情况。
*   合成数据的灵活性使其在行为上存在偏差。
*   对于用户来说，用合成测试数据来验证它可能还不够。他们可能需要你用真实的数据来验证它。
*   在用合成数据的简化表示训练的算法的性能上，可能存在一些隐藏的愚蠢行为，这些行为最近可能会在处理真实数据时出现。
*   许多用户可能不接受合成数据是有效的。
*   从真实数据中复制所有必要的特征在本质上可能会变得复杂。在此过程中，还可能会遗漏一些必要的功能。

# **案例分析**

合成数据有很多运营用例。一些著名的使用案例如下

*   谷歌的自动驾驶汽车 Waymo[https://www . telegraph . co . uk/business/2017/11/07/Waymo-unleashes-自动驾驶-cars-no-back-up-driver-us-roads/](https://www.telegraph.co.uk/business/2017/11/07/waymo-unleashes-self-driving-cars-no-back-up-driver-us-roads/)
*   https://www.uber.com/in/en/atg/technology/优步的自动驾驶汽车
*   Amazon Go 使用合成数据训练无收银员商店算法[https://venturebeat . com/2019/06/05/Amazon-Go-uses-synthetic-data-to-train-cashier-store-algorithms/](https://venturebeat.com/2019/06/05/amazon-go-uses-synthetic-data-to-train-cashierless-store-algorithms/)
*   亚马逊无人机和仓库机器人也使用合成数据来提高效率和准确性。

感谢阅读！:)

# 关于作者

[Kajal Singh](https://www.linkedin.com/in/kajal-singh-197385148/) 是牛津大学[人工智能-云和边缘实施课程](https://www.conted.ox.ac.uk/courses/artificial-intelligence-cloud-and-edge-implementations)的数据科学家和导师。她还是《强化学习在真实世界数据中的应用(2021) 》一书的合著者

# **参考文献**

[https://www . riaktr . com/synthetic-data-become-major-competitive-advantage/](https://www.riaktr.com/synthetic-data-become-major-competitive-advantage/)

[https://www . tech world . com/data/what-is-synthetic-data-how-can-it-help-protect-privacy-3703127/](https://www.techworld.com/data/what-is-synthetic-data-how-can-it-help-protect-privacy-3703127/)

[https://blog.aimultiple.com/synthetic-data/](https://blog.aimultiple.com/synthetic-data/)

[https://mro . Massey . AC . NZ/bitstream/handle/10179/11569/02 _ whole . pdf？sequence=2 &被允许=y](https://mro.massey.ac.nz/bitstream/handle/10179/11569/02_whole.pdf?sequence=2&isAllowed=y)

[https://tdwi . org/articles/2019/06/28/adv-all-synthetic-data-ultimate-ai-disruptor . aspx](https://tdwi.org/articles/2019/06/28/adv-all-synthetic-data-ultimate-ai-disruptor.aspx)

[https://www . techrepublic . com/resource-library/white papers/re-identificati on-and-synthetic-data-generators-a-case-study/](https://www.techrepublic.com/resource-library/whitepapers/re-identification-and-synthetic-data-generators-a-case-study/)

[https://arxiv.org/pdf/1909.11512.pdf](https://arxiv.org/pdf/1909.11512.pdf)