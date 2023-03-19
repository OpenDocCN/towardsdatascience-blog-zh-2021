# 2021 年学习计算机视觉的初学者指南

> 原文：<https://towardsdatascience.com/beginners-guide-to-learn-computer-vision-in-2021-6083ab6de6af?source=collection_archive---------26----------------------->

## 阅读可以帮助你提高计算机视觉技能的资源

![](img/df2b2c9841c53ff5721230f8baa5f0b3.png)

尼克·莫里森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

从我本科学习的第一年开始，我就是机器人俱乐部的一员，在那段时间，深度学习是我们大学里一个相当新的流行词。在我的俱乐部里，每个人都对计算机视觉着迷，因为在一次关于图像处理的研讨会之后，我们都认为这个世界已经向我们展示了它的真正潜力！现在在从众的驱使下，我也学习和发掘了和身边所有人一样的资源，他们给了我很大的基础。但是我第一次面试一家计算机视觉初创公司就让我找到了自己的位置。我不知道这个领域到底有多广阔，所以为了帮助你们避免尴尬，我汇集了一些资源，让你们的旅程更轻松。

首先，花点时间了解自己是否真的喜欢这个领域。为此，我将链接一些展示特定应用的博客，如果您感兴趣，请继续查看相应的资源。

## 在线课程

先说计算机视觉的基础知识，以及大家需要的基本概念。我更喜欢的学习方式总是通过在线讲座，因为我可以控制速度，而且对于更简单的概念，我可以浏览笔记而不是观看整个视频。所以接下来的系列讲座对学习 CV 是相当全面的。

*   [计算机视觉简介](https://www.udacity.com/course/introduction-to-computer-vision--ud810)，Udacity，佐治亚理工学院提供。这门课程帮助我克服了学习这个全新领域的恐惧。
*   [CS231n:用于视觉识别的卷积神经网络](http://cs231n.stanford.edu/)，斯坦福大学。这门课的老师是这个领域的世界知名教授，在我看来，这是 CNN 最好的课程之一。
*   [计算机视觉讲座](https://www.youtube.com/playlist?list=PL7v9EfkjLswLfjcI-qia-Z-e3ntl9l6vp)，Alberto Romay。通俗易懂的讲座系列为了让初学者对 CV 有一个完整的了解，我的一些朋友学习了这个课程而不是第一个。

为了充分利用这些课程，坚持一个系统的时间表，并适当地练习每一项作业，以清除你的基础知识。这将让你在使用计算机视觉和深度学习的通用框架和平台时充满信心。

## 编码和实现

学习 CV 的第一步是用你选择的语言学习 OpenCV 框架。对于大多数应用程序，我更喜欢 OpenCV 而不是 MATLAB，但这取决于您。学习图像处理、相机几何和 OpenCV 的一些书籍有:

*   使用 OpenCV 库学习 OpenCV 3:c++中的计算机视觉，Adrian Kaehler 和 Gary Bradski( [Book](https://amzn.to/3xnjuCT) )
*   3D 计算机视觉技术和算法介绍，博古斯瓦夫·西加内克和 j .保罗·西伯特([书](https://amzn.to/3gv8KwL))

## 深度学习框架:

深度学习框架帮助你实现各种深度学习架构。它们使得不同神经网络的实现更加容易。最常用的框架有[**【py torch】**](https://pytorch.org/tutorials/)**[**Keras、**](https://www.tensorflow.org/guide/keras) 和 [**TensorFlow**](https://www.tensorflow.org/) 。我个人在大多数情况下使用 PyTorch，因为我觉得它易于理解和实现。还有更多构建在 PyTorch 或 TensorFlow 之上的库/框架，使实现更加容易。下面是两个这样的库，我相信会很有帮助。**

*   **Fast.ai 是你应该注意的下一个课程。还有，fast.ai 是 PyTorch 上面的高层框架，但是他们改 API 太频繁了，文档的缺乏使得用起来不太靠谱。然而，理论和有用的技巧只是花时间观看本课程的幻想。(免费)**
*   **PyTorch Lightning 是当今最热门的图书馆之一。它可以帮助你实现多 GPU 训练和许多其他工程设施，只需改变一些参数。如果你想学习 PyTorch 闪电，我写了这篇[博客](/an-introduction-of-pytorch-lightning-230d03bcb262)。**

## **研究及其实施**

**计算机视觉在不断进步，这个领域的研究也在以极快的速度进行。因此，尤其是在深度学习和计算机视觉的交汇点，让自己跟上当前最先进的方法是很重要的。**

*   **ArXiv.org——你可以在这里找到几乎所有的研究论文，最棒的是，它是完全免费的。**
*   **[Github](https://github.com/topics/computer-vision?l=python) —你可以在 Github 上找到所有的开源代码。查看和阅读代码有助于您清晰地实现不同的研究论文，并增强您实现任何新架构的信心。**

## **竞争**

**如果你在学习新事物的时候需要持续的动力，参与竞争将是最好的选择。我有很多朋友，只通过参加多个比赛，就了解了很多不同的概念。有许多可用的网站，但我通常更喜欢以下方式:**

*   **他们有许多活跃的竞争和不活跃的竞争。你可以利用这个地方通过参加不活跃的竞赛来学习，看看哪种方法表现最好，然后找出原因。你可以参与到积极的竞争中来，让自己相信你能够理解问题并相应地处理它(显然，只有当你足够自信，能够找出哪件事最适合当前的问题陈述时，你才应该这样做)**
*   **参加 CVPR、ECCV、ICCV、AAAI 等会议组织的比赛。这些会议是顶级的计算机视觉会议，每个人都梦想在这些会议上发表论文，因此如果你能在排行榜上名列前茅，你也将有机会参加这些会议，我个人认为这将使你完美地接触到当前的艺术状态。**

## **必须阅读研究论文**

**以上提到的东西足以成为计算机视觉的媒介。但是，我强烈建议也阅读下面列出的一些研究论文。阅读研究论文有助于你了解研究中的新观点，并激发你进行创新思维。我还附上不同主题的介绍性博客或视频。**

*   **图片分类([博客](/how-artificial-intelligence-learns-regardless-of-day-or-night-e9badcc1b649))——[AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks)、 [GoogLeNet](https://arxiv.org/abs/1409.4842) 、 [VGG16](https://arxiv.org/abs/1505.06798) 、 [ResNet](https://arxiv.org/abs/1704.06904) 、 [Inception](https://arxiv.org/abs/1512.00567) 、 [Xception](https://arxiv.org/abs/1610.02357) 、 [MobileNet](https://arxiv.org/abs/1704.04861)**
*   **物体检测。([视频](https://www.youtube.com/watch?v=VOC3huqHrss))——[RCNN](https://arxiv.org/abs/1311.2524)、 [Fast-RCNN](https://arxiv.org/abs/1504.08083) 、[Fast-RCNN](https://arxiv.org/abs/1506.01497)、 [SSD](https://arxiv.org/abs/1512.02325) 、 [YOLO](https://arxiv.org/abs/1506.02640)**
*   **物体跟踪。([视频](https://www.youtube.com/watch?v=FuvQ8Melz1o))——[SMOT](https://paperswithcode.com/paper/smot-single-shot-multi-object-tracking)，[费尔莫特](https://paperswithcode.com/paper/a-simple-baseline-for-multi-object-tracking)**
*   **语义分割。([博客](https://www.jeremyjordan.me/semantic-segmentation/))——[FCN](https://arxiv.org/abs/1411.4038)、[塞格内](https://arxiv.org/abs/1511.00561) t、 [UNet](https://arxiv.org/abs/1505.04597) 、 [PSPNet](https://arxiv.org/abs/1612.01105) 、 [DeepLab](https://arxiv.org/abs/1606.00915) 、 [ICNet](https://arxiv.org/abs/1704.08545) 、 [ENet](https://arxiv.org/abs/1606.02147)**
*   **实例分割。([博客](/deep-learning-for-ship-detection-and-segmentation-71d223aca649) ) — [Mask-RCNN](https://arxiv.org/abs/1703.06870) ， [YOLACT](https://arxiv.org/abs/1904.02689)**

**希望这篇文章能为你学习计算机视觉提供必要的资源。如果你认为添加更多的东西可以使这篇文章更好，更值得一读，我随时欢迎你的建议。关注我们的[媒体](https://medium.com/@AnveeNaik)阅读更多此类内容。**

***成为* [*介质成员*](https://medium.com/@AnveeNaik/membership) *解锁并阅读介质上的许多其他故事。***