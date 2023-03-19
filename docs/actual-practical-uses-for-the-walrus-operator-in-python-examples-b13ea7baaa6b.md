# Python 中 walrus 运算符的实际应用(+示例)

> 原文：<https://towardsdatascience.com/actual-practical-uses-for-the-walrus-operator-in-python-examples-b13ea7baaa6b?source=collection_archive---------26----------------------->

## 提高性能还是仅仅是语法上的好处？

![](img/87c5f54118e6f6afe180f66be0c72c49.png)

海象仅仅是糖做的还是有更多的成分？(图片由[马里·梅德](https://www.pexels.com/@mali)在[像素](https://www.pexels.com/photo/strawberry-beside-spoon-of-sugar-141815/)上拍摄)

快速坦白:自从在 Python 3.8 中引入以来，我从来没有这么频繁地使用过 walrus 操作符。当它第一次被介绍时，我认为它是一种句法糖，并且找不到使用它的理由。在本文中，我们将了解它是否仅仅提高了我们代码的可读性，或者是否有更多的意义。我们将通过这个模糊操作符的一些实际用例，让您的生活更轻松。在本文结束时，您将:

*   理解海象操作员做什么
*   知道怎么用吗
*   识别使用 walrus 运算符的情况。

# 0.什么是海象运营商？

让我们从头开始。海象算子长这样`:=`。它允许你在同一个表达式中既赋值又返回一个变量。查看下面的代码块

```
beerPrice = 9.99
print(beerPrice)
```

在第 1 行，我们使用`=` 操作符将值 9.99 赋给一个名为‘beer price’的变量。然后，在第 2 行，我们打印变量。使用 walrus 运算符，我们可以一次完成这两项操作:

```
print(beerPrice := 9.99)
```

现在我们都打印出啤酒价格，我们已经设置为 9.99，此外，我们可以将变量用于其他用途。轻松点。现在让我们看看为什么我们需要这个功能，以及它对我们的代码意味着什么。

# 1.walrus 操作员的用例

在这一部分中，我们将应用运算符。我将尝试用一个尽可能类似真实应用程序的项目来说明这一点。在我看来，这比“foo”和“bar”的例子要好一些。

![](img/f807484c1e5cd5b5275af01fcc9e3b1d.png)

让我们让这家伙工作吧(图片由美国国家海洋和大气管理局发布)

我已经确定了一些情况，在这些情况下，walrus 操作符可能是实用的。首先简单介绍一下我们的项目，然后我们将讨论每种情况。

## 设置

我们是一家出于分析目的分析书籍的公司。我们已经建立了一个很好的 [**API**](/build-a-real-life-professional-api-with-an-orm-part-1-8fce4d480d59) ，允许用户向我们发送书籍。API 将图书文件保存到磁盘，之后我们可以分析它们的内容。

## 1.读取文件

我们的第一个目标是读取文本文件。请记住，我们不知道书籍的尺寸；人们可能会给我们发几十亿字节的百科全书！由于这个原因，我们想把这本书分成几部分来读。

```
""" The inferior non-walrussy way """openedBook = open('c:/bookDirectory/lotr_1.txt')
longWordCount:int = 0
while True:
    chunk = openedBook.read(8192)
    if (chunk == ''):
        break
    longWordCount += count_long_words(chunk=chunk)
print(longWordCount)
```

在这段代码中，我们以 8192 字节的块读取一本书。我们将它发送给一个计算超过 10 个字符的单词的函数。注意，我们必须检查块是否包含数据。如果我们不这样做，那么 while 循环就不会终止，我们就会被卡住。让我们看看如何使用 walrus 改进这段代码。

```
""" Walrus power! """openedBook = open(os.path.join(booksFolder, book))
longWordCount: int = 0
while chunk := openedBook.read(8192):
    longWordCount += count_long_words(chunk=chunk)
print(longWordCount)
```

你会注意到我们的代码现在干净多了。魔术在第五行。我们将打开的书的内容分配给块，并使用它来确定我们是否应该循环。好多了！

## 2.匹配正则表达式模式

假设我们的客户是一名图书管理员，他想给这本书增加一个难度等级。为了了解难度，我们将分析这篇课文。我们在阅读这本书时使用了以前用过的组块，现在我们将使用 RegEx(正则表达式)来查找一些困难的单词，我们将这些单词定义为包含 q 或 x 的单词。

```
# The Old way
openedBook = open(os.path.join(booksFolder, book))
while chunk := openedBook.read(8192):
    match = re.findall("\w*[xXqQ]\w*", chunk)
    if (match == []):
        continue
    print(len(list(set(match))))
```

在上面的代码中，我们找到了与第 4 行定义的特定正则表达式匹配的所有单词(`\w*[xXqQ]\w*`的意思是:找到包含 X、X、Q 或 Q 的所有单词)。这个函数默认返回一个空列表，所以我们必须检查我们是否在第 5 行和第 6 行找到了什么。最后，我们打印出唯一匹配数的计数。

```
# Walrus way
openedBook = open(os.path.join(booksFolder, book))
while chunk := openedBook.read(8192):
    if ((match := re.findall(pattern, chunk)) != []):
        print(len(list(set(match))))
```

当我们使用 walrus 操作符时，我们可以执行 regex findall 并将其结果赋给 match 变量，如果有匹配的话！尽管性能提高了一点点(3.1 毫秒)，但这样做最能提高可读性。

## 3.列表理解中的共享子表达式

下一步:我们要分析我们在上面的正则表达式部分中发现的所有难词。我们将首先对每个单词进行词干处理，检查词干是否超过 8 个字符，如果是这样:保留词干。顺便说一下，词干是动词的词根:playing→ play，plays→ play，am，are，is → be 等。在本文中，我们不会深入讨论这是如何工作的，只是假设我们有一个名为`stem_word()`的词干函数。我们可以通过三种方式做到这一点:

**常规方式** 这种方式使用一个标准进行循环。最大的缺点是由于操作的数量，这是非常慢的。关于可读性:一个简单的操作需要很多行。

```
my_matches = []
for w in matches:
    s = stem_word(w)
    if (len(s)) >= 10:
        my_matches.append(s)
print('l', my_matches)
```

**懒惰的方式** 让我们对这一点更深入一点，使用列表理解。优势在于我们将之前需要的 5 行代码减少到了一行。我们还大大提高了性能；这种方式比传统方式大约快 25%。缺点:我们必须调用两次昂贵的 stem_word()函数。这是不可接受的低效。

```
fmatches = [stem_word(w) for w in matches if len(stem_word(w)) >= 8]
```

有了我们钟爱的操作符，我们可以将前面两者结合起来:我们不仅将所有代码放在一行中，通过列表理解来增强可读性和性能，而且我们只需调用一次昂贵的 stem_word()函数。

```
smatches = [s for w in matches if len(s := stem_word(w)) >= 8]
```

## 4.重用一个计算成本很高的值

一旦一个单词被词干化，我们就想把它翻译成荷兰语、德语和西班牙语。假设我们有翻译单词的功能。这里的主要问题是，stem_word()函数非常昂贵，但是我们需要这个函数的结果来将其翻译成四种语言(英语和其他三种)。对于 walrus 操作符，它看起来像这样:

```
word = 'diving'
translations = [s := stem_word(word), translate_dutch(s), translate_german(s), translate_spanish(s)]
```

请注意，我们构建了一个只调用一次 stem_word()函数的列表，将它存储到变量`s`中，然后使用该变量调用翻译函数。我们最终得到了一个包含四种语言的翻译词干的数组！

# 5.从字典中检索

假设我们有一本关于当前用户的字典，如下所示。

```
userdata = {
    'firstname': 'mike',
    'lastname': 'huls',
    'beaverage': 'coffee'
}
```

我们想从字典中检索一个值，所以我们需要提供一个键。我们不确定哪些键存在，这取决于用户提供的数据。最安全、最快捷、最常规的方法是:

```
age = settings.get(key)
if (age):
    print(age)
```

我们使用字典对象上的 *get* 方法。如果键不存在，它要么返回键值，要么不返回。让我们看看如何应用 walrus 操作符。

```
if (age := settings.get(key)):
    print(age)
```

同样，我们可以将设置年龄变量与检查值是否存在的表达式结合起来。

# 把所有的都集合起来→什么时候去海象

看一下我们所有的例子，我们得出结论，它们可以总结为两种情况:

1.  如果存在，则分配一个值(1、2 和 5)
2.  在 iterable (3 和 4)中重复使用一个计算量很大的值

第一类在性能上稍有提高。多亏了 walrus 操作符，我们可以减少语句的数量，因为我们可以将变量赋值和检查变量是否存在结合起来。然而，主要的改进在于代码的可读性；减少线的数量。

第二个类提供了改进的可读性和性能。这样做的主要原因是我们使用了列表理解，并且 walrus 操作符只允许我们调用一次昂贵的函数。

![](img/da9ca727a023c5b1eeb3ee68327c023a.png)

这只北极熊现在知道如何控制他的海象了

# 结论

walrus 操作符在性能上有一点改进，但主要是为了编写更易读、更简洁的代码。有一些特定的情况是 walrus 可以解决的，比如在 list comprehensions 中调用一个昂贵的函数两次，但是这种情况可以通过实现一个传统的循环来轻松避免。尽管如此，当“*赋值 if exists”*(情况 1)时，我已经开始使用 walrus 操作符来增强我的代码的可读性。在我的日常工作中，详细描述重用昂贵的计算值的另一种情况相当少见；我通常会像之前描述的那样避免这些情况。

我希望我能对这个有争议的运营商有所帮助。请让我知道您是否有任何使用案例，在这些案例中，这个操作符通过留下评论而大放异彩。编码快乐！

迈克

又及:喜欢我正在做的事吗？[跟我来](https://mikehuls.medium.com/)！