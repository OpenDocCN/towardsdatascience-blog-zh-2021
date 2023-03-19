# 机器学习的特征选择:3 类 12 种方法

> 原文：<https://towardsdatascience.com/feature-selection-for-machine-learning-3-categories-and-12-methods-6a4403f86543?source=collection_archive---------8----------------------->

![](img/cf6155bf4115d4fa87c76b63f4a83fab.png)

图片由 [Faye Cornish](https://unsplash.com/@fcornish) 在 [Unsplash](https://unsplash.com/photos/ywRNdDfvMWs) 上拍摄

这篇文章的大部分内容来自我最近的一篇题为:
“环境数据特征选择方法的评估”的论文，任何感兴趣的人都可以在这里[获得](https://www.sciencedirect.com/science/article/pii/S1574954121000157)。

# 降维的两种方法

有两种**方法**来减少特征的数量，也就是所谓的降维。

**第一种**方式被称为**特征提取**，其目的是转换特征并基于原始/给定特征的组合创建全新的特征。
最流行的方法是**主成分分析**、**线性判别分析**和**多维标度**。然而，新的特征空间几乎不能为我们提供关于原始特征的有用信息。新的更高层次的特征不容易被人类理解，因为我们不能将它们直接与初始特征联系起来，从而难以得出结论和解释变量。

**第二种**降维方式是**特征选择**。
它可以被认为是一个**预处理步骤**，并且不创建任何新特征，而是选择原始特征的一个子集，提供更好的**可解释性**。
从一个有意义的初始数字中找到最佳特征可以帮助我们提取有价值的信息**发现新知识**。
在分类问题中，特征的重要性根据其**解决不同类别**的能力进行评估。
给出每个特征在区分不同类别中的便利性的估计的属性被称为**特征相关性。**

# 特征选择的目标

特征选择有许多目标。
1。它**通过保留与目标变量具有最小冗余和最大相关性的特征来消除** **不相关和有噪声的特征**。
2。它**减少了训练和测试分类器的计算时间和复杂性**，因此它产生了更具成本效益的模型。
3。它**提高学习算法的性能**，避免过度拟合，并有助于创建更好的通用模型。

# 3 种特征选择方法

根据它们与分类器的交互方式，有三类特征选择方法，即过滤器、包装器和嵌入式方法。

**过滤方法**是可扩展的(直到非常高维的数据),并且在分类之前执行*快速*特征选择，使得*学习算法的偏差不会与特征选择算法的偏差*相互作用。他们主要充当排序者，将特性从最好到最差排序。
特征的排序取决于数据的*内在属性*，例如，方差、一致性、距离、信息、相关性等。
存在许多过滤方法，新的方法也在不断发展，每种方法都使用不同的标准来衡量数据的相关性。
相关性*的一个通用定义*是，如果一个特征有条件地独立于类别标签或者不影响类别标签，则该特征可以被认为是不相关的；在这些情况下，它可以被丢弃。

**包装器方法**使用机器学习算法作为*黑盒评估器*来寻找特征的最佳子集，因此，它们依赖于分类器。
实际上，搜索策略和建模算法的任何组合都可以用作包装器。
当在具有大量特征的数据集中执行包装时，它会消耗额外的计算资源和运行时间。
最后，这些方法实现起来很简单，并且可以对特征依赖进行建模。

**嵌入式方法** *在过滤器和包装器之间架起了桥梁*。首先，他们像过滤器一样融合可测量和统计标准来选择一些特征，然后使用机器学习算法，选择具有最佳分类性能的子集。
它们*降低了*包装器的计算复杂度，无需在每次迭代中对子集进行重新分类，并且可以对特征依赖性进行建模。他们*不执行迭代*。
特征选择在学习阶段进行，这意味着这些方法同时实现了*模型拟合和特征选择*。一个缺点是它们依赖于分类器。

# 过滤方法

## 1.卡方检验

基于常用χ统计检验的单变量过滤器，用于测量与预期分布的偏差(如果假设要素的出现实际上与类值无关)。
像任何单变量方法一样，我们计算每个特征和目标变量之间的卡方，并观察它们之间的关系。
如果目标变量与特征无关，则得分较低，或者如果它们是相关的，则特征很重要。
卡方值越高，表示要素与类别的相关性越高。

要使用该方法，安装 *scikit-learn* 。

> 对于所有代码示例，我们假设 X 是特性的熊猫数据框架，y 是目标的熊猫系列。

```
!pip install scikit-learn
from sklearn.feature_selection import chi2
X_norm = MinMaxScaler().fit_transform(X)
chi_selector = SelectKBest(chi2, k='all')
chi_selector.fit(X_norm, y)
```

## 2.交互信息

这个方法就是一个滤波器，也叫*信息增益*。
它是最流行和最常用的单变量特征选择方法之一，因为它的计算速度和*简单的公式*。
它测量每次考虑单个特征时熵的减少。
如果一个特征具有高信息增益，则该特征被认为是相关的。
它不能处理冗余特征，因为特征是以单变量方式选择的。
因此，它属于“近视”方法，在这种方法中，独立地评估特征而不考虑它们的上下文。

要使用该方法，安装 *scikit-learn* 。

```
!pip install scikit-learn
from sklearn.feature_selection import mutual_info_classif
mi_selector = SelectKBest(mutual_info_classif, k='all')
mi_selector.fit(X, y)
```

## 3.方差分析 F 值

这是一种单变量过滤方法，使用*方差*来找出类之间的个体特征的可分性。
适用于多类端点。

要使用该方法，安装 *scikit-learn* 。

```
!pip install scikit-learn
from sklearn.feature_selection import f_classif
anov_selector = SelectKBest(f_classif, k='all')
anov_selector.fit(X, y)
```

## 4.方差阈值

这种过滤方法并不总是被认为是一种要素选择方法，因为它的标准并不是在每个数据集中都能满足。
移除*变化低于某个截止值*的特征。这个想法是，当一个特征在样本间变化不大时，它通常没有什么预测能力。

要使用该方法，安装 *scikit-learn* 。

```
!pip install scikit-learn
from sklearn.feature_selection import VarianceThreshold
var_selector = VarianceThreshold(*threshold=*0)
var_selector.fit_transform(X)
```

## 5.费希尔评分

这是一种使用均值和方差对要素进行分级的过滤方法。相同类的实例中具有相似值而不同类的实例中具有不同值的特征被认为是最佳的。与之前的单变量方法一样，它单独评估特征，并且它不能处理特征冗余。

要使用该方法，请安装 *skfeature-chappers。*

```
!pip install skfeature-chappers
from skfeature.function.similarity_based import fisher_score
score = fisher_score.fisher_score(X.to_numpy(), y.to_numpy())
```

## 6.多重表面

它是一种扩展的 ReliefF 及其多类扩展 ReliefF 滤波器的特征选择方法。
原始 relief 的工作方式是从数据集中随机抽取一个
实例，然后从
相同和相反的类中定位其最近的邻居。
将最近邻属性的值与采样实例进行比较，以更新每个属性的相关性分数。
基本原理是一个有用的属性应该区分来自不同类的实例，并且对于来自相同类的实例应该有相同的值。
在所有 relief 扩展中，MultiSURF 在各种问题类型中提供了最可靠的特征选择性能。

要使用该方法，请安装 *skrebate* 。

```
!pip install skrebate
from skrebate import MultiSURF
fs = MultiSURF(n_jobs=-1, n_features_to_select=featureCutoff)
fs.fit(X.values, y.values)
```

# 包装方法

## 1.递归特征消除

这种广泛使用的包装方法使用一种算法来迭代地训练模型，并且每次都使用算法的权重作为标准来移除最不重要的特征。
这是一种多变量方法，它评估共同考虑的几个特征的相关性。
当用作排序器时，在每次迭代中，被移除的特性被添加到堆栈中，直到所有特性都被测试。
为了提高计算效率，可以在单个步骤中移除多个特征。
这种方法计算速度慢。

要使用该方法，安装 *scikit-learn* 。

```
!pip install scikit-learn
from sklearn.feature_selection import RFE
rfe_selector = RFE(estimator=LogisticRegression(), 
                   n_features_to_select=1, step=1, verbose=-1)
rfe_selector.fit(X_norm, y)
```

## 2.排列重要性

排列重要性是一种用于规范化特征重要性度量的启发式方法，可以纠正特征重要性偏差。
该方法基于结果向量的重复排列，用于
估算非信息环境中每个变量的测量重要性分布。
观察到的重要性的 p 值提供了特征重要性的校正度量。

要使用该方法，安装 *scikit-learn* 和 *eli5* 。使用选择的算法。这里我使用了逻辑回归。

```
!pip install eli5
!pip install scikit-learn
from eli5.sklearn import PermutationImportance
from sklearn.feature_selection import SelectFromModel
from sklearn.linear_model import LogisticRegression
perm = PermutationImportance(LogisticRegression(), random_state=42, cv=10)
perm.fit(X, y)
perm_selector = SelectFromModel(perm,max_features=featureCutoff).fit(X, y)
```

## 3.SHAP

SHapley 附加解释是一种解释任何机器学习模型输出的统一方法。
它将博弈论与局部解释联系起来，联合了以前的几种方法，代表了唯一可能一致且局部精确的基于期望的加性特征归因方法。
近年来已经成为事实上的特征选择方法。

要使用该方法，安装*shap**并使用选择的算法。这里我用了 xgboost。*

```
*!pip install shap
import shap
import xgboost
model = xgboost.train({"learning_rate": 0.01}, xgboost.DMatrix(X, label=y), 100)
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X)*
```

## *4.博鲁塔*

*设计为随机森林包装器的算法。
它迭代地移除被统计测试证明为不如随机探针相关的特征*

*要使用该方法，请安装*Boruta**并使用算法。这里我使用随机森林。**

```
**!pip install Boruta
from boruta import BorutaPy
rf = RandomForestClassifier(n_jobs=-1, class_weight='balanced', max_depth=5)
boru_selector = BorutaPy(rf, n_estimators='auto', verbose=0, random_state=1)
boru_selector.fit(X.values, y.values)**
```

# **嵌入式方法**

## **1.嵌入式随机森林**

**这种嵌入式特征选择使用随机森林算法。
通过随机置换出袋样本中的特征，并计算与所有变量保持不变的出袋率相比，误分类率的增加百分比，来测量特征的重要性。**

**要使用该方法，安装 *scikit-learn* 。**

```
**!pip install scikit-learn
from sklearn.feature_selection SelectFromModel
from sklearn.ensemble import RandomForestClassifier
embeded_rf_selector = SelectFromModel(RandomForestClassifier(n_estimators=100, random_state=42), max_features=featureCutoff)
embeded_rf_selector.fit(X, y)**
```

## **2.嵌入式 LightGBM**

**这种嵌入式特征选择使用流行的 LGB 算法。**

**要使用该方法，安装 *scikit-learn* 和 *lightgbm* 。**

```
**!pip install lightgbm
from lightgbm import LGBMClassifier
from sklearn.feature_selection SelectFromModel
lgbc=LGBMClassifier(n_estimators=500, learning_rate=0.05,
                    num_leaves=32, colsample_bytree=0.2,                                           
                    reg_alpha=3, reg_lambda=1, min_split_gain=0.01,    
                    min_child_weight=40)
embeded_lgb_selector = SelectFromModel(lgbc, max_features=featureCutoff)
embeded_lgb_selector.fit(X, y)**
```

# **不确定接下来要读什么？这里有两个选择:**

**</time-series-analysis-with-theory-plots-and-code-part-1-dd3ea417d8c4>  </outlier-detection-theory-visualizations-and-code-a4fd39de540c>  

# 保持联络

跟随我在 [Medium](https://medium.com/@dimitris.effrosynidis) 上获取更多类似的内容。
我们在 [LinkedIn](https://www.linkedin.com/in/dimitrios-effrosynidis/) 上连线吧。
检查我的 [GitHub](https://github.com/Deffro) 。**