# 如何衡量数据质量？

> 原文：<https://towardsdatascience.com/how-can-i-measure-data-quality-9d31acfeb969?source=collection_archive---------10----------------------->

## YData Quality 简介:一个用于全面数据质量的开源包。

![](img/3de2a5058512b8abb08cb60a52fda691.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 用几行代码按优先级标记所有数据质量问题

> *“每个人都想做模型工作，而不是数据工作”——*[*谷歌研究*](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/0d556e45afc54afeb2eb6b51a9bc1827b9961ff4.pdf)

根据 Alation 的数据文化状态[报告](https://venturebeat.com/2021/03/24/employees-attribute-ai-project-failure-to-poor-data-quality/)，87%的员工将糟糕的数据质量归因于为什么大多数组织未能有意义地采用 AI。根据麦肯锡 2020 年[的一项研究](https://www.mckinsey.com/business-functions/mckinsey-digital/our-insights/designing-data-governance-that-delivers-value)，高质量的数据对于推动组织超越竞争对手的数字化转型至关重要。

根据 2020 年 YData [的一项研究](https://medium.com/ydata-ai/what-we-have-learned-from-talking-with-100-data-scientists-d65ed2475f4b)，数据科学家面临的最大问题是无法获得高质量的数据。快速发展的人工智能行业的根本问题是显而易见的，现在人工智能最稀缺的资源是大规模的高质量数据。

尽管认识到了这一点，业界多年来一直专注于改进[模型、库和框架。数据科学家通常认为建模是令人兴奋的工作，而数据清理是乏味的任务。被忽略的数据问题会在整个机器学习解决方案开发过程中造成不利的下游影响。](https://www.stateof.ai/)

令人欣慰的是，我们已经看到了由吴恩达开创的范式转变，从以模型为中心的方法转变为以数据为中心的方法。我们正在见证[以数据为中心的竞争](https://https-deeplearning-ai.github.io/data-centric-comp/)，社区意识。我们正朝着正确的方向前进。

然而，问题依然存在；仍然缺乏行业就绪的工具来理解底层数据质量问题并加以改进。

# 我们对提高数据质量的执着

![](img/f3f0b7f6617afea5a6de425d1f21829c.png)

作者创作—原创图片由[故事](https://www.freepik.com/vectors/website)提供

正如你已经知道的，我们痴迷于解决人工智能行业的这个紧迫的数据问题。首先，我们开源了我们的[合成数据引擎](https://github.com/ydataai/ydata-synthetic)，并围绕它建立了一个[社区](https://syntheticdata.community/)。合成数据可以帮助创建高质量的数据，但现有的真实世界的杂乱数据会怎么样？

如果我们可以分析数据的标准质量问题，并提前按优先级标记它们，从而为数据科学家节省宝贵的时间，会怎么样？这就是我们今天想要回答的问题。

**今天，我们很高兴地宣布**[**YData Quality**](https://github.com/ydataai/ydata-quality)**，这是一个开源的 python 库，用于评估数据管道开发的多个阶段的数据质量。**

# 核心特征:里面是什么？

这个包非常方便，尤其是在开发的初始阶段，此时您仍然在掌握可用数据的价值。我们只能通过从多个维度查看数据来获取数据的整体视图。YData Quality 对其进行模块化评估——每个维度的特定模块，最终打包到一个数据质量引擎中。

质量引擎对输入数据执行多项测试，并根据数据质量发出警告。警告不仅包含检测到的问题的详细信息，还包含基于预期影响的优先级。

以下是 YData Quality 中核心模块的快速概述:

*   **偏见和公平:**保证数据没有偏见，其应用在敏感属性方面是公平的，法律和道德上有义务不区别对待(如性别、种族)。
*   **数据预期:**断言特定属性的数据的单元测试。利用[远大期望](https://greatexpectations.io/)验证，将其结果整合到我们的框架中，并检查验证的质量。
*   **数据关系:**检查要素之间的关联，测试因果关系，估计要素重要性，以及检测具有高度共线性的要素。
*   **漂移分析:**通常，随着时间的推移，数据可能演变出不同的模式。使用此模块，您可以在查看不同的数据块时检查要素(即协变量)和目标(即标注)的稳定性。
*   **重复:**数据可能来自不同的来源，并不总是唯一的。该模块检查数据中的重复条目，这些重复条目是冗余的，并且可以(或者应该)被丢弃。
*   **标签:**通过分类和数字目标的专用引擎，该模块提供了一个测试套件，可检测常见(如不平衡标签)和复杂分析(如标签异常值)。
*   **缺失:**缺失值会在数据应用程序中导致多种问题。通过本模块，您可以更好地了解其影响的严重性以及它们是如何发生的。
*   **错误数据:**数据可能包含没有内在含义的值。使用此模块，您可以检查数据(表格和时间序列)中数据点上的典型错误值。

# 我如何开始？

这是我们都喜欢的部分。启动您的终端并键入以下内容:

```
pip install ydata-quality
```

您可以在一个命令中安装所有的数据质量引擎。为了让您了解各种库的用法，我们提供了多个教程，以 jupyter 笔记本的形式呈现。

让我们向您展示一下我们的数据质量引擎是如何工作的:

下面是一个示例报告的样子:

从上面的输出中，我们注意到重复的列是一个高优先级的问题，需要进一步检查。对此，我们使用`get_warnings()`方法。

只需输入以下内容:

```
dq.get_warnings(test="Duplicate Columns")
```

我们可以看到针对我们想要解决的问题的详细输出:

```
[QualityWarning(category='Duplicates', test='Duplicate Columns', description='Found 1 columns with exactly the same feature values as other columns.', priority=<Priority.P1: 1>, data={'workclass': ['workclass2']})]
```

根据评估，我们可以看到列`workclass`和`workclass2`完全是重复的，这会对下游产生严重的后果。我们必须删除重复的列，并根据其优先级转移到下一个确定的问题。

我们看到了该软件包如何通过提前标记细节来帮助数据科学家解决数据质量问题。此外，我们建议从本教程笔记本[开始，本教程笔记本](https://github.com/ydataai/ydata-quality/blob/master/tutorials/main.ipynb)评估混乱的数据集的数据质量问题并修复它们。

有什么问题吗？加入我们专门的 [**社区 Slack**](http://slack.ydata.ai/) 空间，问走一切。我们是一群友好的人，希望互相学习，并在这个过程中成长。

用改进的数据加速人工智能是我们工作的核心，这个开源项目是我们朝着有意义的旅程迈出的又一步。我们[邀请](http://slack.ydata.ai/)你成为它的一部分——共同创造无限可能。

[*法比亚娜*](https://www.linkedin.com/in/fabiana-clemente/) *是 CDO*[*y data*](https://ydata.ai/?utm_source=medium&utm_medium=signature&utm_campaign=blog)*。*

**用改进的数据加速 AI。**

[*YData 为数据科学团队提供首个数据开发平台。*](https://ydata.ai/?utm_source=medium&utm_medium=signature&utm_campaign=blog)