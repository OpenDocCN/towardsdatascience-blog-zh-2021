# 训练生成性对抗网络(提供代码)

> 原文：<https://towardsdatascience.com/training-generative-adversarial-network-with-codes-2a6af80cf1f0?source=collection_archive---------23----------------------->

## 打造您的 GAN 系列——第 4 部分，共 4 部分

[![](img/f876cbe9a7948cbb31d55085ee3578eb.png)](https://github.com/jinglescode/generative-adversarial-networks/tree/main/tutorials/04%20Training%20GAN)

图片由[com break](https://pixabay.com/users/comfreak-51581/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=794978)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=794978)

生成敌对网络(GANs)是强大的模型，它学习产生难以与现有真实对象区分的现实对象，如图像和音频(语音/音乐)。

在之前的教程中，我们已经看到了 GAN 中的两个组件——T4 发生器和鉴别器。

在本教程中，我们将合并生成器和鉴别器，并开始训练模型。

这是*构建你的生成性敌对网络的第 4 部分:*

第一部分:[生成对抗网络背后的直觉](/intuition-behind-generative-adversarial-networks-52628d3119f5?sk=3cd90c14b830754e5695533db851b5e1)
第二部分:[甘鉴别器背后的直觉](/intuition-behind-gans-discriminator-122ed821e9e5?sk=040bf4125e6d1c5a790c50db0fe2d4f7)
第三部分:[甘生成器背后的直觉](/intuition-behind-gans-generator-e66f6b0dfa7c?sk=195a36e4191093f789f857ec578ace98)
第四部分:[训练生成对抗网络](/training-generative-adversarial-network-with-codes-2a6af80cf1f0?sk=1ca3e71e91dcb6633e08fb6ee3415fee)(本)

[笔记本](https://github.com/jinglescode/generative-adversarial-networks/blob/main/tutorials/04%20Training%20GAN/Train%20Basic%20GAN.ipynb):训练甘生成手写数字
[GitHub repo](https://github.com/jinglescode/generative-adversarial-networks) :包含本教程系列

# 鉴频器损耗

首先，我们给生成器输入一个随机噪声，生成器会产生一些假样本。这些生成的样本与来自数据集的真实样本一起被输入到鉴别器中。

鉴别器将试图区分样品是真的还是假的。这产生了将在二进制交叉熵损失函数中使用的概率，以与真实标签(真实或虚假)进行比较，并确定错误率和反向传播以更新鉴别器的参数。

# 发电机损耗

然后，我们来训练发电机。同样，我们将噪声向量输入生成器，它会生成一些假样本。这些假样本被送入鉴别器(这次没有真样本)。

鉴别器做出预测，产生这些样本有多假的概率。这些概率在二进制交叉熵损失函数中进行计算，以与*真实*标签进行比较，因为当鉴别器将这些生成的样本分类为真实时，生成器得分。根据错误率，我们将反向传播并更新生成器的参数。

# 训练循环

在训练过程中，我们交替进行训练，使得在任何时候只有一个网络被训练，在生成器和鉴别器之间交替。

我们的目标是确保两个网络具有相似的技能水平。因此，两个网络不能同时更新。否则，鉴别器的性能将优于生成器；因为鉴别器学习区分真假样本要比生成器学习生成样本容易得多。

## 两个网络都需要以相似的速度改进

为什么两个网络应该一起提高，并保持在相似的技能水平？如果鉴别器优于生成器，则所有生成的样本都被分类为 100%假的，因为“100%假”(概率`1`假)损失函数对于网络学习是没有用的。因此没有学习的机会。

`0.7`为假的概率对于网络在反向传播期间更新参数更有意义。(额外:在一个强化学习的情境中，任务是如此的困难，以至于无论代理做什么都失败了，才能给出任何奖励。)

同样，假设发生器优于鉴别器。在这种情况下，来自鉴别器的预测都是 100%真实的，鉴别器将没有机会学会区分真实和伪造的样本。

# 概括起来

鉴别器的目标是最小化真假样本之间的错误分类造成的损失。而生成器的目标是最大化鉴别者成为`real`的概率。

在培训过程中，我们在生成器和鉴别器之间交替进行培训，以确保他们的技能水平相似。

[笔记本](https://github.com/jinglescode/generative-adversarial-networks/blob/main/tutorials/04%20Training%20GAN/Train%20Basic%20GAN.ipynb):训练甘生成手写数字
[GitHub repo](https://github.com/jinglescode/generative-adversarial-networks) :包含本教程系列