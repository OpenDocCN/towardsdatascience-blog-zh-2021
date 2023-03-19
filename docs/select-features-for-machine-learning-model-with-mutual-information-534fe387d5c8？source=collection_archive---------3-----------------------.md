# 基于互信息的机器学习模型特征选择

> 原文：<https://towardsdatascience.com/select-features-for-machine-learning-model-with-mutual-information-534fe387d5c8?source=collection_archive---------3----------------------->

## 为什么我们需要为机器学习模型挑选特征，如何使用互信息来选择特征，以及使用 Scikit-Learn 包中的互信息工具。

![](img/f2a3a99e4ac0c99fcb807cfa91ce1129.png)

图片来自[unsplash.com](https://unsplash.com/photos/hJ5uMIRNg5k)

# 特征选择问题

当用一组适当的训练数据进行训练时，机器学习模型是惊人的。教科书中描述的 ML 模型和使用来自 Scikit-learn 的数据集，样本数据集已经被仔细策划并有效地用于 ML 模型。

问题是，在现实生活中，训练数据可能是海量的，根本不适合任何模型。例如，您希望训练一个模型，根据当前 100 多个客户属性(如年龄、位置、国家/地区、活跃访问次数、上次访问时间等等)来预测下个月的收入。

有些属性可能有用，有些可能完全没用，比如客户名称。

难怪许多 ML 专家说，为 ML 选择正确的特性是最重要的步骤之一，甚至比 ML 模型本身还重要！

# 相互信息是如何工作的

互信息可以回答这个问题:有没有一种方法可以在一个特性和目标之间建立一个可测量的联系。

使用互信息作为特征选择器有两个好处:

*   MI 是模型中立的，这意味着该解决方案可以应用于各种 ML 模型。
*   MI 解的快。

那么，什么是互信息呢？

如果你熟悉决策树分类器。它与我在另一篇文章[中描述的信息增益 100%相同，理解决策树分类器](/understand-decision-tree-classifier-8a7497d4c5b3)。

互信息测量目标值条件下的**熵下降**。我发现对这个概念最干净的解释是这个公式:

```
MI(feature;target) = Entropy(feature) - Entropy(feature|target)
```

MI 分数将在从 0 到∞的范围内。值越高，该特征和目标之间的联系越紧密，这表明我们应该将该特征放在训练数据集中。如果 MI 分数为 0 或非常低，如 0.01。低分表明该特征和目标之间的联系很弱。

# 使用 Scikit 中的交互信息——学习 Python

您可以自己编写一个 MI 函数，或者使用 Scikit-Learn 中现成的函数。

我将使用来自 Scikit-Learn 的[乳腺癌数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_breast_cancer.html)来构建一个应用了互信息的样本 ML 模型。使用[决策树分类器](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)从癌症数据中训练 3 个数据集，并比较结果，以查看 MI 分数将如何影响 ML 模型的有效性。

1.  训练数据集 1，使用所有特征。
2.  训练数据集 2，仅使用 MI 分数大于 0.2 的要素
3.  训练数据集 3，仅使用 MI 分数小于 0.2 的要素

**第一步。加载乳腺癌数据**

```
from sklearn.datasets import load_breast_cancer as LBC
cancer = LBC()
X = cancer['data']
y = cancer['target']
```

**第二步。计算 MI 分数**

```
from sklearn.feature_selection import mutual_info_classif as MIC
mi_score = MIC(X,y)
print(mi_score)
```

您将看到 mi_score 数组，如下所示:

```
[0.37032947 0.09670886 0.40294198 0.36009957 0.08427789 0.21318114
 0.37337734 0.43985571 0.06456878 0.00276314 0.24866738 0.00189163
 0.27600984 0.33955538 0.01503326 0.07603828 0.11825812 0.12879402
 0.0096701  0.03802394 0.45151801 0.12293047 0.47595645 0.46426102
 0.09558928 0.22647456 0.31469449 0.43696443 0.0971793  0.06735096]
```

30 个数字代表 30 个特征的 MI 分数。

**第三步。构建 3 个训练数据和测试数据集**

无 MI 评分治疗的数据集 1，名称标有`_1`。注意，所有三个数据集将使用相同的`y_train`和`y_test`。因此，不需要分离目标数据。

```
from sklearn.model_selection import train_test_split as tts
X_train_1,X_test_1,y_train,y_test = tts(
    X,y
    ,random_state=0
    ,stratify=y
)
```

包含 MI 分数大于 0.2 的要素的数据集 2

```
import numpy as np
mi_score_selected_index = np.where(mi_scores >0.2)[0]
X_2 = X[:,mi_score_selected_index]
X_train_2,X_test_2,y_train,y_test = tts(
    X_2,y
    ,random_state=0
    ,stratify=y
)
```

你会看到 X_2 的形状是`(569,15)`而不是`(569,30)`。这是因为 15 个特征的 Mi 分数大于`0.2`。

包含 Mi 分数小于 0.2 的要素的数据集 3

```
mi_score_selected_index = np.where(mi_scores < 0.2)[0]
X_3 = X[:,mi_score_selected_index]
X_train_3,X_test_3,y_train,y_test = tts(
    X_3,y
    ,random_state=0
    ,stratify=y
)
```

巧合的是，0.2 MI 分数将 30 个特征分成 15 个和 15 个。

**第四步。用决策树分类器比较 3 个数据集**

```
from sklearn.tree import DecisionTreeClassifier as DTC
model_1 = DTC().fit(X_train_1,y_train)
model_2 = DTC().fit(X_train_2,y_train)
model_3 = DTC().fit(X_train_3,y_train)
score_1 = model_1.score(X_test_1,y_test)
score_2 = model_2.score(X_test_2,y_test)
score_3 = model_3.score(X_test_3,y_test)
print(f"score_1:{score_1}\n score_2:{score_2}\n score_3:{score_3}")
```

结果是:

```
score_1:0.9300699300699301
score_2:0.9370629370629371
score_3:0.8251748251748252
```

看，包含 15 个要素的数据集 2 的 MI > 0.2，精度达到 0.93，与包含所有要素的数据集 1 一样好。而 score_3 仅为 0.82，这是 15 个特征的结果，这些特征具有 MI score < 0.2.

From this sample, it is clear that the MI score can be used as a signal for feature selection.

# Use the feature selector from Scikit-Learn

In real ML projects, you may want to use the top n features, or top n percentile features instead of using a specified number 0.2 like the sample above. Scikit-Learn also provides [许多选择器](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.feature_selection)作为方便的工具。以便您不必手动计算 MI 分数并获取所需的特征。下面是选择前 50%特性的示例，其他选择器也有类似的用法。

```
from sklearn.feature_selection import SelectPercentile as SP
selector = SP(percentile=50) # select features with top 50% MI scores
selector.fit(X,y)
X_4 = selector.transform(X)
X_train_4,X_test_4,y_train,y_test = tts(
    X_4,y
    ,random_state=0
    ,stratify=y
)
model_4 = DTC().fit(X_train_4,y_train)
score_4 = model_4.score(X_test_4,y_test)
print(f"score_4:{score_4}")
```

# 参考链接

*   [机器学习的信息增益和互信息](https://machinelearningmastery.com/information-gain-and-mutual-information/)
*   [1.13。功能选择](https://scikit-learn.org/stable/modules/feature_selection.html)
*   [理解决策树分类器](/understand-decision-tree-classifier-8a7497d4c5b3)

# 附录——准则

您可以在 Jupyter 笔记本或 [vscode python interactive](https://code.visualstudio.com/docs/python/jupyter-support-py) 窗口中复制并运行以下代码。

```
#%% load cancer data
from sklearn.datasets import load_breast_cancer as LBC
cancer = LBC()
X = cancer['data']
y = cancer['target']

#%% compute MI scores
from sklearn.feature_selection import mutual_info_classif as MIC
mi_scores = MIC(X,y)
print(mi_scores)

#%% prepare dataset 1
from sklearn.model_selection import train_test_split as tts
X_train_1,X_test_1,y_train,y_test = tts(
    X,y
    ,random_state=0
    ,stratify=y
)

#%% prepare dataset 2, MI > 0.2
import numpy as np
mi_score_selected_index = np.where(mi_scores >0.2)[0]
X_2 = X[:,mi_score_selected_index]
X_train_2,X_test_2,y_train,y_test = tts(
    X_2,y
    ,random_state=0
    ,stratify=y
)

#%% prepare dataset 3, MI <0.2
mi_score_selected_index = np.where(mi_scores < 0.2)[0]
X_3 = X[:,mi_score_selected_index]
X_train_3,X_test_3,y_train,y_test = tts(
    X_3,y
    ,random_state=0
    ,stratify=y
)

#%% compare results with Decision Tree Classifier
from sklearn.tree import DecisionTreeClassifier as DTC
model_1 = DTC().fit(X_train_1,y_train)
model_2 = DTC().fit(X_train_2,y_train)
model_3 = DTC().fit(X_train_3,y_train)
score_1 = model_1.score(X_test_1,y_test)
score_2 = model_2.score(X_test_2,y_test)
score_3 = model_3.score(X_test_3,y_test)
print(f"score_1:{score_1}\n score_2:{score_2}\n score_3:{score_3}")

#%% use Scikit-learn feature selector
from sklearn.feature_selection import SelectPercentile as SP
selector = SP(percentile=50) # select features with top 50% MI scores
selector.fit(X,y)
X_4 = selector.transform(X)
X_train_4,X_test_4,y_train,y_test = tts(
    X_4,y
    ,random_state=0
    ,stratify=y
)
model_4 = DTC().fit(X_train_4,y_train)
score_4 = model_4.score(X_test_4,y_test)
print(f"score_4:{score_4}")
```