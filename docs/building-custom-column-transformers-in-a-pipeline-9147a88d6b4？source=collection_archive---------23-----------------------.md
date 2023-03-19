# 在管道中构建自定义列转换器

> 原文：<https://towardsdatascience.com/building-custom-column-transformers-in-a-pipeline-9147a88d6b4?source=collection_archive---------23----------------------->

## 向现有管道添加自定义转换。

![](img/4caad8408c4f3635a17dbfad73f1ee50.png)

[维克多](https://unsplash.com/@victor_g?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# **当标准变换不够时**

标准的 scikit-learn 库提供了许多不同的功能。然而，许多转换、修改和变更数据的常用函数已经存在，并且很容易与管道兼容。

这篇文章将讨论如何为管道构建定制的转换器。**有几个代码块可供复制。**最终的变压器具有您选择的超参数优化方法的优化参数。

Scikit-learn 并没有完全涵盖数据科学家需要的所有内容，大多数问题都需要根据手头的问题进行定制**。幸运的是，有专门为此定制的变压器。**

您可能会有一个直接的问题，为什么定制转换需要作为管道的一部分。或者，您可以在通过管道推送数据之前手动转换数据。然而，将转换合并到您的管道中有几个好处。

*   **管道是可重复和可扩展的。**从原始数据进行转换所执行的操作可以在另一台机器上从原始数据中以平稳的流程轻松重现。因此，您可以将工作负载分散到多台机器上，并分配计算负载。
*   **管道内的参数是可优化的。**虽然不是所有的转换都需要参数，但其他转换可能需要参数。例如，假设您想要测试某个要素的平方值或立方值在模型中的表现是否更好。然后，指数可以被参数化和优化，以确定最佳配置。

# **定制变压器需要什么**

创建自定义转换有几个考虑因素。第一，转换器应该被定义为一个类。这种设计创建了易于整合到管道中的框架。

该类继承自 **BaseEstimator 和 TransformerMixin 类。**这些类为自定义转换类提供一些基本功能，并确保与 sklearn 管道的兼容性。下一步是定义三个基本方法，一个 **__init__()** 方法、一个 **fit()** 方法和一个 **transform()** 方法。需要使用 **__init__()** 方法来初始化类，在管道拟合时调用 **fit()** 方法，在管道上调用 transform 或 fit 时调用 **transform()** 方法。

# **问题建构**

定制转换器的实验使用乳腺癌数据集。代码和转换很容易根据不同的用例进行调整，以满足您的问题的需要。数据被分成训练集、测试集和用于分类问题的“ROC_AUC”度量。

转换后，整个管道将进行随机超参数搜索。该搜索允许通过选择超参数搜索来有效地优化变换中的参数。关于不同的超参数搜索的更多细节，请参考我关于这个主题的帖子:

[](/hyperparameter-tuning-always-tune-your-models-7db7aeaf47e9) [## 超参数调整—始终调整您的模型

### 不要放弃免费的性能提升。

towardsdatascience.com](/hyperparameter-tuning-always-tune-your-models-7db7aeaf47e9) 

```
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split, RandomizedSearchCV
from sklearn.metrics import roc_auc_score
from sklearn.pipeline import make_pipeline
from sklearn.pipeline import Pipeline
from sklearn.datasets import load_breast_cancer
from sklearn.tree import DecisionTreeClassifier
from scipy.stats import randintTEST_SIZE = 0.1
RANDOM_STATE = 10
data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target
X = df.drop(['target'], axis=1)
y = df['target'].astype(float)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=TEST_SIZE, random_state=RANDOM_STATE, stratify=y)
```

# **定义自定义变换**

下面的代码块中定义的 CustomTransformer 类进行了一些修改，以提供高度的灵活性。应用了三种不同的变换，幂、对数和根。幂变换要求指定指数的次数。

**对于这些转换中的每一个，保持原始特征列。**这种构造有潜在的问题，因为转换后的特征和原始特征是相互依赖的。或者，您可以调整代码，用一个转换覆盖原始列，以消除这些依赖性。

要转换的列通过转换器中的“feature_names”参数确定。电力变压器的指数由“电力”参数决定。将您想要优化的参数定义为类的属性是至关重要的。**在超参数搜索和随后的优化过程中，只能修改类别的属性。**关于不同管道参数定义的更多细节，请参考我在完整 sklearn 管道上的帖子:

[](/automated-machine-learning-with-sklearn-pipelines-a2be2a0a6e1) [## 使用 Sklearn 管道的自动机器学习

### 一条管道来统治他们。

towardsdatascience.com](/automated-machine-learning-with-sklearn-pipelines-a2be2a0a6e1) 

```
from sklearn.base import BaseEstimator, TransformerMixinclass CustomTransformer(BaseEstimator, TransformerMixin):
    # List of features in 'feature_names' and the 'power' of the exponent transformation
    def __init__(self, feature_names, power):
        self.feature_names = feature_names
        self.power = power def fit(self, X, y=None):
        return self def transform(self, X, y=None):
        X_copy = X.copy()
        for feat in self.feature_names:
            X_copy[feat + '_power' + str(self.power)] = self.power_transformation(X_copy[feat])
            X_copy[feat + '_log'] = self.log_transformation(X_copy[feat])
            X_copy[feat + '_root'] = self.power_transformation(X_copy[feat])
        return X_copy

    def power_transformation(self, x_col):
        return np.power(x_col, self.power) def log_transformation(self, x_col):
        return np.log(x_col +0.0001) def root_transformation(self, x_col):
        return np.root( x_col)
```

# **在管道中使用变压器**

为流水线初始化几个元参数。这些包括折叠数、评分、指标、详细信息和并行化作业。请根据需要随时更新这些内容；然而，它们被初始化为以有限的计算能力运行。流水线是用决策树分类器和决策树的几个超参数分布建立的。

此外，我还包括了定制转换器的参数。首先是功率参数的随机分布，如前所述。接下来是一组不同特性名称的集合。**该设置仅允许对特征子集进行转换，而不是对所有特征进行转换**。由于使用的优化技术是随机超参数搜索，这三个特性列表将被随机选择并用于优化。

```
N_ITER = 5
K_FOLDS = 5
SCORING_METRIC = 'roc_auc'
VERBOSE = 1
N_JOBS = 1hyperparameter_dict = {
    'custom_transformer__power': randint(1,4),
    'custom_transformer__feature_names': [
        ['mean texture', 'mean perimeter', 'mean area'],
        ['smoothness error', 'compactness error', 'concavity error'],
        ['worst concave points', 'worst symmetry', 'worst fractal dimension'],
    ],
    'model__max_depth': randint(3,10),
    'model__max_features': ['sqrt'],
    'model__min_samples_split': randint(2,20),
}pipe = Pipeline(steps=[
    ('custom_transformer', CustomTransformer(feature_names=list(X.columns), power=2)),
    ('model', DecisionTreeClassifier())
])
optimal_model = RandomizedSearchCV(
    pipe, hyperparameter_dict, n_iter = N_ITER, cv=K_FOLDS,
    scoring=SCORING_METRIC, n_jobs = N_JOBS,
    return_train_score=True, verbose = VERBOSE
)
optimal_model.fit(X_train, y_train)
print(
    optimal_model.score(X_test, y_test),
    optimal_model.best_params_
)
```

# **结论**

这篇文章讨论了如何创建一个与 sklearn 管道兼容的自定义转换函数。此外，**这些变压器结合了优化参数**。

最终，标准 sklearn 包和函数中已经包含了许多功能。然而，肯定不是一切。可以有效地利用自定义转换来为现有管道增加额外的灵活性，并添加自定义功能。

如果你有兴趣阅读关于新颖的数据科学工具和理解机器学习算法的文章，可以考虑在 Medium 上关注我。

如果你对我的写作感兴趣，并想直接支持我，请通过以下链接订阅。这个链接确保我会收到你的会员费的一部分。

[](https://zjwarnes.medium.com/membership) [## 通过我的推荐链接加入 Medium-Zachary Warnes

### 阅读扎卡里·沃恩斯(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

zjwarnes.medium.com](https://zjwarnes.medium.com/membership)