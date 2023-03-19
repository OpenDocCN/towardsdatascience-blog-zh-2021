# Fuzzywuzzy:合并不同转录名称的数据集

> 原文：<https://towardsdatascience.com/fuzzywuzzy-basica-and-merging-datasets-on-names-with-different-transcriptions-e2bb6e179fbf?source=collection_archive---------13----------------------->

## 使用词向量距离比较字符串相似度的模糊匹配

![](img/2dc61d22f00ceb573b636fdf3158dad3.png)

安娜斯塔西娅·切平斯卡在 Unsplash 上拍摄的照片

我最近做了一个项目，涉及到合并同一个人的姓名的数据集，这些姓名的转录略有不同。一些名字拼写错误，包括昵称、中间名或后缀，它们在数据集之间不匹配。在本例中，我使用了一个字符串匹配包 [Fuzzywuzzy](https://pypi.org/project/fuzzywuzzy/) 来合并正确的名称。

为了省去你很多我在合并前做的额外工作，我想解释一下你可以使用的 [**Fuzzywuzzy**](https://pypi.org/project/fuzzywuzzy/) 的模块、方法和用途。

# 模糊的距离计算

Fuzzywuzzy 使用 Levenshtein distance 来计算距离，简单来说，它决定了需要对字符串中的每个字符进行多少次更改才能找到另一个单词。

例如，“dog”和“dogs”需要一个变化:添加“s”。而“dog”和“dig”也需要一个变化:替换一个字母。而将“start”改为“tarp”将会有两处改动:删除“s”并用“t”代替“p”。

在实现 one names 的计算时需要注意的一点是，在词根发生变化的情况下，将名字与昵称进行比较时会有困难。例如,“Bobby”和“Robert”需要大量的更改，可能会导致其他不相关的名称出现为更好的匹配。

# **模糊不清的模块和方法**

Fuzzywuzzy 有两个模块:`process`和`fuzz`。

## 绒毛

返回字符串的相似率，在 0-100 之间，最基本、最精确的方法是`fuzz.ratio()`。

1.  `fuzz.partial_ratio()`将一个字符串与较长字符串中长度相等的子字符串进行比较。例如:`**fuzz.partial_ratio(‘Dogs’, ‘Dog’)**`会有一个 **100 的部分 _ 比率**，但只会有一个 **86** 与`**fuzz.ratio()**`。

2.`fuzz.token_sort_ratio()`不考虑顺序测量字符串，您可以从包含单词 **sort** 中猜到这一点。这在比较不同顺序的姓名时很有用，例如，比较**姓**、**名**与**名**、姓。

3.`token_set_ratio()`，你可以从`.token_sort_ratio`中猜到，把每一个令牌做一套来比较。重复的话不会影响比例。如果我们在歌名列表中查找歌曲，其中一首歌曲名为**谁放出了狗**，而我们想要匹配的歌曲名为**谁放出了狗，谁，谁，谁**，我们将获得 **100 的比率分数。**

## 过程

基于比率抓取最相似的单词。

1.  `process.extract()`和`process.extractOne()`将从字符串列表中取出最相似的(extractOne)或 n 个最相似的(extract(limit = n))单词与你给定的字符串进行比较。

# 与 Fuzzywuzzy 的过程融合

在我的合并过程中的这一点上，我已经将我的一个数据集中的名称重新排序为与我的第二个数据集相同的顺序，所以我在我的过程中没有使用`fuzz`。现在我们知道，在未来的尝试中，我们不必这样做。

首先，安装您正在使用的软件包和模块。

```
*# pip install fuzzywuzzy* from fuzzywuzzy import process
```

为了合并名称，我在要合并的数据帧中创建了一组名称，称为`names`，并在第二个数据帧中创建了一组要比较的名称:`member_names`。然后我做了一个字典，里面有与`names`最接近的两个匹配项，这样我就可以手动检查分数，看名字匹配是否正确。

```
# *make a dictionary of closest matches to names* keys = {}**for** name **in** names: *#names in smaller dataset to compare and match**#get closest match of `name` compared to larger data `member_names`*keys[name] = ((process.extract(name, member_names, limit=2)))#*you can limit to 1 with extractOne to take less time but I wanted to check if names were returning as correct.*
```

下面是`keys`返回的前 5 行。

```
Robert J. Wittman [('J Cox', 86), ('Rob Wittman', 86)]
James R. Langevin [('James Risch', 86), ('James Comer', 86)]
Gerald E. Connolly [('Gerry Connolly', 75), ('J Cox', 54)]
Kathy Manning [('Kay Granger', 58), ('Kathy Castor', 56)]
Michael K. Simpson [('Mike Simpson', 86), ('Michael Waltz', 66)]
```

正如您所看到的，最匹配的名字并不完美，所以从这一点上，我过滤并检查了不匹配的名字。我查了比率低于或等于 86%的名字。

```
*#checking names that might not match correctly***for** name **in** keys:
    **if** keys[name][0][1] <= 86: * #this gives the second value in the first tuple, which is the score of the first name*

    print(name, keys[name])
```

此时，您可以将您的限制扩展到 2 个以上，因为我的一些匹配项的比率较低。找到正确的参数后，您可以为没有匹配项的名称创建一个列表，在不同的索引处有匹配项，并迭代字典值以创建一个合并关键字过滤列表中的名称，如果需要的话:

```
no_match = ['Michael Enzi', 'Kathy Manning', 'John Hickenlooper', 'William Hagerty', 'cott Franklin', 'Victoria Spartz',
           'Marjorie Taylor Greene', 'Marie Newman', 'eter Meijer', 'David Cheston Rouzer', 'Robert "Bobby" Scott']

second_match = ['Mitchell Mcconnell, Jr.', 'Harold Dallas Rogers', 'Neal Patrick Dunn MD, FACS']
```

下面，我提供了匹配代码的简化版本来生成 merge_keys:

```
# *getting list of the matches from the bigger dataset to the smaller one as keys to merge on*merge_key = [keys[name][1][0] **if**
                      name **in** second_match \
                      **else** **None** **if** name **in** no_match **else**
                      keys[name][0][0] **for** name **in \** df['name']]df[‘merge_key’] = merge_keydf.merge(second_df, how = ‘left’, left_on = ‘merge_key’, right_on=’member_names’)
```

现在你知道了！如果愿意，您可以覆盖任一数据集中的原始列，但是我希望保留原始数据集中的原始名称，以及我希望用于合并第二个数据集中的名称。

感谢阅读，我希望它是有帮助的！

你可以阅读更多关于 fuzzywuzzy 的内容，或者在这里下载。