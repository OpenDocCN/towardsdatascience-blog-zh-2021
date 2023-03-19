# 数据科学面试的基本 Python 编码问题

> 原文：<https://towardsdatascience.com/essential-python-coding-questions-for-data-science-interviews-4d33d15008e6?source=collection_archive---------1----------------------->

## 破解数据科学面试

## Python 中的数据操作和字符串提取

![](img/239f547a3d914e164da71aa79b7d8565.png)

拉娅·索德伯格在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

随着工程文化的不断发展，数据科学家经常与其他工程师合作，构建管道并执行大量软工程工作。求职者预计会面临 R/Python 和 SQL ( [基本](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f)和[棘手](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77) SQL)方面的大量编码挑战。

从我过去的面试经历来看，仅仅会编码是远远不够的。有经验的程序员和经过代码训练营训练的初学者的区别在于，他们有能力将大问题分解成小问题，然后编写代码。

这是我的“啊哈”时刻。

在过去的几个月里，我一直在刻意练习剖析代码，并遍历我的思维过程，如果你跟踪我的进展，你可能会注意到这一点。今天的内容我也会这么做。

数据操作和字符串提取是数据科学访谈的重要组成部分，无论是作为一个单独的主题还是与其他主题相结合。我在之前的一篇文章中阐述了数据科学家的 6 个 Python 问题，并提供了主要科技公司提出的其他真实面试问题:

[](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) [## 2021 年 6 个 Python 数据科学面试问题

### 数据科学家/工程师的数据操作和字符串提取

towardsdatascience.com](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) 

在这个领域有一个普遍的误解，那就是使用野蛮的强制方法是不值得推荐的，因为它效率低并且占用内存。

在这里，我倾向于不同的观点。这对新的 Python 程序员非常有帮助，因为野蛮力量一步一步地解决了问题，并加强了我们的逻辑推理技能，也就是对问题的更好理解和更好的“想法实现”能力。

事不宜迟，我们开始吃吧

# 问题 1:反转字符串

> -编写一个反转字符串的函数。
> 
> - sentence = "我喜欢用 Python 编程。你呢？”

## 穿过我的思维

这是一个热身问题，但将其嵌入到一个更大的问题中后会变得复杂。无论如何，反转一个字符串最直接的方法是向后访问它。

## 解决方案 1:字符串[::-1]

```
'?uoy tuoba woH .nohtyP ni gnimmargorp evol I'
```

## 解决方案 2:列表理解

```
'?uoy tuoba woH .nohtyP ni gnimmargorp evol I'
```

如果问我另一种方法，我会推荐列表理解，因为它很简单。

# 问题 2:斐波那契数，FAANG

> 通常表示为 F(n)的斐波纳契数列形成了一个序列，称为斐波纳契数列，使得每个数字都是前面两个数字的和，从 0 和 1 开始。
> 
> 也就是说，
> 
> F(0) = 0，F(1) = 1，
> 
> F(n) = F(n -1) + F(n-2)，对于 n > 1。
> 
> -给定 n，计算 F(n)。
> 
> ——[https://leetcode.com/problems/fibonacci-number/](https://leetcode.com/problems/fibonacci-number/)

## 穿过我的思维

这就是“*如果你知道你知道*的问题类型。面试考生要么爱，要么恨，就看站在哪一边了。如果他们熟悉快捷方式，这几乎是一行代码。否则，它可能是不必要的繁琐，甚至无法解决。

首先，在提出更好的解决方案之前，我会使用暴力说出我的想法。作为一个经验法则，如果我们用简单的方法编码，列出一些斐波纳契数并尝试找出任何一致的模式是一个好主意。

*F(1) = 1。*

*F(2) = F(1) +F(0) = 1+0 = 1*

*F(3) =F(2) + F(1) = 1+1 = 2*

*F(4) = F(3) + F(2) = 2+1 = 3*

*F(5) = F(4) + F(3) = 3+ 2 = 5*

*…*

你发现什么有用的模式了吗？

事实证明，这是一个迭代的过程。在每一步，我们必须连续更新三个值，并为下一次迭代准备值。有关详细解释，请参见以下代码。

## 解决方案 1:暴力

```
2
```

正如所看到的，当一个简单的解决方案存在时，它变得不必要的乏味。很有可能，你的面试官会要求你改进它。

## 解决方案 2:递归

```
2
```

实际上，斐波那契数是采用递归的理想场景。我们指定当 n ≤1 时的基本条件，并将函数应用于自身两次。

简洁的解决方案！

你的面试官现在很开心。

# 问题 3:检查两个字符串数组是否等价，脸书

> -给定两个字符串数组 word1 和 word2，如果这两个数组表示相同的字符串，则返回 true，否则返回 false。
> -如果按顺序连接的数组元素形成字符串，则字符串由数组表示。
> -[https://leet code . com/problems/check-if-two-string-arrays-are-equivalent/](https://leetcode.com/problems/check-if-two-string-arrays-are-equivalent/)

## 穿过我的思维

问题问两个字符串数组是否相同。要相同，它们必须包含相同的元素和顺序。例如，以下两个数组由于元素顺序不同而不同:

> word1 = ["a "，" cb"]，word2 = ["ab "，" c"]

残酷的方法表明，我们可以创建两个新的字符串，并将每个数组中的元素分别迭代到空字符串中，并判断它们是否相同。

## 解决方案 1:暴力

```
True
```

这不是一个最佳的解决方案，因为它创建了两个新的对象，占用了额外的空间，并且 for 循环通常在速度方面效率不高。如果数据很大，解决方案 1 会导致延迟。

你的面试官不满意，要求改进。

好吧，让我们做解决方案 2。

## 解决方案 2: join and ==

```
True
```

join()方法是一个强大的工具。我们可以将两个数组中的元素连接到空字符串中，并检查它们是否相同，而无需创建任何额外的内容。

太棒了。你通过了面试。

![](img/814ea3d5f99b6087b7df7ea3f797623d.png)

在 [Unsplash](https://unsplash.com/t/business-work?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Nelly Antoniadou](https://unsplash.com/@nelly13?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 问题 4:矩阵对角和，Adobe

> -给定一个正方形矩阵 mat，返回矩阵对角线的和。
> -仅包括主对角线上的所有元素和次对角线上不属于主对角线的所有元素的总和。
> -[https://leetcode.com/problems/matrix-diagonal-sum/](https://leetcode.com/problems/matrix-diagonal-sum/)

## 穿过我的思维

TBH，我因为两个原因被困了足足 20 分钟。首先，我没有意识到，我不必扣除中心元素，但只有当矩阵长度是奇数。其次，我不确定如何在第二对角线上添加元素。在探索和检查了其他人的 Leetcode 解决方案之后，我想出了下面的代码。

## 解决方案 1

```
8
```

根据 Leetcode 的说法，运行时间和内存使用率都在 80%以上。问题 4 有没有更好的解决方法？如果你知道一个，请让我知道。

> (2021 年 1–14 日更新。多亏了 Nebadita Nayak，我们现在有了一个更好的解决方案。)

向[内巴蒂塔·纳亚克](https://medium.com/u/110079cc30ed?source=post_page-----4d33d15008e6--------------------------------)致谢:

## 解决方案 2

# 问题 Dream11 商店中特殊折扣的最终价格

> -给定数组价格，其中 prices[i]是商店中第 I 件商品的价格。
> -商店中的商品有特殊折扣，如果您购买第 I 件商品，那么您将获得相当于 prices[j]的折扣，其中 j 是最小指数，使得 j > i 和 prices[j] < = prices[i]，否则，您将不会获得任何折扣。
> -返回一个数组，其中第 I 个元素是考虑到特殊折扣，你将为商店的第 I 件商品支付的最终价格。
> -[https://leet code . com/problems/final-prices-with-a-special-discount-in-a-shop/](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/)

## 穿过我的思维

我在第一次审判中犯了一个错误。我认为 j 应该在 I 的下一个位置。j = i+1。事实证明，j 可以在任何地方，只要它比 I 大。我没有得到它，直到交叉检查我的提交与正确的输出。

同样，一旦我们找到一个元素，我们必须**中断**for 循环；否则，for 循环将遍历整个数组。

## 解决方案:两个 for 循环

嵌套的 for 循环占用大量时间，运行时间为 O(N)，可以使用另一种数据类型 stack ( [link](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/discuss/685390/JavaC%2B%2BPython-Stack-One-Pass) )来改进。我会再写一篇关于这个话题的文章。

# 问题 6:颠倒字符串中的单词 III，亚马逊

> -给定一个字符串，您需要颠倒句子中每个单词的字符顺序，同时仍然保留空白和初始单词顺序。
> -[https://leet code . com/problems/reverse-words-in-a-string-iii/](https://leetcode.com/problems/reverse-words-in-a-string-iii/)

## 穿过我的思维

这个问题有几种变体。在阅读以上信息的同时，我有了一些大致的方向。为了颠倒每个字符但保持原来的顺序，我可以使用空格作为分隔符来分割字符串。然后，通过单词[::-1]反转单词*，并在单词之间添加空格。包括 rstrip()方法来去除尾部的空白。*

然而，除了返回的结果包含空格(这会阻止我正确提交代码)之外，所有其他步骤都可以工作。

## 不正确的代码:

```
def reverse_words(s):
    result = “” 
    new = s.split(“ “)
    for word in new:
         word = word[::-1]+” “
         result += word
 **result.rstrip()          # don't put it here**    return result         **   # instead, put it here**
```

如果我将 rstrip()放在 return 命令之前，它将无法正常工作。它生成以下带有尾随空格的结果。

```
"s'teL ekat edoCteeL tsetnoc "    # trailing whitespace
```

除了最后一行，每一行代码都运行良好。真扫兴。在四处搜索之后，我发现 rstrip()必须在开头或结尾。如果在中间就不行了。

修复故障后，我们开始。

## 解决方案:for 循环

```
"s'teL ekat edoCteeL tsetnoc"
```

同样，rstrip()方法必须在末尾或开头！

*完整的 Python 代码在我的* [*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding) *上有。*

# 外卖食品

*   我喜欢外卖，因为这是我反思过去错误并从中吸取教训的部分，也是今天的第一课。
*   虽然不建议使用暴力，但它有助于我们更好地理解潜在的逻辑并记录我们的想法。
*   没有好的编码这种东西，因为总有更好的解决方案，这就是为什么我们的采访不断要求改进。尝试另一种数据类型/结构？

*Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership) 

# 我的数据科学面试序列

## 统计和编程

[](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [## FAANG 在 2021 年提出这 5 个 Python 问题

### 数据科学家和数据工程师的必读！

towardsdatascience.com](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) [## 2021 年 6 个 Python 数据科学面试问题

### 数据科学家/工程师的数据操作和字符串提取

towardsdatascience.com](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。