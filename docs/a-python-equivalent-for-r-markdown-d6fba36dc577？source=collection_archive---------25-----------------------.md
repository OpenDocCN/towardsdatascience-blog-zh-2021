# R markdown 的 python 等价物

> 原文：<https://towardsdatascience.com/a-python-equivalent-for-r-markdown-d6fba36dc577?source=collection_archive---------25----------------------->

## R markdown 是分享见解的强大工具。在这篇文章中，我们将展示如何用 python 编写类似的报告。

R markdown 是一个与利益相关者分享见解的强大工具。您可以编写生成图的 R 代码片段。然后可以将其编译成 HTML 或 pdf 文件，与非技术相关人员共享。

这在 python 中没有这么简单。是的，Jupyter 笔记本是与其他开发人员共享分析的一种很好的方式。但是编译成 HTML/pdf，去掉代码片段，对于非技术利益相关者来说已经很不错了，我发现使用 Jupyter 笔记本很笨拙。R markdown 也有很棒的工具来生成[好看的表格](https://bookdown.org/yihui/rmarkdown-cookbook/kable.html)，而不仅仅是绘图。

不过，我一直在和一个 python 重型团队一起工作，所以一直在试图弄清楚如何生成 R markdown 风格的文档。在这篇文章中，我将概述我用 python 生成的 HTML 报告，这些报告看起来很好，足以与非技术利益相关者分享。这个过程使用一些工具。

![](img/4e532c467d3c494a033b74a9020488c9.png)

卢克·切瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 步骤 1:在 HTML 中嵌入绘图

第一步是将图嵌入到一个静态 HTML 中，然后可以与他人共享。一个很好的工具是 [plotly](https://plotly.com/python/) 。Plotly 有一个`[to_html](https://plotly.com/python-api-reference/generated/plotly.io.to_html.html)` [函数](https://plotly.com/python-api-reference/generated/plotly.io.to_html.html)(我的一个了不起的同事发现了这个)，它将把图形写成 HTML 字符串，然后你可以把它写到一个文件中。

Plotly 图形看起来像零件，它们允许用户将鼠标悬停在点上，以查看值是多少；我发现这很受顾客欢迎。

有时用户需要可以复制粘贴的图形。在这种情况下，我推荐使用 python 更标准的绘图库，如 [seaborn](https://seaborn.pydata.org/) 或 [matplotlib](https://matplotlib.org/) 。要将图像嵌入到 HTML 中，而不需要 HTML 引用的单独的图像文件，这就有点复杂了。但是您可以通过将图像编码为 base64 并直接写入您的 HTML 来实现。只需按照这个 [stackoverflow 帖子](https://stackoverflow.com/questions/48717794/matplotlib-embed-figures-in-auto-generated-html)中的说明。

![](img/2407b4dfef3eee09985f13539e7983db.png)

照片由 [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 步骤 2:设置 HTML 文档的布局

太好了，现在你可以在 html 中嵌入图形了，实际上如果你关闭了 plotly 的`to_html`函数中的`full_html`选项，你就可以通过添加字符串来编写任意多的 plotly 图形到 HTML 文件中。

但是布局、评论和数据表呢？这就是我使用 python 的 HTML 模板库的地方——它允许您使用 python 从模板生成静态 HTML 文件。这种技术非常强大，用于 python 后端开发库，如 [flask](https://flask.palletsprojects.com/en/1.1.x/) 和 [django](https://www.djangoproject.com/) 。学习起来很快，但是需要一些 HTML 知识。

我使用 [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) 库来生成我的 HTML 报告(这是 python 中最流行的 HTML 模板库之一)。

# 第三步:让报告看起来漂亮

对于任何以前使用过 HTML 的人来说，你会知道做任何看起来像样的东西都需要很长时间。

我不想在这上面花太多时间，因为我想尽快生成报告。所以我用了 [bootstrap](https://getbootstrap.com/) CSS 库。这个工具可以让你在很短的时间内让你的报告看起来很漂亮(我倾向于做的就是用`<div class="container">`把所有的东西都包起来，可能还会添加一些填充)。

Bootstrap 基本上是一组预建的 HTML 类，可以用来格式化 HTML。例如，让一个 HTML 表格在 bootstrap 中看起来漂亮，就像`<table class="table">`一样简单(在标准 HTML 中，表格看起来很难看)。您可以通过添加`<h1 class="pt-1">`在标题元素的顶部添加填充。

# 思想

正如你所看到的，这是一个比 R markdown 更复杂的过程(如果有人有更好的方法，请联系我们！).但是学习这些工具很有用，尤其是 Jinja 和 bootstrap，它们是 web 开发的标准工具。一旦你学会了这些库，并且准备好了一些预制的模板，这个过程会变得很快。

*原载于 2021 年 5 月 4 日*[*【https://jackbakerds.com】*](https://jackbakerds.com/posts/python-equivalent-rmarkdown/)*。*