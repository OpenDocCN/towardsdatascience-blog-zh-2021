# 使用 Optuna 调整超参数

> 原文：<https://towardsdatascience.com/tuning-hyperparameters-with-optuna-af342facc549?source=collection_archive---------12----------------------->

## 以自动化和智能的方式实现最佳 ML 模型

![](img/99222e2bc691d8ddabcc07940a9bda2e.png)

Alexis Baydoun 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 目录

1.  **简介**
2.  **超参数和 sci kit-学习调整方法**
3.  **用 Optuna 调节超参数**
4.  **结论**

## 介绍

我最近完成了我在 Kaggle 上的第一个机器学习项目，用波士顿住房数据集预测销售价格(合著者: [Julia Yang](https://medium.com/u/ec687522df18?source=post_page-----af342facc549--------------------------------) )。请看我们的作品，它在 5000 名竞争者中以[【最高 9%】的房价](https://www.kaggle.com/garylucn/top-9-house-price)取得了最高 9%的成绩。

从这个项目中，朱莉娅和我都学到了很多东西，我们将写一系列关于我们的学习的文章。在这篇文章中，我将讨论我使用 Optuna 调优 ML 模型超参数的经验。

## 超参数和 sci kit-学习调整方法

在机器学习项目中，调整超参数是最关键的步骤之一。由于 ML 模型无法从数据中学习超参数，因此我们有责任对它们进行调优。如果我们不仔细选择超参数，我们在项目的其他部分花费的所有努力都可能是徒劳的。

在 scikit-learn 中，我们可以使用一些方法来调整超参数:

1.  穷举网格搜索([*GridSearchCV*](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)*和[*halvinggridsearccv*](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.HalvingGridSearchCV.html))
    本质上，我们需要建立一个超参数可能组合的网格。scikit-learn 将尝试每种组合来拟合模型，并使用指定的评分方法和指定的交叉验证拆分器来计算模型的性能。*
2.  *random search([*randomzedsearchcv*](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)*和[*HalvingRandomSearchCV*](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.HalvingRandomSearchCV.html))
    这些方法还需要我们建立超参数网格。但是在随机搜索中，并不是所有的参数值都被尝试，而是从网格中抽取固定数量( *n_iter* )的参数设置。**
3.  **scikit-learn 还提供了一些特定于模型的交叉验证方法，我们可以使用这些方法来调整超参数，例如， [*RidgeCV*](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.RidgeCV.html) 、 [*LassoCV*](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LassoCV.htm) 和[*elastic netcv*](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.ElasticNetCV.html)*等。这些方法是可行的，因为这些模型可以像拟合超参数的单个值的估计量一样有效地拟合一些超参数值范围的数据。***

***然而，根据我的经验，这些方法似乎存在一些问题。***

***对于某些情况，在我们预测房价的工作中，我们使用了六种不同的模型来拟合和预测数据，然后将这些模型集成在一起成为最终模型。因此，我需要调整所有这六个模型的超参数。由于一些模型有相当多的超参数，scikit-learn 的网格搜索方法将很快变得非常昂贵。例如，当我调整 Ridge 时，我只需要调整到 alpha 并尝试它的 10 个不同值，网格搜索将匹配模型 10 次。尽管如此，当我调优 XGBRegressor 时，如果我想调优 6 个超参数并尝试其中每个参数的 10 个不同值，网格搜索将不得不适应模型 1，000，000 次。***

***另一方面，与网格搜索相反，随机搜索可以限制拟合模型的预算，但是对于找到超参数的最佳组合来说，它似乎太随机了。***

***为了用 scikit-learn 的方法克服这些问题，我在网上搜索工具，我找到了几个用于超参数调优的包，包括 [Optuna](https://optuna.org/) 和 [Ray.tune](https://docs.ray.io/en/latest/tune/index.html) 。在这篇文章中，我将分享我与 Optuna 的经验。***

## ***使用 Optuna 调整超参数***

***Optuna 是“一个自动化超参数搜索的开源超参数优化框架。”Optuna 的主要特性包括“自动搜索最佳超参数”、“有效搜索大空间并删除无用的尝试以获得更快的结果”，以及“在多个线程或进程上并行化超参数搜索”(引自 https://optuna.org/***

***我对 Optuna 的超参数搜索算法不是太了解，所以我想重点介绍一下如何使用 Optuna。***

***第一步是为 Optuna 定义最大化的目标函数。目标函数应该将一个“*试验*对象作为输入，并返回分数、浮点值或浮点值列表。下面是我为山脊模型定义的目标函数的一个例子:***

```
**def ridge_objective(trial): # Use the trial object to suggest a value for alpha
    # Optuna will come up with a suggested value based on the scores of previous trials
   _alpha = trial.suggest_float("alpha", 0.1, 20) # Define the model with the suggested alpha
    ridge = Ridge(alpha=_alpha, random_state=42) # Calculate the score with 10-folds cross validation, which returns a list of scores
    # scoring is defined as negative RMSE as it is what this Kaggle competition uses to evaluate the result
    scores = cross_val_score(ridge, X, y, 
                             cv=KFold(n_splits=10,
                                      shuffle=True,
                                      random_state=42),
                             scoring="neg_root_mean_squared_error"
                            ) # Return the mean of 10 scores
    return scores.mean()**
```

**下一步是使用目标函数创建一个“研究”对象，然后对其进行优化。**

```
**# Create Study object
study = optuna.create_study(direction="maximize")# Optimize the study, use more trials to obtain better result, use less trials to be more cost-efficient
study.optimize(objective, n_trials=100) # Use more # Print the result
best_params = study.best_params
best_score = study.best_value
print(f"Best score: {best_score}\n")
print(f"Optimized parameters: {best_params}\n")**
```

**仅此而已。看起来很简单吧？让我们把它们和所有的模型放在一起。**

```
**RANDOM_SEED = 42

*# 10-fold CV*
kfolds = KFold(n_splits=10, shuffle=True, random_state=RANDOM_SEED)# Define the helper function so that it can be reused
def tune(objective):
    study = optuna.create_study(direction="maximize")
    study.optimize(objective, n_trials=100)

    params = study.best_params
    best_score = study.best_value
    print(f"Best score: {best_score}\n")
    print(f"Optimized parameters: {params}\n")
    return params##################
# Ridge
##################def ridge_objective(trial):
    _alpha = trial.suggest_float("alpha", 0.1, 20)
    ridge = Ridge(alpha=_alpha, random_state=RANDOM_SEED)
    scores = cross_val_score(
        ridge, X, y, cv=kfolds,
        scoring="neg_root_mean_squared_error"
    )
    return scores.mean()

ridge_params = tune(ridge_objective)# After tuning it for once, we can copy the best params to create the model without tunning it again
# ridge_params = {'alpha': 7.491061624529043}ridge = Ridge(**ridge_params, random_state=RANDOM_SEED)##################
# Lasso
##################def lasso_objective(trial):
    _alpha = trial.suggest_float("alpha", 0.0001, 1)
    lasso = Lasso(alpha=_alpha, random_state=RANDOM_SEED)
    scores = cross_val_score(
        lasso, X, y, cv=kfolds,
        scoring="neg_root_mean_squared_error"
    )
    return scores.mean()lasso_params = tune(lasso_objective)
# lasso_params = {'alpha': 0.00041398687418613947}lasso = Lasso(**lasso_params, random_state=RANDOM_SEED)##################
# Random Forest
##################def randomforest_objective(trial):
    _n_estimators = trial.suggest_int("n_estimators", 50, 200)
    _max_depth = trial.suggest_int("max_depth", 5, 20)
    _min_samp_split = trial.suggest_int("min_samples_split", 2, 10)
    _min_samples_leaf = trial.suggest_int("min_samples_leaf", 2, 10)
    _max_features = trial.suggest_int("max_features", 10, 50)

    rf = RandomForestRegressor(
        max_depth=_max_depth,
        min_samples_split=_min_samp_split,
        min_samples_leaf=_min_samples_leaf,
        max_features=_max_features,
        n_estimators=_n_estimators,
        n_jobs=-1,
        random_state=RANDOM_SEED,
    )

    scores = cross_val_score(
        rf, X, y, cv=kfolds, scoring="neg_root_mean_squared_error"
    )
    return scores.mean()

randomforest_params = tune(randomforest_objective)
# randomforest_params = {'n_estimators': 180, 'max_depth': 18, 'min_samples_split': 2, 'min_samples_leaf': 2, 'max_features': 49}rf = RandomForestRegressor(n_jobs=-1, random_state=RANDOM_SEED, **randomforest_params)# So on with other models...**
```

**请在[https://www.kaggle.com/garylucn/top-9-house-price/notebook](https://www.kaggle.com/garylucn/top-9-house-price/notebook)或[https://github . com/Glu cn/ka ggle/blob/main/House _ Prices/notebook/top-9-House-price . ipynb](https://github.com/glucn/kaggle/blob/main/House_Prices/notebook/top-9-house-price.ipynb)找到完整代码**

## **结论**

**我对 Optuna 非常满意，因为调优过程比 scikit-learn 的网格搜索快得多，结果也比随机搜索好。以下是我将在下一步继续挖掘的一些项目:**

1.  **尝试使用其他工具，如 Ray.tune。**
2.  **在目标函数中，我需要为每个超参数指定一个范围。如果我对模型有更好的理解，我可以使范围更准确，以提高分数或节省一些预算。所以对我来说，一项重要的工作就是深入研究这些模型。**

**Julia 和我关于数据科学主题的其他文章:**

*   **[比较&对比:探索具有许多特征的数据集](https://levelup.gitconnected.com/compare-contrast-eda-of-datasets-with-many-features-f9665da15132)**

## **参考**

*   **[scikit-learn:调整估计器的超参数](https://scikit-learn.org/stable/modules/grid_search.html)**
*   **[Python 中超参数调优:2021 年完整指南](https://neptune.ai/blog/hyperparameter-tuning-in-python-a-complete-guide-2020)**