# Python 中高效机器学习的三种流行特征选择方法

> 原文：<https://towardsdatascience.com/four-popular-feature-selection-methods-for-efficient-machine-learning-in-python-fdd34762efdb?source=collection_archive---------13----------------------->

![](img/94342e2206b5fa8c6e1b6e205d5b425d.png)

叶夫根尼·切尔卡斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 使用真实数据集执行特征选择方法，并在每个方法后检索所选特征

特征选择是机器学习最重要的部分之一。在现实世界的大多数数据集中，可能有许多要素。但并不是所有的特征都是某个机器学习算法所必需的。使用太多不必要的功能可能会导致很多问题。第一个肯定是计算成本。不必要的大数据集将花费不必要的长时间来运行算法。同时，它可能导致完全没有预料到的过拟合问题。

有几种特征选择方法。我将在这里用 python 演示四种流行的特征选择方法。如果我们不得不从头开始执行，这将是一个漫长而耗时的过程。但是幸运的是，python 具有强大的功能，使得使用几行代码来执行特性选择变得非常容易。

在开始特征选择方法之前，请随意下载用于本练习的数据集:

  

让我们首先导入数据集:

```
import pandas as pd
import numpy as np
df = pd.read_csv('Feature_Selection.csv')
```

数据集太大。它有 108 列和 11933 行。因此，我不能在这里显示截图。以下是一些栏目:

```
df.columns
```

输出:

```
Index(['x.aidtst3', 'employ1', 'income2', 'weight2', 'height3', 'children','veteran3', 'blind', 'renthom1', 'sex1',
       ...
'x.denvst3', 'x.prace1', 'x.mrace1', 'x.exteth3', 'x.asthms1', 'x.michd', 'x.ltasth1', 'x.casthm1', 'x.state', 'havarth3'], dtype='object', length=108)
```

假设我们想要使用机器学习算法来预测‘hawarth 3’变量。这是 X 和 y。

```
X= df.drop(columns=["havarth3"])
y= df['havarth3']
```

现在，我们将使用不同的方法找出哪些特征是预测“havarth3”变量的最佳方法。

## [**单变量特征选择**](https://scikit-learn.org/stable/modules/feature_selection.html#univariate-feature-selection)

该方法基于单变量统计测试选择最佳特征。将用于此目的的函数是 sklearn 库中的 SelectKBest 函数。此函数移除除了顶部指定数量的特征之外的所有特征。在该数据集中，有 107 个要素。k 值 10 仅用于保留 10 个特征。选择 f_classif 的 score_function，它使用方差分析表中的 F 值来选择特征。还有另外两个分类选项。分别是 chi2 和 mutual_info_classif。请随时检查他们自己。

这是流程。

从 sklearn 库中导入方法。

```
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import f_classif
```

传递前面提到的 score_function 'f_classif '和您希望在' SelectKBest '函数中保留的特征数，并将 X 和 y 拟合到函数中:

```
uni = SelectKBest(score_func = f_classif, k = 10)
fit = uni.fit(X, y)
```

这是该函数选择的 10 个特征:

```
X.columns[fit.get_support(indices=True)].tolist()
```

输出:

```
['employ1',
 'rmvteth4',
 'genhlth',
 'x.age.g',
 'x.age80',
 'x.ageg5yr',
 'x.age65yr',
 'x.rfhlth',
 'x.phys14d',
 'x.hcvu651']
```

特征选择完成了！

## **使用相关矩阵的特征选择**

该过程计算所有特征与目标特征的相关性。基于这些相关值，选择特征。对于这个项目，阈值选择为 0.2。如果特征与目标的相关性超过 0.2，则选择该特征用于分类。

```
cor = df.corr()
cor_target = abs(cor["havarth3"])
relevant_features = cor_target[cor_target > 0.2]
relevant_features.index
```

输出:

```
Index(['employ1', 'genhlth', 'x.age.g', 'x.age80', 'x.ageg5yr', 'x.age65yr', 'x.hcvu651', 'havarth3'],
      dtype='object')
```

请注意,“havarth3”变量也被选中。因为‘hawarth 3’变量与自身的相关性最高。因此，请记住在执行机器学习算法之前删除它。

## 包装方法

这是一个有趣的方法。在这种方法中，使用一种机器学习方法来找到正确的特征。此方法使用 p 值。这里我将使用 statsmodels 库中的普通线性模型。我选择 statsmodels 库，因为它提供 p 值作为模型的一部分，而且我发现它很容易使用。

```
import statsmodels.api as sm
X_new = sm.add_constant(X)
model = sm.OLS(y, X_new).fit()
model.pvalues
```

输出:

```
const        2.132756e-01
x.aidtst3    6.269686e-01
employ1      4.025786e-20
income2      3.931291e-04
weight2      2.122768e-01
                 ...     
x.asthms1    3.445036e-01
x.michd      3.478433e-01
x.ltasth1    3.081917e-03
x.casthm1    9.802652e-01
x.state      6.724318e-01
Length: 108, dtype: float64
```

看所有的 p 值。现在，基于 p 值，我们将逐个移除特征。我们将继续运行 statsmodels 库中的机器学习算法，在每次迭代中，我们将找到具有最高 p 值的特征。如果最高 p 值大于 0.05，我们将移除该特征。将进行相同的过程，直到我们达到最高 p 值不再大于 0.05 的点。

```
selected_features = list(X.columns)
pmax = 1
while (len(selected_features)>0):
    p= []
    X_new = X[selected_features]
    X_new = sm.add_constant(X_new)
    model = sm.OLS(y,X_new).fit()
    p = pd.Series(model.pvalues.values[1:],index = selected_features)      
    pmax = max(p)
    feature_pmax = p.idxmax()
    if(pmax>0.05):
        selected_features.remove(feature_pmax)
    else:
        break
selected_features
```

输出:

```
['employ1',
 'income2',
 'blind',
 'sex1',
 'pneuvac4',
 'diffwalk',
 'diffdres',
 'smoke100',
 'rmvteth4',
 'physhlth',
 'menthlth',
 'hlthpln1',
 'genhlth',
 'persdoc2',
 'checkup1',
 'addepev2',
 'chcscncr',
 'asthma3',
 'qstlang',
 'x.metstat',
 'htin4',
 'wtkg3',
 'x.age.g',
 'x.ageg5yr',
 'x.age65yr',
 'x.chldcnt',
 'x.incomg',
 'x.rfseat3',
 'x.rfsmok3',
 'x.urbstat',
 'x.llcpwt2',
 'x.rfhlth',
 'x.imprace',
 'x.wt2rake',
 'x.strwt',
 'x.phys14d',
 'x.hcvu651',
 'x.denvst3',
 'x.prace1',
 'x.mrace1',
 'x.ltasth1']
```

所以，这些是选择的特征。

## 结论

我在我的一个项目中使用了所有四种方法，使用包装器方法得到了最好的结果。但并非所有项目都是如此。当您有很多像这个项目这样的特性时，尝试至少几种特性选择方法是一个好主意。不同的功能选择可能会给你完全不同的功能集。在这种情况下，使用另一种方法来确认是一个很好的解决方案。在机器学习中，收集正确的特征是成功的一半。

## 更多阅读:

</stochastic-gradient-descent-explanation-and-complete-implementation-from-scratch-a2c6a02f28bd>  </exploratory-data-analysis-of-text-data-including-visualization-and-sentiment-analysis-e46dda3dd260> [## 文本数据的探索性数据分析，包括可视化和情感分析

towardsdatascience.com](/exploratory-data-analysis-of-text-data-including-visualization-and-sentiment-analysis-e46dda3dd260) </an-ultimate-cheat-sheet-for-numpy-bb1112b0488f>  </an-ultimate-data-visualization-course-in-python-for-free-12a5da0a517b>  </a-collection-of-advanced-data-visualization-in-matplotlib-and-seaborn-f08136172e14> 