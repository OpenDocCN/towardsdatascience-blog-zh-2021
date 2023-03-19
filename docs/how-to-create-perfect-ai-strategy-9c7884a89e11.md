# 如何打造完美的 AI 战略

> 原文：<https://towardsdatascience.com/how-to-create-perfect-ai-strategy-9c7884a89e11?source=collection_archive---------34----------------------->

## [人工智能笔记](https://pedram-ataee.medium.com/list/notes-on-ai-84e1081cf2dd)

## 深入了解人工智能战略的三大支柱

![](img/41afe1d2f0e4f0c6efa7cefbcdefc3e7.png)

[工作室 Kealaula](https://unsplash.com/@studiokealaula?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

大多数技术都是从行业内的大肆宣传开始的，并逐步失去人们的关注，因为它们在解决现实世界的问题时不能满足我们的期望。人工智能是仅有的几项达到并超过预期的最新技术之一。然而，如果我们在追求人工智能发展方面没有一个坚实的战略，这项技术可能会像其他技术一样最终走向相同的终点。人工智能战略有三个主要支柱:**可行性**、**性能**和**可扩展性**。如今，每个人都在谈论人工智能模型的潜力，但我想指出为了创造一个完美的人工智能战略必须仔细研究的问题。有了坚实的人工智能战略，你就增加了构建人工智能产品的成功几率。出于同样的目的，我最近写了一篇关于如何创建完美数据策略的文章。你可以在下面找到。

[](/how-to-create-a-perfect-data-strategy-7e8fd9bbfad0) [## 如何创建完美的数据策略

### 通过实施创建数据策略的最佳实践

towardsdatascience.com](/how-to-create-a-perfect-data-strategy-7e8fd9bbfad0) 

# —可行性

要构建和推出一个人工智能产品，你会遇到许多挑战。如果你认为在人工智能世界里一切都是可行的，你可能会很容易失望。许多公司开始开发一种人工智能产品，在中途，发现它对他们不起作用。这造成了失望，更重要的是，阻止了他们正确使用人工智能并为企业创造价值。强烈建议在开始实施人工智能产品之前与人工智能专家进行头脑风暴，并以你的人工智能任务为短期目标。

例如，在一些应用中，必须在设备级别(也称为边缘人工智能)实现人工智能模型，以遵守数据隐私法案。当 AI 模型在最接近数据收集的点实现时，隐私问题被最小化。然而，在这个级别实现人工智能模型会产生许多问题。例如，你在这个层次上没有强大的计算能力。或者，你不能很容易地在那些与 edge AI 兼容的语言(如 C)上实现你用高级编程语言(如 Python)开发的东西，因为所需的库不存在。再比如，你开始为一个应用开发深度学习模型。然后，你发现所需的计算能力或深度学习专家对你来说是不可及的，或者不是以一种经济高效的方式。在那一点上你必须三思。你可以阅读更多关于大公司在分析构建人工智能产品的可行性时所犯的错误。

[](https://pub.towardsai.net/ai-strategy-a-battlefield-that-even-big-firms-struggle-539a9f3df396) [## 人工智能战略——一个连大公司都在努力的战场

### 深入了解方舟投资公司创造的 2021 年大创意

pub.towardsai.net](https://pub.towardsai.net/ai-strategy-a-battlefield-that-even-big-firms-struggle-539a9f3df396) 

> 要创建一个完美的 AI 策略，你必须有一个计划来保证技术上的可行性。为实现一个强大的人工智能模型的复杂性做好准备。

# —性能

您可能会说“在文献中存在许多标准的度量标准来帮助人们评估模型的质量。因此，人工智能模型的性能很容易衡量。”不幸的是，我必须说这并不完全正确。一个主要问题是，您只能在您收集的数据范围内衡量性能。换句话说，您必须始终处理样本外错误(也称为泛化错误)。**没人能保证你的用户数据是什么样的。**你可以尽最大努力使用与真实世界数据高度相似的数据集来评估 AI 模型；然而，你总是会惊讶于从未预料到的新情况。

更重要的是，大多数时候(如果不是总是)你不能保证一个人工智能模型的最差性能。存在许多方法来减少概括误差；然而，他们不太成功地给它划定界限，特别是在深度学习模型等灰箱模型中。此外，输入数据中的小扰动可能会导致结果不稳定，这在许多使用情况下是不可接受的。你可以在下面的文章中了解更多关于如何管理人工智能模型的性能。

[](https://pedram-ataee.medium.com/why-experiment-management-is-the-key-to-success-in-data-science-b286aaa4700d) [## 为什么实验管理是数据科学成功的关键

### 如果你认识到它的优点，你今天就会采用它。

pedram-ataee.medium.com](https://pedram-ataee.medium.com/why-experiment-management-is-the-key-to-success-in-data-science-b286aaa4700d) 

> 要创建一个完美的 AI 策略，你必须有一个评估 AI 模型的计划。超越标准指标思考！

# —可扩展性

为试点项目建立人工智能模型很容易。然而，当你决定建立一个人工智能产品时，它变得更加困难，当你决定扩展它时，它变得更加困难。帕累托原则(也叫 80-20 法则)在这里也是有效的。一般来说，你可以用 20%的努力建立一个符合你 80%预期的 AI 模型。剩下的 20%的期望将会很难实现，这会让你震惊，尤其是当你想要扩展产品的时候。

例如，可伸缩性挑战源于收集大量干净且有标签的数据，并构建一个可以解决大多数用例的通用模型。人工智能模型的质量与其数据的质量一样好。因此，你必须确保用于训练人工智能模型的数据集是全面的，并涵盖大多数用例。实际上，训练数据集并不是普遍完整的。这就是为什么在构建人工智能模型时，你总是会遇到偏差-方差的权衡。例如，这种权衡基本上迫使您降低当前数据集中的精度，同时希望提高尚未收集的数据集中的精度。

最后，但同样重要的是，您经常需要构建各种上下文化的模型来处理不同的上下文。用一个模型来处理各种不同的环境通常是不可能的。这就是为什么 AI 聊天机器人必须为每个行业专门设计才能使用。你可以在下面的文章中阅读更多关于帮助我们管理情境化模型的人工智能模型的信息。

[](/word2vec-models-are-simple-yet-revolutionary-de1fef544b87) [## Word2Vec 模型简单而具有革命性

### Gensim 还是 spaCy？不了解 Word2Vec 机型的基础知识也没关系。

towardsdatascience.com](/word2vec-models-are-simple-yet-revolutionary-de1fef544b87) 

> 为了创造一个完美的人工智能战略，你必须有一个计划来扩大人工智能模型的规模。请注意，达到您预期的 80%并不意味着您有一个案例！

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我的* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium-Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)