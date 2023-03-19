# 如果 A/A 测试失败，A/B 测试就失去了光彩

> 原文：<https://towardsdatascience.com/an-a-b-test-loses-its-luster-if-a-a-tests-fail-2dd11fa6d241?source=collection_archive---------7----------------------->

## 实验和因果推理

## A/A 测试的统计方法

![](img/89e1f59e18c607c2b12aa56c5af77c3f.png)

安迪·萨拉扎在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

一个严格的实验过程。A/B 测试已经成为一种时尚，并在技术领域广泛采用。作为早期采用者，FAANG 公司已经将实验融入到他们的决策过程中。

例如，[微软必应](https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/)对其 80%的产品变更进行 A/B 测试。谷歌依靠实验来确定面试过程中表现最好的候选人。[网飞](https://netflixtechblog.com/interleaving-in-online-experiments-at-netflix-a04ee392ec55)利用交错，一种成对实验设计，改进了个性化算法。

越来越多的采用实验源于其高水平的内部有效性，这进一步由两个因素决定。首先，数据科学家在宏观层面上对整体研究设计和模型选择具有选择性，确保所选设计适合问题及其关键假设完整无损。第二，数据科学家在微观层面上对随机化过程非常小心，并将人群分成两个可比较的组。

> 然而，你怎么能如此确定分配分割按设计的那样工作呢？
> 
> 拆分比例不对怎么办？
> 
> 你如何诊断错配？

这些问题值得立即关注。为了建立可信的实验平台，数据科学家必须仔细检查随机化过程；否则，实验平台会在干预前在治疗组和对照组中引入偏差，从而使任何测试结果无效。

在之前的帖子中，我们已经学习了 [A/B 测试的有效性](/the-turf-war-between-causality-and-correlation-in-data-science-which-one-is-more-important-9256f609ab92?sk=10169c40b1a6077758385525f22f405d)、[常见陷阱](/online-controlled-experiment-8-common-pitfalls-and-solutions-ea4488e5a82e?sk=736ba64dc8481993cd98e043256c4bf4)、[用户干扰](/how-user-interference-may-mess-up-your-a-b-tests-f29abfcfccf8?sk=9ff3bbae01ff9951e6eca33e1c75cff4)和[最佳实践](/a-practical-guide-to-a-b-tests-in-python-66666f5c3b02?sk=069c502acb69d326a3ea559273c7a04f)。在今天的帖子中，我们建立了一个简化的内部实验平台，并检查它是否可以呈现两个可比较的组。

# 什么是 A/A 测试？

正如 Ronny Kohavi 所建议的，我们应该进行一系列的 A/A 测试，并检查 p 值分布。对于那些实验领域的新手来说，Ronny 被认为是在线实验的教父和“[值得信赖的在线控制实验](https://www.amazon.com/Trustworthy-Online-Controlled-Experiments-Practical/dp/1108724264/ref=sr_1_1?dchild=1&keywords=Trustworthy+Online+Controlled+Experiments&qid=1625367079&sr=8-1)的作者

> 如果我们不能通过 A/A 测试，A/B 测试就失去了它的光彩。

从概念上讲，A/A 测试遵循与 A/B 测试相同的设计逻辑，将用户分成两组，并相应地分配处理方法。唯一的区别是 A/A 测试对两组都分配了相同的治疗。

因此，实验组在随机化后具有相同的协变量分布，并接受相同的治疗条件。我们不应该期望结果指标有什么不同。在测试之前知道真实的无效效果作为基准，并将其与从实验企业获得的实际效果进行比较，这是 A/A 设计的最大优点。

# 为什么要进行 A/A 测试？

主要有两个目的。首先，收集数据并评估功率计算关键指标的可变性( [Kohavi 等人，2012](https://notes.stephenholiday.com/Five-Puzzling-Outcomes.pdf) & [2020](https://www.amazon.com/Trustworthy-Online-Controlled-Experiments-Practical/dp/1108724264) )。[在之前的帖子](/a-practical-guide-to-a-b-tests-in-python-66666f5c3b02?sk=069c502acb69d326a3ea559273c7a04f)中，我们已经了解了什么是功效分析，样本量由以下公式决定:

![](img/710041f96afea0995fbe1e885712a0db.png)

我自己的截图

其中:

*   ***σ:样本方差。***
*   *𝛿:治疗组和对照组之间的差异。*

我们可以对指标运行 A/A 测试，以获得其方差(*)并为 A/B 测试的样本量计算做准备。*

*第二，A/A 测试评估实验平台是否按预期工作，这是今天帖子的主要目的。*

*例如，我们希望流量平均分配，一半用于处理，另一半用于控制。在流量分割之后，我们获得每组中的实际观察数量。*

> *我们如何判断观察到的比率分割在统计上是否不同于预期的比率？*
> 
> *进行卡方检验！*

*我们可以运行卡方检验来检查观察到的比率和设计的比率在统计上是否不同( [Kohavi 等人 2012](https://notes.stephenholiday.com/Five-Puzzling-Outcomes.pdf) & [2020](https://www.amazon.com/Trustworthy-Online-Controlled-Experiments-Practical/dp/1108724264) )。如果差异确实具有统计学意义，我们可以得出结论，在干预之前，实验系统包含选择偏差。让我们在下一节详细说明如何解释这个结果。*

*![](img/125dbe50b9f8969949d825ff2865d4b4.png)*

*迈克尔·巴钦在 [Unsplash](https://unsplash.com/s/photos/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片*

# *如何解读？*

*尽管两组接受相同的治疗条件，我们仍然应该期望在 A/A 测试中有合理的*假阳性率。在重复的 A/A 试验中，给定指标的零假设应该被拒绝 5%的时间，设置置信水平为 95%。**还有，重复 A/A 试验的 p 值遵循均匀分布，这也是今天博文**的中心论点。**

**有两种检查分布的方法。首先，我们可以观察 p 值，并检查其分布是否具有以下方案，[正如微软](https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/p-values-for-your-p-values-validating-metric-trustworthiness-by-simulated-a-a-tests/)的实验团队所建议的那样:**

*   ***p < 0.05 为 5%的时间***
*   ***p < 0.10 为 10%的时间***
*   ***p < 0.20 为 20%的时间***
*   ***p < 0.30 为 30%的时间***
*   ***…***

**此外，我们可以进行假设检验，并应用**Kolmogorov–Smirnov 检验**来正式检查它是否遵循均匀分布。我们将在 Python 模拟中实现这两种方法。**

# **当 A/A 测试失败时该怎么办？**

**如果 A/A 测试显示统计差异的结果超过 5%,这意味着我们的实验系统包含选择偏差。我们应该回到数据收集过程并检查根本原因。**

> **如果 A/A 测试失败，停止分析数据并返回数据收集。**
> 
> **检查出了什么问题。**

**A/A 测试可能由于各种原因而失败，我们将重点讨论两个典型原因。首先，治疗组和对照组之间的实际流量分配在统计上不同于预期比率，这种现象称为 ***样本比率不匹配*** 、 ***SRM*** 。请查看以下资源，了解如何诊断 SRM:**

*   **[*Python 中 A/B 测试的实用指南*](/a-practical-guide-to-a-b-tests-in-python-66666f5c3b02?source=friends_link&sk=null)**
*   **[*Fabijan 等，2020，诊断 A/B 测试中的样本比例不匹配*](https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/diagnosing-sample-ratio-mismatch-in-a-b-testing/) *。***
*   **[*Fabijan 等，2019，在线对照实验中诊断样本配比错配*](https://exp-platform.com/Documents/2019_KDDFabijanGupchupFuptaOmhoverVermeerDmitriev.pdf) *。***

**第二，另一个常见的原因是哈希函数，它错误地倾向于一种变体而不是另一种。哈希是实验过程中的一个关键话题，值得在单独的博客文章中讨论。**

# **Python 中的统计模拟**

**在本节中，我们将实验系统中使用的实际散列过程降级，并简化治疗分配过程。降级意味着我们不遵循分配治疗状态的标准两步程序:**

> **1.用户→存储桶**
> 
> **2.铲斗→变型**

**相反，我们使用一个随机数生成器来决定分配组。**

**这很好地满足了我们的目的，因为我们主要关注的是如何运行 A/A 测试，而不是实际的散列过程。**

****第一步:生成正态分布****

**我们的总体服从均值为 100、标准差为 5 的正态分布。**

****步骤 2:对于单次迭代****

```
**Ttest_indResult(statistic = 0.21648391567473418, pvalue = 0.8286548031632164)**
```

**这里，我们应用双样本 t 检验来检验独立性。由于 p 值(0.829)大于阈值，我们得出结论，a1 和 a2 之间没有差异。换句话说，我们已经通过了一次迭代的 A/A 测试。**

**接下来，我们模拟一万次这个过程，计算它导致多少次误报。**

****#目视检查 p 值分布****

**这是一个多余的 if-elif-else 语句。**

```
**count_10_perc/10000
0.0997count_20_perc/10000
0.1035count_30_perc/10000
0.0986
...**
```

**从表面上看，p 值遵循均匀分布，并在 10，000 次模拟中在[0，1]范围内均匀分布。**

****#正式检验 p 值是否服从均匀分布****

**我们从上面的 for 循环中获得 p 值，并应用下面的命令来检查它的分布。**

```
**KstestResult(statistic=0.004665761913861394,pvalue=0.98142384029721)**
```

**对于 Kolmogorov-Smirnov 检验，零假设假设给定的分布(“p_values”)与指定的分布(“均匀”)没有不同。**事实证明，我们未能拒绝零假设，并得出 p 值遵循均匀分布的结论。****

**我对不等分割(例如，10%对 90%)和重尾分布(例如，指数分布)应用了相同的分析程序，没有观察到不同的结果，这与我们的直觉相矛盾。**

**主要原因可能在于，如上所述，我们的模拟处理分配比实际的散列过程更具确定性。我会写一篇关于哈希的后续文章，检查结果是否有变化。**

***完整的 Python 代码请参考我的*[*Github*](https://github.com/LeihuaYe/Statistical_Simulation_In_Python)*。***

# **外卖食品**

*   **我们的实验平台如预期的那样工作！这并不奇怪，考虑到我们使用一个随机生成器来决定变量赋值。**
*   **如果我们遵循实际的散列过程，结果可能会改变。**
*   **Kolmogorov-Smirnov 检验是测试获得的 p 值是否遵循均匀分布的好方法。**

***Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。***

**[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership)** 

# **进一步阅读**

**[](/a-practical-guide-to-a-b-tests-in-python-66666f5c3b02) [## Python 中 A/B 测试的实用指南

### 数据科学家在实验前、实验中和实验后应遵循的最佳实践

towardsdatascience.com](/a-practical-guide-to-a-b-tests-in-python-66666f5c3b02) [](/how-user-interference-may-mess-up-your-a-b-tests-f29abfcfccf8) [## 用户干扰如何会搞乱你的 A/B 测试？

### 来自 Lyft、LinkedIn 和 Doordash 的三个解决方案

towardsdatascience.com](/how-user-interference-may-mess-up-your-a-b-tests-f29abfcfccf8) [](/online-controlled-experiment-8-common-pitfalls-and-solutions-ea4488e5a82e) [## 运行 A/B 测试的 8 个常见陷阱

### 如何不让你的在线控制实验失败

towardsdatascience.com](/online-controlled-experiment-8-common-pitfalls-and-solutions-ea4488e5a82e) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。**