# 使用 Plotly 和 Chartstudio 和 Python 在媒体/网站中嵌入交互式图表

> 原文：<https://towardsdatascience.com/embed-interactive-charts-in-medium-websites-using-plotly-and-chartstudio-with-python-15359c87149f?source=collection_archive---------45----------------------->

## 数据科学|数据可视化

## 使用 Plotly 和 Chartstudio 将您的交互式绘图嵌入媒体、网页和其他平台

![](img/63c7133b95eada370784abaea0ac8e72.png)

照片由[威廉·艾文](https://unsplash.com/@firmbee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> 没有所谓的信息过载。只有糟糕的设计。—爱德华·塔夫特

我们都知道**一张图片胜过千言万语** *，*数据可视化是对信息的可视化总结，使其更容易理解/识别模式和趋势，而不是查看电子表格中的数千行。一个好的数据可视化以精确和简洁的方式表达复杂数据集的含义。交互式数据可视化使理解数据和从数据中发现真知灼见变得更加容易。

本文介绍了如何使用 Plotly 创建不同的交互式绘图，以及如何使用 Chartstudio 在媒体和网站中嵌入交互式数据可视化。

*我们去找 started✨*

# 与 Chartstudio 连接

Chartstudio 是一个在云中托管交互式绘图/图表的平台。它还为 Python、R、MATLAB、Javascript 和其他计算机编程语言开发了开源图形应用编程接口(API)库。Plotly 和 chartstudio 的重要特点是:

*   它制作交互式图形/图表。
*   这些图形以 JavaScript 对象符号 **(JSON)数据格式**存储，以便可以使用其他编程语言(如 R、Julia、MATLAB 等)的脚本读取。
*   图形可以以各种光栅和矢量图像格式导出

让我们看看如何连接 chartstudio，

1.  使用谷歌、脸书、Github 等凭证注册 chartstudio 平台
2.  进入设置> API 密钥，保存用户名和 API 密钥。

以下代码服务于使用 Python 连接 chartstudio 的目的。`chartstudio.tools.set_credentials_file`通过`username`和`api key`与平台连接。

# 嵌入图表

要在媒体/网站上嵌入交互图，我们需要使用 chartstudio 存储交互图。让我们看看如何使用 Chartstusio Python API 保存交互图。

在本文中，我们将使用`gapminder`数据集，该数据集包含一年的大量国家级健康、财富和发展指标。

## 圆形分格统计图表

下面的代码绘制了 2007 年亚洲每个国家的人口饼状图。使用`plotly.express.pie()`方法绘制图表。

创建好情节后，我们必须将其保存到 chartstudio cloud 中。为此，我们使用了`chart_studio.plotly.plot()`方法，即图形对象、文件名和其他参数。执行下面一行代码后，会产生一个超链接，可用于嵌入媒体或其他网站。

```
py.plot(pop, filename = 'Population_Asia', auto_open=True)# Output
[https://plotly.com/~syamkakarla98/41/](https://plotly.com/~syamkakarla98/41/)
```

将链接粘贴到媒体中以嵌入交互式图表。

不仅是饼图，你还可以嵌入三维图，盒图，分布图，多维图，瓷砖地图，等等

## 分类图

您还可以使用 Plotly 创建分类图表，并使用 chartstudio 嵌入它们。以下示例显示了如何为来自`sklearn.datasets`的合成圆数据集创建简单的 KNN 分类图，结果图如下所示。

# 结论

本文帮助读者使用 Plotly 和 Chartstudio API 和 Python 创建交互式绘图，并将它们嵌入到媒体或其他网站中。

# 更多来自作者

[](/comprehensive-guide-to-satellite-imagery-analysis-using-python-1b4153bad2a) [## 使用 Python 进行卫星影像分析的综合指南

### 使用 Python 分析卫星图像的不同方法和机器学习技术，以及实践教程和…

towardsdatascience.com](/comprehensive-guide-to-satellite-imagery-analysis-using-python-1b4153bad2a) [](/hyperspectral-image-analysis-getting-started-74758c12f2e9) [## 超光谱图像分析—入门

### 使用 Python 进行高光谱图像分析的演练。

towardsdatascience.com](/hyperspectral-image-analysis-getting-started-74758c12f2e9) [](/beginners-guide-to-pyspark-bbe3b553b79f) [## PySpark 初学者指南

### 第 1 章:使用美国股票价格数据介绍 PySpark

towardsdatascience.com](/beginners-guide-to-pyspark-bbe3b553b79f) [](/character-recognition-and-segmentation-for-custom-data-using-detectron2-599de82b393c) [## 使用 Detectron2 对自定义数据进行字符识别和分段

### 利用迁移学习的力量

towardsdatascience.com](/character-recognition-and-segmentation-for-custom-data-using-detectron2-599de82b393c) [](/wildfire-detection-using-satellite-imagery-with-python-d534d74d0505) [## 利用卫星影像和 Python 进行野火探测

### 卫星图像与计算机视觉相遇:使用 Python 在澳大利亚进行野火探测的教程

towardsdatascience.com](/wildfire-detection-using-satellite-imagery-with-python-d534d74d0505) 

# **参考文献**

[](https://plotly.com/python/) [## Plotly Python 图形库

### Plotly 的 Python 图形库制作出交互式的、出版物质量的图形。如何制作线图的示例…

plotly.com](https://plotly.com/python/) [](https://chart-studio.plotly.com/feed/#/) [## Plotly |在线制作图表和仪表盘

### 编辑描述

chart-studio.plotly.com](https://chart-studio.plotly.com/feed/#/)