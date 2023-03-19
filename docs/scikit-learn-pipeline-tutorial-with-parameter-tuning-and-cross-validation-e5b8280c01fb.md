# sci kit-学习管道教程，包括参数调整和交叉验证

> 原文：<https://towardsdatascience.com/scikit-learn-pipeline-tutorial-with-parameter-tuning-and-cross-validation-e5b8280c01fb?source=collection_archive---------11----------------------->

## 在机器学习项目中，对用于训练和验证目的的不同数据集应用预处理步骤通常是一个问题——sci kit-learn 管道功能有助于解决这个问题

![](img/c95343d4fdea6ce6d394d19a8ce948e6.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

什么是机器学习工作流？—它包括所有的预处理步骤，如一键编码、标签编码、缺失值插补，然后是任何特征选择步骤，如选择最佳或递归特征消除(RFE)，然后是模型开发和验证步骤—所有这些都构成了一个机器学习工作流。

这个工作流程面临哪些挑战？—一个主要挑战是将相同的变换函数应用于训练、测试和验证数据集。在干净的代码中，优化的方式是使用用户定义的函数，不是每个数据科学家都愿意编写有效的函数。

对于这个问题，scikit-learn Pipeline 特性是一个开箱即用的解决方案，它可以在没有任何用户定义函数的情况下实现干净的代码。

让我用一个示例数据集来演示管道是如何工作的。我采用了一个关于信贷审批的 UCI 机器学习数据集，其中混合了分类列和数字列。

```
data = pd.read_csv('bank-full.csv', sep=';')
target = data.pop('y')
target = target.map({'yes': 1, 'no':0})
X_train, X_test, y_train, y_test = train_test_split(data, target, test_size=0.2, random_state=1234)
```

在我的真实项目中，我有 40 多个分类特征，有些有超过 50 个类别，为此我必须使用 OrdinalEncoder。为此类功能创建一次性编码将导致内存错误。对于演示数据集，我有两列想要应用 ordinal encoder——月份和教育资格——这两列都有意义，因为它们是有序值。

```
categorical_mask = (data.dtypes=='object')
categorical_columns = data.columns[categorical_mask].tolist()
num_cols = data.select_dtypes(include=['int64','float64']).columns.tolist()oe_cols = [c for c in categorical_columns if data[c].nunique()>5]
ohe_cols = [c for c in categorical_columns if data[c].nunique()<=5]
len(oe_cols), len(ohe_cols), len(num_cols)
```

在分离列之后，我们现在从每个分类列的初始数据集中获得一个唯一值列表，以便可以在定型、测试和验证数据集之间使用相同的值。这也将给出所有值的详尽列表。人们经常发现，有些值存在于训练中，而不存在于验证中(反之亦然)。为了避免这个问题，我们从初始数据集中取唯一值。有些人可能会质疑建模时可能会有数据泄漏，但是我们是单独应用转换的。

我们现在定义要使用的转换器类型——OneHotEncoder、OrdinalEncoder、simple imputr——我们也可以在此步骤中使用某种缩放函数，如 MinMaxScaler / StandardScaler。

```
ohe_unique_list = [data[c].unique().tolist() for c in ohe_cols]
oe_unique_list = [data[c].unique().tolist() for c in oe_cols]ohe = OneHotEncoder(categories=ohe_unique_list)
oe = OrdinalEncoder(categories=oe_unique_list)
imp = SimpleImputer(strategy='constant', fill_value=0)
```

我们使用`scikit-learn`的`make_column_transformer`函数来创建预处理列转换器。此外，我们定义了一个参数`remainder='passthrough'`，让没有任何转换器标准的所有其他列通过。我们可以使用像`drop`这样的其他值来删除任何没有预处理步骤的列。

正如我最初所说的，我有如此多的预测变量，我必须进行特征选择，为此，我尝试了 2 个函数——select kbest 和 recursivefeaturelimination(RFE)。SelectKBest 简单快捷。并且它需要像`f_classif`、`chi2`这样的函数来找到最佳特性。而 RecursiveFeatureElimination 是一个缓慢的过程，它试图一个接一个地移除特征以找到最佳特征。

在特性选择步骤的顶部，我使用 XGBoost 作为估计器来预测概率。

现在所有这些都被定义为流水线的步骤。因此，如果我们调用管道，它将对数据集进行预处理、特征选择和模型拟合。

```
preprocess = make_column_transformer(
    (oe, oe_cols),
    (ohe, ohe_cols),
    (imp, num_cols),
    remainder='passthrough'
)
estimator = XGBClassifier(learning_rate=0.05, max_depth=3, n_estimators=2500, random_state=1234)
fs = SelectKBest(score_func=f_classif, k=5)
selector = RFE(estimator, n_features_to_select=5, step=1)steps = [
    ('preprocess', preprocess),
    ('select', fs),
    ('clf', estimator)
]
pipeline = Pipeline(steps)
```

现在，它就像任何其他机器学习算法一样简单，我们首先拟合，然后使用预测。预测函数执行所有其他预处理，然后应用定型模型。

```
pipeline.fit(X_train, y_train)
y_pred = pipeline.predict(X_test)
pred_df = pd.DataFrame({'y': y_test,'y_pred': y_pred})
gini = 2*roc_auc_score(y_test, y_pred)-1
```

对于我的用例，我需要评估基尼指数，为此我使用了`43.33`。

现在，让我们进行随机搜索交叉验证，以找到最佳 AUC (Gini ),为此，我将一些搜索参数传递给 XGBoostRegressor。

```
param_grid = {
    'clf__learning_rate': np.arange(0.05, 1, 0.05),
    'clf__max_depth': np.arange(3,10,1),
    'clf__n_estimators': np.arange(50,250,50)
}rand_auc = RandomizedSearchCV(estimator=pipeline, param_distributions=param_grid, n_iter=5, scoring='roc_auc', cv=5, verbose=False)
rand_auc.fit(X_train, y_train)
rand_auc.best_score_y_pred = rand_auc.predict(X_test)
pred_df = pd.DataFrame({'y': y_test,'y_pred': y_pred})
gini = 2*roc_auc_score(y_test, y_pred)-1
```

现在我有了 Gini 或`46.48`，比之前的方法稍微好一点——可能需要对模型进行更多的微调。但这不是本教程的重点。

现在，我们可以在 for 循环中的各种分类器上测试相同的管道，比较分数并选择最佳模型。

```
classifiers = [
    KNeighborsClassifier(3),
    SVC(kernel="rbf", C=0.025, probability=True),
    NuSVC(probability=True),
    DecisionTreeClassifier(),
    RandomForestClassifier(),
    AdaBoostClassifier(),
    GradientBoostingClassifier()
    ]for classifier in classifiers:
    steps = [
        ('preprocess', preprocess),
        ('select', fs),
        ('clf', classifier)
    ]
    pipeline = Pipeline(steps)
    pipeline.fit(X_train, y_train)   
    print(classifier)
    print("model score: %.3f" % pipeline.score(X_test, y_test))
```

如您所见，使用 scikit-learn 的管道功能有助于简化机器学习工作流程，并使数据科学家的工作更加轻松，可以将时间集中在微调模型上，而不是重复进行数据预处理步骤。