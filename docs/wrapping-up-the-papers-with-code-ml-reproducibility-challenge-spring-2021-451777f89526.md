# 用 Code ML 再现性挑战结束论文—2021 年春季

> 原文：<https://towardsdatascience.com/wrapping-up-the-papers-with-code-ml-reproducibility-challenge-spring-2021-451777f89526?source=collection_archive---------37----------------------->

![](img/c7c7f4f8d91e3eccda7f01b66f1d8700.png)

DagsHub x 论文，带代码，图片由作者提供

## 2021 年春季 ML 再现性挑战正式结束，我们有一些由 DagsHub 社区贡献的鼓舞人心的项目来分享！

数据科学的可再现性是 DagsHub 成立的核心原因之一，我们正在不断开发新的工具和集成来支持[完全可再现的工作流](https://dagshub.com/blog/introducing-the-machine-learning-reproducibility-scale/)。这就是为什么我们对[论文与代码-再现性挑战](https://paperswithcode.com/rc2020)如此兴奋，并决定第二次支持其参与者(**剧透** : **我们也支持 2021 年秋季挑战**！).

> *“数据的独立验证是跨学科科学研究的基本原则。科学方法的自我修正机制依赖于研究人员复制已发表研究结果的能力，以便加强证据并在现有工作的基础上更进一步。”* [*性质*](https://www.nature.com/articles/d42473-019-00004-y)

在 2021 年春季版中，我们支持 3 个团队提交了完全开源且可复制的 ML 论文，您现在可以轻松使用它们了！在我们深入项目之前，我们想对组织这次活动的代码为的[论文以及投入时间和精力复制论文并使其可访问的社区成员表示敬意。](https://paperswithcode.com/)

![](img/a822a63f465401e188522962c6c5d186.png)

图片来自[期限](https://tenor.com/search/tender-gifs)

因此，没有进一步的到期，我想欢迎 2021 年春季版的转载论文！

# 上下文分解解释惩罚

[***论文***](https://arxiv.org/abs/1909.13584)*[***知识库***](https://dagshub.com/ShaileshSridhar2403/Re-CDEP)*

## *贡献者:*

*   *沙伊莱什·斯里达尔*
*   *[阿兹哈尔谢赫](https://dagshub.com/azhar199865)*
*   *[Midhush](https://dagshub.com/midsterx)*

> **“为了有效地解释深度学习模型，它必须提供对模型的洞察，并建议相应的行动，以实现某些目标。太多时候，一连串可解释的深度学习方法在第一步就停止了，为从业者提供了对模型的洞察力，但没有办法对其采取行动。”论文作者**

***该论文提出了上下文分解解释惩罚，CDEP，其允许在训练期间使用解释来惩罚模型，从而不学习虚假的相关性**。CDEP 在解释的帮助下将特征空间分解为相关和不相关特征，并惩罚模型来查看相关特征进行分类。例如，在 [ISIC 皮肤癌分类任务](https://challenge.isic-archive.com/)中，数据集包含阳性患者佩戴创可贴的偏倚图像。通过使用 CDEP，模型可以被训练成忽略有偏差的创可贴特征，并学习正确的特征。在存储库中，CDEP 已经被应用于跨多种模态的不同架构。*

*![](img/d2a595a4cba35e2d4729bb7ebd52a97f.png)*

*图 S4。来自 ISIC 的良性样本热图，图片来自[官方文件](https://arxiv.org/abs/1909.13584)*

*在这个项目中，Shailesh、阿兹哈尔和 Midhush 在 [Tensorflow](https://www.tensorflow.org/) 中重新实现了原来的 [PyTorch 项目](https://github.com/laura-rieger/deep-explanation-penalization)。这要求他们从头开始编写 [PyTorch 的“un pool”](https://pytorch.org/docs/stable/generated/torch.nn.MaxPool2d.html)函数，以便与 Tensorflow 一起使用。这后来成为一个[补丁](https://github.com/tensorflow/addons/commits?author=midsterx)被推送到 Tensorflow 插件库，让它们以一个的价格贡献给两个开源项目！*

# *少投学习的自我监督*

*[***论文***](https://arxiv.org/abs/1910.03560)*[***知识库***](https://dagshub.com/arjun2000ashok/FSL-SSL)**

## ****投稿人:****

*   **哈斯旺斯·艾库拉**
*   **阿尔琼·阿肖克**

****在本文中，研究者在**<https://research.aimultiple.com/few-shot-learning/>****【FSL】的背景下考察了** [**自监督学习**](https://en.wikipedia.org/wiki/Self-supervised_learning) **(SSL)的作用。**尽管最近的研究显示了 SSL 在大型无标签数据集上的优势，但它在小型数据集上的效用相对来说还未被探索。他们发现，SSL 将少数元学习者的相对错误率降低了 4%-27%，即使数据集很小，并且只利用数据集中的图像。****

****![](img/5302ca756e766f0edc0ee5eb7e01571b.png)****

****结合监督和自我监督损失进行少镜头学习，图片来自[官方论文](https://arxiv.org/abs/1910.03560)****

> *****“我们选择这篇论文是因为少镜头学习是一种新兴的、越来越受欢迎的机器学习范式，而自我监督学习似乎是一种在 FSL 获得更好性能的简单方法，不需要花里胡哨。”Arjun 和 Haswanth*****

****Arjun 和 Haswanth 基于作者的代码库，在五个基准数据集上复制并验证了论文的主要结果。此外，他们从头实现了域选择算法，并验证了它的好处。****

****论文使用了 224x224 的图像尺寸，这引起了 Arjun 和 Haswanth 的兴趣，所以他们决定研究它如何影响模型性能。他们将图像大小修改为 84x84，这是 FSL 论文中常用的设置，同时还简化了架构。他们发现这种设置是失败的，它降低了模型的性能。****

# ****可解释的 GAN 控制****

****[***论文***](https://arxiv.org/abs/2004.02546)*[***知识库***](https://dagshub.com/midsterx/Re-GANSpace)*****

## ****贡献者****

*   ****[毗湿奴 Asutosh Dasu](https://dagshub.com/vdasu)****
*   ****[Midhush](https://dagshub.com/midsterx)****

******本文描述了一种简单的技术，用于分析** [**GANs 模型**](https://en.wikipedia.org/wiki/Generative_adversarial_network) **并为图像合成创建可解释的控件，如视点变化、老化、光照和一天中的时间。******

****![](img/cf1d2a3bd8d720271ca4528202010a40.png)****

****图 1:使用我们的方法发现的控件执行的图像编辑序列，应用于三个不同的 GANs，将[官方文件](https://arxiv.org/abs/2004.02546)成像****

****Vishnu 和 Midhush 使用了 [StyleGAN](https://arxiv.org/abs/1812.04948) 和 [StyleGAN2](https://arxiv.org/abs/1912.04958) 模型来重现论文的结果。这两种模型都是通过计算映射网络输出的主成分分析来对几个采样的潜在向量进行工作的。这给出了映射网络空间的基础，从中我们可以通过改变 PCA 坐标来编辑新的向量。扩充的向量然后被馈送到合成网络，以获得具有修改的属性的图像。****

****Vishnu 和 Midhush 将最初的 [PyTorch 实现](https://github.com/harskish/ganspace)转换为 Tensorflow，并验证了论文中提出的主张。他们用论文中使用的基准数据集训练模型，比如 [FFHQ](https://github.com/NVlabs/ffhq-dataset) 、 [LSUN Car](https://www.tensorflow.org/datasets/catalog/lsun) 和 [CelebAHQ](https://www.tensorflow.org/datasets/catalog/celeb_a_hq) 。为了验证他们的结果，他们用原始论文中没有使用的数据集测试了模型的性能，如[甲虫](https://asombro.org/beetles/)和[动漫肖像](https://www.gwern.net/Crops)。****

> *****“最初，我们试图使用原始 PyTorch 代码和我们在 Tensorflow 中修改的代码重新创建具有相同 RGB 值的图像。然而，由于 PyTorch 和 Tensorflow 中随机数生成器的差异，即使使用相同的种子，随机值也不相同。这导致了一些生成图像的背景伪影的微小差异。一旦我们确定这是微小差异的原因，我们就能够在 Tensorflow 实现中插入 PyTorch 的随机数生成器，并成功地再现这些图像。* ***最终，我们能够验证与 StyleGAN 和 StyleGAN2 型号*** *相关的所有声明。”毗湿奴和 Midhush*****

# ****摘要****

****我们要感谢所有参与这项挑战的了不起的数据科学家。DagsHub 团队喜欢与你们每一个人一起工作，并在这个过程中学到了很多。你对社区产生了巨大的影响，让我们离开源数据科学又近了一步。如前所述，我们正在支持**2021 年秋季版的代码可再现性挑战论文。**如果你想参与并推动机器学习领域向前发展，请前往[新指南页面](https://dagshub.com/DAGsHub-Official/reproducibility-challenge/wiki/ML+Reproducibility+Challenge+Fall+2021)并加入我们的 [Discord 社区](https://discord.gg/7Bhm3WTxHk)开始吧！团队是来帮忙的！****