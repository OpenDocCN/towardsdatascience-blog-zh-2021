# Expire-Span:忘记无关信息的新人工智能算法

> 原文：<https://towardsdatascience.com/expire-span-the-new-ai-algorithm-that-forgets-irrelevant-information-61da9b442237?source=collection_archive---------51----------------------->

## 人工智能新闻

## 脸书提出的最新人工智能算法的快速描述

![](img/62445584ad29d042019b71fa2c1fe565.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=426559)的 Gerd Altmann 提供

5 月 14 日，AI 脸书研究团队发表了一篇关于 Expire-Span 的[文章](https://ai.facebook.com/blog/teaching-ai-how-to-forget-at-scale/)，Expire-Span 是一种类似人脑的算法，它*学会忘记*不相关的信息。在[主论文](https://scontent-fco1-1.xx.fbcdn.net/v/t39.8562-6/185356217_1177665109329269_6669883335010742565_n.pdf?_nc_cat=111&ccb=1-3&_nc_sid=ae5e01&_nc_ohc=OdO7NshGxz8AX8SFcfq&_nc_ht=scontent-fco1-1.xx&oh=0f0c34161a7388479391d3e0e8457892&oe=60D0371E)中，作者提出:

> Expire-Span，一种学习保留最重要信息，让无关信息过期的方法。

# 到底是什么意思？

在实践中，Expire-Span 是一种深度学习算法，它首先预测与给定任务最相关的信息，然后为每条信息配备一个截止日期，即截止日期。当日期到期时，相关信息被遗忘。

***一个信息越相关，有效期越长。***

这一点使得 Expire-Span 算法在内存方面非常具有可伸缩性。根据作者的说法，Expire-Span 的准确性令人难以置信地超过了最初的 Transformer-XL 算法，它是从该算法中派生出来的。

# 为什么 Expire-Span 效果更好？

Expire-Span 是一种神经网络，它试图像人脑一样工作，人脑只记得有用的信息，忘记其余的信息。实际上，Expire-Span 只记住执行任务所需的信息。

***基本思想是为了执行任务，只需要它的上下文。***

# Expire-Span 是如何工作的？

想象一下，一个人工智能代理必须学会如何到达一扇黄色的门，沿着一条走廊，那里还有其他的门。许多现有的神经网络存储了寻找黄门所需的所有中间信息和框架。相反，Expire-Span 只存储第一帧，其中有任务描述(即找到黄色的门)。因此，所有中间帧都可以被遗忘，因为它们存储了不相关的信息，即学习如何找到黄色门所需的步骤。

这允许 Expire-Span 更有效地利用内存。

脸书也发布了 Python 代码来文本 Expire-Span。因此，如果你想，你可以玩它:)

# 摘要

在这篇简短的文章中，我简要介绍了脸书开发的最新人工智能算法 Expire-Span。

Expire-Span 依赖的概念是，为了降低内存成本，神经网络必须只保留相关信息。这是通过给每条信息分配一个截止日期来实现的，*就像一瓶牛奶的截止日期一样。*