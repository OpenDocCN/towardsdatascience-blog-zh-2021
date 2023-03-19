# 等待模特训练时的 6 个待办事项

> 原文：<https://towardsdatascience.com/6-to-do-tips-when-waiting-for-models-to-train-cced7b79a0df?source=collection_archive---------30----------------------->

模特训练很费时间。它的长度可以从几小时到几天甚至几周。数据科学家在等待模型训练的时候都做些什么？为了回答这个问题，我采访了 20 多位数据科学家，结合我自己的经历与你分享这个故事。

***你呢？*** *如果模特培训也是你工作的一部分，等待时你最喜欢的活动是什么？欢迎您留下评论，与社区分享您的故事。*

# 待办事项提示 1 —编写自动调优脚本

单个机器学习(ML)模型有数百万个超参数组合。建议在调优脚本中使用贝叶斯优化、进化搜索等方法来提高超参数调优效率，这确实需要时间去学习。

为了简化脚本开发，我想推荐一些有用的超参数优化工具:

*   [ml lib](https://spark.apache.org/docs/latest/ml-tuning.html):Apache Spark 的可扩展机器学习库。
*   [Hyperopt](http://hyperopt.github.io/hyperopt/) :为分布式异步超参数优化而构建的库。
*   [DEAP](https://github.com/DEAP/deap) :一个应用超参数调整的进化计算框架。
*   [GpyOpt](https://github.com/SheffieldML/GPyOpt) :使用 GPy 为高斯过程优化而构建的库。
*   [AutoSklearn](https://automl.github.io/auto-sklearn/master/) :专注于算法选择和超参数调优的 Sklearn 子集。
*   [TPOT](http://epistasislab.github.io/tpot/using/) :一个应用超参数调整的 python 自动化机器学习工具。

# 待办事项提示 2——搜索备选方案

因为“*站在巨人的肩膀上*”，一个单一的研究往往会在多个地方被用作“巨人”，并被分支为备选方案。您可能想探索类似的方法，并将其与您当前的工作进行比较。备选方案不应局限于模型架构，还应包括数据矢量化、标准化、特征提取等。

![](img/a2787995117b560633aab183c547b885.png)

照片由[本工程 RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 待办事项提示 3 —分析数据

一些接受采访的数据科学家评论说，他们总是通过简单地观察相同的数据集几个星期来发现新的东西。他们可以通过维度映射、特征可视化和分布图在非常不同的视图中分析数据。他们声称，重复的数据分析给他们带来了许多新的创新想法，尤其是当遇到瓶颈时。

**也许？** *如果你的机构有人类数据标签员，你也应该试着问问他们是否注意到了什么有趣的事情。*

# 待办事项提示 4 —添加单元测试

“*单元测试*”是工程师们经常使用的一个项目，但是我发现有更多的数据科学家开始引入单元测试来保证工作质量。单元测试可以避免意想不到的输出，检测边缘情况，并防止损坏的代码被推送。识别缺失数据、重复数据、数据范围是数据科学中单元测试的常见用法。Pytest 可能是数据科学家使用的最流行的单元测试工具。

![](img/c612c1b6412d2574b903b85f80823672.png)

由 [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 待办事项提示 5——记录进展

对于数据科学家来说，文档并不是一个很有吸引力的任务，但是一旦你完成了，你和你的团队将会珍惜未来。培训的等待期是回顾当前进度并记录下来的最佳时机。保持文档的更新不仅有助于在你的主管随机走进来时展示你的工作，也有助于避免团队成员重复工作。

# 待办事项提示 6——开始平行培训工作

从谷歌和英伟达等大科技公司的数据科学家那里了解到，强烈建议推出平行培训工作。它将显著提高你的工作效率。

如果你的本地电脑足够强大，可以运行多个培训工作，那就太好了。否则，您可能需要考虑来自云提供商(如 AWS、GCP)的额外计算资源。异地培训模式可以减少您的工作设备的工作量，并轻松提升计算能力。唯一的代价是您可能会在基础设施设置上多花一点时间。

***我不会告诉你*** 😜 ***:*** *有一些诚实的回答，比如喝咖啡，分散同事的注意力，看网飞，玩英雄联盟，改变笔记本窗口大小，祈祷成功，等等。Shuuuu！！！我知道你想说什么！让我们对数据科学家保密。*

# 为了加速训练

如果你需要额外的计算资源比如云机器来加速训练，我给你介绍 **AIbro** ，这是我和我的朋友正在努力开发的一款无服务器模型训练工具。它用一行代码帮助您在 AWS EC2 机器上训练基于 Tensorflow 的模型。作为早期采用者，你将可以免费使用 4 个 V100 图形处理器**。要了解更多细节，你可能想看看这个故事:**

**[https://towards data science . com/train-neural-network-on-cloud-one-line-of-code-8ae 2e 378 BC 98](/train-neural-network-on-cloud-with-one-line-of-code-8ae2e378bc98)。**

**![](img/dc67a4aba5a47b62b17b3c17cf0141b0.png)**

**作者图片**