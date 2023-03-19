# Pytorch vs Tensorflow 2021

> 原文：<https://towardsdatascience.com/pytorch-vs-tensorflow-2021-d403504d7bc3?source=collection_archive---------2----------------------->

## PyTorch (1.8)和 Tensorflow (2.5)最新版本的比较

![](img/f71fc24aca98ea251dc84764a4d71564.png)

Vanesa Giaconi 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Tensorflow/Keras & Pytorch 是目前最受欢迎的两个主要机器学习库。Tensorflow 由 Google 维护和发布，Pytorch 由脸书维护和发布。在本文中，我想从以下几个方面对它们进行比较:

*   最新发布版本中的新增功能
*   使用哪一个？为什么(基于 2 年的 ML 项目)

## Tensorflow 2.x:

Tensorflow 1 和 Tensorflow 2.x 之间有许多变化，我将尝试找出最重要的变化。

第一个是 **Tensorflow.js 的发布**随着 web 应用程序越来越占主导地位，在浏览器上部署模型的需求已经增长了很多。使用 Tensorflow.js，您可以使用 Node 在浏览器中运行现有的 python 模型，重新训练现有的模型，并完全使用 Javascript 构建&训练您自己的模型(您不需要 python)。

Tensorflow 2.x 的另一个版本是 **Tensorflow Lite，**一个“在移动和嵌入式设备上部署模型的轻量级库”。这是有意义的，因为移动网络应用是两种最主要的应用类型。使用 Tensorflow Lite，您可以简单地将现有模型转换为“压缩平面缓冲区”,并将该缓冲区加载到移动设备或任何其他嵌入式设备中。发生的主要优化过程是将 32 位浮点转换成更适合嵌入式设备的 8 位浮点(占用更少的内存)。

最终的主要版本是 **Tensorflow Extended (TFX)** ，这是一个用于部署生产 ML 管道的端到端平台。我不得不承认，他们在确定机器学习的 3 个最重要的领域(网络应用、移动应用&生产管理)方面做得很好。ML 生产管道仍然需要大量的研究和开发。TFX 帮助解决传统的软件生产挑战，如可伸缩性、可维护性和模块化。此外，它有助于应对机器学习的特定挑战，如持续在线学习、数据验证、数据管理等。

**Pytorch 1.8:**

与 Tensorflow Lite 类似，Pytorch 也改进了他们现有的 **Pytorch Mobile** 。一个框架可以量化、追踪、优化和保存 Android 和 iOS 的模型。他们还发布了 Pytorch Lite 解释器的原型，减少了移动设备上的二进制运行时大小。

此外，他们还通过更具体的错误处理和流水线并行性，增加了对分布式培训的更多支持。最后，他们推出了 **Pytorch Profiler** ，这是一款用于调试和排除大规模深度学习模型故障的工具。

虽然 Pytorch lightning 不是 Pytorch 1.8 的一部分，但我认为它值得一提。 **Pytorch lightning** 已经发布，使神经网络编码更加简单。你可以把它想象成 Pytorch 的 Keras。它已经获得了很大的吸引力。我认为值得一提的原因是，Keras 一直在显著改进 Tensorflow，因为它使实现模型变得更加容易和简短。Pytorch 闪电对 Pytorch 做了同样的事情。

## 用哪个？

![](img/86258beb2e2bb6cad0d4bb92a690a0e3.png)

[迈特·沃尔什](https://unsplash.com/@two_tees?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

每个人心中的下一个问题是使用哪一个。在过去的 2 年里，每当我开始一个机器学习项目时，我都在想这个问题。我会给你一个快速反映我的经验，以及如何选择它们的提示。

本质上，这两个库都非常好，它们在性能和提供的特性上非常接近。总的来说，你必须意识到这两者在编码风格上是有区别的，当你在进行机器学习项目时，这实际上是有区别的。

Pytorch 以其 OOP(面向对象编程)风格而闻名。例如，当您创建自定义模型或自定义数据集时，您很可能会创建一个继承默认 PyTorch 库的新类，然后修改您自己的方法。就我个人而言，我不是 OOP 的忠实粉丝。尽管它以某种方式为代码提供了一种结构，但就代码行数而言，它使实现变得更长。

另一方面，当您使用 Tensorflow 时，您很可能会使用 Keras。在进行 Kaggle 竞赛(监督图像分类、对象检测、图像分割、NLP 等)时，我几乎总是发现 Keras 的实现比 Pytorch 的短。这对于初学者/中级人员来说是非常好的，因为您不必花费大量时间阅读和分解代码行。

在某些情况下，你会在一个特定的机器学习子领域中寻找一个特定的模型。根据我的经验，一个非常有用的提示是，在这种情况下，您很可能只在其中一个库中找到对该模型的更多支持。原因很简单，因为它是在那里第一次实现的，教程也是从第一次实现开始堆积的。在这种情况下，只要图书馆有更多的支持，因为它会让你的生活更容易。例如，当我在做一个对象检测比赛，我想实现 DETR(脸书的数据高效转换器)，我发现的大多数资源都是用 Pytorch 编写的(显然)，因此在这种情况下使用 Pytorch 要容易得多。

此外，最后一个反思是，我发现 Pytorch 的实现更长，因为它们似乎覆盖了许多底层细节。这既是优点也是缺点。之所以说是 pro，是因为当你是一个初学者的时候，最好先学习那些底层的细节，然后再转到 Keras 等更高级的 API。然而，这是一个缺点，因为你会发现自己迷失在许多细节和相当长的代码中。所以本质上，如果你的工作期限很紧，最好选择 Keras 而不是 Pytorch。

## 最后的想法

我希望你喜欢这篇文章，并发现我的提示很有帮助。我知道可能会有一些关于它们的不同意见，但这仅仅是因为人们对这两个库有不同的体验。但是，我总是发现初学者/中间用户在使用这两个库时有非常相似的经历。由于这两个库在性能上非常相似，我认为选择哪个库可能只是一个简单的偏好问题。

如果你想了解更多关于 TensorFlow 的知识，我建议在这里购买这本书[。我完整地阅读了这本书，它极大地帮助了我的 ML 知识。另外，请注意，这是一个亚马逊联盟链接，如果你买这本书，我会得到佣金。我只是推荐这本书，因为我已经买了它，我学到了很多非常有用的概念，帮助我找到了一份新工作。](https://www.amazon.co.uk/gp/product/1492032646/ref=as_li_tl?ie=UTF8&tag=mostafaibrahi-21&camp=1634&creative=6738&linkCode=as2&creativeASIN=1492032646&linkId=75417accfbb7c4f03156af126a652058)

如果你想定期收到关于人工智能和机器学习的最新论文的评论，请在这里添加你的电子邮件并订阅！

https://artisanal-motivator-8249.ck.page/5524b8f934