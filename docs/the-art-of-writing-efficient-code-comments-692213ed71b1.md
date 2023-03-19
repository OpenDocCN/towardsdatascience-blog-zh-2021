# 编写高效代码注释的艺术

> 原文：<https://towardsdatascience.com/the-art-of-writing-efficient-code-comments-692213ed71b1?source=collection_archive---------10----------------------->

## 数据科学任务代码注释的最佳实践

![](img/b8769b95210ea402770748afb2f7587f.png)

[来自 Unsplash](https://unsplash.com/photos/PT_9ux0j-x4)

如果我们在 Jupyter 笔记本上运行`import this`，或者简单地打开[这个链接](https://www.python.org/dev/peps/pep-0020/)，我们将得到*Python 的禅*，这是 Tim Peters 收集的 19 条 Python 设计指南的集合。其中一条说:“可读性很重要”，在这篇文章中，我们将讨论这个原则的一个方面:代码注释，它是包含在代码中的自然语言文本片段，用于它的文档。事实上，代码注释，加上干净的代码，变量和函数的正确命名，以及使代码尽可能清晰，对我们项目的可读性有很大的贡献。这不仅对将来阅读我们作品的人很有帮助，而且对我们自己也很有帮助。

在本文中，我们将关注注释应用于数据科学任务的 Python 代码的最佳实践。然而，这些准则中的大部分也适用于任何其他编程语言或领域。

# 语法和样式

在 Python 中，有两种类型的代码注释:块注释和内联注释。

根据 [PEP 8](https://www.python.org/dev/peps/pep-0008/#comments) ，**块注释**以一个 hash ( `#`)开头，后跟一个空格，由一个或多个句子组成，第一个单词大写，每句话尾加一个句号。如果有几个句子，用两个空格隔开。块注释通过一个空行与上面的代码分开。它们适用于直接跟在它们后面的代码(没有空行)，并且具有相同的缩进。

一长行块注释应该分散在几行中，每一行都以一个散列开头，后跟一个空格。用 Python 编写多行注释的另一种方式是使用**多行字符串**——那些嵌入在三个单引号或双引号中的字符串。从技术上讲，它们最初是为另一个目的而设计的:将文档分配给函数、方法或类。然而，如果不在这种质量中使用或不赋给变量，多行字符串不会生成代码，因此它可以作为 Python 中代码注释的一种非常规方式。这种方法得到了 Python 创始人吉多·范·罗苏姆在他的推特上的认可。

```
✔️
# Isolate the outliers with at least $3,500 spent per month except 
# for the cases when the respondent hadn't attended any bootcamp or 
# had been learning programming for a maximum 3 months before the 
# survey.
above_3500 = usa[(usa['MoneyPerMonth']>=3500)\
                &(usa['AttendedBootcamp']!=0.0)\
                &(usa['MonthsProgramming']>3.0)]✔️
'''Isolate the outliers with at least $3,500 spent per month except for the cases when the respondent hadn't attended any bootcamp or had been learning programming for a maximum 3 months before the survey.
'''
above_3500 = usa[(usa['MoneyPerMonth']>=3500)\
                &(usa['AttendedBootcamp']!=0.0)\
                &(usa['MonthsProgramming']>3.0)]✔️
"""Isolate the outliers with at least $3,500 spent per month except for the cases when the respondent hadn't attended any bootcamp or had been learning programming for a maximum 3 months before the survey.
"""
above_3500 = usa[(usa['MoneyPerMonth']>=3500)\
                &(usa['AttendedBootcamp']!=0.0)\
                &(usa['MonthsProgramming']>3.0)]
```

一个**行内注释**被放置在它所注释的代码段的同一行上，用至少两个空格、一个散列和一个空格隔开。行内注释通常较短，应谨慎使用，因为它们往往会在视觉上与上下的代码行混在一起，因此变得不可读，如下面这段代码所示:

```
❌
ax.set_title('Book Rating Comparison', fontsize=20)
ax.set_xlabel(None)  # remove x label
ax.tick_params(axis='both', labelsize=15)
plt.show()
```

不过，有时行内注释可能是不可或缺的:

```
✔️
colors = [[213/255,94/255,0],         # vermillion
          [86/255,180/255,233/255],   # sky blue
          [230/255,159/255,0],        # orange
          [204/255,121/255,167/255]]  # reddish purple
```

请注意，在上面的代码中，行内注释的对齐有助于提高它们的可读性。

尽管代码注释语法的指导方针是为了使代码更具可读性和一致性，但我们应该记住，语言本身是不断发展的，样式约定也是如此。此外，一个公司(或一个特定的项目)可以有自己的编码风格方法，如果与 PEP 8 不同，那么优先考虑那些特定情况的建议。因此，有时官方的指导方针可以被忽略。但是，有些做法几乎总是应该避免的:

*   在注释前使用多个哈希
*   在注释的结尾也使用一个或多个散列
*   省略哈希和注释之间的空格
*   使用全部大写字母(我们稍后会看到一个例外)
*   在注释上方的一段代码后省略一个空行
*   在注释及其相关代码之间插入空行
*   忽略注释代码的缩进

这些方法是不可取的，因为它们用不必要的元素使代码变得混乱，降低了代码的可读性，并使区分不同的代码块变得困难。比较这些注释样式不同的相同代码:

```
❌
###REMOVE ALL THE ENTRIES RELATED TO 2021###questions = questions[questions['Date'].dt.year < 2021]
###CREATE A LIST OF TAGS###tags_list = list(tags.keys())✔️
# Remove all the entries related to 2021.
questions = questions[questions['Date'].dt.year < 2021]# Create a list of tags.
tags_list = list(tags.keys())
```

# 写有意义的评论

如果代码本身告诉计算机要做什么，那么代码注释是写给人类的，向他们解释这段代码到底是做什么的，特别是为什么我们需要它。关于“什么”,有一种普遍的观点认为，如果我们的代码尽可能显式和简单，如果我们对变量和函数使用自解释的名称，那么我们几乎根本不需要代码注释。尽管我同意编写易于理解的代码和仔细选择变量/函数名非常重要，但我认为代码注释也相当有用。

*   它们有助于将代码分成几个逻辑部分，使其更容易导航。
*   它们有助于解释很少使用的或新的方法/功能的功能。此外，如果我们必须应用不寻常/不明显的方法来编码，添加代码注释是一个好主意，因为当试图遵循更明确的方法时，可能会检测到一些 bugs 技术问题/版本冲突。
*   编写代码时，我们应该记住将来可能会阅读我们代码的人(例如我们的同事)。对一个人来说很清楚的事，对另一个人来说可能一点也不明显。因此，我们应该考虑到不同人在经验、技术和背景知识方面的潜在差距，并通过使用注释来促进他们的任务。
*   代码注释对于添加关于作者、日期和修改状态的信息很有价值(例如`# Updated by Elena Kosourova, 22.04.2021\. Added an annotation of the current year on the graph.`)。

因此，代码注释是提高代码可读性和可访问性的快速而强大的方法，对于编写清晰而有意义的注释来说，仅仅成为一个好的编码员是不够的，成为一个好的作者也很重要，这本身几乎是一项需要学习的技能。

让我们考虑一些我们应该遵循的建议，以便有效地应用代码注释并防止过度使用。

## 避免明显的代码注释

我们的评论不应该脱离上下文陈述明显或清楚的事情。让我们再次记住，“证据”可能因人而异，我们工作的范围和目标受众也很重要(例如，如果我们正在编写一个关于数据科学的真实数据项目或一本学生手册)。我们的任务是找到平衡。

```
❌
# Import libraries.
import pandas as pd
import numpy as np
```

## 避免低效的代码注释

为了最大限度地发挥作用，我们的代码注释应该尽可能地精确和简洁，给出关于代码的所有必要细节，并排除任何不相关的信息，如对以前代码的观察、对未来的打算或括号。此外，由于代码通常暗示了一些动态(它*做了*，*创建了*，*移除了*，等等)。)，一个好的做法是使用动词而不是名词:

```
❌ **Too vague**
# Select a specific type of columns.
characters = star_wars.iloc[:, 15:29]❌ **Too wordy**
# Now, let's select all the columns of a radio-button type from our dataframe.
characters = star_wars.iloc[:, 15:29]❌ **Observations from the previous code**
# Select radio-button type columns, as done earlier for checkbox type ones.
characters = star_wars.iloc[:, 15:29]❌ **Using a noun instead of a verb**
# Selection of radio-button type columns.
characters = star_wars.iloc[:, 15:29]✔️ **Good**
# Select radio-button type columns.
characters = star_wars.iloc[:, 15:29]
```

## 更新代码注释

这是一个棘手且非常常见的陷阱，但我们应该始终记住，当代码发生变化时，也要对代码注释进行所有必要的修改。正如 PEP 8 所说，“与代码相矛盾的注释比没有注释更糟糕”。更重要的是，有时我们不仅要检查与被修改的代码相关的代码注释，还要检查其他可能被这种修改间接影响的代码注释。

## 要使用的语言

一种常见的做法是始终用英语编写代码注释。然而，这是在某些情况下可以忽略的规则之一:如果我们绝对确定我们的代码永远不会被不讲我们的语言(或任何其他通用语言)的人阅读，那么我们最好完全用那种语言写注释。在这种情况下，对于我们特定的目标受众来说，代码变得更容易理解。

## 注释掉部分代码

有时，在一个项目中，我们发现为同一任务或调试代码测试几种方法是有用的，然后我们可以决定保留一些测试过的部分，即使我们在最后选择了另一个，以防万一。然而，这种方法只是暂时的，在项目的最终版本中，清除这样的碎片是很重要的。事实上，对于那些将来会阅读这些代码的人(包括我们自己)来说，这可能会让他们分心。读者可能会开始怀疑被注释掉的代码在任何一步是否有用，以及它是否应该保留在项目中。为了避免这种混乱，最好删除所有注释掉的代码。

像往常一样，这种最佳实践可能存在非常罕见的例外。比方说，我们的工作是展示获得相同结果的不同方法，一种方法比另一种方法更可取(仍然可行，但使用较少)。在这种情况下，我们可以关注 main 方法，并将第二个方法显示为注释掉的代码，并带有相应的注释。此外，这里我们可以使用前面提到的另一个例外，在代码注释中使用大写字母，以使其更加清晰可见:

```
✔️
# Create a stem plot.
plt.stem(months.index, months)# # ALTERNATIVE WAY TO CREATE A STEM PLOT.
# plt.vlines(x=months.index, ymin=0, ymax=months)
# plt.plot(months.index, months, 'o')
```

# 结论

总而言之，代码注释是一种方便的技术，可以向其他人传达我们用于编码的方法以及它们背后的原因。它提高了整体代码的可读性，并使在不同的逻辑块之间导航变得更加容易。为了在 Python 中创建有意义的代码注释，我们必须遵循简单明了的技术准则，牢记可能的本地公司或项目特定的约定，使我们的注释尽可能简洁和信息丰富，在代码修改时更新它们，最后但同样重要的是，避免过度使用它们。

感谢阅读！

如果你喜欢这篇文章，你也可以发现下面这些有趣的:

<https://python.plainenglish.io/the-little-prince-on-a-word-cloud-8c912b9e587e>  <https://medium.com/geekculture/creating-toyplots-in-python-49de0bb27ec1>  <https://medium.com/mlearning-ai/11-cool-names-in-data-science-2b64ceb3b882> 