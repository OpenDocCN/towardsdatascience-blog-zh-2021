# 介绍合成数据社区

> 原文：<https://towardsdatascience.com/introducing-the-synthetic-data-community-9db08d8679ed?source=collection_archive---------19----------------------->

## 一个充满活力的社区开创了数据科学的基本工具包

![](img/a47973e01953ceaecdbce93666cb921c.png)

迪伦·吉利斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

根据 2017 年哈佛商业评论[的一项研究](https://hbr.org/2017/09/only-3-of-companies-data-meets-basic-quality-standards)，只有 3%的公司数据符合基本质量标准。根据 2020 年 YData [研究](https://medium.com/ydata-ai/what-we-have-learned-from-talking-with-100-data-scientists-d65ed2475f4b)，数据科学家面临的最大问题是无法获得高质量的数据。

尽管知道[数据是新的石油](https://www.economist.com/leaders/2017/05/06/the-worlds-most-valuable-resource-is-no-longer-oil-but-data)和最有价值的资源，但并不是每个公司、研究人员和学生都能像一些科技巨头那样获得最有价值的数据。随着机器学习算法、编码框架[的快速发展](https://www.stateof.ai/)，可以说人工智能中最稀缺的资源是大规模的高质量数据。

[合成数据社区](http://syntheticdata.community/)旨在为数据科学团队、研究人员和初学者打破障碍，释放合成数据的力量。我们相信拥有高质量的数据是真正的游戏规则改变者。

如果我们可以创建类似于最初无法访问的真实世界数据的高质量数据，会怎么样？这会开启多少无限的可能性？

# 什么是合成数据，为什么我们应该关注它

![](img/e1adab3985ebc7a1585f518523060930.png)

作者创作—原始照片由 [Rawpixel](https://www.freepik.com/free-photo/business-team-hands-raised-with-successful_2892704.htm#page=1&query=community&position=24) 提供

在深入了解之前，让我们先了解基本的构建模块，以及为什么它们在我们的数据科学工具包中至关重要。

合成数据是人工生成的数据，不是从现实世界的事件中收集的。它复制实际数据的统计成分，不包含任何可识别的信息，确保个人隐私。如果使用得当，它可以为每个人提供大规模的高质量数据访问。

Unity 的 AI 和 ML 高级副总裁 Danny Lange，[声称](https://venturebeat.com/2021/07/12/unitys-danny-lange-explains-why-synthetic-data-is-better-than-the-real-thing-at-transform-2021-2/)合成数据可以解决组织面临的最重要的问题:收集正确的数据，构建数据，清理数据，并确保数据无偏见且符合隐私法。我们完全同意。

它不仅有利于组织，也有利于个体数据科学家和研究人员。正在构建投资组合的自我驱动的数据科学家[可以从大规模高质量数据的可用性中受益](/synthetic-data-generation-a-must-have-skill-for-new-data-scientists-915896c0c1ae)。

合成数据比真实数据更好，因为我们可以控制我们所创造的东西:平衡、无偏见、隐私合规、无限扩张的数据。

# 最先进的合成器和开源软件

在合成数据社区，我们不仅宣扬合成数据的重要性；我们采取行动打破重要新兴技术的所有进入壁垒。

生成对抗网络(GAN)是一种基于深度神经网络的生成模型，是生成与真实数据集难以区分的人工数据集的有力工具。最常见的数据科学问题需要表格和时间序列数据，因此我们使用 GANs 来创建合成的表格和时间序列数据。

我们没有重新发明轮子，而是参考了在合成数据领域发表的领先的最先进的研究论文，实现了合成器，并将它们放在一个包中以方便使用。我们参考的一些研究包括:

*   [GAN](https://arxiv.org/abs/1406.2661) 和 [CGAN(条件 GAN)](https://arxiv.org/abs/1411.1784)
*   [WGAN](https://arxiv.org/abs/1701.07875) (Wassertain GAN)和 [WGAN-GP](https://arxiv.org/abs/1704.00028) (带梯度惩罚的 WGAN)
*   [德拉甘](https://arxiv.org/pdf/1705.07215.pdf)(关于甘斯的收敛性和稳定性)
*   [克拉姆甘](https://arxiv.org/abs/1705.10743)(克拉姆距离作为有偏瓦瑟斯坦梯度的解决方案)
*   [时序甘](https://papers.nips.cc/paper/2019/file/c9efe5f26cd17ba6216bbe2a7d26d490-Paper.pdf)

我们说开源了吗？是的，你没听错。我们所做的所有工作都是开源的，当我们致力于向该库添加更多功能时，我们邀请您做出贡献，使该库变得更好。

# 无限的可能性

对研究内容不感兴趣——告诉我如何开始？

当然，打开你的终端，输入以下内容:

```
pip install ydata-synthetic
```

就是这样。您可以在一个命令中安装所有的合成器。现在，为了让您了解各种库的用法，我们包含了多个以 jupyter 笔记本和 python 脚本形式呈现的示例。

我们推荐从[这个例子开始，Google Colab notebook](https://colab.research.google.com/github/ydataai/ydata-synthetic/blob/master/examples/regular/gan_example.ipynb) ，它综合了[信用卡欺诈数据集的少数类。](https://www.kaggle.com/mlg-ulb/creditcardfraud)

有什么问题吗？加入我们专门的 [**合成数据社区 Slack**](http://slack.ydata.ai/) 空间，问走一切。我们是一群友好的人，希望互相学习，并在这个过程中成长。

你了解合成数据及其重要性。只需一行命令，您就可以使用最先进的合成器。你有一大堆例子来引导你。你有一个充满活力、充满热情的社区来共同学习和成长。

你知道这意味着什么吗？**无限可能。**

当所有高质量数据的壁垒都被打破，我们能完成的事情是无穷无尽的。女士们、先生们，介绍[合成数据社区](https://syntheticdata.community/) —加入我们这一激动人心的旅程。

[*法比亚娜*](https://www.linkedin.com/in/fabiana-clemente/) *是 CDO*[*y data*](https://ydata.ai/?utm_source=medium&utm_medium=signature&utm_campaign=blog)*。*

**用改进的数据加速 AI。**

[*YData 为数据科学团队提供第一个数据开发平台。*](https://ydata.ai/?utm_source=medium&utm_medium=signature&utm_campaign=blog)