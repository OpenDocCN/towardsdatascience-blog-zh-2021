# 为什么边缘计算对人工智能如此重要

> 原文：<https://towardsdatascience.com/why-edge-computing-is-so-important-for-ai-e4695d4e7960?source=collection_archive---------36----------------------->

每个人都在谈论人工智能。当你在手机上问 Siri 今天天气如何时，你会看到它；当你需要到达目的地的最佳路线时，你会在谷歌地图上看到它；当你在为你推荐的产品之间滚动时，你会在亚马逊上看到它。

![](img/9b7691e0e78a0d45d383ee93a0c5d992.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)拍摄

然而，人工智能并不是独立存在的。我们通常使用底层技术来实现它。就在这些天，范式正在离开一种进化，这种进化将把人工智能带到一个更安全、更便宜的地方。让我们看看是关于什么的。

# 并非所有闪光的都是金子

虽然人工智能的势头越来越大，但云计算已经成为其发展的核心部分。然而，这种方法具有挑战性，因为基于云的人工智能系统在数据中心运行其核心人工智能算法。设备和数据中心之间持续的**信息传输**是一种可行的方法，但对于一个强大的人工智能系统的集成来说，它固有地涉及一些问题。

![](img/65d5b3913312c3fcba3bd52c895896ec.png)

照片由 [Denys Nevozhai](https://unsplash.com/@dnevozhai?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**延迟**是下一个问题。使用远程数据中心的产品必须发送数据，并在接收结果之前等待数据被处理。由于互联网连接不是即时的，因此总会有最小的延迟，这种延迟会因流量而异。此外，随着网络上用户数量的增加，系统的延迟也会增加。这个问题可能会导致产品无法正常响应。

云计算的中心地位带来了另一个问题。**敏感信息**被发送到一个未知位置，该位置可能会被未授权人员访问。亚马逊的 Alexa 可能会在客户不知情或不同意的情况下记录对话并存档，从而使许多有权访问数据或人工智能系统的亚马逊员工能够访问它们。

# 我想在边缘上做！

边缘计算是正义的捍卫者，穿着闪亮盔甲的骑士。凭借其超能力，Edge 解决了人工智能在云上产生的所有问题。

边缘计算将 AI 的执行从数据中心转移到设备上。虽然训练通常不在本地执行(因为这是一项复杂且昂贵的任务，并且仍然需要使用云资源)，但是模型的推理可以在本地执行**。**

**这也立即解决了延迟问题。AI 模型在数据可用时被处理，这可以显著减少执行时间。边缘计算无需等待设备建立互联网连接、发送数据、等待数据中心处理数据并将结果返回给设备以获得响应，而是在本地执行整个过程，**减少了互联网连接需求**，从而减少了延迟。**

**边缘计算的使用也将潜在的**敏感信息保存在本地设备**上，因为数据中心并不用于处理信息。**

**![](img/9633e44fcd601a6209c086ef0482369c.png)**

**埃斯特万·洛佩兹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片**

**然而，所有的超级英雄都有弱点。虽然氪星石削弱了超人和超女，但边缘设备的计算能力惊人地有限。**

**幸运的是，大玩家已经在构建具有良好计算能力的边缘设备方面做了大量工作，而这仅仅是个开始！**

# **单身但从不孤独**

**![](img/7411d0f44c71584d4bbefe8489a1a015.png)**

**由 [Vagelis Lnz](https://unsplash.com/@lnz_uk?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

**让我通过提及一些具有巨大人工智能潜力的小型计算机来结束这场讨论。NVIDIA Jetson Nano 和 Raspberry Pi 4 是两款功能极其相似的单板电脑(SBC)。虽然它们采用 ARM 处理器，分别采用四核 ARM Cortex-A72 64 位@ 1.5 GHz 和四核 ARM Cortex-A57 64 位@ 1.42 GHz，但这两种主板之间的最大区别是 NVIDIA Jetson Nano 包括更高性能、更强大的 GPU。NVIDIA 主板包括一个 Maxwell 微体系结构，具有 128 个 CUDA 内核@ 921 Mhz。CUDA(Compute Unified Device Architecture 的缩写)是英伟达创建的并行处理硬件架构，软件开发人员可以在其上编写能够在英伟达显卡的 GPU 上执行并行计算的应用。这被证明是人工智能计算的一个巨大优势，尤其是在进行推理方面。**

# **光明的未来**

**边缘计算正迅速成为人工智能的颠覆性工具，几乎成为事实上的标准。因此，由于可穿戴计算的推动，单板计算机每天都有新的惊人功能诞生。这种变化会把我们带向何方？TinyML 就在眼前，用不了多久它就会变得更加普及。**