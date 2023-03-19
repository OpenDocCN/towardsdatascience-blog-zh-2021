# 数据科学三部曲

> 原文：<https://towardsdatascience.com/the-data-science-trilogy-numpy-pandas-and-matplotlib-basics-42192b89e26?source=collection_archive---------3----------------------->

## NumPy，Pandas 和 Matplotlib 基础

![](img/da7d3fa981979a8d73137776899c1ac3.png)

img src:[https://www . pexels . com/photo/business-charts-commerce-computer-265087/](https://www.pexels.com/photo/business-charts-commerce-computer-265087/)

那么你是 Python 的新手。或者您可能已经熟悉了这些库，但是想快速复习一下。无论情况如何，Python 毫无疑问已经成为当今最流行的编程语言之一，正如下面的[堆栈溢出趋势](https://insights.stackoverflow.com/trends?tags=python%2Cjavascript%2Cjava%2Cc%23%2Cphp%2Cc%2B%2B&utm_source=so-owned&utm_medium=blog&utm_campaign=gen-blog&utm_content=blog-link&utm_term=incredible-growth-python)中的图表所示:

![](img/91f5e566b5c1d385f289f3484b0e5903.png)

Img src:堆栈溢出趋势

在数据科学和机器学习中使用的最流行的 Python 包中，我们找到了 **Numpy、Pandas** 和 **Matplotlib。**在本文中，我将简要提供一个从零到英雄(双关语，wink wink) )数据科学 Python 入门所需的所有基础知识介绍。我们开始吧！

## 设置

首先，您需要安装软件包。如果你对 Python 完全陌生，我推荐你阅读本教程，或者任何你认为合适的教程。您可以使用以下`pip`命令:

```
pip install numpy -U 
pip install pandas -U 
pip install matplotlib -U 
```

完成后，我们现在将有以下导入:

这里，我们使用了每个包的通用别名。

## Numpy

![](img/0adf75e1268b113297155f0e02108822.png)

img src:[https://github . com/numpy/numpy/blob/main/branding/logo/primary/numpylogo . SVG](https://github.com/numpy/numpy/blob/main/branding/logo/primary/numpylogo.svg)

Numpy 是机器学习和数据科学中使用的主要库之一:它用于各种数学计算，以优化的 C 代码为基础编写。

**Numpy 数组**

数组只是对象的集合。一个 1 级数组是一个列表。2-秩数组是一个矩阵，或者一个列表的列表。一个三级数组是一系列列表的列表，等等。

我们可以用一个常规的 Python 列表作为参数，用`np.array()`构造函数创建一个 numpy 数组:

np 数组最常见的属性之一是它的`shape`，它表示数组的秩:

我们得到一个具有相应秩的元组，我们称之为数组的维数。在这种情况下，上面的阵列是一维的，也称为**平面阵列。**我们还可以用一个列表的列表来获得一个更清晰的类似矩阵的形状:

在这种情况下，这看起来更像是一个**行向量。**类似地，我们可以如下初始化一个**列向量**:

我们也可以用同样的方法初始化全矩阵:

当然，这只是一个元组:

**重塑数组**

我们可以使用`np.reshape`函数来改变数组的维数，只要它包含相同数量的元素并且维数有意义。例如:将`M1`矩阵整形为行向量:

**平面阵列到矢量**

我们可以将平面阵列转换为 2D 阵列(向量)，如下所示:

**Numpy 数组不是列表**

注意，列表和 NumPy 数组之间有一个重要的区别！

我们可以直接看到的一点是印刷风格。我们也有非常不同的行为:

**点积**

我们可以用`np.dot(a1,a2)`函数“乘”匹配的相邻维度的数组，其中`a1.shape[1]==a2.shape[0]`。《出埃及记》两个数组(通常用于机器学习)或 n 维向量的数学点积，由下式给出

![](img/fa2cdaf23a09a00b07403bb2873f6d0d.png)

(图片由作者使用 LaTeX)

对于循环，它看起来像这样:

更好的方法是:

现在，对于大量数据来说，速度上的差异是显而易见的，如下面的示例所示:

这是因为它利用了**并行化**。

**生成随机数组**

一种方法是使用`np.random.randn()`功能:

## 熊猫

![](img/53f30c5adabaab7bf05fec079f5afae9.png)

img src:[https://pandas.pydata.org/about/citing.html](https://pandas.pydata.org/about/citing.html)

Pandas 是一个用于数据操作的库。它与 Numpy 高度兼容，尽管它有一些微妙之处。

**初始化表格形成 Numpy 数组**

您可以将 rank-2 Numpy 数组传递给`pd.DataFrame()`构造函数来初始化 pandas 数据框对象。

![](img/8c7f4d639e57f49e5c99f2e54a1d90e8.png)

代码输出(图片由作者提供)

**重命名列**

我们可以使用`.columns`属性访问表的列名:

我们可以通过指定具有相应名称的列表来一次性重命名所有列:

![](img/a527b903680f8913b1da92b3aa9410d6.png)

代码输出(图片由作者提供)

现在我们的数据框有了输入列名。

**列选择**

我们可以使用`df['colname']`语法从 pandas 数据帧中选择列。结果是一个 Pandas `Series`对象，它类似于平面数组，但是集成了许多属性和方法。

有许多内置方法(参见[文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html)，但是作为一个例子，我们可以通过使用`describe()`方法获得**汇总统计数据**:

**设置多个列的子集**

用已定义列的列表对多个列进行子集化会输出一个 pandas dataframe 对象。

![](img/4d4f1eb1183872a48bf93e492c90152b.png)

代码输出(图片由作者提供)

请注意，这些列不必按照任何特定的顺序排列；这有助于按照我们的意愿重新排列表格:

![](img/bce476fc0065dcefa9557b92a08fb10a.png)

代码输出(图片由作者提供)

**分配列**

假设我们想要获得满足特定条件的所有行。我们如何做到这一点？以下面的虚拟数据为例:

![](img/5d17ebc50c9e3220771fcf07dff6ab9b.png)

代码输出(图片由作者提供)

我们如何将一个额外的列集成到这个数据中？我们可以简单地用`df['new_col'] = new_col`语法分配一个兼容大小(行数)的数组、列表或序列:

![](img/c78b356066eb76510e039418c31cc1a0.png)

代码输出(图片由作者提供)

现在假设我们想获取满足某个条件的所有记录。这种选择可以使用以下语法来完成:

```
df[condition]
```

其中`condition`是布尔值的 **NumPy 数组**。感谢 **NumPy 广播**，将 NumPy 数组/pandas 系列与**的一个**值进行比较，将产生一个相同类型的数组，其中每个值都与查询进行比较。
例如，将`Request`列与值`no`比较得出:

它指示条件为真的所有地方。然后，我们可以使用上面提到的语法，使用这个数组只选择属于`True`的行:

![](img/f1a9adaee51989449f89ab3e97f3219b.png)

代码输出(图片由作者提供)

我们可以使用逻辑产生更复杂的查询。例如:取所有消耗指数为负的行**或**的影响分值大于 1，**和**的服务类型为`service1`:

![](img/06c4ecd33b0ac17f9f4e2f9ddd900a38.png)

代码输出(图片由作者提供)

这里，我们可以对提取信息所需的所有三个条件进行编码

![](img/e5ffd73a882e60e8a1f82f6a0e3c8c9f.png)

代码输出(图片由作者提供)

**熊猫申请**

按照前面的示例，假设我们希望将某个函数应用于特定列的每个值，并获得一个序列，其中每个元素都是对相应元素应用的函数的输出。对于这些情况，我们可以使用熊猫`Series`对象的`apply()`方法。在数据帧中，它将具有以下语法:

```
df['column'].apply(func)
```

其中`func` 可以是返回一个**单输出**的任何函数。

**示例:**将每个服务映射到某个 bin，并将结果列分配给 dataframe。

![](img/8f7c10f127a7f7f9ab00b4d182affc62.png)

代码输出(图片由作者提供)

**熊猫位置**

现在假设您想要**改变**某一列中的某一行，当这些行满足指定的条件时。其语法是

```
df.loc[condition, 'column'] = new_val
```

**示例:**将`Request`的值设置为“否”则`Factor`的类型为 1:

![](img/0639889d22cc05679449fdff1dc123dc.png)

代码输出(图片由作者提供)

**对数据帧进行排序**

要按一定的顺序对数据帧进行排序，我们可以使用`sort_values()`。使用`inplace=True`参数改变原始数据帧。通过在列表中指定多个列，可以对它们进行排序。

![](img/6a35ca5bde6477ffbe00e660eef5aa82.png)

代码输出(图片由作者提供)

我们看到这些值已经相应地更新了。

**重置索引**

对值进行排序时，索引会发生变化。我们可以重置它，以便再次获得排序的索引:

![](img/7564be5755f7f78fe50119ac16e31653.png)

代码输出(图片由作者提供)

**删除列**

重置索引时，旧索引作为一列保留。我们可以用`drop()`方法删除任何列。

![](img/80cb203aecf2a4bcdd362933c58da212.png)

代码输出(图片由作者提供)

**聚合**

Matplotlib 的另一个有用之处是**聚合**。假设我们想要合计每个`EDX`组的`Impact`计数。语法如下:

```
df.groupby("column").agg_function().reset_index()
```

`agg_function()`是一个熊猫函数，如`sum()`或`mean()`(见本文)。

![](img/7a389a21212b95595a39058fe9a92432.png)

代码输出(图片由作者提供)

## Matplotlib

![](img/64f2abd8baf5225003de8130b382f2a6.png)

img src:[https://matplotlib.org/stable/gallery/misc/logos2.html](https://matplotlib.org/stable/gallery/misc/logos2.html)

顾名思义，这是 Python 的主要绘图库。许多其他库如`seaborn`依赖于此。

*   文件:[https://matplotlib.org/](https://matplotlib.org/)

**散点图**

散点图是通过指定两个长度相同的数组或列表而得到的 2D 图。例如，我们可以比较消费指数及其影响。

以下是一些常见的 Pyplot 函数:

*   `plt.figure()`:指定参考画布。通过`figsize=(a,b)`改变绘图画布尺寸。
*   `plt.scatter(x,y,color="b", marker="")`:生成散点图。`marker`指定绘制点的形状。

![](img/8aa6c8f99a98f1d81df4d95985a368cd.png)

使用熊猫的代码输出(图片由作者提供)

当然，如果有更多的数据，这将更有意义。让我们人工生成这个:

![](img/9b26d4822ec03c3d7a5cedd7f7ab3f0c.png)

使用熊猫的代码输出(图片由作者提供)

**剧情**

如果我们想以线性的方式来观察这种关系，有时最好使用`plot`函数来代替。语法是相似的。

![](img/85de0dd77bacaa9fce837ae69057f388.png)

使用熊猫的代码输出(图片由作者提供)

**注意:** Matplolib `plot`方法将按照给出的顺序绘制给定点**。**

**重叠地块**

我们可以通过调用 plot 函数两次来覆盖共享相同 x 轴值的两个图，每次调用 y 轴值:

![](img/95dd3e7863c1cacaeb976574baf752d5.png)

使用熊猫的代码输出(图片由作者提供)

**柱状图**

熊猫的一个优点是，它整合了许多常见的 Matlotlib 情节。例如，我们可以用`plot()`方法绘制一个柱状图。这可以与其他 matplotlib 函数结合使用，如`title()`、`xlabel()`等。

![](img/ee6fb88e8f7dbddf5f22ec808cd708c5.png)

使用熊猫的代码输出(图片由作者提供)

对于 instnace，我们可以使用`df.plot.bar()`方法直接从数据帧创建柱状图:

![](img/89dcb6971e48fa230179369615b94ec4.png)

使用 Pandas 和 Matplolib 的代码输出(图片由作者提供)

`figsize`参数与`figure(figsize=())`相同，`rot`表示标签的旋转。当然，你也能猜到这里的`x`和`y`是干什么用的。

组条形图也可以被覆盖。

![](img/3723d1742cb6ea2823a2248529d92b5a.png)

使用 Pandas 和 Matplolib 的代码输出(图片由作者提供)

其他示例可在[文档](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.bar.html)中找到。

**饼状图**

使用相同的数据框架设计，我们可以很容易地绘制如下条形图:

![](img/e94b222b7a2300941467b6274b56b3c8.png)

使用 Pandas 和 Matplolib 的代码输出(图片由作者提供)

![](img/64c7354ef7e7758b8d9900c3352cd11e.png)

使用 Pandas 和 Matplolib 的代码输出(图片由作者提供)

启用`subplots=True`允许为不同的计数列绘制不同的条形图。

## **奖金:Seaborn！**

有时，有些情节天生就更漂亮，或者更容易用高级 Matplotlib 包装库 Searborn 制作。

**回归图**

对于这个例子，我们将使用 Searborn 提供的 [Tips 数据集](https://github.com/mwaskom/seaborn-data)。

![](img/f866d10134a27f33b84aa3605cf027db.png)

由 Seaborn 提供的 tips 数据集预览(图片由作者提供)

![](img/6c1cf706c40913408601c58a39a2b13b.png)

代码输出(图片由作者提供)

传递`x_bins`参数会将连续数据聚集到箱中，并提供额外的线条来指示箱的密度:

![](img/3f037a18f72722908e52b1d05811fbb9.png)

代码输出(图片由作者提供)

**箱线图**

箱线图是一种直观显示数据向量的常见分布统计信息的快速方法，如下图所示:

![](img/7eecfe4c0c11b7825a1eca6d1d503291.png)

img src:[https://upload . wikimedia . org/Wikipedia/commons/1/1a/box plot _ vs _ pdf . SVG](https://upload.wikimedia.org/wikipedia/commons/1/1a/Boxplot_vs_PDF.svg)

![](img/3b612dd5a6c0503e8475cd369ee1248e.png)

代码输出(图片由作者提供)

**分布图**

另一个有趣的事情是使用 displot 来获得一些数据分布的完整图像。

![](img/53be8e58be68b99363f84c46ded5db77.png)

Seaborn 代码输出(图片由作者提供)

**Pairplots**

Pairplots 允许我们同时看到 out dataframe 列和每个列的分布之间的相关性，以各种方式表示。我们将用 seaborn 的预编码数据来说明这一点。

![](img/b5938aafea04dd34f235825fe4c77c8d.png)

代码输出(图片由作者提供)

我们可以简单地使用 Seaborn 的`pairplot`函数来生成下面的图:

![](img/b448c8cfe02e63cbd757efb80f27769e.png)

Seaborn 代码输出(图片由作者提供)

使用`hue`参数也允许我们在图中指定组:

![](img/ed605d7dd02b68c5aa36fb713559063b.png)

Seaborn 代码输出(图片由作者提供)

所有的标签和细节都是自动完成的。您也可以通过使用`diag_kind="hist"`作为参数来改变对角线分布图的类型。

![](img/c1860dc5ce17b3d1c87d797e4fd5b413.png)

Seaborn 代码输出(图片由作者提供)

为了绘制**内核密度估计值**，您还可以指定`kind="kde"`作为参数。

![](img/97b546464dc80d30ed4429b9486c00c8.png)

Seaborn 代码输出(图片由作者提供)

**相关图**

我们可以通过在 pandas 数据帧上使用`corr()`方法生成一个相关矩阵来进行快速分析，这将生成一个数值列之间的相关值矩阵。然后，我们可以简单地用`annot=True`参数将它包在`sns.heatmap()`函数周围，得到下面的图:

![](img/0cc5d3adcc7df6c769a4d8f6f7a4d27b.png)

Seaborn 代码输出(图片由作者提供)

**线图**

线图对于可视化类似时间序列的数据很有用；即从一点组织到另一点。这里有一个取自`Seaborn` [文档](https://seaborn.pydata.org/examples/errorband_lineplots.html)的例子:

首先，我们加载 FMRI 数据集(参见[来源](https://github.com/mwaskom/Waskom_CerebCortex_2017))。

![](img/c3347419894e78e9177f43d541538455.png)

Seaborn 数据集显示(图片由作者提供)

接下来，我们使用`lineplot`函数指定与时间相关的变量`timepoint`，以及作为“响应”的`signal`变量。因为这是一个分类变量，我们为每个变量获取多行！此外，我们还将`hue`指定为区域，即分组。这产生了一个非常好的图表，其中有四个不同的趋势被初步分组:

![](img/72842e5a8988891a3e27849ddc6d7c34.png)

Seaborn 代码输出(图片由作者提供)

## 接下来会发生什么？

安:今天的介绍就到这里！当然，没有哪一篇文章会涵盖所有内容，所以这里有几篇其他的好文章，这样你就可以继续你的学习之旅:

</a-beginners-guide-to-data-analysis-in-python-188706df5447>  </introduction-to-data-visualization-in-python-89a54c97fbed>  

## 来源

*   [Python 标志](https://www.python.org/community/logos/)
*   [趋势图](https://insights.stackoverflow.com/trends?tags=python%2Cjavascript%2Cjava%2Cc%23%2Cphp%2Cc%2B%2B&utm_source=so-owned&utm_medium=blog&utm_campaign=gen-blog&utm_content=blog-link&utm_term=incredible-growth-python)
*   [数字标识](https://en.wikipedia.org/wiki/File:NumPy_logo_2020.svg)
*   [熊猫标志](https://pandas.pydata.org/about/citing.html)
*   [Matplotlib 标志](https://matplotlib.org/stable/gallery/misc/logos2.html)
*   [箱线图&正态分布图](https://commons.wikimedia.org/wiki/File:Boxplot_vs_PDF.svg)
*   [FMRI 数据集](https://github.com/mwaskom/Waskom_CerebCortex_2017)

## 跟我来

1.  [https://jairparraml.com/](https://jairparraml.com/)
2.  [https://blog.jairparraml.com/](https://blog.jairparraml.com/)
3.  [https://www.linkedin.com/in/hair-parra-526ba19b/](https://www.linkedin.com/in/hair-parra-526ba19b/)
4.  https://github.com/JairParra
5.  【https://medium.com/@hair.parra 

<https://www.linkedin.com/in/hair-parra-526ba19b/> 