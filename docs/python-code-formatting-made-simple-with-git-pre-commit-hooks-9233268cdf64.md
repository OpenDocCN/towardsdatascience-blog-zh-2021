# 使用 Git 预提交挂钩简化 Python 代码格式化

> 原文：<https://towardsdatascience.com/python-code-formatting-made-simple-with-git-pre-commit-hooks-9233268cdf64?source=collection_archive---------12----------------------->

## 编写每个人都会爱上的代码

![](img/b56ca540701b4c9fe97cf3adff4216dc.png)

使用 Black、Isort 和 Autoflake 的预提交钩子自动格式化 python 代码。——[黑暗女王](https://unsplash.com/@thdrkqwn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Python 因其优雅的语法而成为世界上最流行的编程语言。但是仅仅这样并不能保证代码清晰可读。

Python 社区已经发展到创建标准，使不同程序员创建的代码库看起来像是同一个人开发的。后来，像 Black 这样的包被创建来自动格式化代码库。然而，问题只解决了一半。Git [预提交](https://pre-commit.com/)钩子完成剩下的工作。

什么是预提交挂钩？

预提交钩子是在 git 提交之前自动运行的有用的 git 脚本。如果预提交挂钩失败，git 推送将被中止，并且取决于您如何设置它，您的 CI 软件也可能失败或根本不触发。

请注意，在设置提交前挂钩之前，请确保您具备以下条件。

*   您需要 git 版本> = v.0.99(您可以使用 git — version 检查这一点)
*   你需要安装 Python(因为它是 git 钩子使用的语言。)

# 安装提交前挂钩

您可以使用单个 pip 命令轻松安装预提交包。但是要将它附加到您的项目中，您还需要一个文件。

```
pip install pre-commit
```

*。pre-commit-config.yaml* 文件包含项目所需的所有配置。您可以在这里告诉预提交在每次提交之前需要执行什么操作，并在需要时覆盖它们的默认值。

以下命令将生成一个示例配置文件。

```
pre-commit sample-config > .pre-commit-config.yaml
```

下面是一个示例配置，它在每次提交更改之前对 requirements.txt 文件进行排序。将它放在项目目录的根目录下，或者编辑使用上一步生成的目录。

我们已经安装了预提交包，并配置了它应该如何工作。现在，我们可以使用下面的命令对这个存储库启用预提交挂钩。

厉害！现在将下面的 requirements.txt 文件添加到您的项目中，并进行提交。请参见预提交钩子的作用。

在提交之前，git 将从存储库中下载指令，并使用需求修复模块来清理文件。生成的文件将如下所示。

# 黑色带有预提交挂钩，可以自动格式化 Python 代码。

[Black](https://github.com/psf/black) 是一个流行的包，帮助 Python 开发者维护一个干净的代码库。

大多数代码编辑器都有键盘快捷键，您可以将其绑定到黑色，这样您就可以随时清理代码。例如，Linux 上的 VSCode 使用 Ctrl + Shift + I，在第一次使用该快捷键时，VScode 会提示使用哪个代码格式化程序。您可以选择黑色(或 autopep8)来启用它。

但是，如果按快捷键困扰你，你可以把它放在预提交钩子上。下面的代码片段可以达到这个目的。

请注意，这比以前的设置更多。这里，除了使用黑色，我们还覆盖了它的默认值。我们使用 args 选项配置 black 来设置 120 个字符的最大行长度。

让我们看看 git 提交钩子如何与 black 一起工作，Black 的文档中给出了一个例子。用以下内容创建一个 python 文件(名称不重要，只要是. py 文件即可)。

提交后的上述文件将如下所示。这个相比上一个更标准。它很容易阅读，代码审查人员也喜欢这样看。

# 配置预提交挂钩来查找本地存储库

有时，您希望从本地安装包运行预提交挂钩。让我们尝试使用本地安装的 [isort 包](https://github.com/PyCQA/isort)来对 python 导入进行排序。

您可以使用以下命令安装 isort。

```
pip install isort
```

现在编辑。pre-commit-config.yaml 文件并插入下面的代码片段。

要了解这一点，请创建一个包含多个导入的 python 文件。这里有一个样本。

提交后，相同的文件将如下所示。

# 使用 Black、Isort 和 Autoflake 预提交钩子来获得更整洁的 python 代码库。

这是我在几乎所有项目中使用的预提交钩子模板。我们已经在这个列表中讨论了两个钩子，Black 和 Isort。 [Autoflake](https://github.com/myint/autoflake) 是另一个有用的钩子，可以移除未使用的变量、空白和导入。

由于该模板使用本地包，请确保您已经安装了它们。您可以运行下面的命令一次性安装它们，并设置对 git 存储库的预提交。

```
pip install isort autoflake black pre-commit
pre-commit install
```

# 最后的想法

Git 预提交在很多方面都是革命性的。它们主要在 CI/CD 管道中用于触发活动。另一个主要用例是将它们用于自动代码格式化。

对于不同的人来说，格式良好的代码易于阅读和理解，因为它遵循社区中共享的通用准则。Python 有一个名为 PEP8 的标准，Black、Isort 和 Autoflake 等工具可以帮助开发人员自动化这个标准化过程。

然而，记住这一点并每次手动使用该工具可能会很麻烦。预提交钩子快速地把它放到代码审查清单中，并在每次提交前自动运行它。

在这篇文章中，我们讨论了如何从远程存储库和本地安装包中使用预提交钩子。

我希望你会喜欢它。

> 朋友，谢谢你的阅读。看来你和我有许多共同的兴趣。 ***跟我打招呼*** *上* [*领英*](https://www.linkedin.com/in/thuwarakesh/)*[*推特*](https://twitter.com/Thuwarakesh)*[*中*](https://thuwarakesh.medium.com/subscribe) *。我会为你打破僵局。***

**还不是中等会员？请使用此链接 [**成为会员**](https://thuwarakesh.medium.com/membership) 因为我为你免费推荐赚取佣金。**