# 5 个用 Python 解决的端到端数据科学项目

> 原文：<https://towardsdatascience.com/5-solved-end-to-end-data-science-projects-in-python-acdc347f36d0?source=collection_archive---------0----------------------->

## 初级和高级 Python 数据科学项目，带源代码。

![](img/d71f41bf493b7c7169097d580746df63.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 拍摄的照片

如果您已经研究数据科学有一段时间了，您可能知道为了学习数据科学，您需要学习数学、统计学和编程。这对任何对数据科学感兴趣的人来说都是一个好的开始，但是您知道如何更多地接触数据科学吗？

是有项目的！一个项目将帮助你把从数学、统计学和编程中学到的所有知识付诸实践。到目前为止，你可能已经单独看到了它们中的每一个，但是在你完成一个项目后，你在每个领域学到的概念将会更有意义。

在本文中，我列出了一些可以用 Python 完成的端到端数据科学项目。项目按难度排列，所以初学者项目在开头，高级项目在文章末尾。

*注意:本文中列出的大多数项目都需要对 Python 有一定的了解。你至少应该知道诸如 Pandas、Numpy 和 Scikit-learn 等库的基础知识。我将留下每个项目的源代码以及每个项目中使用的库的指南。如果你还是 Python 初学者，我推荐你先从* [*基础 Python 项目开始*](/6-python-projects-you-can-finish-in-a-weekend-f53552279cc) *。*

# 首先要做的事情——学习探索性数据分析

您将来要解决的大多数真实项目以及本文中列出的一些项目都需要您执行 EDA(探索性数据分析)。这一步在每个数据科学项目中都是必不可少的，因为它有助于您理解数据，并通过可视化技术获得有用的见解。

EDA 还有助于暴露数据中的意外结果和异常值。例如，直方图、箱线图和条形图等图表将帮助您识别异常值，以便您可以去除它们并执行更好的分析。

![](img/73df1b680266379b3a31f62e23f5a250.png)

照片由 [Myriam Jessier](https://unsplash.com/@mjessier?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我没有把 EDA 作为这个列表中的一个项目，因为它通常不是最终项目的目标，而是为了更好地执行分析而必须做的事情。要了解如何执行 EDA，请查看本[指南](/a-simple-guide-to-beautiful-visualizations-in-python-f564e6b9d392)，它将向您介绍 Python 中的数据可视化。在本指南中，您必须从包含足球运动员统计数据的数据集中获得洞察力。另外，查看另一本[指南](/a-straightforward-guide-to-cleaning-and-preparing-data-in-python-8c82f209ae33)，学习 Python 中数据清理的最佳实践。第二个指南将向您展示如何使用您在第一个指南中学到的图来识别和处理异常值。

# 1.情感分析

这个列表的第一个项目是建立一个预测电影评论情绪的机器学习模型。情感分析是一种 NLP 技术，用于确定数据是积极的、消极的还是中性的。这对企业真的很有帮助，因为它有助于了解客户的总体意见。

对于这个项目，您将使用包含 50k 电影评论的 IMDB 数据集。有两个栏目(评论和感悟)。目标是建立最佳的机器学习模型，预测给定电影评论的情绪。为了使这个项目对初学者友好，你只需要预测一个电影评论是积极的还是消极的。这被称为二进制文本分类，因为只有两种可能的结果。

![](img/18bbad99d2a73409e75ff6bba0b4a11a.png)

由[绝对视觉](https://pixabay.com/users/absolutvision-6158753/)在 [Pixabay](https://pixabay.com/photos/smiley-emoticon-anger-angry-2979107/) 上拍摄的照片

*   图书馆(包括指南):[熊猫](/a-complete-yet-simple-guide-to-move-from-excel-to-python-d664e5683039)， [Scikit-learn](/a-beginners-guide-to-text-classification-with-scikit-learn-632357e16f3a)
*   源代码:[Python 中的情感分析(文本分类)](/a-beginners-guide-to-text-classification-with-scikit-learn-632357e16f3a)

第一个项目的特别之处在于，您将探索 scikit-learn 库，同时从头开始构建一个基本的机器学习模型。

# 检测项目

使用 Python 可以做许多“检测”项目。我将按照难度列出我用 Python 实现的那些，而不是仅仅列举一个。

## 2.假新闻检测

最适合初学者的检测项目大概就是假新闻检测了。网上到处都是假新闻。这在民众中造成了混乱和恐慌。这就是为什么识别信息的真实性很重要。幸运的是，我们可以使用 Python 来处理这个数据科学项目。

![](img/f6f32922078987123466891eebaa00dc.png)

罗马卡夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*   库(包括指南):Scikit learn([tfidf 矢量器](/7-nlp-techniques-you-can-easily-implement-with-python-dc0ade1a53c2#fd70)和 PassiveAggressiveClassifier**)**、[Pandas](/a-complete-yet-simple-guide-to-move-from-excel-to-python-d664e5683039) 和 [Numpy](/numpy-basics-for-people-in-a-hurry-8e05781503f)
*   源代码:[检测假新闻](https://data-flair.training/blogs/advanced-python-project-detecting-fake-news/)

这个项目的目标是从假新闻中分离出真实的新闻。为此，我们将使用 sklearn 的工具，如 TfidfVectorizer 和 PassiveAggressiveClassifier。

## 3.信用卡欺诈检测

如果你想让这类项目更有挑战性，你可以试试信用卡欺诈检测。信用卡欺诈给消费者和公司都造成了数十亿美元的损失，同时欺诈者不断试图寻找新的方法来实施这些非法行为。这就是为什么欺诈检测系统对于银行最大限度地减少损失至关重要。

在此项目中，您应该从包含交易历史的数据集中分析客户的消费行为。位置等变量将帮助您识别欺诈交易。

![](img/b64e104bdc98f376be69f70b2079022e.png)

照片由[rupixen.com](https://unsplash.com/@rupixen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

*   库(包含指南):[熊猫](/a-complete-yet-simple-guide-to-move-from-excel-to-python-d664e5683039)、 [Numpy](/numpy-basics-for-people-in-a-hurry-8e05781503f) 、 [Matplolib](/a-simple-guide-to-beautiful-visualizations-in-python-f564e6b9d392) 、 [Scikit-learn](/a-beginners-guide-to-text-classification-with-scikit-learn-632357e16f3a) 、[机器学习算法](/11-most-common-machine-learning-algorithms-explained-in-a-nutshell-cc6e98df93be) (XGBoost、随机森林、KNN、逻辑回归、SVM、决策树)
*   源代码:[用 Python 进行机器学习的信用卡欺诈检测](https://medium.com/codex/credit-card-fraud-detection-with-machine-learning-in-python-ac7281991d87)

# 4.聊天机器人

聊天机器人只是一个通过语音命令或文本聊天来模拟人类对话的程序。高级聊天机器人是使用人工智能构建的，用于手机上的大多数消息应用程序。

虽然创建像 Siri 和 Alexa 这样的语音助手过于复杂，但我们仍然可以使用 Python 和深度学习创建一个基本的聊天机器人。在这个项目中，你必须使用数据科学技术用数据集训练聊天机器人。随着这些聊天机器人处理更多的交互，它们的智能和准确性将会提高。

![](img/0dd6a8f301576824afb49b9a35aa0412.png)

[奥米德·阿明](https://unsplash.com/@omidarmin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

*   包:Keras，NLTK， [Numpy](/numpy-basics-for-people-in-a-hurry-8e05781503f)
*   源代码:[如何用 Python &深度学习在不到一个小时内创建一个聊天机器人](/how-to-create-a-chatbot-with-python-deep-learning-in-less-than-an-hour-56a063bdfc44)

构建一个简单的聊天机器人会让你接触到各种有用的数据科学和编程技能

# 5.客户流失预测

客户流失率是客户停止与公司做生意的比率。这表示在给定时间段内停止订阅的订阅者的百分比。

这是一个测试您的数据科学技能的好项目。我甚至不得不在黑客马拉松中解决它！

这个项目的主要目标是对客户是否会流失进行分类。为此，您将使用包含银行客户财务数据的数据集。信用评分、任期、产品数量和估计工资等信息将用于构建此预测模型。

*   软件包:[熊猫](/a-complete-yet-simple-guide-to-move-from-excel-to-python-d664e5683039)、 [Matplolib](/a-simple-guide-to-beautiful-visualizations-in-python-f564e6b9d392) 、 [Scikit-learn](/a-beginners-guide-to-text-classification-with-scikit-learn-632357e16f3a) 、[机器学习算法](/11-most-common-machine-learning-algorithms-explained-in-a-nutshell-cc6e98df93be) (XGBoost、随机森林、KNN、逻辑回归、SVM、决策树)
*   源代码:[银行客户流失预测](https://www.kaggle.com/kmalit/bank-customer-churn-prediction)

这个项目和信用卡欺诈检测项目是本文列出的最完整的数据科学项目。它包括探索性数据分析、特征工程、数据准备、模型拟合和模型选择。

*就是这样！希望在完成所有这些项目后，你能更好地理解到目前为止你所学到的关于数据科学的一切。*

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)