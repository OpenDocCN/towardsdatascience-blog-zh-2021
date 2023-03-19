# 特色商店——什么、为什么、在哪里以及如何？

> 原文：<https://towardsdatascience.com/feature-stores-what-why-where-and-how-17cd960c394c?source=collection_archive---------18----------------------->

## “功能商店”这个术语最近已经流传很久了。这篇文章试图阐明这个话题。

![](img/3d4b304db177f8e757c19878a9b5918e.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*这篇文章正被移到我的* [*子栈发布*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读文章* [*。这篇文章将于 2022 年 7 月 18 日被删除。*](https://vishalramesh.substack.com/p/feature-stores-what-why-where-and?r=9u6n7&s=w&utm_campaign=post&utm_medium=web)

但是在我们去特色商店之前，

# 什么是特性？

特征是作为模型输入的独立属性。考虑模型

*y = f(x)*

这里 **x** 是你的输入向量， **y** 是你的输出向量， **f** 是你的模型。 **x** 中的每一列都是您的型号的特征。机器学习模型从特征中学习，并在训练期间更新其参数，以便能够对输出做出良好的预测。

现在，

# 什么是功能商店？

我是在参加 [apply()](https://www.applyconf.com) 的时候被介绍到特色店的。简而言之，要素存储是存储要素的数据存储。听起来很直接，对吗？那么，为什么不直接创建一个 SQL 或 BigTable 数据库并完成它呢？是什么让特色店如此特别？嗯，实现是它的特别之处。因此，让我们看看为什么我们需要功能商店来理解这一点。

# 为什么选择特色商店？

为了理解我们为什么需要特征存储，我们需要知道机器学习生命周期的不同阶段是什么。下面列出了它们。

*本文其余部分已移至出版物* [*机器学习—科学、工程和 Ops*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读整篇文章* [*。*](https://vishalramesh.substack.com/i/52123103/why-feature-stores)