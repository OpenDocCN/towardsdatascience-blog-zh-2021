# 如何将 Python Jupyter 笔记本转换成 RMarkdown 文件

> 原文：<https://towardsdatascience.com/how-to-convert-a-python-jupyter-notebook-into-an-rmarkdown-file-abf826bd36de?source=collection_archive---------9----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## Pythonist，试试 RMarkdown，准备好大吃一惊吧！

![](img/29e38ebb9ed3867db261ccc102fba581.png)

[克莱门特·赫拉尔多](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/jupyter-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

**1。简介**

*注 1:如果你想直接进入我的代码，直接进入第 3 节(“从 Jupyter 到 RMarkdown 世界”)*。

*注 2:对于 R 用户，勾选* `*rmarkdown::convert_ipynb()*` *功能。它是官方的，比我在这里展示的 Python 代码更通用。*

几年前，当我开始我的编程之旅时，Python 是我的初恋，它仍然是我最喜欢的语言。然而，在过去的几个月里，由于工作和学业的原因，我越来越喜欢 R。而且我必须承认:R 也超级好玩！我对这两种语言研究得越多，就越确定数据分析和数据科学领域的****多语种道路**** *就是我想要的。*

*事实上，编程语言应该被简单地视为工具，并根据任务和您需要解决的问题的具体环境来使用。事实上，Python 中有一些很好的资源可能是 R 中没有的，反之亦然。我相信 RMarkdown 生态系统是 Python 用户应该考虑了解的资源之一，以便只使用 Markdown 和 Python(以及 Google Colab 中的五行 R 代码)来构建像这样的报告或[另一个](https://bookdown.org/paul/applied-data-visualization/plot-types-basic-2d.html)。*

***2。节目全景***

*我写了一个简短的程序，旨在帮助一个事先没有 R 知识的 Python 用户加入 RMarkdown party。所以让我们开始派对吧！这里的主要目标是使从一个编程世界到另一个编程世界的过渡尽可能容易。*

*这个程序是用 Python 写的，可以将整个 Python Jupyter 笔记本转换成 RMarkdown 文件。然后，我们将打开一个简单的带有 R 内核的 Google 协作笔记本，运行五行 R 代码，并从我们的 RMarkdown 文件中生成任意多种格式的文档:HTML 输出页面、PDF、EPUB、Microsoft Word、Power Point 演示文稿、xaringan 演示文稿、bookdown 文档等等。所有这些都来自同一个源文件，这极大地提高了可重复性和可维护性。*

*我们的 Python 代码将关注如何将 Jupyter 笔记本转换为 Rmarkdown 文件，当使用 R 的`markdown::render()`函数执行时，会生成一个 HTML 输出文件，带有漂亮的侧边栏菜单和布局设计。如果您对其他类型的输出感兴趣，我强烈建议您阅读由谢一辉、J. J. Allaire 和 Garrett Grolemund 以 bookdown code 编写的 [*R Markdown:权威指南*](https://bookdown.org/yihui/rmarkdown/) 的前几章。*

*我必须承认，我还没能在 Python 中找到类似于 RMarkdown 所提供的资源；这就是我通过发表这篇文章，努力让 RMarkdown 更容易被 Python 社区访问的主要原因。如果你知道任何与 RMarkdown 结果相似的 Python 包，请告诉我，我也愿意探索它们。*

***3。从 Jupyter 到 RMarkdown 世界***

*有些用户可能不知道，Jupyter notebook 只是一个 JSON 文件，由某个引擎呈现，然后以我们熟悉的交互式布局(markdown 单元格和 code 单元格)呈现给我们。但是所有的笔记本信息都组织在一个 JSON 结构中，一个有四个键(`cells`、`metadata`、`nbformat`和`nbformat_minor`)的字典存储所有的笔记本数据。*

*`cells`键无疑是最重要的一个，因为它保存着用户写在笔记本上的代码。`cells`键的值是一个字典列表，列表中的每个字典对应于笔记本中的一个单元格。*

*出于教学目的，我创建了一个名为`test_notebook.ipynb` 的简短 Python Jupyter 笔记本，并[将其发送到我的 Github 页面](https://github.com/fabricius1/JupyterToRMarkdown/blob/master/test_notebook.ipynb)。如果你在这里检查[它的原始版本](https://raw.githubusercontent.com/fabricius1/JupyterToRMarkdown/master/test_notebook.ipynb)，你会注意到 JSON 文件是如何构造的，然后你有希望更好地理解我在前面两段中的意思。`test_notebook.ipynb`文件是我们稍后将转换成 RMarkdown 的 Jupyter 笔记本。当然，您可以使用自己的笔记本来进行第一次测试。我只建议你事先做一份原始文件的副本，以防万一。*

*进行这种转换(Jupyter 到 RMarkdown)的代码位于另一个笔记本(`ConvertToRMarkdown.ipynb`)、[、*中，可在下面的 Github 存储库*、](https://github.com/fabricius1/JupyterToRMarkdown/blob/master/ConvertToRMarkdown.ipynb)、*、*中找到。*

*我们将从导入 json 模块开始，用`open()`打开`test_notebook.ipynb`文件，并使用`json.load()`函数将 JSON 对象转换成 Python 字典。我们还将把这个字典保存到`data`变量中。*

*我们需要从`metadata`键获得的唯一信息是笔记本内核中使用的编程语言名称。我们只用一行代码就可以做到:*

*现在我们将注意力转移到 JSON 主字典中的`cells`键，由`data["cells"]`代码调用。列表中的每一项都是一个字典，代表笔记本中的一个单元格。*

*此外，每个单元字典有两个对我们非常重要的键:`cell_type`，在我们分析的情况下，它的值是`"markdown"`或`"code"`；和`source`，它有一个字符串列表作为它的值。这些字符串是在笔记本单元格中一行一行写下的实际代码，稍后我们需要将它们汇编到最终的 RMarkdown 文件中。*

*我使用列表理解将这些信息保存到嵌套列表的结构中，如下所示:*

*一方面，在我们代码中循环的后续*中，我们将把来自`x["cell_type"]`的值保存在一个名为`cell_type`的临时变量中；另一方面，`x["source"]`的值将存储在一个名为`source`的临时变量中。**

*在将`cells`变量定义为上述列表理解的结果后，我们现在将创建强制 RMarkdown 文件头。它由编辑成 YAML 文件的文本组成，包含所有 RMarkdown 配置，比如主标题、作者、日期和文件的输出类型(在我们的例子中，只有`html_document`一个)。*

*在这段代码中，您应该注意到两个重要的方面:*

*   *缩进在 YAML 代码中很重要，类似于我们在 Python 中所做的。因此，如果您在 RMarkdown 文件的顶部错过了 YAML 缩进结构中的一个空格，您可能会在某个时候面临 RMarkdown 呈现问题。如果发生这种情况，回到代码中，尝试发现错误所在，然后修复它。您可以随时在您选择的代码编辑器中打开 RMarkdown 文件，并根据需要进行修改；*
*   *在标题、作者和日期选项中，我插入了一些 HTML 标签，以赋予最终的布局以个人风格。请注意，在这些情况下，管道符号是必需的。*

*这里不是讲授可以添加到 RMarkdown 文件这一部分的所有不同配置的地方。如果你想了解更多这方面的内容，我推荐你去咨询前面提到的[*R Markdown book*](https://bookdown.org/yihui/rmarkdown/)*。关于这些主题和其他主题，你需要的大多数答案都可以通过查阅那本书或 RMarkdown 软件包文档找到。**

*下面是 YAML 部分的代码。您可以直接在为存储这些数据而创建的变量中更改自己的作者和日期信息。我们将把这个长 f 字符串保存到`file_text`变量中。*

*我们几乎没有开始讨论我们的代码，人们已经可以看到它的结束。现在我们只需要遍历笔记本单元格的内容，并将它们添加到`file_text`变量中。然后我们将把这个`file_text`字符串写入一个全新的 RMarkdown 文件，这个文件必须有`.Rmd`扩展名(注意其中的大写 R)。*

*在上面循环的*中，注意我们之前提到的临时变量`cell_type`和`source`最终被创建。`cell_type`值将用于检查当前笔记本单元格是降价单元格还是代码单元格。如果是前者，我们只需要把弦连在一起，用`"".join(source)`的方法；如果是 code cell，我们还需要在 RMarkdown 中添加 chunk block 结构(更详细的内容可以从前面提到的书中[查看这部分](https://bookdown.org/yihui/rmarkdown/r-code.html))。条件语句中的`and source`部分将使程序跳过降价，并用空值编码单元格(换句话说，没有源信息)。**

*代码块结构允许 RMarkdown 运行代码，不仅可以运行 R 和 Python 代码，还可以运行许多其他语言的代码。当我们在花括号内插入单词`python`时，就在打开代码块的`````符号之后，我们告诉 RMarkdown 这是一段 Python 代码，R 应该使用特定的资源来运行它(在本例中，是 R 的`reticulate`包)。RMarkdown 代码块也支持许多其他编程语言，如 Julia、SQL、C、Javascript 等。这个系统还允许简单的 [chunck 代码重用](https://bookdown.org/yihui/rmarkdown-cookbook/reuse-chunks.html)，这是一个很好的资源，你一定要有一天去看看。*

*现在我们已经创建了`converted_notebook.Rmd`文件，我们已经准备好迁移到 Google 协作环境了。然而，让我们在这里做一个重要的观察:Google 协同实验室的下一步可能不能由您轻松地在本地完成(例如，仅仅通过运行一个神奇的语句使 Python Jupyter 笔记本中的一个单元格运行 R 代码)。除了在 R 命令中做进一步的配置以使用本地 Pandoc 副本正确地呈现 RMarkdown 文件之外，您还需要在您的机器上安装 Pandoc 程序。*

*在本地呈现 RMarkdown 文件的一个更简单的方法是安装 R 程序并使用集成了 Pandoc 的 RStudio IDE。您应该稍后测试这个选项，因为 RStudio 也是一个非常有趣的 IDE。然而，对于那些没有 R 知识的人来说，最快、最好的选择无疑是简单地转向 Google Colab，我将在下面解释。*

***4。生成 HTML 输出文件***

*现在我们需要打开一个新的谷歌 Colab 笔记本**用 R 内核**。为此，请在您的浏览器上粘贴以下链接之一。您需要登录您的 Google 帐户(或创建一个帐户)才能使用 Google 联合实验室:*

*   *[http://colab.to/r](http://colab.to/r)*
*   *[https://colab.research.google.com/#create=true&语言=r](https://colab.research.google.com/#create=true&language=r)*

*在 Google Colaboratory 笔记本中，将`converted_notebook.Rmd`文件上传到`/content`目录，一旦 Colab 启动，该目录将成为当前的工作目录。当`.Rmd`文件在正确的位置时，在 Google Colab 代码单元中执行下面的 R 代码(可能需要一段时间来运行它。R Colab 笔记本好像比 Python 的慢):*

*`install.packages()`是相当于 Python `pip install`的 R。在 Google Colab with R 中，每次开始新的会话时，您都需要安装这些包。接下来，用`library()`命令导入两个包，这类似于 Python `import`语句。最后，`rmarkdown::render()`函数会将`.Rmd`文件转换成`.html`文件。刷新 Google Colab 文件列表后，您将看到这个新文件。*

***5。最终想法***

*最后一步是从 Google Colab 下载`.html`文件，并在您最喜欢的浏览器上打开它。通过这样做，您可以检查从 RMarkdown 生成的 HTML 页面报告与 Jupyter 笔记本布局相比看起来有多么不同，如果您喜欢您所看到的，可以继续了解 RMarkdown 生态系统。*

*如果您想将带有代码的长报告转换成 HTML 文档并在页面间导航，bookdown 库是一个特别有趣的库。您还可以使用 CSS、HTML 和 Javascript 代码来进一步定制报表布局和行为。*

*如果你想更多地了解 RMarkdown 的历史、演变和未来，我邀请你观看[几天前在 9 月 9 日](https://www.youtube.com/watch?v=T24nt-d4cEg)谢一辉本人的会议报告。虽然在视频的开头和结尾，一些组织者教授会说一点葡萄牙语，但所有 Yihui Xie 的演示和 Q & A 部分都是英语。我在这里提到这个视频也是因为它是我写这篇文章的一个主要灵感。所以，我要感谢易慧和来自巴西 UFPR ( *巴拉那联邦大学/* 巴拉那联邦大学)的组织者，他们组织了第三次 R 日活动，并与我们分享了他们的知识。*

*此外，易慧非常友好地阅读了我的这篇文章，并给了我重要的反馈，指出 RMarkdown 库已经有了`*convert_ipynb()*`函数，我并不知道它的存在，它是一个比我的解决方案更完整的解决方案。因此，即使你是一个很少或没有 R 经验的 Python 用户，也可以考虑将`*convert_ipynb()*`与 RStudio 或 Google Colab 一起使用，尤其是如果你打算定期将 Jupyter 笔记本转换为 RMarkdown。*

*最后，亲爱的读者，非常感谢你花时间和精力阅读我的文章。*

**快乐编码*！*