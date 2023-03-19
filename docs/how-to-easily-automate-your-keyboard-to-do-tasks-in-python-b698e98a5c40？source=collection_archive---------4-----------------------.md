# 如何轻松地自动化您的键盘来完成 Python 中的任务

> 原文：<https://towardsdatascience.com/how-to-easily-automate-your-keyboard-to-do-tasks-in-python-b698e98a5c40?source=collection_archive---------4----------------------->

## 永远告别单调的任务

![](img/b308f55004f07af931a3dff7843631a8.png)

[https://unsplash.com/@girlwithredhat](https://unsplash.com/@girlwithredhat)

在 Python 中自动化你的键盘是非常容易的，下面是如何做到的。

你可以很容易地设置脚本，让它在你睡觉的时候帮你完成任务。

在这里，我将使用一个将成千上万的 PDF 文件转换为。txt 文件。在处理任何 NLP 项目时，知道如何做也是一件非常有用的事情。我还会写一些关于如何从 pdf 中提取数据的后续文章，所以请保持警惕。

你当然可以用 Adobe 来做这个，但是那会让你付出代价，而且这种方式肯定更有趣。

然而，实际应用并不止于此。如果你能自动化你的键盘，你就能自动化你的电脑所做的大部分事情！

让我们开始吧。

# 键盘包装

这个例子中的重物是“键盘”包。它的文档可以在[这里找到。这是一个用纯 Python 编写的非常简单的包，并且没有依赖性。它可以在 Linux 和 Windows 上运行，从个人经验来看，它的易用性非常好。](https://pypi.org/project/keyboard/)

你大概能猜到怎么安装。

```
pip install keyboard
```

使用起来非常简单。下面的代码只有七行，它将打开 windows 栏，搜索 google chrome，然后打开一个新的 chrome 浏览器。

作者代码—可在 https://gist.github.com/jasher4994 的[获得。](https://gist.github.com/jasher4994)

尽管这可能很酷，但绝对没用。所以让我们进入有用的东西。

可以节省你数周时间的东西。

# 结合使用键盘和 Python 脚本来自动化重复的过程

我发现键盘包最有用的地方是它是一个设计良好的系统的一部分，这个系统围绕着一个只能用键盘执行的重复动作。

因此，具有挑战性的部分是设计一个系统，这样我们就可以自动化这个过程——在这个系统中，我们可以只用键盘做任何事情。

在这种情况下，很简单，我们必须设计一个系统，在其中我们可以使用键盘来自动化单调的动作，节省。txt 文件。

因此，我们需要按顺序做这些事情:

*   打开我们要更改为. txt 文件的文件。
*   点击“文件”
*   单击“另存为文本”
*   指定新文件名
*   单击保存

我们需要做很多次。因此，我们希望对所有想要转换的文件自动执行这些操作。txt 文件。

因此，首先，我们必须把所有的文件放在一个地方，任何文件夹都可以。把它们放在这个文件夹中，使用下面的函数提取所有文件的名称列表。输入只是文件夹的路径。

作者编写的代码—可在[https://gist.github.com/jasher4994](https://gist.github.com/jasher4994)下载

现在是有趣的部分，设置你的脚本来自动完成单调的部分。下面这个相对较短的函数就是你所需要的。

作者编写的代码—可以在 https://gist.github.com/jasher4994 的[下载。灵感来源:](https://gist.github.com/jasher4994)[https://stack overflow . com/questions/58774849/using-adobe-readers-export-as-text-function-in-python](https://stackoverflow.com/questions/58774849/using-adobe-readers-export-as-text-function-in-python)

这个函数非常简单。它接受一个路径(到存储所有文件的文件夹)和一个文件名作为输入。然后，它指定了一个用于紧急情况的 kill 键，然后概述了我在前面的列表中指定的同一组命令。第 3 行将文件名和文件夹路径连接起来，给出我们要找的文件的完整路径。我们可以从一开始就一起指定它们，但是当我们遍历同一个文件夹中的所有文件时，这将更有意义——路径是相同的，但是文件扩展名将会改变。**第 4 行**则相反，说明我们将保存新创建的. txt .文件的路径。**第 5 行**通过声明我们允许覆盖文件来舍入第一个块——如果我们想要覆盖，稍后需要额外的点击。

因此，**第 7–18 行**无非是说明要按哪些键，以及两次按键之间要等待多长时间。这些行应该是不言自明的。输入是要按下的按钮和时间的整数输入。sleep()是在执行下一个命令之前要等待的时间。

如果你的文件很小，你可以在不到 5 秒的时间内完成，但是我有几个很长的 pdf 文件需要 3-4 秒。你不想催促计算机，否则它会在完成保存之前就开始按下当前文件的下一个文件的键。这可不好。如果你打算让它通宵运行，你最好保守估计。用一些最大的文件进行测试是一个好主意。

# 将这一切结合在一起

因此，要将所有这些放在一起并自动化这个过程，您只需要指定文件夹、导入包并遍历文件。

作者编写的代码—可以在[https://gist.github.com/jasher4994](https://gist.github.com/jasher4994)下载

那很容易，不是吗？现在你所有的 pdf 文件都应该附有。txt 文件放在它们下面的同一文件夹中。

您可以通过将您拥有的文档数量乘以一次迭代的总时间来粗略估计它将运行多长时间。您可以在一次迭代中测试它，或者只计算睡眠时间，每按一次键增加一秒钟。

按照这种逻辑，1000 份文件需要的时间和睡一个好觉的时间差不多。

**所以，买一个吧。**

```
If I’ve inspired you to join medium I would be really grateful if you did it through this [link](https://jamesasher4994.medium.com/membership) — it will help to support me to write better content in the future.If you want to learn more about data science, become a certified data scientist, or land a job in data science, then checkout [365 data science](https://365datascience.pxf.io/c/3458822/791349/11148) through my [affiliate link.](https://365datascience.pxf.io/c/3458822/791349/11148)
```

如果你喜欢这篇文章，下面是我写的一些类似的文章:

[](/how-to-easily-run-python-scripts-on-website-inputs-d5167bd4eb4b) [## 如何在网站输入上轻松运行 Python 脚本

### 这是我建立的一个网站的演示，它将动态分析文本情感

towardsdatascience.com](/how-to-easily-run-python-scripts-on-website-inputs-d5167bd4eb4b) [](/how-to-easily-show-your-matplotlib-plots-and-pandas-dataframes-dynamically-on-your-website-a9613eff7ae3) [## 如何轻松地在你的网站上动态显示你的 Matplotlib 图和 Pandas 数据框。

### 这是一种令人惊讶的简单方法，可以在线向全世界展示您的图表和数据框架，而且不到…

towardsdatascience.com](/how-to-easily-show-your-matplotlib-plots-and-pandas-dataframes-dynamically-on-your-website-a9613eff7ae3) 

干杯，

詹姆斯