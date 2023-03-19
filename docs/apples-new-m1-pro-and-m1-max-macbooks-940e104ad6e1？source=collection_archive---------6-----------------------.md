# 苹果新的 M1 Pro 和 M1 Max MacBooks

> 原文：<https://towardsdatascience.com/apples-new-m1-pro-and-m1-max-macbooks-940e104ad6e1?source=collection_archive---------6----------------------->

## 使用它们进行机器学习时，要做好学习的准备

![](img/863f7104db71cc50b51f7f09fb764a0b.png)

由 [Ales Nesetril](https://unsplash.com/@alesnesetril?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

规格确实令人生畏:多达 32 个 GPU 核心和多达 16 个 CPU 核心。再加上 64 GB 的 RAM，您就可以轻松应对任何工作负载了。还有，别忘了设计。嗯，看来苹果又来了。

但是，他们真的这样做了吗？让我们再深入一点。就在一年前，M1 芯片亮相，这是苹果公司的第一个定制的片上系统(SoC)。那时候最多可以装 16GB 的主存。并且，在 MacBook Pro 中，这个芯片有 8 个 CPU 和 8 个 GPU 核心。与最新的模型相比，这听起来很小，不是吗？

答案是:看情况。我经常在工作中使用 M1 的 MacBook Pro，我发现唯一的限制是小内存。如果让我再选这台机器，32g 甚至 64 GB 都有选项，我肯定会选。

但是，即使有这些令人印象深刻的规格，我们也不要忘记我们必须为道路提供动力。对我们来说，这主要意味着进行机器学习任务，如训练网络或探索数据集。这就是麻烦的开始。

是的，M1 芯片既快又节能。但是通常情况下，从 pip 安装 python 包是很麻烦的。安装 TensorFlow 是**而不是**运行那么简单

当我尝试这样做时，我花了三天时间让系统启动并运行。尽管苹果提供了一个专用的 TensorFlow 版本，但要让它运行起来还是很有挑战性。

令人欣慰的是，当他们宣布[支持 TensorFlow 的 PluggableDevice 机制](https://blog.tensorflow.org/2020/11/accelerating-tensorflow-performance-on-mac.html)时，这一点得到了改善，该机制旨在[改善对不同加速器](https://blog.tensorflow.org/2021/06/pluggabledevice-device-plugins-for-TensorFlow.html) (GPU、TPU、定制 SOC 等)的支持。随着该版本的发布，事情变得更加顺利——至少在让框架运行方面。

一年后，当我试图从 pip 本地安装包时，仍然经常遇到麻烦。是的，使用 Anaconda 使事情变得更容易。但是，一旦您准备部署您的脚本，您可能希望冻结需求并在远程环境中使用相同的设置。然后，您必须处理两个需求文件:一个来自 Anaconda，另一个来自 pip。

那么，在所有这些感叹之后，有什么解决办法呢？**第一个也是最简单的解决方案:**使用 Linux 机器，配备久经考验的 AMD 或 Intel CPU，并与专用 GPU 配对。

**第二种解决方法:**拥抱学习。是的，我花了三天时间让 TensorFlow 开始工作。是的，我绕了很长的路去安装想要的包。但是，由于我被 M1 芯片卡住了，我没有其他选择，只能经历这一切。

我学到了很多东西。我自己钻研编译包。我开始使用蟒蛇。我深入研究了皮普。我在 GitHub 上滚动了几个小时，了解到写简洁问题的重要性。

所以，我会推荐 M1 的机器学习芯片吗？可以，但是你要做好学习的准备。不像即插即用那么简单。但这也不是火箭科学。

编辑:自从我写了这篇文章，我有机会测试新的 MacBooks，包括“小”的 14 英寸和“大”的 16 英寸。引起我注意的是，大版本比 14 英寸和去年的 13 英寸厚得多。它也更重(官方规格说明)。鉴于此，我更喜欢我的 13 英寸版本，因为它的便携性和重量。

如果可以的话，我建议你在花两千或更多钱之前，先去当地的商店体验一下。这样，你会更好地注意到我指的是什么。