# 2021 年 7 月:ML 新闻与代码

> 原文：<https://towardsdatascience.com/july-2021-ml-news-and-code-367f7d09a77e?source=collection_archive---------29----------------------->

## 全球范围内对更大的语言模型的竞赛，由专家组成，从 Yandex 和 Huggingface、SpeechBrain 等处进行分布式学习。而 OpenAI 驱动的 GitHub Copilot 会改变计算机编程吗？以下是我们每月精选的最新 ML 新闻、研究和代码:

![](img/c7c51a9415af6198739e2574f208cc00.png)

图片作者。

我们已经到了 2021 年的中途，ML 领域还在继续旋转:关于[计算机视觉和模式识别(CVPR 2021)](http://cvpr2021.thecvf.com/) 的会议刚刚召开，Github 和 OpenAI 发布了 Copilot，这是一款前所未有的智能代码完成助手，在过去几周还发生了更多事情。[泽塔阿尔法](http://www.zeta-alpha.com/)很乐意帮助你发现最新的人工智能研究和软件，并让你保持最新。尽情享受吧！

# 🗞有些消息

超大型模特的趋势还远未结束。一年前 [OpenAI 的 GPT-3](https://arxiv.org/abs/2005.14165) 发布了 1750 亿个参数，让 AI 社区大吃一惊。本月是武道 2.0 打破记录的时候，这表明中国在人工智能研究方面一点也不落后。悟道是一个多模态(文本和图像)大规模模型，具有 1.75 万亿个参数，基于混合专家架构(稍后将详细介绍！).虽然官方新闻稿只触及了模型的表面，并没有太多关于它的公开信息，但论文概述了用于训练模型的系统: [FastMoE:一个快速的专家混合训练系统](https://arxiv.org/abs/2103.13262)在 arXiv 上，代码在 GitHub 上开源。希望 OpenAI 能做得更多。

虽然武道没有公开， [GPT-J](https://arankomatsuzaki.wordpress.com/2021/06/04/gpt-j/) 是:迄今为止最佳零拍表演，表演公开可用的 GPT 变形金刚(在 6B 参数)，最近由[王贲](https://github.com/kingoflolz)和[阿兰小松崎](https://twitter.com/arankomatsuzaki)发布。由 JAX 建造，这是对图书馆的又一次推动，在过去的两年里，图书馆缓慢但稳定地受到欢迎。

最后， [Github Copilot](https://copilot.github.com/) 几天前刚刚发布:一个带来下一代代码合成的插件，基于 Codex，一个来自 OpenAI 的类似 GPT 的模型，在公共 Github 代码的大规模数据集上训练。但是该公告导致了一个醒目的登录页面，上面有精选的示例，并且公开演示仍然不可用。很多问题还悬而未决:这个模型能做多大、多快推断？使用的训练数据集的细节是什么？我们是否应该担心受版权保护的数据会像在 previously⁵展示的那样被模型意外地暴露出来？这个 [twitter 帖子](https://twitter.com/Skiminok/status/1409961744294838273?s=20)揭示了这个话题，我们迫不及待地想亲自尝试一下……它有可能使编程效率提高 10 倍，并使编写代码民主化，但它必须非常非常好地工作。我们知道无 bug 代码是不存在的。会比带自动驾驶汽车上路容易吗？

# 🐍密码

这里有一些值得一试的库和资源。

## 👾[在家学习/hivemind](https://github.com/learning-at-home/hivemind) ⭐️ 715 |📄[论文](https://arxiv.org/abs/2002.04013)

*👉想通过众包计算用专有技术训练像 Google 和 OpenAI 那样的大型模型吗？别再看了。*

<https://github.com/learning-at-home/hivemind>  

🚀主要功能(来自自述文件)

*   训练任意大小的神经网络。
*   没有主节点的分布式训练:分布式哈希表允许连接分散网络中的计算机。
*   容错反向传播:即使某些节点没有响应或响应时间过长，向前和向后传递也会成功。
*   分散的参数平均:迭代地聚集来自多个工作者的更新，而不需要在整个网络上同步。

## 📈更多用于分布式培训的框架和库…

👾[微软/DeepSpeed](https://github.com/microsoft/DeepSpeed) ⭐️ 5.2k|🌐[网站](https://www.deepspeed.ai/) *👉为极端规模的机器学习模型(如微软的图灵-NLG (17B 参数))执行分布式训练的框架。*

👾 [horovod/horovod](https://github.com/horovod/horovod) ⭐️ 11.4k |🌐[网站](https://horovod.ai/) |📄[论文](https://arxiv.org/abs/1802.05799) *👉用于 TensorFlow、Keras、PyTorch 和 Apache MXNet 的分布式深度学习培训框架。*

👾[Facebook research/fair scale](https://github.com/facebookresearch/fairscale)⭐️1.2k*👉用于高性能和大规模培训的 PyTorch 扩展库。*

👾 [pytorch/xla](https://github.com/pytorch/xla) ⭐️ 1.4k *👉使用* [*XLA 深度学习编译器*](https://www.tensorflow.org/xla) *连接* [*PyTorch 深度学习框架*](https://pytorch.org/) *和*[*Cloud TPUs*](https://cloud.google.com/tpu/)*的 Python 包。*

## 👾⭐️ 2.5k

*👉基于 PyTorch 的开源一体化语音工具包。*

<https://github.com/speechbrain/speechbrain>  

🚀主要功能(来自自述文件)

*   涵盖的领域和任务:语音识别、特征提取和增强、说话人识别、身份识别和二进制化、语音增强和分离以及多麦克风处理。
*   集成了多个预训练模型🤗拥抱脸。
*   抽象，如 Brain 类，删除模式的不必要的细节，同时完全可定制的定价和评估。
*   通过 PyTorch 数据并行或分布式数据并行和混合精度训练进行多 GPU 训练。
*   透明且可定制的输入和输出管道:原生 PyTorch 数据加载器、缩减采样、BPE 标记化等。

➕:一个新的类似的库值得一试:[软件/OpenSpeech](https://github.com/sooftware/OpenSpeech)

## 🤖最近流行的变压器实现…

👾Facebook research/xcit⭐️415 |📄[论文](https://arxiv.org/abs/2106.09681)👉交叉协方差图像变换器的实现(XCiT)⁸

👾[kzl/决策变压器](https://github.com/kzl/decision-transformer) ⭐️ 661 |📄[论文](https://sites.google.com/berkeley.edu/decision-transformer)👉通过序列建模的强化学习。

👾 [NVlabs/SegFormer](https://github.com/NVlabs/SegFormer) ⭐️ 403 |📄[论文](https://arxiv.org/abs/2105.15203)👉用变形金刚进行语义分割。

👾[oatml/非参数变压器](https://github.com/OATML/non-parametric-transformers) ⭐️ 217 |📄[论文](https://arxiv.org/abs/2106.09681)👉一次处理整个数据集，并使用数据点而不是参数。

## 👾[comp photo/BoostingMonocularDepth](https://github.com/compphoto/BoostingMonocularDepth)@ cvpr 21 | P[项目页面](http://yaksoy.github.io/highresdepth/)

*👉高分辨率单目深度估计。*

<https://github.com/compphoto/BoostingMonocularDepth>  

🚀这个实现太酷了，我们不得不包括它。它实现了 CVPR 2021 论文[通过内容自适应多分辨率合并](https://arxiv.org/abs/2105.14021) ⁰.将单目深度估计模型提升到高分辨率

我们的每月选择到此结束，如果你想了解更多关于事物的研究方面，请查看 2021 年 7 月 arXiv 的最佳，这是最近 ML 学术文献的选择。

*参考文献*

[1] [*开关变压器:以简单有效的稀疏性缩放至万亿参数模型*](https://arxiv.org/abs/2101.03961)——William Fedus，Barret Zoph 和 Noam Shazeer，2021。

[2] [*FastMoE:一种快速的专家混合训练系统*](https://arxiv.org/abs/2103.13262) —何家傲等，2021。

[3] [*一幅图像抵得上 16x16 个字:大规模图像识别的变形金刚*](https://arxiv.org/abs/2010.11929)——作者阿列克谢·多索维茨基、卢卡斯·拜尔、亚历山大·科列斯尼科夫、德克·韦森博恩、翟晓华等人 2021。

[4] [*贪婪函数逼近:一个梯度推进机*](https://projecteuclid.org/journals/annals-of-statistics/volume-29/issue-5/Greedy-function-approximation-A-gradient-boostingmachine/10.1214/aos/1013203451.full)—j . h . Friedman 著，2001。

[5] [*从大型语言模型中提取训练数据*](https://arxiv.org/abs/2012.07805)*——*Nicholas Carlini 等人 2020。

[6] [*ByT5:用预先训练好的字节到字节模型*](https://arxiv.org/abs/2105.13626)*——作者:*薛，阿迪蒂亚·巴鲁阿，诺亚·康斯坦特，拉米·阿尔-Rfou 等人 2021。

[7] [*XGBoost:一个可扩展的树提升系统*](https://arxiv.org/abs/1603.02754) —陈天琦，Carlos Guestrin，2016。

[8] [*XCiT:互协方差图像变换器*](https://arxiv.org/abs/2106.09681)*—*Alaaeldin El-Nouby 等人 2021。

[9] [*注意力是图灵——完成*](https://jmlr.org/papers/v22/20-302.html)*——*豪尔赫·佩雷斯、巴勃罗·巴塞洛和哈维尔·马林科维奇，2021。

[10] [*通过内容自适应多分辨率合并将单目深度估计模型提升到高分辨率*](https://arxiv.org/abs/2105.14021) *—* 作者:S. Mahdi H. Miangoleh，Sebastian Dille，龙脉，Sylvain Paris 和 yaz Aksoy，2021。