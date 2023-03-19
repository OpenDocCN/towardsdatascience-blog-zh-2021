# 数据准备备忘单

> 原文：<https://towardsdatascience.com/data-preparation-cheatsheet-8201e1fcf9cf?source=collection_archive---------32----------------------->

## 通用特征工程/EDA 任务，已编译

![](img/7e795f387759f02462a17acc7555ae41.png)

图片来自 [Unsplash](https://unsplash.com/photos/1K6IQsQbizI)

任何数据分析/科学任务中最耗时的部分就是正确地准备和配置数据。一个**模型的表现只和它所接收到的数据一样好**，而且数据可能需要经过大量的转换才能为模型训练做好准备。这些年来，我编写了一个 [**概念**](/using-notion-to-organize-your-data-science-learning-bbcb500364b6) 页面，突出了数据科学家为**数据准备**需要执行的许多常见任务。我在下面列出了几个例子，但是完整的例子可以在下面的[链接](https://www.notion.so/Common-Use-Cases-583a1537a87d433fb5359bd0c2c99eb9)中找到。随着我继续学习 EDA 或特性工程中反复使用的其他常用函数，我将继续扩展这个链接。

**注意**:所有这些例子都是用 Python 编写的，主要使用了 Pandas、Numpy 和 Sci-Kit 学习库。为了可视化，使用了 MatPlotLib 或 Seaborn。

# 目录

1.  检查数据帧中的缺失值
2.  删除列
3.  将函数应用于列
4.  绘制列的值计数
5.  按列值对数据帧排序
6.  基于列值删除行
7.  顺序编码
8.  用所有分类变量编码数据帧
9.  额外资源

# 检查数据帧中的缺失值

该代码块使用 **Pandas 函数** **isnull()** 和 **sum()** 给出数据集中所有列缺失值的汇总。

# 删除列

要删除一个列，使用 pandas **drop()函数**来删除您选择的列，对于**多个列**只需在包含列名的**列表**中添加它们的名称。

# 将函数应用于列

使用虹膜数据集

许多要素工程任务需要编码或数据转换，这可以通过传统的 Python 函数来完成。通过使用 pandas **apply()** 函数，您可以将您创建的函数应用于整个列，以创建新列或转换您选择的列。

# 绘制列的值计数

条形图

特征工程的一项常见任务是了解数据集的平衡程度。例如，在一个二元分类问题中，如果有将近 90%的一个类和 10%的数据点代表另一个类，这将导致模型在大多数情况下预测第一个类。为了帮助避免这种情况，特别要将你的反应变量的数量可视化。熊猫 **value_counts** ()函数使您能够获得一列中每个值的出现次数，然后 **plot()** 函数让您通过条形图直观地看到这一点。

# 按列值对数据帧排序

有时对于数据分析，您希望以特定的顺序可视化您的列，您可以在数据框架的 **sort_values()** 函数中添加多个列。

# 基于列值删除行

如果您希望根据另一列的值对数据进行子集划分，可以通过捕获一组特定行的**索引**来实现。通过创建这些序列，您可以使用 **drop** 函数来删除您已经确定的这些特定的行/索引。

# 顺序编码

[序号编码](https://machinelearningmastery.com/one-hot-encoding-for-categorical-data/)是对分类数据进行编码的多种方式之一。有各种各样的编码方法，比如一键编码等等，我已经在这里链接了[。**当您想要保留分类变量的顺序，并且如果您的列有一个固有的** **顺序**时，可以使用顺序编码。](https://www.analyticsvidhya.com/blog/2020/08/types-of-categorical-data-encoding/)

# 用所有分类变量编码数据帧

如果数据集只有分类列，您可能需要创建一个管道/函数来编码整个数据集。请注意，在使用此函数之前，您需要**确定您正在处理的每一列的顺序是否重要**。

# 额外资源

*   [熊猫小抄](https://www.datacamp.com/community/blog/python-pandas-cheat-sheet)
*   [Python GroupBy 语句](https://realpython.com/pandas-groupby/)
*   [EDA 备忘单](/data-visualization-cheat-sheet-with-seaborn-and-matplotlib-70cac11c6517)
*   [Python CheatSheet](https://elitedatascience.com/python-cheat-sheet)

— — —

数据争论是为模型训练/反馈准备数据的必要条件。Pandas、Numpy 和 Sci-Kit Learn 等 Python 库有助于在必要时轻松操作和转换数据。随着如此多的新 ML 算法进入该领域，理解如何为将要使用的模型准备数据仍然是必不可少的，无论是传统模型如逻辑回归还是领域如 NLP，数据准备都是必须的。

我希望这些例子中的一些对那些对他们的特定数据集执行任何 EDA 或特征工程的人有用并节省了时间。查看 [**概念链接**](https://www.notion.so/Common-Use-Cases-583a1537a87d433fb5359bd0c2c99eb9) 对于我记录的所有其他例子，这将继续更新。我附上了其他资源和特征工程的备忘单，我发现上面有帮助。请随时在 [Linkedln](https://www.linkedin.com/in/ram-vegiraju-81272b162/) 上与我联系，或者在 [Medium](https://ram-vegiraju.medium.com/) 上关注我，了解我更多的写作内容。分享任何想法或反馈，谢谢阅读！