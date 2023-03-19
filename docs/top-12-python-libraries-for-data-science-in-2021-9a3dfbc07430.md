# 2021 年数据科学的 12 大 Python 库

> 原文：<https://towardsdatascience.com/top-12-python-libraries-for-data-science-in-2021-9a3dfbc07430?source=collection_archive---------14----------------------->

## 数据科学

## 这些库是启动您的数据科学之旅的良好起点。

![](img/748f31cabc3fd926dbd3173b0220679c.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

无论您是初学者还是寻求跟上潮流的经验丰富的老手，这 12 个 Python 库都是您 2021 年数据科学工具包中绝对需要的工具。请记住，这个列表并不详尽，所以如果我遗漏了你最喜欢的库，请在下面的评论中添加！

# 数据挖掘

*注意:* *从网上抓取数据时，请在抓取前检查你的数据来源的条款和准则。遵守数据源可能拥有的所有许可和版权规则非常重要。*

## 1.Scrapy

Scrapy 是 Python 开发人员最流行的工具之一，他们希望从 web 上抓取结构化数据。Scrapy 非常适合构建能够从任何格式的网页中收集结构化数据的网络爬虫，这使得它成为一个收集数据的优秀工具。

## 2.美丽的声音

另一个收集和组织网络数据的伟大的库， [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 使抓取网站变得容易。BeautifulSoup 非常适合使用特殊字符的网页，因为在收集 web 数据时，您可以轻松地将不同的编码格式传递给它的函数。

## 3.要求

你可以说我过时了，但是在收集基于网络的数据，尤其是从 API 收集数据时，没有什么比得上 [requests](https://realpython.com/python-requests/) 库。Requests 使得在简单的一行解决方案中与 API 和其他 HTML 源进行交互变得容易。

# 数据处理

## 4.熊猫

[Pandas](https://pandas.pydata.org/docs/) 是一个开源库，是使用最广泛的数据科学库之一，GitHub repo 上有超过 2300 名贡献者，这个库不会很快消失。Pandas 使数据处理和争论变得容易，它从大量不同的来源(如 CSV、SQL 或 JSON)获取数据，为您提供强大的操作功能，如处理丢失的数据、输入丢失的数据文件和操作列，甚至提供一些基本但非常有用的可视化功能。

## 5.Numpy

如果你想对你的数据集做任何高等数学运算，那么你需要导入 [Numpy](https://numpy.org/) 库。在深度学习和机器学习中大量使用，这对任何计算量大的算法和分析都是绝对必要的。Numpy 的多维数组也使得复杂的问题比标准列表简单得多。

## 6.Scipy

[Scipy](https://www.scipy.org/) 源自 Numpy，也可以进行大量不同的数学复杂计算。如果您希望进行多维图像处理、微分方程或线性代数，这个库将使您的生活变得特别容易。

# 机器学习

## 7.克拉斯

现在进入深层的东西。 [Keras](https://keras.io/) 已经成为深度学习的首选库，特别是在神经网络方面。它建立在 TensorFlow 之上，但更加用户友好，使用户能够使用他们的深度学习 API 进行轻量级和快速的实验。

## 8.张量流

[TensorFlow](https://www.tensorflow.org/) 之所以这样命名，是因为它使用了多维数组，称之为 tensors。所有大型科技公司都将 TensorFlow 用于其神经网络算法是有原因的:这个库几乎可以做任何事情。优秀的用例包括情感分析、语音识别、视频检测、时间序列分析和面部识别等。TensorFlow 是由谷歌开发的，所以这不会很快消失。

## 9.PyTorch

TensorFlow 使用静态图， [PyTorch](https://pytorch.org/) 可以随时定义和操作图形，使其更加灵活。尽管 PyTorch 比 TensorFlow 更接近 Pythonic，但后者更受欢迎，因此在它上面更容易找到资源。如果你正在寻找比 TensorFlow 更灵活、更容易掌握的东西，这个库(由脸书开发)是一个很好的资源。

# 形象化

## 10.散景

我见过的一些由 Python 代码创建的最令人惊叹的可视化效果是使用[散景](https://docs.bokeh.org/en/latest/index.html)库开发的。Bokeh 提供了交互式可视化选项，可以很容易地在 Flask 等其他 Python web 工具中显示，这使得它成为向广大受众共享可视化的一个很好的选项。

## 11.海生的

[Seaborn](https://seaborn.pydata.org/) (至少在数据科学方面)最好的特性是关联图，它可以非常容易地直观地发现数据集所有维度之间的关联。Seaborn 构建在 MatPlotLib 之上，所以它很容易访问，是快速可视化数据的一个很好的工具。

## 12.Plotly

[Plotly](https://plotly.com/python/) 是另一个创建高级交互式可视化的伟大工具，它非常适合做探索性分析和显示结果。真的没有什么是 Plotly 做不到的，但是某些类型的可视化在这个库中比其他替代方案更加用户友好。最终，在为您的项目选择最佳可视化库时，这只是一个品味和熟悉程度的问题。

该列表绝非详尽无遗，但对于任何踏上数据科学之旅的人来说，这都是一个很好的起点。你最喜欢的数据科学 Python 库有哪些？请在下面的评论中让我知道你在做什么！

又及:你有没有厌倦过等待熊猫把你的数据上传到你的数据库？通过使用原生 SQL 命令，你可以将它们 [**的加载速度提高 10 倍**。](/upload-your-pandas-dataframe-to-your-database-10x-faster-eb6dc6609ddf)