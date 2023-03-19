# 二元多维背包问题(MKP)

> 原文：<https://towardsdatascience.com/the-binary-multidimensional-knapsack-problem-mkp-2559745f5fde?source=collection_archive---------15----------------------->

## 概述、基准和代码

有许多文章将背包问题作为整数规划问题和解释动态规划的简单例子来讨论。但是多维背包问题的内容还不够多。在这篇文章中，我将讨论**多维背包问题“MKP”**，指出我们可以在哪里找到基准实例，提供我使用的代码文件来读取这些实例(用 Python)，然后继续讨论如何在 Python 上建模 MKP 实例，并用 IBM CPLEX 解决它。本文旨在为感兴趣的优化爱好者和年轻的从业者提供一个关于 MKP 的简单介绍和一个关于如何编码的简单教程。

![](img/321200cb0c12cbc35e95004b37f0911d.png)

由[卢卡斯·罗伯逊](https://unsplash.com/@sheetstothewind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/backpacks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# **多维背包问题‘MKP’**

MKP 是标准二进制背包选择问题的 NP-hard 扩展。目标是一样的；然而，为了找到使总利润/收益最大化的物品子集(目标函数)，不同之处在于，不是具有单个背包或资源，而是具有多个背包/资源(每个都是单独的约束)，并且物品子集不应该违反这些背包中的任何一个的容量。对各种版本的背包问题感兴趣的热心读者可以参考 Kellerer、Pferschy 和 Pisinger 的书 [***背包问题***](https://www.springer.com/gp/book/9783540402862) 了解更多细节。这里我们将只把我们的范围限制在二进制 MKP。

MKP 的数学公式是:

> **(MKP)最大化𝒛 = ∑ 𝒄.𝒙**
> 
> **主题:**
> 
> A.𝒙 ≤ 𝒃，∀ i = 1，…，n
> 
> 𝒙∈{𝟎,𝟏**},∀𝒋=**𝟏**，…,𝒎**

其中 ***c*** 为每项利润向量， ***b*** 为右侧向量或每个背包的容量， ***x*** 为表示某项是否被选中的二元变量向量， ***A*** 为约束的系数矩阵。变量的数量是 **m** 而约束的数量是 **n** 。

# **基准实例**

文献中广泛使用的基准实例可以在在线[或图书馆](http://people.brunel.ac.uk/~mastjjb/jeb/orlib/mknapinfo.html)找到，其中有关于每个基准集的格式和内容的完整解释。这些实例要么是从文献中收集的测试问题，要么是在[楚&Beasley(1998)](https://link.springer.com/article/10.1023/A:1009642405419)【1】中解决的测试实例。基准测试集在变量(列)和约束(行)的数量上有所不同，最难的基准测试集包含 30 个实例，每个实例包含 500 列和 30 行(在本文中，列和变量可以互换使用，行和约束也是如此)。

## **读取基准集(代码和测试文件)**

在线[或-Library](http://people.brunel.ac.uk/~mastjjb/jeb/orlib/mknapinfo.html) 中的基准集是文本文件，包含:

*   每组中测试实例的数量，
*   每个实例的大小(即列数和行数)，
*   目标函数系数(利润)、
*   约束系数(来自每个背包/资源的每个变量的资源消耗)以及，
*   右手边(每个背包的容量)。

为了简化，我们将考虑一个只包含一个实例的数据的文本文件。 [*在这个 GitHub 存储库*](https://github.com/AghaMS/Multidimensional_Knapsack_Problem_Modelling) 中，您可以找到文本文件，每个文件都包含与单个实例相关的数据，此外还有一个函数，它读取文本文件，填充实例并准备数学模型(c、A 和 b)中使用的输入。

# 为 MKP 实例建模

有多种线性规划开源或商业解算器可用于建模优化问题。这篇信息丰富的帖子是学习如何使用一些解算器的一个很好的开始。

> [**Python 中的优化建模:PuLP、Gurobi 和 CPLEX**](https://medium.com/opex-analytics/optimization-modeling-in-python-pulp-gurobi-and-cplex-83a62129807a)

我将使用 IBM CPLEX，因为我有更多使用它的经验。CPLEX 应该提前安装，并有一个 [**免费版**](https://www.ibm.com/uk-en/products/ilog-cplex-optimization-studio) 供学生和学者使用。

我们从导入相关库开始:

```
import cplex
from docplex.mp.model import Model
```

我们还将导入包含读取和填充 MKP 实例的函数的文件(参考上一节)。

```
# Import the reading function 
import MKP_populate_function as rdmkp
```

接下来，我们将调用实例上的函数，并获取用于创建模型的参数:

```
# Call the function on a given instance
instance = 'mknapcb1_1.txt'
c, A, b = rdmkp.MKPpopulate(instance)# Define the ranges for variables and constraints
nCols, nRows = range(len(c)), range(len(b))
```

我通常使用前面代码块中的最后一个附加步骤来定义变量和约束的数量范围。它们主要表示变量和约束的集合。当我们在数学模型中定义变量和约束时，这将证明是很方便的。

## 创建模型

由于我们已经准备好了参数，我们将继续创建一个空模型:

```
# Create an empty model 
mkp = Model('MKP')
```

## 声明决策变量

现在，我们将通过定义决策变量来扩充空模型。在 MKP，变量是二进制的，所以我们会相应地声明它们。您会注意到，我添加了二元变量不需要的上下界，但是，有时我想检查线性松弛界，所以我在所有模型中保持定义的界，但将二元类型更改为连续。

```
# Define decision variables      
x = mkp.binary_var_list(nCols, lb = 0, ub = 1, name = 'x')
```

## 定义约束

每个约束代表一个有自己容量的不同背包。这个背包中每个物品的消耗由*矩阵中的元素给出。*

```
*# Declare constraints
constraints = mkp.add_constraints(sum(A[i][j] * x[j] for j in nCols) <= b[i] for i in nRows)*
```

## *定义目标函数*

*下一步是创建目标函数，即最大化所选商品子集的利润总和。将目标函数添加为 KPI 是一种很好的做法，可以更好地报告结果。我从更有经验的建模师那里学到了这一点。*

```
*# Declare the objective function
profit = mkp.sum(c[j] * x[j] for j in nCols)# Add Obj. Function as a kpi for better reporting of results     
mkp.add_kpi(profit, 'profit')

# Add objective function to the model as a maximization type
obj = mkp.maximize(profit)*
```

## *求解模型*

*现在剩下的就是解决创建的模型并报告结果。*

```
*# Solving the model
mkp.solve()# Reporting results
mkp.report()*
```

## *一个额外的有用命令*

*当调用以下方法时，该方法提供关于变量的数量和它们的类型、约束的数量和问题的类型以及目标函数的意义(最大化或最小化)的信息。*

```
*mkp.print_information()*
```

## *最后一点*

*需要注意的是，CPLEX 找到了 MKP 的精确解。虽然它在解决小实例时看起来很快，但在解决较大规模的整数/二进制实例时需要更长的时间。*

> *正如 J. E. Beasly 教授所言:"当问题很小的时候，是什么使解决问题变得容易，而当问题变大的时候，恰恰是什么使它变得非常困难"。*

*这仅仅是因为当问题规模相对较小时，我们可以列举所有可能的解决方案，检查它们的可行性，并找到最佳方案。然而，当问题规模增大时，解决方案的数量会增长得更快，我们无法在可行的时间内检查所有的解决方案。可以使用其他方法，如启发式/元启发式，但也许我们可以在另一篇文章中讨论它们。*

*如果你喜欢这个内容，你可能会对这个[帖子](https://blog.satalia.com/measuring-team-disruption-b16979a6038d?source=your_stories_page-------------------------------------)感兴趣，这个帖子是关于一个非常有趣的网络优化应用，我们使用社交网络分析来衡量和最小化对团队的干扰。*

## *参考资料:*

*[1]朱立群，比斯利，[多维背包问题的遗传算法](https://scholar.google.co.uk/citations?view_op=view_citation&hl=en&user=qM9xh_cAAAAJ&citation_for_view=qM9xh_cAAAAJ:d1gkVwhDpl0C) (1998)，启发式学报 4 (1)，63–86*