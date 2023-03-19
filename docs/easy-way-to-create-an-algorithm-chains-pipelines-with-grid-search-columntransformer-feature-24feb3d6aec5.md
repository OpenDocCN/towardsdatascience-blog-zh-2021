# 创建算法链的简单方法——带网格搜索、列转换器、特征选择的管道

> 原文：<https://towardsdatascience.com/easy-way-to-create-an-algorithm-chains-pipelines-with-grid-search-columntransformer-feature-24feb3d6aec5?source=collection_archive---------16----------------------->

## 使用 python 实现设计编译流程的管道

```
***Table of Contents*****1\. Introduction
2\. Pipeline
3\. Pipeline with Grid Search
4\. Pipeline with ColumnTransformer, GridSearchCV
5\. Pipeline with Feature Selection**
```

![](img/a85de34498700b9ead2946011a579271.png)

帕特里克·亨德利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 1.介绍

为算法准备数据集、设计模型和调整算法的超参数(由开发人员自行决定)以概化模型并达到最佳精度值在之前的文章中已有提及。正如我们所知，对于模型预处理、数据预处理和调整算法的超参数，开发人员都有可供选择的解决方案。开发人员负责应用最合适的组合，并保持他的项目在准确性和通用性方面的最优化。本文包括使用 sklearn 提供的管道一次性实现所有这些提到的操作以及更多内容。python 实现支持所有的头。

# 2.管道

在其最基本的形式中，管道是用一行代码将指定的数据预处理操作和模型实现到数据集:

```
IN[1]
iris=load_iris()
iris_data  =iris.data
iris_target=iris.targetIN[2]
x_train,x_test,y_train,y_test = train_test_split(iris_data, iris_target,test_size=0.2, random_state=2021)
pip_iris = Pipeline([("scaler", RobustScaler()),("lr",LogisticRegression())])
pip_iris.fit(x_train,y_train)
iris_score=pip_iris.score(x_test,y_test)
print(iris_score) **OUT[2]
0.9333333333333333**
```

照常使用`train_test_split`分离虹膜数据集，并选择`RobustScaler()`作为已知为数值数据集的数据集的定标器方法，选择 LogisticRegression 作为分类器。管道也包含各种属性，如`.fit`、`.score`，就像网格搜索一样。训练数据集在创建的管道中用`.fit`命令拟合，分数用`.score`创建。

# 3.带网格搜索的管道

网格搜索评估算法中的超参数组合或任何具有已定义超参数的操作，通知用户具有各种属性的准确率或最佳超参数组合(更多信息点击[此处](/evaluating-all-possible-combinations-of-hyperparameters-grid-search-e41c7044e8e))。将 GridSearchCV 与管道结合使用是消除工作量和混乱的一种非常有效的方式。现在，让我们用各种超参数组合来测试我们在上面实现的逻辑回归算法:

```
IN[3]
x_train,x_test,y_train,y_test = train_test_split(iris_data, iris_target,test_size=0.2, random_state=2021)
pip_iris_gs = Pipeline([("scaler", RobustScaler()),("lr",LogisticRegression(solver='saga'))])param_grids={'lr__C':[0.001,0.1,2,10],
             'lr__penalty':['l1','l2']}gs=GridSearchCV(pip_iris_gs,param_grids)gs.fit(x_train,y_train)
test_score = gs.score(x_test,y_test)
print("test score:",test_score)
print("best parameters: ",gs.best_params_)
print("best score: ", gs.best_score_)
**OUT[3]
test score: 0.9333333333333333
best parameters:  {'lr__C': 2, 'lr__penalty': 'l1'}
best score:  0.9583333333333334**
```

除此之外，*‘C’*和*‘penalty’*值由用户通过创建字典定义为`param_grids`。后来，包含算法和缩放的管道作为估计器被添加到 GridSearchCV 中。训练数据集用`.fit` 命令训练，用`.score`评估。此外，还获得了关于借助 GridSearchCV 中的各种属性创建的模型的信息。

# 4.带 ColumnTransformer 的管道

到目前为止，只使用了只包含数字数据的数据集 iris 数据集。为了使情况更复杂，让我们使用玩具数据集，它包含数字和分类数据，并应用:

*   用`MinMaxScaler()`标准化“收入”栏
*   用`OneHotEncoder()`对分类列进行编码
*   将“年龄”列与宁滨分组。

首先，让我们快速浏览一下数据集:

```
IN[4]
toy = pd.read_csv('toy_dataset.csv')
toy_final=toy.drop(['Number'],axis=1)IN[5]
toy_final.isna().sum()
**OUT[5]
City       0
Gender     0
Age        0
Income     0
Illness    0
dtype: int64**IN[6]
numeric_cols=toy.select_dtypes(include=np.number).columns
print("numeric_cols:",numeric_cols)
categorical_cols=toy.select_dtypes(exclude=np.number).columns
print("categorical_cols:",categorical_cols)
print("shape:",toy_final.shape)
**OUT[6]
numeric_cols: Index(['Number', 'Age', 'Income'], dtype='object')
categorical_cols: Index(['City', 'Gender', 'Illness'], dtype='object')
shape: (150000, 5)**
```

现在让我们执行上面提到的操作:

```
IN[7]
bins = KBinsDiscretizer(n_bins=5, encode='onehot-dense', strategy='uniform')
ct = ColumnTransformer([
    ('normalization', MinMaxScaler(), ['Income']),
    ('binning', bins, ['Age']),
    ('categorical-to-numeric', OneHotEncoder(sparse=False, handle_unknown='ignore'), ['City','Gender'])
], remainder='drop')x_train, x_test, y_train, y_test = train_test_split(toy_final.drop('Illness', axis=1), toy_final.Illness,
                                                   test_size=0.2, random_state=0)param_grid_lr=[{'lr__solver':['saga'],'lr__C':[0.1,1,10],'lr__penalty':['elasticnet','l1','l2']},
               {'lr__solver':['lbfgs'],'lr__C':[0.1,1,10],'lr__penalty':['l2']}]IN[8]
pipe_lr = Pipeline([
    ('columntransform', ct),
    ('lr', LogisticRegression()),
    ])gs_lr =GridSearchCV(pipe_lr,param_grid_lr,cv=5)
gs_lr.fit(x_train,y_train)
test_score_lr = gs_lr.score(x_test,y_test)
print("test score:",test_score_lr)
print("best parameters: ",gs_lr.best_params_)
print("best score: ", gs_lr.best_score_)
**OUT[8]
test score: 0.9198666666666667
best parameters:  {'lr__C': 0.1, 'lr__penalty': 'l1', 'lr__solver': 'saga'}
best score:  0.9188750000000001**
```

sklearn 库中带有`KBinsDiscretizer()`的 bins 方法被设置为 5 组，并由 OneHotEncoder 编码。用`ColumnTransformer()`应用的预处理过程集中在一只手里。这些操作是:
-规范化为*、*列、
-离散化为*、【年龄】、
-用`OneHotEncoder()`编码为分类列*

然后，数据集被分成训练和测试两部分。使用选定的超参数创建字典(`param_grids_lr`)以评估参数组合。要应用的数据预处理方法由 ColumnTransformer 一手收集(更多信息请单击[此处](https://pub.towardsai.net/an-overview-of-data-preprocessing-converting-variables-column-transformers-onehotencoding-9ff521362159))并且算法-LogisticRegression-被放置在管道中。和上面的例子一样，通过在 GridSearchCV 中选择交叉验证值 5 来完成模型。

> `param_grid_lr`字典创建为算法+双下划线+超参数。`LogisticRegression()`定义为 lr，我们知道' *C* '是逻辑回归的超参数，所以使用 lr__C。要查看所有可用的超参数，应用`lr.get_params().keys()`。

现在让我们试试我们用 `DecisionTreeClassifier()`准备的模型:

```
IN[9]
pipe_dt = Pipeline([
    ('columntransform', ct),
    ('dt', DecisionTreeClassifier()),
])
param_grid_dt={'dt__max_depth':[2,3,4,5,6,7,8]}
gs_dt =GridSearchCV(pipe_dt,param_grid_dt,cv=5)
gs_dt.fit(x_train,y_train)
test_score_dt = gs_dt.score(x_test,y_test)
print("test score:",test_score_dt)
print("best parameters: ",gs_dt.best_params_)
print("best score: ", gs_dt.best_score_)
**OUT[9]
test score: 0.9198333333333333
best parameters:  {'dt__max_depth': 2}
best score:  0.9188750000000001**
```

我们选择的 *max_depth* 值被逐一拟合，通过网格搜索确定最成功的一个。

# 5.具有特征选择的管线

正如简介中提到的，使用管道和 GridSearchCV 是评估超参数组合并轻松编译它们的一种非常有效的方法。它不仅对于数据预处理和算法非常有用，而且对于数据清理(`SimpleImputer`)、特征处理(`SelectKBest`、`SelectPercentile` ，更多信息点击[此处](/an-overview-of-data-preprocessing-features-enrichment-automatic-feature-selection-60b0c12d75ad)等也非常有用。现在，让我们将以下内容应用于包含 30 个特征的乳腺癌数据集:

—用 `StandardScaler()`标准化数值

— `PolynomialFeatures()`以数值表示

—带`SelectPercentile()`的方差分析

—逻辑回归超参数( *C* 和*罚值*)

—调整交叉验证=3

```
IN[10]
cancer=load_breast_cancer()
cancer_data   =cancer.data
cancer_target =cancer.targetIN[11]
anova = SelectPercentile()
poly = PolynomialFeatures()
lr=LogisticRegression(solver='saga')param_grid_cancer=dict(poly__degree=[2,3,4],
                   anova__percentile=[20, 30, 40, 50],
                   lr__C=[0.01,0.1,1,10],
                   lr__penalty=['l1','l2']
                   )pipe_cancer = Pipeline([
    ('standardization',StandardScaler()),
    ('poly',poly),
    ('anova',anova),
    ('lr',lr)
    ])gs_final = GridSearchCV(pipe_cancer,param_grid_cancer,cv=3,n_jobs=-1)x_train, x_test, y_train, y_test = train_test_split(cancer_data, cancer_target,test_size=0.2,random_state=2021)gs_final.fit(x_train,y_train)
test_score_final = gs_final.score(x_test,y_test)
print("test score:",test_score_final)
print("best parameters: ",gs_final.best_params_)
print("best score: ", gs_final.best_score_)
**OUT[11]
test score: 0.9736842105263158
best parameters:  {'anova__percentile': 20, 'lr__C': 0.1, 'lr__penalty': 'l1', 'poly__degree': 2}
best score:  0.9626612059951203**
```

用`param_grid_cancer`测试的超参数组合已经定义:

`PolynomialFeatures()`的度数=[2，3，4]

`SelectPercentile()`的百分位数= [20，30，40，50]

对于`LogisticRegression()`，C=[0.01，0.1，1，10]

`LogisticRegression()`的惩罚=['l1 '，' l2']

这些都是用`StandardScaler()`输送进来的。然后在 GridSearchCV 中将交叉验证值设置为 3。数据集用`train_test_split`分割，并一如既往地用`.fit`装配。当 SelectPercentile 中的'*百分位*'设置为 20%，*C*logistic regression 中的值设置为 0.1，*penalty*logistic regression 中的参数设置为“L1”，多项式 Features 中的' *degree* '设置为 2 时，精度最高。

> 在评估从单一来源创建管道模型时需要的许多事情时，管道是有用的。`make_pipeline`可以和管道一样使用。`make_pipeline`自动为步骤创建必要的名称，因此只需添加流程即可。

## 回到指南点击[此处](https://ibrahimkovan.medium.com/machine-learning-guideline-959da5c6f73d)。

<https://ibrahimkovan.medium.com/machine-learning-guideline-959da5c6f73d> 