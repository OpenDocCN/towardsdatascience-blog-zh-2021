# 你可能不知道的五个降价技巧

> 原文：<https://towardsdatascience.com/five-r-markdown-tricks-that-you-may-not-know-about-71e93f50c026?source=collection_archive---------20----------------------->

## 这五个简单的技巧开启了你用 R Markdown 做什么的可能性

![](img/1ee4e6acbaa8e27c16814e2fae92a4d2.png)

在 [Unsplash](https://unsplash.com/s/photos/happy-computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[窗口](https://unsplash.com/@windows?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

经常阅读我的文章的许多人会知道我是一个多么狂热的 R Markdown 迷。它无疑是最好的数据科学出版软件，而且完全免费，还在不断改进，变得更好、更灵活。

随着时间的推移，我不断学习新的技巧，让我的生活变得更轻松，或者从 R Markdown 中获得最大收益。我在这里与你分享最近的五个。希望你觉得有用。

## 1.新的内嵌块选项

你们中的许多人可能知道 R Markdown 中的 chunk 选项。当你写一个代码块的时候，你可以添加许多选项来指示你想用这个代码块做什么。例如:

```
```{r, echo = FALSE}
# some R code 
```
```

这将显示代码块的结果，但不会显示代码本身。或者:

```
```{python, eval = FALSE}
# some python code
```
```

这将显示代码，但不会运行它。事实上，有非常多的块选项可用，如果你不得不把它们放在 curly`{}`中，有时会变得混乱和难以理解。

如果你安装了 1.35+版本的`knitr`，你现在可以使用一个新的块选项特殊注释`#|`在块中逐行输入你的块选项，例如:

```
```{r}
#| echo = FALSE, out.width = "80%"
#| fig.cap = "My caption"my_plot
```
```

## 2.有条件地评估代码块

你知道你可以在你的块选项中加入条件吗？这非常有用，可以让你只在满足某些条件的情况下运行程序块。例如，只有当对象`my_data`超过三行时，才会评估这个块:

```
```{r}
#| eval = nrow(my_data) > 3my_data
```
```

您还可以使用条件块来显示不同的内容，这取决于您是要渲染为 PDF 还是 HTML——这意味着您只需保留一个与您的预期输出无关的`Rmd`文件。例如:

```
```{r}
#| eval = knitr::is_html_output()# code for html output (eg interactive graphic)
``````{r}
#| eval = knitr::is_latex_output()# code for pdf output (eg static graphic)
```
```

您也可以在 if 语句中使用这些`knitr`输出函数，因此另一种方法是:

```
```{r}
if (knitr::is_html_output()) {
  # code for html output
} else {
  # code for latex output
}
```
```

## 3.直接使用 Javascript 定制你闪亮的应用程序

你们中的一些人可能会使用两个独立的`ui.R`和`server.R`文件来编写自己的应用程序，但是你也可以在一个 R Markdown 文件中编写完整的应用程序。这样做的一个好处是，你可以使用 Javascript 定制你的应用程序，只需在`js`块中直接输入 Javascript。

让我们看一个简单的例子。在 Shiny 中，目前没有直接选项来限制文本区域输入框中的字符数。所以如果我使用这个标准的界面函数:

```
```{r}
shiny::textAreaInput(
  "comment",
  "Enter your comment:"
)
```
```

这将创建一个 ID 为`comment`的`textarea`框。然而，如果我想将所有的评论限制在 1000 个字符以内，我在`textAreaInput()`函数中没有简单的方法。

不过，不要担心——如果你懂一点 Javascript/jQuery，你可以添加一个`js`块来给`textarea`框添加一个`maxlength`属性，就像这样:

```
```{js}
$('[id="comment"]').attr('maxlength', 1000)
```
```

## 4.使用块选项来简化/模块化你的文档/应用

如果您的文档或应用程序变得很长，并且块中有很多代码，那么跟踪起来会很麻烦，并且很难维护。相反，您可以从 chunk 中取出代码并保存在 R 文件中，然后使用`code` chunk 选项从 R 或 Python 文件中读取代码。例如:

```
```{python}
#| code = readLines("my_setup_code.py")# python code for setup
```
```

## 5.使用 CSS 块定制你的文档

如果你想定制你的文档或应用程序的风格，你可以使用 CSS 代码块很容易地做到这一点。您可能必须呈现文档，然后在 Google Chrome 中检查它，以识别您想要定制的特定元素的类或 ID，但是作为一个简单的示例，如果您想要将文档主体的背景颜色更改为漂亮的浅蓝色，您可以简单地插入以下内容:

```
```{css}
body {
 background-color: #d0e3f1;
}
```
```

希望这些简洁的小技巧对你有用。如果你有任何其他的建议，请在评论中提出。

最初我是一名纯粹的数学家，后来我成为了一名心理计量学家和数据科学家。我热衷于将所有这些学科的严谨性应用到复杂的人的问题上。我也是一个编码极客和日本 RPG 的超级粉丝。在[*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/)*或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找我。也可以看看我在 drkeithmcnulty.com 的***上的博客。**