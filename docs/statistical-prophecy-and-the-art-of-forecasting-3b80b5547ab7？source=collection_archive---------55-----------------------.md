# 统计预言和预测的艺术

> 原文：<https://towardsdatascience.com/statistical-prophecy-and-the-art-of-forecasting-3b80b5547ab7?source=collection_archive---------55----------------------->

## 利用脸书的先知进行平衡预测

作者:普拉森·比斯瓦斯和[钱丹·杜吉亚](https://medium.com/u/25a92c29cc1?source=post_page-----cbd4d7965c2e--------------------------------)

![](img/40f56799a81484b83e6b70bd5392da4d.png)

资料来源:Unsplash 的 Hannah Jacobson

**前奏:** 《追加保证金通知》被认为是 2008 年金融危机前后制作的最好的电影之一。如果你没看过，一定要看。有着令人震惊的资本主义故事和强大的演员阵容——这的确是一个成功。虽然电影中有许多精彩的对话，但有一段对话一直留在我的脑海中，其中银行的首席执行官/董事长对他的下属说:“你想知道为什么我会和你们一起坐在这张椅子上吗？我是说，为什么我能挣大钱。我来这里只有一个原因。我在这里猜测音乐从现在起一周、一个月、一年后会发生什么。就是这样。仅此而已”。

在这种情况下，音乐意味着“情况”。也就是说，一切都是为了预测未来“一周、一个月、一年以后”，你预测得越好，你就能赢得越大的比赛…

已经有很多统计工具，如 ARIMA(X)、指数平滑、LSTM、误差修正模型等被大量用于预测。然而，使用这些技术，模型的质量通常是不合格的，因为调整这些模型并不容易——除非您的团队中有一个博学的人。凭借极其直观的超参数，Prophet 成为了游戏规则的改变者。

正如他们所说，银行业是管理风险的行业，当你的部门被称为“银行中的银行”时，你就知道自己在主持大局。

简而言之，任何银行的资产负债管理(ALM)部门处理两个关键职能:管理货币(存款)的供应和满足贷款账簿的需求。这些功能反过来驱动银行的两个最重要的 KPI——流动性和盈利能力(与借贷利率差相关)。

从传统银行业的角度来看，ALM 一直是并且仍然是任何银行的核心职能。然而，随着竞争压力的增加，ALM 桌面的优化受到了极大的关注，这在商业上非常有意义。假设有一笔 5 亿美元的贷款，即使 ALM 设法在一年内为一家银行提高 100 个基点的回报率。这相当于节省了 500 万美元。

利率等市场变量。受全球经济的驱动，而且总是难以准确预测。但是，如果 ALM 部门能够准确预测贷款余额提取/预付和存款流入/提取，银行将能够以最佳方式管理资金，从而提高流动性和盈利能力。

预测贷款和存款余额具有挑战性，因为有太多因素在起作用:

1.**趋势**(例如，某一特定存款产品可能会在市场上大获成功，而且随着口碑的传播，存款可能会在其饱和之前显著增加)

2.**期权性**(例如贷款提前还款、基于行为的存款支取)

3.**季节性**(例如，存款余额在某些月份随着人们从税务部门获得退款而增加)

4.**节假日**(如节日假期存款余额减少)

5.**银行策略**(例如，为了提高某一特定产品的可销售性，银行可能会向客户提供更高的回报或开展强有力的营销活动)

6.**其他行为维度**。

有鉴于此，传统技术无法产生强有力的模型。关键问题在于——这些技术可能很难调整，并且通常太过不灵活，无法结合某些假设或试探法。

这就是脸书的先知模式正在获得巨大的接受。Prophet 将预测问题归结为曲线拟合，而不是明确考察时间序列中每个观察值基于时间的相关性。此外，它在逻辑上将时间序列函数划分为:

![](img/d0e0b159f464a64abe849759f1526d8d.png)

最后，它提供了极为直观的超参数来管理这些组件中的每一个。下文将讨论一些关键参数。

## A.**“趋势”**相关超参数:

a.**变化点**T16:这是一个超参数，用于捕捉时间序列中的任何突变。使用此参数可以轻松捕获未来的计划战略转移(#5，在上面的列表中)。请注意，如果用户没有提供任何变化点，Prophet 会尝试自己确定这些变化点。

**b.** **变化点 _ 优先 _ 规模:**并非所有战略变化都有影响。其中一些比另一些更有影响力。此超参数调整趋势的强度。默认值为 0.05 时，可以减小该值以使趋势更灵活，反之亦然。

c.**增长**:时间序列的趋势可以在一定水平上继续增加/减少或饱和。再次举例来说，一个新的营销活动可以促进销售在短期内，但它会饱和一段时间后。此参数可帮助定义趋势的上限和下限。(以上列表中的第 1 位)

## B.”**季节性**相关超参数:

这就是 Prophet 明显优于其他模型的地方。(以上列表中的#3)

a.**季节性 _ 模式**:可以设置为“加法”或“乘法”。如果预计季节模式在未来保持相似，则使用“加法”模式(2023 年的季节性将与 2015 年的季节性相似)，否则使用“乘法”模式。

b.y 的 3 个超参数**早期季节性，周季节性，日季节性**。根据数据的不同，这些可以设置为真或假

c.**季节性 _ 先验 _ 规模**:类似 changepoint _ 先验 _ 规模。这反映了不同程度的季节性模式。

## C.**假期**相关参数:

a.**假日列表:**我们可以向模型传递一个定制的假日列表来捕捉任何假日(上面列表中的#4)

b. **holidays_prior_scale:** 类似于 changepoint_prior_scale。这反映了各种假期的不同影响。

上面列表中的#2 和#6 是作为利用“附加模型”框架的“趋势”拟合的一部分捕获的。

因此，总的来说，除了 Prophet 提供了简单的超参数外，值得注意的是，即使使用默认参数，模型也会自动调整到很高的精度。因此，它不需要很强的时间序列建模能力。

## 下面是一个通用代码，它遍历各种参数，目标是获得最小的 MAPE。

```
*from fbprophet import Prophet**from sklearn.model_selection import ParameterGrid**params = {‘growth_mode’:(‘multiplicative’,’additive’),**‘seasonality_mode’:(‘multiplicative’,’additive’),**‘changepoint_prior_scale’:[0.1,0.2,0.3,0.4,0.5],**‘holidays_prior_scale’:[0.1,0.2,0.3,0.4,0.5],**‘n_changepoints’ : [50,100,150]}**grid = ParameterGrid(params)**count = 0**for p in grid:**count = count+1**start=start_date**end=end_date**model_parameters = pd.DataFrame(columns = [‘MAPE’,’Parameters’])**for p in grid:**Pred = pd.DataFrame()**random.seed(120)**train_model =Prophet(growth = p[‘growth_mode’],**changepoint_prior_scale = p[‘changepoint_prior_scale’],**holidays_prior_scale = p[‘holidays_prior_scale’],**n_changepoints = p[‘n_changepoints’],**seasonality_mode = p[‘seasonality_mode’],**weekly_seasonality=True,**daily_seasonality = True,**yearly_seasonality = True,**holidays=holiday,**interval_width=0.95)**train_model.add_country_holidays(country_name=’US’)**train_model.fit(X_train)**train_forecast = train_model.make_future_dataframe(periods=42, freq=’D’)**train_forecast = train_model.predict(train_forecast)**test=train_forecast[[‘ds’,’yhat’]]**Actual = df[(df[‘ds’]>start) & (df[‘ds’]<=end)]**Mape = mean_absolute_percentage_error(Actual[‘y’],abs(Pred[‘yhat’]))**print(‘MAPE is : ‘,MAPE)**model_parameters = model_parameters.append({‘MAPE’:Mape,’Parameters’:p},ignore_index=True)*
```

*#一旦收到最终参数，您就可以构建最终模型(final_model)并可以预测 n 个周期。*

```
*future = final_model.make_future_dataframe(periods=18, freq=’D’)**forecast = final_model.predict(future)*
```

**顶端的樱桃:**

Prophet 在某种程度上只是自回归(AR)模型的扩展，其中除了使用滞后变量之外，还使用输入变量的傅立叶变换来生成补充特征。这可以更好地调整模型，从而提高性能，并提供分解结果的能力，以获得更好的可解释性。

这里的关键问题是，时间序列数据很少在一段时间内遵循单一模式。为了解决这个问题，引入了 *NeuralProphet* 来帮助映射非线性函数来逼近任何连续函数，从而给出更好的拟合。这里不涉及太多细节，但要记住 NeuralProphet 的关键特性。

1.PyTorch 的梯度下降优化引擎使建模过程比 Prophet 快得多

2.自相关使用自回归网络建模

3.使用单独的前馈神经网络对滞后回归量进行建模

4.前馈神经网络的可配置非线性深层

5.您可以使用“neuralprophet”包在 python 中定制损耗和度量模型。

**结论:**

准确的余额预测是任何银行的关键需求之一。围绕余额预测的其他用例可以围绕财务规划和分析、PPNR (CCAR)建模、业务单位级别的余额预测等。

Prophet 是一个优秀的软件包，它为时间序列预测提供了很大的准确性。随着越来越受欢迎，这可能成为银行在未来进行时间序列预测时所依赖的关键工具之一。在那之前，这是一场竞赛，一些 ALM 部门肯定会比其他部门多创造额外的基点。

时间会证明一切！..在那之前，祝你学习愉快！！

**免责声明**:本文中表达的观点是作者以个人身份表达的观点，而非其各自雇主的观点。