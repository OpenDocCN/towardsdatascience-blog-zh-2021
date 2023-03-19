# 数据科学家应该关心新的 M1 Pro 和 M1 Max MacBook Pro 吗？

> 原文：<https://towardsdatascience.com/should-data-scientists-care-about-the-new-m1-pro-and-m1-max-macbook-pro-cb28988725a9?source=collection_archive---------2----------------------->

## 苹果发布了新款 Macbook Pro。这是它对数据科学的意义。

![](img/f004a2884d44f863e9bc6ee1ba064a3a.png)

韦斯·希克斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你可能知道，苹果发布了新的 MacBook Pro，有 M1 Pro 和 M1 Max。它们应该比最初的 M1 更快更强大，但这对数据科学家来说意味着什么呢？

我很高兴拥有一台配有原版 M1 的 Mac 电脑，我不得不说它的能力令人印象深刻——我甚至无法想象如果有一台配有 M1 Pro 或 M1 Max 的新 MacBook Pro 会有多好。

也就是说，如果您经常使用数据科学中的工具，那么在购买最新的计算机、迁移甚至更新您的操作系统时，您应该三思而行(在更新到 macOS Catalina 并突然失去 Anaconda 之后，我得到了这个惨痛的教训)

让我们看看是否值得购买新的 MacBook。

# 数据科学的当前和(可能的)未来优势

M1 Pro 和 Max 芯片的一些优势是高单线程性能。[M1 Max 的分数显示单线程分数在 1，700 到 1，800 之间，多线程分数在 11，000 到 12，000 之间](https://www.macworld.com/article/545435/intel-m1-pro-m1-max-cpu-gpu-performance.html)(这比最快的英特尔笔记本电脑高出 15%)。此外，配备 M1 Pro 和 Max 的 MacBooks 电池续航时间也非常出色。

也就是说，这些并不是数据科学的独有优势。事实是，最初的 M1 MAC 没有提供任何特别有利于数据科学家的东西，新的 M1 Pro 和 M1 Max 芯片也没有太大变化(我们将在下一节检查技术规格)。

幸运的是，据 [Anaconda 网站](https://www.anaconda.com/blog/apple-silicon-transition)报道，未来可能会有一些即将到来的好处。

> 最令人兴奋的发展将是机器学习库可以开始利用苹果硅上的新 GPU 和苹果神经引擎核心。苹果提供了像 Metal 和 ML Compute 这样的 API，可以加速机器学习任务，但它们在 Python 生态系统中没有得到广泛应用。苹果有一个 TensorFlow 的 alpha 端口，它使用 ML Compute，也许未来几年其他项目也能利用苹果的硬件加速。
> [——斯坦利·塞伯特(Anaconda)](https://www.anaconda.com/blog/apple-silicon-transition)

# 数据科学 M1 的利与弊

M1 Pro 和 Max 的资源使用效率很高，但它适合数据科学活动吗？

*   高性能内核:新的 M1 Pro 和 M1 Max 支持[八个高性能和两个低功耗内核](https://www.engadget.com/m-1-max-pro-announced-upscaled-131531800.html)。TensorFlow 和 PyTorch 等数据科学库受益于更多的 CPU 核心，因此从原来 M1 的 4 个高性能 CPU 核心升级到新 M1 Pro/Max 的 8 个，对于执行数据科学任务来说肯定是好的。
*   ARM64 包可用性:目前，在新 MAC 上运行 Python 的最佳选择是通过 Rosetta 2。然而，这个选项无助于在 M1 MAC 电脑上运行 Python。谁知道 PyData 生态系统需要多少时间才能赶上苹果芯片。(请记住[与原生 ARM64](https://www.anaconda.com/blog/apple-silicon-transition) 相比，使用 Rosetta2 运行 x86–64 程序时，人们会看到 20–30%的性能损失)
*   内存:配备 M1 Pro 的 MacBook Pro 配备 16GB 内存，而配备 M1 Pro Max 的 MacBook Pro 配备 32GB 内存。自然，当处理大型数据集时，更多的内存总是有帮助的，所以如果你已经有一台支持 8-16GB 内存的原始 M1 的 Mac，它可能值得升级到 M1 Pro Max。

# 我作为 M1 Mac 所有者的经历

目前，我有一台 Mac mini (M1，2020)，它对我来说很好。使用 Rosetta2 运行 x86–64 程序时，我不介意 20–30%的性能损失；不过，如果我能原生运行 Python 就更好了。

不过，我不得不说，在新的 Mac 机型上设置带有数据科学和非数据科学包的 Python 简直是一场噩梦。然而，当涉及到数据科学时，有两个简单的解决方案来设置 Python——使用 anaconda 或 miniforge。

如果你是那些已经购买或打算购买配备 M1 Pro 和 M1 Max 的新 MacBook 的幸运儿之一，[查看本指南](/how-to-easily-set-up-python-on-any-m1-mac-5ea885b73fab)轻松设置 Python(或观看下面的视频)。

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。每月 5 美元，让您可以无限制地访问数以千计的 Python 指南和数据科学文章。如果你使用[我的链接](https://frank-andrade.medium.com/membership)注册，我会赚一小笔佣金，不需要你额外付费。

<https://frank-andrade.medium.com/membership> 