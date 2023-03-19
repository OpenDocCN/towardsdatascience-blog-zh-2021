# 用 Python 合并多个 Jupyter 笔记本

> 原文：<https://towardsdatascience.com/merge-multiple-jupyter-notebooks-together-with-python-1696eb8d0dab?source=collection_archive---------38----------------------->

## 避免手动复制粘贴笔记本单元格

![](img/89b7ed037fd062c26d2b7f107af28025.png)

图片来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2876776)

Jupyter 笔记本被认为是任何数据科学家工具箱中不可或缺的一部分。Jupyter Notebook 也称为 IPython Notebooks，是基于服务器-客户端结构的 web 应用程序。

Jupyter Notebook 提供了一个易于使用的交互式数据科学环境，支持各种编程语言。它对原型设计非常有用，并通过结合代码、文本和可视化提供交互式计算。

在任何项目开发中，使用多个 Jupyter 笔记本是很常见的。有时需要将几个笔记本合并在一起，您需要手动将笔记本的单元格复制粘贴在一起，这是一项繁琐的任务。

在本文中，您可以阅读如何高效地将几个 IPython 笔记本合并在一起，只需编写几行 Python 代码。

## Jupyter 笔记本的结构:

Jupyter 笔记本保存为 JSON 文件格式。这个 JSON 文件包含所有笔记本内容，包括文本、代码、输出和绘图。数字、图像和视频被编码为 base64 字符串。Jupyter 笔记本 JSON 文件的基本结构是:

```
**{
 "cells": [],
 "metadata": {},
 "nbformat": 4,
 "nbformat_minor": 4
}**
```

*   **元数据:**包含关于内核和所用语言的信息的字典。
*   **nbformat，nbformat_minor:** 笔记本格式版本(4.0)。
*   **cells:** Cell 是 JSON 文件最重要的组成部分，它包含了每个单元格的信息。每个单元格由具有以下组件的字典表示:

```
**{**
**"cell_type": "code",** // can be code, markdown or raw
**"execution_count":** null, // null or an integer
**"metadata": {},** // metadata of the cell: tags,editable,collapsed
**"outputs": [],** // result of code or rendered markdown
**"source": []** // the input code or source markdown
**}**
```

# 合并笔记本:

将几个 Jupyter 笔记本合并在一起的基本思想是操作笔记本的内容，并将 JSON 文件合并在一起以创建一个单独的笔记本。

nbconvert 是一个 Python 工具，它为研究人员提供了跨不同格式及时传递信息的灵活性。它可以用来合并几个 JSON 文件的内容。让我们通过以下步骤开始合并笔记本。

*   导入 nbconvert 库:`**import nbconvert**`
*   使用 nbconvert 的`**.read()**` 函数读取所有 IPython 笔记本，该函数读取 JSON 格式的笔记本。

```
# Reading the notebooks
**first_notebook = nbformat.read(file1, 4)
second_notebook = nbformat.read(file2, 4)
third_notebook = nbformat.read(file3, 4)**
```

*   创建一个新笔记本，其中包含上述三个笔记本的合并内容。新笔记本的元数据将与早期笔记本的元数据相同。

```
# Creating a new notebook
**final_notebook = nbformat.v4.new_notebook(metadata=first_notebook.metadata)**
```

*   将每个 JSON 文件中的`**cell**` 键的所有值连接起来。`**+**` 关键字可以用来合并三个 IPython 笔记本的`**cell**`键上的字典值。

```
# Concatenating the notebooks
**final_notebook.cells = 
first_notebook.cells + second_notebook.cells + second_notebook.cells**
```

*   最后，使用 nbconvert 库中的`**.write**`函数保存新创建的 IPyton 笔记本。

```
# Saving the new notebook 
**nbformat.write(final_notebook, 'final_notebook.ipynb')**
```

用几行 Python 代码将包含每个笔记本内容的新文件保存到所需位置。

# 结论:

在本文中，我们讨论了如何使用 nbconvert 工具将多个 IPython 笔记本文件合并到一个笔记本中。根据个人需求，他们可以对脚本进行修改，并以简单高效的方式合并笔记本。

nbconvert 可以用来将笔记本转换成其他静态格式包括 HTML，LaTeX 等。这是一个有趣的包，你可以通过[文档页面](https://nbconvert.readthedocs.io/en/latest/)来了解。

# 参考资料:

[1] NbConvert 文档:[https://nbconvert.readthedocs.io/en/latest/](https://nbconvert.readthedocs.io/en/latest/)

> 感谢您的阅读