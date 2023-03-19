# 查找缺少的索引:就地

> 原文：<https://towardsdatascience.com/find-missing-index-in-place-f9153c849c43?source=collection_archive---------10----------------------->

## 如何在不使用额外空间的情况下找到数组中第一个缺失的整数

![](img/0a06eb4f30822d9d178a9674c478cac4.png)

作者图片

数据操作算法设计中一个有趣的技术是就地数据操作。这种技术对于节省空间非常有用，在某些情况下这可能是必要的(但是一般不推荐，因为粗心的应用可能导致数据损坏)。

> 问题是

是这样的:我有一个整数列表，有些是正数，有些是负数。我想找到最小的缺失索引。

这意味着给定一个包含 3 个数字的列表:[-1，1，0]，它们的索引是[1，2，3]。哪个是最小的缺失指数？那就是两个。

再比如:[4，5，6]。它们的指数是[1，2，3]，现在最小的缺失指数是 1。

> 解决方案 1:设置

最简单的解决方案是使用一个集合结构来存储找到的整数，然后遍历每个索引来找到第一个丢失的整数:

```
def get_missing_min0001(inputs):

    input_set = set()
    input_max = None
    for input in inputs:
        input_set.add(input)
        input_max = max(input, input_max) \
            if input_max is not None \
            else input

    i = 1
    while i in input_set and i < input_max:
        i = i + 1

    return i if i != input_max else None inputs = [1, 3, 2, 6]
expected = 4
assert get_missing_min0001(inputs) == expected
```

*   *时间复杂度*

因为我们遍历数组中的每个元素一次，所以时间复杂度是 O(N ),其中 N 是输入元素的数量。集合结构和字典一样，具有 O(1)的查找和插入复杂度。

*   *空间复杂度*

我们需要将元素存储在一个集合中，这需要 O(N)空间。

> 解决方案 2:就地

时间复杂度看起来是最优的，空间复杂度也不错，还有什么可以优化的呢？在现实生活的应用程序中，这可能是你能做的最好的事情(也可能是应该做的)。但是实际上有一种方法可以进一步优化这个解决方案的空间复杂度。

注意输入本身是一个数据结构(确切地说，是一个大小为 N 的列表)？如果我们可以利用它来替代我们用来存储 seen 索引的集合，那么我们就不需要分配额外的空间。

一个简单的方法是，对于我们在列表中遇到的每个整数，我们将该索引处数字的符号变为负数，例如:

我们从以下内容开始:

[1, 3, 2, 6]

我们遇到的第一个数字是 1，因此我们将索引 1 处的数字变为负数:

[-1, 3, 2, 6]

第二个数字是 3，因此我们将索引 3 处的数字变为负数:

[-1, 3, -2, 6]

第三个数字是 2(取绝对值):

[-1, -3, -2, 6]

第四个数字是 6，它超出了数组的长度，所以什么也不做。

最后我们得到[-1，-3，-2，6]，再看一遍这些数字，我们发现最小的正数索引是索引 4，这就是我们的答案。

输入数据包含负数怎么办？如果有零呢？对于负数或零，我们可以用 1 代替它们，因为第一个索引是 1。当然，我们需要做一个检查，以确保列表中存在 1。如果不是，那么答案就是 1。

解决方案如下:

```
def get_missing_min0002(nums):

    has_one = False
    for i in range(len(nums)):
        # check that 1 exists
        if nums[i] == 1:
            has_one = True

        # convert < 1 to 1
        if nums[i] < 1 or nums[i] > len(nums):
            nums[i] = 1

    if not has_one:
        return 1

    for i in range(len(nums)):
        nums[abs(nums[i]) - 1] = - abs(nums[abs(nums[i]) - 1])

    for i in range(len(nums)):
        if nums[i] > 0:
            return i + 1

    return len(nums) + 1
```

*   *时间复杂度*

我们对每个数字循环两次，因此运行时间复杂度是 O(N)。

*   *空间复杂度*

输入数据用于存储索引信息，没有额外分配内存，所以空间复杂度为 O(1)。

> 解决方案 3:就地递归

用负数做集合指标是不是有点 hacky？是的，是的，我同意。

理想情况下，我们更喜欢一个明确的指标，不依赖于特殊的含义，如“无”。但是有一个实际的困难，如果我们把一个索引上的数字设置为 0，那么我们就丢失了那个索引上的信息。使用前面的例子:

我们从以下内容开始:

[1, 3, 2, 6]

我们遇到的第一个数字是 1，因此我们将索引 1 设置为“无”:

[无，3，2，6]

我们遇到的第二个数字是 3，因此我们将索引 3 设置为“无”:

[无，3，无，6]

但是等等！

既然我们已经将索引 3 设置为 None，我们就不再知道索引 3 的数字是多少了。

在这种情况下，我们需要的是递归。

当我们在索引 2 处遇到 3 时，我们检查并意识到在索引 2 处找到的数字不是 2，所以我们首先将数字保存在索引 3 处(将 2 保存到一个变量)，将索引 3 设置为“无”，并递归地遍历索引 2，最终我们在索引 2 处找到 3，由于索引 3 已经是“无”，我们只需要将索引 2 设置为“无”，我们就都设置好了:

[无，无，无，6]

我们遇到的最后一个数字是 6，这又一次超出了索引，所以我们什么也不做，最后的结果是缺少的索引是 4。

```
def get_missing_min0003(nums):

    for i in range(len(nums)):
        nums[i] = nums[i] - 1

    # recursive reduction function
    def _reduce_inputs(inputs, i):

        if i is None:
            return
        if i < 0:
            return
        if i >= len(inputs):
            return

        r = inputs[i]
        inputs[i] = None

        if r is None or r == i:
            return

        _reduce_inputs(inputs, r)

    for i in range(len(nums)):
        _reduce_inputs(nums, nums[i])

    for idx, i in enumerate(nums):
        if i is not None:
            return idx + 1

    return len(nums) + 1
```

*   *时间复杂度*

O(N)

*   *空间复杂度*

O(1)

就这样，问题优雅地解决了。(我也想使用队列，但这会增加内存使用……)

> 结论

尽管就地内存操作很酷、很有趣并且节省空间，但它不是最健壮的编程技术。如果函数的用户希望输入保持不变，以便用于其他目的，该怎么办？修改输入的 API 不是一个好主意。

权力越大，责任越大。

(递归解决方案有一个问题，如果你想通了，请告诉我！)