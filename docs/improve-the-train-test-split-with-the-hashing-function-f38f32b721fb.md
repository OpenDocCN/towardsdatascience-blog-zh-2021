# 用哈希函数改进训练测试分割

> 原文：<https://towardsdatascience.com/improve-the-train-test-split-with-the-hashing-function-f38f32b721fb?source=collection_archive---------12----------------------->

![](img/e46550adbacd277b025732b8d38c4d45.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/cryptography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 更新数据集时，确保定型集和测试集不会混淆的最佳方法是

最近，我在阅读 Aurélien Géron 的*用 Scikit-Learn、Keras 和 TensorFlow* 进行机器学习(第二版),这让我意识到，在为机器学习模型准备数据时，我们处理训练测试分割的方式可能存在问题。在本文中，我快速演示了问题是什么，并展示了如何修复它的示例。

# 说明问题

我想提前说，我提到的问题并不总是问题*本身*，这完全取决于用例。在准备用于培训和评估的数据时，我们通常使用 Scikit-Learn 的`train_test_split` 等函数分割数据。为了确保结果是可重复的，我们使用了`random_state`参数，所以无论我们分割相同的数据集多少次，我们将总是得到完全相同的训练测试分割。这句话中存在我之前提到的潜在问题，特别是在关于*相同数据集*的部分。

想象一下这样一种情况，您构建了一个预测客户流失的模型。您收到了令人满意的结果，您的模型已经投入生产并为公司创造了价值。干得好！然而，一段时间后，随着更多的客户加入公司，客户中可能会出现新的模式(例如，全球疫情改变了用户行为)，或者您只是收集了更多的数据。出于任何原因，您可能希望重新训练模型，并使用新数据进行训练和验证。

这正是问题出现的时候。当您在新的数据集上使用好的旧的`train_test_split`(所有的旧的观察结果+自训练以来收集的新的观察结果)时，不能保证您在过去训练的观察结果将仍然用于训练，对于测试集也是如此。我将用 Python 中的一个例子来说明这一点。

首先，我生成了一个包含 1000 个随机观察值的数据框架。我使用`random_state`应用了 80–20 训练测试分割，以确保结果是可重复的。然后，我创建了一个新的数据帧，在初始数据帧的末尾添加了 500 个观察值(在这种情况下，重置索引对于跟踪观察值非常重要！).我再一次应用了训练测试分割，然后调查了初始集合中有多少观察值实际出现在第二个集合中。为此，我使用了 Python 的`set`的便捷的`intersection`方法。答案是 800 分中的 669，200 分中的 59。这清楚地表明，数据被重新洗牌。

这种问题的潜在危险是什么？这完全取决于数据量，但在一次不幸的随机抽取中，所有新的观察结果都将出现在其中一个集合中，这对正确的模型拟合没有多大帮助。尽管这种情况不太可能发生，但更有可能出现的在集合中分布不均匀的情况也不是那么理想。因此，最好是将新数据平均分配给两个集合，同时将原始观测值分配给各自的集合。

# 解决问题

那么我们该如何解决这个问题呢？一种可能性是基于某个唯一的标识符将观察值分配给训练集和测试集。我们可以使用某种散列函数来计算观察值标识符的散列，如果该值小于最大值的 x%,我们就将该观察值放入测试集。否则，它属于训练集。

您可以在下面的函数中看到一个示例解决方案(基于 Aurélien Géron 在他的书中提出的解决方案)，它使用 CRC32 算法。算法的细节我就不赘述了，你可以在这里阅读关于 CRC [的内容。或者，](https://en.wikipedia.org/wiki/Cyclic_redundancy_check)[在这里](https://stackoverflow.com/questions/10953958/can-crc32-be-used-as-a-hash-function)你可以找到一个很好的解释，为什么 CRC32 可以很好地充当哈希函数，以及它有什么缺点——主要是在安全性方面，但这对我们来说不是问题。该函数遵循上一段中描述的逻辑，其中 2 是该散列函数的最大值。

**注意:**上面的函数适用于 Python 3。要针对 Python 2 进行调整，我们应该遵循`crc32`的文档，使用方法如下:`crc32(data) & 0xffffffff`。你也可以在这里阅读更详细的描述[。](https://stackoverflow.com/questions/50646890/how-does-the-crc32-function-work-when-using-sampling-data)

在实际测试该函数之前，有一点非常重要，那就是应该为散列函数使用一个唯一且不可变的标识符**。对于这个特定的实现，也是一个数字(虽然这可以相对容易地扩展到包括字符串)。**

在我们的 toy 示例中，我们可以安全地使用行 ID 作为惟一的标识符，因为我们只在初始数据帧的末尾添加新的观察值，从不删除任何行。然而，在使用这种方法处理更复杂的情况时，需要注意这一点。因此，一个好的标识符可能是客户的唯一编号，因为根据设计，这些编号应该只会增加，不会有重复。

为了确认这个函数正在做我们想要它做的事情，我们再次运行测试场景，如上所示。这一次，对于两个数据帧，我们都使用了`hashed_train_test_split`函数。

当使用散列的唯一标识符进行分配时，我们实现了训练集和测试集的完美重叠。

# 结论

在本文中，我展示了如何使用散列函数来改进训练测试分割的默认行为。对于许多数据科学家来说，所描述的问题不是很明显，因为它主要发生在使用新的和更新的数据集重新训练 ML 模型的情况下。所以这并不是教科书中经常提到的东西，或者人们在使用示例数据集时不会遇到它，即使是来自 Kaggle 竞赛的数据集。我之前提到过，这对我们来说可能不是问题，因为这真的取决于用例。然而，我确实相信一个人应该意识到这一点，并且如果有这样的需要，应该知道如何修复它。

您可以在我的 [GitHub](https://github.com/erykml/medium_articles/blob/master/Machine%20Learning/hashing_train_test_split.ipynb) 上找到本文使用的代码。一如既往，我们欢迎任何建设性的反馈。你可以在推特上或者评论里联系我。

如果您喜欢这篇文章，您可能还会对以下内容感兴趣:

[](/lazy-predict-fit-and-evaluate-all-the-models-from-scikit-learn-with-a-single-line-of-code-7fe510c7281) [## 懒惰预测:拟合和评估 scikit 中的所有模型——用一行代码学习

### 查看哪些模型最适合您的数据集的最简单方法！

towardsdatascience.com](/lazy-predict-fit-and-evaluate-all-the-models-from-scikit-learn-with-a-single-line-of-code-7fe510c7281) [](/a-comprehensive-guide-to-debugging-python-scripts-in-vs-code-b9f9f777d4b8) [## 在 VS 代码中调试 Python 脚本的综合指南

### 了解如何在 10 分钟内高效调试您的脚本！

towardsdatascience.com](/a-comprehensive-guide-to-debugging-python-scripts-in-vs-code-b9f9f777d4b8) [](/5-free-tools-that-increase-my-productivity-c0fafbbbdd42) [## 5 个提高我工作效率的免费工具

### 这不是一个好的 IDE，尽管它很有帮助！

towardsdatascience.com](/5-free-tools-that-increase-my-productivity-c0fafbbbdd42) 

# 参考

*   Géron，A. (2019)。使用 Scikit-Learn、Keras 和 TensorFlow 进行机器学习:构建智能系统的概念、工具和技术。第二版，奥赖利媒体。