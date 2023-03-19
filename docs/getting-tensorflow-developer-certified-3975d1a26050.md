# 获得 TensorFlow 开发者认证

> 原文：<https://towardsdatascience.com/getting-tensorflow-developer-certified-3975d1a26050?source=collection_archive---------12----------------------->

## 即使你有一台 M1 Mac 电脑，你也可以用它来通过测试

准备了很久，最近参加并通过了 TensorFlow 开发者证书考试。该考试测试您使用 TensorFlow 处理图像、时间序列和文本数据的熟练程度。除了这些领域，它还涵盖了减少过度拟合的策略，如增强和辍学层。

![](img/5e4b4a9c6e56acb25b3182534892e897.png)

路易斯·基根在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 你为什么要获得认证？

有两个原因你应该尝试考试。第一，拿到这个证书对学习 TensorFlow 是一个很大的激励。其次，这也是证明和展示你技能的绝佳机会。

如果你以前没有任何机器学习的经验，那么最好先学习一下(使用这些示例资源作为开端: [1](https://www.mrdbourke.com/5-beginner-friendly-steps-to-learn-machine-learning/) 、 [2](/a-checklist-to-track-your-machine-learning-progress-801405f5cf86) 、 [3](https://www.mrdbourke.com/2020-machine-learning-roadmap/) 、 [4](https://www.tensorflow.org/resources/learn-ml?hl=en) )，然后再回来应付考试。

# 资源

我一年前第一次读到关于考试的消息，但只是在去年圣诞节才开始积极地追求它。我最初的计划是在寒假里做准备课程，但最终，我忙于大学的工作，不得不推迟。在三月份，我开始准备，为此我使用了以下资源。

## 1.深度学习。AI TensorFlow 开发者专业证书

费用:七天免费后每月约 50 美元。或者免费审核。

**它教什么**

尽管我已经在 TensorFlow 和 Keras 上有了两年左右的经验，我还是决定开始进行[深度学习。Coursera 上的 AI TensorFlow 开发者专业证书](https://www.coursera.org/professional-certificates/tensorflow-in-practice)。它首先介绍 TensorFlow，教你如何建立和训练基本的神经网络。如果您对这些主题有经验，可以跳过这一部分。第二部分涵盖卷积神经网络，更大的网络，转移学习，和增强技术。然后，在第三个课程中，你将关注自然语言处理:你从嵌入开始，前进到序列模型，并以创作诗歌结束。最后，在最后一门课程中，您将学习时间序列数据。

**对考试的用处**

如果你有一到两年的 TensorFlow 经验，那么你可以跳过这个课程。或者，甚至更好的是:免费旁听个别课程，并检查到目前为止你可能错过了什么。如果你没有任何经验，那么这个课程是强烈推荐的。有两种选择。首先，你可以在 YouTube 上免费观看《T4》。其次，还可以做 2021 年 [TensorFlow 开发者证书:零到精通](https://academy.zerotomastery.io/p/learn-tensorflow)课程(也可以[这里](https://www.udemy.com/course/tensorflow-developer-certificate-machine-learning-zero-to-mastery/)，代码是[这里](https://github.com/mrdbourke/tensorflow-deep-learning))。

尽管证书是一个很好的正面评价的资源，但它不是我唯一的资源。此外，我还参加了一个定制课程来学习更多知识，主要是在我为大学和个人项目所做的基础上。

## 2.重新实施一份文件

费用:免费。

**它教什么**

为了深入了解 TensorFlow，我接受了重新实现一篇论文的挑战。作为参考，我选择了一篇 CycleGAN 的论文，它涵盖了将象征性音乐从一种流派翻译成另一种流派。结果，我学到了很多东西:如何编写高效的输入管道、高效的预处理和定制的训练循环。

**对考试的用处**

很难评估我学到的关于证书的知识有多大用处。它通常超出了考试的要求，但在幕后仍然证明是有价值的。这是因为它不仅仅是编码，而且是如何处理问题和寻找让你进步的解决方案。但是，在后续证书中，可能会对此进行测试。尽管如此，我还是有一个很好的机会用 PyCharm 深入研究编码，这是考试所必需的。

## 3.使用 TFRecords

费用:免费。

**它教什么**

如前所述，在重新实现论文时，我还使用了 TFRecords，TensorFlow 存储数据的原生格式。这对于开始来说有点挑战性，但是一旦你掌握了它，它就非常有价值。这种格式的最大好处是与 TensorFlow 的数据集处理的高度互操作性、从磁盘的快速流式传输以及存储数据的高效方式。更详细的信息可以在[这个 TensorFlow 教程](https://www.tensorflow.org/tutorials/load_data/tfrecord)和[这个 Colab 笔记本](https://colab.research.google.com/drive/1xU_MJ3R8oj8YYYi-VI_WJTU3hD1OpAB7?usp=sharing)中找到。

**对考试的有用性**

我发现这个不是考试必考的，可以跳过这一部分。然而，对于证书范围之外的项目，使用 TFRecords 肯定是好的。

## 4.深度学习简介

费用:免费。

**它教什么**

深度学习的[介绍](http://introtodeeplearning.com)由麻省理工学院免费提供。这个讲座在 12 节课中涵盖了深度学习领域，同时也适用于非技术人员。它从深度学习的一般介绍开始，并继续处理序列和图像数据。之后，它涵盖了其他主题，如强化学习和生成技术。

**对考试的有用性**

前三节课和第八节课是对考试最有用的。但是，我建议您看完整个课程，因为它的表现非常好。此外，检查[附带的代码](https://github.com/aamini/introtodeeplearning)。

## 5.习惯 PyCharm

费用:免费。

**它教什么**

考试需要 PyCharm 代码编辑器。跟随[示例项目](https://www.jetbrains.com/help/pycharm/creating-and-running-your-first-python-project.html)教你如何设置项目、运行和调试代码。

对考试的用处

如果您有使用 PyCharm 的经验，那么您可以跳过这一步。如果您是 PyCharm 的新手，那么这是必须的。在我看来，这将进一步帮助你在尝试证书之前进行几个较小的项目，因为它们将教会你更多关于 PyCharm 的知识。而你已经知道的，考试的时候就不用查了。

## 6.候选人手册

费用:免费。

**它教什么**

[这本手册](https://www.tensorflow.org/extras/cert/TF_Certificate_Candidate_Handbook.pdf)介绍了考试，考什么，怎么考等等。

**对考试的用处**

强制性的。即使您有使用 TensorFlow 的经验，也请阅读此文档。

## 7.TensorFlow 教程

费用:免费。

**他们教什么**

在 [TensorFlow 的主页](https://www.tensorflow.org/tutorials)上列出了很多教程。不过*初学者*类别就足够了；它涵盖了图像和文本分类、回归和基本的 I/O 工具。

**对考试的有用性**

如果您已经完成了 TensorFlow 开发者证书课程或其替代课程，那么您可以跳过此课程。如果您还没有，那么教程是很有价值的资源。

# 我是如何准备的

我在三月份参加了 TensorFlow 课程，每天留出一个小时来进步。以这样的速度，我在近三周内完成了它。我没有为其他资源留出任何特定的时间:大部分工作是为大学项目做的，这教会了我有价值的东西。

如果你正在寻找一门课程，我推荐如下:

1.  观看深度学习介绍讲座
2.  每天留出一两个小时，先学习张量流课程或其他课程
3.  在此期间，使用 PyCharm 进行编码
4.  为了加深理解，重新实现一篇论文或做几个 TensorFlow 的教程
5.  阅读候选人手册
6.  设置一个示例测试环境(说明在上面的手册中；如有问题，请参见下文)
7.  购买并最终通过考试

## 在考试中使用 M1 Mac 电脑

在较新的苹果 M1 电脑上，设置考试环境可能会有问题。如果这是你的情况，诉诸以下两个选项。

第一种解决方案是故意让环境设置失败，然后开始测试。然后，就

1.  把考试代码复制到谷歌实验室，
2.  在那里解决
3.  在那里训练模特
4.  下载它们
5.  将它们放在正确的文件夹中(考试时会很清楚，不要担心)
6.  上传它们(也变得清晰)
7.  并且让他们毫无问题的打分。

第二种解决方案需要更多的设置:

1.  创建一个单独的环境:这也可以是 Anaconda 环境
2.  从任何来源(pip、conda 等)安装尽可能多的软件包——实际的软件包版本不是关键的
3.  开始检查(即使设置可能已经失败)
4.  在考试期间，将 python 解释器切换到之前创建的独立环境
5.  通过考试(如果考试失败，就求助于 Colab 解决方案)。

我在 M1 的 MacBook 上应付考试时，成功地混合使用了这两种方法。如果你设置虚拟环境有困难，请参考这些链接:[苹果官方文档](https://developer.apple.com/metal/tensorflow-plugin/)；[更详细的版本由我](/setting-up-apples-new-m1-macbooks-for-machine-learning-f9c6d67d2c0f)；[尼科斯的方法在这里](https://betterprogramming.pub/installing-tensorflow-on-apple-m1-with-new-metal-plugin-6d3cb9cb00ca)。

# 考试

一旦你购买了每次 100 美元的考试，你将会得到详细的操作说明。在不暴露任何细节的情况下，考试涵盖了你在各个方面的能力。这些类别越来越难解决。但是，你可以使用任何你想要的资源；最后你只需要上传每个类别的训练模型。

对于其中一个类别，我不能运行代码，所以我切换到 Colab。然后我在那里训练模型，把它下载到我的电脑上，并作为考试的一部分上传。

您有五个小时的时间来完成所有问题。我发现这是相当多的时间。考试的时候，你已经有过的感觉了。如果你能解决所有问题(即上传一个在评分基础设施上达到 100%的经过训练的模型)，你就通过了。总的来说，我的目标是在进步之前完美地解决一个类别。除了一个例外，这种方法效果很好。

提交考试或考试时间结束后，我们会通过电子邮件通知您考试结果。对我来说，这花了不到一分钟，迎接我的是

“恭喜你，你已经通过了 TensorFlow 开发者证书考试！”

如果你使用了上面描述的资源，你也会听到这些话！