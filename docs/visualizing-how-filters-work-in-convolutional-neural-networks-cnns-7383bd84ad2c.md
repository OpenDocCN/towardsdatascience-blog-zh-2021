# 可视化滤波器如何在卷积神经网络(CNN)中工作

> 原文：<https://towardsdatascience.com/visualizing-how-filters-work-in-convolutional-neural-networks-cnns-7383bd84ad2c?source=collection_archive---------8----------------------->

## 使用 Excel 了解边缘检测的工作原理

![](img/a3e516b14446803a685a632fefa4aacf.png)

约翰·巴克利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在深度学习中，**卷积神经网络** (CNN)是一种特殊类型的神经网络，旨在通过多层阵列处理数据。CNN 非常适合像图像识别这样的应用，特别是经常用于人脸识别软件。

在 CNN 中，*卷积层*是创造奇迹的基本构件。在典型的图像识别应用中，卷积层由几个滤波器*组成，用于检测图像的各种*特征*。理解这项工作如何最好地用类比来说明。*

> 假设你看到有人从远处朝你走来。从远处看，你的眼睛会试图检测图形的边缘，你会试图将该图形与其他物体区分开来，如建筑物或汽车等。当这个人向你走近时，你试着关注这个人的形状，试着推断这个人是男是女，瘦还是胖，等等。随着这个人越来越近，你的注意力转移到这个人的其他特征上，比如他的面部特征，他是否戴眼镜，等等。总的来说，你的关注点从宽泛的特性转移到了具体的特性。

同样，在 CNN 中，有几层包含各种过滤器(通常称为内核)的层，负责检测您试图检测的目标的特定特征。早期层试图集中于广泛的特征，而后面的层试图检测非常具体的特征。

在 CNN 中，每个卷积层中各种滤波器的值是通过对特定训练集进行训练而获得的。在训练结束时，您将拥有一组唯一的过滤器值，用于检测数据集中的特定要素。使用这组滤镜值，您可以将它们应用到新图像上，以便预测图像中包含的内容。

向 CNN 初学者教授的挑战之一是解释过滤器是如何工作的。学生们常常难以想象(并非有意双关)过滤器的用法。正是带着这个目标，我开始写这篇文章。我希望在这篇文章结束时，你会对 CNN 中的过滤器如何工作有一个更好的理解。

# 获取我们的数据

深度学习的一个经典例子是 MNIST 数据集。我将在我们的例子中使用它。

> 何 **MNIST** 数据库(**修改后的国家标准与技术研究所数据库**)是一个手写数字的大型数据库，通常用于训练各种图像处理系统。

![](img/f0adac2fc46c4d557d310ca9a91bc6f0.png)

来源:[https://en . Wikipedia . org/wiki/MNIST _ 数据库#/media/File:mnistexamples . png](https://en.wikipedia.org/wiki/MNIST_database#/media/File:MnistExamples.png)

使用 TensorFlow，您可以按如下方式加载 MNIST 数据:

```
from tensorflow.keras.datasets import mnist
(X_train, y_train), (X_test, y_test) = mnist.load_data()
```

我现在要做的是使用该数据集中的特定项目，提取其数据，然后将其保存到 CSV 文件中。下面的代码片段可以做到这一点。

```
item = 66             # index of the digit to load
data = X_train[item]  # data is a 2D array# get the rows and columns of the data
rows    = data.shape[0]
columns = data.shape[1]# used to store all the numbers of the digits
lines = ''# convert all the cells into lines of values separated by commas
for r in range(rows):
    print(data[r])
    lines += ','.join([f'{i}' for i in data[r]]) + "\n"# write the lines to a csv file
with open('mnist.csv','w') as file:
    file.write(lines)
```

如果您打开保存的 **mnist.csv** 文件，您将看到以下内容:

![](img/274cdedd3020f6fec4f2a388e4e1776f.png)

更好的可视化方法是使用 Excel 打开它:

![](img/ebce448a236e8a158cf527f42c002994.png)

您现在可以非常清晰地看到，这个数据集表示数字“2”。

# 将滤镜应用于图像

为了直观显示筛选器的工作方式，让我们使用 Excel 并创建一个新的工作表。

> 我已经把最终的电子表格放在[https://bit.ly/2QVLnSS](https://bit.ly/2QVLnSS)供下载。

首先，用以下值创建一个 28x28 的网格(我们稍后将使用 MNIST 数字的数据；现在我要给你看一些更容易理解的东西):

![](img/2256a4b94b28fe0e4327c89e32c23699.png)

假设 28x28 网格中的每个值代表一种颜色(255 代表白色，0 代表黑色)。

接下来，创建另一个 28x28 网格，其值是通过将第一个网格中的每个值除以 255 获得的:

![](img/8fc212d328a88eae81c04268013a2b71.png)

接下来，我们创建一个代表过滤器(内核)的 3x3 网格:

![](img/25248396f352932fe3595eff66ee3012.png)

过滤器代表我们在图像中寻找的模式类型，其中 1 代表白色，-1 代表黑色。在上面的过滤器中，我在图像中寻找一个垂直边缘，颜色从**白色变为黑色**，就像这样:

![](img/439fd047faddb6d73db730a5df7fa714.png)

将过滤器应用于网格只是将过滤器中的每个值与网格中的相应值相乘:

![](img/f598bae6944e81d4bf53cdc8364dd57f.png)

过滤器中的每个值都与网格中的相应值相乘，然后求和

![](img/e00be92b6f477179ba63bae4ff96544b.png)

应用于图像的滤镜的值；然后，结果的小数部分被截断

生成的网格就是我们试图寻找的**特征图**。看数值，我们不容易知道特征图的意义。因此，让我们添加一些颜色编码到我们的原始图像和特征地图，以便我们可以清楚地看到我们在寻找什么。

对于这个图像网格，我们希望通过选择整个网格，然后选择**格式|条件格式…** 来应用颜色:

![](img/df2e9cbe948136f76253f7853adbb5d3.png)

在**管理规则**窗口中，点击对话框左下角的+按钮:

![](img/2f92f1768aa6ef3d68809de22c919507.png)

如下设置颜色，然后点击**确定**两次:

![](img/7a76051ffc0213f1db27761cbe8b513e.png)

网格现在看起来像这样:

![](img/6c18ca6ba7375c24db53a8e56a36c235.png)

我们的图像只是一个中间有一个黑色矩形的白色图像

从图像中可以看到，图像中有两条边，一条从白到黑，另一条从黑到白:

![](img/d80fdfb7c20843e00605463fd878b133.png)

现在让我们也对我们的特征图进行颜色编码(应用了过滤器的图像):

![](img/147c82c10ddeb0ea7e2ff6a42c091cf3.png)

您现在应该看到以下内容:

![](img/2ce6af77245766c21abbc5ae263b7608.png)

基于我们的过滤器，白色的列是我们正在寻找的(记住我们正在寻找颜色从**白色变为黑色**的边缘)。如果我们现在更改过滤器，寻找从**黑色变为白色**的垂直边缘，那么输出将如下所示:

![](img/f5a17f62630c167fced09782ff70f8a0.png)

水平边缘怎么样？嗯，如果您更改过滤器以寻找水平边缘，您将一无所获(这并不奇怪):

![](img/2274079d54da3f712f067928aea0ee76.png)

现在，您可以看到过滤器如何处理 MNIST 数据集了。将您在上一节中提取的数据(数字“2”)粘贴到工作表中:

![](img/50e551549f6826175fbdf7400e22f833.png)

标准化和颜色编码的图像现在看起来如下:

![](img/6726a9ea19fbfac92e9800786a8aa71a.png)

如果您想检测*水平边缘*，请使用以下过滤器:

![](img/f6523ed931d9ebf1b855b22933b035dc.png)

现在，图像中的所有水平边缘都高亮显示:

![](img/755fb875d4ef52e8c6520665045f74d2.png)

使用另一个例子(对于数字“6”):

![](img/48abeb5efbc284f81ea6ef4a48c93618.png)

您可以像这样检测所有垂直边缘:

![](img/62408e9bedfefd2dc6ce127fc1e36b7e.png)

您也可以检测水平边缘:

![](img/b494eedbf5e3c155bd6352ce035a5260.png)

每个卷积层中各种滤波器的组合使得 CNN 中的预测成为可能。

# 自己试试吧

了解过滤器工作原理的最佳方式是亲自尝试。在[https://bit.ly/2QVLnSS](https://bit.ly/2QVLnSS)下载电子表格，使用不同的过滤值，例如:

![](img/0c528e55ba8d7ecaa5ee8350630d0806.png)

此外，在 MNIST 数据集中尝试不同的数字。当你这样做的时候，试着使用一些除了数字以外的其他图像！