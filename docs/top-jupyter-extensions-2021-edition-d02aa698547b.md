# 顶级 Jupyter 扩展:2021 版

> 原文：<https://towardsdatascience.com/top-jupyter-extensions-2021-edition-d02aa698547b?source=collection_archive---------17----------------------->

## 将您的 Jupyter 环境转变为成熟的 IDE

![](img/e7103e8e4f909cd5bde673a107b1f276.png)

哈维·卡夫雷拉在 [Unsplash](/s/photos/puzzle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

[Jupyter 笔记本](https://jupyter.org/)一直是软件思想增量开发的工具。就像 Donald Knuth 一样，文化编程概念背后的思想说，“把一个程序当作一篇文学作品，写给人类而不是计算机。”

JupyterLab 是为了解决 Jupyter 笔记本的一些缺点而开发的。除了新的文件浏览器和类似 ide 的体验，JupyterLab 还集成了许多优秀的扩展，丰富了您的工作环境。

然而，当我搜索可以融入我的日常编码程序的伟大扩展的列表时，大多数文章都是过时的，或者以死亡或休眠的项目为特色(例如，超过六个月不活动的项目)。此外，几乎每个列表的顶部都有 [*目录*](https://github.com/jupyterlab/jupyterlab-toc) 扩展，这个扩展现在是核心 JupyterLab 3.0 版本的一部分。

</the-long-awaited-jupyterlab-3-0-is-finally-here-3b6648b3a860>  

因此，我决定编制自己的清单。在这个 2021 JupyterLab 扩展版本中，我只考虑活跃的、高质量的项目。为此，对于每个项目，我将记录 GitHub 星的数量，自上次提交以来的日期，以及其他细节，如 PyPi 下载和贡献者计数。我将使用以下图标使一切更简洁:

*   ⭐ : GitHub 明星
*   📅:上次提交
*   👷:贡献者人数
*   📥:下载次数

开始吧！

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=jupyter-extensions)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# 扩展ˌ扩张

首先，让我们深入了解一下值得安装的 JupyterLab 扩展。然后，我们将看看其他好东西，如渲染器和主题。

[**jupyterlab-git**](https://github.com/jupyterlab/jupyterlab-git)(⭐813📅5 天前👷 52 📥15K/月):
一个使用 Git 进行版本控制的 JupyterLab 扩展。

[**调试器**](https://github.com/jupyterlab/debugger) (⭐ 476📅三个月前👷 11 📥63K/月):
一个 JupyterLab 调试器 UI 扩展。如果你想了解更多关于这个项目的信息，请查看下面的文章:

</jupyter-visual-debugger-is-becoming-mainstream-92fe0c336e9e>  </jupyter-get-ready-to-ditch-the-ipython-kernel-54d60776d7ef>  

[**jupyterlab-LSP**](https://github.com/krassowski/jupyterlab-lsp)**(⭐803📅23 小时前👷 21 📥20K/月):
JupyterLab 的编码辅助(代码导航+悬停建议+ linters +自动补全+重命名)使用语言服务器协议。**

**[**【jupyterlab】-variable inspector**](https://github.com/lckr/jupyterlab-variableInspector)**(⭐748📅两个月前👷 13 📥15K/月):Jupyterlab 扩展，显示当前使用的变量及其值。****

****[**jupyterlab _ code _ formatter**](https://github.com/ryantam626/jupyterlab_code_formatter)**(⭐413📅9 天前👷 25 📥12K/月):JupyterLab 的通用代码格式化程序。******

******<https://github.com/jpmorganchase/jupyterlab_templates>****(⭐223📅上个月👷 9 📥4.5K/月):支持 jupyterlab 中的 jupyter 笔记本模板。**********

******[**【jupyterlab _ tensor board**](https://github.com/chaoleili/jupyterlab_tensorboard)**(⭐245📅29 天前👷 6 📥5.7K/月):
jupyterlab 的 Tensorboard 扩展。********

******[**【jupyterlab-系统-监视器】**](https://github.com/jtpio/jupyterlab-system-monitor) (⭐ 154📅26 天前👷5📥8.3K/月):JupyterLab 扩展，用于显示系统指标。******

****[**【jupyterlab】执行时间**](https://github.com/deshaw/jupyterlab-execute-time) (⭐ 113📅上个月👷七📥8.4K/月):Jupyter 实验室的执行时间插件。****

****[**可折叠 _ 标题**](https://github.com/aquirdTurtle/Collapsible_Headings) (⭐ 108📅两个月前👷四📥6.9K/月):为 Jupyter 实验室笔记本实现可折叠标题。****

****[**拼写检查**](https://github.com/jupyterlab-contrib/spellchecker) (⭐ 105📅23 天前👷5📥5.3K/月):JupyterLab 笔记本降价单元格和文件编辑器的拼写检查器。****

****另外，我想特别提一下 [idom](https://github.com/idom-team/idom) 这个项目，我认为它是 Jupyter widgets 的下一个级别。它不完全是 JupyterLab 的扩展，但它是一个很好的工具。请在下面的文章中阅读更多信息:****

****</the-new-age-of-jupyter-widgets-cc622bee2c4b>  

# **渲染器:**

在本节中，我们将能够呈现和显示特定 MIME 类型文件的扩展进行分组。

[**jupyterlab-latex**](https://github.com/jupyterlab/jupyterlab-latex)**(⭐350📅两个月前👷 15 📥3.5K/月):JupyterLab 扩展，用于 LaTeX 文档的实时编辑。**

**<https://github.com/QuantStack/jupyterlab-drawio>****(⭐450📅7 天前👷 15 📥6K/月):将 FOSS drawio / mxgraph 包独立嵌入到 jupyterlab 中。******

******[**【jupyterlab】-电子表格**](https://github.com/quigleyj97/jupyterlab-spreadsheet) (⭐ 95📅两个月前👷四📥3.9K/月):JupyterLab 插件，用于查看电子表格，如 Excel `.xls` / `.xlsx`工作簿和 OpenOffice `.ods`文件。******

# ****主题****

****在这最后一节中，我们将从可以定制 JupyterLab 外观的扩展中获得一些乐趣。****

****[**【jupyterlab】-霓虹-主题**](https://github.com/yeebc/jupyterlab-neon-theme) (⭐ 81📅16 天前👷3📥1.1K/月):一个平面，80 年代的霓虹启发的 JupyterLab 主题。****

****[**【jupyterlab】-主题-曝光-黑暗**](https://github.com/AllanChain/jupyterlab-theme-solarized-dark) (⭐ 33📅12 天前👷2📥5.7K/月):JupyterLab 2.x 日晒黑暗扩展。****

# ****结论****

****JupyterLab 不是传统意义上的 IDE。然而，对于热爱文化编程的数据科学家和工程师来说，这是一个昂贵的项目。除了许多内置工具，JupyterLab 还提供了一系列扩展，可以丰富您的工作环境，改变我们开发代码的方式。****

****这篇文章提供了一份最新的 JupyterLab 扩展列表，供您今天使用。如果你认为ι遗漏了什么或者有一个新的扩展完全改变了你使用 Jupyter 的方式，请在评论区留下它！****

******看看我的其他笔记本故事，用这个优秀的工具提升你的游戏:******

*   ****[Jupyter 现在是一个成熟的 IDE](/jupyter-is-now-a-full-fledged-ide-c99218d33095)****
*   ****Jupyter 已做好生产准备；原样****
*   ****[Jupyter 在 VS 代码:利弊](/jupyter-is-taking-a-big-overhaul-in-visual-studio-code-d9dc621e5f11)****
*   ****Jupyter 有一个完美的代码编辑器****
*   ****[Jupyter widgets 的新时代](/the-new-age-of-jupyter-widgets-cc622bee2c4b)****

# ****关于作者****

****我叫 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=jupyter-extensions) ，是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。****

****如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请在 Twitter 上关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=medium&utm_medium=article&utm_campaign=jupyter-extensions)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！********