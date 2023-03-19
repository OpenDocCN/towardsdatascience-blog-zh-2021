# Jupyter 可视化调试器正在成为主流

> 原文：<https://towardsdatascience.com/jupyter-visual-debugger-is-becoming-mainstream-92fe0c336e9e?source=collection_archive---------28----------------------->

## 也许 JupyterLab 最受欢迎的特性现在已经在`ipykernel`发布了

![](img/22e16669681fb9d2f90b1f34ac73b9ed.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4297771) 的 [JohnArtsz](https://pixabay.com/users/johnartsz-12835614/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4297771)

大约一年前，JupyterLab 的可视化调试器发布了。这是使 JuypterLab 成为一个成熟的 IDE 的一步，因为它提供了你所期望的 IDE 调试器的大部分功能。

</jupyter-is-now-a-full-fledged-ide-c99218d33095>  

然而，这个特性是建立在 [**xeus-python** 内核](https://blog.jupyter.org/a-new-python-kernel-for-jupyter-fcdf211e30a8)之上的，这是 python 编程语言 Jupyter 内核的一个轻量级实现。当时，`xeus-python`内核并没有提供与`ipykernel`完全对等的特性。最值得注意的是，它缺少对`matplotlib`和 Jupyter magic 命令的支持。

但是在这个领域，现状每天都在变化，新的特性以前所未有的速度实现。因此，上个月`xeus-python`宣布支持`matplotlib`和 Jupyter 魔法指令。

</jupyter-get-ready-to-ditch-the-ipython-kernel-54d60776d7ef>  

今天，对`ipykernel`的合并 pull 请求将可视调试器带到 Python 编程语言的主 Jupyter 内核，并再次改变状态！

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=ipykernel-debugger)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# `ipykernel`上的 Jupyter 调试器

笔记本一直是软件思想增量开发的工具。尽管 JupyterLab 将项目推向提供类似 IDE 的开发体验，但仍有一些缺失。

解决这些问题产生了新的问题；最受欢迎的功能，可视化调试器，直到现在还依赖于`xeus-python`，而`xeus-python`并没有提供与`ipykernel`完全对等的功能。

![](img/692ebf27e6c58c62f05d12fb8b2c11a0.png)

Jupyter 可视化调试器—作者图片

然而，上个月，`xeus-python`背后的团队宣布支持`matplotlib`和 Jupyter magics，使`xeus-python`离`ipykernel`更近了一步。

但是，仍然缺少一些东西。这一次，最讨厌的事实是`xeus-python`并没有真正支持一个`pip`安装程序。我们可以一直跑:

```
pip install xeus-python notebook
```

然而，上传到 PyPI 上的轮子是实验性的。因此，我们被`conda`安装程序卡住了，由于许可问题，许多公司不想要或可以使用它。

今天，我很高兴地宣布一个对`ipykernel`的 PR 被合并，这为 Python 编程语言的主 Jupyter 内核带来了可视化调试器的实现。尽管实现还不完整，但它已经具备了您入门所需的一切:

*   变量资源管理器、断点列表和源代码预览
*   导航调用堆栈的可能性(下一行、进入、退出等)。)
*   直观地在感兴趣的行旁边设置断点的能力
*   指示当前执行停止位置的标志

让我们希望这项工作将很快结束，我们将能够在日常编程活动中使用这样一个重要而强大的工具。

# 结论

可视化调试器很可能是 JupyterLab 最需要的特性。一年前，这个在`xeus-python`上的实现解决了部分问题。

现在一个新的 PR 试图把它带到`ipykernel`，Python 编程语言的主要 Jupyter 内核。尽管实现还不完整，但它已经具备了您开始工作所需的一切。

**看看我的其他笔记本故事，用这个优秀的工具提升你的游戏:**

*   Jupyter 已做好生产准备；原样
*   [期待已久的 JupyterLab 3.0 终于来了](/the-long-awaited-jupyterlab-3-0-is-finally-here-3b6648b3a860)
*   [Jupyter 在 VS 代码:利弊](/jupyter-is-taking-a-big-overhaul-in-visual-studio-code-d9dc621e5f11)
*   [Jupyter 有一个完美的代码编辑器](/jupyter-has-a-perfect-code-editor-62147cb9bf21)
*   [小配件的新时代](/the-new-age-of-jupyter-widgets-cc622bee2c4b)

# 关于作者

我叫 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=ipykernel-debugger) ，我是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=medium&utm_medium=article&utm_campaign=ipykernel-debugger)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！