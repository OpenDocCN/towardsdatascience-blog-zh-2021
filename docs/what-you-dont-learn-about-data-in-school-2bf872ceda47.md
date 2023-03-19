# 你在学校学不到的数据

> 原文：<https://towardsdatascience.com/what-you-dont-learn-about-data-in-school-2bf872ceda47?source=collection_archive---------38----------------------->

## 如何用不完美的数据得到有用的结果

![](img/81dd99d97ae7c9f24824e86aa6eb5e63.png)

照片由来自[佩克斯](https://www.pexels.com/photo/woman-using-vr-goggles-outdoors-123335/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[布拉德利·胡克](https://www.pexels.com/@bradleyhook?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

有许多资源可供数据科学家和数据分析师学习机器学习、SQL、Python、数据可视化等等。这些对开始数据分析职业生涯的人和试图提高技能的人非常有帮助。然而，学校并没有为你准备好我所说的“数据现实”。你通过完美完整的数据进行学习，但你在现实世界中得到的是肮脏的，有时是不完整的数据。运用完美数据调整所学并将其应用于“数据现实”的能力将使你成功——以下是几个例子。

## 显示方向结果

我曾经被要求帮助市场部建立一个 A/B 测试，以评估他们的电子邮件系列的有效性，该系列旨在将用户转化为试用用户并成为付费用户。我被拉去做其他项目，我们不得不等到雇佣了营销数据分析师之后，才能对 A/B 测试进行评估。这是当我们发现测试已经运行了 6 个月还没有正确设置的时候。控制组和测试组的比例并不像我们最初打算的那样是 50/50。

如果这是一个关于 A/B 测试的类，你将会收到完美的测试数据，有适当的 50/50 分割，足够的样本大小的用户，并继续评估统计显著性。实际的电子邮件测试并没有满足所有这些参数。我们不能告诉市场部我们必须重新进行测试，再等 6 个月。我们如何挽救这个数据不完善的测试？

由于数据不符合适当的 A/B 检验标准，统计显著性被排除了。队列分析是我们能想出的挽救结果的唯一方法。用户被分为控制组和测试组，然后根据点击或打开电子邮件的用户与没有点击或打开电子邮件的用户进行细分，以显示产品参与度、试用启动率以及向付费会员的转化。

在利益相关者的陈述中，结果被认为只是方向性的，不符合统计学意义的标准。营销部门很高兴，因为与对照组相比，测试组表现出了更高的参与度和试用启动率，尽管这并不显著。现实是利益相关者需要向他们的老板报告结果，只要他们是积极的，即使没有统计学意义，这也比显示消极的结果要好。

你可能会想，如果测试组的参与度低于控制组，我们会如何处理这些结果。然后，我们可能会尝试分析系列中的每封电子邮件，以确定测试组中与控制组相比参与度较低的邮件，或者试用开始率较低的邮件。A/B 测试出错的可能性无穷无尽，适应显示方向洞察力是挽救结果的一种方法。

**Takeway:** 在缺乏完美数据的情况下，对你的用户进行细分，找到有方向性的见解。利益相关者不需要完美。有时候，在更好的数据出现之前，正确方向的指导就足够了。

## 针对数据差距进行调整

[营销归因](https://www.marketingevolution.com/marketing-essentials/marketing-attribution)在现实中比你在学校里学到的更难实现。问题是，公司很少或根本没有追踪将销售和转化归因于特定的营销接触点。虽然我知道这些问题，但当我被要求将营销努力归因于收入时，我不能很好地说这是不可能的。我如何利用部分跟踪数据将收入归因于营销努力？

首先，我确定了所有推动收入的营销活动，如引导用户开始试用的付费营销广告，以及旨在将当前试用用户转化为付费会员的试用电子邮件系列。然后，我将个人转换率和总转换率结合起来，应用于我拥有用户级别和活动级别信息的活动，以估算收入。结果并不完美，但在归因追踪得到改善之前，对于第一轮测试来说已经足够好了。

**要点**:用汇总信息或外部来源填补数据缺口，直到更好的数据出现。结果不会尽善尽美，但有方向性的见解是良好的第一步。

## 最后的想法

无论你上了多少课，当你遇到现实生活中的情况时，都不会是一样的。成功的关键是能够把你学到的东西应用到可用的数据中。虽然我没有涵盖每一个不完美的数据场景，但我希望这能让您在如何处理您的“数据现实”方面有一个良好的开端。

## 你可能也会喜欢…

[](https://medium.com/swlh/how-i-used-a-machine-learning-model-to-generate-actionable-insights-3aa1dfe2ddfd) [## 我如何使用机器学习模型来生成可操作的见解

### 将数据科学与数据分析相结合

medium.com](https://medium.com/swlh/how-i-used-a-machine-learning-model-to-generate-actionable-insights-3aa1dfe2ddfd) [](/how-to-present-machine-learning-results-to-non-technical-people-e096cc1b9f76) [## 如何向非技术人员展示机器学习结果

### 展示利益相关者能够理解的模型结果

towardsdatascience.com](/how-to-present-machine-learning-results-to-non-technical-people-e096cc1b9f76) [](/how-to-translate-machine-learning-results-into-business-impact-d0b323112e87) [## 如何将机器学习成果转化为商业影响

### 向高管解释模型结果

towardsdatascience.com](/how-to-translate-machine-learning-results-into-business-impact-d0b323112e87)