# 20 烧 XGBoost 常见问题解答像专业人士一样使用库

> 原文：<https://towardsdatascience.com/20-burning-xgboost-faqs-answered-to-use-the-library-like-a-pro-f8013b8df3e4?source=collection_archive---------8----------------------->

## 梯度-通过学习这些重要的课程来提升你的 XGBoost 知识

![](img/124cb345f0532c8260da6d99b95fcd9c.png)

**照片由**[**hai them Ferdi**](https://unsplash.com/@haithemfrd_off?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)**上** [**Unsplash。**](https://unsplash.com/s/photos/boost?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) **除特别注明外，所有图片均为作者所有。**

XGBoost 是一个真正的野兽。

这是一个基于树的动力之马，是许多表格竞赛和数据马拉松获胜解决方案的幕后推手。目前，它是世界上“最性感”工作的“最热门”ML 框架。

虽然使用 XGBoost 进行基本建模可能很简单，但是您需要掌握细节才能获得最佳性能。

也就是说，我向你展示这篇文章，它是

*   数小时阅读文档(这并不有趣)
*   哭着通过一些可怕但有用的 Kaggle 内核
*   数百个谷歌关键词搜索
*   通过阅读大量文章，我完全耗尽了我的中等会员资格

这篇文章回答了关于 XGBoost 及其 API 的 20 个最棘手的问题。这些应该足够让你看起来像是一直在用库了。

<https://ibexorigin.medium.com/membership>  

获得由强大的 AI-Alpha 信号选择和总结的最佳和最新的 ML 和 AI 论文:

<https://alphasignal.ai/?referrer=Bex>  

# 1.应该选择哪个 API——Scikit-learn 还是核心学习 API？

Python 中的 XGBoost 有两个 API——Scikit-learn 兼容的(估计器有熟悉的`fit/predict`模式)和核心的 XGBoost-native API(用于分类和回归的全局`train`函数)。

Python 社区的大多数人，包括 Kagglers 和我自己，都使用 Scikit-learn API。

这个 API 使您能够将 XGBoost 估算器集成到您熟悉的工作流中。好处包括(但不限于)

*   能够将核心 XGB 算法传递到 [Sklearn 管道](/how-to-use-sklearn-pipelines-for-ridiculously-neat-code-a61ab66ca90d?source=your_stories_page-------------------------------------)
*   使用更高效的交叉验证工作流程
*   避免学习新 API 带来的麻烦，等等。

# 2.我如何完全控制 XGBoost 中的随机性？

> 其余对 XGBoost 算法的引用主要是指与 Sklearn 兼容的 XGBRegressor 和 XGBClassifier(或类似的)估计器。

估计器有参数`random_state`(替代的参数`seed`已经被弃用，但仍然有效)。然而，使用默认参数运行 XGBoost 将会产生相同的结果，即使种子不同。

这是因为 XGBoost 只有在使用了`subsample`或任何其他以`colsample_by*`前缀开头的参数时才会产生随机性。顾名思义，这些参数与[随机抽样](/why-bootstrap-sampling-is-the-badass-tool-of-probabilistic-thinking-5d8c7343fb67?source=your_stories_page-------------------------------------)有很大关系。

# 3.XGBoost 中的目标是什么，如何为不同的任务指定它们？

回归和分类任务有不同的类型。它们根据目标函数、它们可以处理的分布以及它们的损失函数而变化。

您可以使用`objective`参数在这些实现之间切换。它接受 XGBoost 提供的特殊代码字符串。

回归目标以`reg:`为前缀，而分类以`binary:`或`multi:`开始。

我将让您从本文档的第[页](https://xgboost.readthedocs.io/en/latest/parameter.html#learning-task-parameters)中探索完整的目标列表，因为有很多目标。

此外，指定正确的目标和度量可以消除在装配 XGB 分类器时出现的令人难以置信的恼人的警告。

# 4.XGBoost 中应该用哪个 booster——GB linear，gbtree，dart？

> XGBoost 有 3 种类型的梯度增强学习器，它们是梯度增强(GB)线性函数、GB 树和 DART 树。您可以使用`booster`参数切换学习者。

如果你问 Kagglers，他们会在任何一天选择提升树而不是线性函数(我也是)。原因是树可以捕捉非线性的、复杂的关系，而线性函数不能。

所以，唯一的问题是您应该将哪个树助推器传递给`booster`参数- `gbtree`还是`dart`？

我不会用这里的全部差异来烦你。你应该知道的是，XGBoost 在与`gbtree` booster 一起使用时，使用了一组基于决策树的模型。

DART 树是一种改进(有待验证),它引入了决策树子集的随机丢弃，以防止过度拟合。

在我用默认参数`gbtree`和`dart`做的几个小实验中，当我把`rate_drop`设置在 0.1 和 0.3 之间时，我用`dart`得到了稍微好一点的分数。

有关更多详细信息，我建议您参考 XGB 文档的[本页](https://xgboost.readthedocs.io/en/latest/tutorials/dart.html)，以了解细微差别和附加的超参数。

# 5.在 XGBoost 中应该使用哪种树方法？

有 5 种控制树结构的算法。如果你在做分布式培训，你应该通过`hist`到`tree_method`。

对于其他场景，默认的(也是推荐的)是`auto`，对于中小型数据集从`exact`变为大型数据集的`approx.`。

# 6.XGBoost 中的助推轮是什么？

正如我们所说的，XGBoost 是梯度增强决策树的集合。集合中的每棵树被称为基础或弱学习器。弱学习者是任何比随机猜测表现稍好的算法。

通过组合多个弱学习者的预测，XGBoost 产生了最终的、健壮的预测(现在跳过了很多细节)。

> 每次我们用一棵树来拟合数据，就叫做一轮提升。

因此，要指定要构建的树的数量，请将一个整数传递给学习 API 的`num_boost_round`或 Sklearn API 的`n_estimators`。

通常，太少的树会导致拟合不足，太多的树会导致拟合过度。您通常会使用超参数优化来调整该参数。

# 7.XGBoost 中的`early_stopping_rounds`是什么？

从一轮提升到下一轮，XGBoost 建立在最后一棵树的预测之上。

如果预测在一系列回合后没有改善，那么停止训练是明智的，即使我们没有在`num_boost_round`或`n_estimators`中硬停下来。

为了实现这一点，XGBoost 提供了`early_stopping_rounds`参数。例如，将其设置为 50 意味着如果预测在最后 50 轮中没有改善，我们就停止训练。

为`n_estimators`设置一个较大的数值，并相应地改变提前停止以获得更好的结果，这是一个很好的做法。

在我展示如何用代码实现它的例子之前，还有两个其他的 XGBoost 参数要讨论。

# 8.XGBoost 中的`eval_set`有哪些？

只有当您将一组评估数据传递给`fit`方法时，才能启用提前停止。这些评估集用于跟踪从一轮到下一轮的整体表现。

在每一轮通过的训练集上训练一棵树，并且为了查看分数是否一直在提高，它对通过的评估集进行预测。下面是它在代码中的样子:

> 将`verbose`设置为 False 以删除日志消息。

第 14 次迭代后，分数开始下降。所以训练在第 19 次迭代时停止，因为应用了 5 轮早期停止。

也可以将多个评估集作为一个元组传递给`eval_set`，但是当与早期停止一起使用时，只有最后一对会被使用。

> 查看[这篇文章](https://machinelearningmastery.com/avoid-overfitting-by-early-stopping-with-xgboost-in-python/)以了解更多关于提前停止和评估集的信息。

# 9.评估指标什么时候影响 XGBoost？

您可以使用`fit`方法的`eval_metric`指定各种评估指标。通过的指标只在内部产生影响——例如，它们用于评估早期停止期间的预测质量。

您应该根据您选择的目标来更改指标。您可以在文档的[本页](https://xgboost.readthedocs.io/en/latest/parameter.html#learning-task-parameters)中找到目标及其支持指标的完整列表。

以下是一个 XGBoost 分类器的示例，以多类对数损失和 ROC AUC 作为度量:

无论您传递给`eval_metric`什么度量，它只影响`fit`函数。所以，当你在分类器上调用`score()`时，它仍然会产生*精度*，这是 Sklearn 中的默认。

# 10.XGBoost 中的学习率(eta)是多少？

每次 XGBoost 添加一个新的树到集成中，它被用来校正最后一组树的残差。

问题是这种方法快速而强大，使得算法能够快速学习和过度拟合训练数据。因此，XGBoost 或任何其他梯度增强算法都有一个`learning_rate`参数来控制拟合的速度，并防止过度拟合。

`learning_rate`的典型值范围从 0.1 到 0.3，但也可能超过这些值，特别是接近 0。

无论传递给`learning_rate`的值是什么，它都作为新树所做修正的加权因子。因此，较低的学习率意味着我们不太重视新树的修正，从而避免过度拟合。

一个好的实践是为`learning_rate`设置一个较低的数字，并使用具有较大数量估计器的早期停止(`n_estimators`):

您将立即看到慢速`learning_rate`的效果，因为早期停止将在训练期间的更晚时间应用(在上述情况下，在第 430 次迭代之后)。

但是，每个数据集都是不同的，所以需要用超参数优化来调优这个参数。

> 查看[这篇关于如何调整学习率的文章](https://machinelearningmastery.com/tune-learning-rate-for-gradient-boosting-with-xgboost-in-python/)。

# 11.应该让 XGBoost 处理缺失值吗？

为此，我将给出我从两位不同的围棋比赛大师那里得到的建议。

1.  如果您将`np.nan`赋予基于树的模型，那么，在每个节点分裂时，丢失的值要么被发送到节点的左边子节点，要么被发送到节点的右边子节点，这取决于哪一个是最好的。因此，在每次分割时，缺失值会得到特殊处理，这可能会导致过度拟合。一个对树非常有效的简单解决方案是用不同于其他示例的值填充空值，比如-999。
2.  尽管像 XGBoost 和 LightGBM 这样的包可以不经过预处理就处理空值，但是开发自己的插补策略总是一个好主意。

对于真实世界的数据集，您应该始终调查缺失的类型(MCAR、马尔、马尔)，并选择插补策略(基于值的[平均值、中值、众数]或基于模型的[KNN 插补器或基于树的插补器])。

如果您不熟悉这些术语，我在这里为您介绍了。

# 12.用 XGBoost 进行交叉验证的最好方法是什么？

尽管 XGBoost 带有内置的 CV 支持，但请始终使用 Sklearn CV 拆分器。

我说的 Sklearn，并不是指`cross_val_score`或者`cross_validate`这样的基础效用函数。

2021 年没有人会交叉验证这种方式(至少在 Kaggle 上没有)。

为 CV 过程提供更多灵活性和控制的方法是使用 Sklearn CV 分离器的`.split`功能，并在`for`循环中实现您自己的 CV 逻辑。

下面是一个五重简历的代码:

在`for`循环中执行 CV 可以使用评估集和提前停止，而像`cross_val_score`这样的简单函数则不能。

# 13.如何在 Sklearn 管道中使用 XGBoost？

如果使用 Sklearn API，可以将 XGBoost 估计器作为管道的最后一步(就像其他 Sklearn 类一样):

如果想在管道中使用 XGBoost 的`fit`参数，可以很容易地将它们传递给管道的`fit`方法。唯一的区别是你应该使用`stepname__parameter`语法:

因为我们在管道中将 XGBoost 步骤命名为`clf`，所以每个`fit`参数都应该以`clf__`为前缀，这样管道才能正常工作。

> 此外，由于`StandardScaler`删除了熊猫数据帧的列名，XGBoost 不断抛出错误，因为`eval_set`和训练数据不匹配。因此，我在两组中都使用了`.values`来避免这种情况。

# 14.如何显著提高默认分数？

在使用默认的 XGBoost 设置建立了基本性能之后，您可以做些什么来显著提高分数呢？

许多人匆忙转向超参数调优，但它并不总是给你想要的分数带来巨大的飞跃。通常，参数优化带来的改进可能微不足道。

在实践中，任何实质性的分数增加主要来自于适当的特征工程和使用模型混合或堆叠等技术。

你应该把大部分时间花在特性工程上——有效的 FE 来自于正确的 EDA 和对数据集的深刻理解。特别是，创建特定于领域的特性可能会对性能产生奇迹。

然后，尝试将多个模型组合成一个整体。在 Kaggle 上运行良好的方法是堆叠三大组件——XGBoost、CatBoost 和 LightGBM。

# 15.XGBoost 中最重要的超参数有哪些？

超参数调整应该始终是项目工作流程的最后一步。

如果时间紧迫，应该优先调优 XGBoost 的超参数来控制过度拟合。这些是:

*   `n_estimators`:要训练的树木数量
*   `learning_rate`:步进收缩或`eta`
*   `max_depth`:每棵树的深度
*   `gamma`:复杂度控制-伪正则化参数
*   `min_child_weight`:控制树深度的另一个参数
*   `reg_alpha` : L1 正则项(如 LASSO 回归)
*   `reg_lambda` : L2 正则项(如在岭回归中)

# 16.如何在 XGBoost 中调优 max_depth？

`max_depth`是树的根节点和叶节点之间的最大长度。它是控制过拟合的最重要的参数之一。

典型值范围是 3-10，但很少需要高于 5-7。此外，使用更深的树会使 XGBoost 非常消耗内存。

# 17.如何在 XGBoost 中调 min_child_weight？

`min_child_weight`控制创建新节点时数据中所有样本的权重之和。当该值较小时，每个节点将在每个节点中分组越来越少的样本。

如果它足够小，树将很可能过度适应训练数据中的特性。因此，请为该参数设置一个较高的值，以避免过度拟合。

默认值为 1，其值仅限于定型数据中的行数。然而，一个很好的调优范围是 2–10 或高达 20。

# 18.如何在 XGBoost 中调 gamma？

一个更有挑战性的参数是`gamma`，对于我这样的外行来说，你可以把它看成是模型的复杂度控制。伽玛越高，应用的正则化越多。

它的范围从 0 到无穷大——因此，调整它可能很困难。此外，它高度依赖于数据集和其他超参数。这意味着一个模型可以有多个最佳伽马值。

通常，您可以在 0–20 之间找到最佳的伽玛。

# 19.XGBoost 中 reg_alpha 和 reg_lambda 如何调优？

这些参数指的是特征权重的正则化强度。换句话说，增加它们将使算法更保守，因为对具有低系数(或权重)的特征不太重视。

`reg_alpha`指 Lasso 回归的 L1 正则化，`reg_lambda`指岭回归。

调整它们可能是一个真正的挑战，因为它们的值也可以从 0 到无穷大。

首先，选择一个较宽的区间，如[1e5，1e2，0.01，10，100]。然后，根据从该范围返回的最佳值，选择其他几个附近的值。

</intro-to-regularization-with-ridge-and-lasso-regression-with-sklearn-edcf4c117b7a>  

# 20.如何调优 XGBoost 中的随机采样超参数？

以上 6 个参数之后，强烈建议调优那些控制随机采样的。事实上，随机采样是算法中应用的另一种方法，以进一步防止过拟合。

*   `subsample`:推荐值【0.5 - 0.9】。每个增强回合中随机抽样(不替换)的所有训练样本的比例。
*   `colsample_by*`:以此前缀开头的参数是指随机选择的列的比例
*   `colsample_bytree`:每一轮助推
*   `colsample_bylevel`:树中达到的每个深度级别
*   `colsample_bynode`:每个节点创建或在每个拆分

像`subsample`，推荐范围是【0.5 - 0.9】。

# 摘要

终于，这篇痛苦漫长但希望有用的文章结束了。

我们已经介绍了很多——如何为任务选择正确的 API、正确的目标和指标、`fit`最重要的参数，以及从不断更新的来源(如 Kaggle)收集的一些有价值的 XGBoost 最佳实践。

如果你对 XGBoost 有更多的问题，请在评论中发表。在 StackExchange 网站上，我会试着比其他人回答得更快(在写这篇文章的时候，我还没有得到这个恩惠😉).

![](img/f91b8a21f12d3c3b7d428e0f8d5f48be.png)

这里有些你可能会感兴趣的东西…

[![](img/735b0439c5891f3beeb750ed5f4fa895.png)](https://towardsdatascience.com/love-3blue1brown-animations-learn-how-to-create-your-own-in-python-in-10-minutes-8e0430cf3a6d)

## 特别感谢这些帖子:

*   [XGBoost 超参数指南](https://www.kaggle.com/prashant111/a-guide-on-xgboost-hyperparameters-tuning/notebook)
*   [掌握 XGBoost](/mastering-xgboost-2eb6bce6bc76)
*   [使用 Python 代码在 XGBoost 中调整参数的完整指南](https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/)