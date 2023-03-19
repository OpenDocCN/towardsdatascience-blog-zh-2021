# 营销增量提升测试 101

> 原文：<https://towardsdatascience.com/marketing-incremental-lift-test-101-f2983af1da8e?source=collection_archive---------4----------------------->

## **什么是电梯测试？为什么重要？一步一步的指导如何设置它，并使用提供的模板分析您的结果。**

![](img/a26da889a074190433ac4d66a086f6d4.png)

Firmbee.com 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[的照片](https://unsplash.com/@firmbee?utm_source=medium&utm_medium=referral)

在市场上这么多不同的营销平台和策略中，选择能产生有效营销 ROI 的最佳策略组合可能是压倒性的。通常，由于担心失去潜在的营销机会，越来越多的美元被花费在额外的营销技术堆栈或活动上，而不知道额外的收益是否值得。为了避免浪费开支，Lift Test 是一种统计方法，用于在为项目分配预算之前评估您的选项。

**电梯测试简介**

提升测试通过将一个独立变量应用于测试组而不是维持组来帮助识别因变量的增量回报和因果效应。

例如，Lift Test 可以帮助您回答在脸书上投放广告(自变量)与不投放广告相比，可以从一组受众中提高多少转化率(因变量)。

然后通过[假设检验](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing)方法评估提升测试结果。如果测试组和抵制组之间的提升具有统计显著性，则接受替代假设以支持在脸书投放广告将有助于增加转化率，并且拒绝零假设，即在脸书投放广告不会有助于增加转化率。

**A/B 测试和 Lift 测试有什么区别？**

提升测试是 A/B 测试的一种。提升测试的本质是通过不向坚持组提供处理来发现测试组的增量值。因此，你不只是衡量一个活动的绝对结果，而是量化如果没有这个额外的活动，你还能获得多少转化率。另一方面，A/B 检验将为两组都提供治疗，但只是不同的治疗，它不是用来寻找增量回报，而是绝对相关性。因此，在 A/B 测试中通常称为控制组，而在 Lift 测试中则称为保持组。

**有什么统计学意义？**

测量[统计显著性](https://hbr.org/2016/02/a-refresher-on-statistical-significance)是为了帮助您确信您的测试结果不是随机的，您希望测试结果的置信水平与您在计算样本量时设置的 alpha 水平(显著性水平)有关，参见[样本量计算器模板](https://docs.google.com/spreadsheets/d/1EBQYvJsgYRjMRP6t-aBVvGngfjVL_VjxFntnF61jNfA/edit?usp=sharing)中的公式。基于 P 值测试结果产量(参见[显著性计算模板](https://docs.google.com/spreadsheets/d/12C8vXYmzQHTcuSWvF7_FHc5DPGMbaRWExUIjG23SySw/edit?usp=sharing)，如果 1- (P 值)，观察显著性，大于目标显著性水平，您接受替代假设并拒绝零假设。

**电梯测试用例示例:**

1.  考虑到人们可以从其他现有的活动中转化过来，一个新的活动值得发起吗？
2.  通过使用新的供应商来帮助拓展潜在客户，我的增量投资回报率是多少？
3.  与不提供任何折扣相比，当提供折扣以激励客户下第一个订单时，我的增量 CAC(客户采购成本)是多少？

**启动电梯测试的逐步指南:**

1.概述测试目标和测试变量(独立变量)，如是否投放脸书广告。注意，总是建议一次只测试一个变量，以控制数据噪声。

2.根据目标，您将能够选择要用于衡量结果的 KPI，包括因变量，如转换率和每次收购的允许成本。

3.确定测试是单尾的还是双尾的。测试的类型将影响您在下一步如何计算所需的样本量。如果测试组的提升被认为只是一个积极的影响，那么它就是一个单尾测试。但是，如果电梯可以造成负面影响，如过度营销，可以推开客户不转换，那么它将是一个双尾测试。

4.根据不同的提升水平确定所需的最小样本量。首先，设置α和β，以获得所需的显著性和功效水平。样本大小将根据预期的维持组性能而有所不同，维持组的性能越高，相同提升级别所需的样本大小就越小。这同样适用于升力水平，升力越高，所需的样本量越小。

> [样本量计算器模板](https://docs.google.com/spreadsheets/d/1EBQYvJsgYRjMRP6t-aBVvGngfjVL_VjxFntnF61jNfA/edit?usp=sharing)

5.根据所需的最小样本量，检查满足预期显著性所需的提升对您的测试场景是否有意义。如果是，估计收集所需样本量的测试费用。如果所需的升力太高，寻找更大的样本大小及其所需的升力，看看是否可以实现。同样的事情也适用于预算控制。如果较大的样本量需要超出预算的成本，那么您可以选择用较小的样本获得更高的提升。这是一种平衡行为。

6.准备测试受众，并确保在维持组和测试组之间随机选择样本。

比如可以用 Python [熊猫。DataFrame.sample](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sample.html) 函数创建随机生成的两组(如果使用自定义受众列表)。请注意，添加 random_state 参数有助于为将来的验证复制相同的随机选择。

代码示例:

```
#import pandas packages
import Pandas as pd#Create a subset of audience for the test group
test_female_under_30_ios = df[(df.segment_name == 'female_age_under_30') & (df.ios_user_flag == 1)].sample(n = 4442, random_state = 1)
```

7.根据测试类型，准备其他测试材料，如活动创意、活动跟踪设置等。

8.通过量化因变量(如维持组和测试组之间的转换率)的提升来测量测试结果，以确定基于样本大小的提升水平是否达到所需的统计显著性。

9.根据统计显著性拒绝或接受零假设。如果要拒绝零假设(接受投放广告有帮助)，那么就要计算业务盈利能力指标，以评估这种处理方式是否划算。如果每次收购的增量成本在允许的范围内，这意味着测试变量(例如，提供脸书广告)产生了足够的增量转换(例如，比维持组更多的转换)，值得实施。

**计算统计显著性的指标包括:**

> [提升分析模板](https://docs.google.com/spreadsheets/d/12C8vXYmzQHTcuSWvF7_FHc5DPGMbaRWExUIjG23SySw/edit?usp=sharing)

注:n1=测试样本大小，n2 =维持样本大小

测试转换率:p1

保持转换速率:p2

测试标准误差(SE1): SQRT(p1×(1-p1)/n1)

维持标准误差(SE2): SQRT(p2×(1-p2)/n2)

Z 分数:(p1-p2)/SQRT(POWER(SE1，2)+POWER(SE2，2))

P 值(Excel): 1-常模。DIST(Z 分数，真)

P 值(Google Sheet): 1- NORM。DIST(Z 分数)

观察到的显著性:1 个 P 值

上面的公式是使用 Excel 和 Google Sheets 中的语法编写的，所以可以直接应用到您的 Excel/Google 工作簿中进行分析。

希望你喜欢这篇文章，并找到它的帮助。如果您有任何问题，请随时发表评论。我希望听到您的反馈！