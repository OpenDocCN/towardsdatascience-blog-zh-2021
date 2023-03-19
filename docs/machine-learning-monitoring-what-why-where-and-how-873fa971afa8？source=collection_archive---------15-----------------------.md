# 机器学习监控——什么、为什么、在哪里以及如何？

> 原文：<https://towardsdatascience.com/machine-learning-monitoring-what-why-where-and-how-873fa971afa8?source=collection_archive---------15----------------------->

## 确保你有一个健康的模型做出正确的预测

*本文正被移至我的* [*子栈发布*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读文章* [*。这篇文章将于 2022 年 7 月 18 日被删除。*](https://vishalramesh.substack.com/p/machine-learning-monitoring-what?r=9u6n7&s=w&utm_campaign=post&utm_medium=web)

您已经部署了您的模型，它现在正在处理请求并根据实时数据进行预测。太好了！但是你还没有完成。像任何其他服务一样，您需要监控您的模型。但是监控一个模型并不等同于监控任何其他服务。本文将尝试解释为什么需要监控模型，需要监控什么指标，监控在机器学习生命周期中的位置，以及如何开始监控模型。

# 为什么我们需要监控？

![](img/de0342cc2bb895c953e36484bc94bb5f.png)

由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

任何在生产环境中运行服务的人都需要受到监控，以确保服务是健康的(运行和处理请求时不会出现意外错误)。

但是机器学习监控就不一样了。一个模特发球可以不发出任何声音就失败。该服务将启动、运行并处理请求。当预测质量变差时，就会发生这种情况。

但是为什么会这样呢？在将模型投入生产之前，我们训练它具有良好的准确性。这是因为数据会随着时间“漂移”。模型被训练的数据的分布最终可能与它在生产中遇到的不同。有时模型也可能变得陈旧。当模型的环境发生变化时，就会发生这种情况。

我们不久前见过这个。当新冠肺炎和洛克德斯风靡全球时，世界各地人们的行为和购物模式都发生了变化。这意味着电子商务和类似网站使用的模型遇到的数据与它们接受的训练非常不同。这意味着他们可能做出不太相关的预测和建议。

数据漂移和陈旧的模型是我们需要机器学习监控的众多原因之一。使机器学习监控变得必不可少的一些其他原因是-数据依赖性的失败、功能不可用和负反馈循环。

# 您应该监控哪些指标？

有两种方法来进行机器学习监控-设置仪表板进行实时监控，并让作业定期运行以计算必要的指标。在这两种情况下，您监控的指标通常是相同的。

*本文其余部分已移至出版物* [*机器学习——科学、工程和 Ops*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读整篇文章* [*。*](https://vishalramesh.substack.com/i/52123764/what-metrics-should-you-monitor)