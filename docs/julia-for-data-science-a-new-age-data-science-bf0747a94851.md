# 朱莉娅:数据科学的新时代

> 原文：<https://towardsdatascience.com/julia-for-data-science-a-new-age-data-science-bf0747a94851?source=collection_archive---------13----------------------->

## 了解为什么 Julia 是数据科学的未来，并编写您的第一个 Julia 代码

![](img/2d6a358878fc961f1cd853869b8924d9.png)

封面照片由[**Starline | Freepik**](https://www.freepik.com/starline)

# 介绍

Julia 是一种高级通用语言，可用于编写执行速度快、易于实现的科学计算代码。该语言旨在满足科学研究人员和数据科学家优化实验和设计实现的所有需求。[茱莉亚(编程语言)](https://en.wikipedia.org/wiki/Julia_(programming_language))。

> *“Julia 是为科学计算、机器学习、数据挖掘、大规模线性代数、分布式和并行计算而构建的”——开发者背后*[*the Julia language*](https://julialang.org/)*。*

Python 在数据科学爱好者中仍然很有名，因为他们获得了一个具有加载库的生态系统来完成数据科学的工作，但 Python 不够快或方便，并且它具有安全性可变性，因为大多数库是从 JavaScript、Java、C 和 C++等其他语言构建的。快速的执行和方便的开发使它在数据科学社区中非常有吸引力，并且大多数库都是直接用 Julia 编写的，以提供额外的安全层。信息世界。

> *根据* [*杰瑞米·霍华德*](https://en.wikipedia.org/wiki/Jeremy_Howard_(entrepreneur))T22【Python 不是机器学习的未来】。

在他最近的[视频](https://www.youtube.com/watch?v=4I1ejhQqD4c)中，他谈到 Python 在运行机器学习模型时是多么令人沮丧，因为你必须使用 CUDA 等其他语言库，并且要运行并行计算，你必须使用其他库，这使得它非常具有挑战性。Jeremy 还建议，如果你想成为面向未来的人，那么开始学习 Julia，因为它将在几年内接管 Python。

# 朱莉娅

与 Python 相比，Julia 在多个领域处于领先地位，如下所述。

## 朱莉娅跑得很快

Python 可以通过使用外部库和优化工具进行优化，但是默认情况下 Julia 更快，因为它附带了 JIT 编译和类型声明。

## 数学友好的语法

Julia 通过提供类似于非计算世界的简单数学运算语法吸引了非程序员科学家。

## 自动内存管理

在内存分配方面，Julia 比 Python 更好，它为您提供了更多手动控制垃圾收集的自由。而在 python 中，您需要不断地释放内存并收集有关内存使用的信息，这在某些情况下令人望而生畏。

## 卓越的并行性

在运行科学算法时，有必要使用所有可用的资源，例如在多核处理器上运行 parallels computing。对于 Python，您需要使用外部包进行并行计算或者线程间的序列化和反序列化操作，这可能很难使用。对于 Julia 来说，它的实现要简单得多，因为它本身就具有并行性。

## 本机机器学习库

Flux 是 Julia 的机器学习库，还有其他正在开发的深度学习框架，完全用 Julia 编写，可以根据用户的需要进行修改。这些库自带 GPU 加速，不需要担心深度学习模型训练慢的问题。

> 更多信息，你可以阅读 [InfoWorld](https://www.infoworld.com/article/3241107/julia-vs-python-which-is-best-for-data-science.html) 文章。

# 概观

在本文中，我将讨论 Julia 语言的优势，并展示 DataFrame.jl 的易用性，就像 python 中的熊猫一样。我将使用简单的例子和几行代码来演示数据操作和数据可视化。我们将使用著名的[心脏病 UCI | Kaggle](https://www.kaggle.com/ronitf/heart-disease-uci) 数据集，该数据集基于多种因素对心脏病进行二元分类。

# 探索心脏病数据集

在我们开始编码之前，我想让你看看[为 JuliaCon2021 准备的 DataFrames.jl 教程](https://github.com/bkamins/JuliaCon2021-DataFrames-Tutorial)，因为我的大部分代码都是受现场会议的启发。

让我们设置你的 Julia repl，要么使用 [JuliaPro](https://juliacomputing.com/products/juliapro/) 要么为 [Julia](https://www.julia-vscode.org/docs/stable/setup/) 设置你的 **VS 代码**，如果你像我一样使用云笔记本，我建议你将下面的代码添加到你的 docker 文件中并构建它。

> 下面的 **Docker** 代码仅适用于[deep note](https://deepnote.com/)environment。

```
FROM gcr.io/deepnote-200602/templates/deepnote
RUN wget [https://julialang-s3.julialang.org/bin/linux/x64/1.6/julia-1.6.2-linux-x86_64.tar.gz](https://julialang-s3.julialang.org/bin/linux/x64/1.6/julia-1.6.2-linux-x86_64.tar.gz) && 
    tar -xvzf julia-1.6.2-linux-x86_64.tar.gz && 
    sudo mv julia-1.6.2 /usr/lib/ && 
    sudo ln -s /usr/lib/julia-1.6.2/bin/julia /usr/bin/julia && 
    rm julia-1.6.2-linux-x86_64.tar.gz && 
    julia  -e "using Pkg;pkg"add IJulia LinearAlgebra SparseArrays Images MAT""
ENV DEFAULT_KERNEL_NAME "julia-1.6.2"
```

# 安装 Julia 包

下面的方法将帮助您一次下载并安装所有多个库。

# 导入包

我们将更加关注加载数据操作和可视化。

## 加载数据

我们使用著名的心脏病 UCI | Kaggle 数据集进行初级数据分析。

**功能/栏目:**

1.  年龄
2.  性
3.  胸痛类型(4 个值)
4.  静息血压
5.  血清胆固醇(毫克/分升)
6.  空腹血糖> 120 毫克/分升
7.  静息心电图结果(值 0，1，2)
8.  达到最大心率
9.  运动诱发的心绞痛
10.  旧峰
11.  运动 ST 段峰值的斜率
12.  主要船只数量
13.  thal: 3 到 7，其中 5 是正常的。

简单地使用`CSV.read()`就像熊猫`pd.read_csv()`一样，你的数据将被加载为数据帧。

> **关于** [与 Python/R/Stata data frames . JL(juliadata.org)](https://dataframes.juliadata.org/stable/man/comparisons/)比较的更多信息

检查数据帧的形状

检查多列分布。通过使用`describe()`，我们可以观察平均值、最小值、最大值和缺失值

## 数据选择

使用`:fbs => categorical => :fbs`将列转换成分类类型，我们使用`Between`一次选择多个列。select 函数很简单，用于选择列和操作类型。

## 使用链条

如果你想一次对数据集应用多个操作，我建议你使用`@chain`功能，在 R 中它相当于`%>%`。

*   `dropmissing`将从数据库中删除丢失的值行。我们的数据集中没有任何缺失值，所以这只是为了展示。
*   `groupby`函数对给定列上的数据帧进行分组。
*   `combine`函数通过聚合函数合并数据框的行。

> 更多操作请查看 [Chain.jl](https://github.com/jkrumbiegel/Chain.jl) 文档

我们按目标对数据框进行了分组，然后合并五列以获得平均值。

使用`groupby`和`combine`的另一种方式是使用`names(df, Real)`，它返回所有具有真实值的列。

我们还可以通过`nrows`使用`groupby`和`combine`添加多个列，这将为每个**子组**显示若干行。

我们也可以将它合并，然后按如下所示进行拆分。以至于一个类别变成了**索引**，另一个变成了**列**。

## 分组依据

简单的`groupby`函数将一次显示所有组，要访问特定的组，您需要使用 Julia hacks。

**我们可以使用👇**

```
gd[(target=0,)] | gd[Dict(:target => 0)] | gd[(0,)]
```

为了获得我们关注的特定群体，这在我们处理多个类别时会有所帮助。`gd[1]`将显示第一组，其中**目标=0** 。

## 密度图

我们将使用 [StatsPlots.jl](https://github.com/JuliaPlots/StatsPlots.jl) 包来绘制图形和图表。该软件包包含扩展 [Plots.jl](https://github.com/JuliaPlots/Plots.jl) 功能的统计配方。就像 seaborn 的简单代码一样，我们可以用颜色定义不同的组来得到我们的密度图。

我们将按`target`列对其进行分组，并显示`cholesterol`，它显示胆固醇的分布。

## 组直方图

类似于密度图，我们可以使用`grouphist`来绘制不同目标类别的直方图。

# 多重情节

您可以在同一个图形上绘制多个图形，方法是在函数名的末尾使用`**!**`，例如`boxplot!()`。

以下示例显示了按目标分组的胆固醇的**小提琴图**、**箱线图**和**点线图**。

## 预测模型

我们将使用 GLM 模型，就像在 **R** 中一样，您可以使用`y~x`来训练模型。

下面的例子有二进制的`x= trestbps, age, chol, thalach, oldpeak, slope, ca`和`y= target`。我们将训练二项分布的广义线性模型来预测心脏病。正如我们所看到的，我们的模型经过了训练，但仍需要一些调整来获得更好的性能。

# 结论

我们已经展示了编写 Julia 代码是多么简单，以及它在科学计算方面是多么强大。我们发现这种语言有潜力超越 Python，因为它语法简单，性能更高。Julia 仍然是数据科学的新手，但我确信这是机器学习和人工智能的未来。

老实说，我每天都在学习关于 Julia 的新东西，如果你想了解更多关于并行计算和使用 GPU 或机器学习的深度学习，请关注我将来的另一篇文章。我没有对预测模型做更多的探索，因为这是一篇介绍性的文章，有一些概括的例子。所以，如果你认为还有更多，我可以做的，一定要让我知道，我会试着在我的下一篇文章中添加它。

> 你可以在 LinkedIn 和 T2 上关注我，我每周都会在那里发表文章。

***本文中显示的媒体不归 Analytics Vidhya 所有，由作者自行决定使用。***

*相关*

*原载于 2021 年 7 月 30 日 https://www.analyticsvidhya.com*<https://www.analyticsvidhya.com/blog/2021/07/julia-a-new-age-of-data-science>**。**