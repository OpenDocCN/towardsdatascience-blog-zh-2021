# 如何解决数据科学中的任何问题

> 原文：<https://towardsdatascience.com/how-to-solve-any-problem-in-data-science-469a67b7ab15?source=collection_archive---------42----------------------->

## 解决数据科学中复杂问题的简单方法。

![](img/598f85f661d06d9d14ad09bc5e2cbbba.png)

解决复杂问题的简单方法。

# TLDR:

## 理解问题

1.  写下来
2.  写下你所知道的
3.  写下你的假设/条件

## 规划您的解决方案

1.  把你知道的和你不知道的联系起来
2.  写下计划

## 实施您的解决方案

## 评估您的解决方案

# 介绍

数据科学家的工作是解决问题。大多数教程强调特定的技能、工具或算法。相反，本教程将概述一个解决问题的方法，你可以适用于几乎任何情况。

乔治·波利亚的《T2 如何解决:数学方法的一个新方面》一书概述了解决数学问题的四步方法论。

这种方法，加上一些小的改动，非常适合数据科学家。

这四个步骤是:

1.  理解问题
2.  设计一个计划
3.  实施解决方案
4.  评估解决方案

为了实际演示这种方法，我们将进行一个相当简单的编码练习。

# 理解问题

理解问题是解决问题最容易被忽视的方面。

你能做的最重要的事情就是把问题写下来。

一旦你把它写下来，确定它是真正的问题。我在 IT 部门工作时学到的一件事是，我被要求解决的问题很少是真正需要解决的问题。

一旦你确定了正确的问题，写下你所知道的与问题相关的一切。

执行此操作时要包括的项目:

*   你以前解决过的类似问题
*   限制
*   要求
*   你知道的任何与寻找解决方案有关的事情

最后，写下与解决方案相关的所有条件。其中一些是组织性的，另一些是技术性的。

例如，如果您将使用您公司的 Amazon EC2 实例来运行您的 python 解决方案，并且该 Python 环境使用 Pandas 版本. 23，那么您就不应该在您的解决方案中使用 Pandas 1.x 的特性。

# 潘格拉姆问题:

这个问题摘自[exercism . io](http://exercism.io)的 Python 赛道

> 确定一个句子是否是一个字谜。A pangram(希腊语:παν γράμμα，pan gramma，“每个字母”)是一个使用字母表中每个字母至少一次的句子。最著名的英语 pangram 是:
> 敏捷的棕色狐狸跳过懒惰的狗。
> 使用的字母表由 ASCII 字母 `*a*` *到* `*z*` *组成，不区分大小写。输入将不包含非 ASCII 符号。*
> 
> *exercism . io*

# 问题是

一个给定的字符串输入是英语语言的字谜吗？

# 我们知道些什么？

1.  Pangram:至少包含字母表中每个字母中的一个的字符串
2.  字母表中有 26 个字母
3.  字母是 ABCDEFGHIJKLMNOPQRSTUVWXYZ
4.  如果一个字母不在字符串中，它就不可能是一个盘符
5.  字母从少到多的共性是:QJZXVKWYFBGHMPDUCLSNTOIRAE 来源:[https://www3 . nd . edu/~ busi forc/讲义/cryptography/letter frequency . html](https://www3.nd.edu/~busiforc/handouts/cryptography/letterfrequencies.html)

## 我们解的条件是什么？

1.  我们不关心非阿拉伯文字
2.  我们不在乎一个字母是否大写
3.  我们不在乎信件是否有重复
4.  我们不在乎字母的顺序

# 规划解决方案

一旦我们理解了我们的问题，我们就可以计划我们的解决方案。这就是我们把我们所知道的东西拿出来，并勾画出我们如何用它来解决问题的地方。

在此阶段我们应该问的问题:

*   怎样才能把我们知道的和我们不知道的联系起来？
*   为了实现这一目标，我们还需要解决其他问题吗？
*   我们过去是否解决过类似的问题，并且可以采用我们已经知道的解决方案？
*   如果我们不能解决现存的问题，我们能不能先解决一个更简单的版本？

## 潘格拉姆计划:

为了解决 pangrams 问题，我们可以开始使用我们所知道的东西来游戏如何确定一个输入是否是 pangrams

*   如果输入的长度小于 26，则它不能是一个 pangram
*   如果一个字母不在输入中，它就不是一个盘符
*   如果我们从最不常见的字母到最常见的字母来遍历字母表，那么计算效率会更高，因为失败更有可能发生在早期迭代中
*   如果我们遍历字母表，我们最多有 26 个循环要经历

# 实施解决方案

这是您实际构建解决方案的地方。

这是我们许多人做得相当好的部分；如此之好，以至于这通常是我们试图解决问题时的第一步(可能就在我们疯狂地查看 google 或 stack overflow 之后)。

成功实施的关键是有条不紊地遵循计划。如果计划不起作用，回到计划阶段看看你是否错过了什么。

## pangrams 实现:

这是我为执行我制定的计划而编写的 Python 函数:

```
# Execute the plan:

letter_list = ['Q', 'J', 'Z', 'X', 'V', 'K', 'W', 'Y', 'F', 'B', 'G', 'H', 'M',
               'P', 'D', 'U', 'C', 'L', 'S', 'N', 'T', 'O', 'I', 'R', 'A', 'E']

def is_pangram(input_string):
    pangram = True
    if len(input_string) < 26:
        pangram = False

    else:
        input_string = input_string.upper()
        for letter in letter_list:
            if letter not in input_string:
                pangram = False
                break
    return pangram# This will evaluate to false, it is missing a 'Z'
is_pangram("The quick brown fox jumps over the lay dog")# This will evaluate to True
is_pangram("The quick brown fox jumps over the lazy dog")
```

# 评估解决方案:

这最后一步很容易跳过，但如果我们想持续改进，这一步至关重要。我们需要评估我们答案的质量。

我们可以问的问题有:

*   这是最好的解决方案吗？
*   有更好的方法来解决这个问题吗？
*   就内存和速度而言，我们解决方案中最昂贵的部分是什么？
*   我的代码可读吗？
*   评论够不够？
*   三个月后这个坏了，我会知道它会做什么吗？

## Pangrams 评估:

虽然我对这个解决方案相当满意，但我们还可以做一些改进。

我们可以使用 Python set()函数删除重复的字符，而不是评估整个输入，这将使我们的“如果不在”评估更容易。

我们也可以完全不同地处理这个问题，而不是分配一个字典，将一个字母与其 ASCII 码配对，然后使用一个索引函数来查找它。

我特别自豪的是，我的想法是按照共性的逆序迭代字母表。在研究这篇文章的时候，我查看了几个代码实践网站，没有看到其他人实现了这个想法。

# 结论

直接进入一个问题并试图立即解决它是非常容易的。这对于简单而熟悉的问题非常有效。然而，在复杂的非正统问题上，采用这样一种严格的方法将有助于你从同行中脱颖而出。

当我的团队打算招聘员工时，我们对他们解决问题的过程比对他们给出的具体答案更感兴趣。

最后，我强烈推荐你阅读波利亚的书

# 关于作者:

Charles Mendelson 是 PitchBook 的营销数据分析师。他还在攻读哈佛扩展学院的心理学硕士学位。如果你正在为你的数据导向播客或 YouTube 频道寻找一位客人，与他联系的最佳方式是在 [LinkedIn](https://www.linkedin.com/in/charles-mendelson-carobert/) 上。

*原载于 2021 年 2 月 11 日 https://charlesmendelson.com*[](https://charlesmendelson.com/tds/how-to-solve-any-problem-in-data-science/)**。**