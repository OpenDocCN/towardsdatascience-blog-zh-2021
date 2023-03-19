# 5 个可怕的数字功能，可以在紧要关头拯救你

> 原文：<https://towardsdatascience.com/5-awesome-numpy-functions-that-can-save-you-in-a-pinch-ba349af5ac47?source=collection_archive---------16----------------------->

## 避免被 5 个简单的功能困住

![](img/cba6613d2f0d690e8bb9f8bd2b3bd452.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 您的旅程概述

*   [设置舞台](#9585)
*   [1 —快速过滤](#17f2)
*   [2 —重塑自我，走出困境](#3e97)
*   [3 —重组你的形状](#40a0)
*   [4 —查找唯一值](#cb34)
*   [5 —组合数组](#fce7)
*   [包装](#65ba)

# 搭建舞台

用 Python 做数据科学时，包 [NumPy](https://numpy.org/) 无处不在。无论你是用 [Scikit-Learn](https://scikit-learn.org/stable/) 开发机器学习模型，还是用 [Matplotlib](https://matplotlib.org/) 绘图，你的代码中肯定会有一些 NumPy 数组。

当我开始学习 Python 中的数据科学时，我对 NumPy 能做什么知之甚少。这些年来，我提高了我的数字技能，并因此成为一名更好的数据科学家。

擅长操作 NumPy 数组可以挽救你的生命…或者至少一个小时令人沮丧的搜索。当事情变得困难时，我在这里给你的五个数字函数可以帮助你🔥

在这篇博文中，我假设您已经安装了 NumPy，并且已经使用别名`np`导入了 NumPy:

```
import numpy as np
```

我建议在阅读这篇博客之前先看看 NumPy。如果你对 NumPy 完全陌生，那么你可以查看一下 [NumPy 的初学者指南](https://numpy.org/doc/stable/user/absolute_beginners.html)或者 NumPy 上的这个 [YouTube 视频系列。](https://www.youtube.com/playlist?list=PLSE7WKf_qqo2SWmdhOapwmerekYxgahQ9)

# 1 —快速过滤

您可以使用`where`函数根据条件快速过滤数组。假设您有一个表示为一维数组的音频信号:

```
# Audio Signal (in Hz)
signal = np.array([23, 50, 900, 12, 1100, 10, 2746, 9, 8])
```

假设您想要删除`signal`中所有 Hz 小于 20 的内容。要在 NumPy 中有效地做到这一点，您可以编写:

```
# Filter the signal
filtered_signal = np.where(signal >= 20, signal, 0)# Print out the result
print(filtered_signal)
>>> np.array([23, 50, 900, 0, 1100, 0, 2746, 0, 0])
```

`where`函数有三个参数:

*   第一个参数(在我们的例子中是`signal >= 20`)给出了您想要用于过滤的条件。
*   第二个参数(在我们的例子中是`signal`)指定了当条件满足时你希望发生什么。
*   第三个参数(在我们的例子中是`0`)指定了当条件不满足时您希望发生什么。

作为第二个例子，假设你有一个数组`high-pitch`指示声音的音高是否应该提高:

```
# Audio Signal (in Hz)
signal = np.array([23, 50, 900, 760, 12])# Rasing pitch
high_pitch = np.array([True, False, True, True, False])
```

每当相应的`high-pitch`变量这么说时，要提高`signal`的音调，您可以简单地写:

```
# Creating a high-pitch signal
high_pitch_signal = np.where(high_pitch, signal + 1000, signal)# Printing out the result
print(high_pitch_signal)
>>> np.array([1023, 50, 1900, 1760, 12])
```

那很容易😃

# 2——重塑自我，摆脱困境

通常，一个数组的元素是正确的，但形式是错误的。更具体地说，假设您有以下一维数组:

```
my_array = np.array([5, 3, 17, 4, 3])print(my_array.shape)
>>> (5,)
```

这里你可以看到这个数组是一维的。您想将`my_array`输入到另一个需要二维输入的函数中吗？在 Scikit-Learn 这样的库中，这种情况经常发生。为此，您可以使用`reshape`功能:

```
my_array = np.array([5, 3, 17, 4, 3]).reshape(5, 1)print(my_array.shape)
>>> (5, 1)
```

现在`my_array`是恰当的二维。你可以把`my_array`想象成一个五行单列的矩阵。

如果你想回到一维，那么你可以写:

```
my_array = my_array.reshape(5)print(my_array.shape)
>>> (5,)
```

> **专业提示:**简单来说，你可以使用 NumPy 函数`squeeze`删除所有长度为 1 的维度。因此，你可以使用`squeeze`函数代替上面的`reshape`函数。

# 3-重组你的形状

你有时需要重组你已经拥有的维度。一个例子可以说明这一点:

假设您将一个大小为 1280x720(这是 YouTube 缩略图的大小)的 RGB 图像表示为一个名为`my_image`的 NumPy 数组。你的图像有形状`(720, 1280, 3)`。数字 3 来源于这样一个事实，即有 3 个颜色通道:红色、绿色和蓝色。

如何重新排列`my_image`以使 RGB 通道填充第一维？您可以通过`moveaxis`功能轻松实现:

```
restructured = np.moveaxis(my_image, [0, 1, 2], [2, 0, 1])print(restrctured.shape)
>>> (3, 720, 1280)
```

通过这个简单的命令，您已经重构了图像。`moveaxis`中的两个列表指定了轴的源位置和目的位置。

> **Pro 提示:** NumPy 还有其他函数，比如`swapaxes`和`transpose`，它们也处理数组的重构。`moveaxis`函数是最通用的，也是我用得最多的一个。

## 为什么重塑和重组不一样？

![](img/d75aa39d85186043dadcd74c928cd2ea.png)

照片由[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

很多人认为用`reshape`功能整形和用`moveaxis`功能重组是一样的。然而，它们以不同的方式工作😦

最好的方法是用一个例子:假设你有矩阵:

```
matrix = np.array([[1, 2], [3, 4], [5, 6]])# The matrix looks like this:
1 2
3 4
5 6
```

如果您使用`moveaxis`功能切换两个轴，那么您会得到:

```
restructured_matrix = np.moveaxis(matrix, [0, 1], [1, 0])# The restructured matrix looks like this:
1 3 5
2 4 6
```

然而，如果您使用`reshape`函数，那么您会得到:

```
reshaped_matrix = matrix.reshape(2, 3)# The reshaped matrix looks like this:
1 2 3
4 5 6
```

`reshape`函数只是按行处理，并在适当的时候生成新行。

# 4 —寻找独特的价值

`unique`函数是一个很好的实用函数，用于查找数组的唯一元素。假设您有一个数组，代表从民意调查中抽取的人们最喜欢的城市:

```
# Favorite cities
cities = np.array(["Paris", "London", "Vienna", "Paris", "Oslo", "London", "Paris"])
```

然后您可以使用`unique`函数来获取数组`cities`中的唯一值:

```
unique_cities = np.unique(cities)print(unique_cities)
>>> ['London' 'Oslo' 'Paris' 'Vienna']
```

请注意，独特的城市不一定按照它们最初出现的顺序排列(例如，奥斯陆在巴黎之前)。

有了民调，画柱状图真的很常见。在这些图表中，类别是投票选项，而条形的高度代表每个选项获得的票数。要获得这些信息，您可以使用可选参数`return_counts`,如下所示:

```
unique_cities, counts = np.unique(cities, return_counts=True)print(unique_cities)
>>> ['London' 'Oslo' 'Paris' 'Vienna']print(counts)
>>> [2 1 3 1]
```

`unique`函数可以让你避免编写许多烦人的循环😍

# 5-组合数组

有时，您会同时使用许多阵列。那么将阵列组合成单个“主”阵列通常是方便的。在 NumPy 中使用`concatenate`函数很容易做到这一点。

假设您有两个一维数组:

```
array1 = np.arange(10)
array2 = np.arange(10, 20)
```

然后您可以用`concatenate`将它们组合成一个更长的一维数组:

```
# Need to put the arrays into a tuple
long_array = np.concatenate((array1, array2))print(long_array)
>>> [0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19]
```

## 组合我们的工具

如果您想将`array1`和`array2`堆叠在彼此的顶部会怎么样？因此，您希望创建一个二维向量，如下所示:

```
[[ 0  1  2  3  4  5  6  7  8  9]
 [10 11 12 13 14 15 16 17 18 19]]
```

你可以先用`reshape`函数将`array1`和`array2`整形为二维数组:

```
array1 = array1.reshape(10, 1)
array2 = array2.reshape(10, 1)
```

现在您可以使用`concatenate`函数中可选的`axis`参数来正确组合它们:

```
stacked_array = np.concatenate((array1, array2), axis=1)print(stacked_array)
>>> 
[[ 0 10]
 [ 1 11]
 [ 2 12]
 [ 3 13]
 [ 4 14]
 [ 5 15]
 [ 6 16]
 [ 7 17]
 [ 8 18]
 [ 9 19]]
```

差不多了…现在您可以使用`moveaxis`功能来完成工作:

```
stacked_array = np.moveaxis(stacked_array, [0, 1], [1, 0])print(stacked_array)
>>> 
[[ 0  1  2  3  4  5  6  7  8  9]
 [10 11 12 13 14 15 16 17 18 19]]
```

![](img/2c212522cd5675ae8c4cded71cb49d8b.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Japheth 桅杆](https://unsplash.com/@japhethmast?utm_source=medium&utm_medium=referral)拍摄

厉害！我希望这个例子向您展示了您刚刚学到的一些不同的工具是如何组合在一起的。

# 包扎

现在，您应该对在一些棘手的情况下使用 NumPy 感到满意了。如果您需要了解更多关于 NumPy 的信息，那么请查看 NumPy 文档。

**喜欢我写的？**查看我的博客文章[类型提示](/modernize-your-sinful-python-code-with-beautiful-type-hints-4e72e98f6bf1)、[黑色格式](/tired-of-pointless-discussions-on-code-formatting-a-simple-solution-exists-af11ea442bdc)、Python 中的[下划线](https://medium.com/geekculture/master-the-5-ways-to-use-underscores-in-python-cfcc7fa53734)和 [5 字典提示](/5-expert-tips-to-skyrocket-your-dictionary-skills-in-python-1cf54b7d920d)了解更多 Python 内容。如果你对数据科学、编程或任何介于两者之间的东西感兴趣，那么请随意在 LinkedIn 上加我，并向✋问好