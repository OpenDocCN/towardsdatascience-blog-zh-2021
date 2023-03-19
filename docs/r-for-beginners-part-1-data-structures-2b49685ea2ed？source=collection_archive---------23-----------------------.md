# 初学者 r—第 1 部分:数据结构

> 原文：<https://towardsdatascience.com/r-for-beginners-part-1-data-structures-2b49685ea2ed?source=collection_archive---------23----------------------->

## 数据科学教学

## 一篇以非常简单的方式学习 R 语言的交互式文章。

![](img/01f880413aa3ee276007b66c29c2a326.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4727942) 的 [Tumisu](https://pixabay.com/users/tumisu-148124/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4727942)

在这篇文章中，我为初学者开始了一系列的教程，以一种非常简单和交互式的方式学习 R 语言。互动？是的，真的互动:)

在完整的 **R for 初学者第 1 部分教程**中，您将学习如何处理最常见的数据结构:

*   矢量
*   矩阵
*   因素
*   数据帧
*   列表。

这篇文章是关于向量的。完成第 1 部分教程后，您将能够使用这些数据结构:)。

# 1 创建一个向量

向量是相似事物的集合，它们必须具有相同的类型。例如，数字向量或布尔向量。可以通过函数 combine 创建一个向量:`c()`例如，下面一行代码创建一个包含四项的向量`a`。

```
a <- c(12, 23, 4, 34)
```

现在，你可以尝试用三个项目分别创建两个向量`population`和`countries`:`100, 120, 30`和`”Italy”, “France”, “Germany”`。您可以使用以下控制台进行游戏:

 [## 向量

### 向量教程

replit.com](https://replit.com/@alod83/Vectors) 

解决方案是:

```
population <- c(100, 120, 30)
print(population)countries <- c("Italy", "France", "Germany")
print(countries)
```

如果你试着创建下面的向量`v <- c('a', 10, TRUE)`呢？由于所有项目必须属于同一类型，它们都被转换为字符串！

# 2 分配名称

您可以为向量的每个项目指定一个名称。这可以通过应用于矢量的`names()`功能来完成:

```
names(population) **<-** countries
```

或者，可以在构建向量时指定名称，如下所示:

```
population <- c('Italy'=100, 'France'=33)
```

你可以回到 R 控制台，试着给`population`向量的每一个项目分配一个名称。

 [## 向量

### 向量教程

replit.com](https://replit.com/@alod83/Vectors#names.r) 

解决方案是:

```
names(population) <- countries
```

如果您再次打印`population`向量，您会看到向量的每一项都有一个名称。

# 3 基本统计

您可以通过`sum()`函数计算一个向量的所有项的和:

```
sum(population)
```

以及通过`mean()`功能得到的各项的平均值。

```
mean(population)
```

你可以在之前的 R 主机上试试这些功能。

 [## 向量

### 向量教程

replit.com](https://replit.com/@alod83/Vectors#statistics.r) 

# 4 选择项目

可以通过以下方法访问项目:

*   按索引
*   名叫
*   按条件

## **按索引选择**

按索引选择非常简单:只需在方括号中传递项目的位置号。注意，索引从 1 开始，而不是从 0 开始，因为几乎所有其他编程语言都是这样。

```
population[1]
```

它返回值`100`。

还可以访问 vector 的多个项目:例如，可以访问 vector 的元素 1 和元素 3。

```
population[c(1,3)]
```

或者，您可以通过`:`符号访问向量的一些连续项。例如，您可以访问从 1 到 3 的项目:

```
population[1:3]
```

## **按名称选择**

您可以使用以下语法通过相关名称访问给定项目:

```
population['Italy']
```

您可以通过以下 R 控制台进行选择:

 [## 向量

### 向量教程

replit.com](https://replit.com/@alod83/Vectors#select.r) 

## **按条件选择**

最终，您可以通过指定条件来访问项目。例如，您可以访问值等于或大于 100 的所有项目。首先，您指定条件:

```
condition <- population >= 100
```

然后将条件应用于向量:

```
sub_population <- population[condition]
```

有条件练吧。能否选择`population`向量中所有名称等于`France`或`Italy?`的项目，尝试使用条件，而不是按名称选择。

您可以利用以下控制台进行练习:

[](https://replit.com/@alod83/Vectors#select_cond.r) [## 向量

### 向量教程

replit.com](https://replit.com/@alod83/Vectors#select_cond.r) 

前面练习的解决方案可能如下:

```
condition <- names(population) == 'Italy' | names(population) == 'France'
sub_population <- population[condition]
```

# 5 向量间的运算

向量之间的运算行为如下:运算应用于相同位置的项目。基本运算包括和、差、除、乘。例如，以下两个向量的和:

```
a <- c(1,2,3)
b <- c(4,5,6)
a + b
```

将返回以下向量:

```
5, 7, 9
```

**如果两个向量有不同的名称，给定操作的结果保持第一个向量的名称。**

我们来练习一下！计算两个向量之间的差。哪个名字有合成向量？

 [## 向量

### 向量教程

replit.com](https://replit.com/@alod83/Vectors#operations.r) 

解决方案是:

```
result <- population1 - population2
```

一个非常有用的操作是**连接，**允许将一个向量附加到另一个向量上。这通过组合功能`c()`完成，如下所示:

```
c(a,b)
```

你可以尝试连接两个向量`population1`和`population2`。你有哪个结果？

前面练习的结果如下:

```
result <- c(population1, population2)
```

# 摘要

在这篇互动文章中，我已经说明了如何在 r 中构建向量。如果你想通过另一篇互动文章学习矩阵，请继续关注:)

如果你想了解我的研究和其他活动的最新情况，你可以在 [Twitter](https://twitter.com/alod83) 、 [Youtube](https://www.youtube.com/channel/UC4O8-FtQqGIsgDW_ytXIWOg?view_as=subscriber) 和 [Github](https://github.com/alod83) 上关注我。

# 相关文章

[](/how-to-run-r-scripts-in-jupyter-15527148d2a) [## 如何在 Jupyter 中运行 R 脚本

### 关于如何在 Jupyter 中安装并运行 R 内核的简短教程

towardsdatascience.com](/how-to-run-r-scripts-in-jupyter-15527148d2a) [](https://medium.com/analytics-vidhya/basic-statistics-with-python-pandas-ec7837438a62) [## python 熊猫的基本统计数据

### 入门指南

medium.com](https://medium.com/analytics-vidhya/basic-statistics-with-python-pandas-ec7837438a62) [](/why-a-data-scientist-needs-to-also-be-a-storyteller-89b4636cb83) [## 为什么数据科学家也需要讲故事

### 作为一名数据科学家是一份非常棒的工作:你可以收集数据，通过奇妙的抓取机制或网络…

towardsdatascience.com](/why-a-data-scientist-needs-to-also-be-a-storyteller-89b4636cb83)