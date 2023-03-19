# 每个数据科学家都必须知道的 10 大 Python 库

> 原文：<https://towardsdatascience.com/top-10-python-libraries-every-data-scientist-must-know-9260006df1ea?source=collection_archive---------2----------------------->

## 和大量免费资源来学习它们

![](img/82471e582ea272f20aa4bbfa837ea804.png)

照片由来自 [Pexels](https://www.pexels.com/photo/man-with-hand-on-temple-looking-at-laptop-842554/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[安德里亚·皮亚卡迪奥](https://www.pexels.com/@olly?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

数据科学很难。作为初学者，即使是为了解决最基本的任务，你也必须学习一些库。雪上加霜的是，库不断变化和更新，而且几乎总是有更好的工具来完成这项工作。

不知道使用哪种工具的问题很容易理解——它会导致完全失败或不能以最佳方式完成任务。同样危险的是对库不够了解。你最终会从零开始实现算法，完全不知道已经有了一个这样的函数。两者都耗费你的时间、精力和金钱。

如果你发现自己被数据科学图书馆淹没了，那你来对地方了。本文将向您展示启动您的数据科学之旅的 10 个基本要素。

了解学习数据科学需要时间是至关重要的。你不可能一蹴而就。看书看视频是个好的开始，但解决自己关心的问题才是唯一的长久之道。

# Numpy

这是显而易见的。Numpy 代表*数字 Python* ，是处理数组、科学计算和一般数学的最佳库。

Numpy 附带了线性代数(`np.linalg`)、傅立叶变换(`np.fft`)和几乎所有相关数学的函数。它比传统的 Python 列表要快得多。当以速度和效率为目标时，总是使用它。

因此，在决定从头开始实现特征分解算法之前，请尝试一下 Numpy。

最佳起点:

*   [初学者的绝对基础知识](https://numpy.org/doc/1.21/user/absolute_beginners.html)
*   [线性代数教程](https://numpy.org/doc/1.21/user/tutorial-svd.html)
*   [1 小时视频速成班](https://www.youtube.com/watch?v=8Y0qQEh7dJg)

# 熊猫

想想类固醇的作用。Pandas 是一个用于数据分析的基本库。大多数时候，你只需要为机器学习加载和准备数据集。它与 Numpy 和 Scikit-Learn 很好地集成在一起。

Pandas 基于两个基本的数据结构— `Series`和`DataFrame`。第一个非常类似于数组，而后者只是以表格形式呈现的`Series`对象的集合。

一条建议——花尽可能多的时间学习熊猫。它为操纵数据、填充缺失值甚至数据可视化提供了无尽的选项。不可能很快学会，但是一旦学会，分析可能性是无穷无尽的。

最佳起点:

*   [官方入门指南](https://pandas.pydata.org/docs/getting_started/index.html#getting-started)
*   埃尔南·罗哈斯的一套课程
*   [2 小时视频速成班](https://www.youtube.com/watch?v=PcvsOaixUh8)

# Plotly

在一个静态和可怕的数据可视化的世界里，有一个库脱颖而出——很明显。它比 Matplotlib 领先几光年，Matplotlib 是您可能首先学习的可视化库。

普洛特利做得更好。默认情况下，可视化是交互式的，调整的选项是无穷无尽的。可视化对于出版物和仪表板都是现成的。他们的 [Dash](https://dash.plotly.com) 库就是一个完美的例子。我无数次使用它来围绕数据或机器学习模型构建交互式仪表盘。

最佳起点:

*   [Python 官方入门指南](https://plotly.com/python/getting-started/)
*   [1.5 小时的视频速成班](https://www.youtube.com/watch?v=GGL6U0k8WYA)

# 美丽的声音

偶尔，你会需要一个非常具体的数据集。你不会在网上以表格的形式找到它，但是你知道数据存在于某个地方。问题是——它列在网站上，格式不对(想想亚马逊上的产品列表)。

这就是 BeautifulSoup 的用武之地。这是一个从 HTML 和 XML 文件中提取数据的库。你必须*下载*带有`requests`库的 HTML，然后使用 BeautifulSoup 解析它。

网页抓取是一个灰色地带。有些网站允许，有些不允许。如果你的请求太多，被网站屏蔽是很常见的。总是确保事先检查`robots.txt`文件(更多信息[在这里](https://developers.google.com/search/docs/advanced/robots/intro))。检查您想要抓取的网站是否有可用的 API。那样的话，刮就没有意义了。

最佳起点:

*   [官方文件](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
*   [我的刮痧指南(上)——刮痧正文](/no-dataset-no-problem-scrape-one-yourself-57806dea3cac)
*   [我的刮痧指南(下)——刮痧图片](/scraping-continued-download-images-with-python-6122623f3d82)

# 达斯克

Dask 与 Pandas 非常相似，但提供了一个重要的优势——它是为并行性而构建的。Numpy 和熊猫不是你的大数据集的好朋友。默认情况下，不可能将 20 GB 的数据集放入 16 GB 的 RAM 中，但 Dask 可以做到。

小数据集上 Numpy 和熊猫没毛病。当数据集变得比可用的 RAM 大，并且计算时间变长时，事情就会变得不可收拾。Dask 可以将数据分割成块并并行处理，解决了这两个棘手问题。

最好的部分是——你不必从头开始学习一个新的库。Dask 中的数组和数据帧与 Numpy 和 Pandas 中的几乎具有相同的功能，所以它应该感觉就像在家里一样。

还可以用 Dask 训练机器学习模型。这是一个最适合大型数据集的三合一包。

最佳起点:

*   【Dask 官方教程
*   [4 小时视频教程](https://www.youtube.com/watch?v=EybGGLbLipI)
*   [我的平行运行 Python 指南](/dask-delayed-how-to-parallelize-your-python-code-with-ease-19382e159849)

# 统计模型

Statsmodels 允许您训练统计模型并执行统计测试。它与其他用于统计建模的 Python 库略有不同，因为它与 R 非常相似。例如，您必须使用 R 风格的公式语法来训练线性回归模型。

该库为每个估计器返回一个结果统计的详细列表，使模型易于比较。

这是迄今为止训练时间序列模型的最佳库。它提供了你能想到的所有统计模型——从移动平均线和指数平滑，到季节性 ARIMA 和 GARCH。唯一的缺点是——它不能用深度学习算法训练时间序列模型。

最佳起点:

*   [官方入门指南](https://www.statsmodels.org/stable/gettingstarted.html)
*   [我的时间系列从零开始系列](https://towardsdatascience.com/tagged/time-series-from-scratch)

# sci kit-学习

这是用 Python 进行机器学习的圣杯。该库构建在 Numpy、Scipy 和 Matplotlib 之上，为大多数监督和非监督学习算法提供了实现。它还附带了一套用于数据准备的函数和类，例如缩放器和编码器。

这个图书馆到处都是。你会发现它被用在任何基于 Python 的机器学习书籍或课程中。几乎所有的算法都有相同的 API——你首先要通过调用`fit()`函数来训练模型，然后你可以用`predict()`函数进行预测。这种设计选择使得学习过程更加容易。

该库可与 Pandas 一起开箱即用，因此您可以将准备好的数据帧直接传递给模型。

最佳起点:

*   [官方教程](https://scikit-learn.org/stable/tutorial/index.html)
*   [官方入门指南](https://scikit-learn.org/stable/getting_started.html)
*   [2 小时视频速成班](https://www.youtube.com/watch?v=0B5eIE_1vpU)

# OpenCV

图像是深度学习和人工智能的一大部分。OpenCV 打包了几乎可以做任何事情的工具。可以把它想象成 Photoshop 的非 GUI 版本。

该库可以处理图像和视频，以检测物体、人脸和几乎任何你可以想象的东西。它的效果不如复杂的物体检测算法(想想 YOLO)，但对于计算机视觉的新手来说，这是一个很好的开端。

我不喜欢的一点是语法。不是 Pythonic 的。图书馆用骆驼箱代替蛇箱。例如，函数被命名为`getRotationMatrix2D()`而不是`get_rotation_matrix_2d()`。后者更像 Pythonic，而 prior 看起来更像 Java。这并不妨碍交易，你很快就会习惯的。

最佳起点:

*   [入门指南](https://learnopencv.com/getting-started-with-opencv/)
*   [极客帮极客网站](https://www.geeksforgeeks.org/opencv-python-tutorial/#drawing)
*   [4 小时视频速成班](https://www.youtube.com/watch?v=oXlwWbU8l2o)

# 张量流

深度学习是数据科学的重要组成部分。TensorFlow，再加上高层的 Keras API，可以让你用很少的代码训练深度学习模型。

发展的可能性是无限的。您可以对表格数据集、图像或音频使用神经网络。您还可以构建高度精确的检测和分割模型，或者尝试图像风格转换。

机器学习和深度学习的另一个重要方面是部署。你不希望你的模型闲置在硬盘上。TensorFlow 允许您将模型部署到云、内部、浏览器和设备。所有理由都包括在内。

最佳起点:

*   [官方示例教程](https://www.tensorflow.org/tutorials)
*   [GitHub 上的免费教程](https://github.com/ageron/tf2_course)
*   [7 小时视频速成班](https://www.youtube.com/watch?v=tPYj3fFJGjk)

# 瓶

最后，还有烧瓶。这是一个用于构建 web 应用程序的库。我几乎在每个项目中都使用它，尽管我并不关心 web 开发。在数据科学中，Flask 是围绕机器学习模型构建 web APIs 和应用程序的首选库。

您可以坚持使用 Flask 来开发应用程序，但是我推荐使用 [Flask-RESTful](https://flask-restful.readthedocs.io/en/latest/) 来构建 API。

最佳起点:

*   [官方快速入门指南](https://flask.palletsprojects.com/en/2.0.x/quickstart/)
*   [米格尔·格林伯格的烧瓶大型教程](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)
*   [6 小时视频速成班](https://www.youtube.com/watch?v=Qr4QMBUPxWo)

# 最后的想法

这就是你想要的——每个数据科学家都应该知道的十个库。不需要所有的专家知识，但是你应该知道他们的能力。Numpy、Pandas、Scikit-learn 和任何可视化库都是必须的，你应该学习你认为合适的其他库。

当然，您可以很容易地交换这些库，只要它们实现相同的目标。例如，可以使用 PyTorch 代替 TensorFlow。最终的选择将归结为个人偏好，或者你工作的公司的偏好。

*你有什么想法？你同意这份清单吗，或者你认为还应该增加些什么？*

喜欢这篇文章吗？成为 [*中等会员*](https://medium.com/@radecicdario/membership) *继续无限制学习。如果你使用下面的链接，我会收到你的一部分会员费，不需要你额外付费。*

[](https://medium.com/@radecicdario/membership) [## 阅读达里奥·拉德契奇(以及媒体上数以千计的其他作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@radecicdario/membership) 

# 保持联系

*   在[媒体](https://medium.com/@radecicdario)上关注我，了解更多类似的故事
*   注册我的[简讯](https://mailchi.mp/46a3d2989d9b/bdssubscribe)
*   在 [LinkedIn](https://www.linkedin.com/in/darioradecic/) 上连接