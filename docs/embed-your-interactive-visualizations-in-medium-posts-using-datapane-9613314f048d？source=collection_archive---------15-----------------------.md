# 使用 datapane 将您的交互式可视化嵌入到中型帖子中

> 原文：<https://towardsdatascience.com/embed-your-interactive-visualizations-in-medium-posts-using-datapane-9613314f048d?source=collection_archive---------15----------------------->

![](img/d5f1a464e496c8cd03205b4a1cf5d6aa.png)

照片由[埃米尔·佩伦](https://unsplash.com/@emilep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

## 以及如何以编程方式为风险承担者创建报告

还记得你创造了那个令人敬畏的互动情节，并且很难把它嵌入到你的文章中吗？或者你想让读者看到数据框，但不一定只是粘贴一张`df.head()`的截图？这样的奋斗不仅限于写文章。你也可以在准备任何类型的报告时遇到它们，这些报告可以使用漂亮的 viz 来展示你的分析结果，但同时不需要大量的时间和精力来准备。我最近遇到了一个 Python 库，可以帮助你解决这些问题。

# 关于数据面板

`datapane`是一个 Python API/库，对于想要分享分析结果的人来说特别方便。这并不局限于在中型文章(或其他网站)中嵌入可视化，因为您也可以轻松地生成 HTML 报告等。还记得您准备了某种分析(通常是临时的)，将笔记本的 HTML 版本发送给涉众(隐藏代码，这样他们就不会不知所措)，然后他们回来向您添加更多内容或更改过滤器并重新共享结果的情况吗？然后同样的操作重复多次，导致几十个 HTML 文件被发送过来。绝对不是愉快的经历…

在这种情况下，`datapane`的一个很好的特性可以成为救命稻草，那就是你可以创建一份报告，与他人(同事、利益相关者等)分享。)然后万一有东西需要更新或刷新，你可以自己做，只需重新发布。不再重复发送相同的报告。最重要的是，这些报告可以是交互式的，因此利益相关者可以更深入地研究数据，自己做一些探索。

在`datapane`中，您以编程的方式创建报告，也就是说，像在 Jupyter 笔记本中一样用 Python 运行您的分析，准备输出(数据帧或图表),然后将所有这些构建模块与一些描述组合在一起，形成一个漂亮的报告。为了使我们的生活更容易，`datapane`允许包装日常分析中非常常见的对象，例如`pandas`数据帧、来自不同库的图(`matplotlib`、`plotly`、`bokeh`、`altair`等)。)、降价文本和乳胶配方。

我认为这对于介绍来说已经足够了，让我们开始编码并展示结果。

# 运行中的数据面板

我不需要努力寻找`datapane`可以派上用场的分析例子。在我之前的文章中，我简要介绍了一个简单的交易策略。与此同时，我下载了网飞的股票价格，并使用`plotly`创建了一个蜡烛图。虽然我的笔记本中的情节很好，也很有互动性，但文章中的截图并非如此。

在你说任何事情之前，我知道对于`plotly`来说，发布图表并将其作为嵌入对象在媒体上共享也是相对容易的。然而，对于其他流行的绘图库来说，情况可能并非如此。最后我展示的不仅仅是情节。但是我们不要想太多。和往常一样，我们从加载所需的库开始。

由于`datapane`是一个平台，你需要在这里创建一个账户[。对于简单的情况，比如你将在本文中看到的报告，免费帐户已经足够了。创建帐户后，我们需要连接到他们的 API。我们可以在终端中运行以下命令:](https://datapane.com/accounts/signup/)

```
datapane login --server=https://datapane.com/ --token=API_TOKEN
```

或者在笔记本上运行下一行:

```
dp.login(API_TOKEN)
```

其中 API 密钥是您连接到`datapane`的个人密钥，您可以在您的设置中找到它。然后，我们将下载与上一篇文章相同的数据集— Netlix 从 2019 年到 2021 年调整后的股票价格。

这次我将跳过粘贴`df.head()`的结果和一个简单的线图。相反，让我们看看如何使用`plotly`绘制一个蜡烛图。

对于那些感兴趣的人来说，有一个很好的 Python 库`cufflinks` (repo [here](https://github.com/santosjorge/cufflinks) )，这使得创建烛台图表变得更加容易。我在另一篇文章中也提到过。

## 第一次报告

我们就从最简单的报道开始，也就是刚刚我们在上面创造的情节。为此，我们创建了一个`dp.Report`类的实例，并提供构建块作为参数。首先，我们使用`dp.Text`来添加标题(使用 Markdown)，但是我们也可以使用相同的对象来提供对报告中发生的事情的更多描述。然后，我们将存储在`fig`中的情节嵌入到`dp.Plot`中，并为其添加一个漂亮的标题。

你可以在下面看到结果。

我们使用的第一种方法(`save`)存储报告的 HTML 版本，并在默认浏览器中打开它。第二个— `publish` —正如它的名字所表明的那样，即将它发布到 Datapane，从那里我们可以复制链接并粘贴到我们的文章中。我们还必须指定报告是公开的还是保密的。

提示:在`datapane,`中有不同类型的报告，每种报告适合显示不同类型的信息。在本文中，我们使用默认的，但是你可以在这里阅读更多关于它们的内容。

如您所见，创建第一个报告非常容易。让我们添加一些其他功能！

## 第二次报告

这一次，我们在标题下添加了一个额外的描述，并包含了两个表，呈现了数据集的前/后五个观察值。为此，我们使用`dp.Table`类，直接捕获我们准备好的数据帧的输出。

## 第三次报告

最后，我们更进一步，创建了一个两页的报告。为了指定页面，我们使用了`dp.Page`类，直接传递给通用的`dp.Report`类。第一页将与上面的报告非常相似，我们只是使用`dp.Group`助手类将两个表存储在不同的列中，以便更好地呈现。在使用`dp.Page`时，我们还将页面上想要使用的所有元素作为列表传递给`blocks`参数。

在第二页上，我们使用`dp.DataTable`类嵌入整个数据集。让事情变得更好的是，观众可以使用 SandDance 或 Pandas Profiling 自动探索它。只需单击报告的*数据集*页面上的按钮，所有这一切就都完成了！

如您所见，我们用几行代码在笔记本上准备的结果的基础上准备了一个漂亮的交互式报告。这种形式在与其他人合作时肯定更容易使用，尤其是非技术观众。

# 外卖食品

*   `datapane`是一个平台，使您能够轻松共享您以编程方式生成的报告。
*   报告可以包括文本、情节(也可以是交互式的)和整个数据集，观众可以使用熊猫概况等工具对其进行进一步分析。
*   这些报告可以嵌入到 Medium、Reddit、Confluence、您自己的网站等。

你可以在我的 [GitHub](https://github.com/erykml/medium_articles/blob/master/Misc/datapane.ipynb) 上找到本文使用的代码。此外，欢迎任何建设性的反馈。你可以在推特上或者评论里联系我。

如果您喜欢这篇文章，您可能还会对以下内容感兴趣:

[](/beautiful-decision-tree-visualizations-with-dtreeviz-af1a66c1c180) [## 用 dtreeviz 实现漂亮的决策树可视化

### 改进绘制决策树的旧方法，永不回头！

towardsdatascience.com](/beautiful-decision-tree-visualizations-with-dtreeviz-af1a66c1c180) [](/the-simplest-way-to-create-an-interactive-candlestick-chart-in-python-ee9c1cde50d8) [## 用 Python 创建交互式烛台图表的最简单方法

towardsdatascience.com](/the-simplest-way-to-create-an-interactive-candlestick-chart-in-python-ee9c1cde50d8) [](/5-types-of-plots-that-will-help-you-with-time-series-analysis-b63747818705) [## 有助于时间序列分析的 5 种绘图类型

### 以及如何使用 Python 快速创建它们！

towardsdatascience.com](/5-types-of-plots-that-will-help-you-with-time-series-analysis-b63747818705)