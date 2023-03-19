# 用最大似然法预测周期性事件

> 原文：<https://towardsdatascience.com/forecasting-of-periodic-events-with-ml-5081db493c46?source=collection_archive---------10----------------------->

## 根据先前事件的日期预测周期性发生的事件。

![](img/b457820a09c3653ca384a13f0fab2d5d.png)

由[罗曼·博日科](https://unsplash.com/@romanbozhko?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

例如，如果您是数据聚合者，定期事件预测就非常有用。数据聚合者或数据提供者是从不同来源收集统计、财务或任何其他数据，对其进行转换，然后提供给进一步分析和探索的组织(数据即服务)。

> 数据即服务(DaaS)

对于这样的组织来说，监控发布日期非常重要，这样可以在发布后立即收集数据，并规划容量来处理即将到来的大量数据。

有时发布数据的权威机构有未来发布的时间表，有时没有。在某些情况下，他们只宣布未来一两个月的时间表，因此，您可能希望自己制定出版时间表并预测发行日期。

对于大多数统计数据发布，您可能会发现一种模式，如一周或一个月中的某一天。例如，可以发布统计数据

*   每月的最后一个工作日，
*   每个月的第三个星期二，
*   每月最后第二个工作日，等等。

考虑到这一点和以前的发布日期历史，我们希望预测下一次数据发布可能发生的潜在日期或日期范围。

# 个案研究

作为案例研究，让我们以[美国会议委员会(CB)消费者信心指数](https://www.investing.com/economic-calendar/cb-consumer-confidence-48)为例。这是衡量经济活动中消费者信心水平的领先指标。通过使用它，我们可以预测在整体经济活动中起主要作用的消费者支出。

官方数据提供商[没有提供该系列的时间表，但许多数据聚合者，如](https://conference-board.org/eu/)[Investing.com](https://www.investing.com/economic-calendar/cb-consumer-confidence-48)已经收集了一段时间的数据，该系列的发布历史也在那里。

**目标**:我们需要预测下一次发布的日期。

# 数据准备

我们从导入所有用于数据操作、构建机器学习模型和其他数据转换的包开始。

```
# Data manipulation
import pandas as pd# Manipulation with dates
from datetime import date
from dateutil.relativedelta import relativedelta# Machine learning
import xgboost as xgb
from sklearn import metrics
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.neighbors import KNeighborsClassifier
from sklearn.ensemble import RandomForestClassifier
```

下一步是获取发布历史日期的列表。您可能有一个数据库，其中包含您可以使用的发布日期的所有数据和历史记录。为了使这变得简单并专注于发布日期预测，我将手动向 DataFrame 添加历史记录。

```
data = pd.DataFrame({'Date': ['2021-01-26','2020-12-22',
                     '2020-11-24','2020-10-27','2020-09-29',
                     '2020-08-25','2020-07-28','2020-06-30',
                     '2020-05-26','2020-04-28','2020-03-31',
                     '2020-02-25','2020-01-28','2019-12-31',
                     '2019-11-26','2019-10-29','2019-09-24',
                     '2019-08-27','2019-07-30','2019-06-25',
                     '2019-05-28']})
```

我们还应该添加一个包含 0 和 1 值的列来指定发布是否发生在这个日期。现在，我们只有发布日期，所以我们创建一个用 1 值填充的列。

```
data['Date'] = pd.to_datetime(data['Date'])
data['Release'] = 1
```

之后，您需要在 DataFrame 中为发布之间的日期创建所有行，并用零填充发布列。

```
r = pd.date_range(start=data['Date'].min(), end=data['Date'].max())
data = data.set_index('Date').reindex(r).fillna(0.0)
       .rename_axis('Date').reset_index()
```

现在，数据集已准备好进行进一步的操作。

# 特征工程

对下一个发布日期的预测很大程度上依赖于特性工程，因为实际上，除了发布日期本身，我们没有任何特性。因此，我们将创建以下功能:

*   月
*   一个月中的某一天
*   工作日编号
*   星期几
*   月份中的周数
*   每月工作日(每月的第二个星期三)

```
data['Month'] = data['Date'].dt.month
data['Day'] = data['Date'].dt.day
data['Workday_N'] = np.busday_count(
                    data['Date'].values.astype('datetime64[M]'),
                    data['Date'].values.astype('datetime64[D]'))
data['Week_day'] = data['Date'].dt.weekday
data['Week_of_month'] = (data['Date'].dt.day 
                         - data['Date'].dt.weekday - 2) // 7 + 2
data['Weekday_order'] = (data['Date'].dt.day + 6) // 7
data = data.set_index('Date')
```

# 训练机器学习模型

默认情况下，我们需要将数据集分成两部分:训练和测试。不要忘记将 shuffle 参数设置为 False，因为我们的目标是基于过去的事件创建预测。

```
x_train, x_test, y_train, y_test = train_test_split(data.drop(['Release'], axis=1), data['Release'],
                 test_size=0.3, random_state=1, shuffle=False)
```

一般来说，shuffle 通过选择不同的训练观察值来帮助摆脱过度拟合。但这不是我们的情况，每次我们都应该出版所有历史事件。

为了选择最佳预测模型，我们将测试以下模型:

*   XGBoost
*   k-最近邻(KNN)
*   随机森林

## XGBoost

我们将使用 XGBoost 与树基学习器和网格搜索方法来选择最佳参数。它搜索所有可能的参数组合，并基于交叉验证评估选择最佳组合。

这种方法的缺点是计算时间长。

或者，可以使用随机搜索。它在给定次数的给定范围内迭代，随机选择值。经过一定次数的迭代后，它会选择最佳模型。

但是，当您有大量参数时，随机搜索会测试相对较少的组合。这使得找到一个真正的 T2 最优组合变得几乎不可能。

要使用网格搜索，您需要为每个参数指定可能值的列表。

```
DM_train = xgb.DMatrix(data=x_train, label=y_train)
grid_param = {"learning_rate": [0.01, 0.1],
              "n_estimators": [100, 150, 200],
              "alpha": [0.1, 0.5, 1],
              "max_depth": [2, 3, 4]}
model = xgb.XGBRegressor()
grid_mse = GridSearchCV(estimator=model, param_grid=grid_param,
                       scoring="neg_mean_squared_error",
                       cv=4, verbose=1)
grid_mse.fit(x_train, y_train)
print("Best parameters found: ", grid_mse.best_params_)
print("Lowest RMSE found: ", np.sqrt(np.abs(grid_mse.best_score_)))
```

如您所见，我们的 XGBoost 模型的最佳参数是:`alpha = 0.5, n_estimators = 200, max_depth = 4, learning_rate = 0.1`。

让我们用获得的参数来训练模型。

```
xgb_model = xgb.XGBClassifier(objective ='reg:squarederror', 
                            colsample_bytree = 1, 
                            learning_rate = 0.1,
                            max_depth = 4, 
                            alpha = 0.5, 
                            n_estimators = 200)
xgb_model.fit(x_train, y_train)
xgb_prediction = xgb_model.predict(x_test)
```

## k-最近邻(KNN)

k-最近邻模型是在试图寻找观察值之间的相似性时使用的。这正是我们的情况，因为我们试图在过去的发布日期中找到模式。

KNN 算法需要调整的参数更少，因此对于以前没有使用过它的人来说更简单。

```
knn = KNeighborsClassifier(n_neighbors = 3, algorithm = 'auto',     
                           weights = 'distance') 
knn.fit(x_train, y_train)  
knn_prediction = knn.predict(x_test)
```

## 随机森林

随机森林基本模型参数调整通常不会花费很多时间。您只需迭代估计器的可能数量和树的最大深度，并使用肘方法选择最佳值。

```
random_forest = RandomForestClassifier(n_estimators=50,
                                       max_depth=10, random_state=1)
random_forest.fit(x_train, y_train)
rf_prediction = random_forest.predict(x_test)
```

# 比较结果

我们将使用混淆矩阵来评估训练模型的性能。它帮助我们并排比较模型，并了解我们的参数是否应该进一步调整。

```
xgb_matrix = metrics.confusion_matrix(xgb_prediction, y_test)
print(f"""
Confusion matrix for XGBoost model:
TN:{xgb_matrix[0][0]}    FN:{xgb_matrix[0][1]}
FP:{xgb_matrix[1][0]}    TP:{xgb_matrix[1][1]}""")knn_matrix = metrics.confusion_matrix(knn_prediction, y_test)
print(f"""
Confusion matrix for KNN model:
TN:{knn_matrix[0][0]}    FN:{knn_matrix[0][1]}
FP:{knn_matrix[1][0]}    TP:{knn_matrix[1][1]}""")rf_matrix = metrics.confusion_matrix(rf_prediction, y_test)
print(f"""
Confusion matrix for Random Forest model:
TN:{rf_matrix[0][0]}    FN:{rf_matrix[0][1]}
FP:{rf_matrix[1][0]}    TP:{rf_matrix[1][1]}""")
```

如你所见，XGBoost 和 RandomForest 都表现出了不错的性能。在大多数情况下，他们都能捕捉到模式并正确预测日期。然而，这两个模型都在 2020 年 12 月发布时犯了一个错误，因为它打破了发布模式。

KNN 不如前两者准确。它未能正确预测三个日期，错过了 5 次发布。在这一点上，我们不继续讨论 KNN。一般来说，如果数据是规范化的，效果会更好，所以如果您愿意，可以尝试对其进行调优。

关于其余两个，对于初始目标，XGBoost 模型被认为在超参数调整方面过于复杂，因此 RandomForest 应该是我们的选择。

现在，我们需要创建预测未来日期的数据框架，并使用训练好的 RandomForest 模型来预测未来一年的未来释放量。

```
x_predict = pd.DataFrame(pd.date_range(date.today(), (date.today() +
            relativedelta(years=1)),freq='d'), columns=['Date'])
x_predict['Day'] = x_predict['Date'].dt.day
x_predict['Workday_N'] = np.busday_count(
                x_predict['Date'].values.astype('datetime64[M]'),
                x_predict['Date'].values.astype('datetime64[D]'))
x_predict['Week_day'] = x_predict['Date'].dt.weekday
x_predict['Week_of_month'] = (x_predict['Date'].dt.day - 
                              x_predict['Date'].dt.weekday - 2)//7+2
x_predict['Weekday_order'] = (x_predict['Date'].dt.day + 6) // 7
x_predict['Month'] = x_predict['Date'].dt.month
x_predict = x_predict.set_index('Date')prediction = xgb_model.predict(x_predict)
```

就是这样——我们预测了未来一年美国 CB 消费者信心系列的发布日期。

# 结论

如果您想要预测周期性事件的未来日期，您应该考虑创建有意义的要素。它们应该包括你能在历史中找到的关于模式的所有信息。正如你所看到的，我们没有在模型的调整上花太多时间——如果你使用正确的特性，即使简单的模型也能给出好的结果。

谢谢你一直读到最后。我真的希望它是有帮助的，如果你在评论中发现任何错误，请让我知道。