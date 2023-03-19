# 幕府将军:一个被低估的 Python 机器学习包

> 原文：<https://towardsdatascience.com/shogun-an-underrated-python-machine-learning-package-d306ac07b0e0?source=collection_archive---------36----------------------->

## 看看 Python 的幕府包，以及为什么它是 Python 中机器学习的一个很少使用的库

![](img/392a81aff5f35b5cc3bea79947b7a7d3.png)

(src =[https://pixabay.com/images/id-1556912/](https://pixabay.com/images/id-1556912/)

# 介绍

在 Python 这个奇妙的世界中，有许多选项可以用来完成机器学习的任务。流行的例子通常包括使用 SkLearn 和 Tensorflow 这样的模块。SkLearn 是一个有用的模块，因为它可以用来解决预建机器学习模型的许多问题，这些模型试图通过使用一致的超参数来避免黑箱问题。然而，在这方面，SkLearn 肯定不是以这种方式有效解决问题的唯一解决方案。

虽然我们经常认为这是理所当然的，但是 Pythonic 包生态系统绝对是巨大的。当我们想以一种新的方式处理模型，或者只是从一个不同的 API 处理模型时，这就很方便了。我遇到的 SkLearn 包的最酷的替代品之一是一个叫做幕府将军的模块。幕府将军是一个基于 C++的库，使用与 SkLearn 相同的预构建概念。

# 为什么用幕府？

当然，由于幕府的用户群和维护者列表较小，使用这个包会有一些主要的缺点。文档就是一个很好的例子。没有文档，有效地使用任何用于机器学习的软件都是非常困难的，不管是不是最先进的。在幕府文档中已经投入了大量的工作，但是这将永远无法达到技术写作和组织的水平，像 SkLearn 这样一个得到良好支持的库已经工作了十多年了。由于缺乏文档和更大的用户群，另一个问题是缺乏支持。幕府将军类中可能会有抛出返回不等于零的东西，而没有关于为什么会发生异常的详细信息。当在 SkLearn 这样的包中发生这种情况时，可以很容易地搜索 throw 并找出您的代码到底有什么问题。另一方面，对于幕府将军来说，情况往往不是这样。

使用幕府将军而不是 SkLearn 的一个优点是，该库在语言方面更加成熟，这使得跨平台和在通常不能使用 SkLearn 的不同应用程序中更容易访问。Python、Javascript、C++、R、Ruby、C# mono 等语言都有实现，这使得幕府将军这个包更有可能被移植到任何其他可能需要使用的语言中。另一方面，SkLearn 主要是专门为 Python 用 C 编写的。当然，这意味着在大多数 C 衍生语言中都有通向该语言的桥梁，例如 C++或 C#，但它也不能与幕府将军在这方面的能力所提供的可访问性相提并论。考虑到这一点，可能需要注意的是，SkLearn 主要是为 Pythonic 数据科学家服务的，并且可能是他们个人的最佳选择。也就是说，我仍然认为交替使用两个包和它们的代码库是有好处的。

# 使用幕府将军

为了使用幕府包，我们当然需要安装 PIP。我使用的是 Ubuntu，以下是我个人的安装过程。我首先需要添加幕府工具 PPA:

```
sudo add-apt-repository ppa:shogun-toolbox/stable
sudo apt-get update
```

之后我分别装了 lib 幕府 18，然后 python-幕府。

```
sudo apt-get install libshogun18
sudo apt-get install python-shogun
```

关于机器学习对幕府的实际使用，首先要注意的是幕府是非常基于类型的，比 SkLearn 更是如此，例如，在 SkLearn 中，通常使用已经包含在其他模块(如 NumPy 或 Pandas)中的类型。这是一个重要的区别，很可能源于幕府将军在 C++中的独创性。此外，我认为这肯定有助于从如此多的不同 API 使用幕府将军，因为它真的不需要担心从许多不同的语言促进类型，而是更担心与自己的泛型类型工作。

对于今天的例子，我真的想展示一些我们在 Python 中使用的其他机器学习包并不普遍可用的东西。很可能大多数 SkLearn 用户都熟悉决策树和随机森林分类器的概念，它们使用 gini 杂质来计算模型的下一次分裂。幕府包中的一个非常酷的替代品是 CHAID，或者卡方自动交互检测器，tree。CHAID 树没有使用基尼指数，而是使用卡方检验来计算每一个分裂。这就是为什么我选择这个模型来进行讨论，因为它是不同的，它显示了你可以用幕府将军做很多用 SkLearn 做不到的事情。当然，为了开始这个项目，我们将在 Python 中引入科学计算的动态二重奏:

```
import pandas as pd
import numpy as np
```

我还将使用 SkLearn 中的 train-test-split 方法和 Ordinal Encoder 类，以便准备我的数据用于这些类型和这个模型。

```
from sklearn.preprocessing import OrdinalEncoder
from sklearn.model_selection import train_test_split
```

现在是时候准备我的数据，把它放入新的幕府类型，然后把它传递到我的模型。我不打算过多地描述我用于处理数据的代码，但是您可以随意检查我做了什么:

```
df = pd.read_csv("LatheBooks/atlcrime.csv")
df.head()
df = df.dropna()
target = "neighborhood"
feature = "feat"
encoder = OrdinalEncoder()
df["feat"] = encoder.fit_transform(np.array(df["npu"]).reshape(-1, 1))
train, test = train_test_split(df)
trainX = train[feature]
trainy = train[target]
testX = test[feature]
testy = test[target]
```

现在我们需要将这些数据放入新的特征类型中，然后我们就可以最终拟合我们的 Chi 树了！

```
features_train = RealFeatures(f_feats_train)
features_test = RealFeatures(f_feats_test)
labels_train = MulticlassLabels(f_labels_train)
labels_test = MulticlassLabels(f_labels_test)
```

接下来，我们需要设置我们的特征类型。这方面有三种选择:

*   0 —标称值
*   1-序数
*   2-连续

如果我们要处理多个特性，我们将需要设置许多特性类型，但是因为我们只处理一个顺序编码的特性，我们可以只提供一个顺序为 1 的迭代列表。

```
ft = [1]
```

现在我们可以初始化新的 CHAID 树了:

```
classifier = CHAIDTree(0, ft, 10)
classifier.set_labels(labels_train)
```

然后训练和预测:

```
classifier.train(features_train)
labels_predict = classifier.apply_multiclass(features_test)
```

# 结论

幕府将军是一个非常棒的 Python 机器学习包。也就是说，我当然可以理解为什么与 Python 中的许多主流解决方案相比，它很少被使用。不管怎样，我很高兴能把这个包中的知识带回 C++中，并从这个包中得到一点乐趣。值得添加到您的 Python 安装中吗？有一些很酷的模型，我认为很难在其他地方找到，我认为可能对某些事情有好处。许可证也是 GPL3，它可能会阻止这个包在某些情况下被使用。话虽如此，我认为这是一个很酷的包检查出来！非常感谢您的阅读，我希望这篇文章影响了更多的探索！