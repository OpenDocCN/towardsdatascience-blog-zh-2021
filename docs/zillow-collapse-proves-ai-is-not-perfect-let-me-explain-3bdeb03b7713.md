# Zillow 崩溃证明 AI 并不完美:让我解释一下

> 原文：<https://towardsdatascience.com/zillow-collapse-proves-ai-is-not-perfect-let-me-explain-3bdeb03b7713?source=collection_archive---------7----------------------->

## [社区笔记](https://pedram-ataee.medium.com/list/notes-on-community-cc93416f5a13)

## 我从 Zillow 崩溃的故事中学到的两个教训

![](img/eb567cdc5976fa8f1b862af02a4a815b.png)

由[杰里米·贝赞格](https://unsplash.com/@jeremybezanger?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

最近的疫情导致我们周围的许多事情发生变化，包括房地产市场。房地产市场的剧烈变化导致用于预测房价的机器学习模型不再有效。这就是 Zillow 崩溃的原因，主要是因为他们过于依赖人工智能算法来预测房价。

Zillow 的房屋交易部门在他们承认他们的房价预测算法(也称为 [Zestimate](https://www.zillow.com/z/zestimate/?utm_campaign=The%20Batch&utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--BLcGdrnQNkKoFecXVa1Cpckmz_Su-3IHByaQKd9k_sy0_RSR8Dtr-x4nuefSVtf5wtg9R) )无法再为他们的业务创造价值后彻底关闭。在疫情之后，该算法无法以足够的精度预测房价，从而让他们更好地决定买卖房产。该算法仍然为在线用户提供房价的猜测。**问题是“明知失败，还会用这些结果吗？”**

[房价预测](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)是数据科学应用的经典例子之一，仅次于“[预测泰坦尼克号乘客的生存](/predicting-the-survival-of-titanic-passengers-30870ccc7e8)”或“[识别 MNIST 数据集上的手写数字](https://machinelearningmastery.com/how-to-develop-a-convolutional-neural-network-from-scratch-for-mnist-handwritten-digit-classification/)”。Zillow 崩溃不能将房价预测从经典的机器学习挑战中剔除；然而，它提醒我们人工智能解决方案可能会失败和工作。在这篇文章中，我想分享两个值得吸取的教训。

# —历史不押韵，AI 就失败了。

历史不会重复自己，但经常押韵。当它押韵时，它可以被 AI 算法预测。在这个语境中，预测指的是房屋在几个月的范围内，装修后的未来价格。我们在过去两年中经历的劳动力市场或供应链的波动，使得预测与疫情之前相比几乎是不可能的。

人工智能是预测的有力工具；然而，它只有在未来建立在过去的基础上时才起作用。发生在疫情之后的事情以前没有发生过；所以依赖历史数据的人工智能算法可能不会很好地工作。每当问题中出现时间因素时，我们必须更加小心。这就是为什么估计比预测更容易。在这里，估价是指给定一系列特征，如位置、大小和建筑年份，房屋的当前价格。

> 历史不会重复自己，但经常押韵。当它押韵时，它可以被 AI 算法预测。

由于其他挑战，您可能无法使用人工智能为商业案例创造价值。你可以阅读下面的文章，了解如何评估在商业案例中使用人工智能的可行性。

<https://medium.com/swlh/if-you-consider-using-ai-in-your-business-read-this-5e666e6eca23>  

# —系综，先是神经网络，后来。

集成方法在需要快速结果和持续改进的行业中非常有用，例如在开发的早期。当你展示人工智能如何为企业创造价值时，必须使用更复杂、性能更高的方法，如神经网络。我听说过一些失败的故事，高管们在早期要求开发神经网络模型，而人工智能模型的价值尚未得到证明。**坚持在早期建立神经网络模型会增加团队失败的几率。**为什么？主要是因为很难对他们进行适当的培训，或者公司内部还不了解人工智能引擎的价值(不考虑实现技术)。要阅读另一个关于系综模特的成功故事，你可以阅读下面的文章。

</the-story-of-an-ensemble-classifier-acquired-by-facebook-53eeaa5b5a97>  

Zestimate 是预测房价的 Zillow 算法，最初由大约 1000 个模型组成。你现在不奇怪了！在他们能够用他们的人工智能算法创造价值后，他们转向神经网络模型。新方法将算法误差减少了约 10%,同时为更频繁地更新模型创造了机会。Zillow 的数据科学团队甚至已经开始探索其他人工智能技术，如文档理解或自然语言处理，直到它们因疫情而失败。最后一点是，当问题由于大数据漂移而变得无法解决时，无论您使用什么技术都无关紧要。这不仅仅是可解的！

> 坚持在早期建立神经网络模型会增加团队失败的几率。

# 临终遗言

简而言之，我学到的教训是:“当历史不押韵时，人工智能就失败了。”以及“先整体，后神经网络”。我想听听你对这件事的看法。这件事对你如何看待人工智能的力量有影响吗？请在这里分享你的看法。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

<https://pedram-ataee.medium.com/membership> 