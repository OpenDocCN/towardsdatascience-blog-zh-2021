# 如何在网站输入上轻松运行 Python 脚本

> 原文：<https://towardsdatascience.com/how-to-easily-run-python-scripts-on-website-inputs-d5167bd4eb4b?source=collection_archive---------4----------------------->

## 这是我建立的一个网站的演示，它将动态分析文本情感

![](img/384d22fc5949e283c37d41b8460a27ed.png)

蒂莫西·戴克斯在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

所有数据科学家都知道计算机在分析文本方面有多好，但大多数其他人不知道。所以作为一个普通人，我想我会给他们他们不知道他们需要的东西——一个[网站](http://www.ebookanalyzer.com)，在那里他们可以分析他们自己的电子书。

你可以在这里阅读网站[的用处](https://jamesasher4994.medium.com/this-free-website-uses-algorithms-to-analyse-the-sentiment-of-your-writing-26ca9164ab0e)。但是这篇文章更多的是针对你们这些技术人员，集中在如何在用户输入上运行 python 脚本并产生输出——而不是将输入保存在任何地方。

这是非常有用的，因为它是完全自动化的，只要服务器不崩溃(他们可能会——我不知道),那么它应该会自己运行。没有任何维护。

## 背景

所以游戏的目的是制作一个[网站](http://www.ebookanalyzer.com)，它接受用户输入，在上面运行 python 脚本，然后以一种可用的方式返回给用户。事实证明，在线运行 Python 脚本并不容易，我在网上也没有找到太多的帮助，但下面是我找到的全部内容。

我发现最好的方法是使用[Flask](https://flask.palletsprojects.com/en/2.0.x/)——一个用 Python 编写的微型 web 框架。Flask 和 Django 一起，通常用于 web 开发。因为它们是用 Python 编写的，所以很容易将 Python 脚本集成到您的路由中。

我以前写过一篇非常受欢迎的关于如何显示 Matplotlib 图和 Panda 的数据帧的[文章——如果听起来有用，那么请检查一下。](/how-to-easily-show-your-matplotlib-plots-and-pandas-dataframes-dynamically-on-your-website-a9613eff7ae3)

> 提醒一下，这里的目标是从 HTML 表单中获取用户输入，在其上运行 python 脚本，并在全自动过程中输出结果。

## 建立

这个项目的设置非常简单。我们首先必须从 Flask 导入所有需要的包，如下所示( **Line 2** )。然后我们初始化应用程序(**第 5 行**)——你可以随便叫它什么，但习惯上使用邓德的“名字”。只是不要叫它 Flask，因为这将与 Flask 本身相冲突。最后，我们使用 route decorator 告诉 Flask 哪个 URL 应该调用我们的函数。在这种情况下，它只是'/' —所以我们的初始页面。

保持 debug = True 打开，以便开发时自动重新加载。但是不要忘记在部署时删除它。

可以看到，我们已经返回了“render_template”函数，这是任何 Flask 应用程序都必须的。它从 templates 文件夹(Flask 需要一个特定的 templates 文件夹)返回一个 HTML 文件，并将其呈现在指定的页面上。这是我们需要的 HTML。

可以看出，这是一个非常简单的 HTML 页面，您可能已经看过一百次了。**然而，第 11–15 行**是我们感兴趣的。在第 11 行，我们指定了表单的开始，并在第 15 行关闭它。在表单中，我们有三个 div 标签(**第 12、13 行，& 14** )。**第 12 行**仅仅是表单标题，只不过是文本，**第 13 行**是文件选择，**第 14 行**是提交按钮。正如您在这里看到的，我们已经将输入类型指定为“file ”,并特别声明。txt 文件。

这里声明“enctype= 'multipart/form-data”很重要，因此 Flask 知道如何对输入进行编码。更重要的是，不要忘记声明 method = "POST ",这样 Flask 就知道请求存储在表单中的信息。

## 对输入运行 Python 脚本。

现在我们已经有了网站的基本框架，我们如何在输入上运行 python 脚本，然后向用户显示结果呢？

我们需要做的是，创建另一个仅在接收 post 请求时运行的装饰器。

这正是下面要做的。您可以将它直接放在原始的 route decorator 下面。

因此，在这里，当用户单击提交按钮时，这个函数被触发，因为它在使用 POST 方法时被激活。**第 5 行**然后使用请求库选择输入的文件。然后，您可以像在 python 中一样，简单地在这个变量上运行 Python 脚本。一旦你完成了操作，你就可以以多种方式之一将变量返回给用户。您可以像我上面一样使用 Response 方法，或者您可以将它传递给一个变量，并再次使用 render_template()在另一个 HTML 模板中直接输出它。可能性是无限的。

你可以用多种方式托管你的网站，但我更喜欢 PythonAnywhere。如何做到这一点的指导可以在我的文章[的结尾这里](/how-to-easily-show-your-matplotlib-plots-and-pandas-dataframes-dynamically-on-your-website-a9613eff7ae3)看到。

如上所述，你可以在 www.ebookanalyzer.com 的[看到一个工作实例。](http://www.ebookanalyzer.com)

我当然不是网络开发人员，所以请原谅我的丑陋。

也请让我知道是否有任何错误，或者你可以看到任何改进或更优雅的解决方案。

```
If I’ve inspired you to join medium I would be really grateful if you did it through this [link](https://jamesasher4994.medium.com/membership) — it will help to support me to write better content in the future.If you want to learn more about data science, become a certified data scientist, or land a job in data science, then checkout [365 data science](https://365datascience.pxf.io/c/3458822/791349/11148) through my [affiliate link.](https://365datascience.pxf.io/c/3458822/791349/11148)
```

如果你喜欢这篇文章，下面是我写的一些类似的文章:

</how-to-easily-show-your-matplotlib-plots-and-pandas-dataframes-dynamically-on-your-website-a9613eff7ae3>  </how-to-analyze-survey-data-in-python-c131764ea02e>  </how-to-predict-something-with-no-data-and-bonsai-trees-b6ebc6471da3> [## 如何预测没有数据的事物——还有盆景树

towardsdatascience.com](/how-to-predict-something-with-no-data-and-bonsai-trees-b6ebc6471da3) 

干杯，

詹姆斯