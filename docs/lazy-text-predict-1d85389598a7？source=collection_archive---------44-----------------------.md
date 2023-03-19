# 懒惰文本预测

> 原文：<https://towardsdatascience.com/lazy-text-predict-1d85389598a7?source=collection_archive---------44----------------------->

## 用 AUTO-ML 分类文本

## 一个用于快速简单的文本分类的开源库

![](img/5a0b04a3b36952aaec3dae5647902c76.png)

由[杰洛特](https://pixabay.com/users/geralt-9301/)通过 [pixabay](https://pixabay.com/photos/apple-fruit-selection-especially-1594742/) 拍摄(CC0)

## 简介和动机

有很多很棒的文本分类包可以帮助你解决你的业务问题。比如决定一句话的情绪。然而，有一个复杂的问题。许多任务都是定制的，因此没有预先训练好的现成模型可供您开箱即用。对于初学者来说，制作一个定制的文本分类模型是非常具有挑战性的。定制机器学习包之间的差异通常非常细微。因此，需要丰富的经验来选择不同的工具。此外，为您的数据集实现选择的解决方案需要技术经验。这阻止了门外汉前进。

如果你想直接跳到代码，[点击这里](https://github.com/lemay-ai/lazyTextPredict)。

## 汽车救援队

我们是一个机器学习开发人员团队，认识到开发人员社区面临的挑战，我们问“有没有一种解决方案可以让开发人员轻松地在不同的文本分类选项之间进行选择，而不必成为机器学习专家？”由于没有找到文本数据集的简单解决方案，我们决定构建一个并与开发人员社区共享。我们称之为**懒惰文本预测。**使用这一新工具，您可以在各种不同的**文本分类模型上测试您的数据集，**然后进一步训练最佳模型，以产生为您的特定需求定制的非常好的解决方案！

由于我们接触了 [lazypredict 项目](https://pypi.org/project/lazypredict/)和其他 [AutoML 解决方案](https://cloud.google.com/automl)，我们受到了构建这个项目的启发。

**lazy-text-predict** 包含一个工具包，让您加载数据来训练不同的文本分类模型，并比较它们的性能，以选择适合您情况的最佳工具。我们选择了几个深度学习(例如，transformers)和基于计数矢量器(sci-kit learn pipelines)的模型，这些模型可以解决各种文本分类任务。开始非常简单。

## 快速入门

开始使用这个工具真的很容易！您需要运行一个命令并键入 5 行代码:

首先，安装软件包:

```
pip install lazy-text-predict
```

接下来，使用代码:

上面的代码将训练一系列模型，根据输入文本“X”预测标签“Y”。该工具还将报告每种模型类型的性能。您可以使用“trial.print_metrics_table()”查看试验的摘要，并根据您即时想到的一些定制数据测试模型。例如:

```
trial.predict("This movie was so good that I sold my house to buy tickets to see it")
```

可以将自己的数据集加载到“X”和“Y”中，自己训练模型。一旦你做到了这一点，你就可以更严格地训练你最喜欢的这些模型，然后导出它用于你自己的代码中(这个工具也很方便)。我们已经准备了几个带注释的例子来展示如何做到这一点，我们的文档也给出了非常清晰的说明(我们希望！).以下是文档的链接:

[**https://github.com/lemay-ai/lazyTextPredict**](https://github.com/lemay-ai/lazyTextPredict)

## **其他工具**

**我们认为我们的工具在帮助人们开始学习 NLP 方面非常棒，我们希望您能使用它，但是如果您真的想深入这个令人兴奋的领域，您应该看看其他几个来源:**

****拥抱脸**:拥抱脸的团队策划了大量的自然语言处理模型和用于部署它们的软件包。我们已经将他们的许多模型整合到这个工具中。[看看吧！](https://huggingface.co/)**

****Scikit-learn**:[Scikit-learn](https://scikit-learn.org/stable/)是当今最流行的机器学习和数据科学库之一，所以它们被包含在我们的工具中是很自然的。一般来说，它们是学习机器学习的很好的资源，并且它们有很多关于如何实现 NLP 和其他机器学习管道的有用教程。**

## **下一步是什么？**

**lazy-text-predict 是一个不断改进的开源项目。我们有兴趣与社区合作承担这个项目。如果您想参与，请联系我们，联系方式为 [daniel@lemay.ai](mailto:daniel@lemay.ai)**

**特别感谢开发团队对这个项目的第一次发布和这篇文章的贡献。**

*   **爱德华·布克( [GitHub](https://github.com/epb378)**
*   **吉特什·帕布拉( [GitHub](https://github.com/jiteshpabla) )**
*   **Rajitha Hathurusinghe**
*   **赛义德·阿巴斯( [GitHub](https://github.com/abbasshujah)**
*   **farzad amirjeavid(“t0”github**
*   **rabah al-qudah(“T2”ACM【T3])**

**-Daniel
[lemay . ai](https://lemay.ai/)
[【Daniel @ lemay . ai】](mailto:daniel@lemay.ai)**