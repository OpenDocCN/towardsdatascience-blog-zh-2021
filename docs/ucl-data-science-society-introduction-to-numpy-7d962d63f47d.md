# UCL 数据科学协会:Numpy 简介

> 原文：<https://towardsdatascience.com/ucl-data-science-society-introduction-to-numpy-7d962d63f47d?source=collection_archive---------25----------------------->

## 工作坊 5:什么是 Numpy，Numpy 数组，数学运算符，随机数

![](img/f52a6e99f3a1c7fba2568287185aa868.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Dhru J](https://unsplash.com/@dhruj?utm_source=medium&utm_medium=referral) 拍摄

今年，作为 UCL 数据科学学会的科学负责人，该学会将在整个学年举办一系列 20 场研讨会，涵盖的主题包括数据科学家工具包 Python 和机器学习方法简介。每一篇文章的目标都是创建一系列的小博客，这些小博客将概述要点，并为任何希望跟进的人提供完整研讨会的链接。所有这些都可以在我们的 [GitHub](https://github.com/UCL-DSS) 资源库中找到，并将在全年更新新的研讨会和挑战。

第五个研讨会是对 Numpy python 包的介绍，我们将向您介绍 Numpy，涵盖 Numpy 数组的创建，介绍 Numpy 中的数学运算，并与包的随机数功能进行交互。虽然这里将分享一些亮点，但完整的研讨会(包括自测题)可以在这里找到[。](https://github.com/UCL-DSS/numpy-workshop)

如果您错过了我们之前的任何研讨会，您可以点击以下链接:

</ucl-data-science-society-python-sequences-e3ffa67604a0>  </ucl-data-science-society-python-logic-3eb847362a97>  </ucl-data-science-society-object-oriented-programming-d69cb7a7b0be>  

## 什么是 Numpy？

Numpy 是我们在研讨会系列中引入的第一个完整的 Python 包，之前我们只是使用了 Python 的基本功能。从这个意义上说，包通常包含许多有用的内置函数，这意味着我们不必重新发明轮子来得到想要的结果。特别是，Numpy 专注于许多数学功能，并支持其他 Python 包中的许多方法和函数。

根据[numpy.org 的](https://numpy.org/devdocs/user/absolute_beginners.html) NumPy 是:

> …是一个开源的 Python 库，几乎用于科学和工程的每个领域。这是在 Python 中处理数字数据的通用标准，也是科学 Python 和 PyData 生态系统的核心。

这意味着它是一个基础库，虽然您可能不会以其原始形式与之交互，但您可能会以某种方式通过基于此功能的其他包遇到它。

## Numpy 数组

Numpy 最重要的特性可能是数组，它是 Numpy 的基础数据结构。这些 Numpy 数组在嵌入它们的一些功能和它们的结构方面类似于列表，但是你可以用它们做更多列表做不到的事情，比如允许你在大数据集上执行各种操作。

有许多方法可以生成数组，例如:

**np.array**

它将一个列表转换成一个 numpy 数组:

```
# Generates an array [1 2 3]
x = np.array([1,2,3])
print(x)# Generates an array [[1 2 3] [4 5 6]]
x = np.array([[1,2,3],[4,5,6]])
print(x)#out:
[1 2 3]
[[1 2 3]
 [4 5 6]]
```

**np.linspace**

它有三个参数:起点、终点和点数。第一个数字`0`告诉内核从`0`开始，第二个数字告诉它在`10`结束，而第三个数字告诉它数组中应该有多少个值。该数组将由一组均匀分布的值创建，从`0`开始，到`10`结束:

```
# Generates an array [0 2.5 5 7.5 10]
x = np.linspace(0,10,5)
print(x)#out:
[ 0\.   2.5  5\.   7.5 10\. ]
```

**np.arange**

它同样需要三个参数:起点、终点(不包含！)，以及每个值之间的间距。在我们的例子中，我们指定`0`的开始点和`11`的结束点(不包括在最终数组中)，其中`1`是值之间的间距:

```
# Generates an array [1 2 3 4 5 6 7 8 9 10]
x = np.arange(0,11,1)
print(x)#out:
[ 0  1  2  3  4  5  6  7  8  9 10]
```

**np.zeros** 和**NP . one**

就产生特定大小的数组而言，这些可能是最有用的选择，尽管如果你想使用除了 1 或 0 之外的东西，你可能必须填充它们。在第一个实例中，函数中的数字简单地指定了数组中需要多少个 1 或 0，同时我们还可以使用指示维度大小的元组来创建更复杂的多维数组:

```
# Generats an array [0, 0.] in float64
x = np.zeros(2)
print("1st: ", x)# Generates an array [1 1 1] in integer
x = np.ones(3, int)
print("\nSecond: ", x)# Generates an 3x3 array of zeros
x = np.zeros((3,3))
print("\nThird: \n", x)# Generates an 3x3 array of ones in integer
x = np.zeros((3,3), int)
print("\nFourth: \n", x)#out:1st:  [0\. 0.]

Second:  [1 1 1]

Third: 
 [[0\. 0\. 0.]
 [0\. 0\. 0.]
 [0\. 0\. 0.]]

Fourth: 
 [[0 0 0]
 [0 0 0]
 [0 0 0]]
```

一旦定义了这些，我们就可以像使用索引和切片来访问列表一样访问信息。但是，当我们在一个阵列中有多个维度时，这可能会变得更加复杂:

```
# Creates an array [[0 1 2] [3 4 5]]
x = np.array([[0,1,2],[3,4,5]])
print(x)# First row
print("First row:", x[0])# Second row
print("Second row:", x[1])# Second column
print("Second column:", x[:,1])# First row, third column 
print("First row, third column:", x[0,2])#out: 
[[0 1 2]
 [3 4 5]]
First row: [0 1 2]
Second row: [3 4 5]
Second column: [1 4]
First row, third column: 2
```

## 数学运算

数组优于列表的一个好处是数学运算可以应用于数组。虽然除非迭代列表中的值，否则不能对它们执行数学运算，但是可以简单地对 Numpy 数组中的每一项执行数学运算。例如:

```
# Defining a list
x = [0, 1, 2, 3, 4, 5]# Multiply list by 2
print("List multiplied by 2:", x*2)# List added to a list
print("List added to a list:", x + x)#out:
List multiplied by 2: [0, 1, 2, 3, 4, 5, 0, 1, 2, 3, 4, 5]
List added to a list: [0, 1, 2, 3, 4, 5, 0, 1, 2, 3, 4, 5]
```

在这里我们可以看到，任何应用于一个整体列表的数学运算，都将列表视为一个整体的值结构。但是，当我们将此应用于阵列时:

```
# Turns list into array
x = np.array(x)# Multiply array by 2
print("Array multiplied by 2:", x*2)# Adding 2 to array
print("Adding 2 to array:", x + 2)# Adding array to array
print("Adding array to array:", x + x)# Multiplying array to array (a.k.a squaring it)
print("Squaring an array:", x * x)#out:
Array multiplied by 2: [ 0  2  4  6  8 10]
Adding 2 to array: [2 3 4 5 6 7]
Adding array to array: [ 0  2  4  6  8 10]
Squaring an array: [ 0  1  4  9 16 25]
```

我们可以看到该操作已经应用于数组本身的每个值。因此，这加强了 numpy 库对大量数据进行数学运算的有用性。

Numpy 还为这些阵列提供了广泛的功能，包括:

```
# Creating an array
x = np.array([3,2,1,0])# Finds the sum of all values in the array
print("Sum of the values:", np.sum(x))# Finds the average of all values in the array
print("Average of the values:", np.mean(x))# Sorts the values in the array into ascending order
print("Sorted values:", np.sort(x))# Returns the maximimun value in the array
print("Maximum value:", np.max(x))# Returns the minimum value in the array
print("Minimum value:", np.min(x))# Returns the standard deviation
print("Standard deviation:", np.std(x))# Returns the variance 
print("Variance:", np.var(x))# Returns the size of the array
print("Size:", np.size(x))# Returns the shape of the array
# which is a tuple of ints giving the lengths of the corresponding array dimensions
print("Shape:", np.shape(x))# Multi-dimensional array
y = np.array([[1, 2, 3], [4, 5, 6,], [4, 5, 6,]])
print("Shape of y:", np.shape(y))#out:Sum of the values: 6
Average of the values: 1.5
Sorted values: [0 1 2 3]
Maximum value: 3
Minimum value: 0
Standard deviation: 1.118033988749895
Variance: 1.25
Size: 4
Shape: (4,)
Shape of y: (3, 3)
```

内置于此的其他功能包括沿数组移动值、将数组添加到现有数组、将多个数组相加以及重塑数组的能力。

然而，numpy 的另一个有用的内置功能是内置的数学函数和常数，帮助它作为 Python 中数学运算的基础:

```
# Pi - NOT A FUNCTION
print("Pi:", np.pi)# Square root function
print("Square root:", np.sqrt(9))# Trigonometric functions (default input is in radians)
print("Sine:", np.sin(np.pi/2))
print("Cosine:", np.cos(0))# Natural Constant e
print("e:", np.e)# Exponential (input is the power)
print("Exponential:", np.exp(0))# Natural logarithm ln
print("ln:", np.log(1))# A complex number 1+2i 
print("Complex Number:", np.complex(1, 2))#out:
Pi: 3.141592653589793
Square root: 3.0
Sine: 1.0
Cosine: 1.0
e: 2.718281828459045
Exponential: 1.0
ln: 0.0
Complex Number: (1+2j)
```

## **随机数**

除了数组和数学运算之外，Numpy 还可以用于生成随机数，这些随机数可以用于许多功能，例如模拟、随机值选择或生成一组虚拟数据。有两种主要方法可以做到这一点:

**随机的**

它从区间[0.0，1.0]中返回一个随机浮点数。如果没有传递参数，那么它将只是一个数字，但是您也可以传递维度来创建一个数组:

```
# Generates a random number from 0 to 1, exclusively
print(np.random.random())# Generate 6 random number from 0 to 1, exclusively
print(np.random.random(6))# Generates an array of random numbers for a 3x3 array
print(np.random.random((3,3)))#out:0.5641615164281893
[0.01950781 0.72695885 0.47460399 0.26587963 0.9011898  0.871929  ]
[[0.30523122 0.60809893 0.53312007]
 [0.07440937 0.81311535 0.49964168]
 [0.75633448 0.88113332 0.70084173]]
```

**相反，该函数能够返回特定范围内的随机整数，其中第一个值是可能的最小值，而第二个输入比可能的最大值高一。但是，和前面一样，我们也可以使用第三个输入来创建数组，第三个输入是要用随机整数填充的数组的维度(作为一个元组):**

```
# Generates a random number from 1 to 6
print(np.random.randint(1, 7))# Generates a 3x3 array of random integers from 1 to 6
print(np.random.randint(1, 7, (3,3)))# 9x4 array of random integers from 10 to 25
print(np.random.randint(10, 26, (9,4)))#out:
6
[[5 6 2]
 [5 6 5]
 [6 3 2]]
[[17 10 22 19]
 [18 10 19 24]
 [13 20 10 19]
 [21 23 22 15]
 [16 25 25 21]
 [19 20 25 12]
 [24 19 24 23]
 [14 16 11 20]
 [23 17 11 20]]
```

**当然，Numpy 的功能远不止这里描述的这三个用例，这包括能够处理复数、创建不同形式的数组(使用对数标度的数组或从现有数组生成 2D 数组)以及更多的数学运算(如向量的绝对值、点积的标量和矩阵乘法)。**

**虽然这些大部分都包含在[研讨会笔记本](https://github.com/UCL-DSS/numpy-workshop/blob/master/workshop.ipynb)中，Numpy 当然比这延伸得更远。但是，如果您想在我们这里介绍的任何功能上挑战自己，请随意查看我们的[问题工作表](https://github.com/UCL-DSS/numpy-workshop/blob/master/problem.ipynb)！**

**完整的研讨会笔记，以及进一步的示例和挑战，可以在 这里找到 [**。如果您想了解我们协会的更多信息，请随时关注我们的社交网站:**](https://github.com/UCL-DSS/numpy-workshop)**

**https://www.facebook.com/ucldata[脸书](https://www.facebook.com/ucldata)**

**insta gram:[https://www.instagram.com/ucl.datasci/](https://www.instagram.com/ucl.datasci/)**

**领英:[https://www.linkedin.com/company/ucldata/](https://www.linkedin.com/company/ucldata/)**

**如果你想了解 UCL 数据科学协会和其他优秀作者的最新信息，请使用我下面的推荐代码注册 medium。**

**<https://philip-wilkinson.medium.com/membership> ** **</univariate-outlier-detection-in-python-40b621295bc5>  </introduction-to-hierarchical-clustering-part-1-theory-linkage-and-affinity-e3b6a4817702>  </introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d> [## scikit-learn 决策树分类器简介

towardsdatascience.com](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d)**