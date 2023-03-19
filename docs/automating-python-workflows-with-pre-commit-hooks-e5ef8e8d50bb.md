# 使用预提交挂钩自动化 Python 工作流

> 原文：<https://towardsdatascience.com/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb?source=collection_archive---------28----------------------->

## 什么是预提交钩子，它们如何给你的 Python 项目带来好处

![](img/d9fa673ffae244cf9da8c663ef034fd6.png)

unsplash.com上[维沙尔·贾达夫](https://unsplash.com/@vishu_2star)的照片

像大多数版本控制系统(VCS)一样，Git 具有运行脚本和在特定点触发动作的能力。这些脚本被称为**钩子**，它们可以驻留在客户端或服务器端。**服务器端**挂钩通常在接收到推送之前或之后触发，而**客户端**挂钩通常在提交或合并之前触发。有许多**钩子类型**可以用来执行特定的动作来服务于您的工作流。

在本文中，我们将探讨预提交挂钩，这是在客户端执行的特定操作。我们将讨论它们的用途，以及在 Python 项目的工作流中需要考虑触发哪些特定操作。最后，我们将探索一种在创建提交时绕过预提交挂钩的快速方法。

## 什么是预提交挂钩？

顾名思义，预提交挂钩是在创建提交之前触发的特定操作。预提交挂钩通常用于检查您的代码，确保它符合某些标准。例如，您可以启用预提交挂钩，以便验证提交内容的代码风格是否符合 PEP-8 风格指南。

现在，执行的每个预提交钩子都返回一个状态代码，任何非零代码都将中止提交，关于失败的钩子的详细信息将报告给用户。

开发团队同意并共享适当的预提交挂钩是很重要的，因为它们有助于确保源代码的一致性和高质量。此外，预提交挂钩可以提高开发速度，因为开发人员可以使用它们来触发某些操作，否则，这些操作将需要手动重复执行。

## 如何安装和启用预提交挂钩

首先，确保您已经启用了虚拟环境。一旦完成，您就可以开始安装`pre-commit`到`pip`:

```
$ pip install pre-commit
```

我们现在可以验证安装是否成功

```
$ pre-commit --version
pre-commit 2.10.1
```

既然我们已经成功安装了`pre-commit`包，下一步就是启用期望的预提交钩子。命令行工具附带了一个方便的选项，允许我们自动生成预提交挂钩的示例配置:

```
$ pre-commit sample-config# See [https://pre-commit.com](https://pre-commit.com) for more information
# See [https://pre-commit.com/hooks.html](https://pre-commit.com/hooks.html) for more hooks
repos:
-   repo: [https://github.com/pre-commit/pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks)
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
```

示例配置带有 4 个不同的预提交挂钩；

这个钩子从每一行的末尾开始修剪所有的空白。如果出于某种原因，您希望修剪额外的字符，您可以指定以下参数:`args: [--chars,"<chars to trim>"]`。举个例子，

```
hooks:
    -   id: trailing-whitespace
        args: [--chars,"<chars to trim>"]
    -   id: end-of-file-fixer
```

`**end-of-file-fixer**`:这个钩子确保所有文件都以一个换行符结束，并且只有一个换行符。

`**check-yaml**`:这个钩子将尝试加载所有 yaml 文件并验证它们的语法

`**check-added-large-files**`:最后，这个钩子将防止非常大的文件被提交。默认情况下，任何超过 500Kb 的文件都将被视为大文件。您可以通过提供以下参数来修改大小:

```
hooks:
    -   id: check-added-large-files
        args: [--maxkb=10000]
```

既然我们已经探索了一些基本的预提交挂钩，那么是时候看看它们的实际应用了。我们需要做的是将这些钩子添加到一个名为`.pre-commit-config.yaml`的文件中。在这个例子中，我们将使用我们之前已经研究过的示例配置。为此，只需运行以下命令:

```
$ pre-commit sample-config > .pre-commit-config.yaml
```

现在，验证示例配置是否已成功添加到 yaml 文件中:

```
$ cat .pre-commit-config.yaml# See [https://pre-commit.com](https://pre-commit.com) for more information
# See [https://pre-commit.com/hooks.html](https://pre-commit.com/hooks.html) for more hooks
repos:
-   repo: [https://github.com/pre-commit/pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks)
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
```

最后，我们需要安装文件中的钩子。以下命令将自动解析`.pre-commit-config.yaml`的内容并安装所有定义的钩子:

```
$ pre-commit install --install-hooks
```

注意，默认情况下，`pre-commit`会在目录`.git/hooks/pre-commit`下安装目标钩子。如果一切按计划进行，您应该会看到下面的输出

```
pre-commit installed at .git/hooks/pre-commit
[INFO] Initializing environment for [https://github.com/pre-commit/pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks).
[INFO] Installing environment for [https://github.com/pre-commit/pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks).
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
```

现在，让我们提交我们所做的更改，并在我们的 Git 存储库上添加预提交挂钩。

```
$ git add .pre-commit-config.yaml
$ git commit -m "Added pre-commit hooks in the repo"
```

一旦执行了`git commit`命令，预提交钩子将在创建新提交之前运行所有指定和安装的检查。

```
Trim Trailing Whitespace............................................Passed
Fix End of Files......................................Passed
Check Yaml............................................Passed
Check for added large files...........................Passed
[main f3f6caa] Test pre-commit hooks
 1 file changed, 10 insertions(+)
 create mode 100644 .pre-commit-config.yaml
```

正如我们所见，`pre-commit`钩子已经运行了所有 4 个已定义的钩子，并且它们都通过了。因此，新的提交将被成功创建，现在您可以`git push`对您的远程存储库进行更改了。请注意，如果至少有一个`pre-commit`挂钩失败，提交的创建将会失败，您将会被告知哪些挂钩失败了。此外，它将尝试解决问题(如果可能)。如果这是不可能的，你将不得不作出必要的修改，并重新提交。

例如，假设我们的`.pre-commit-config.yaml`文件在最后有额外的新行。`pre-commit`挂钩将失败，并自动为您解决问题:

```
Trim Trailing Whitespace............................Passed
Fix End of Files....................................Failed
- hook id: end-of-file-fixer
- exit code: 1
- files were modified by this hookFixing .pre-commit-config.yamlCheck Yaml.........................................Passed
Check for added large files........................Passed
```

现在问题已经解决了，你要做的就是再次`git commit`，最后`git push`。

## 哪些预提交挂钩用于 Python 项目

根据您想要实现的具体目标，您需要在创建提交之前触发某些操作。然而，在开发 Python 应用程序时，有几个必备的预提交钩子将使您的生活变得更加容易。

首先，让我们定义适合大多数 Python 应用程序的`pre-commit-config.yaml`文件。请注意，我认为示例配置的 4 个预提交挂钩非常重要，所以我也要包括它们。

```
# .pre-commit-config.yaml# See [https://pre-commit.com](https://pre-commit.com) for more information
# See [https://pre-commit.com/hooks.html](https://pre-commit.com/hooks.html) for more hooks
repos:
-   repo: [https://github.com/pre-commit/pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks)
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
        args: [--maxkb=10000]
    -   id: double-quote-string-fixer
    -   id: mixed-line-ending
        args: [--fix=lf]
    -   id: check-ast
    -   id: debug-statements
    -   id: check-merge-conflict
-   repo: [https://github.com/pre-commit/mirrors-autopep8](https://github.com/pre-commit/mirrors-autopep8)
    rev: v1.4
    hooks:
    -   id: autopep8
```

我们已经讨论了前四个钩子，它们是我们在上一节中探索的示例配置的一部分。现在让我们探索文件中剩余的钩子。

`**double-quote-string-fixer**`:这个钩子，将在适用的地方用单引号(`'`)代替双引号(`"`)。当需要双引号的时候(比如说`my_string = "Hello 'World'"`)，这个钩子不会代替它们

`**mixed-line-ending**`:该挂钩将检查混合行尾，并可选择用 CRLF(回车线)和 LF 替换行尾。在我们的例子中，我们显式地指定了`--fix`选项，以便钩子强制用 LF 替换行尾。

`**check-ast**`:这个钩子将检查以`.py`结尾的文件是否解析为有效的 Python。

`**debug-statements**`:这个钩子将检查所有以`.py`结尾的文件中的调试器导入。对于 Python 3.7+来说，它还将验证在源代码中没有提到`breakpoint()`。

`**check-merge-conflict**`:这将检查包含合并冲突字符串的文件。

`**autopep8**`:最后，这个钩子会自动格式化所有以`.py`结尾的文件，以符合 PEP-8 样式指南。

有关所有可用挂钩的更多详细信息，您可以参考本页。

## 如何在不创建提交的情况下运行提交前挂钩

如果出于某种原因，您想运行`pre-commit`钩子而不创建提交，您所要做的就是运行下面的命令

```
$ pre-commit run --all-files
```

这个命令将验证您的存储库的所有文件是否都符合预提交挂钩所施加的限制。如果一切都符合标准，您应该会看到以下输出

```
Trim Trailing Whitespace.............................Passed
Fix End of Files.....................................Passed
Check Yaml...........................................Passed
Check for added large files..........................Passed
```

## 如何绕过预提交挂钩

在某些时候，你可能希望做一个`git commit`并绕过预提交钩子执行的检查。这可以通过提供`--no-verify`选项来实现:

```
git commit -m "Bypassing pre-commit hooks" --no-verify
```

## 结论

在本文中，我们探讨了预提交挂钩的重要性，以及它们如何使开发团队受益，以便他们能够维护满足特定标准的高质量代码。预提交挂钩可以通过自动化工作流中的某些操作来帮助您节省时间。此外，我们讨论了如何在您的本地机器上安装预提交钩子，以及启用哪些特定的钩子，以使您在开发 Python 应用程序时更加轻松。