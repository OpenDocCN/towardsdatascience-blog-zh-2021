# 提示和技巧:为 NLP 任务增加数据

> 原文：<https://towardsdatascience.com/tips-tricks-augmenting-data-for-nlp-tasks-983e33ad55a7?source=collection_archive---------22----------------------->

## [自然语言处理笔记](https://towardsdatascience.com/tagged/nlpnotes)

## 扩大 NLP 数据集的方法

![](img/6fc84b70e432dc358c6379970203cab1.png)

由[马克西姆·霍普曼](https://unsplash.com/@nampoh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

一般机器学习或自然语言处理管道的开始——在业务问题被确定之后很久——由数据获取阶段组成。在数据采集阶段，从业人员面临的挑战是识别测量现实世界物理条件的采样信号，以便将这些数据转换为计算机可以使用的数字数值。

如果数据随时可用，那么可以跳过这一步——现实世界中很少出现这种情况。如果我们要执行一个 NLP 项目，我们必须知道一套技术来改进我们的数据集，以满足我们的期望要求。技术的全部范围超出了本文的范围，但是感兴趣的读者可以钻研一下 [*永远记住数据先于科学*](/always-remember-data-comes-before-the-science-681389992082) 。

[](/always-remember-data-comes-before-the-science-681389992082) [## 永远记住数据先于科学

### 获取数据的不同方法

towardsdatascience.com](/always-remember-data-comes-before-the-science-681389992082) 

## 数据扩充简介

深度学习模型的预测精度与训练模型所需的数据量有关系。可以肯定地说，对于无监督的深度学习任务，预测精度主要受两个因素的影响；

*   可用于训练模型的数据量
*   训练数据的多样性

如果我们希望在解决复杂的 NLP 任务时达到高性能水平，深度学习通常是首选解决方案。然而，要学习一个可以准确地将输入映射到输出的函数，通常需要我们建立一个具有大量隐藏神经元的非常强大的网络。

[](/deep-learning-may-not-be-the-silver-bullet-for-all-nlp-tasks-just-yet-7e83405b8359) [## 深度学习可能还不是所有 NLP 任务的银弹

### 为什么你仍然应该学习启发式和基于规则的方法

towardsdatascience.com](/deep-learning-may-not-be-the-silver-bullet-for-all-nlp-tasks-just-yet-7e83405b8359) 

随着我们增加隐藏神经元的数量，我们也增加了可训练参数的数量，因此，如果我们希望我们的模型有效地学习，那么我们拥有大量数据是很重要的。

> **注意**:我们可以说模型中可学习参数的数量与训练模型所需的数据量成正比。

**如果我们没有足够的数据怎么办？**

问得好。数据扩充是我们可以用来增加数据集的众多解决方案之一。从本质上讲，数据扩充由各种技术组成，可用于增加我们数据集中的数据量，方法是添加我们已经拥有的数据的稍微修改的副本，或者通过从我们现有的数据创建新的合成数据-我们使用我们的数据来生成更多的数据。

这些小技巧听起来可能很疯狂，但它们在实践中往往非常有效。数据扩充技术的一些优点是:

*   实现速度比允许我们增加数据集的其他技术快得多
*   作为一种正则化形式，帮助我们在训练模型时减少过度拟合

对于希望探索更多实践方法的好奇读者，您可能希望深入了解以下资源；

[简单的数据扩充](https://arxiv.org/abs/1901.11196)

[NLP 蛋白沉淀](https://github.com/albumentations-team/albumentations)

[NLP 8 月](https://github.com/makcedward/nlpaug)

## 同义词替换

为了执行同义词替换，我们在句子中随机选择非停用词的单词，例如，所有以“s”开头的单词。然后，我们利用这些单词和工具，比如 Wordnet 中的 Sysnets，用它的同义词替换这个单词。

## 回译

想象我们有一个用英语写的句子。这个想法是把这个句子传递给一个翻译系统，这样这个系统就可以把它翻译成另一种语言，比如法语。通过我们的法语翻译，我们用它将句子翻译回英语——下面的 GIF 展示了一个使用谷歌翻译的很好的演示。

![](img/2d658e1209ff54e9f496e197e3cc3d3c.png)

作者 Gif

你可能会注意到，开头的句子写的是“*我没有时间给你*”，翻译成法语就是“*我没有时间为你准备*”。当我们把法语翻译成英语时，返回的英语句子写着“*我没有时间给你*”。虽然这些句子具有完全相同的意思，但所用单词的细微差异使我们能够以一种非常有创意的方式添加到我们的数据集。

## 二元翻转

要做二元模型翻转，我们首先要把句子分成二元模型。然后，我们随机选择一些二元模型，然后翻转它们——例如，如果一个二元模型是“*我有*”，我们将翻转它，使它变成“*我有*”。

## 替换实体

这个技巧类似于同义词替换，也很有趣；为了扩大我们的数据集，我们可以简单地用同一实体类别中的另一个实体替换现有的实体。例如，我们可以把一个地方的名字替换成另一个地方的名字——“我住在伦敦”将变成“我住在纽约”。

## 添加噪声

根据数据的来源，我们的数据集中可能会有一些拼写错误。例如，Twitter 上的人们在创建推文时倾向于使用更加非正式的语气。在这种情况下，我们可以向我们的数据中添加噪声，以便通过在句子中随机选择一个单词，并用另一个更接近实际单词的实际拼写的单词来替换它，来训练一个更健壮的模型。

## 包裹

数据扩充是一种快速有效的增加训练数据集大小的技术。在本文中，我们介绍了可以用于自然语言处理任务的各种类型的数据扩充技术。同样重要的是要注意，为了使这些技术有效地工作，我们需要有一个初始的干净数据集来开始，不管它有多大。

感谢您的阅读！

在 LinkedIn 和 T2 Twitter 上与我保持联系，了解我关于数据科学、人工智能和自由职业的最新消息。

## 相关文章

[](/deep-learning-may-not-be-the-silver-bullet-for-all-nlp-tasks-just-yet-7e83405b8359) [## 深度学习可能还不是所有 NLP 任务的银弹

### 为什么你仍然应该学习启发式和基于规则的方法

towardsdatascience.com](/deep-learning-may-not-be-the-silver-bullet-for-all-nlp-tasks-just-yet-7e83405b8359) [](/never-forget-these-8-nlp-terms-a9716b4cccda) [## 永远不要忘记这 8 个 NLP 术语

### 所有 NLP 爱好者都应该知道术语

towardsdatascience.com](/never-forget-these-8-nlp-terms-a9716b4cccda) [](/a-guide-to-encoding-text-in-python-ef783e50f09e) [## Python 文本编码指南

### 教计算机理解人类语言

towardsdatascience.com](/a-guide-to-encoding-text-in-python-ef783e50f09e) [](/a-guide-to-cleaning-text-in-python-943356ac86ca) [## Python 文本清理指南

### 为机器阅读准备自然语言

towardsdatascience.com](/a-guide-to-cleaning-text-in-python-943356ac86ca)