# 4 种替换 Python 列表中项目的方法

> 原文：<https://towardsdatascience.com/4-ways-to-replace-items-in-python-lists-68b5ec04f79a?source=collection_archive---------2----------------------->

## 了解如何通过求解基本算法来替换 Python 列表中的条目，为下一次编码面试做好准备

[![](img/6db218db68d5da5d2b27967ba9e14dd7.png)](https://anbento4.medium.com/)

马丁·沃特曼在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片。

*更新:你们很多人联系我要有价值的资源* ***钉 Python 编码访谈*** *。下面我分享 4 门我强烈推荐在练习完本帖的算法后坚持锻炼的课程:*

*   [**Python 中的 leet code:50 种算法编码面试题**](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fleetcode-in-python-50-algorithms-coding-interview-questions%2F) *→最适合编码回合备考！*
*   [***Python 高级编码问题(StrataScratch)***](https://platform.stratascratch.com/coding?via=antonello)***→****我找到准备 Python 的最佳平台& SQL 编码面试到此为止！比 LeetCode 更好更便宜！*
*   [**用 Python 练习编码面试问题(60+题)**](https://datacamp.pxf.io/DV3mMd)
*   [**Python 数据工程纳米学位**](https://imp.i115008.net/jWWEGv) **→** *优质课程如果你有更多的时间投入。***2022 年 3 月 UDACITY 球场最高 75**%**的折扣* *

希望你也会发现它们有用！现在欣赏:D 的文章

[](/3-nanodegrees-you-should-consider-to-advance-your-data-engineering-career-in-2021-baf597debc72) [## 3 门数据工程课程，在 2021 年推进您的职业发展

### 加入数据行业，改变角色或通过注册数据工程简单地学习前沿技术…

towardsdatascience.com](/3-nanodegrees-you-should-consider-to-advance-your-data-engineering-career-in-2021-baf597debc72) 

# 介绍

在准备下一轮 Python 编码时，您可能已经注意到需要操作一个或多个列表的算法出现得相当频繁。迟早，你也会在面试中遇到他们中的一员。

> 需要操作一个或多个列表的算法出现得相当频繁。迟早，你也会在面试中遇到他们中的一员。

为了帮助你在掌握这种数据结构的过程中提高你的编码技能，下面我介绍 **4** **方法来替换 Python 列表**中的一项，以及四个简单的算法供你测试你的技能。

大多数经典的面试问题都可以用多种方法解决，所以对于每个问题，在看我提供的解决方案之前，先试着想出你的解决方案。与其他技能类似，算法面试是一个你可以通过不断练习来大大提高的领域。

# 1.索引

替换列表中项目的最直接方法是使用索引，因为它允许您选择列表中的一个项目或项目范围，然后使用赋值运算符更改特定位置的值:

例如，假设您正在处理包含六个整数的以下列表:

```
lst = [10, 7, 12, 56, 3, 14]
```

你被要求在左起第三个整数上加 10。因为列表是从零开始索引的，所以可以使用下面的语法之一来改变带有`index = 2`的元素，在我们的例子中是数字`12`:

```
#Option 1
lst[2]= lst[2] + 10#Option 2
lst[2]+= 10 #no need to repeat "lst[2]" then more elegant#Option 3
lst[2] = 22 #that is 12 + 10
```

现在试着用第一种方法解决下面的问题:

## 问题#1

*给定一个包含整数(介于 1 和 9 之间)的非空列表，将其视为表示一个非负的唯一整数，并递增 1。返回更新的列表。*

*在结果中，必须存储数字，使得通过求和获得的数字的第一个数字位于列表的开头，并且列表中的每个元素包含一个数字。你可以假设整数不包含任何前导零，除了数字 0 本身。*

注意，我们的输入列表由四个非负整数组成，它们共同表示整数 9999。这是一种边缘情况，因为通过在这个数字上加 1，您将获得一个 5 位数的整数(*而不是原来的 4* )和一个前导 1。

为了解决这种情况，解决方案利用了两个条件语句，从最后一个到第一个开始计算数字(*使用* `reverse()` *翻转索引的顺序*)。如果不等于 9，则通过现在熟悉的语法`digits += 1`仅最后一位数字增加 1，并且立即返回修改后的列表，而不评估剩余的索引。

或者，如果最后一个数字是 9，则用 0 代替，并计算后面的(*倒数第二个*)数字。如果不等于 9，则加 1 并返回修改后的列表。否则，如果后面的每个数字都是 9，那么该函数将返回一个以 1 开头的列表和与原始列表中的元素数量一样多的 0。

[](https://anbento4.medium.com/10-mcqs-to-practice-before-your-databricks-apache-spark-3-0-developer-exam-bd886060b9ab) [## 参加 Databricks Apache Spark 3.0 开发人员考试前的 10 个 MCQ 练习

### 你付出了努力。你投入了金钱和时间。现在，考试就要开始了，你只有一次机会…准备好了吗？

anbento4.medium.com](https://anbento4.medium.com/10-mcqs-to-practice-before-your-databricks-apache-spark-3-0-developer-exam-bd886060b9ab) 

# 2.For 循环

在上面的问题中，目标只是替换列表中的最后一个整数，将它递增 1，而迭代只是用来覆盖边缘情况。
然而，*如果我们想同时替换列表中的多个元素呢？*

在这种情况下，使用 for 循环会很好，因为它可以用来迭代列表中的项目。为了展示它是如何工作的，让我们回到原始列表，将所有的整数乘以 2:

```
lst = [10, 7, 12, 56, 3, 14]for i in range(len(lst)):
    lst[i] = lst[i] * 2print(lst)Output: [20, 14, 24, 112, 6, 28]
```

上面的例子很简单，但是如果要求您在替换列表中的项目时应用稍微复杂一点的逻辑，该怎么办呢？

## 问题#2

考虑一个整数列表。如果是奇数，就加 1，如果是偶数，就加 2。

注意，这个解决方案不是使用`range(len(nums))`，而是使用`enumerate()`方法迭代列表中的所有元素，通过[模操作符](https://www.geeksforgeeks.org/what-is-a-modulo-operator-in-python/)检查整数是奇数还是偶数，并通过分别添加 1 或 2 来替换它们。

Enumerate 是 Python 中的内置函数，可用于在遍历 iterable 对象时添加自动计数器。当可选的`start`参数没有指定时，计数器从 0 开始计数，就像它是一个实际的索引一样。

出于这个原因，通常使用`enumerate()`来解决需要您根据应用于索引或列表值的条件来操作列表的算法。

# 3.列出理解

list comprehension 语法是一种更优雅的方法，可以遍历列表元素并应用某种形式的操作。这是因为理解允许你创建一个新的内联列表，使得你的代码看起来非常简洁。

您可以将上面示例中的 for 循环转换为如下理解:

```
lst = [10, 7, 12, 56, 3, 14]lst = [lst[i] * 2 for i in range(len(lst))]print(lst)Output: [20, 14, 24, 112, 6, 28]
```

为了填充新列表，还允许您指定基本条件作为列表理解语法的一部分。这正是解决以下算法需要做的事情:

## 问题#3

*给定一个按升序排序的整数列表，返回一个也按升序排序的列表，包括:
-整数的平方，如果能被 3 整除
-原始整数，如果不能被 3 整除*

在这种情况下，if 条件在`for loop`之前指定，因为存在一个`else`语句。然而，当不需要`else`时，您可以简单地遵循语法:

```
output = [ expression for element in list_1 if condition ]
```

# 4.切片和洗牌

有时，在编写面试代码时，您可能会被要求重新排列列表中的项目，以便它们以不同的顺序出现。这可以通过*切片*和*洗牌*来实现。

例如，如果您希望交换初始列表中的前 3 个和后 3 个元素，您可以写:

```
lst = [10, 7, 12, 56, 3, 14]lst = lst[3:] + lst[:3]print(lst)Output: [56, 3, 14, 10, 7,
 12]
```

也许，在这种情况下，谈论替换有点牵强，因为您实际上只是改变了元素在列表中的位置。尽管如此，这种方法还是很有效的，可以帮助你解决下面的问题。

## 问题#4

*给出由 2n 个元素组成的 num 列表，形式为[x1，x2，…，xn，y1，y2，…，yn]。以[x1，y1，x2，y2，…，xn，yn]的形式返回数组。*

该练习为您提供了用于切片的索引(在本例中为`n=3`)，并要求您通过创建新的整数对来打乱输入列表。

属于每一对的整数应该是共享相同索引的整数，如果我们将原始列表分割成两个子列表(`nums[:n]`和`nums[n:]`)，使用`zip()`将子列表中包含的元素配对在一起并递归地将它们添加到新的`shuffled`列表中，可以很容易地获得这个结果。

# 结论

在本文中，我向您介绍了 4 种方法来替换 Python 列表中的条目，它们是*索引*、用于循环的、*列表理解*和*切片洗牌*。

每个操作本身都很简单*，但是你应该能够通过选择最合适和有效的方法来掌握它们，同时解决更复杂的挑战，为你的下一轮编码做准备。*

*出于这个原因，我还展示并分享了 4 种算法的解决方案(每种方法一个)，它们代表了您在面试初级和中级数据角色时会发现的复杂程度。*

*还要注意的是*这篇文章中出现的问题是对 Leetcode 上出现的问题的重新解释。每一个问题都有多种解决方案，因此地雷只是象征性的。**

## *给我的读者一个提示*

*这篇文章包括附属链接，如果你购买的话，我可以免费给你一点佣金。*

## ***来源***

*[停止在 Python 中使用 range()for Loops | Jonathan Hsu |更好的编程](https://betterprogramming.pub/stop-using-range-in-your-python-for-loops-53c04593f936)*

*[如何同时迭代两个(或更多)列表| Jonathan Hsu |更好的编程](https://betterprogramming.pub/how-to-iterate-over-two-or-more-lists-at-the-same-time-5f70b5c822ad)*

*【在 Python 中替换列表中的项目:完整指南:完整指南(careerkarma.com)*