# Excel 不会消失

> 原文：<https://towardsdatascience.com/excel-wont-go-away-fb856378151d?source=collection_archive---------42----------------------->

## 关于如何拥抱它的一些想法

Excel 是世界上使用最广泛的软件包之一。它从 1985 年开始出现，大约有 7 . 5 亿用户。微软喜欢称 Excel 公式为“世界上最广泛使用的编程语言”。

Excel 擅长进行简单的分析，帮助人们理解数字。它是惊人的灵活和多才多艺。Excel 很受用户欢迎，但在 IT 部门却不那么受欢迎。这种流行的一个例子是对[《华尔街日报》的一篇文章](https://www.wsj.com/articles/finance-pros-say-youll-have-to-pry-excel-out-of-their-cold-dead-hands-1512060948)的反弹，这篇文章要求停止使用 Excel——“你可以拿走我的 Excel，在你从我冰冷、死气沉沉的手中拿走它之后。”

![](img/383aac4fd3a40ad7c5105a8f503e4515.png)

冰冷的手|摄影:[马特·福斯特](https://unsplash.com/@mfoster?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **这是什么意思？**

所有这些灵活性的缺点是 Excel 的范围已经扩大，只受到想象力的限制。需要建立一个会计系统，一个电网模型，创建一个数据存储系统…你可以在 Excel 中完成。但是你能做一些事情并不意味着你应该做。在许多方面，Excel 是企业 IT 部门的祸根，存在安全和存储问题。电子表格允许敏感数据被容易地共享，但是它们提供很少或没有保护。随着公司中的多个用户保存同一 Excel 文件的各自版本，不断膨胀的存储会产生问题。

# **这有什么关系？**

你会碰到无数的文章告诉你现在就停止使用 Excel。Python 对于执行分析来说更好/更快/更可靠。所有这些都是真的。然而，大约有 1000 万 Python 开发人员和 7 . 5 亿 Excel 用户，你可能不得不在某种程度上使用 Excel。

如果我们作为数据科学家和分析师的目标是让数据变得有用，我们需要用利益相关者或决策者想要使用的工具来交付我们的分析。这些工具中通常会有一个是 Excel。

![](img/70225354776803b3681310ad07ccbb1e.png)

正确的工具|照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Oxa Roxa](https://unsplash.com/@oxaroxa?utm_source=medium&utm_medium=referral) 拍摄

# **是什么让 Excel 如此受欢迎？**

Excel 与 MS Office 打包在一起，这帮助它成为全球办公室的默认分析工具。有大量的 Excel 用户，这意味着有大量的资源可以帮助你。在线或 office Excel 专家提供了大量帮助。

Excel 很直观，基本步骤很容易学会。数据立即可见，并且没有要执行的代码。可以直接录入信息，给人一种掌控感。在 Excel 中，调整数据、排序、过滤都是毫不费力的。您可以通过单击鼠标来更改格式。你甚至可以创建令人惊讶的吸引人的图表和仪表盘。

使用 Excel 比学习编码更容易。你可以剪切和粘贴，拖放，最重要的是，ctrl-z(撤销)删除任何最近的错误。有许多公式可供你使用。数据透视表、SUMIF 和 VLOOKUP 语句以及迷你图使汇总数据变得容易。灵活性意味着您可以自己进行研究或回答特定的查询，而完全不需要 IT 或商业智能的支持。您可以运行简单的回归模型而不会有太多的麻烦，运行更复杂的统计模型会有很大的麻烦。

Excel 在不断改进。像 Power Query 这样的工具使加载和转换数据变得更加容易，而无需人工操作。与 PowerBI 的接口使 Excel 成为一个更强大的可视化工具。在过去的几个月里，微软发布了 [LAMBDA](https://techcommunity.microsoft.com/t5/excel-blog/announcing-lambda-turn-excel-formulas-into-custom-functions/ba-p/1925546) ，它允许你使用 Excel 的公式语言定义自己的自定义函数。

![](img/02d0c8cc558e0a2acf81d3e22646cd9e.png)

热门|照片由[在](https://unsplash.com/@thehumantra?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 Humantra 拍摄

# 【Excel 有什么问题？

Excel 的灵活性成本很高。破坏一个公式真的很容易。在这里插入一行，在那里删除一个单元格，或者不小心在公式上键入数据，突然间，电子表格出现错误。更糟糕的是，电子表格在隐藏错误的情况下继续工作。互联网上流传的一些统计数据声称，80%-90%的电子表格都有错误。(我找不到可靠的消息来源。可能这些百分比是基于他们自己的 Excel 错误)。

与其他电子表格和文档链接的能力使 Excel 更加强大，但也使错误跟踪更加困难。具有多个函数或层的公式阅读起来很复杂，这增加了错误跟踪的麻烦。任何收到别人的电子表格并被告知修改#REF 的人！错误会确切地知道我的意思。

使用 Excel 可能需要大量的手动工作来清理数据和格式。这种努力通常是不可重用的。像 Power Query 和宏这样的工具会有所帮助，但是你需要成为一个更加老练的用户才能使用它们。

当处理大型数据集时，一些计算(如 SUMIFS 和 VLOOKUP)需要很长时间才能完成。你的电脑在计算的时候基本上是锁定的。您可以将计算从自动切换到手动，但这会产生一系列新问题。现在，您可能正在用陈旧的数据做决策。在计算过程中，您的计算机也有可能冻结或崩溃，所有工作都将丢失。

我们已经讨论了 Excel 的一些 IT 问题。数据和存储需求的过度共享。毫无疑问，如果我们真的知道关键基础设施在其处理过程中的某一点上对 Excel 的依赖程度，我们会感到震惊。

![](img/9416d044d10390fb5c1df497e1945918.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# **前途如何？**

最佳解决方案可能是混合模式。Python 是复杂数据分析的更好选择。但是，如果你的老板或股东是《华尔街日报》Excel 的死硬派，该怎么办？有越来越多的工具可以用来连接 Python 和 Excel。如果您需要比将最终数据框导出为 Excel 文件或 CSV 文件更高级的工具，那么这里有一些值得研究的工具。

这三个包可以读写 Excel 文件，执行操作，绘制图形。

*   [openpyxl](https://openpyxl.readthedocs.io/en/stable/)
*   [xlsxwriter](https://xlsxwriter.readthedocs.io/)
*   [pyexcel](http://docs.pyexcel.org/en/latest/)

接下来的两个包:xlwings 和 pyxll，允许您从 Excel 中调用 Python。Pyxll 使用 Excel 作为前端，在 Excel 中运行 Python 代码。Xlwings 使用 Excel 作为用户界面来运行 Python 脚本。请注意，pyxll 有每月订阅费用。Xlwings 基础款开源，pro 版有授权费。

*   [xlwings](https://docs.xlwings.org/en/stable/)
*   [pyxll](https://www.pyxll.com/)

我希望这些工具在功能和易用性方面都有所进步，从而更容易两全其美。