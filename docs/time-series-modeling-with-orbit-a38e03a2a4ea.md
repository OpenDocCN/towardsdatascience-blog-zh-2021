# 用轨道进行时间序列预测

> 原文：<https://towardsdatascience.com/time-series-modeling-with-orbit-a38e03a2a4ea?source=collection_archive---------4----------------------->

## 部署优步销售预测新模型的用户指南

在一个数据消耗不断增长的世界中，时间序列分析已经成为数据科学家越来越普遍和必要的技术。这份利用优步新时间序列模型 ORBIT 的分步用户指南是 [5 销售预测机器学习技术](/5-machine-learning-techniques-for-sales-forecasting-598e4984b109)的延续。这两篇文章一起阐述了一些常见的预测方法。

# 了解轨道

ORBIT(面向对象的贝叶斯时间序列)是一个时间序列包，旨在易于实现和推广。尽管构建于概率编程语言之上，Python 包允许以类似于 scikit-learn 模型的方式部署该模型。因此，该界面可以轻松实现贝叶斯指数平滑和自回归时间序列建模。

轨道是指数平滑模型的一种发展，在该模型中，预测是使用过去观测值的加权平均值生成的，并且随着当前日期的时间推移，观测值的权重呈指数下降。指数平滑模型有一个水平(平滑)组件和一个趋势组件。Orbit 目前包含两个模型，局部全球趋势(LGT)和阻尼局部趋势(DLT)。LGT 是完全可加 ETS，ETS(A，A，A)和自回归模型的组合。LGT 使用对数变换来解释乘法趋势，这意味着所有输入值(y_t)必须大于 0。DLT 提供了一个确定性的全球趋势分类为线性，对数线性，或逻辑。在 DLT 模型中，还可以灵活地加入外生变量。

# 应用模型

## 获取数据

出于演示的目的，我们将把轨道模型应用于这个 [Kaggle 数据集](https://www.kaggle.com/c/demand-forecasting-kernels-only)。要提取数据，可以利用 Kaggle API，如下所示。

导入/安装 Kaggle 包。下面的代码首先检查是否安装了这个包，如果没有，就安装它。[完整代码在此](https://github.com/mollyliebeskind/Forecasting_Orbit)。

```
def checkForPackages(package):
    reqs = subprocess.check_output([sys.executable, '-m', 'pip', 'freeze'])
    installed_packages = [r.decode().split('==')[0].lower() for r in reqs.split()]if not package in installed_packages:
    print("Installing", package, "...")
    subprocess.check_call([sys.executable, "-m", "pip", "install", package])try:
    import kaggle
except:
    checkForPackages('kaggle')from kaggle.api.kaggle_api_extended import KaggleApi
```

2.连接到 Kaggle API 并下载数据集。为了验证您的访问，您需要[创建一个 API 密钥并验证连接](https://stackoverflow.com/questions/55934733/documentation-for-kaggle-api-within-python)。

```
dirPath = str(pathlib.Path().resolve()) #Get current directory path
dataPath =  dirPath + "/data/"
api = KaggleApi() 
api.authenticate() #Follow instructions at link above# Download the dataset for this example
api.competition_download_file('demand-forecasting-kernels-only','train.csv', path=dataPath)
```

## 应用模型

为了证明 Orbit 对这些数据的有效性，我们将把 LGT 和 DLT 模型的一些迭代与一个简单的 ETS 模型和一个自动 ARIMA 模型进行比较。对于 ARIMA 车展，我们将利用 pmdarima 包。同样，我们将从检查和安装我们没有的建模包开始。

```
modelpackages = {'orbit': 'orbit-ml', 'pmdarima': 'pmdarima'}for package in modelpackages:
    try:
        import package
    except:
        checkForPackages(modelpackages[package])from orbit.models.dlt import ETSFull, DLTMAP, DLTFull
from orbit.models.lgt import LGTMAP, LGTFull, LGTAggregated
from orbit.diagnostics.plot import plot_predicted_components
from pmdarima.arima import auto_arima
```

接下来的步骤是预处理以生成可用的数据集。首先，我们将每日数据转换为每月数据(在[这篇文章](/5-machine-learning-techniques-for-sales-forecasting-598e4984b109)中有更详细的描述)。然后我们分成训练集和测试集。完整的前处理代码可以在[这里找到](https://github.com/mollyliebeskind/Forecasting_Orbit)。

这里的主要工作是实例化和运行 LGT 和 DLT 模型。轨道模型是由 DLT/LGT/ETS 指标结合一种估计指标方法构建的。可用的估计量有 MAP、FULL 和 Aggregation。

![](img/26cf36233915df507165414950185480.png)

眼眶后部比较。莫莉·里伯斯金图片。

首先，我们将创建一个包含我们想要测试的所有模型的字典。我们将它放在一个函数中，这样我们也可以在其他脚本中利用这些实例化的模型(例如绘图)。下面的列表并不包括所有的模型，但是给出了如何构建模型的例子。

```
def getModelDict(train):
    modelDict = {'ETSFull': ETSFull(
                    response_col=response_col,
                    date_col=date_col,
                    seasonality=12,
                    seed=8888
                    ),
                'DLTMAP_Linear': DLTMAP(
                        response_col=response_col,
                        date_col=date_col,
                        seasonality=12,
                        seed=8888
                        ),
                'DLTMAP_LogLin': DLTMAP(
                                response_col=response_col,
                                date_col=date_col,
                                seasonality=12,
                                seed=8888,
                                global_trend_option='loglinear'
                                ),
                'DLTMAP_Logistic': DLTMAP(
                                    response_col=response_col,
                                    date_col=date_col,
                                    seasonality=12,
                                    seed=8888,
                                    global_trend_option='logistic'
                                    ),
                'LGTMAP': LGTMAP(
                            response_col=response_col,
                            date_col=date_col,
                            seasonality=12,
                            seed=8888
                            ), # Commented out because the results were too poor
                'LGTFull': LGTFull(
                            response_col=response_col,
                            date_col=date_col,
                            seasonality=12,
                            seed=8888,
                        ),
                'LGTAggregation': LGTAggregated(
                                    response_col=response_col,
                                    date_col=date_col,
                                    seasonality=12,
                                    seed=8888,
                                ),
                'DLTFull': DLTFull(
                            response_col=response_col,
                            date_col=date_col,
                            seasonality=12,
                            seed=8888,
                            num_warmup=5000,
                        ),
            'ARIMA': auto_arima(train[[response_col]],
                                    m=12,
                                    seasonal=True
                                    ) }
    return modelDict
```

接下来，我们将创建一个运行任何轨道模型和标准 scikit-learn 结构化模型的可调用函数。下面的代码使我们能够用上面模型字典中的任何模型调用函数 runTheModel。对于最初的运行，我们测试了所有的模型而没有绘图。再往下，还有一个绘制模型分解的例子。

```
def runTheModel(df_train, df_test, model, modName, date_col, response_col, decompose=False, imgPath=imgPath):

    #Structure model fit & predict for Orbit
    if not 'ARIMA' in modName: 
        model.fit(df_train)
        if not decompose:
            pred = model.predict(df_test[[date_col]]).prediction.unique()
        # Add plotting code
        else:
            pred = model.predict(df_train, decompose=True)
            vis = plot_predicted_components(pred, date_col, is_visible=False,
                                            plot_components=['prediction', 'trend', 'seasonality'],
                                            path=imgPath)
            return vis, pred

    #Structure model fit & predict for ARIMA
    else:
        pred = model.predict(df_test.shape[0])error = mape(df_test[response_col], pred)
    prediction_df = pd.DataFrame.from_dict({'Date': df_test[date_col], 'Actual': df_test[response_col], 'Prediction': pred, 'MAPE': error, 'Model': modName})

    # print(prediction_df.head())
    return prediction_df
```

然后，我们实例化所有模型的字典，并为每个模型调用 runTheModel 函数。

```
models = getModelDict(train, test)# Run the models
predictions = []for mod in models:
        print('running', mod)
        predictions.append(runTheModel(train, test, models[mod], mod, date_col, response_col))
```

## 查看结果

下图([这里的绘图代码](https://github.com/mollyliebeskind/Forecasting_Orbit))包括一段时间内的实际销售数据，每个模型的输出都覆盖了测试集。很明显，某些模型配置在测试集上表现得更好/更差。这里需要注意的一点是，尽管 auto-arima 会包含一个 difference(d)组件(如果它是最佳的),但不会对数据集执行任何差分操作。

![](img/bb518ec0f6e9f6cc3d04af20ec3e598d.png)

时间序列模型输出比较。莫莉·里伯斯金图片。

为了更好地量化每个模型的性能差异，下图显示了测试集中预测的平均绝对百分比误差(MAPE)。具有最低 MAPE 的模型可以被认为是“最好的”。我们在下面看到，默认的 DLTMAP (DLTMAP_Linear)和具有对数线性全球趋势的 DLTMAP 表现最好，略好于自动 ARIMA 模型。我们还看到，加入这些阻尼的局部趋势会比标准 ETS 模型驱动更好的性能。

![](img/4de6ce730fd26ed12529fbf35c0458bd.png)

模拟 MAPE 比较。莫莉·里伯斯金图片。

仅通过相同的模型代码运行我们的最佳模型，但启用绘图，我们能够看到趋势和季节性的分解([代码在这里](https://github.com/mollyliebeskind/Forecasting_Orbit/blob/main/4_decomposition.py))。我们也可以在同一图中显示回归量，但在这种情况下，我们没有包括回归量。

![](img/40107dc567b52345dda8becbca81405f.png)

轨道分解。莫莉·里伯斯金图片。

上面我们测试了几个标准轨道模型与自动 ARIMA 和标准 ETS 模型的比较。但是，对于 Orbit 包内的超参数调整，有一个模块[Orbit . diagnostics . back test](https://uber.github.io/orbit/tutorials/backtest.html?highlight=orbit%20diagnostics%20backtest)支持时间相关的列车测试分割，并具有输出 smape、wmape、mape、mse、mae、rmsse 分数的方法。回溯测试模块还包含网格搜索功能，使您能够测试不同的参数，比较结果，并打印出性能最佳的超参数。

时间序列分析是任何数据科学家的基本工具。Orbit 提供了一种在面向对象的包中利用贝叶斯方法的方法。通过 DLT 提供的回归量的增加有助于提高模型性能，是对 ARIMAX 实现的一个很好的补充。在轨道上仍有许多问题需要解决，开发工作正在进行中。

要继续学习 ARIMA、XGBoost、LSTM、随机森林和线性回归的时间序列和预测，请查看 [5 销售预测机器学习技术](/5-machine-learning-techniques-for-sales-forecasting-598e4984b109)。

快乐学习！