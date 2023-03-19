# 飞机上的蛇

> 原文：<https://towardsdatascience.com/snakes-on-a-plane-75b5b95612e0?source=collection_archive---------29----------------------->

## **使用 Python 包分发系统运送您的 Snakemake 管道**

![](img/b4594e0a94eb1bf9a9ccc1fd93e97af6.png)

图片由 pch.vector / Freepik 提供

Snakemake 是最流行的数据科学工作流管理包之一，但是它缺乏分发您创建的工作流(管道)的功能。在这篇文章中，我将演示如何通过使用 Python 包分发系统来添加这样的功能。使用这种分配系统有以下好处:

*   在可安装包中将工作流分发给无法访问您的基础架构的其他数据科学家
*   向管道添加版本控制
*   减少 Snakemake 的样板文件(例如必需的参数`—-cores`)

遵循这些指导方针，安装和运行 snakemake 工作流将像执行以下命令一样简单:

# 步骤 0:先决条件

要构建一个包，需要(可发现的)Python 3 安装。最简单的方法是创建一个包含 Python 3 的 Conda 环境。在这篇文章中，我在 Unix 系统上使用了 Python 3.9。要继续学习，需要一些使用 Conda 和 Snakemake 的经验。

# 步骤 1:创建包装材料

![](img/25cdaa1b3e38edfadb23716ba1abf8da.png)

图片来自 burst.shopify.com

对于这个例子，我们将创建一个 Python 包(称为 snakepack ),它使用一个简单的内部 Snakemake 管道将所有的`.txt`文件从输入目录复制到输出目录。

为了模拟真实情况，我们将创建两个单独的文件夹，一个包含包文件，另一个包含用户用来运行包的输入和配置文件。

**包文件夹结构**

这个结构将包含创建实际包的所有文件。根文件夹包含构建包所需的文件。snakepack 子文件夹包含实际的模块。snakepack 子文件夹中是 snakemake 文件夹，其中包含管道文件。

这种结构可以例如通过运行

这些文件一直是空的，直到我们以后把它们填满。

**用户文件**

这些文件夹包含将用于演示管道工作的文件。它们可以位于任何位置，但在本例中，它们将位于个人(`~`)文件夹中:

这种结构可以例如通过运行

# 第二步:锻造管子进行包装

![](img/9b4c8449a204a9c6f774a6bd96d59939.png)

图片来自 burst.shopify.com

让我们从在 Snakemake 文件夹中创建 snakemake 管道开始，看看它在没有包包装的情况下是否工作。因为我们希望我们的包的用户能够配置管道，所以我们将使用 Snakemake 的配置文件来定义输入和输出目录，并将它们传递给 Snakemake。

**Snakefile**

在我们的`snakepack_files`文件夹中(见上文),我们可以填写配置文件:

**config.yaml**

## 测试

现在可以通过命令行从`snakemake`文件夹运行该管道

# 第三步:打包

![](img/0cddcb86d27433c2ab0eb721163e0df6.png)

图片来自 burst.shopify.com

我们将围绕现有的 snakemake 管道创建一个包包装器，它允许我们创建一个包含包并可以分发的`.whl`文件。使用上述文件夹/文件结构填写这些文件:

**copyfiles.py**

因为我们希望能够在安装包之后从命令行调用管道，所以这个文件充当一个包装器，它接受命令行参数并使用它们来启动 snakemake 管道。一个优点是，我们可以控制用户设置哪些参数，以及我们自动固定或确定哪些参数。

在这种情况下，我们可以区分以下参数:

*   用户定义的:配置文件的位置
*   修正:snake 文件在包中的位置
*   自动确定:可用 CPU 的数量

copyfiles.py

**requirements.txt**

该文件包含 Python 包的要求。在这种情况下，snakemake 是一个需求。我们还包括了曼巴，因为这优化了蛇制造的使用。

requirements.txt

当您稍后安装 snakepack 包时，这些需求将被自动下载和安装。

**setup.py**

`setup.py`文件将为 python 包工具提供如何创建包的指导。

setup.py

`setup`命令指示包构建器这是包 snakepack 的 0.0.1 版本。`find_packages()`调用确保 python 模块将包含在轮子中。`install_requires`定义了我们从`requirements.txt`读入的需求。最后，我们将`include_package_data`设置为`True`，并指向包含在包中的 snakemake 目录。

## 测试

一旦创建了上述文件，您就可以测试您的包是否可以在开发模式下安装，并且可以运行该模块。

这将运行管道并复制文件。之后删除复制的文件。

# 第四步:起跳！

![](img/9b9afe80bae36a1a02090a7df58a38c9.png)

图片来自 burst.shopify.com

如果第 3 步成功，您就可以构建实际的包了:

这将在名为`dist/`的新子文件夹中创建一个 wheel ( `.whl`)文件。这个轮子可以和别人分享。

## 测试

让我们试试它是否安装并运行:

# 结论

这篇文章使用了一个非常简单的 snakemake 管道来展示它可以通过包含在一个定制的 Python 包中来进行分发。如果您经常使用 Snakemake，并且希望轻松地共享和版本控制您的管道，那么您可以使用更复杂的管道来扩展所提供的框架，并根据您的需要进行定制。

# 来源

*   [https://snakemake.readthedocs.io/en/stable/](https://snakemake.readthedocs.io/en/stable/)
*   https://packaging.python.org/overview/