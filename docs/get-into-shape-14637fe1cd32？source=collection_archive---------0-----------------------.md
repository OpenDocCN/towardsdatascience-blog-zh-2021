# 进入状态！🐍

> 原文：<https://towardsdatascience.com/get-into-shape-14637fe1cd32?source=collection_archive---------0----------------------->

## 塑造和重新塑造 NumPy 和 pandas 对象以避免错误

形状误差是许多学习数据科学的人的祸根。我敢打赌，人们已经放弃了他们的数据科学学习之旅，因为无法将数据整理成机器学习算法所需的形状。

更好地了解如何重塑您的数据将使您不再流泪，节省您的时间，并帮助您成长为一名数据科学家。在本文中，您将看到如何以您需要的方式获取数据。🎉

![](img/4e977ea290deaaf6bfb901eec6e40bf7.png)

雅典有许多行和列。资料来源:pixabay.com

# 正在做

首先，让我们确保使用相似的包版本。让我们用它们通常的别名导入我们需要的库。所有代码都可以在[这里](https://github.com/discdiver/reshape)获得。

```
import sys
import numpy as np
import pandas as pd
import sklearn
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import LogisticRegression
```

如果您没有安装所需的库，请取消对以下单元格的注释并运行它。然后再次运行单元格导入。您可能需要重启您的内核。

```
# !pip install -U numpy pandas scikit-learn
```

让我们检查一下我们的软件包版本。

```
print(f"Python: {sys.version}")
print(f'NumPy: {np.__version__}')
print(f'pandas: {pd.__version__}')
print(f'scikit-learn: {sklearn.__version__}')Python: 3.8.5 (default, Sep  4 2020, 02:22:02) 
[Clang 10.0.0 ]
NumPy: 1.19.2
pandas: 1.2.0
scikit-learn: 0.24.0
```

# 规模

熊猫[数据帧](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)有两个维度:行和列。

让我们用一些飓风数据做一个小小的数据框架。

```
df_hurricanes = pd.DataFrame(dict(
    name=['Zeta', 'Andrew', 'Agnes'], 
    year=[2020, 1992, 1972 ]
))
df_hurricanes
```

![](img/17eae2cef4a25d534948632c6c869b6d.png)

您可以使用 [*ndim*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.ndim.html) 属性查看熊猫数据结构的维数。

```
df_hurricanes.ndim2
```

一个数据帧既有行又有列，所以它有两个维度。

# 形状

*形状*属性显示每个维度中的项目数。检查数据帧的[形状](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.shape.html)会返回一个包含两个整数的元组。第一个是行数，第二个是列数。👍

```
df_hurricanes.shape(3, 2)
```

我们有三行两列。酷毙了。😎

[*大小*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.size.html#pandas.DataFrame.size) 属性显示我们有多少单元格。

```
df_hurricanes.size6
```

3 * 2 = 6

从 *shape* 属性中很容易得到维度和大小的数量，所以这是我们要记住和使用的一个属性。🚀

让我们从我们的数据框架制作一个熊猫系列。使用*仅方括号*语法通过将列名作为字符串传递来选择列。你会得到一系列。

```
years_series = df_hurricanes['year']
years_series0    2020
1    1992
2    1972
Name: year, dtype: int64type(years_series)pandas.core.series.Series
```

熊猫[系列](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html)的外形是什么样子的？我们可以用系列 [*形状*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.shape.html) 属性来了解一下。

```
years_series.shape(3,)
```

我们有一个只有一个值的元组，即行数。请记住，索引不能算作一列。☝️

如果我们再次只使用方括号会发生什么，除了这次我们传递一个包含单个列名的列表？

```
years_df = df_hurricanes[['year']]
years_df
```

![](img/6d65f8766c922c4b09532dbd157d018f.png)

```
type(years_df)pandas.core.frame.DataFrame
```

我的变量名可能泄露了答案。😉如果你传递一个列名列表，你总是得到一个数据帧。

```
years_df.shape(3, 1)
```

**拿走**:熊猫系列的造型和一栏熊猫数据框的造型是不一样的！数据帧的形状为*行*乘*列*，系列的形状为*行*。这是让人犯错的关键点。

现在我们知道了如何在熊猫身上找到*形状*，让我们看看如何使用 NumPy。熊猫扩展 NumPy。

![](img/cdf56e85caa4d1e5fee534296d073798.png)

列。资料来源:pixabay.com

# NumPy

NumPy 的 ndarray 是它的核心数据结构——从现在开始我们就把它称为数组。根据您的目标，有许多方法可以创建 NumPy 数组。点击查看我的主题[指南。](/the-ten-best-ways-to-create-numpy-arrays-8b1029a972a7)

让我们从数据帧中创建一个 NumPy 数组，并检查它的形状。

```
two_d_arr = df_hurricanes.to_numpy()
two_d_arrarray([['Zeta', 2020],
       ['Andrew', 1992],
       ['Agnes', 1972]], dtype=object)type(two_d_arr)numpy.ndarraytwo_d_arr.shape(3, 2)
```

返回的形状与我们使用熊猫时看到的形状相匹配。熊猫和 NumPy 共享一些属性和方法，包括*形状*属性。

让我们将之前制作的熊猫系列转换成 NumPy 数组，并检查其形状。

```
one_d_arr = years_series.to_numpy()
one_d_arrarray([2020, 1992, 1972])type(one_d_arr)numpy.ndarrayone_d_arr.shape(3,)
```

同样，我们在熊猫和熊猫身上看到了同样的结果。酷！

![](img/a21fc450dd56f650840a472dc71a617d.png)

行和列。资料来源:pixabay.com

# 问题是

当一个对象期望数据以某种形式到达时，事情就变得棘手了。例如，大多数 scikit-learn 变压器和估算器都希望获得二维形式的预测 X 数据。目标变量 y 应该是一维的。让我们用一个愚蠢的例子来演示如何改变形状，在这个例子中，我们使用*年*来预测飓风名称。

我们将使 *x* 小写，因为它只有一维。

```
x = df_hurricanes['year']
x0    2020
1    1992
2    1972
Name: year, dtype: int64type(x)pandas.core.series.Seriesx.shape(3,)
```

我们的输出变量 *y* 也是如此。

```
y = df_hurricanes['name']
y0      Zeta
1    Andrew
2     Agnes
Name: name, dtype: objecttype(y)pandas.core.series.Seriesy.shape(3,)
```

让我们实例化并拟合一个逻辑回归模型。

```
lr = LogisticRegression()
lr.fit(x, y)
```

你会得到一个值错误。最后几行写道:

```
ValueError: Expected 2D array, got 1D array instead:
array=[2020\. 1992\. 1972.].
Reshape your data either using array.reshape(-1, 1) if your data has a single feature or array.reshape(1, -1) if it contains a single sample.
```

让我们试着按照错误消息的指示去做:

```
x.reshape(-1, 1)
```

如果你通过一个 NumPy 数组，整形是很棒的，但是我们通过了一个 pandas 系列。所以我们得到了另一个错误:

```
AttributeError: 'Series' object has no attribute 'reshape'
```

我们可以把我们的系列变成一个 NumPy 数组，然后把它重新做成二维的。然而，正如你在上面看到的，有一种更简单的方法可以让 *x* 成为 2D 对象。只需使用*方括号*语法将列作为列表传递。

我将结果设为大写 *X* ，因为这将是一个 2D 数组——大写字母是 2D 数组(也称为*矩阵*)的统计命名约定。

我们开始吧！

```
X = df_hurricanes[['year']]
X
```

![](img/6d65f8766c922c4b09532dbd157d018f.png)

```
type(X)pandas.core.frame.DataFrameX.shape(3, 1)
```

现在我们可以准确无误地拟合我们的模型了！😁

```
lr.fit(X, y)LogisticRegression()
```

# 重塑 NumPy 数组

如果我们的数据存储在 1D NumPy 数组中，那么我们可以按照错误消息的建议，用`reshape`将它转换成 2D 数组。让我们用之前保存为 1D NumPy 数组的数据来尝试一下。

```
one_d_arrarray([2020, 1992, 1972])one_d_arr.shape(3,)
```

让我们重塑它！

```
hard_coded_arr_shape = one_d_arr.reshape(3, 1)
hard_coded_arr_shapearray([[2020],
       [1992],
       [1972]])hard_coded_arr_shape.shape(3, 1)
```

传递一个正整数意味着*给出那个维度的形状*。所以现在我们的数组有了形状 *3，1* 。

然而，使用灵活、动态的选项是更好的编码实践。所以还是用 *-1* 搭配*吧。shape()* 。

```
two_d_arr_from_reshape = one_d_arr.reshape(-1, 1)
two_d_arr_from_reshapearray([[2020],
       [1992],
       [1972]])two_d_arr_from_reshape.shape(3, 1)
```

让我们解开代码。我们传递了一个 *1* ，所以第二维度——列——得到了 1。

我们为另一个维度传递了一个负整数。这意味着剩余的维度变成了保存所有原始数据所需的任何形状。

把 *-1* 想象成*填空做一个维度让所有的数据都有个家*。🏠

在这种情况下，您最终得到一个 3 行 1 列的 2D 数组。 *-1* 取值 *3* 。

让我们的代码变得灵活是一个很好的实践，这样它就可以处理我们向它抛出的任何观察。所以不要硬编码两个维度，使用`-1`。🙂

![](img/a98bcc46e34fa2d824ff0092217acfdf.png)

资料来源:pixabay.com

# 高维数组

同样的原理可以用于整形更高维的阵列。我们先做一个三维数组，然后再把它重塑成四维数组。

```
two_d_arrarray([['Zeta', 2020],
       ['Andrew', 1992],
       ['Agnes', 1972]], dtype=object)two_d_arr.shape(3, 2)three_d_arr = two_d_arr.reshape(2, 1, 3)
three_d_arrarray([[['Zeta', 2020, 'Andrew']],

       [[1992, 'Agnes', 1972]]], dtype=object)
```

使用 *-1* ，指示应该计算哪个尺寸，以准确地给出所有数据的位置。

```
arr = two_d_arr.reshape(1, 2, -1, 1)
arrarray([[[['Zeta'],
         [2020],
         ['Andrew']],

        [[1992],
         ['Agnes'],
         [1972]]]], dtype=object)
```

注意，如果*整形*尺寸没有意义，你会得到一个错误。像这样:

```
two_d_arr.reshape(4, -1)
two_d_arr--------------------------------------------------------------------

ValueError: cannot reshape array of size 6 into shape (4,newaxis)
```

我们有六个值，所以我们只能将数组调整为恰好能容纳六个值的维数。

换句话说，维数必须形成乘积*的六个*。记住 *-1* 就像一个可以变成任意整数值的通配符。

# 预测

Scikit-learn 希望大多数预测都使用 2D 阵列。

假设列表中有一个样本，您想用它来进行预测。您可能会天真地认为下面的代码可以工作。

```
lr.predict(np.array([2012]))
```

并没有。☹️

```
ValueError: Expected 2D array, got 1D array instead:
array=[2012].
Reshape your data either using array.reshape(-1, 1) if your data has a single feature or array.reshape(1, -1) if it contains a single sample.
```

但是，我们可以按照有帮助的错误建议，用`reshape(1, -1)`做一个二维数组。

```
lr.predict(np.array([2012]).reshape(1, -1))array(['Zeta'], dtype=object)np.array([2012]).reshape(1, -1).shape(1, 1)
```

您已经使第一个维度(行) *1* 和第二个维度(列)与特征数量 *1* 相匹配。酷！

不要害怕检查一个物体的形状——即使只是确认它是你所想的那样。🙂

当我们讨论 scikit-learn 的整形主题时，请注意文本矢量化转换器，如 [CountVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) 的行为与其他 scikit-learn 转换器不同。他们假设你只有一列文本，所以他们期望一个 1D 数组而不是 2D 数组。你可能需要重塑。⚠️

# 制作 1D 阵列的其他方法

除了用`reshape`整形，NumPy 的`flatten`和`ravel`都返回一个 1D 数组。区别在于它们是创建原始阵列的拷贝还是视图，以及数据是否连续存储在内存中。查看[这个](https://stackoverflow.com/a/28930580/4590385)不错的堆栈溢出答案了解更多信息。

让我们看看将 2D 数组压缩成 1D 数组的另一种方法。

![](img/1e9d13d21a7a48f6be2e180b9d4f78e2.png)

挤压重塑。资料来源:pixabay.com

# 挤出不需要的维度

当你有一个多维数组，但其中一个维度不包含任何新信息时，你可以用`.squeeze()`将*挤出不必要的维度。举个例子，我们用之前做的数组。*

```
two_d_arr_from_reshapearray([[2020],
       [1992],
       [1972]])two_d_arr_from_reshape.shape(3, 1)squeezed = np.squeeze(two_d_arr_from_reshape)squeezed.shape(3,)
```

哒哒！

请注意，TensorFlow 和 PyTorch 库与 NumPy 配合得很好，可以处理表示视频数据等内容的高维数组。将数据转换成神经网络输入层所需的形状是一个常见的错误来源。您可以使用上面的工具将数据调整到所需的维度。🚀

# 包装

您已经看到了如何重塑 NumPy 数组。希望您看到的未来代码会更有意义，您将能够快速地将 NumPy 数组处理成您需要的形状。

如果你觉得这篇关于重塑 NumPy 数组的文章很有帮助，请在你最喜欢的社交媒体上分享。😀

我帮助人们学习如何用 Python、熊猫和其他工具来处理数据。如果你觉得这听起来很酷，请查看我的其他[指南](https://medium.com/@jeffhale)并加入我在 Medium 上的 15，000 多名粉丝，获取最新内容。

![](img/7a977a980f2cb93d7c796fd794225721.png)

快乐重塑！🔵🔷