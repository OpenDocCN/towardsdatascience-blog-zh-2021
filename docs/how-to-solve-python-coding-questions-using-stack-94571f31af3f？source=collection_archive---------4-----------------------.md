# 如何使用堆栈解决 Python 编码问题

> 原文：<https://towardsdatascience.com/how-to-solve-python-coding-questions-using-stack-94571f31af3f?source=collection_archive---------4----------------------->

## 破解数据科学面试

## 2021 年数据科学家和软件工程师的基本数据类型

![](img/f4f89228a2eee8815bae9d1ce5a2248d.png)

萨姆拉特·卡德卡在 [Unsplash](https://unsplash.com/s/photos/a-stack-of-plates?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Python 是一种通用的基于脚本的编程语言，在人工智能、机器学习、深度学习和软件工程中有着广泛的应用。它的流行得益于 Python 存储的各种数据类型。

[如果我们必须存储键和值对，字典](/master-python-dictionary-for-beginners-in-2021-1cdbaa17ec45)是自然的选择，就像今天的问题 5。[字符串](/python-string-manipulation-for-data-scientists-in-2021-c5b9526347f4)和[列表](/essential-python-coding-questions-for-data-science-interviews-4d33d15008e6)是一对孪生姐妹，她们走到一起解决字符串操作问题。[集合](/master-python-dictionary-for-beginners-in-2021-1cdbaa17ec45)具有独特的位置，因为它不允许重复，这是一个独特的功能，允许我们识别重复和非重复项目。嗯，tuple 是技术面试中最少被问到的数据类型。上周，一位资深数据科学家在 Twitter 上发布了一个编码问题，可以通过 tuple 轻松解决。然而，很多候选人都被绊倒了。我很快会就这个话题再写一篇文章。

数据类型在 Python 编程中起着重要的作用！能够区分彼此对于解决面试问题非常重要。在今天的文章中，让我们转到另一个基本的数据类型，叫做堆栈。

在撰写这篇博文时，我花了无数个小时来选择以下问题，并以一种新程序员容易理解的方式对它们进行排序。每个问题都建立在前一个问题的基础上。如果可能的话，请按顺序解决问题。

# 叠起来

堆栈是一种线性数据结构，以**后进/先出(LIFO)或先入/后出(FILO)的方式(**[GeeksForGeeks](https://www.geeksforgeeks.org/stack-in-python/)**)**保存元素。这意味着最后添加的元素将首先从堆栈中弹出。我们可以想象一堆盘子:我们总是使用最上面的那个盘子，这个盘子是最后加到盘子堆里的。Push 和 pop 是栈中最基本的操作，为其他更高级的采用奠定了基础。

![](img/4e0abe462ead8510ec9d6fb92bce3c1a.png)

瑞安·斯通在 [Unsplash](https://unsplash.com/s/photos/a-stack-of-plates?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在 Python 中，有三种实现堆栈的方法:使用列表、collections.deque 和链表。为了准备数据科学面试，第一种方法已经足够好了。如果你对另外两个感兴趣，请查看这个帖子([链接](https://www.geeksforgeeks.org/stack-in-python/))。

在 Python 中，我们可以简单地用一个列表来表示一个栈。因此，它具有与列表相同的属性。它有两个基本操作:push()和 pop()。为了将一个元素推入或者添加到堆栈中，我们使用 append()方法。要弹出或删除最后一个元素，我们使用 pop()方法，如下所示。

```
stack = []          # create an empty stack
stack.append(‘a’)   # push 'a' to the stack
stack.append(‘b’)   # push 'b' to the stack
stack.append(‘c’)   # push 'c' to the stackprint(stack)
['a', 'b', 'c']stack.pop()         #pop the last element
'c'stack.pop()        #pop the last element
'b'stack.pop()        #pop the last element
'a'stack              #check the remaining stack
[]
```

# 问题 1:删除所有相邻的重复字符串，由脸书，亚马逊，彭博，甲骨文

> -给定小写字母的字符串 S，重复删除包括选择两个相邻且相等的字母，并删除它们。
> -我们在 S 上重复删除重复，直到我们不能再这样做。
> -在所有此类重复删除完成后，返回最终字符串。保证答案是唯一的。
> -[https://leet code . com/problems/remove-all-adjacent-duplicates-in-string/](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## 走过我的思考

**脸书、亚马逊、彭博和甲骨文都包含了这个问题。**在看问题提示的时候，我发现以下几个关键词值得注意:*去重，两个相邻相等，去重*。**简单来说，这个问题翻译过来就是去掉相邻的等号字母。翻译过程对于简化问题至关重要，因为很多时候，提示包含太多琐碎的信息，分散了我们解决问题的注意力。此外，确定关键点并大声说出来，让面试官参与进来。**

要检查两个字母是否相等，我首先想到的是采用野蛮的力量，在两个连续的位置检查相同的元素。但是，这种方法不起作用，因为移除过程是动态的。

假设测试用例是' ***阿巴卡*** '如果使用野蛮的力量，我们将只能删除' ***bb*** '字符串而不是删除' ***aa*** '后创建的' ***bb*** . '

我们需要一个动态的解决方案来检查新创建的连续位置是否有相同的元素。

还好宇宙提供了 stack！

我们可以创建一个空栈(列表)来存储新元素。如果元素等于堆栈中的最后一个元素，我们就把它从堆栈中弹出来，不把新元素添加到列表中。换句话说，简单的 pop 操作删除了两个相邻的相同字母。相当整洁！最后，我们将列表重新加入到一个空字符串中，这是一种常见的转换策略。

## 解决办法

```
'ca'
```

作为热身，我故意选择这个简单的问题。下面的问题看起来有点挑战性，但是按照同样的过程完全可以解决。

# #问题 2:让字符串变得伟大，谷歌

> -给定一串 s 个小写和大写英文字母。
> -好的字符串是没有两个相邻字符 s[i]和 s[i + 1]的字符串，其中:0<= I<= s . length-2
> -s[I]是小写字母，s[i + 1]是大写字母，反之亦然。
> -为了使字符串变好，你可以选择两个使字符串变坏的相邻字符，并删除它们。你可以一直这样做，直到弦变好。
> -修复后返回字符串。在给定的约束条件下，答案保证是唯一的。注意空字符串也是好的。【https://leetcode.com/problems/make-the-string-great/】-

## 走过我的思考

**谷歌问了这个问题**。让我们再次扫描提示并检索关键信息。下面是吸引眼球的关键词:*两个相邻的字符，一个小写，一个大写(反之亦然)，去掉两个相邻的不同大小写的字符*。**简单来说，问题是想让我们去掉两个相邻的不同大小写的字符(一个上一个下，不分位置)。**

记住说出你的想法，让你的面试官知道你已经抓住了关键信息。如果你没有，你的面试官会问你为什么决定采用 stack 而不是其他数据类型。

按照与问题 1 相同的逻辑，我们创建一个空堆栈来存储元素，并检查要添加的字母是否相同，但大小写不同。在 Python 中，有不同的检查案例的方法。如果你和我一样，不知道任何内置的方法/函数，我们可以按照以下步骤:首先确定这两个字母不相同( **stack[-1]！= i** )其次，在使用较低的方法(**堆栈[-1])后，它们变得相同。lower() ==i.lower()** )。

这是一个难题。我们不能从空堆栈中弹出一个元素。它会产生索引错误。像下面这样:

```
IndexError
```

为了避免错误消息，我们必须使用第 4 行中的堆栈 if 来确保堆栈不为空。这是 Python 程序员在使用堆栈时最常犯的错误之一。

## 解决方案 1

```
'leetcode'
```

## 解决方案 2

事实证明，Python 有一个内置的方法 swapcase()，它将字母从大写转换成小写，*反之亦然*。此外，你可能想和面试官确认一下你是否可以使用任何内置的方法。继续沟通！

```
'leetcode'
```

# #问题 3:使用堆栈操作构建数组，Google

> -给定一个数组目标和一个整数 n .在每次迭代中，你将从 list = {1，2，3…，n}中读取一个数。
> -使用以下操作构建目标数组:
> -Push:从开始列表中读取一个新元素，并将其推入数组中。
> — Pop:删除数组的最后一个元素。
> —如果目标数组已经建立，停止读取更多的元素。
> -返回构建目标数组的操作。向您保证答案是唯一的。
> -[https://leet code . com/problems/build-an-array-with-stack-operations/](https://leetcode.com/problems/build-an-array-with-stack-operations/)

## 走过我的思考

**谷歌问了这个问题。**这个问题好人性化，明确告诉你推送和弹出元素。一叠就好了！

创建一个空堆栈并遍历列表:将元素推送到新堆栈，弹出最后一个元素。最后，检查构造的数组是否与目标数组相同。如果是，停止迭代。

唯一的问题是，如果我们已经获得了目标数组，就要中断 for 循环。为此，保持 **stack==target** (第 16 行& 17)的缩进块与 if-else 语句一致。面试官可能会问一些后续问题，比如为什么你把缩进放在 for 循环内部而不是外部。

## 解决办法

```
['Push', 'Push', 'Pop', 'Push']
```

# #问题 4:棒球比赛，亚马逊出品

> -你在用奇怪的规则为一场棒球比赛记分。游戏由几轮组成，过去几轮的分数可能会影响未来几轮的分数。游戏开始时，你从一个空记录开始。您将得到一个字符串列表 ops，其中 ops[i]是您必须应用于记录的第 I 个操作，并且是下列操作之一:
> -整数 x-记录 x 的新得分。
> -“+”-记录前两个得分之和的新得分。保证总会有两个先前的分数。
> —“D”——记录一个新的分数，该分数是之前分数的两倍。保证总会有以前的分数。
> ——“C”——使之前的分数无效，将其从记录中删除。可以保证**总会有一个先前的分数**。
> -返回记录上所有分数的总和。
> -[https://leetcode.com/problems/baseball-game/](https://leetcode.com/problems/baseball-game/)

## 走过我的思考

**亚马逊挑了这个问题**。这是另一个对受访者友好的问题，因为它告诉你推动和弹出元素。此外，我们需要使用包含多个 if 语句的控制流。构建堆栈和添加/移除元素的过程与上述问题相同。在这里，我重点介绍一下这个问题需要特别注意的独特属性。

通常，在弹出元素之前，我们必须检查堆栈是否为空，**如问题 2** 所示。但是，我们不必检查这个问题，因为提示告诉我们**总会有一个以前的分数**。如果没有额外的条件，这个问题会变得有点混乱。最后，我们使用 sum()函数返回所有分数的总和。

问题解决了！

## 解决办法

```
30
```

# #问题 5:亚马逊和彭博的《下一个更伟大的元素 I》

> -给定两个整数数组 nums1 和 nums2，它们都是唯一的元素，其中 nums1 是 nums2 的子集。
> -在 nums2 的相应位置找到 nums1 元素的所有下一个更大的数字。
> -nums 1 中数字 x 的下一个更大的数字是 nums2 中其右侧的第一个更大的数字。如果不存在，则返回-1。
> -[https://leetcode.com/problems/next-greater-element-i/](https://leetcode.com/problems/next-greater-element-i/)

## 走过我的思考

**亚马逊和 Blooomberg 挑了这个问题。**问题 5 需要不止两步的解决方案。但首先，吸引眼球的关键词包括: *nums1 是 nums2 的子集，在 nums2 中为 nums1 寻找下一个更大的数字，为 none 返回-1*。

在我看来，翻译过程是这样的:

```
#1 subset → elements in nums1 should also be in nums2\. #2 find the next greater numbers → positions matter and value matters! # 3 return -1 if such value does not exist → Use if-else statement as a control flow.
```

总之，我们必须找到一种方法来识别元素的位置和值，并使用 if-else 语句来比较值。

要找到元素的位置和值，自然的选择是使用 enumerate()函数。此外，我们希望使用字典来保存键值对的记录。

接下来，我们遍历数组 nums2，找到下一个更大的条目。如果有这样一项，我们打破 for 循环；否则，返回-1。完整的代码在解决方案 1 中。

## 解决方案 1

这种方法很直观，也很容易遵循。

```
 [-1, 3, -1]
```

## 解决方案 2

*声明:解决方案 2 不是我的原代码，特别鸣谢去*[*aditya baurai*](https://leetcode.com/fireheart7/)*的* [*帖子*](https://leetcode.com/problems/next-greater-element-i/discuss/626227/Python3-oror-Stack) *。*

事实证明，我们可以采用堆栈和字典。首先，我们创建一个空堆栈/字典，并迭代 nums2:

```
#1 if the stack is not empty and the last stack element is smaller than num: we pop the stack and create a key-value pair in the dictionary.#2 for other cases, we append the element to the stack.
```

为了澄清，for 循环迭代 nums2 中的所有元素，而 while 循环是为了找到 nums2 中与堆栈相比下一个更大的元素。为了更好地帮助我们理解，我们可以循环 nums2 的前两个元素。

```
# pseudo code
# nums2: [1,3,4,2]# step 1: # when the first element num == 1
for num in nums2: 
    stack.append(num) # since the stack is empty# step 2: for the second element num ==3
for num in nums2: # since the stack isn't empty
    while stack and stack[-1] <num:
        anum = stack.pop() # pop out the last element 1 # create a key-value pair: 1 is the key and 3 is the value
        and[anum] = num
```

简而言之，上面的代码找到了 nums2 中所有较大的元素，最后我们找到了 nums1 中元素的相应值。

```
[-1, 3, -1]
```

*我的*[*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding)*上有完整的 Python 代码。*

# 外卖食品

*   堆栈有两个操作:push()和 pop()。
*   确定关键词，与面试官交流。
*   Stack 经常使用以下关键字进行组合:两个连续的相同字母/数字/单词，如果某个元素与之前的位置有某种关系，则推送并弹出该元素。
*   弹出前检查是否为空堆栈。
*   缩进对于循环来说至关重要。很好的理解问题，决定它的位置(循环内还是循环外)。
*   复杂的编码问题可以分解成多个更小的部分。

*Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership) 

# 我的数据科学面试序列

[](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [## FAANG 在 2021 年提出这 5 个 Python 问题

### 数据科学家和数据工程师的必读！

towardsdatascience.com](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [## 2021 年数据科学家必备的 SQL 技能

### 数据科学家/工程师的重要 SQL 技能

towardsdatascience.com](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [](/crack-data-science-interviews-essential-statistics-concepts-d4491d85219e) [## 破解数据科学访谈:基本统计概念

### 赢在 2021 年:数据科学家/工程师的必读之作，第 2 部分

towardsdatascience.com](/crack-data-science-interviews-essential-statistics-concepts-d4491d85219e) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。