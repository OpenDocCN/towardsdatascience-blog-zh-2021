# 在一行代码中导入所有 Python 库

> 原文：<https://towardsdatascience.com/import-all-python-libraries-in-one-line-of-code-86e54f6f0108?source=collection_archive---------2----------------------->

## 为写多个导入语句而烦恼？让 PyForest 为你做这项工作

![](img/47657059728638679e5549db0cc58d29.png)

由[哈姆扎·蒂格扎](https://unsplash.com/@hamzatighza?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是数据科学家用于数据科学项目的主要语言，因为存在数千个开源库，可以简化和执行数据科学家的任务。超过 235，000 个 Python 包可以通过 PyPl 导入。

在数据科学案例研究中，需要导入多个库和框架来执行任务。每次数据科学家或分析师启动新的 jupyter 笔记本或任何其他 IDE 时，他们都需要根据自己的需求导入所有的库。有时，反复编写多行相同的 import 语句会令人沮丧。在这里 pyforest 图书馆来拯救你，它为你工作。

# Pyforest 图书馆:

![](img/54a1ff5c526b5d7b169783892d9582d0.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2592628) 的 [StockSnap](https://pixabay.com/users/stocksnap-894430/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2592628)

Pyforest 是一个开源的 Python 库，使数据科学家能够感受到自动化导入的幸福。在进行数据科学案例研究时，需要导入多个包或库，如 pandas、matplotlib、seaborn、NumPy、SkLearn 等。每次都导入所有这些库可能会很无聊，并且会破坏您工作的自然流程。此外，您无需搜索确切的导入语句，如`**from sklearn.ensemble import RandomForestClassifier**` **。**

使用 pyforest one 可以克服这些问题。Pyforest 使您能够使用所有您喜爱的库，而无需导入它们。Pyforest 通过自动导入您想要用于案例研究的库来为您完成这项工作。

一旦在一行中导入了 pyforest 库，现在就可以像平常一样使用所有的 python 库了。您使用的任何库都不会被导入，Pyforest 会自动为您导入。这些库只有在您调用它们或创建它们的对象时才会被导入。如果没有使用或调用某个库，pyforest 不会导入它。

## 安装:

可以使用以下命令从 Pypl 安装 Pyforest:

```
**pip install pyforest**
```

安装完这个库之后，您只需要用一行代码导入它。现在你可以像平常一样使用你最喜欢的库了，不用写导入。在下面的示例 jupyter 笔记本中，我们没有导入 pandas、seaborn 和 matplotlib 库，但是我们可以通过导入 pyforest 库来使用它们。

(作者代码)

## pyforest 可以导入所有库吗？

这个包旨在添加所有占导入量 99%以上的库，包括热门库如`pandas`为`pd`、`NumPy` 为`np`、`matplotlob.pyplot`为`plt`、`seaborn` 为`sns`等等。除了这些库之外，它还有一些助手模块，如`os`、`tqdm`、`re`等等。

如果您想查看库列表，请使用`**dir(pyforest)**`。

要将您自己的导入语句添加到 pyforest 库列表中，请在您的主目录`**~/.pyforest/user_imports.py**` 中的文件中键入您的显式导入语句。

## pyforest 的功能:

Pyforest 会在需要时自动调用 python 库。Pyforest 提供了一些函数来了解库的状态:

*   **active_imports()** :返回已经导入并正在使用的库列表。
*   **lazy_imports()** :返回 pyforest 库中要导入的所有库的列表。

# 结论:

在本文中，我们讨论了 pyforest 库，这是一个自动导入库。使用这个库可以减少导入大量必要库的压力，相反，它会自动导入需求。对于经常使用新的 jupyter 笔记本来探索数据科学案例研究的数据科学家来说，这个库很有帮助，现在他们可以不用导入库，从而加快他们的工作流程。

# 参考资料:

[1] Pyforest 文件(2020 年 4 月 17 日):[https://pypi.org/project/pyforest/](https://pypi.org/project/pyforest/)

*喜欢这篇文章吗？成为* [*中等会员*](https://satyam-kumar.medium.com/membership) *继续无限制学习。如果你使用下面的链接，我会收到你的一小部分会员费，不需要你额外付费。*

<https://satyam-kumar.medium.com/membership>  

> 感谢您的阅读