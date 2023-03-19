# 数据报告的兴起

> 原文：<https://towardsdatascience.com/the-rise-of-dataprepops-dd21908a7d1?source=collection_archive---------36----------------------->

## 现代数据开发工具以及数据质量如何影响 ML 结果

![](img/136e10f1848efd3a855515463a686f44.png)

美国宇航局在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

ML 就在我们身边！从医疗保健到教育，它正被应用于影响我们日常活动的许多领域，并且能够带来许多好处。

数据质量在人工智能解决方案的开发中具有非常重要和重大的作用——就像古老的“垃圾进，垃圾出”——我们可以很容易地理解数据质量的权重及其在癌症检测或自动驾驶系统等解决方案中的潜在影响。

但是，矛盾的是，数据可能是[最被低估、最不被炒作的人工智能。](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/0d556e45afc54afeb2eb6b51a9bc1827b9961ff4.pdf)幸运的是，在经历了[几个世界范围的有影响的错误](https://www.axios.com/england-exams-algorithm-grading-4f728465-a3bf-476b-9127-9df036525c22.html)之后，随着数据开发工具的兴起，数据的魅力正在回归。

# 数据开发

很多人都在谈论 ML 能带来的好处和好处。然而，围绕许多组织为获得人工智能投资回报而进行的斗争的数字不断涌现。

> 他们是在撒谎吗？或者在 ML 流程中是否有一个缺失的部分比我们想象的更相关？

答案简单明了:数据质量。

数据质量有许多不同的方面，特别是，用于机器学习开发的数据有其自身的质量要求。在机器学习开发领域出现了许多新工具，人们对 [MLOps](http://mlops.community/) 以及如何解决现有的组织问题以将人工智能成功投入生产有着浓厚的兴趣。但是**数据质量**呢？

在流程的不同阶段，可以看到和测量不同的数据质量——ML 流程中的数据质量始于数据的定义，是测量可能影响生产模型的数据漂移所必需的，如下图所示。

![](img/c656b0acf4988989e8d1e7268e932a45.png)

ML 流程中不同的数据质量阶段。

在今天的博文中，我们将特别关注其中的一个步骤:数据准备。

## 数据准备

虽然没有“构建”模型那么有趣，但数据准备是数据科学开发过程中最重要但也是最耗时的工作之一。数据科学团队执行这一步骤所花费的时间可能因公司规模和垂直行业而异，但根据 Anaconda 的[最新报告，这仍然是数据科学的步骤之一](https://www.anaconda.com/blog/2020-anaconda-state-of-data-science-report-moving-from-hype-toward-maturity)

> “(…)占用了真正的数据科学工作的宝贵时间，并对整体工作满意度产生了负面影响。(…)"

数据准备涉及许多不同的步骤，从数据访问一直到特征选择:

*   **数据访问**:每个数据科学项目的开始都是从收集数据来回答一系列业务问题开始的(或者至少应该这样！)，但有时这一过程可能比预期的时间长一点—要么数据不存在，需要设置数据工程流程，要么有太多的安全层阻止对数据的透明访问。像[差分隐私、合成数据和联合学习](https://medium.com/ydata-ai/privacy-preserving-machine-learning-3192b344228c?sk=2c9d3afa436479f8cceba07c4536835e)这样的解决方案是缓解数据访问相关问题的可行选择。
*   **数据扩充:**在某些情况下，可用的数据集太小，不足以支持 ML 模型。数据裁剪、旋转、开窗或[合成数据](https://ydata.ai/products/synthesizer)等解决方案会有所帮助。
*   **数据清洗:**从缺失值插补到不一致，数据充满了错误，需要进行相应的清洗和预处理。根据数据的类型，这一过程不仅[耗时，而且非常复杂](https://pdf.sciencedirectassets.com/280203/1-s2.0-S1877050919X00174/1-s2.0-S1877050919318885/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLWVhc3QtMSJHMEUCIGvb0BSrJQJC%2FYqg8MZl3MDN%2Fq2kJD0zRLLpmgRqmBdKAiEAhjCppr%2BaWGcgJSuYvXXqK344WDB4GalnVFyOGuZnFHwqtAMIfBADGgwwNTkwMDM1NDY4NjUiDKDcvyYdwSAndgg87yqRAyvHVgrrlt53TM6x2zSud0G2nMKOpZBzSD%2F%2FTlRKPz6NAhOnR6sDo70kUPfKsNdFuiKiQ9Hg0enV5k8WjlmrqiqGPnm2tTnn6I5ruUrcqtyYyi2d7PijWV3L9yEzfM8QvaE7YwdfS3%2BrQasiyOPhtu%2FoBkNeKIk%2F20bz2f2AqbWL9CT8AzguYxeSr4Xeqp5Hvbp7%2FZkwvj2kRAcKBoZcpx4E9p3%2F8mIVq60t82dyyj%2FXezpXrc5onRVEQCQhVzau29GOFaqOpKBJTqZVUA08RnzWuW%2FuyFzfgsLRfe3IJCwR21rqMHqm%2BknW6c3ullQqEtHZjuc6kjjRmXonO33e9GxGk00iw3yVpsxbSKcewKfv868fRnYO%2FRUUD8YSViXH1CH%2Bj%2FJtmWoxkKfbbqYbYU6YH4f4xzYdAr2Qole86gPfm5viFcTDCCP1QmbTNzfB%2BZwA9EVgh0TnwYARcwMbC2w8chy6NkfvVXIeZwfZR7u5wenCQhJl2KHs6tv0iHUXubGenwzFs5v6YmMWN7eS1khkMIiA2YIGOusBw61%2FQcVCqgPOwTqpA6pRQ%2BQiy%2FXQKUavU3DOiEWf%2BWVtTKw9ahFQyMfL1PWwxrRDNb%2BAUgwObP%2FHtihekGp4Uh%2BtJOaJLNts%2BwXddToaihQfX1MR5UD1sjGVOKkHkhK7un5rL3ANyV7x8dzE3p2ZpXgbZNMVGxjzmCDRpR9W0ZA2lYX%2F5XLZL9qpYnpOumLJEVYXK%2Fo%2BtiExO1d62JLdjFh7di%2FIm8ZxFNegSIBqOwnpOmUYJ%2FfH7C%2Fv8a4wj%2BvU8bhdWmP3mlXZTIaXTeuwptR9T8gCbPIe6TRuAbj%2FUeqs3BvLgrpph7g9kA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20210320T193302Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYSTRLFPUI%2F20210320%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=78ff413990b3f6e12fd162037690d2995129367cf5c25cad2398947096d1a5cb&hash=33cb7eea330b00f4e915c8ea41e1c90a270992e47e7c1ed4b429d3369a871f16&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S1877050919318885&tid=spdf-c77bccf6-175d-4971-99eb-522cc68b621c&sid=b9164a797b6b614dff1b4161b3b19e054337gxrqb&type=client)。
*   **数据标注:**通常情况下，标签会从数据集中丢失，或者可用数量太小，无法从监督学习方法中受益。从基于规则的解决方案到合成数据，有一些新的选项可以帮助数据科学团队解决这个问题。
*   **数据验证:**我们如何确保我们正在处理的数据的质量？在所有的准备工作之后，我们如何衡量应用的转换带来的好处呢？在整个开发过程中持续测量数据质量对于优化决策至关重要，从对分布的单变量理解到验证数据对模型开发的效用和影响。
*   **特征工程:**数据准备过程的最后阶段。这也是业务知识派上用场的地方，对于特征提取过程，数据科学团队绝对可以大放异彩。毕竟，功能对业务的相关性和影响力越大，也会影响下游[模型的可解释性](/how-can-i-explain-my-ml-models-to-the-business-dc4d97997d64)。

尽管如此，在 ML 过程的这个阶段还有很多事情要做，正如风险投资公司 [Workbench 的最新报告](https://www.work-bench.com/the-state-of-data-infrastructure-analytics-report-2021)中所指出的，数据准备毫无疑问是 ML 开发领域中许多不同工具中缺失的一部分，它正以一个新的名字**data repo PS 崛起。**

**Data repo PS**是一种数据科学和 ML 工程文化和实践，包括一系列旨在为 ML 系统运营(Ops)构建训练数据集(DataPrep)的步骤。

# 结论

既然我们已经确定并体验了数据质量在 ML 模型中的影响(这里的例子)，一个新的范例可能会浮出水面——数据开发——这都是因为糟糕的数据质量在许多不同的环境中会产生巨大的影响。

数据质量差的[成本](/the-cost-of-poor-data-quality-cd308722951f?sk=d3bddad82f326a3917be48e447684a0d)，不仅直接影响业务，尤其是在后期才被发现(使用不良数据开发的模型已经投入生产)，还会严重影响数据科学团队的生产力和效率。

*数据报告*或*数据开发工具*是 ML 开发流程中缺失的部分。结合来自[人工智能基础设施堆栈](https://ai-infrastructure.org/)的正确工具，如功能商店和模型部署平台，数据开发工具可以帮助组织将人工智能用作竞争优势，利用他们真正有价值的资产，即他们的数据。

[*法比亚娜*](https://www.linkedin.com/in/fabiana-clemente/) *是 CDO*[*YData*](https://ydata.ai/?utm_source=medium&utm_medium=signature&utm_campaign=blog)*。*

**用改进的数据加速 AI。**

[*YData 为数据科学团队提供第一个数据开发平台。*](https://ydata.ai/?utm_source=medium&utm_medium=signature&utm_campaign=blog)