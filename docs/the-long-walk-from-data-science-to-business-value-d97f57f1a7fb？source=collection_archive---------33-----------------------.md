# 从数据科学到商业价值的漫长旅程

> 原文：<https://towardsdatascience.com/the-long-walk-from-data-science-to-business-value-d97f57f1a7fb?source=collection_archive---------33----------------------->

## 为什么从业务角度来看数据科学项目是其成功的关键。

![](img/cf123e9d65934b5b9c4af94d6db7bdd5.png)

照片由[伊斯拉姆·哈桑](https://unsplash.com/@ishassan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在竞争激烈的市场中，企业必须积极探索差异化体验如何丰富其最终用户和生态系统合作伙伴。数据被视为企业的“自然资源”。通过使用企业数据来识别和理解趋势、模式甚至行为，可以实现差异化。机器学习(ML)模型可以被设计为从这些宝贵的企业数据中生成预测，这可以为向最终用户提供差异化提供见解。

这是我从现场学到的:大多数数据科学项目失败是因为他们忽视了业务需求。专家们经常陷入数据科学的虫洞，忽视了共同的目标。如果我们不能从商业的角度建立我们的 ML 模型，我们就丧失了推动业务增长的潜力。让我们考虑一些有助于解决这个问题的关键问题。

# 理解业务问题。

*   [**商业框架工作坊**](https://www.ibm.com/garage/method/practices/discover/frame-your-business-opportunity/) **:** 作为一个值得骄傲的[IBM 人](https://www.ibm.com/uk-en)和 [CSMer](https://hbr.org/2019/11/what-is-a-customer-success-manager) ，我可以说我们在 IBM 做到了这一点。[壁画](https://www.mural.co/)是远程做这个练习的好工具。业务框架研讨会的唯一目的是定义一个适合业务需求的用例。
*   **行业研究:**你的业务是在银行业、零售业还是电信业？了解你所在的行业可以让你根据行业背景来识别关键的业务驱动因素。
*   **业务案例:**用例是否得到了业务案例的支持和资助？如果成功，我们的努力会推动收入增长吗？

![](img/94e2bb6319ea2d4acbd5467396d041d7.png)

照片由[哈迪·穆拉德](https://unsplash.com/@hadimu23?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 从商业角度看这个项目。

这里有三个问题可以帮助我们把握数据项目的脉搏。

1.  我能清楚地向他人阐述业务问题吗？
2.  我是否拥有理解问题所需的所有信息？
3.  我的 ML 模型如何解决这个问题？

# 确保模型解决了业务问题。

有专门研究这个问题的书籍和职业，所以把这个作为模特选择的入门。在这一点上值得一提的是，数据准备和清理不在这里的讨论范围之内——那是另外一天……或者一年，取决于你清理数据需要多长时间。

1.  **ML 模型测试:**测试将数据分成[训练、验证和测试集](https://machinelearningmastery.com/difference-test-validation-datasets/)。然后，我们在训练集上拟合候选模型，在验证集上评估和选择，并在测试集上报告最终模型的性能。保持训练、验证和测试集完全分离对于一个公平的评估是至关重要的。
2.  **ML 模型评估:**哪种模型最能满足业务需求？我们能转向更有效的模式吗？我们是否以合理的方式分割可用数据？

没有完美的模型，我们的目标是找到一个足以解决业务问题的模型。

# 结论

巨大的数据带来巨大的力量。从业务的角度出发锚定数据项目对其成功至关重要。当选择一个 ML 模型时，质疑这个模型是否真正解决了业务问题。

![](img/a5f89ed56fc94645eefd72eb2b179ad1.png)

Joshua Sortino 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 参考

1.  [福斯特·普罗沃斯特和汤姆·福塞特的《商业数据科学》](https://www.amazon.co.uk/Data-Science-Business-data-analytic-thinking/dp/1449361323)
2.  数据挖掘的跨行业标准流程( *CRISP-DM* ) — [什么是 CRISP-DM？](https://www.datascience-pm.com/crisp-dm-2/)
3.  [将数据转化为行动](/transforming-data-into-action-5c8cffd3e8b2)—Lee sch lenker 撰写的《走向数据科学》一文。

## 脚注

实际上，可能没有足够的数据来进行训练、验证和测试。在这种情况下，有两种技术用于近似模型选择:

*   [**概率测度**](https://machinelearningmastery.com/a-gentle-introduction-to-model-selection-for-machine-learning/) :通过样本内误差和复杂度选择模型。
*   [**重采样方法**](https://machinelearningmastery.com/a-gentle-introduction-to-model-selection-for-machine-learning/) :通过估计的样本外误差选择模型。