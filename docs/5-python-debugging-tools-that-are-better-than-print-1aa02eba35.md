# 比“打印”更好的 5 个 Python 调试工具

> 原文：<https://towardsdatascience.com/5-python-debugging-tools-that-are-better-than-print-1aa02eba35?source=collection_archive---------3----------------------->

## 更快更有效地调试您的代码

![](img/fa6d1c073a9ca422f621f854d0245efa.png)

约书亚·阿拉贡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

对于任何开发人员来说，调试代码都是最重要但又令人厌烦的任务之一。当你的代码行为怪异，崩溃，或者只是导致错误的答案时，这通常意味着你的代码包含了导致这些错误的 bug。

在用任何编程语言编写的任何代码中，都可能出现两种主要的编程错误:语法错误和语义错误。语法错误是指当您错误地键入一些命令时，或者在没有定义变量/函数的情况下使用它们时，或者如果代码中的缩进不正确时所导致的错误。

大多数情况下，如果您遵循 Python Traceback 的说明，语法错误很容易修复。另一方面，我们有所谓的语义错误。这些类型的错误是:代码运行，但计算出错误的答案，或者代码的行为与预期不同。

</5-data-science-programming-languages-not-including-python-or-r-3ad111134771>  

发现代码中的错误并修复它以消除错误的唯一方法是调试。我知道，当我们面对一个 bug 时，我们大多数人做的第一件事就是使用一堆打印语句来跟踪代码的执行，并检测错误发生在哪里。如果您的代码只有几行或最多几百行，这可能是一种有效的方法，但是随着您的代码库变长，这种方法变得不太可行，您将需要使用其他方法。

幸运的是，Python 是最流行的编程语言之一。所以它有很多工具，你可以用来调试你的代码，这比在每隔几行代码后插入一个 print 语句更加有效和可行。本文将介绍其中的 5 种工具，您可以选择哪一种最适合您的风格。

# №1: Python 标准调试器(pdb)

让我们从谈论最简单的调试器开始，一个用于中小型项目的调试器， [Python 标准调试器(pdb)](https://docs.python.org/3/library/pdb.html#module-pdb) 。PDB 是 Python 所有版本自带的默认调试器，这意味着不需要安装或麻烦；如果您的计算机上已经安装了任何 Python 版本，您就可以开始使用它。

pdb 是一个命令行调试器，您可以在代码中插入断点，然后使用调试器模式运行代码。使用这些断点，您可以检查代码和堆栈帧，这与使用 print 语句非常相似。您可以通过在代码开头导入 pdb 来开始使用它。

Pdb 可以用来跳过一些代码行，或者在一个循环中迭代特定的次数。如果需要，还可以扩展调试器，因为它是作为 Python 标准库中的一个类来实现的。Pdb 是一个非常基本的调试器，但可以添加各种扩展使其更加有用，例如 [rpdb](https://pypi.python.org/pypi/rpdb/) 和 [pdb++](https://github.com/antocuni/pdb) ，如果你正在使用 IPython，这可以使调试体验更好 [ipdb](https://github.com/gotcha/ipdb) 。

</7-tips-for-data-science-newbies-d95d979add54>  

# №2:皮查姆

pdb 是一个命令行调试器，并不是所有人都觉得有趣或易于使用。这是 ide 被创建的原因之一(集成开发环境)。ide 提供了一种可视化调试和测试代码的方法，可以更容易、更有效地调试任何代码库。但是，这些 ide 通常尺寸很大，需要进一步安装。

最著名的 Python IDE 是 [PyCharm](https://www.jetbrains.com/pycharm/) ，是 JetBrains 开发的 Python 专用 IDE。PyCharm 不仅仅是一个调试工具；相反，它是一个完整的开发环境。我发现 PyCharm 接口使用起来并不困难，但是它需要一些时间来适应，尤其是如果您以前从未使用过 IDE 的话。

PyCharm 中的调试工具使用对话框来引导您完成代码执行过程，并允许您选择各种调试参数。在 PyCharm 中使用调试模式时，可以在特定的代码行上插入断点，也可以选择设置异常断点(如果遇到特定的异常，调试器就会设置异常断点)。

# №3: Visual Studio 调试器

另一个众所周知的 IDE 是微软的 Visual Studio (VS)。不像 PyCharm，它是专门为 Python 设计和构建的。VS 是一个支持不同编程语言的开发环境。VS 有两种变体:

*   [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) :这是一个全功能、多语言的 IDE。
*   [Visual Studio 代码](https://code.visualstudio.com/)(vs Code):vs 2019 的轻量级替代。

VS2019 IDE 支持基本的 Python 调试以及 [IronPython](https://wiki.python.org/moin/IronPython) 。NET 调试。这意味着您可以使用 MPI 集群调试、断点、条件断点、跳过步骤(进入/退出/结束)、异常断点以及在未处理的异常上中断。

另一方面，VSCode 除了 Git 控制、语法突出显示和代码重构之外，还有更高级的调试工具。VSCode 的优势之一是它可以处理多语言代码库；然而，它在识别语言方面并不灵活，这反映了其使用的局限性。

</how-to-learn-programming-the-right-way-d7f87bdc7d6a>  

# №4:科莫多

如果你正在构建一个多语言的代码库，包括 Python，并且想要一个强大的能够处理各种语法的调试环境，那么 [Komodo](https://www.activestate.com/products/komodo-ide/) 应该是你的首选。Komodo 是 ActiveState 为混合语言应用程序设计和开发的全功能 IDE。

Komodo 调试器使用对话框从您那里获得调试器选项。如果您选择默认调试器设置，它将在没有进一步提示的情况下运行。Komodo 有一种复杂而灵活的方法来检测不同的编程语言，甚至可以在同一个代码文件中处理不同的语言。

Komodo 还在调试器模式中提供不同的可视化，让你加深对代码库的理解。此外，它允许您轻松地执行单元测试，并支持实时同行查看和团队协作。您还可以将 Git 与 Komodo 集成在一起，进行实时版本控制。

# №5: Jupyter 可视化调试器

大多数数据科学应用程序都是使用 Jupyter 笔记本或 Jupyter 实验室构建的。尽管您仍然可以在我们已经讨论过的任何 ide 中使用您用 Jupyter 构建的代码。有时候你只想在 Jupyter 中调试你的代码，这会让你的任务更容易。

幸运的是，一年前，Jupyter 发布了一个[可视化调试器](https://blog.jupyter.org/a-visual-debugger-for-jupyter-914e61716559)，可以在 Jupyter 环境中使用，使得使用 Jupyter 更加有效。使用此调试器，您可以在笔记本单元格或源文件中设置断点，检查变量，并导航调用堆栈。您可以通过运行以下命令轻松安装调试器:

`conda install xeus-python -c conda-forge`

使用这个[调试器](https://jupyterlab.readthedocs.io/en/stable/user/debugger.html)，您最终可以使用 Jupyter 作为一个完整的开发环境，而不需要在其他地方测试和调试您的代码。

</version-control-101-definition-and-benefits-6fd7ad49e5f1>  

# 最后的想法

当我第一次开始编程的时候——十多年前——我一直认为调试对于一个好的程序员来说不是必不可少的。作为初学者，这可能是真的。你首先需要学习如何编写代码，然后在代码出错时修复它。但是，随着我在你的学习旅程中的推进，我发现调试不仅是必要的，而且，如果你不知道如何调试代码，你就不能称自己为程序员。

直到现在，每次我想到调试，首先想到的是“打印语句”,我浪费了很多时间使用打印来检测和定位代码中的错误。尽管如此，随着我工作的代码库越来越长，使用 spring 语句变得越来越麻烦，而不是有用的方法。当我决定深入调试并找到更有效的方法来完成我的任务时。

</6-machine-learning-certificates-to-pursue-in-2021-2070e024ae9d>  

在我学习关于调试和调试器的所有知识的过程中，我使用了如此多的工具来了解那里有什么和什么最适合我。虽然我更喜欢使用的可能不是其他人更喜欢使用的，但是在选择调试器时，您只需要选择您最喜欢的。对我来说，那是而且仍然是 PDB。

因此，如果您是编程、Python 的新手，或者想在不使用 print 语句的情况下调试代码，那么试试本文中的 5 个工具，看看哪一个对您来说最合适。