# 五大数据科学实践

> 原文：<https://towardsdatascience.com/the-top-5-data-science-practices-83fdc3a68887?source=collection_archive---------14----------------------->

## 意见

## 深入了解数据科学家和机器学习工程师的最佳实践。

![](img/e1f9fa3d4af9d08172f46f05399a33c2.png)

照片由[威廉·艾文](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/process?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄。

# 目录

1.  介绍
2.  业务问题陈述
3.  隔离业务指标
4.  算法比较
5.  代码审查
6.  MLOps 和 DevOps
7.  摘要
8.  参考

# 介绍

在数据科学领域工作了几年后，我从各种不同的经历中发现了一些最佳实践。我将重点介绍我的五大数据科学实践，希望对您未来的努力有所帮助。虽然有无数种方法可以改善您的数据科学流程，但这些方法不仅可以改善您作为数据科学家的日常工作，还可以改善您作为员工的日常工作。也就是说，其中一些实践不仅可以应用于数据科学，还包括但不限于数据分析、机器学习、软件工程、数据工程和 DevOps。如果您想了解关于五大最佳数据科学实践的更多信息，请继续阅读。

# 业务问题陈述

![](img/8384daf9cfbad19fb962652c115eddf1.png)

由 [Daria Nepriakhina](https://unsplash.com/@epicantus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/whiteboard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

我喜欢在我的许多文章中提到这个最佳实践，因为它非常重要。作为数据科学家，我们可能会如此沉迷于机器学习算法的技术方面，如调整超参数和降低我们的错误率。然而，我们必须记住，数据科学项目的目标不应该是以最高的准确性获得最复杂的答案。事实上恰恰相反——我们希望找到业务问题的最简单的解决方案。数据科学可能已经相当复杂，我们使用它的原因也是如此。这让我想到了业务问题陈述。这就是你首先利用机器学习算法的全部原因。没有人会在意你对一个不能回答或解决当前具体问题的模型有 98%的准确率。这就是为什么隔离问题的真正含义并与您的利益相关者(管理并经常分配数据科学项目的*同事，或提出业务问题*)进行核对是极其重要的。

> 下面，我将给出一个关于业务问题陈述和数据科学解决方案的很好的例子:

***差:***

“用一些数据科学的魔法来解决我们的检测问题，看看我们可以达到多高的准确性来节省资金。”

***伟大:***

“从图像中检测物体既耗时又不准确，如果由人来完成的话。”

***分析* :**

*糟糕的*——虽然糟糕的例子可能会引导你找到看待问题的正确方式，但它实在是太混乱了，没有必要。很多时候，利益相关者或公司中的其他人认为数据科学可以解决一切问题，所以由你来告诉他们他们的问题是否可以通过机器学习算法来解决。当然，魔法这个词的使用是模糊的，过分的；然而，知道有些人在现实世界的商业案例问题中这样说是很令人惊讶的。接下来，有人提出了一个解决方案，声称数据科学将解决这个问题，但这实际上可能并不必要。然后，他们假定数据科学的魔力无论如何都是准确的，并且会省钱。虽然这很有可能是真的，但这不能在问题陈述中断言。问题陈述的主要目标是简单地隔离一个问题。然后，下一步将是开发一个解决方案以及预期结果和/或投资回报( *ROI* )。

*棒极了* —这个问题陈述要好得多，因为它首先展示了当前流程以及它的问题所在。它实际上并没有暗示数据科学或承诺一些可能不会发生的事情。即使你不是数据科学家，你也可以清楚地看到所阐述的内容，这一点很重要，因为跨职能部门工作是公司的首选工作方法。由于该语句定义明确，数据科学家现在可以分离出以下内容:

```
**current process:** detecting objects from images by a person**problem one:** time-consuming**problem two:** innaccuate
```

现在，根据已经分析的内容，您可以研究当前的流程，以评估数据科学是否可以应用。然后，你会有两个问题，你会找到解决方案。这个粒度允许每个人都在同一个页面上。如果选择了数据科学模型作为解决方案，那么算法的目标是使当前流程更加高效和准确。现在，有了模型可以改进的基本指标。

> 示例:

比方说，以前，从一个人那里以 80%的准确率对 500 幅图像进行分类需要 2 个小时。现在，一个数据科学模型可以用来在 10 分钟内以 98%的准确率对同样的 500 张图像进行分类。数据科学家现在知道要做什么，要争取什么。

这个例子让我想到了下一个最佳实践。

# 隔离业务指标

![](img/80d737e4d7d5d3189c840452eacedc46.png)

在 [Unsplash](https://unsplash.com/s/photos/metrics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片。

一旦我们有信心选择数据科学模型作为上述问题的解决方案，我们将需要专门隔离需要改进的业务指标。在这种情况下，度量的例子包括每个员工花费的时间和任务的准确性。这些指标可以与手动流程和自动化数据科学流程相比较。此外，这些指标可以通过仪表板进行监控和可视化，以获得易于理解的结果。

> 隔离业务指标的示例如下:

当前流程:

*   每个员工每天花费的时间
*   每个员工每周花费的时间
*   员工每天的准确性
*   每周员工的准确性

数据科学解决方案:

*   每个模型每天花费的时间
*   每个模型每周花费的时间
*   模型每天的精确度
*   每周模型的精确度

这些指标不仅会突出模型(*希望是*)带来的改进，还会突出您的特定数据科学解决方案对公司的重要性和益处。这一点伴随着证明你的价值，一个数据科学家可以节省时间和金钱，同时也使公司的过程更加准确吗？

我提到了一些简单而具体的指标；然而，任何公司都可以监控和分析更多的信息，最终取决于具体情况、流程和产品。要查看的一些常规指标包括:

—每个用户的点击次数

—特定年龄组的每个用户的点击数

—特定位置组的每个用户的点击次数

—每日流失

—每周变动

—指标的每周改进

—等等。

# 算法比较

![](img/fefbc578fce2c4ed6c8ddff6ab4e71d9.png)

照片由 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/accuracy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄。

我们刚刚讨论了数据科学对业务的重要性，现在让我们讨论一下算法比较的重要性。比较几种不同的机器学习算法是最佳实践，因为它可以让您大致了解哪种算法最适合您的数据和解决方案。例如，您可能想看看决策树、随机森林或 XGBoost 等算法，并比较它们如何在相同的训练和测试数据上相互叠加。虽然这种做法稍微明显一些，但提出来仍然很重要。我花了一段时间才意识到，我应该总是至少测试几个不同的算法，以找到最好的一个。这是因为有时你可以陷入一个特定的算法，并将其用于每一个解决方案。然而，在有些情况下，一种通常更好的算法有时可能会更差，反之亦然。

> 算法比较的一个例子如下:

***用例 1:对图像进行分类***

决策树准确率— 94%

随机森林准确率— 96%

XGBoost 精度— 98%

***用例 2:给动物园动物分类***

决策树准确率— 95%

随机森林准确率— 98%

XGBoost 精度— 97%

在这个例子中，您可以看到 XGBoost 在过去是如何做得更好的( *aka，用例 1* )，但不一定对未来的每一种情况都是最好的。请确保查看数据的大小、数值特征与分类特征的数量，等等。有一个非常强大和有用的库，它比较了几乎所有你听说过的机器学习算法。

*这个库叫做*[*py caret*](https://pycaret.org/)*【5】我强烈推荐使用它:*

下面是一个关于如何实现 PyCaret 的 Python 代码片段的小例子。在一行代码中，你可以看到几种机器学习算法是如何相互叠加的。如果您知道要查看哪些算法或者想要节省更多时间，也可以在比较时隔离某些算法:

```
# import the library
from pycaret.classification import *
model = setup(data = your_data, target = image_name)# compare models
compare_models()# best models based on AUC, Accuracy is default
compare_models(sort = 'AUC')# compare exact models
compare_models(include = ['rf','xgboost'])
```

# 代码审查

![](img/45cc38c33dd7258441c8fe97e95fc02a.png)

在[Unsplash](https://unsplash.com/s/photos/code-review?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上[海拉戈斯蒂奇](https://unsplash.com/@heylagostechie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片。

这个下一个最佳实践可能看起来更明显或者更普遍，但是我相信数据科学家经常忽略这一点。虽然软件工程师在大多数时候肯定会遵守代码审查，但数据科学家并不总是按照他们应该做的那样审查代码。通常情况下，数据科学家独自工作，即使他们的团队中有其他数据科学家(*每个数据科学家负责一个特定的项目*)。

不仅让另一位数据科学家检查您的代码，而且让一位机器学习操作工程师或软件工程师检查您的代码，这是非常有益的。一般来说，在将你的最终代码库推送到一个主分支之前，最好是引入别人而不是你自己(例如 GitHub 上的*)。将这一最佳实践作为提醒，让您的代码接受审查。它可以像修改一行代码一样简单，最终为你和你的公司节省时间——一个熟练的软件工程师可能会更快地理解这一点。特别是有一些熊猫和 NumPy 操作可以增强，节省了大量的时间和金钱。一般来说，连续几天甚至几周看着相同的代码可能会导致你对它过于熟悉，从而让你意识不到容易犯的错误。尝试进入代码审查的常规程序，以确保您的代码、项目和业务处于最佳状态。*

# MLOps 和 DevOps

![](img/356178139b67d23efdd784a3aaab5a11.png)

由[彼得·冈博斯](https://unsplash.com/@pepegombos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/software?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【7】上拍摄。

最后一个实践与代码审查有些关系，但重点是熟悉在您的数据科学模型部署到生产中之后会发生什么。通常，在一个更大的公司，你可以把你的模型交给另一个人，他可以正确地部署它，但是可能会犯错误，例如，培训时间。如果您不知道您的模型在从本地测试转移到生产时正在做什么，那么还会发生其他的后果。

有时候，数据科学家负责整个过程，所以这种做法可能不适合你，但是如果你幸运地与机器学习操作工程师( *MLOps* )或 DevOps 工程师这样的人一起工作，你可以习惯于在没有实际看到之后过程中发生的事情的情况下交付模型。结果可能与您预期的不同，特别是测试的结果，因此您会想要回顾自您移交以来发生的整个过程(*如果您的公司有，当然是*)。

> 由于未执行此最佳实践而导致错误的一个示例可能是:

*   训练和测试模型导致 94%准确性
*   移交给 MLOps 或 DevOps 工程师(*或软件工程师等)。*)
*   在投入生产后进行检查，结果是 88%
*   您会发现，训练数据不是 100，000 行数据，而是 20，000 行数据
*   也许另一个工程师对训练时间设置了时间限制，限制了先前已经设置的训练行数

这个例子可能不会发生在您身上，但就像在模型测试中验证您的数据一样，您会想要验证整个数据科学过程，尤其是当您的模型投入生产时，因为即使是最小的变化也可能导致最显著的差异。

# 摘要

有无数种方法可以改进您的数据科学流程。我已经讨论了将最终改进您的数据科学过程的五个最佳实践。当然还有几个，但这些是我发现特别突出的主要问题。我还提供了问题的解决方案，作为最佳实践。

> 以下是数据科学家的最佳实践总结:

```
Developing a Concise Business Problem StatementIsolating Key Business MetricsAlgorithm ComparisonCode ReviewMLOps and DevOps Incorporation and Validation
```

*我希望你喜欢我的文章，并觉得它很有趣。请在下面随意评论您作为数据科学专家或其他类似角色所遵循的最佳实践。你同意我所讨论的，还是不同意——为什么？谢谢你的阅读，我很感激！*

# 参考

[1]照片由[威廉·艾文](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/process?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2015)上拍摄

[2]照片由 [Daria Nepriakhina](https://unsplash.com/@epicantus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/whiteboard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2017)上拍摄

[3]照片由[奥斯汀·迪斯蒂尔](https://unsplash.com/@austindistel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/metrics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[4]照片由 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/accuracy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄

[5] Moez Ali， [PyCaret 主页](https://pycaret.org/)，(2021)

[6]照片由[heylogostechie](https://unsplash.com/@heylagostechie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/code-review?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)上拍摄

[7]Peter gom Bos 在 [Unsplash](https://unsplash.com/s/photos/software?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2019)