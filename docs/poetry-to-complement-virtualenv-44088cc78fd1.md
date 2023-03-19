# 你还在使用 Virtualenv 管理 Python 项目中的依赖关系吗？

> 原文：<https://towardsdatascience.com/poetry-to-complement-virtualenv-44088cc78fd1?source=collection_archive---------4----------------------->

## 有一种更好的方法来管理依赖项、打包和发布 Python 项目。

![](img/673c2d358b8d5f5203df7e6c84303d1b.png)

惊讶地看到你仍然使用虚拟环境而不是诗歌——来自 [Pexels](https://www.pexels.com/photo/man-in-red-polo-shirt-3779453/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Andrea Piacquadio](https://www.pexels.com/@olly?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

当我开始我的数据科学生涯时，我被项目的复杂性吓住了。我们在所有的 python 项目中都使用了 Virutalenv。

我对节点包管理器(npm)印象深刻，并且一直想知道为什么我们在 Python 中没有一个这样的管理器。

> 我渴望有一个工具来维护隔离的环境、管理开发和生产依赖关系、打包和发布。

谢天谢地，我们现在有了[诗](https://python-poetry.org/)。

简单来说，诗歌就是 Python 中**依赖管理**和**打包**的工具。但是这个官方定义是不完整的，因为我发现诗歌不仅仅是管理依赖性和打包。

在本指南中，您可以找到，

*   [如何打包一个 Python 项目并发布到 PyPI 库](#7280)；
*   [将开发依赖与生产依赖分开](#b79c)；
*   [诗歌如何在团队中帮助一致的开发环境](#0aa3)，以及；
*   [在不破坏虚拟文件夹的情况下，重新定位并重命名项目文件夹。](#3f8b)

诗歌不是虚拟环境的替代品。它为他们提供了管理环境的智能方法等。

以下是我对诗歌一见钟情的原因。

# 与 Virtualenv 不同，我可以用诗歌重命名和重新定位我的项目。

虚拟环境与特定路径相关联。如果我移动或重命名项目文件夹，原始路径不会随之改变。

如果我想做，我会有很大的麻烦。

每次更改路径时，我都会创建一个新的虚拟环境，并再次安装软件包。

很辛苦。

Virtualenv 有一个`— -relocatable`标志来帮助它。但是这个选项我也不满意。每次我安装一个新的包，我必须标记环境`— -relocatable`

这是恼人的重复！我相信数据科学家和开发人员有比记住每次都运行这个更大的问题。

## 诗歌项目是可重定位的。

诗歌将虚拟人与项目隔离开来。它会自动在 HOME 目录下的`.cache`文件夹中创建一个 env。当我重新定位项目时，我可以告诉 poems 在一个命令中使用相同的 env。

```
poetry env use <your env location>
```

如果您喜欢将 env 放在自定义位置，可以用同样的方式指定路径。这样你就可以把它和外部环境联系起来。

我发现它对测试非常有用。如果我的代码需要兼容不同的 Python 版本，我可以随时更换解释器。

```
poetry env use python3.8
poetry env use python3.6
```

# 在诗歌中，我可以单独管理开发依赖。

这是虚拟环境的一个明显的缺点。

Virtualenv 在隔离的环境中管理依赖关系。但是他们并没有专门为开发而维护一套。

然而，诸如 black、flake8 和 isort 之类的 Python 包只在开发时需要。它们在生产服务器中没有任何用途。

我还必须格外小心生产服务器上开发包的 [***安全漏洞。***](https://www.theregister.com/2021/07/28/python_pypi_security/)

我通常维护两个 requirements.txt 文件来区分它们。但这种做法是非常无效的。我可以使用 pip freeze 来更新开发版本。但对于制作版，我必须手动编辑。

这足以让你沮丧地度过一整天。

另一方面，诗歌有智能的方法来管理项目依赖性。

当向项目中添加一个新包时，我可以使用-D 标志指定它是否仅用于开发。

```
poetry add -D black
```

当我在生产服务器上安装依赖项时，我可以使用`no-dev`标志来过滤掉开发依赖项。

```
poetry install --no-dev
```

我也可以用`remove-untracked`标志删除我过去使用的多余包。

```
poetry install --remove-untracked
```

管理 Python 项目的依赖关系并不容易。

# 诗歌确保团队成员之间版本的一致性。

如果你是团队工作，你会因为不一致而遇到问题。

人们使用不同版本的依赖关系。因此，代码要么中断，要么没有给出预期的结果。

当然啦！pip freeze 命令可以捕获软件包的版本。但是，他们没有获得 Python 解释器版本。此外，如果您手动将一个包添加到需求文件中，并且没有指定版本，这将会产生不一致。

诗歌有保持一致性的巧妙方法。

`pyproject.toml`文件相当于 virtualenv 中的 requirement.txt。但是当诗歌安装包时，它首先检查是否有一个`poetry.lock`文件可用。如果是这样，它将从锁文件中获取依赖关系。

您不需要手动编辑锁定文件。每次您更改项目依赖项时，poems 都会创建并更新它。您可以使用 poems add 命令，也可以在 TOML 文件上指定依赖项并运行 install 命令。

```
poetry add pandas
```

诗歌文档鼓励您将锁文件提交到代码库中，并与其他成员共享。因此，当他们建立依赖关系时，总是与他人保持同步。

# 打包和发布 python 包。

如果您将包发布到 PyPI 或其他存储库，您必须以有助于索引的方式构建它们。对我来说，这不是一项容易的任务。

这涉及到许多配置，它们肯定会阻碍新作者。

然而，有了诗歌，我可以毫不费力地将包发布到任何存储库中。

这是我用诗歌向 PyPI 发布的的[包。做这件事不超过几分钟。](https://pypi.org/project/python-eda/)

这个包帮助您在一个终端命令中为任何数据集生成 HTML 分析报告。

猜猜看，这是 pip installable:

```
pip install python-eda
```

您可以在这个 [GitHub 资源库](https://github.com/ThuwarakeshM/python-eda)中找到源代码。另外，如果你喜欢这个包，你可以看看我写的关于它的文章。

[](/how-to-do-a-ton-of-analysis-in-the-blink-of-an-eye-16fa9affce06) [## 如何在眨眼之间用 Python 做一吨的分析？

### 使用这些 Python 探索性数据分析，将您的数据探索时间缩短到原来的十分之一…

towardsdatascience.com](/how-to-do-a-ton-of-analysis-in-the-blink-of-an-eye-16fa9affce06) 

在结束之前，我想带你看一下我发布这个包的具体步骤。

# 让我们创建并发布一个带有诗歌的 Python 项目。

与 Virtuelenvs 不同，在 Virtuelenvs 中创建项目文件夹，然后创建 env，我可以直接创建诗歌项目。诗歌自动把一个项目结构和初始文件。

```
poetry init python-eda
cd python-eda/
```

下一步，我用-D 标志安装了项目的核心依赖项和开发依赖项。

```
poetry add pandas sweetviz typer -D black flake8 isort pre-commit
```

我不打算解释我如何使用开发依赖来保持这篇文章的简洁。但是您可以找到无数关于如何使用这些包来维护干净代码的资源。

然后我在 python_eda 文件夹中添加了一个名为`main.py.`的文件，我用我之前的[文章](/how-to-do-a-ton-of-analysis-in-the-blink-of-an-eye-16fa9affce06#50f4)中的代码替换了它的内容。这是我的应用程序中所有内容的入口点。

至此，一切都是普通的 Python 应用。

现在，我们在`pyproject.toml`文件中添加一小段代码来讲诗，这是你的切入点。

```
[tool.poetry.scripts]
pyeda = "python_eda.main:app"
```

我们在这里做什么？我们在 python_eda 文件夹中的 main.py 中调用该应用程序。

现在，只需一个命令，您就可以构建应用程序。

```
poetry build
```

这将在你的项目中创建一个`dist`文件夹，包含你的项目的车轮和焦油文件。

要在本地测试项目，您可以运行`poetry install`，并且您将能够使用 CLI 来生成 EDA 报告。

```
$ poetry install
$ pyeda report https://raw.githubusercontent.com/ThuwarakeshM/analysis-in-the-blink-of-an-eye/main/titanic.csv
```

要将您的包发布到 PyPI，您需要一个帐户并创建一个 API 令牌。你可以从[官方文件](https://packaging.python.org/tutorials/packaging-projects/)中找到更多信息。

一旦有了 API 令牌，您只需要多两行命令。

```
$ poetry config pypi-token.pypi <TOKEN> 
$ poetry publish
```

厉害！应用程序已发布。

现在，python-eda 可以通过 pip 进行安装。

# 总而言之，

多年来，我在每个项目上都使用 Virtualenv。使用它们的唯一优势是隔离的环境和列出项目依赖关系。

但即使在那时，使用它也有几个问题，例如

*   难以区分开发和生产依赖关系；
*   无法重新定位或重命名项目文件夹；
*   难以在团队之间保持一致的环境，以及。
*   打包和发布时有许多样板文件。

诗歌是所有这些问题的一站式解决方案。它满足了我长期以来对一个类似 npm 的 Python 包管理器的渴望。

> *感谢阅读，朋友！在* [**上跟我打招呼 LinkedIn**](https://www.linkedin.com/in/thuwarakesh/) *，*[**Twitter**](https://twitter.com/Thuwarakesh)*，* [**中**](https://thuwarakesh.medium.com) *。看来你和我有许多共同的兴趣。*

还不是中等会员？请使用此链接 [**成为会员**](https://thuwarakesh.medium.com/membership) 因为，在没有额外费用的情况下，我为你引荐赚取一小笔佣金。