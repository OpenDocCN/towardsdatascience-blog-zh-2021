# GIS tyc——基于 Python 的 GitHub GIST 管理工具包

> 原文：<https://towardsdatascience.com/gistyc-a-python-based-github-gist-management-toolkit-ee507d5a0e7b?source=collection_archive---------33----------------------->

## [*小窍门*](https://towardsdatascience.com/tagged/tips-and-tricks)

## GitHub GISTs 是在介质上显示代码片段的完美方式。但是，如何以可行的方式创建、更新和删除 GISTs，并将这样的过程集成到 CI/CD 管道中呢？

![](img/e41f0b42f0a08b15367a11f1a8baf861.png)

照片由[rich Great](https://unsplash.com/@richygreat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 使用 GISTs

媒体已经成为各种文章的重要来源。这还包括涵盖数据科学或机器学习主题的编程教程。但是覆盖实际的编码部分需要一样东西:适当的格式和编程语言依赖的颜色突出。有时人们会看到以下格式的代码片段:

```
# Import standard module
import time# Print the current Unix Time
print(time.time())
```

…这种格式对于几行代码来说完全没问题，但是用这些灰色的块来覆盖整个教程会让人读起来很累。相反，大多数 Medium 上的作者使用 [GitHub GIST](https://gist.github.com/) 。GitHub 提供了一个创建要点的简单链接，允许用户在介质上显示正确格式化的代码。以下要点…

要点示例

…只需在 Medium 文本编辑器中添加以下链接即可(灰色块样式阻止 Medium 正确格式化要点):

```
[https://gist.github.com/ThomasAlbin/1b42f2fbe6470ae54286b83b57f5acd6](https://gist.github.com/ThomasAlbin/1b42f2fbe6470ae54286b83b57f5acd6)
```

![](img/c64202661c6432b26c25d2d0ccc692a3.png)

由 [Finn Whelen](https://unsplash.com/@finnwhelen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 维护要点的问题是

Medium 有数百甚至数千篇文章，涵盖了编程相关的主题。但是，旧的文章可能会引用已经过时的库和更新的工具，它们的功能发生了变化。结果是一些显示的要点代码片段不再工作了。提出的异常、警告和错误结果会抑制阅读有希望的标题或摘要后的兴奋感。考虑到作者对代码进行了测试，并且审阅者或编辑对文章进行了彻底的检查，我们只能假设最近的文章工作正常。

但是一个人(例如，一篇文章的作者)如何保持要点的更新呢？手动编辑大量的 GISTs 既乏味又麻烦。还有一个 [GitHub GIST REST API](https://docs.github.com/en/rest/reference/gists) ，但是有一个简单的解决方案会更好。

为此，我开发了一个具有 CLI 功能的基于 Python 的工具包，我想在这里介绍一下: ***gistyc*** 。 *GISTsyc* 应使用户能够从外壳或 Python 程序中创建、更新和删除 gist。它的功能、特性和 GitHub 动作集成(新的 GIST 代码的持续部署)是本文的一部分。

[](https://github.com/ThomasAlbin/gistyc) [## ThomasAlbin/gistyc

### gistyc 是一个基于 Python 的库，使开发者能够创建、更新和删除他们的 GitHub GISTs。客户端…

github.com](https://github.com/ThomasAlbin/gistyc) 

总的来说，gistyc 很容易理解。让我们举个例子:

*   您有一个想要向全世界展示的 Python 文件！就叫牛逼吧. py。
*   awesome.py 要在一篇中等文章里详细解释。这个文件很大，有 200 多行代码。一个要点对于一篇教程文章来说太大了。
*   无论如何，你可以简单地将你的代码分割成单独的要点或者要点中的“子部分”。手动完成这个过程是乏味的…如果你以后需要更新它，由于一些读者抱怨错误，它变得更糟…
*   awesome.py 具有专用的“编码块”，例如由“# % %”([Spyder-way](https://docs.spyder-ide.org/current/editor.html#defining-code-cells))分隔。这些单元格应该是你想要显示的要点部分。
*   现在是*GIS tyc*:*GIS tyc*获取你的文件，将它分割成单独的部分(取决于代码块分隔符)并自动创建要点。如果你已经有了一个名为“awesome.py”的要点， *gistyc* 会相应地更新要点！第一个 GIST 代码-cell 的名称是“awesome.py”，第二个得到后缀“_1”，导致“awesome_1.py”等等。稍后你会看到一个例子。
*   您更新的要点会自动与您的中型文章同步，您就大功告成了！

这里只涉及几个陷阱:

1.  您应该坚持在您的 GIST 存储库中使用唯一的文件名(否则 *gistyc* 会与不明确的名称混淆并返回一个错误)。
2.  您还应该坚持代码单元格分隔。只编辑错误或异常，但不要改变代码的逻辑！否则你的中型文章不符合代码和你的读者越来越困惑后，你更新它！

# 先决条件和安装

在我们深入研究 gistyc 之前，我们需要满足一些先决条件才能使用它的全部特性列表。除了安装 Python，你还需要一个 [GitHub](https://github.com/) 帐户来创建和存储 GISTs。之后，你只需要遵循这三个步骤:

1.  安装 Python ≥3.8(建议:使用虚拟环境)
2.  您需要一个 *GitHub 个人访问令牌*和 GIST 访问:

*   点击您的个人账户资料(右上角)
*   点击**设置**
*   在左侧菜单栏中，进入**开发者设置**，选择**个人访问令牌**
*   **生成新的令牌**并写下您的令牌的名称(注释)。注释不影响功能，但选择描述令牌用途的注释，例如 *GIST_token*
*   在**要点** ( *创建要点*)处设置一个标记，并点击页面底部的**生成令牌**
*   重要提示:显示的令牌只出现一次。将其复制并作为一个秘密存储在 GitHub 项目中和/或作为一个环境变量存储在本地。

3.通过以下方式安装 *gistyc* :

```
pip install gistyc
```

或者，可以随意下载[库](https://github.com/ThomasAlbin/gistyc)来下载和使用所提供的测试。

现在我们准备和吉斯泰克一起工作。以下部分描述了 Python 函数调用，第二部分概述了对 CD 管道更感兴趣的 CLI 功能！

## Python 要点准备

***请注意:当前版本仍处于测试阶段，仅适用于 Python(。py)文件。将来会有更新来涵盖更多的通用解决方案。***

为了测试或使用 *gistyc* 在我们的设备上准备一个简单的 Python 文件。结局还得是*。py* 。

此外，要点可以被分成几个文件。这可能有助于将您想要一步一步解释的较大的教程脚本分割开来。下面的例子(我们将在后面更详细地讨论这个例子)给你一个印象，它意味着什么:

看一看 gistyc 提供的示例脚本:[示例脚本](https://github.com/ThomasAlbin/gistyc/blob/main/examples/example_sub_dir/gistyc_example2.py)

作为一个单独的要点，脚本存储在…

```
[https://gist.github.com/ThomasAlbin/77974cc3e14b2de8caba007dfa3fccf9](https://gist.github.com/ThomasAlbin/77974cc3e14b2de8caba007dfa3fccf9)
```

…格式为:

另一个 GIST 示例名为“gistyc_example2.py”

如你所见，提供了 [Spyder](https://www.spyder-ide.org/) 代码块分隔符“#%%”。gistyc 识别这些代码分隔符，并在单个 GIST 中创建单独的文件(或“子 GIST”)。分隔符可以由用户设置，但在本文中，我们将坚持 Spyder 风格。

以上示例是 *gistyc* 官方 GIST 示例的一部分，可在以下位置找到:

```
[https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc)
```

此外，这三个独立的块可以通过以下方式调用:

## 1.第一代码单元

```
[https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2.py](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2.py)
```

gistyc_example2.py 的第 1 部分—通过[https://gist . github . com/Thomas Albin/caddb 300 AC 663 e 60 AE 573 b 1117599 FCC 调用？file=gistyc_example2.py](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2.py)

## 2.第二代码单元

```
[https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2_1.py](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2.py)
```

gistyc_example2.py 的第二部分—通过[调用 https://gist . github . com/Thomas Albin/caddb 300 AC 663 e 60 AE 573 b 1117599 FCC？file=gistyc_example2_1.py](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2_1.py)

## 3.第三代码单元

```
[https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2_2.py](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2_2.py)
```

gistyc_example2.py 的第三部分—通过[调用 https://gist . github . com/Thomas Albin/caddb 300 AC 663 e 60 AE 573 b 1117599 FCC？file=gistyc_example2_2.py](https://gist.github.com/ThomasAlbin/caddb300ac663e60ae573b1117599fcc?file=gistyc_example2_2.py)

> 总体而言，用户的(*用户* ) GIST 及其值( *GIST_VALUE* )和相应的文件名( *GIST_NAME* )以及后缀依赖子节( *ID* )具有以下结构:
> 
> https://gist.github.com/USER/GIST_VALUE?fileGIST_NAME_ID.py

![](img/4a52bf5581377186de26ef0641b6bfc2.png)

[David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# Python 中的 gistyc

Python API 目前提供 4 个通用函数来创建、更新和删除 GIST。此外，它允许用户获得所有可用 gist 的当前列表。

我们有以下参数:

*   AUTH_TOKEN:是 GIST 访问令牌(字符串)
*   FILEPATH:是 Python 文件的绝对或相对路径(string 或 [pathlib。路径](https://docs.python.org/3/library/pathlib.html#module-pathlib)
*   要点 ID:要点(字符串)的 ID*。我们一会儿会谈到这个参数*

## *创建一个要点*

基本上所有的 *gistyc* 调用都是从同样的两行代码开始的:首先是库的表面导入，其次是 gistyc 类的实例化(第 5 行)。这个类只需要 GIST 身份验证令牌。返回的类，这里命名为 *gist_api* ，然后用于调用相关的方法。

以下示例显示了如何创建要点。该类调用方法 *create_gist* ，其输入是文件路径。返回值( *response_data* )是来自 GitHub 服务器的回调 JSON，包含新创建的 GIST 的元信息(如时间戳、GIST 的 ID 等)。).

使用 gistyc Python API 创建 GIST

## 更新要点

有两种方法可以更新要点。首先，让我们假设所有的要点名称都是唯一的，不会重复。

同样，我们创建了这个类(第 5 行)并在第 8 行调用了方法 *update_gist* ,这里我们只是提供了文件路径作为输入。返回的 JSON 提供回调信息。

使用 gistyc Python API 更新 GIST(仅使用 Python 文件的文件路径)

然而，让我们假设你有几个同名的 gist***(顺便说一下:为了保持整洁，避免重复/同名的 gist！)*** 。如果只使用文件名调用更新函数，就会引发一个异常(我称之为 AmbiguityError)。在这种情况下，你还需要知道你要点的要点 ID。然后，您可以通过以下方式更新您的要点:

使用 gistyc Python API 更新 GIST(仅使用 Python 文件的文件路径和相应的 GIST ID)

## 获取所有 GISTs

有时候知道你所有的要点是有用的，尤其是当你有上百个要点的时候。在一个单独的数据库中维护它们或者手动获取所有的 GIST IDs 可能会令人精疲力尽…

为此，实现了 *get_gists* 方法，该方法通过所有的 GIST 页面调用 REST API，并将所有结果附加到最终列表中。每个元素都是一个 JSON 元素，代表一个要点。

使用 gistyc Python API 获得所有 GitHub GISTs 的列表

## 删除要点

最后，我们来看看如何删除一个要点。该过程类似于更新例程。如果您有具有唯一文件名的 GISTs，那么 FILEPATH 是唯一需要的输入参数…

使用 gistyc Python API 删除 GIST(仅使用 Python 文件的文件路径)

…否则，您需要提供一个要点 ID:

使用 gistyc Python API 删除 GIST(仅使用 GIST_ID)

# gistyc 作为 CLI

使用 [click](https://click.palletsprojects.com/en/7.x/) 可以创建基于 Python 的 CLI 工具，可以从 shell 中直接调用这些工具。gistyc 也具有部分基于上述功能的 CLI 功能。

我们有以下参数:

*   AUTH_TOKEN:是 GIST 访问令牌(字符串)
*   FILEPATH:是 Python 文件(字符串)的绝对或相对路径
*   GIST _ ID:GIST(字符串)的 ID

***请注意:代码单元格分隔符必须是“#%%”。CLI 的分隔符输入参数将跟在后面。***

## 创造一个要点

首先， *gistyc* 需要一个标记来指示所需的功能(创建、更新、删除),后跟输入参数。所有 CLI 调用都需要一个允许更改要点的身份验证令牌。

下面的 bash 片段基于 FILEPATH 创建了一个 GIST。

```
gistyc --create --auth-token AUTH_TOKEN --file-name FILEPATH
```

## 更新要点

类似地，如前所示，我们有一个 CLI 调用来基于文件名称更新 GIST，并且…

```
gistyc --update --auth-token AUTH_TOKEN --file-name FILEPATH
```

…如果文件名不唯一，则带有相应的 GIST ID:

```
gistyc --update --auth-token AUTH_TOKEN --file-name FILEPATH --gist-id GIST_ID
```

## 删除要点

类似地，可以通过 CLI 删除 GISTs 通过提供文件名…

```
gistyc --delete --auth-token AUTH_TOKEN --file-name FILEPATH
```

…或要点 ID:

```
gistyc --delete --auth-token AUTH_TOKEN --gist-id GIST_ID
```

# gistyc 目录 CLI

为单个文件调用 gistyc CLI 是管理您的 GISTs 的一种快速可行的方法。然而，让我们假设您有一个更大的教程系列，包含几十个 Python 脚本和数百个代码单元。多次调用 *gistyc* 或者用几十个 *gistyc* 调用创建一个 bash 脚本可能会变得令人困惑。

为此，已经实现了第二个基于 *gistyc* 的 CLI:*GIS tyc _ dir*。

我们假设您将所有 Python 文件存储在一个名为 directory 的目录中。现在，您需要创建和/或更新所有相应的 GISTs。您只需拨打:

```
gistyc_dir --auth-token AUTH_TOKEN --directory DIRECTORY
```

请注意:文件名在 GIST repo 中必须是唯一的，因为这里不能提供 GIST ID。此外，您的文件也可以存储在子目录中。 *gistyc_dir* 递归搜索目录以找到所有 Python 文件。

调用 *GISTsyc_dir* 后，新的 GISTs 被创建，现有的 gist 也被创建！

![](img/e73132d78531d121789da32e49d85a3c.png)

[Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# CI/CD 中的 gistyc

*gistyc* 的 CLI 功能和 *gistyc_dir* 提供的目录功能是将其集成到类似 CI/CD 的管道中的良好基础。什么时候，为什么要为 GISTs 创造这样一个管道？

让我们考虑下面的例子:

*   您有一个主要的中型教程项目，其中涵盖了许多您希望存储在 GitHub GIST 上的 Python 示例。
*   所有 Python 脚本都存储在一个名为*的目录下的相应 GitHub 存储库中。/例/* 。
*   每次你在*上传新的脚本。/examples/* (例如，通过 Pull 请求并合并到主分支)应自动创建新的 GISTs。
*   此外，Pull 请求还可能包含一些读者已经解决的示例脚本的更正和错误修复。同样在这种情况下:您将您的更改合并到主分支中，管道会相应地自动更新相应的 GISTs。无需在繁琐的手动过程中更新 GISTs。

在 GitHub 上，CI/CD 管道被称为动作。管道逻辑存储在 YAML 文件中。下面的 YAML 文件展示了我的 [gistyc GitHub 库](https://github.com/ThomasAlbin/gistyc)中提供的一个例子。看一看【行动】，*。github/workflow* 目录和*。/examples/* 目录，以便更好地了解这个工作流是如何工作的。每次我在*里改变一些东西。/examples/* ，创建一个 Pull 请求，最后合并到主分支，所有代码部分来自*。/examples/* 创建新的 GISTs 或相应地更新现有的 GISTs。

YAML *gist-push.yml* 如下所示，工作方式如下:

第 8–11 行:如果*中有任何内容。/examples/* 文件夹发生变化，动作正在执行。

第 14–19 行:管道进一步检查主分支上是否已经执行了更改。我们不想更新来自另一个未审查或未测试分支的 GISTs。

第 21–36 行:GitHub 准备环境，其中…

第 37–38 行:… *gistyc* 正在安装(这里， *gistyc* 是从 pypi 安装的)

第 39–40 行:现在 *gistyc_dir* 被应用到*上。/examples/* 文件夹。创建新的 GISTs，并更新现有的 GISTs。

[gist-push.yml](https://github.com/ThomasAlbin/gistyc/blob/main/.github/workflows/gist-push.yml)

这个 YAML 文件可以适用于任何其他 GitHub 存储的项目。您需要设置一个包含 GIST 标记的项目密码。此外，您还可以添加更多功能，例如，使用[**pytest**](https://docs.pytest.org/en/stable/)**测试您的 Python 脚本。现在，在成功合并到主分支之后，您就可以自动创建和/或更新 GISTs 了。也可以随意更改示例目录。**

# 进一步的支持和想法

官方的 *gistyc* 库提供了一个示例目录和所示的 GitHub 动作 YAML 文件。如果您需要任何帮助或有任何想法，请使用 GitHub [问题](https://github.com/ThomasAlbin/gistyc/issues)页面，在本文下方写下评论或在 [Twitter](https://twitter.com/MrAstroThomas) 上给我发消息。

## 贡献

另外，随时欢迎投稿！需要做一些事情，比如 CLI 的代码块分隔参数，或者替换“ *gistyc* 只考虑 Python 文件”——这是一个更通用的解决方案的功能。

请从一开始就为每个功能或测试驱动开发(TDD)风格的工作添加测试。严格执行 PEP8 标准、静态类型和其他要求，并对每个拉取请求进行检查。所有测试项目的列表可以在[YAML 文件](https://github.com/ThomasAlbin/gistyc/blob/main/.github/workflows/python-package.yml)中找到。相应的配置文件存储在存储库的根目录中。

现在，我祝大家编码愉快。我期待您的评论和反馈，

托马斯