# 使用 PyCaret 创建新的时间序列

> 原文：<https://towardsdatascience.com/new-time-series-with-pycaret-4e8ce347556a?source=collection_archive---------11----------------------->

## 测试版教程-轻松进行预测

![](img/8e933ec624609b933617a3b338138423.png)

由 [Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[un splash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄的照片。

# 目录

1.  介绍
2.  设置、比较、调整和混合模型
3.  生产模型
4.  摘要
5.  参考

# 介绍

坦率地说，即使看起来很简单，时间序列算法通常也很难处理。[py caret](https://github.com/pycaret/pycaret/?platform=hootsuite&utm_campaign=HSCampaign#pycaret-new-time-series-module)【2】通过自动化建立和比较时间序列的模型过程，减轻了许多单调和混乱的工作。您可以更进一步，使用这个 Python 库生产您选择的模型。然而，通过调整不同的参数，您仍然可以在库中拥有自己的输入和自主权。话虽如此，让我们更深入地研究一个用例，以说明使用下面这个独特的工具和库进行预测是多么简单。

# 设置、比较、调整和混合模型

![](img/e4b60f8034d264295d3a59bb0e959ad5.png)

格伦·卡斯滕斯-彼得斯在[Unsplash](https://unsplash.com/s/photos/comparison?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄的照片。

PyCaret 似乎有无数的模型可供选择，也可以进行比较。一些，老实说我甚至没有听说过，以及一些更流行和可靠的像 ARIMA ( *自回归综合移动平均线*)模型。

> 以下是一些您可以尝试的时间序列模型名称，重申一下，还有更多:

*   季节性天真预测者
*   指数平滑法
*   ARIMA
*   ARIMA 汽车
*   多项式趋势预测器
*   k 邻居 w/ Cond。去季节化和去趋势化
*   线性 w/ Cond。去季节化和去趋势化
*   弹性网 w/ Cond。去季节化和去趋势化
*   乘坐 w/ Cond。去季节化和去趋势化
*   带导管的套索网。去季节化和去趋势化
*   极端梯度推进 w/ Cond。去季节化和去趋势化

如您所见，有许多模型可供您使用和比较，因此您不必做任何繁琐的工作，例如导入一长串不同的库。你可能还会发现一些你还没有尝试过的新方法。

> 下面是使用 PyCaret 进行简单时间序列预测的简单教程:

```
# Code is edited and reorganized from example source:
[https://nbviewer.org/github/pycaret/pycaret/blob/time_series_beta/time_series_101.ipynb](https://nbviewer.org/github/pycaret/pycaret/blob/time_series_beta/time_series_101.ipynb) [4]ex = TimeSeriesExperiment()
ex.setup(data=y, fh=fh, fold=fold, session_id=100)y_train = ex.get_config("y_train")
y_test = ex.get_config("y_test")best_baseline_models = ex.compare_models(fold=fold, sort='MAE', n_select=10)compare_metrics = ex.pull()best_tuned_models = [ex.tune_model(model) for model in best_baseline_models]mean_blender = ex.blend_models(best_tuned_models, method='mean')y_predict = ex.predict_model(mean_blender)
ex.plot_model(estimator=mean_blender)
```

在上面的代码中，我们可以看到，在短短几行代码中，您可以比较几乎所有的时间序列模型，对它们进行调整，并将其混合以获得最佳结果。如果你想节省一些时间，你甚至可以在不调整或混合的情况下相当精确。另一个很棒的特性是从各种模型中提取指标，并并排查看它们。

总的来说，这一小块代码通常会占用您更多的笔记本空间，因此能够专注于 PyCaret 漂亮的可视化效果，而不是您不再需要处理的较小或不太重要的代码部分，这是一个好处。

# 生产模型

![](img/2095f389261a420f8a48e1becf9449e8.png)

照片由[巴格斯·赫纳万](https://unsplash.com/@bhaguz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/phone-app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】上拍摄。

现在，您已经测试了几个模型并比较了它们的指标，如本例中的 MAE(*或任何其他指标，如 RMSE、MAPE、SMAPE 和 R2，以及运行模型所需的时间*)，您可以开始保存并加载您的模型以供将来使用。

首先，无论出于何种原因，您都会保存您选择的最佳模型。然后，您将在新的会话中加载模型，并预测新的数据。

## 保存您的模型

保存最终模型很容易。只需使用`save_model`函数，并对其进行命名，这样就可以为未来的预测/预报加载相同的名称或版本。

```
ex.save_model(final_model, "my_final_model")
```

## 加载您的模型

加载模型也很容易，您可以应用 predict 方法来查看您的预测与索引或第一列的关系。

```
ex_load = TimeSeriesExperiment()
loaded_model = ex_load.load_model("my_final_model")
```

使用更少的代码，您可以通过保存和加载新的、看不见的数据来生产您的模型。还有更多方法可以让你利用**预测区间**来定制 alphas。其他功能包括:

*   窗口拆分
*   扩展窗口
*   滚动窗
*   错误处理

最好从数据集入手，将这些代码应用到您的特定用例中，这样您就可以开始使用时间序列算法进行预测。

# 摘要

PyCaret 一直在更新，所以请确保每天或每周关注新功能。时间序列可能是不那么吸引人的数据科学问题，但它可能是公司利益相关者要求最多的问题。这真的取决于你，如果你正在与时间序列作斗争，那么这篇文章向你展示了如何，在短短几行代码，你可以比较，分析，并使用预测模型。

> 总结一下，我们了解到的情况如下:

```
* Setting Up, Comparing, Tuning, and Blending Models* Productionalize Model
```

我希望你觉得我的文章既有趣又有用。如果您同意或不同意这种学习和实施时间序列预测的方法，请随时在下面发表评论。为什么或为什么不？你认为你还可以使用哪些库或工具？这个库当然可以被进一步阐明，但是我希望我能够对困难的时间序列问题有所启发。感谢您的阅读！

***我不属于这些公司中的任何一家。***

*请随时查看我的个人资料、* [Matt Przybyla](https://medium.com/u/abe5272eafd9?source=post_page-----4e8ce347556a--------------------------------) 、*和其他文章，并通过以下链接订阅接收我的博客的电子邮件通知，或通过点击屏幕顶部的订阅图标* *的* ***，如果您有任何问题或意见，请在 LinkedIn 上联系我。***

**订阅链接:**[https://datascience2.medium.com/subscribe](https://datascience2.medium.com/subscribe)

# 参考

[1]图片由 [Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2018)

[2] [Moez Ali](https://medium.com/u/fba05660b60f?source=post_page-----4e8ce347556a--------------------------------) ， [PyCaret 时间序列](https://github.com/pycaret/pycaret/?platform=hootsuite&utm_campaign=HSCampaign#pycaret-new-time-series-module)，(2021)

[3]Glenn Carstens-Peters 在 [Unsplash](https://unsplash.com/s/photos/comparison?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2019)

[4] PyCaret，[时间序列 Jupyter 笔记本示例](https://nbviewer.org/github/pycaret/pycaret/blob/time_series_beta/time_series_101.ipynb)，(2021)

[5]Bagus herna wan 在 [Unsplash](https://unsplash.com/s/photos/phone-app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)