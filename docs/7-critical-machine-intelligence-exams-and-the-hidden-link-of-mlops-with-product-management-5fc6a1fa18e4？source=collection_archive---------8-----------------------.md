# 7 项关键的机器智能考试以及 MLOps 与产品管理的隐藏联系

> 原文：<https://towardsdatascience.com/7-critical-machine-intelligence-exams-and-the-hidden-link-of-mlops-with-product-management-5fc6a1fa18e4?source=collection_archive---------8----------------------->

## 现在是机器在被释放到世界上之前进行测试的时候了！一个伟大的机器学习产品通过测试变得非凡。MLOps 应该采用新兴的测试实践，将工程活动与产品管理联系起来。

![](img/d19ebe28368d7548b4873fc7c718143b.png)

由[阿克谢·肖汉](https://unsplash.com/@akshayspaceship?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/exams?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

感恩节前一天下午 3 点 15 分。你正在为漫长的周末清理收件箱。然后，你的销售部门主管打电话:*“销售线索的优先排序发生了什么事？？?"* 近期无部署，数据检查完毕。看起来你的感恩节计划突然改变了！

本文将在一个关键的组织、技术以及最重要的机器学习模型应该通过的考试的快节奏纲要中，检查产品管理和 MLOPs 之间的联系。

在过去的 12 个月里，许多机器学习操作工具越来越受欢迎。有趣的是，有一个特征在讨论中明显缺失或很少被提及:质量保证。

学术界已经启动了机器学习系统测试的[研究](https://link.springer.com/article/10.1007/s10664-020-09881-0)。此外，一些供应商[提供数据质量支持](/automated-data-quality-testing-at-scale-using-apache-spark-93bb1e2c5cd0)或[利用数据测试库](https://github.com/ubisoft/mobydq)或[数据质量框架](https://greatexpectations.io/)。自动化部署也确实存在于许多工具中。但是[模型的金丝雀部署](/automatic-canary-releases-for-machine-learning-models-38874a756f87)以及机器学习领域的单元和集成测试中发生的任何事情怎么样？

这些质量保证提案中有许多源自工程思维。然而，越来越多没有工程背景的专家执行大量的模型工程。此外，回想一下，一个单独的人或团队经常运行质量保证活动。据说这样工程师就可以信任其他人来发现错误。更愤世嫉俗的人物可能会坚持认为工程师需要被控制和检查。

在深入机器智能考试之前，允许快速讨论一下工程思维和质量保证授权的危险。

科学不是工程……没关系！

![](img/efc60b2e624d599356715b7ab150d624.png)

[Via Imgur](https://imgur.com/gallery/KkUB0dL)

大多数时候，在科学上，如果事情在一个受控的环境中工作一次是完全没问题的，而且通常，实验可能实际上失败了。在工程界，没有这种奢侈。对于所有的意图和目的，[许多系统永远运行](https://www.youtube.com/watch?v=cNICGEwmXLU)，产生质量保证。这种心态很重要。数据科学家进行实验，很少需要监管或审计环境之外的可重现结果。工程师必须长期可靠地提供可重复的结果。这并没有使科学比工程更容易，但它揭示了目标的差异。因此，将思维模式从数据科学视角转变为工程视角提供了急需的清晰性。接下来，让我们讨论谁可能执行质量保证工作。

测试，测试，测试…有用吗？

![](img/2ef34ef443fd2490679c388402122594.png)

照片由 [@felipepelaquim](https://unsplash.com/@felipepelaquim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/felipe-pelaquim-microphone?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

从工程的角度来看，让我们检查一下经常观察到的质量保证与工程的分离。质量保证有多种形式，但[归结为测试](/why-dont-we-test-machine-learning-as-we-test-software-43f5720903d)。除非你真的担心[护栏编程](https://www.youtube.com/watch?v=kGlVcSMgtV4)，测试自动化才是正道； [Patric 的文章](http://patrick.lioi.net/2011/11/23/guard-rail-programming/)解释了原因。显然，在测试任何东西之前，你需要知道会发生什么。对于通常被操作为[目标函数](https://medium.com/@bhanuyerra/objective-functions-used-in-machine-learning-9653a75363b5)的机器学习，用外行人的话来说:[弄清楚成功看起来像什么](https://medium.com/hackernoon/the-first-step-in-ai-might-surprise-you-cbd17a35708a)。

两个基本的组织方面支配着测试:

*   工程师应该测试他们自己的工作吗
*   测试应该是正式的还是以数据为中心的

委托是不会错的，对吧？那么，为什么不委派专家来测试这个模型呢？为了专门化的[测试自动化编排](https://blog.testproject.io/2018/11/06/the-software-engineer-in-test/)任务，将测试工程从开发工程中分离出来是有充分理由的。然而，由于工程师和测试人员的分裂而导致的不信任文化本质上是不健康的。因此，理想情况下，工程师确保测试他们自己的模型，测试工程师希望自动化测试工作。毕竟，工程师最了解如何正确检查入站数据，以便模型假设不会被破坏。一些角色之间的轮换可能有助于让每个人都充分了解最新的实践。注意，轮换是在同一个团队的角色之间进行的。团队之间的轮换效率要低得多，这是因为工作量的增加/减少。

是使用正式的还是以数据为中心的模型测试通常取决于[正式验证是否可能](https://arxiv.org/pdf/2104.02466.pdf)和经济。在大多数情况下，以数据为中心的测试会非常有效。

让我们回顾一下:要为生产运营建立可靠的机器学习模型，以数据为中心的自动化测试在大多数情况下都是一个优秀的解决方案。

幸运的是，CI/CD 领域中许多奇妙的软件工程工具可以重新用于模型测试。一些[新兴的](https://www.efemarai.com/) [产品](https://kolena.io/)甚至可能在适当的时候提供内置的模型测试功能。如果您不想等待，那么[您可以构建自己的](https://helda.helsinki.fi/bitstream/handle/10138/328526/Makinen_Sasu_Thesis_2021.pdf?sequence=2&isAllowed=y)，或者在 DevOps 团队的帮助下，重新调整现有工具的用途:

1.  [分割和版本化数据集](https://www.analyticsvidhya.com/blog/2021/06/mlops-versioning-datasets-with-git-dvc/)
2.  [将入站数据质量控制在您的 Grafana 中](https://blog.griddynamics.com/lightweight-data-monitoring-solution-for-the-complex-data-quality-task/)
3.  [使用预提交钩子确保数据和模型测试存在](https://blog.devgenius.io/automate-unit-tests-before-each-commit-by-git-hook-f331f0499786)
4.  利用首选 CI 工具/测试框架对模型和数据进行测试
5.  [建立模型监控指标](https://grafana.com/blog/2021/08/02/how-basisai-uses-grafana-and-prometheus-to-monitor-model-drift-in-machine-learning-workloads/)
6.  [金丝雀部署型号与其他代码相同](https://www.linkedin.com/pulse/shadow-deployments-machine-learning-models-aws-carlos-lara/)

本文的剩余部分说明了在[为机器学习系统](https://www.jeremyjordan.me/testing-ml/)设计有效测试时需要考虑的几个值得注意的方面。如果有帮助的话，这些方面对于机器学习模型就像考试对于小学生一样

**功能测试**

![](img/a54f505c3590c02b9260f09c6c78d757.png)

Artem Bryzgalov 在 [Unsplash](https://unsplash.com/s/photos/yogesh-pedam-switch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

[功能单元和集成测试](https://en.wikipedia.org/wiki/Functional_testing)往往是必不可少的。尤其是当将一个较大的模型分解成专用于特定任务的较小的子模型时，比如说作为较大的文章评级模型的一部分的图像和文本分类模型。功能测试应确保选择适当的计算方法，并为每个子模型选择正确的参数和数据。功能测试可能还包括[输入数据质量测试](https://datascience.codata.org/articles/10.5334/dsj-2015-002/)，包括对结构合理但[统计退化数据](https://www.researchgate.net/publication/333123607_The_utility_of_multivariate_outlier_detection_techniques_for_data_quality_evaluation_in_large_studies_An_application_within_the_ONDRI_project)的模型度量敏感度测试。

**性能测试**

![](img/773781fd7dac88e7d2e97c47e3c516a2.png)

由 [Mikail McVerry](https://unsplash.com/@mcverry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/@mcverry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

性能测试是机器学习最标准的测试场景。在某种程度上，这是寻找对[性能指标](/various-ways-to-evaluate-a-machine-learning-models-performance-230449055f15)敏感度的所有其他测试的先决条件。但是，特定的指标对于特定的训练群组、测试或验证数据集的模型设计或阈值限制可能很重要。因此，在测试用例设计中考虑这样的选择可以提高测试质量。

**标签质量敏感度测试**

![](img/fe40d0d62b95d131068cb316c3b70a5c.png)

照片由[波普&斑马](https://unsplash.com/@popnzebra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[挡泥板](https://unsplash.com/@popnzebra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上拍摄

有六种标签数据问题值得测试。在测试模型的标签数据敏感性时，标签数据可以是人工精选的，也可以是像 [Snokel](https://github.com/snorkel-team/snorkel) 这样的工具可以为模型测试用例生成合成的训练数据。在这两种情况下，测试都在寻找对每一类标签数据问题的关键模型度量灵敏度。由于标签数据管理和潜在合成数据生成的复杂性，标签质量测试可能是一组更复杂的测试活动。

**道德和监管测试**

![](img/ea1c9cda74e169f7e635d544b9e648e0.png)

照片由[廷杰伤害律师事务所](https://unsplash.com/@tingeyinjurylawfirm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/tingey-injury-law-firm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

设计测试案例以最小化意外后果，包括[违反](https://www.corporatecomplianceinsights.com/validating-aml-models-nydfs-part-504-aml-requirement/)监管规则[或减少](https://www.mayerbrown.com/en/perspectives-events/publications/2019/04/model-risk-strikes-again-sec-imposes-3-million-fine-due-to-error-in-computer-model)[道德顾虑](https://www.nature.com/articles/s41599-020-0501-9)，变得越来越重要。适用于您的型号的管理域可能会有所不同。然而，独立于此，对有目的偏差的训练/测试数据的敏感性将有助于确定一个版本的模型如何以及在哪里引入或多或少的偏差。

**一致性测试**

![](img/f1155f1b03847ac9541bd54367d6e2cc.png)

照片由[伯纳德·赫曼特](https://unsplash.com/@bernardhermant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/@bernardhermant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

借用一个比喻，被锤子改造过的钉子依然是钉子。这同样适用于流经模型管道的数据。然而，数据[不变量](https://en.wikipedia.org/wiki/Invariant-based_programming)经常被违反。软件工程师测试这些。

对于机器学习，还有第二个一致性方面。钉子的功能需要保持一致。如果一个模型的一个因素或参数在最后一个模型版本中导致了一个特定的(可测量的)行为，那么这个行为应该是相同的，除非是有意的改变。事实上，因素如何变化的来源有些复杂。此外，用于训练和测试的交互和数据也有很大的影响。然而，可能有你的模型的核心信念，你不想因为好的理由而改变。例如，在大多数情况下，你不想违反物理定律，更高的物体应该导致更高的音量。

**超参数拐角情况**

![](img/ab3096d4dd003944629f205ee9bb4531.png)

[沃洛德梅尔·赫里先科](https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

理解模型配置在超参数空间中的位置可能是说明性的，特别是当使用自动[超参数调整方法](https://en.wikipedia.org/wiki/Hyperparameter_optimization)时。[验证拐角情况的模型结果](https://paperswithcode.com/paper/corner-case-generation-and-analysis-for/review/)是明确超参数选择的一个选项。如果模型参数配置非常接近特定的极限情况，可能需要手动重新评估参数设置。优化算法配置可能过于激进，尤其是在自动确定优化算法设置时。

**漂移测试**

![](img/44cbdb4288692ab19296aca7f98b375a.png)

Ralfs Blumbergs 在 [Unsplash](https://unsplash.com/@rblumbergs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

随着世界的发展，机器学习模型也在发展，或者说它们应该发展。虽然[模型监控可能会检测到生产系统中的偏差](/6-strategic-process-considerations-beyond-mlops-fbe260aa099d)，但在部署新模型版本时可能会有更多偏差。针对多个时间切片数据集运行新模型，以估计新模型版本中固有的漂移，这表明了未来的漂移预期。引入更大的漂移会影响预定的[模型重新训练和更新频率](/the-what-why-and-how-of-model-drift-38c2af0e97ee)，并且是模型运行的重要输入。

**总结**

建立健全的 MLOps 已经是一项复杂的工作；增加测试并不会使它变得更容易。对于任何机器学习工作来说，关键的方面是定义一个清晰和可测量的目标，包括量化目标是否达到以及达到何种程度的模型度量。

[回报通常值得投入的努力](https://www.infoq.com/presentations/tdd-ml/)，不仅因为它有助于防止破坏性事件管理工作。作为测试工作的一部分而收集的数据允许更可控的模型设计。

然而，测试数据在本质上是双重用途的。它们对于审计和监管讨论至关重要，并有助于营销沟通，以展示特定的产品质量属性。这些结果远比作为通信设备的行业基准更能说明问题。尤其是当机器学习产品的目标是技术购买中心时，这样的数据点是演示或社交媒体内容中的绝佳话题。邀请潜在客户检查测试策略可以建立对产品的信心。如果没有严格的测试，这个机会会被完全忽略。

机器智能考试可能会省去一次感恩节聚会，并有可能改善你产品的营销。使用测试数据结果对产品进行更加客观的交流，为 MLOps 和产品管理提供了必要的联系。

**延伸阅读**

1.  [测试基于机器学习的系统:系统映射](https://link.springer.com/article/10.1007/s10664-020-09881-0)
2.  [测试驱动的机器学习](https://www.oreilly.com/library/view/thoughtful-machine-learning/9781449374075/ch01.html)
3.  [机器学习系统的有效测试](https://www.jeremyjordan.me/testing-ml/)
4.  [Efemarai 机器学习模型测试平台](/why-dont-we-test-machine-learning-as-we-test-software-43f5720903d)
5.  [在生产中维护机器学习的实用指南](https://eugeneyan.com/writing/practical-guide-to-maintaining-machine-learning/)