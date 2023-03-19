# 构建您自己的 arcpy 助手包

> 原文：<https://towardsdatascience.com/build-your-own-arcpy-helper-package-6842e2f6d339?source=collection_archive---------32----------------------->

## 告别剪切粘贴 Python

![](img/22255d725029bf414258f788d0eca920.png)

沃洛季米尔·赫里先科在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Esri 的 arcpy package for Python 是最全面的地理信息管理、分析和可视化工具包。

这种详尽的功能集是以代码的简洁性为代价的。看似简单的操作实现起来可能会很复杂，Esri 的 ArcGIS 软件可能会增加学习曲线。

Pythonic 的格言*应该有一个……显而易见的方法去做*并不一定适用。

由于技术需求和个人偏好的混合，每个组织都将以自己的方式组装和配置脚本。结果可能是大量重复的代码，以及复制和粘贴的强烈诱惑。更糟糕的是，在一个部门内，同样的行为可能以不同的方式实施。

![](img/79318dec8349ce4dbf2410acd8de565d.png)

轮子:不要重新发明它。乔恩·卡塔赫纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

软件工程的一个关键原则是不要重复你自己(干)。 [Python 的模块框架](https://docs.python.org/3/tutorial/modules.html)是为帮助我们实现这一目标而量身定制的。然而，从地理学家-GIS 分析师(像我一样)的角度来看，当您第一次面对一项复杂的任务时，将 arcpy 与模块开发结合起来可能会感觉像以毒攻毒。这当然是一个我推迟了几年的问题！

事实上，抽象掉重复的逻辑有很多好处。它确保了一致性，有助于维护，并增加了脚本的可读性。您可以进行渐进的改进，并立即看到它们在所有项目中的应用！短期的痛苦伴随着明确的长期收益。

下面是你需要知道的开始。我假设您已经安装了 ArcGIS Pro，并且可以从命令行运行 python 和 pip。你可以在这里找到下面所有代码[的模板。](https://github.com/countmapula/archelpy)

**结构**

我认为 90%以上的 arcpy 项目都是由单独的脚本组成的。

当我们刚刚找到一种方法来做我们一直试图做的事情时，我们不倾向于用额外的复杂性把它弄乱。

没有必要每次想要自动化某件事情时都创建一个成熟的 Python 项目。然而，当我们包装所有最重要的代码时，我们应该尝试遵循最佳实践。

假设我们有以下 c:\files\parent_folder 文件夹结构

```
parent_folder module_name tests
```

我用一个很基本的例子。假设我们想要调用 arcpy。每次我们调用 AddMessage()时都以一种特殊的方式。我们将调用我们的包 archelpy，并创建一个 messages.py 模块。

```
archelpy archelpy __init__.py
        messages.py tests
```

这个基本的 *messages.py* 示例在脚本工具界面中将时间戳应用于 arcpy 消息。这有助于确定漫长流程中哪个部分最耗时。

消息. py

您可以决定创建相应的模块来管理属性域、索引、切片缓存或任何数量的其他技术问题，

```
archelpy archelpy __init__.py
        caching.py
        domains.py
        indexes.py        
        messages.py tests
```

**测试**

单元测试是另一个经常被归为“太难”的话题。

对于许多工作流来说，编写测试可能是多余的。另一方面，它们可能非常有益。

据说[可测试代码是高质量的代码](https://dzone.com/articles/write-testable-code#:~:text=Testable%20code%20is%20code%20of,straightforward%20to%20maintain%20and%20extend.)。测试鼓励深思熟虑的设计，不鼓励过于复杂或混乱的代码。测试还以其他开发人员能够理解的方式记录了工具的目的和预期行为。随着项目的发展，他们通过验证行为和确保质量来提供安心。

Python 内置的 unittest 框架允许我们检查我们的逻辑是否以我们期望的方式调用了 arcpy(或其他第三方模块)。

```
archelpy
__init__.py archelpy __init__.py
        messages.py tests __init__.py
        test_messages.py
```

测试消息. py

注意*测试 _*。命名约定很重要。*

从父目录中运行下面的命令将通过在整个项目中搜索测试文件来执行我们的测试套件，

```
cd c:\files\archelpypython -m unittest discover -v
```

在这种情况下，我们应该看到这样的结果，

```
testAddMessageParameters (tests.test_messages.HelpyMessagesTestCase)Uses assert_called_with() to make sure that our code passes ... ok----------------------------------------------------------------------Ran 1 test in 0.003sOK
```

在实践中，您将创建更多的测试，并将它们组织到不同的文件和目录中。这时，unittest 中的 discover 特性变得特别有用。

**打造**

我们已经准备好分发包裹了。如果你想大方一点，你可以开源它并把它添加到 PyPi 中。对于专有工作，您更愿意将安装资源保存在安全的地方。

```
cd c:\<path>\archelpy
```

ArcGIS Pro 的 Python 安装附带了 setuptools 和 wheels，它们会创建一个名为 wheel 文件的 pip 兼容安装程序。whl)。

我们在名为 setup.py 的文件的顶层目录中定义我们的包属性，

```
archelpysetup.py
```

setup.py 模板

运行以下命令，

```
python setup.py bdist_wheel
```

这将在项目中生成许多工件，包括一个. whl 文件，您可以用它来安装您的模块！

```
archelpy dist        archelpy-0.1.0-py3-none-any.whl
```

**安装**

使用 pip 来安装使用新创建的 wheel 文件的模块，

```
pip install c:\files\archelpy\archelpy-0.1.0-py3-none-any.whl
```

您应该会看到以下结果，

```
Processing c:\files\archelpy\archelpy-0.1.0-py3-none-any.whl
Installing collected packages: archelpy
Successfully installed archelpy-0.1.0
```

**使用！**

像这样在脚本工具中导入 archelpy，

```
from archelpy import messagesmessages.time_message("Hello!", arcpy)
```

你已经准备好了！当您对软件包进行改进时，使用以下命令重新构建 wheel 文件并更新安装，

```
pip install <wheel file> --upgrade
```

快乐脚本！

![](img/89de5b8695d1a371eaf7e43cdc2aa9e4.png)

由 [uomo libero](https://unsplash.com/@uomo_libero?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**感谢**

参见起亚 Eisinga 的[这篇](https://medium.com/analytics-vidhya/how-to-create-a-python-library-7d5aea80cc3f)文章，对包装进行了更深入的讨论。

参见来自帕特里克·肯尼迪的[这篇](https://www.patricksoftwareblog.com/python-unit-testing-structuring-your-project/)文章，以及来自 Ian Firkin 的[这篇](https://notesfromthelifeboat.com/post/testing-arcpy-2/)文章，分别了解关于测试 Python 和 arcpy 的更多细节。