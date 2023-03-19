# 有效地筛选分类模型

> 原文：<https://towardsdatascience.com/efficiently-shortlisting-a-classification-model-32e5df0faf3d?source=collection_archive---------29----------------------->

## **以更快的方式收敛到正确的模型**

![](img/775ce8112ee278b34fe993128fb1534a.png)

由 [Djim Loic](https://unsplash.com/@loic?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

现在是晚上 8 点，您仍然在清理数据，执行 EDA，并创建更多的功能。您与业务部门的初步讨论是明天的第一件事，期望讨论正在考虑的潜在“分类”方法，以及所考虑的模型的关键特征。作为一名数据科学家，您希望尝试尽可能多的方法，并为您的业务会议获得最佳的模型预测。

但是，和往常一样，时间紧迫！！

如今，数据科学家(DS)有节约成本/增加收入的目标，工作压力巨大。由于数据清理、争论和特性工程消耗了大部分时间，DS 从来没有足够的时间在数据上尝试所有相关的方法。

尽管业内的一些专家确实知道哪种分类算法适用于特定类型的数据，但是通常很难做出选择。要做出明智的决策，需要考虑太多因素，如数据线性、模型训练时间、数据类型(分类与数值)、可解释性、持续监控的难易程度，当然还有准确性。

确实有必要加快模型的初步筛选，列出入围的前“n”个模型，然后对它们进行微调，以提高准确性和其他关键指标。

要列出一些方法，可以一次运行一个模型，并比较各种输出指标。然而，这是低效和费时的，当然有有效的方法。

此后，我们讨论三个关键有效的方法来执行这一初步入围。每种方法都有其优点和缺点，并且取决于一个人所处的情况——从这些方法中选择一种是值得的。

**1。****lazy predict:**lazy predict 是一个 python 包，封装了 30 个分类模型和 40 个回归模型。它自动化了训练管道，并返回与模型输出相对应的各种度量。在度量的比较之后，可以微调入围模型的超参数，以改进输出度量。

训练的一些关键分类模型是 SVC、AdaBoost、逻辑回归、树外分类器、随机森林、高斯朴素贝叶斯、XG Boost、岭分类器等。

**Python 代码:**

#假设已经定义了 X_train、X_test、y_train、y_test，并且已经执行了 EDA。

安装 lazypredict

来自 lazypredict。受监督的进口懒汉

cls = lazy classifier(ignore _ warnings = False，custom_metric=None)
模型，预测= cls.fit(X_train，X_test，y_train，y_test)

**2。** **模型列表迭代:**虽然这是一种正统的必不可少的方法，但是，这种方法提供了选择评分方法、创建各种度量(如混淆矩阵等)的灵活性。)并执行交叉验证。这里的想法是创建一个要执行的模型列表，并在创建各种模型度量时遍历模型列表。

**Python 代码:**

从熊猫导入 read_csv

从 matplotlib 导入 pyplot

从 sklearn.neighbors 导入 KNeighborsClassifier

从 sklearn.tree 导入决策树分类器

从 sklearn.model_selection 导入 KFold

来自 sklearn.linear_model 导入逻辑回归

从 sklearn.discriminant _ analysis 导入线性判别分析

从 sklearn.svm 导入 SVC

从 sklearn.model_selection 导入 cross_val_score

data frame = read _ CSV(' classification data . CSV ')

数组 _a1 = dataframe.values

X = array_a1 [:，0:20]

Y =数组[:，20]

#创建模型列表，并追加各种模型进行测试。

models_list = []

models_list.append(('LogitReg '，LogisticRegression()))

models_list.append(('SVM '，SVC()))

models _ list . append((' KNN _ 算法'，KNeighborsClassifier()))

models_list.append(('LDA_Algo '，LinearDiscriminantAnalysis()))

models_list.append(('CART_DT '，DecisionTreeClassifier()))

模型结果= []

模型名称= []

#这可能是 f1、roc 等。取决于问题陈述

scoring_type = '准确性'

#遍历模型列表，使用交叉验证分数找到最佳模型。

对于伪名称，模型列表中的模型名称:

kfold = KFold(n_splits=10，random_state=7)

cross val _ results = cross _ val _ score(model，X，Y，cv=kfold，scoring= scoring_type)

model _ results . append(crossval _ results)

模型名称.追加(假名)

print (name，cv_results.mean()，cv_results.std())

3. **AutoML 功能:**在所有解决方案中，这是最有效、最受欢迎的解决方案。有各种各样的 python 包支持 AutoML。一些关键的是-自动 sklearn，自动 Keras，TPOT 等。请注意，这些软件包不仅关注模型过滤的自动化，还加速了数据处理、特征工程等。

**结论:** LazyPredict、模型迭代和 AutoML 是在入围和微调模型之前训练模型列表的各种方法。随着数据科学以前所未有的速度发展，我确信有许多其他方法可以更快地获得更好的模型。(如果您遇到任何问题，请在评论中分享)

然而，随时准备好选项也无妨——您永远不知道与业务部门的会议安排在什么时候。

*免责声明:本文中表达的观点是作者以个人身份发表的意见，而非其雇主的意见。*