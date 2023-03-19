# 揭开 ML 的神秘面纱—第 1 部分:基本术语和线性回归

> 原文：<https://towardsdatascience.com/demystifying-ml-part1-basic-terminology-linear-regression-a89500a9e?source=collection_archive---------25----------------------->

![](img/af41872e44088c12de77668794b533b2.png)

*作者图片*

> 机器学习是人工智能(AI)的一种应用，它为系统提供了自动学习和根据经验进行改进的能力，而无需显式编程。

基于上面提到的定义来定义机器学习很容易，但它提出了进一步的问题，如“它是如何完成的？”、‘ML 的各个子域是什么？’、‘各种算法是什么？’，‘我需要懂数学吗？’、‘各种工业应用是什么？’等。此外，在过去几年的行业工作中，我已经意识到，作为应用科学家，我们主要关注 ML 的现有概念和应用级知识，以及其他技能，如解决问题、项目管理、软件工程等，以使其成为现实。因此，对我们许多人来说，核心 ML 概念仍然是一个黑箱。

就我个人而言，通过在计算机视觉、NLP 和结构化数据等领域使用不同算法的各种实验，我已经培养了强大的直觉和信心来实现 ML 的具体目标，但我并不总是能够理解核心概念，因为我不像核心研究人员那样积极解决算法进步。

为了解决我的这个问题，我开始了这个博客系列，以与核心学习保持一致，并分享一页纸的各种 ML/ DL 算法的流程图，如下图所示的线性回归。希望这也能对面临和我类似问题的专业人士有所帮助:)

我相信，从应用的角度来看，将这些算法作为流程图和一页视图中的各种直觉来研究(每次我们在项目中应用这些概念时都可以查看)会更有影响力。

![](img/4c93ac46ecaae6c7c7875b45fa9f0c5f.png)

*图片作者|* 线性回归的单页视图示例

在我开始分享这些核心知识之前，理解将在各种算法中使用的基本概念和术语很重要—

*   [变量](https://www.datarobot.com/wiki/feature/):一个训练样本的单个**观察单位或数据点**称为变量。表格数据可能包含分类/数量/顺序变量。对于单个训练图像，变量可以是对应于图像矩阵中特定位置的像素值。它可以是在一个时间序列的特定时间范围内记录的观察结果。
*   [目标变量](https://www.dtreg.com/solution/classes-and-types-of-variables#:~:text=Target%20variable%20%2D%2D%20The%20%E2%80%9Ctarget,and%20predicted%20by%20other%20variables.&text=Predictor%20variable%20%2D%2D%20A%20%E2%80%9Cpredictor,value%20of%20the%20target%20variable.):您希望**加深理解的数据集的变量或特征**。它可以是针对多个概念的训练样本的注释，如“具有猫和狗的图像的标签”或“表示对象价格的表格中的行的标签”等。
*   [方程](https://www.analyticsvidhya.com/blog/2019/10/mathematics-behind-machine-learning/#:~:text=Our%20machines%20cannot%20mimic%20the,single%20observation%20from%20the%20dataset.):每个方程代表数据集中变量和目标变量之间的**映射。右侧代表独立输入变量，左侧代表目标变量。机器学习只不过是为这些精确代表整个数据集的方程识别系数。简单线性回归使用传统的斜率截距形式，如下所示，其中“m”和“b”是我们的算法将尝试“学习”的变量，以产生最准确的预测。‘x’代表我们的输入数据，‘y’代表我们的预测。**

```
 y = mx + b (Simple regression)
  f(x,y,z) = w1x1 + w2x2 + ... + wNxN (Multivariable regression)
```

*   [正向传递](https://subscription.packtpub.com/book/data/9781789136364/1/ch01lvl1sec18/a-forward-pass):模型训练过程中处理**为每个训练样本**计算‘y’(或预测输出)的步骤称为正向传递。如果是第一次迭代，则选择随机(或 0)系数，否则使用最后一次迭代的系数。这些系数随着每次迭代而优化，以实现更接近目标变量的“y”。
*   [损失/成本函数](https://www.analyticsvidhya.com/blog/2021/02/cost-function-is-no-rocket-science/):它只是帮助我们**了解每次迭代的预测值&和实际值**之间的 **差异。用于在训练阶段以单个实数的形式量化这种损失的函数被称为“损失函数”,它帮助我们达到方程的最优解。损失函数指的是单个训练样本的误差，而成本函数指的是整个训练数据集中损失函数的平均值。**
*   [反向传播](https://en.wikipedia.org/wiki/Backpropagation):从技术上讲，反向传播计算每个系数的损失函数的梯度，这有助于**更新每个系数**，以便在下一次迭代中，这些更接近于最优解，从而进一步减少损失。
*   [梯度下降](https://ml-cheatsheet.readthedocs.io/en/latest/gradient_descent.html):梯度下降是**优化算法，用于更新方程的系数**(或参数)。它用于通过沿梯度负值定义的最陡下降方向迭代移动来最小化成本/损失函数。我们迭代地继续这个过程，直到我们到达全局最小值。

![](img/2d8e6922cdf31447554b5094ec41fd51.png)

图片由[韦尔奇实验室](https://www.youtube.com/watch?v=5u0jaA3qAGk)在 [ML 词汇表](https://ml-cheatsheet.readthedocs.io/en/latest/gradient_descent.html)上提供

*   [学习率:](https://en.wikipedia.org/wiki/Learning_rate)在每次迭代中向损失函数的最小值移动时，每步的**大小。在高学习率或大步长的情况下，存在错过损失函数的最小值的风险，而在非常低的学习率下，存在陷入局部最小值的风险，并且模型训练也可能是耗时的。**
*   [训练迭代](https://www.quora.com/What-will-happen-if-I-train-my-neural-networks-with-too-much-iteration):将**正向传递- >损失计算- >反向传播(更新权重)**的每一步称为一次训练迭代。为了建立一个基于训练数据进行归纳的模型，我们应该避免大量的训练迭代，并在模型开始过度拟合时使用早期停止等技术来停止模型。
*   权重和模型:权重是在所有训练迭代之后计算的等式的所有**系数的。就深度学习而言，它是为神经网络的每一层计算的模型的所有参数(或权重矩阵)。完整的**算法方程被称为模型，其系数**是在基于训练数据集最小化损失的同时计算的。**

下一个系列—第 2 部分:M 中的线性分类器(即将推出…)

我希望这有助于应用 ML 知识的读者理清一些核心概念。感谢 [Anit Bhandari](https://www.linkedin.com/in/anitbhandari/) 通过对 ML 概念的核心理解为 upskill 提供了方向性的意见。

参考资料:

[](https://ml-cheatsheet.readthedocs.io/en/latest/linear_regression.html) [## 线性回归- ML 词汇表文档

### 线性回归是一种有监督的机器学习算法，其中预测输出是连续的，并具有一个…

ml-cheatsheet.readthedocs.io](https://ml-cheatsheet.readthedocs.io/en/latest/linear_regression.html) [](https://medium.com/analytics-vidhya/the-pitfalls-of-linear-regression-and-how-to-avoid-them-b93626e1a020) [## 线性回归的陷阱以及如何避免它们

### 当线性回归假设不成立时该怎么办

medium.com](https://medium.com/analytics-vidhya/the-pitfalls-of-linear-regression-and-how-to-avoid-them-b93626e1a020) [](/linear-regression-from-scratch-with-numpy-5485abc9f2e4) [## 使用 NumPy 从头开始线性回归

### 欢迎使用 NumPy 从头开始线性回归的第一篇文章！我试着解释一下背后的直觉…

towardsdatascience.com](/linear-regression-from-scratch-with-numpy-5485abc9f2e4) [](https://github.com/leventbass/linear_regression/blob/master/Linear_Regression.ipynb) [## levent bass/线性回归

### 从头开始使用 numpy 实现线性回归- leventbass/linear_regression

github.com](https://github.com/leventbass/linear_regression/blob/master/Linear_Regression.ipynb)