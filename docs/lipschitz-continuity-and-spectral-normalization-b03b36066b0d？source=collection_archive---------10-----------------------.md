# Lipschitz 连续性和谱归一化

> 原文：<https://towardsdatascience.com/lipschitz-continuity-and-spectral-normalization-b03b36066b0d?source=collection_archive---------10----------------------->

## 如何让神经网络更健壮

![](img/1b40e3051ae251d57c09cd45cc7b418f.png)

戴维·布鲁克·马丁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

神经网络最大的弱点之一是它们对输入过于敏感。神经网络缺乏鲁棒性的最著名的例子可能是计算机视觉中的对立例子，其中图像像素的微小而难以察觉的扰动能够完全改变神经网络的输出。理想情况下，如果我们有相似的输入，我们希望输出也相似。李普希茨连续性是一个数学性质，使这一概念具体化。在本文中，我们将看到 Lipschitz 连续性如何用于深度学习，以及它如何激发一种称为谱归一化的新正则化方法。

## 李普希茨连续性

让我们从李普希茨连续性的定义开始:

```
A function *f* : *Rᴹ* → *Rᴺ* is Lipschitz continuous if there is a constant *L* such that∥*f*(*x*) - *f*(*y*)∥  ≦  *L* ∥*x* - *y*∥ for every *x*, *y*.
```

这里∨∨表示通常的欧几里德距离。最小的这种 *L* 是 *f* 的李普希兹常数，记为 *Lip* ( *f* )。注意，这个定义可以推广到任意度量空间之间的函数。

在我们的例子中， *f* 是我们的神经网络，我们希望它是 Lipschitz 连续的，带有一个小的 *Lip* ( *f* )。这将为输出的扰动提供一个上限。Lipschitz 连续性还具有以下性质:

```
Let *f* = *g* ∘ *h*. If *g* and *h* are Lipschitz continuous, then *f* is also Lipschitz continuous with *Lip*(*f*)≦ *Lip*(*g*) *Lip*(*h*)*.*
```

因此，只要我们使一个神经网络的每个分量都是具有小的 Lipschitz 常数的 Lipschitz 连续的，那么整个神经网络也将是具有小的 Lipschitz 常数的 Lipschitz 连续的。

作为一个具体的例子，用于二进制分类的标准 2 层前馈网络可以写成

西格蒙德·∘fc₂∘·雷卢·∘fc₁

其中 fcᵢ(*x*)=*w*ᵢ*x*+*b*ᵢ为全连通层。 *f* 的部件有 FC₁、热卢、FC₂和乙状结肠。

## 激活功能和池化

神经网络中常用的函数(如 ReLU、sigmoid、softmax、tanh、max-pooling)的 Lipschitz 常数= 1。因此我们可以简单地继续使用它们。

## 光谱归一化

让我们考虑一个全连接层。为了简单起见，我们省略了偏差项，所以对于某些权重矩阵 *W* ，FC( *x* ) = *Wx* 。可以看出 FC 有 Lipschitz 常数*Lip*(FC)*=σ(*W*)的谱范数 *W* ，相当于 *W* 的最大奇异值。因此一般来说， *Lip* (FC) 可以任意大。*

*光谱归一化通过归一化 *W* 的光谱范数发挥作用:*

```
**W* ↤ *W* / σ(*W*)*
```

*归一化矩阵的谱范数= 1，因此 *Lip* (FC)也是 1。*

*同样的想法也可以应用于卷积层。请注意，卷积是线性运算，因此可以将它们重写为适当大小的矩阵，然后重用上面的思想。然而，这些矩阵及其谱范数的计算并不简单，我们可以参考[3]了解详细信息。*

## *光谱归一化的实现*

*   *张量流:[TFA . layers . spectral normalization](https://www.tensorflow.org/addons/api_docs/python/tfa/layers/SpectralNormalization)*
*   *py torch:[torch . nn . utils . parameterizations . spectral _ norm](https://pytorch.org/docs/stable/generated/torch.nn.utils.parametrizations.spectral_norm.html)*

## *光谱归一化的其他好处*

*假设我们有一个由全连接层、卷积层和常用激活函数组成的神经网络 *f* 。通过对完全连接的层和卷积层应用频谱归一化，我们从上面用 1 来限制 *Lip* ( *f* )。*

*如果我们的神经网络具有其他种类的组件，那么对全连接层和卷积层应用频谱归一化不足以限制 *Lip* ( *f* )。尽管如此，这样做可能仍然是有益的，因为光谱归一化控制爆炸梯度和消失梯度，详细分析见[6]。*

## *进一步阅读*

1.  *[3]研究了除欧几里德范数之外的其他ₚ范数的李普希茨连续性。*
2.  *谱归一化首先在[7]中被引入，以稳定生成对抗网络(GAN)的鉴别器的训练。在 GAN 框架下，对频谱归一化与其他归一化和正则化方法进行了大量比较研究，例如参见[5]。最近，谱归一化在强化学习中也取得了成功，参见[2]。*
3.  *在上一节中，我们已经讨论了完全连接层和卷积层。这里我们为其他架构提供参考:
    —剩余连接的 Lipschitz 常数在【3】中研究。
    —注意层不是 Lipschitz 连续的。[1]和[4]提出了不同的修改以使它们 Lipschitz 连续。*

## *参考*

1.  *G.达苏拉斯，k .斯卡曼和 a .维尔莫。(2021)，ICML 2021，自我注意层的李普希茨标准化及其在图形神经网络中的应用。*
2.  *F.Gogianu、T. Berariu、M. Rosca、C. Clopath、L. Busoniu 和 R. Pascanu。[深度强化学习的光谱归一化:优化视角](https://arxiv.org/abs/2105.05246) (2021)，ICML 2021。*
3.  *H.Gouk，E. Frank，B. Pfahringer 和 M. J. Cree。[通过加强 Lipschitz 连续性来调整神经网络](https://doi.org/10.1007/s10994-020-05929-w) (2021)，Mach Learn **110** ，393–416。*
4.  *H.Kim、G. Papamakarios 和 A. Mnih。[自我注意的李普希茨常数](https://arxiv.org/abs/2006.04710) (2021)，ICML 2021。*
5.  *K.Kurach、M. Lucic、X. Zhai、M. Michalski 和 S. Gelly。[GANs 正规化、规范化大规模研究](https://arxiv.org/abs/1807.04720) (2019)，ICML 2019。*
6.  *Z.林、塞卡尔和范蒂。[为什么光谱归一化会稳定 GANs:分析和改进](https://arxiv.org/abs/2009.02773) (2021)，arXiv 预印本。*
7.  *T.宫藤，t .片冈，m .小山和 y .吉田。[生成对抗网络的谱归一化](https://arxiv.org/abs/1802.05957) (2018)，ICLR 2018。*