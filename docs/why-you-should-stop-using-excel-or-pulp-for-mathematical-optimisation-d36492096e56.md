# 为什么你应该停止使用 Excel 或纸浆进行数学优化

> 原文：<https://towardsdatascience.com/why-you-should-stop-using-excel-or-pulp-for-mathematical-optimisation-d36492096e56?source=collection_archive---------16----------------------->

## 给古罗比一个机会

![](img/e306042b95fbc0d2fce23bf4fd4d1ad6.png)

照片由 [hay s](https://unsplash.com/@hay_leigh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

说到**数学优化**，尤其是**(整数)线性规划**，市面上有几种方案可以解决。

首先有微软，在 **Excel** 中实现了一个求解器作为插件。但是，它有 200 个决策变量的限制。对于相对较小的问题来说，这很好，但是如果您的问题需要更多的变量，我建议您开始寻找替代方案。

随着 **Python** 的大受欢迎，大多数数学家都选择了知名的库 [**纸浆**](https://pypi.org/project/PuLP/) 以及默认的开源求解器[**CBC**](https://projects.coin-or.org/Cbc)**。对于大多数问题来说，这很好。它可以完成工作，网上有很多教程可以教你基本的语法。然而，你可以做得更好！**

**对于我需要解决的大多数复杂的 ILP，我更喜欢使用[**guro bi**](https://www.gurobi.com)**因为三个原因:它**运行迅速**，它可以在有**时间限制**的情况下工作，并且对**数量的决策变量**没有**限制**。哦，我有没有提到它对学术用户 是免费的？下面，我将详细阐述这些论点，最后你可以找到一些有用的链接，帮助你开始使用 Gurobi。****

## ****事先写一个小纸条****

****在我们深入细节之前，我想声明我与古罗比没有任何关系。这个故事仅仅反映了我使用它的经验。应该注意的是，也有其他的商业替代品，如 IBM 的<https://www.ibm.com/analytics/cplex-optimizer>**。此外，也可以使用一个有限的，免费版本的 Gurobi 作为纸浆解算器。然而，由于这些限制，它只对小问题有用。不管怎样，让我们开始吧！******

## ********古罗比更快********

******首先，为什么速度很重要？你可以让你的代码通宵运行，对吗？但是，在某些情况下，这并不合适，例如在实时决策或假设分析中。在这两种情况下，都不希望不必要地等待解决方案。因此，**速度有时很重要**。根据 Gurobi 在 2010 年进行的一项 bechmark 测试，CBC(PuLP 的默认求解器)只能按时解决 61%的问题，而 Gurobi 在给定时间内解决了所有问题。显然，这项研究的可靠性值得怀疑，但它还是给人留下了印象。******

## ******可以给古罗比一个时间限制******

****有时候，在合理的时间内解决一个线性问题是不可能的。如果你用的是纸浆，那么你只是运气不好。它不可能给你一个中间结果。还好有古罗比！只需一行代码，您就可以确保不必等待很长时间，仍然可以获得结果。在下面的例子中，我将运行时间限制为一分钟。****

```
**m = gurobipy.model()
m.setParam('TimeLimit', 60)**
```

## ******Gurobi 对决策变量的数量没有限制******

****Excel 只有 200 个决策变量的限制，而 Gurobi 没有。在受限许可下，这已经是十倍了:[允许 2000 个变量](https://support.gurobi.com/hc/en-us/articles/360051597492-How-do-I-resolve-an-Model-too-large-for-size-limited-Gurobi-license-error-)和 2000 个线性约束。对于行货版本，有 [**没有限制**](https://support.gurobi.com/hc/en-us/community/posts/360071854832-Maximum-number-of-constraints-for-a-model) 。但是，沉住气！由于决策变量的数量不一定能量化问题的难度，因此模型仍然有可能需要一个时间限制，以便在合理的时间内给你一个解决方案。****

## ****但是，古罗比有什么局限性呢？****

****对古罗比来说，生活也不是一帆风顺的。首先是 [**对于商业用户来说比较贵的**](https://ampl.com/products/standard-price-list/) 。虽然他们的价格与竞争对手相当，但我认为他们的许可证对小企业来说是非常昂贵的。其次，如前所述，**古罗比不是巫师**:他能解决的问题复杂性是有限的。快速并不总是足以解决难题。第三，Gurobi 的**安装**，尤其是许可，在你的电脑上可能会**耗时**。为此，我在本文末尾添加了一个 YouTube 教程的链接。****

******Excel** 和**纸浆**没有古罗比有的第一和第三个缺点。两者都是更便宜的数学优化工具。在决定使用哪个选项时，您可以考虑这一点。****

****![](img/75e49350ac58fb23ee2cfb1f6a8719a8.png)****

****本·怀特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片****

## ******总之******

****好吧，我承认我可能对古罗比有点过于热情了。然而，由于它的速度、返回中间结果的能力以及包含大量变量的能力，我认为它值得称赞。然而，你应该考虑商业用途的**成本**，以及**耗时的安装**过程。总而言之，如果你必须解决复杂的数学优化问题，我会建议给古罗比一个机会。我想你不会后悔的！****

## ******有用的链接******

****你喜欢这个故事吗？欢迎在 Medium 上关注我，并留下掌声或评论！下面，你可以找到一些有用的链接来开始使用线性编程和 Gurobi。****

****</five-questions-that-will-help-you-model-integer-linear-programs-better-3256731a258c>  <https://www.gurobi.com/resource/modeling-examples-using-the-gurobi-python-api-in-jupyter-notebook/>  <https://medium.com/opex-analytics/optimization-modeling-in-python-pulp-gurobi-and-cplex-83a62129807a>  

如何在计算机上安装 Gurobi(py)的教程。****