# 挖掘推文回复:一个演练

> 原文：<https://towardsdatascience.com/mining-replies-to-tweets-a-walkthrough-9a936602c4d6?source=collection_archive---------11----------------------->

## 一段简短的历史，讲述了我是如何在 Twitter 上挤来挤去，围着我，抢着回复一条推文

![](img/aa2bb0233d4bd10a1ac7f8c8896ef2c2.png)

我在 twitter 上的“新闻”快照

Twitter 的 API 是我们所有人都喜爱的，不是吗？它让我们几乎实时地接触到信息宝库、错误信息、虚假信息等等。它满足了初学者 NLP 工程师/数据科学家高效收集可处理数据的梦想。可笑的是，足够多的社区成员通过他们的博客帖子、堆栈溢出和包装器对此进行了很好的记录。根据我的记忆，我记得至少有五种不同的 python 包装器来访问 API。

尽管如此，我偶然发现了一个文档很差、解决方案实现更差的任务(这不是程序员的错):挖掘一条推文的回复。

```
*For the rest of this post, I am going to assume that the reader is familiar with twitter API terminology such as a tweet_id or a tweet object. If this is new, please do refer to the* [*twitter's documentation*](https://developer.twitter.com/en/docs)*.*From experience, tutorials that use wrappers such as [tweepy](https://docs.tweepy.org/en/latest/) make it more intuitive to get started- for advanced usage however, the source documentation might help.
```

这是这个[问题](https://hackernoon.com/scraping-tweet-replies-with-python-and-tweepy-twitter-api-a-step-by-step-guide-z11x3yr8)的解决方案。[这个](https://github.com/gmellini/twitter-scraper)也是。精湛的实现。它们在大多数情况下都有效。不幸的是，他们受到了 Twitter 本身的限制。如果您不想深入这些链接中的代码，下面是它们所遵循的过程的快速概述:

```
**Mining replies to tweets**1\. Find the *tweet_id* of the tweet you want the replies to.
2\. Query twitter for tweets that have replied to this tweet id. This can be done based on the *in_reply_to_user_id* attribute that every tweet object carries. Every tweet is represented as a tweet object. 
3\. Compile the replies, assert their correctness. Done.
```

然而，缺点是当您检索这些结果时，Twitter 的旧 API 只返回最近的或流行的结果——或者两者的组合(您可以指定哪一个)。这些新近或流行并不是由每条推文来判定的——而是由发布你想要回复的根推文的用户来判定的。

当你与我的账户互动时，你可以找到我收到的几乎每一个回复。如果你与《纽约时报》的账户互动，你甚至无法对他们几个小时前发布的内容进行回复，因为他们收到了大量“最近”和“热门”的推文。由于只有一定数量的回复被返回，最近的回复和流行的回复保持快速变化，使得不可能为高活动帐户重新创建回复活动。

## 不是所有的都失去了。其实现在更好！

幸运的是，我意识到 Twitter 在去年年底发布了它的 v2 API。“这只是更新-没有太花哨的我猜”(除了这是我最错误的̶ ̶i̶n̶ ̶l̶i̶f̶e̶当谈到 API)。

这里有一个[不错的小帖子](https://developer.twitter.com/en/docs/twitter-api/early-access)解释了新的东西。

我找不到很多使用 v2 端点的 python 包装器(我不喜欢我之前尝试在没有包装器的情况下与 API 交互的经历)，但是纯属巧合和最后一次绝望尝试的勇气——我找到了救星包装器: [TwitterAPI](https://github.com/geduldig/TwitterAPI) 。

在新 API 中所有有趣的特性中，对这项任务最重要的是属性 *conversation_id* ，它现在伴随着每条推文。在错综复杂的推文中，有回复、对回复的回复、对回复的回复也是回复等等，每条推文都被分配了相同的 *conversation_id* ，以尝试将所有内容串连在一起。

TwitterAPI 的示例代码(我在这篇文章中大量借鉴并做了一点修改)就依赖于这个属性。让我们逐步编码。您可以复制并粘贴代码，它将按照描述的方式工作。记住从您自己的 twitter 开发人员门户设置身份验证令牌。

注意:gists 并不意味着独立运行。py 文件。它们只是用来表示的。您可以从这里按顺序复制代码，然后重新创建文件(或者以笔记本的形式使用它[这里](https://colab.research.google.com/drive/1sTFtEUm3BmWOA3ltQm4V9TBVlkPY5pHY?usp=sharing))。

## 1.导入魔法。

```
**# INSTALL TWITTER API (!pip install TwitterAPI) and import necessary libraries/packages
# TwitterAPI Documentation here:** [**https://github.com/geduldig/TwitterAPI/**](https://github.com/geduldig/TwitterAPI/)from TwitterAPI import TwitterAPI, TwitterOAuth, TwitterRequestError, TwitterConnectionError, TwitterPager
import pandas as pd
```

## 2.验证你的应用。点击获取您的代币[。记得在实例化 API 时传递 *version=2* 参数。](https://developer.twitter.com/en/apply-for-access)

```
consumer_key= "CONSUMER KEY"
consumer_secret= "CONSUMER SECRET"
access_token_key= "ACCESS TOKEN KEY"
access_token_secret= "ACCESS TOKEN SECRET"api = TwitterAPI(consumer_key, consumer_secret, access_token_key, access_token_secret, api_version='2')
```

## **3。算出推文的“conversation_id”。以下是我所做的，我发现这是最方便的。**

*   从发布了您想要回复的推文的帐户中查找最近的推文。使用 *tweepy* 来收集 tweet 对象——他们使用 v1 API，但是检索 tweet 对象，因此检索*tweet _ ids*——在我看来，他们的方法是最简单、最快的方法(但是你也可以找到其他方法——我们只需要最少的*tweet _ ids*—*conversation _ ids*

*   使用 TwitterAPI 使用其 *tweet_id* 查找 tweet，并检索其 *conversation_id* 。

## 4.如上所述检索推文。有了代码，您也应该有他们的“conversation _ ids ”,所有这些都整齐地打包到一个数据帧中。

```
# RETRIEVE TWEETS FROM THE SCREEN NAMES(S)tweets = get_new_tweets(names)# RETRIEVE CONVERSATION IDs OF THE RETRIEVED TWEETS
tweets = add_data(tweets)# WRITE THE RETRIEVED TWEETS TO CSV
tweets.to_csv("tweets.csv")# VIEW THE DATAFRAME HEAD WITH THE TWEETS RETRIEVED
tweets.head()
```

## 5.转到 tweets.csv 文件，找到您需要回复的 tweet 的“conversation_id”。复制它。马上回来。

```
conv_ids = ['1349843764408414213'] 
# replace this with conversation IDs from above- this is just a place holder
```

## 6.构建一个数据结构来处理 tweet 层次结构。

我不打算以一种详细的方式从技术上深入这一步。这里所发生的是创建一个树形数据结构来跟踪 tweet，其中*根*是启动线程的第一条 tweet。

(**可选逻辑:**每次回复 root 都是一级回复。对 1 级回复的每个回复都是 2 级回复，依此类推。有趣的是，整个线程中的每条 tweet 都带有 *conversation_id* 属性。使用这个并使用 *in_reply_to_user* 属性，您可以重新创建线程中每个回复的级别。使用这些信息和每条推文的时间戳，你几乎可以重建整个线程。)

## 7.实际检索回复。

我们简单地使用上面创建的树结构来存储每个回复，并通过给每个回复分配一个级别来找到它在线程层次结构中的位置。

我对这篇文章做了一点简化，添加了一些代码，只检索对一条推文的即时回复，而不检索对回复的后续回复。如果您想看到整个对话树被重新创建，请取消注释上面要点的第 27 行、下面要点的第 37 行和第 38 行，以及下面要点的第 36 行，以观看奇迹的发生。

**可选逻辑:**一旦你获得了根和具有相同 *conversation_id* 的 tweet 列表，你需要做的就是使用这个并使用 *in_reply_to_user_id* 找到每条 tweet 的父节点，直到你通过蛮力找到根。然而，递归实现会更有效。函数 *find_parent_of ()* 就是这样做的。

使用以下代码运行此代码:

```
replies = reply_thread_maker(conv_ids)# WRITE REPLIES TO FILE
replies.to_csv("replies.csv")#VIEW SAMPLE REPLIES
replies.head()
```

## 8.请访问 replies.csv 并分析您的回复。鳍。

我试图把这种方法放在一个 colab 笔记本上，让它像一个基于网络的工具一样工作。它仍然需要一些编程知识和对 API 的理解，但是我已经尝试让它尽可能的即插即用，而不是把它变成一个完整的项目。

我还没有看到很多处理这个话题的教程或博客，尤其是在 Twitter 的 v2 API 发布之后。就我个人而言，我所知道的希望这个特性早点出现的人比我用手指和脚趾所能数出来的还要多，这只能意味着一件事——一篇博客文章。

在花了几乎一整天的时间寻找检索推文回复的方法之后——这是我放弃前的最后一次尝试——我偶然发现了这个解决方案。这个故事的寓意是，坚持不懈真的会有回报。这也是我作为程序员在使用 twitter 的 API 时最美好的一天。如果你读到这里，读到了这句话--我欠你一杯咖啡--但只有在我毕业并找到工作之后--这些条件才适用。

## 未来方向

我不希望你问我得到这些回复有什么意义，所以我借此机会列出了一些可能有用的地方:

*   测量推特引发的毒性
*   衡量推文引起的偏见或政治宽容(左倾/右倾反应分类)
*   研究公众对新闻的反应情绪
*   研究公众对谣言和错误信息的反应

对于任何评论，反馈，问题，聊天等。你可以在这里找到我。

—

***aditya Narayan an****是布法罗大学的研究生。他专门从事* ***运筹学*** *并有* ***计算机科学*** *的背景。阅读新闻并加以评论是他的本能。*

他有另一个自我，无聊时喜欢创作各种数字内容，但更喜欢纸和笔。他为多个大型体育实体创建了数字内容，如 LaLiga Santander、Roland-Garros 和 NBA 篮球学校。

*这是他的*[***Linkedin***](http://linkedin.com/in/adithya-narayanan)*，还有他的*[***email***](http://adithyan@buffalo.edu)*。*