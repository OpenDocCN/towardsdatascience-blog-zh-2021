# Scikit-learn 管道介绍

> 原文：<https://towardsdatascience.com/introduction-to-scikit-learns-pipelines-565cc549754a?source=collection_archive---------14----------------------->

## 使用 scikit-learn 构建端到端的机器学习管道

![](img/948c064ea34f7b7e5deb8e054416b71c.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当涉及到我们必须遵循的过程时，尝试解决机器学习问题通常是一项相当重复的任务。一般来说，当我们想要解决表格数据中的回归或分类问题时，我们遵循三个步骤:

1.  从源读取数据
2.  以不同的方式预处理数据
3.  向模型提供预处理的数据

在这篇文章中，我想向初学数据的科学家展示如何使用 scikit-learn 的管道以更易读、更易维护和更容易的方式完成这些步骤。如果你不是初学者，你可能知道管道是如何工作的，以及它们带来的好处。

许多数据科学初学者倾向于在不同的代码块中使用不同的函数逐个执行每个预处理步骤。尽管这是可行的，但是这种预处理数据和创建模型的方式一点也不干净和可维护。

假设我们有一个简单的数据集，比如 scikit-learn 的葡萄酒数据集。一个初学数据的科学家可能会这样做:

如果我们仔细观察这段代码，我们会看到很多冗余的代码，因为我们两次应用了相同的转换。然而，这个代码工作正常，我们得到 97.77%的准确率。

现在，我们将使用 scikit-learn 的管道复制上面的代码。但是等等，什么是 scikit-learn 管道？

scikit-learn 管道是 scikit-learn 包提供的一个组件，它允许我们在 scikit-learn 的 API 中合并不同的组件，并按顺序运行它们。这非常有帮助，因为一旦我们创建了管道，组件就为我们做所有的 X_train 和 X_test 预处理和模型拟合，它可以帮助我们在数据转换和拟合过程中最大限度地减少人为错误。让我们看一个例子:

这种方法也能达到 97.77%的准确率，但是它比其他代码块更具可读性和可维护性。

使用该管道的一个主要优点是，我们可以将它们保存为 scikit-learn 中的任何其他模型。这些管道是估算器，因此我们可以使用 joblib 轻松地将它们与所有预处理和建模步骤一起保存到二进制文件中，一旦保存了它们，我们就可以随时从二进制文件中加载它们:

# 结论

正如您在上面看到的，scikit-learn 管道是一个非常强大的工具，每个数据科学家在进行任何预处理步骤之前都应该牢记在心。然而，本文仅仅触及了使用该组件可以做的事情的表面。如果您有兴趣了解更多关于管道的知识，我可能会在将来写另一篇文章，解释如何创建可以插入 scikit-learn 管道的定制预处理函数，或者如何对管道内部的组件进行一些超参数调优。

如果你喜欢这篇文章，你也可以关注我并查看我的其他文章:

[](/how-to-detect-handle-and-visualize-outliers-ad0b74af4af7) [## 如何检测、处理和可视化异常值

### 当我第一次开始开发数据科学项目时，我既不关心数据可视化，也不关心离群点检测，我…

towardsdatascience.com](/how-to-detect-handle-and-visualize-outliers-ad0b74af4af7)