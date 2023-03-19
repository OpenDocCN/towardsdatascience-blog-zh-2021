# 蒙特卡罗方法解释

> 原文：<https://towardsdatascience.com/monte-carlo-method-explained-8635edf2cf58?source=collection_archive---------8----------------------->

## 理解蒙特卡罗方法以及如何用 Python 实现它

![](img/6b58586822ceda0762b1a7f17305c66c.png)

[https://unsplash.com/photos/tV3Hh38eoSg](https://unsplash.com/photos/tV3Hh38eoSg)

在本帖中，我将向你介绍、解释和实施**蒙特卡罗**方法。这种模拟方法是我最喜欢的方法之一，因为它简单，但它是一种解决复杂问题的精炼方法。它是由波兰数学家 Stanislaw Ulam 在 20 世纪 40 年代发明的。它是以摩纳哥的一个赌博城镇命名的，因为随机原则模仿了轮盘赌游戏。蒙特卡洛模拟是一个非常常见的概念，用于量化各种领域的风险，如股票价格、销售预测、预测建模等。

# 蒙特卡罗方法是如何工作的？

蒙特卡罗模拟是一种模拟统计系统的方法。该方法使用定义系统中的随机性来发展和近似量，而不需要解析地求解该系统。这种方法的主要概念是，运动系统中的一个点最终将以均匀和随机的方式访问该系统运动的空间的所有部分。这就是所谓的遍历性。

![](img/a01ade9dfcb8d35fb18e4753af0b921b.png)

图片由作者提供

该模型通过使用问题领域中的一系列值而不是特定的输入来进行预测。这种方法利用概率分布(正态、高斯、均匀等。)对于任何具有不确定性的变量。根据指定的试验次数，在一个域中使用随机值的过程会重复多次。一般来说，试验次数越多，结果收敛到某个值的可能性就越大。通常用于长期预测建模的时间序列分析。一旦所有的模拟都完成了，你就会有一系列可能的结果，以及每个结果发生的相关概率。

# 例子

在解释这种方法的许多方式中，解释蒙特卡罗模拟最常见的例子是所谓的*布丰针实验*，以近似 **π** 的值。实验如下，我们随机将 N 根尺寸为 L 的针落在一张纸上，这张纸被长度为 2L 的平行条分割。

![](img/a67e77f14c9f16613aa6a21c804bf282.png)

布丰的针实验——作者提供

随机落下这些针后，确定接触纸张分割线的针数和落下的针总数(N)。

```
**π ≈ N / number of needles crossed line****Note :** 1) A large amount of needles must be dropped to have a close approximation of π.
2) This formulation strictly works because we initially stated that the distance between the lines was 2 * L (where L is the length of the needle)
```

关于这背后的数学原理，你可以在这里[找到](https://mathworld.wolfram.com/BuffonsNeedleProblem.html)，你也可以在这里免费运行这个实验[。](https://ogden.eu/pi/)

谨慎一点，这个例子只是为了解释蒙特卡罗方法。蒙特卡罗方法可以用于许多不同的情况，但并不总是建议。尽管这种方法可行，但实际上这是蒙特卡罗方法的一个糟糕的用例。还有许多其他方法可以近似π的值，其中大多数方法的计算效率要高得多。

您应该使用这种方法的情况是，当您需要估计结果存在高度不确定性时。由于股票市场的随机性和不确定性，这种方法通常用于金融行业的股票预测。由于这些限制，像这样的模型是受欢迎的，并且通常比基于普通回归的方法执行得更好。在不确定的情况下，这种方法非常有效。

# 算法

1.  识别自变量和因变量，并定义它们可能的输入域。
2.  确定概率分布以在该域上随机生成输入
3.  根据随机生成的输入计算问题的输出
4.  将实验重复 N 次，然后汇总结果

在进行这个实验时，计算方差和标准差是常见的做法。一般来说，方差越小越好

# 优点和缺点

我将概述使用这种方法的一些最显著的优点和缺点。

**优势**

*   估计不确定性的强有力方法
*   给定正确的边界，该模型可以测量问题的参数空间
*   简单而直观，这种方法很容易理解

**劣势**

*   计算效率低——当有大量变量受限于不同的约束时，使用这种方法需要大量的时间和计算来逼近一个解
*   如果不良参数和约束输入到模型中，那么不良结果将作为输出给出

# Python 实现

# 摘要

总之，本文概述了蒙特卡罗模拟是一种模拟统计系统的方法。他们利用一个确定的系统中的随机性来发展和近似数量，而不需要解析地解决它。当存在高度不确定性时，最好使用这种方法。虽然它的计算效率很低，但理解起来非常直观，可以调查问题的约束条件的大样本，并可以有效地近似不确定性。由于这些原因，它通常用于金融行业。

# 资源

*   【https://en.wikipedia.org/wiki/Ergodic_hypothesis 
*   [https://mathworld.wolfram.com/BuffonsNeedleProblem.html](https://mathworld.wolfram.com/BuffonsNeedleProblem.html)
*   [https://James Howard . us/2019/09/07/Monte-Carlo-simulation-优缺点/](https://jameshoward.us/2019/09/07/monte-carlo-simulation-advantages-and-disadvantages/)

如果你喜欢这本书，那么看看我的其他作品。

</text-summarization-in-python-with-jaro-winkler-and-pagerank-72d693da94e8>  </link-prediction-recommendation-engines-with-node2vec-c97c429351a8>  </word2vec-explained-49c52b4ccb71>  <https://medium.com/nerd-for-tech/markov-chain-explained-210581d7a4a9>  <https://medium.com/nerd-for-tech/k-nearest-neighbours-explained-7c49853633b6> 