# 进化计算(完整课程)概述

> 原文：<https://towardsdatascience.com/evolutionary-computation-full-course-overview-f4e421e945d9?source=collection_archive---------24----------------------->

![](img/69eebac9f72caac07f0a9d71a238acfa.png)

[https://unsplash.com/photos/FHnnjk1Yj7Y](https://unsplash.com/photos/FHnnjk1Yj7Y)

## 进化计算课程

## 关于我将在这个全新的系列中涉及的材料、概念和应用的介绍性帖子！

大家好！我决定开设一门关于进化计算的课程。在这篇文章中，我将只给出课程的简要概述！

进化计算是计算智能的一个子领域，是机器学习和人工智能的一个分支。进化计算的应用很多，从解决优化问题、设计机器人、创建决策树、调整数据挖掘算法、训练神经网络和调整超参数。

如果你熟悉数据科学和机器学习模型，所有的统计和“黑箱”模型都是为了解决优化问题而建立的。你可能熟悉这些问题，如 MSE、交叉熵、MAE 等，在这些情况下，我们希望最小化这些值。进化计算是优化理论的一个领域，它不是使用经典的数值方法来解决优化问题，而是使用生物进化的灵感来“进化”出好的解决方案。当没有适应度函数的已知导数(强化型学习)时，或者当适应度函数具有许多可能陷入序列方法的局部极值时，通常使用进化计算来代替标准数值方法。

# 目录

*   要覆盖的材料
*   文献应用实例
*   预赛
*   结论

# 要覆盖的材料

*   单元 1)最优化理论

[](/unit-1-optimization-theory-e416dcf30ba8) [## 单元 1)最优化理论

### 最优化理论和四种主要最优化问题的概述

towardsdatascience.com](/unit-1-optimization-theory-e416dcf30ba8) 

*   单元 2)进化计算简介

[](/unit-2-introduction-to-evolutionary-computation-85764137c05a) [## 单元 2)进化计算简介

### 进化计算和遗传算法概述！

towardsdatascience.com](/unit-2-introduction-to-evolutionary-computation-85764137c05a) 

*   单元 3)遗传算法

[](/unit-3-genetic-algorithms-part-1-986e3b4666d7) [## 单元 3)遗传算法(第一部分)

### 遗传算法概述—主要是交叉和变异算子

towardsdatascience.com](/unit-3-genetic-algorithms-part-1-986e3b4666d7) [](/unit-3-genetic-algorithms-part-2-advanced-topics-a24f5be287d5) [## 单元 3)遗传算法(第二部分)高级主题

### 遗传算法的高级主题——控制参数、选择性交配和遗传变异

towardsdatascience.com](/unit-3-genetic-algorithms-part-2-advanced-topics-a24f5be287d5) [](/unit-3-application-evolving-neural-network-for-time-series-analysis-63c057cb1595) [## 单元 3 应用)用于时间序列分析的进化神经网络

### 第三单元的高潮是应用我们的概念来发展一个预测时间序列问题的神经网络

towardsdatascience.com](/unit-3-application-evolving-neural-network-for-time-series-analysis-63c057cb1595) 

*   第 4 单元)遗传规划

[](/unit-4-genetic-programming-d80cd12c454f) [## 第 4 单元)遗传规划

### 涵盖遗传规划的主要课题，并将其应用于时间序列分析问题

towardsdatascience.com](/unit-4-genetic-programming-d80cd12c454f) 

*   第 5 单元)进化规划

[](/unit-5-evolutionary-programming-cced3a00166a) [## 第 5 单元)进化规划

### 涵盖进化编程的主要概念:变异和选择操作符

towardsdatascience.com](/unit-5-evolutionary-programming-cced3a00166a) [](/unit-5-application-optimizing-constrained-non-linear-pressure-vessel-design-problem-2fabe9f041ef) [## 单元 5 应用)优化约束非线性压力容器设计问题

### 我们将应用进化规划来寻找压力容器设计问题的最佳解决方案

towardsdatascience.com](/unit-5-application-optimizing-constrained-non-linear-pressure-vessel-design-problem-2fabe9f041ef) 

*   第 6 单元)进化策略

[](/unit-6-evolutionary-strategies-finding-the-pareto-front-65ad9ae54a34) [## 单元 6)进化策略——寻找帕累托前沿

### 涵盖进化策略的主要概念:加号和逗号策略，并应用它们找到帕累托…

towardsdatascience.com](/unit-6-evolutionary-strategies-finding-the-pareto-front-65ad9ae54a34) 

*   第 7 单元)差异进化

[](/unit-7-differential-evolution-automated-machine-learning-eb22014e592e) [## 第 7 单元)差异进化—自动机器学习

### 应用差分进化的概念在进化一个深度卷积神经网络的结构上…

towardsdatascience.com](/unit-7-differential-evolution-automated-machine-learning-eb22014e592e) 

*   单元 8)共同进化

[](/unit-8-co-evolution-reinforcement-learning-for-game-ai-design-97453ed946ec) [## 单元 8)协同进化——游戏人工智能设计的强化学习

### 共同进化的竞争游戏人工智能的发挥月球着陆器使用 Python 健身房环境和进化计算…

towardsdatascience.com](/unit-8-co-evolution-reinforcement-learning-for-game-ai-design-97453ed946ec) 

本课程将是一个简短而深入的系列教程，涵盖上述主题。会有很多帖子，会持续很久。如果你时间紧迫，只需阅读单元 1 至 3，因为它会给你一个优化理论，整体进化计算和遗传算法的基本概述。其余的单元，即单元 4-8，实际上都是遗传算法本身的不同变体。唯一不同的是，它们针对不同的问题进行了超参数化，具有独特的特征；然而，它们都有相似的形式，都属于遗传算法的范畴。

# 文献论文示例

在每个单元的结尾，我们将回顾一个在同行评议文献中发现的真实世界的例子。以下是我们将涉及的各种示例:

*   单元 3)训练用于时间序列分析的前馈神经网络
*   单元 4)发展相同时间序列问题的基本方程/结构
*   单元 5)求解约束非线性规划问题
*   单元 6)求解多目标 Pareto 前沿的算法
*   单元 7)自动机器学习:设计 CNN 的架构
*   单元 8)设计玩月球着陆器的游戏 AI

# 预赛

下面是一些预备知识，我希望你应该知道，以便最好地应用材料和理解概念:

*   基本统计—概率分布
*   线性代数(基本概念:矩阵乘法，欧几里德距离等…)
*   数值方法(基本概念:为什么使用它们，为什么需要它们)
*   如何用 Python 编程—使用数据结构和算法
*   了解 Python 科学库——Numpy 和 Scikitlearn

# **结论**

进化计算是解决优化问题的方法论。优化问题在机器学习和人工智能领域中大量存在。EC 通常用于经典数值方法无法找到足够好的解的情况。

好了，这应该完成了课程的基本概述！如果您对迄今为止讨论的任何主题或应用示例感兴趣，那么请不要离开，因为我们将涵盖所有这些以及更多内容！在下一篇文章中，我们将从单元 1)最优化理论开始

[](/unit-1-optimization-theory-e416dcf30ba8) [## 单元 1)最优化理论

### 最优化理论和四种主要最优化问题的概述

towardsdatascience.com](/unit-1-optimization-theory-e416dcf30ba8)