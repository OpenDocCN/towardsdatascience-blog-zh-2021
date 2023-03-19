# Python 中的主数据结构字典:从零到 Hero，第 1 部分

> 原文：<https://towardsdatascience.com/master-python-dictionary-for-beginners-in-2021-1cdbaa17ec45?source=collection_archive---------21----------------------->

## 破解数据科学访谈

## LeetCode 助你迈向数据科学的位置

![](img/8149b7902c0ec0d13a73116eb72b4359.png)

[彼世恒](https://unsplash.com/@pisitheng?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/dictionary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

正如你们中的许多人所知道的，我正在将我的编程语言从 R 转换为 Python，并且在过去的一年中一直在练习用 Python 进行编码。我不是在吹牛，但我总是惊讶于我的进步和周转快的家伙谁只知道“你好，世界！”对于那些在 Leetcode 上轻松解决复杂的编码挑战的人来说，这是非常重要的。

例如，我在这里对五个 data science 采访问题进行了实时编码:

</5-python-coding-questions-asked-at-faang-59e6cf5ba2a0>  

Python 提供了如此多的数据类型，而字典是其中的佼佼者。掌握 Python 字典有助于我们破解任何编码采访。在今天的内容中，我选择了四个访谈问题，并演示了如何在 Python 中使用字典。

让我们简单回顾一下，看看什么是词典及其主要属性。词典有三大特点:无序、多变、不重复。

> #1 字典没有排序，因此我们不能使用位置来访问元素；相反，我们必须使用密钥-值对。
> 
> #2 它是可变的，这让我们可以更新字典元素。
> 
> #3 不允许重复。

这里有一个简单的例子。

我们创建了一个新的字典，称为 *dictionary_1，*，它有三个密钥-值对。

字典 _1

```
{'brand': 'BMW', 'model': 'Model 3', 'year': 2000}
```

***#1。获取字典*** 的密钥

键

```
dict_keys(['brand', 'model', 'year'])
```

***#2。取值字典***

价值观念

```
dict_values(['BMW', 'Model 3', 2000])
```

***#3。获取密钥-值对***

关键值对

```
dict_items([('brand', 'BMW'), ('model', 'Model 3'), ('year', 2000)])
```

# 问题 1:每家科技公司的两个 Sum

> -给定一个整数 nums 数组和一个整数目标，返回这两个数字的索引，以便它们加起来达到目标值。
> -可以假设每个输入只有一个解决方案，并且不可以将同一元素使用两次。
> -您可以按任何顺序返回答案。
> -[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)

## 走一走我的思路

这是每个大公司都问过或曾经问过的“臭名昭著”而且可能是最广为人知的面试问题。新手和成熟的程序员会以不同的方式处理这个问题。

对于初学者来说，首先想到的是迭代列表，并不断检查两个元素的总和是否等于目标值，也就是残酷的力量。

如果它们相加，我们分别输出索引。唯一的问题是我们不能两次使用同一个元素，并且只有一个解决方案。

## 解决方案 1:嵌套 for 循环(强制)

```
[0, 1]
```

当然，这是可行的，但速度很慢。它的时间复杂度为 O(N ),如果目标数组有数百万个元素需要检查，那么它就不是一个好的选择。

你的面试官要求改进。

## 解决方案 2:字典

与初学者不同，有经验的程序员会分析手头的问题，并将其转化为一个容易理解的问题。成熟的编码人员努力思考如何更有效地访问和存储每个位置的值。

第一个提示:字典是有帮助的！

要创建和使用一个空字典，我们必须设置条件，并指定当值不在字典中和在字典中时该怎么办。按照同样的思路，我们创建一个新变量， *diff* ，它记录目标值和数组元素之间的差异。如果差异没有包含在字典中，我们将键值添加回字典中；如果包含在字典中，我们返回原始元素的索引和字典的键(index)。

让我们检查代码。

```
[0, 1]
```

哇！

第二种方法效率更高，速度更快，线性时间复杂度为 O(N)。

如果你能掌握解决方案 1，你就已经进了门，成为了 Python 初学者。掌握了解决方案 2 之后，你就更上一层楼了。

# 问题 2:谷歌的唯一出现次数

> -给定一个整数 arr 数组，写一个函数，当且仅当数组中每个值的出现次数是唯一的时，返回 true。【https://leetcode.com/problems/unique-number-of-occurrences/】-

## 走过我的思考

谷歌收录了这个问题。任何技术编码的第一步都是阅读问题，并确保你理解了它。如果有些事情没有意义，要求澄清，这增加了分数，也有利于我们。

看完分析问题，有两个部分:

> #1.计算每个值的出现次数。
> 
> #2.检查它们是否是唯一的。

为了计算出现次数(步骤 1)，我们创建一个空字典，并使用 if-else 语句来存储键值对。

为了检查唯一值(步骤 2)，我们可以使用另一个内置的数据类型 set，它不允许重复。顺便提一下，我们通常在迭代后比较两个输出的长度，并决定它们在 Python 中是否相同:如果长度改变了，结果也改变了。

## 解决办法

```
True
```

同样，Python 中的 set 不允许重复。字典和集合的结合是强大的。

![](img/6fb3a5608bf35dbca2d8c72b443df2c8.png)

Annelies Geneyn 在 [Unsplash](https://unsplash.com/s/photos/book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 问题 3:大小为 2N 的数组中有 N 个重复的元素，由苹果公司开发

> -在大小为 2N 的数组 A 中，有 N+1 个唯一元素，并且这些元素中恰好有一个元素重复 N 次。
> -返回重复 N 次的元素。
> -[https://leet code . com/problems/n-repeated-element-in-size-2n-array/](https://leetcode.com/problems/n-repeated-element-in-size-2n-array/)

## 走过我的思考

苹果问这个问题。有两条有用的信息:

> #1.N+1 个唯一元素。
> 
> #2.一个元素重复 N 次。

最自然的做法是计算出现次数，并返回出现 N 次的元素。

## 解决方案 1:内置计数器()

```
3
```

首先，让我们试试内置方法 Counter()，返回出现 1 次以上的元素。

这很容易也很简单，但问题是我们可能不会在面试中导入内置函数。

## 解决方案 2: dictionary.items()

我们构建一个空字典，并通过 for 循环的存储键值对*。接下来，我们遍历字典，当它的值(出现次数)等于 n 时返回键。*

搞定了。

顺便提一下，Python 中的 division 创建了一个 float，不能用来访问项目。这就是为什么我们需要像第 5 行一样使用 floor division， **len(A)//2** 来计算 n。

```
type(22/2)
floattype(22//2)
int
```

# 问题 4:谷歌的单排键盘

> -有一个特殊的键盘，所有的键都在一排。
> -给定一个长度为 26 的字符串键盘，指示键盘的布局(索引从 0 到 25)，最初，您的手指在索引 0 处。要输入一个字符，你必须将手指移到所需字符的索引处。
> -将手指从食指 I 移动到食指 j 所需的时间是|i — j|。
> -您想要键入一个字符串单词。写一个函数来计算用一个手指打字需要多少时间。
> -[https://leetcode.com/problems/single-row-keyboard/](https://leetcode.com/problems/single-row-keyboard/)

## 走过我的思考

谷歌问这个天才问题。很多时候，不是问题有多复杂，而是问题有多棘手。挑战在于分解问题，并将其转化为更易读的形式。

单行键的特殊键盘？

从来没见过。但是经过一些思考，我们可以把这个问题转换成如下:“请计算每个元素的索引的总距离。”

我们可以进一步将其分为两步:

> #1.找到键值对。
> 
> #2.计算差异的总和。

我们将键值对映射到空字典(步骤#1 ),并使用 for 循环将距离相加，如下所示。

## 解决办法

```
4
```

在第 14 行，我们用当前索引更新临时值，并为下一次迭代做准备。

*完整的 Python 代码在我的* [*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding) *上有。*

# 外卖食品

*   如果问题暗示了键值对，字典可能会有所帮助。
*   棘手的部分是如何在字典中存储值:是否必须为多次出现而更新它？怎么会？
*   dictionary 中有三种过滤元素的方式:键()、值()和项()。

*Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

<https://leihua-ye.medium.com/membership>  

# 我的数据科学面试顺序:

</crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa>  </crack-data-science-interviews-essential-statistics-concepts-d4491d85219e>  </essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f>  

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。