# 4 个 Python 库，使得处理大型数据集更加容易

> 原文：<https://towardsdatascience.com/4-python-libraries-that-ease-working-with-large-dataset-8e91632b8791?source=collection_archive---------8----------------------->

## 阅读和处理大型数据集的基本指南

![](img/2415e6afa9a902a2fef67a19ce414f5c.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=498396)

Pandas 是数据科学社区中流行的 Python 库之一。它为 Python 语言提供了高性能、易于使用的数据结构和数据分析工具。但是在处理大规模数据时，它却失败得很惨。

Pandas 主要使用单个 CPU 内核来处理指令，并且没有利用在 CPU 的各个内核之间扩展计算来加速工作流。Pandas 在读取大型数据集时会导致内存问题，因为它无法将大于内存的数据加载到 RAM 中。

还有其他各种 Python 库，它们不会一次加载大量数据，而是与系统操作系统交互，用 Python 映射数据。此外，它们利用 CPU 的所有内核来加速计算。在本文中，我们将讨论 4 个可以读取和处理大型数据集的 Python 库。

```
***Checklist:***
**1) Pandas with chunks
2) Dask
3) Vaex
4) Modin**
```

# 1)大块阅读使用熊猫:

Pandas 将整个数据集加载到 RAM 中，而在读取大型数据集时可能会导致内存溢出问题。其思想是以块为单位读取大型数据集，并对每个块执行数据处理。

样本文本数据集可能有数百万个实例。这个想法是在每个块中加载 10k 个实例(第 11–14 行)，对每个块执行文本处理(第 15–16 行)，并将处理后的数据附加到现有的 CSV 文件中(第 18–21 行)。

(作者代码)

# 2) Dask:

Dask 是一个开源的 Python 库，它提供了对大于内存的数据集的多核和分布式并行执行。Dask 提供了该功能的高性能实现，可在 CPU 的所有内核上并行实现。

Dask 提供了类似于 Pandas 和 Numpy 的 API，这使得开发者在库之间切换变得很容易。

(作者代码)

> 阅读我以前关于 dask 的文章，深入了解 Dask 数据框架

</how-dask-accelerates-pandas-ecosystem-9c175062f409>  

# 3) Vaex:

Vaex 是一个 Python 库，它使用一个**表达式系统**和**内存映射**来与 CPU 交互，并在 CPU 的各个内核之间并行化计算。Vaex 覆盖了 pandas 的一些 API，对于在标准机器上执行大型数据集的数据探索和可视化是有效的。

> 请阅读我以前关于 Vaex 的文章，了解如何使用 Vaex 来加速文本处理工作流。

</process-dataset-with-200-million-rows-using-vaex-ad4839710d3b>  

# 4)摩丁:

熊猫使用单个 CPU 核心来处理计算，而不是利用所有可用的核心。Modin 利用系统中所有可用的内核来加速 Pandas 的工作流程，只需要用户在笔记本上修改一行代码。

> 阅读我以前关于 Modin 的文章，更多地了解 Modin 如何扩展您的数据探索和可视化

</speed-up-your-pandas-workflow-by-changing-a-single-line-of-code-11dfd85efcfb>  

# 结论:

在本文中，我们介绍了 3 个 Python 库，它们让开发人员能够轻松处理大型数据集。Pandas 在读取 CSV 文件时将整个数据集加载到 RAM 中，这使得很难处理内存不足的数据。使用 Pandas 读取和处理文件使得处理大型数据集变得更加容易。

> 阅读我以前关于一些分布式库和类似主题的基本指南的文章:

*   [摩丁——加速你的熊猫笔记本、脚本和图书馆](/modin-speed-up-your-pandas-notebooks-scripts-and-libraries-c2ac7de45b75)
*   [达斯克如何加速熊猫生态系统](/how-dask-accelerates-pandas-ecosystem-9c175062f409)
*   [使用 Vaex 处理 2 亿行数据集](/process-dataset-with-200-million-rows-using-vaex-ad4839710d3b)
*   [Python 中多重处理的基本指南](/25x-times-faster-python-function-execution-in-a-few-lines-of-code-4c82bdd0f64c)

# 参考资料:

[1] Pandas 文档:[https://Pandas . pydata . org/docs/reference/API/Pandas . read _ CSV . html](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)

> 感谢您的阅读