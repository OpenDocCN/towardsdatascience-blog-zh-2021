# 张量流与 Keras:比较

> 原文：<https://towardsdatascience.com/tensorflow-vs-keras-d51f2d68fdfc?source=collection_archive---------8----------------------->

## 查看两个机器学习库的具体细节

![](img/90539af1a073b2bd6349898d9d5e06f1.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Garett Mizunaka](https://unsplash.com/@garett3?utm_source=medium&utm_medium=referral) 拍摄的照片

当我回顾我以前的一些黑客马拉松项目时，我想起了我尝试过的一个机器学习的小样本。这并不是最有效的尝试，但是在没有任何相关知识的情况下，我的两人团队只有 24 小时，比我们开始编码的时间还少，来启动和运行这个项目。尽管我知道什么是基础水平的机器学习，但在那之前我从未尝试过任何项目。当我们决定使用什么库时，当然是使用 Python 库，我对可用选项的数量感到惊讶。

对于决定编写一个实现机器学习的项目的人来说，我认为更深入地理解现有的选项会很有趣。在这篇文章中，我将查看两个流行的机器学习库，并比较这两个库。这样，您可能更容易决定使用两个库中的哪一个。当决定在我的黑客马拉松项目中使用什么库时，我在 Keras 和 TensorFlow 之间犹豫不决。最终我还是和 TensorFlow 走了。但是在这篇博客中，我将重温这两者，以获得更完整的比较。

# **张量流**

TensorFlow 是一个开源库，用于训练和开发机器学习模型。更确切地说，它是一个符号数学库。TensorFlow 是一个端到端的平台，适合初学者和专家。TensorFlow 不仅灵活，而且提供多层抽象。它的 API 在高层和低层都可以运行。因为 TensorFlow 是 Google Brain 团队创建的，所以既有文档也有训练支持。

TensorFlow 用于创建数据流图。在这种结构中，数据通过在每个节点处理的图移动。这些节点是数学运算，连接代表一个张量或多维数据数组。这些操作是学习发生的地方，它可以训练神经网络进行机器学习、自然语言处理，甚至是基于偏微分方程的模拟。这些神经网络可用于图像识别、单词嵌入、递归神经网络、手写数字分类或序列到序列模型。发生的数学运算在 Python 中不会发生。相反，它们出现在 C++中，用强大的高性能二进制代码编写。Python 只是将一切联系在一起的编程抽象。因为是用 Python 写的，所以更便于开发者学习。尽管如此，由于内容的复杂性，TensorFlow 对初学者来说仍然很难学习。有了大量的教程和文档，任何致力于学习的人一旦克服了学习曲线就可以这样做。

# **Keras**

Keras 是一个强大的深度学习库，运行在 TensorFlow 等其他开源机器学习库之上，本身也是开源的。为了开发深度学习模型，Keras 采用了 Python 中的最小结构，使其更容易学习和快速编写。它是可扩展的，同时保持用户友好。Keras 创建的学习模型是离散的组件，这意味着它们可以以多种方式组合。Keras 还支持递归网络和卷积网络。神经网络也是用 Python 编写的，并且有大量的社区支持。

Keras 有大量的文档和开发人员指南。对于常见的用例，它可以最大限度地减少所需的用户操作数量。Keras 的一个目标是提供清晰的错误消息，让开发人员更容易采取适当的措施。虽然功能强大，但重点是简单性和便于不同知识的用户学习。由于这种方便和强大，Keras 在日常环境中使用得越来越普遍。不仅大学开始教授 Keras，而且 Keras 的某些部分也用于你每天可能会用到的应用程序的功能中，比如网飞、优步，甚至 Instacart。Keras 专注于保持模块化，这意味着它不处理底层计算。相反，这些被移交给另一个图书馆。如前所述，Keras 旨在构建其他机器学习库，但它并不需要这样做。Keras 可以独立运行，与 TensorFlow 等其他库分开。从长远来看，Keras 旨在编写在几种不同类型的机器学习技术中有用的人工神经网络，其主要目标是易于不同技能的开发人员学习和使用。

# **比较**

作为一个注意，重要的是要知道，比较 TensorFlow 和 Keras 可能不是最好的，在使用哪个方面，甚至在基本操作方面。这是因为 Keras 是在 TensorFlow 等其他开源库之上运行的。相反，Keras 在 TensorFlow 上更像是一个包装器，因为它更容易使用，而且界面在编写和理解方面更快。但是，如果您需要 Keras 没有的一些功能，您仍然可以使用 TensorFlow。为了便于比较，我们还是要看一些不同之处。

Keras 是在高级别上为 API 处理的，而 TensorFlow 同时具有高级别和低级别的功能。Keras 的重点是易于阅读和编写，并在它的简单性简洁的基础上的架构。相比之下，张量流非常强大，但不容易理解。在查看差异时，张量流要难学得多，也难理解得多。在数据集中，Keras 更适用于较小的数据集。但是，还是有一些可伸缩性的。对于 TensorFlow，数据集可以更大，但仍能以更高的水平运行。Keras 包括简单的网络，所以调试是简单和建设性的。在 TensorFlow 中，调试可能更加神秘，也更难进行。Keras 和 TensorFlow 都有训练模型，所以那里没有区别。在速度方面，TensorFlow 的速度很快，运行性能很高。因此，缩放 TensorFlow 要容易得多，也有效得多。对于 Keras 来说，虽然写得很简单，但它确实损失了一些速度和性能。这意味着与 TensorFlow 相比，Keras 速度更慢，性能更低。不过从受欢迎程度来说，Keras 更受欢迎，而 TensorFlow 是第二受欢迎的。Keras 大部分是用 Python 写的。相比之下，TensorFlow 是用 Python、C++和 CUDA 混合编写的。

# **用**哪个好

当决定使用哪一个时，它取决于你的项目的规模和复杂程度。如果你想快速简单地学习一些东西，Keras 可能是你更好的选择。然而，如果您正在寻找更高水平的性能，并且功能强大且易于扩展，TensorFlow 可能是您的首选，尽管有学习曲线。然而，因为Keras 可以构建在 TensorFlow 之上，它们可以在您的应用程序上一起工作，形成一个强大而简单的组合。

# **结论**

TensorFlow 是一个开源库，可以用来学习和开发机器学习模型。这是一个端到端平台，功能强大，运行性能卓越。Keras 也是开源的，但它是一个旨在构建在其他库之上的库。它的界面使得学习和编写机器学习模型变得更加容易，即使对于初学者也是如此。这种简单性牺牲了一些性能和速度，但弥补了创建模型所需的时间。

比较 TensorFlow 和 Keras 并不容易，因为 Keras 是在 TensorFlow 等框架上构建的。虽然 TensorFlow 的能力范围更广，但 Keras 对于开发者来说要容易得多。虽然 Keras 的网络简单，易于调试，但 TensorFlow 的理解和调试要困难得多。对于初学者来说，Keras 要容易学得多。但是对于更复杂的应用程序，TensorFlow 有更多的功能。

在决定使用哪一个时，考虑数据集的大小、项目可能需要扩展多少以及需要什么级别的性能是很重要的。然而，学习曲线也是需要考虑的。如果可以选择，两者一起使用。这样，您不仅可以享受 Keras 的简单性，还可以选择 TensorFlow 的性能和功能范围。回想我以前的黑客马拉松项目，Keras 是一个我们应该进一步考虑的库。只有 24 个小时，比 TensorFlow 容易学得多，学习曲线也小得多。对于您的项目，我希望比较 Keras 和 TensorFlow 是有用的，即使它们不是最容易比较的。当要决定你是否必须选择一个或另一个时，两者都用会给你带来简单和力量。Keras 旨在构建在 TensorFlow 等框架之上，因此真正使用两者是可能的。但是，如果您打算只选择一个，请考虑您需要的性能、数据集的大小、您将需要的功能，还要考虑您可能需要克服的困难和学习曲线。请随意留下您喜欢哪个库或者如何使用这两个库的评论。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

[](https://python.plainenglish.io/sending-error-emails-and-text-messages-in-python-b8e9a48e00ae) [## 用 Python 发送错误电子邮件和文本消息

### 杜绝错误的电子邮件龙卷风

python .平原英语. io](https://python.plainenglish.io/sending-error-emails-and-text-messages-in-python-b8e9a48e00ae) [](https://medium.com/codex/sessions-tokens-and-cookies-2fcae32bb7a3) [## 会话、令牌和 Cookies

### 讨论 web 开发的构件。

medium.com](https://medium.com/codex/sessions-tokens-and-cookies-2fcae32bb7a3) [](https://medium.com/codex/a-journey-with-kubernetes-84848e5bf195) [## 与 Kubernetes 的旅行

### 第 5 部分:安装 Jenkins

medium.com](https://medium.com/codex/a-journey-with-kubernetes-84848e5bf195) [](/introduction-to-postgresql-part-3-83f64fa68ed) [## Postgresql 简介:第 3 部分

### 插入、更新、删除的时间…

towardsdatascience.com](/introduction-to-postgresql-part-3-83f64fa68ed) [](https://python.plainenglish.io/hosting-your-own-pypi-a55f2a6eca4d) [## 托管您自己的 PyPi

### 是的，有可能。

python .平原英语. io](https://python.plainenglish.io/hosting-your-own-pypi-a55f2a6eca4d) 

参考资料:

[](https://www.tutorialspoint.com/keras/keras_introduction.htm) [## Keras -简介

### 深度学习是机器学习框架的主要分支之一。机器学习是对设计的研究…

www.tutorialspoint.com](https://www.tutorialspoint.com/keras/keras_introduction.htm) [](https://keras.io/) [## keras:Python 深度学习 API

### Keras 是为人类设计的 API，不是为机器设计的。Keras 遵循减少认知负荷的最佳实践:it…

keras.io](https://keras.io/) [](https://www.simplilearn.com/keras-vs-tensorflow-vs-pytorch-article) [## Keras vs Tensorflow vs Pytorch:流行的深度学习框架

### 深度学习是人工智能(AI)的一个子集，在过去几十年里，这个领域越来越受欢迎…

www.simplilearn.com](https://www.simplilearn.com/keras-vs-tensorflow-vs-pytorch-article) [](https://www.tensorflow.org/) [## 张量流

### 一个面向所有人的端到端开源机器学习平台。探索 TensorFlow 灵活的工具生态系统…

www.tensorflow.org](https://www.tensorflow.org/) [](https://www.infoworld.com/article/3278008/what-is-tensorflow-the-machine-learning-library-explained.html) [## 什么是张量流？机器学习库解释说

### 机器学习是一门复杂的学科。但是实现机器学习模型远没有那么令人畏惧和困难…

www.infoworld.com](https://www.infoworld.com/article/3278008/what-is-tensorflow-the-machine-learning-library-explained.html)