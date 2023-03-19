# 如何在 Github 上托管 Jupyter 笔记本幻灯片

> 原文：<https://towardsdatascience.com/how-to-host-jupyter-notebook-slides-on-github-d785f30e6e2?source=collection_archive---------29----------------------->

## *制作幻灯片和在 Github 上托管 html 文件的工作量最小*

![](img/cb4e69047570ca0411c2e4090db3e67f.png)

凯莉·西克玛在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

点击这里查看这篇文章的幻灯片:[https://sophiamyang.github.io/slides_github_pages/.](https://sophiamyang.github.io/slides_github_pages/.)

本文分为两个部分:

1.  如何将您的 Jupyter 笔记本变成幻灯片并输出到 html 文件。
2.  如何在 Github 上托管 html 文件？

# Jupyter 笔记本幻灯片

首先，我们创建一个新环境`slideshow`，安装一个 Jupyter 笔记本扩展 RISE，并启动 Jupyter 笔记本:

```
conda create -n slideshow -c conda-forge python=3.9 rise
conda activate slideshow
jupyter notebook
```

然后像往常一样创建一个 Jupyter 笔记本文件:

*   单击视图→工具栏→幻灯片显示，定义每个单元格的幻灯片类型。
*   RISE 在工具栏的右上角创建了一个按钮“进入/退出实时显示幻灯片”。
*   单击此工具栏，您将能够以幻灯片形式查看您的笔记本。

# 幻灯片到 html

如果您有代码单元格并希望显示所有代码单元格，请使用:

```
jupyter nbconvert github_page_example.ipynb --to slides --stdout > index.html
```

如果您想要隐藏所有代码单元格:

```
jupyter nbconvert github_page_example.ipynb --no-input --no-prompt --to slides --stdout > index.html
```

如果您想隐藏某些单元格的代码，我们可以通过视图→单元格工具栏→标签为这些代码单元格添加标签。这里我使用标签“移除输入”。

```
jupyter nbconvert github_page_example.ipynb --TagRemovePreprocessor.remove_input_tags "remove_input" --to slides --stdout > index.html
```

# Github 上的主机 html 文件

要在 Github 页面上发布一个 html 文件，我们需要推送到`gh-pages`分支。

使用 gh-pages 分支在本地创建新的 git repo:

```
git init
git checkout -b gh-pages
git add index.html
git commit
```

去 Github 创建一个新的空 repo，然后把我们的文件推送到`gh-pages`分支。

```
git remote add origin git**@github**.com:YOUR_USER_NAME/YOUR_REPO_NAME.git
git branch -M gh-pages
git push -u origin gh-pages
```

现在你可以在 Github 页面上看到这个幻灯片:[https://YOUR _ USER _ NAME . Github . io/YOUR _ REPO _ NAME](https://YOUR_USER_NAME.github.io/YOUR_REPO_NAME)

看看我的:[https://sophiamyang.github.io/slides_github_pages/.](https://sophiamyang.github.io/slides_github_pages/.)

# 参考:

[https://rise.readthedocs.io/en/stable/](https://rise.readthedocs.io/en/stable/)

[https://pages.github.com/](https://pages.github.com/)

作者索菲亚·杨 2021 年 7 月 18 日