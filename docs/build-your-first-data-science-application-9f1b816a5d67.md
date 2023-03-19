# 构建您的首个数据科学应用

> 原文：<https://towardsdatascience.com/build-your-first-data-science-application-9f1b816a5d67?source=collection_archive---------8----------------------->

## 七个 Python 库，打造您的第一个数据科学 MVP 应用

![](img/a04df1d8f3ea04851c2a7593339a5b2b.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

要制作我的第一个数据科学应用程序，我需要学习什么？web 部署呢？web 应用需要学习 Flask 或者 Django 吗？做深度学习应用需要学习 TensorFlow 吗？我应该如何制作我的用户界面？我也需要学习 HTML、CSS 和 JS 吗？

当我开始学习数据科学的旅程时，这些是我脑海中一直存在的问题。我学习数据科学的目的不仅仅是开发模型或清理数据。我想开发人们可以使用的应用程序。我在寻找一种快速制作 MVP(最小可行产品)的方法来测试我的想法。

如果你是一名数据科学家，并且想开发你的第一个数据科学应用程序，在本文中，我将向你展示 7 个 Python 库，你需要学习它们来开发你的第一个应用程序。我相信你已经知道其中一些，但我在这里提到它们是为了那些不熟悉它们的人。

![](img/dac369f3916626b5504315abd86e9715.png)

照片由 [Med Badr Chemmaoui](https://unsplash.com/@medbadrc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/design?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

# 熊猫

数据科学和机器学习应用都是关于数据的。大多数数据集是不干净的，它们需要为你的项目进行某种清理和操作。Pandas 是一个库，可以让你加载、清理和操作你的数据。您可以使用 SQL 之类的替代方法进行数据操作和数据库管理，但是 Pandas 对于您这样一个想成为开发人员(或者至少是 MVP 开发人员)的数据科学家来说要容易得多，也更适用。

点击了解更多关于熊猫[的信息。](https://pandas.pydata.org/)

# Numpy

在许多数据科学项目中，包括计算机视觉，数组是最重要的数据类型。Numpy 是一个强大的 Python 库，允许您使用数组、操作数组，并高效地对数组应用算法。学习 Numpy 对于使用我稍后提到的其他一些库是必要的。

点击安装并了解更多关于 Numpy [的信息。](https://numpy.org/)

# SciKitLearn

这个库是许多类型的机器学习模型和预处理工具的工具包。如果你正在做一个机器学习项目，有一点点可能你不需要 SciKitLearn。

安装并了解更多关于 SciKit 的信息-点击了解[。](https://scikit-learn.org/stable/)

# 喀拉斯或皮托尔奇

神经网络，尤其是深度神经网络模型，是数据科学和机器学习中非常流行的模型。许多计算机视觉和自然语言处理方法都依赖于这些方法。几个 Python 库提供了对神经网络工具的访问。TensorFlow 是最著名的一个，但我相信初学者很难从 TensorFlow 入手。而是建议你学习 Keras，它是 Tensorflow 的一个接口(API)。Keras 使您作为一个人可以轻松测试不同的神经网络架构，甚至构建自己的架构。另一个最近越来越流行的选择是 PyTorch。

安装并了解更多关于 [Keras](https://keras.io/) 和 [PyTorch](https://pytorch.org/) 的信息。

# 要求

如今，许多数据科学应用程序都在使用 API(应用程序编程接口)。简而言之，通过 API，您可以请求服务器应用程序允许您访问数据库或为您执行特定的任务。例如，Google Map API 可以从您那里获得两个位置，并返回它们之间的旅行时间。没有 API，你必须重新发明轮子。Requests 是一个与 API 对话的库。如今，不使用 API 很难成为一名数据科学家。

点击安装并了解更多请求[。](https://requests.readthedocs.io/en/master/)

# Plotly

绘制不同类型的图表是数据科学项目的重要组成部分。虽然 Python 中最流行的绘图库是 matplotlib，但我发现 Plotly 更专业、更易用、更灵活。Plotly 中的绘图类型和绘图工具非常多。Plotly 的另一个优点是它的设计。与具有科学外观的 matplotlib 图形相比，它看起来更加用户友好。

点击安装并了解更多关于 Plotly [的信息。](https://plotly.com/)

# ipywidgets

谈到用户界面，你必须在传统外观的用户界面和基于网络的用户界面之间做出选择。您可以使用像 PyQT 或 TkInter 这样的库来构建传统外观的用户界面。但是我的建议是制作可以在浏览器上运行的 web 外观的应用程序(如果可能的话)。要实现这一点，您需要使用一个在浏览器中为您提供一组小部件的库。ipywidgets 为 Jupyter Notebook 提供了丰富的小部件。

点击安装 ipywidgets [并了解更多信息。](https://ipywidgets.readthedocs.io/en/stable/)

# Jupyter 笔记本和瞧

创建第一个数据科学应用程序需要学习的最后一些工具是最简单的。第一，ipywidgets 在 Jupyter 笔记本中工作，你需要使用 Jupyter 来制作你的应用。我相信你们中的许多人已经在使用 Jupyter 笔记本进行建模和探索性分析。现在，把 Jupyter 笔记本想象成前端开发的工具。还有，你需要使用 Voila，一个你可以启动的第三方工具，它隐藏了 Jupyter Notebook 的所有代码部分。当您通过 Voila 启动 Jupyter 笔记本应用程序时，它就像一个 web 应用程序。您甚至可以在 AWS EC2 机器上运行 Voila 和 Jupyter 笔记本，并从互联网访问您的简单应用程序。

点击安装并了解更多关于[的信息。](https://github.com/voila-dashboards/voila)

# 摘要

使用我在本文中提到的 7 个库，您可以构建人们使用的数据科学应用程序。通过成为使用这些工具的大师，您可以在几个小时内构建 MVP，并与真实用户一起测试您的想法。后来，如果您决定扩展您的应用程序，除了 HTML、CSS 和 JS 代码之外，您还可以使用更专业的工具，如 Flask 和 Django。

关注我在[媒体](https://tamimi-naser.medium.com/)和[推特](https://twitter.com/TamimiNas)上的最新报道。