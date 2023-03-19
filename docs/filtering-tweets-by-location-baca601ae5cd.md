# 按位置过滤推文

> 原文：<https://towardsdatascience.com/filtering-tweets-by-location-baca601ae5cd?source=collection_archive---------30----------------------->

## 账户位置元数据+正则表达式==推文位置过滤

![](img/269bd97d9ebde3db76c82ce9320fead5.png)

内森·杜姆劳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在我的[最新项目](https://github.com/lspope/capstone)中，我探索了这个问题**“在新冠肺炎·疫情期间，美国公众对 K-12 学习的看法如何？”**。使用从 Twitter、自然语言处理和监督机器学习收集的数据，我创建了一个文本分类器来预测推文对这个话题的情绪。

因为我想了解美国的情绪，所以我需要按地点过滤推文。Twitter 开发者网站在可用选项上提供了一些很好的指导。

我选择使用**帐户位置**地理元数据。以下是来自 Twitter 开发者网站的详细信息:“基于用户在其公开资料中提供的‘家’的位置。这是一个自由格式的字符字段，可能包含也可能不包含可进行地理引用的元数据”。

在我们开始之前，这里有一些警告:

*   由于账户位置**不能**保证被填充，你必须接受你可能会错过相关推文的事实。
*   我使用方法依赖于包含美国标识符(比如有效的美国州名)的帐户位置。

如果你能接受这些警告，请继续阅读。:)

在我分享实际代码之前，我先简要介绍一下我的方法:

*   我创建了一个 Python 脚本来监听 Twitter 流中与我感兴趣的主题相关的推文。
*   在收到一条 Tweet 时，我获得帐户位置属性，并使用正则表达式检查它是否是美国的位置。
*   通过位置检查的 Tweets 将通过我的代码进行进一步处理。(在我的例子中，我将 Tweet 存储在 MongoDB 集合中。)

现在我已经分享了整个工作流程，让我们看看一些代码。

我的脚本使用了一个 tweepy。StreamListener 用于从 Twitter 流中收听传入的推文。神奇的事情发生在我的 StreamListener 的 **on_status** 函数中。

```
def on_status(self, tweet):
    # Checking tweet for likely USA location
    is_usa_loc = self.get_is_usa_loc(tweet) if is_usa_loc:
        # insert the tweet into collection
        twitter_collection.insert(tweet_doc)
    else:
        print(‘Could not determine USA location’) def get_is_usa_loc(self, tweet):
    is_usa_loc = False
    if tweet.user.location:
        user_loc_str = str(tweet.user.location).upper()
        is_usa_loc = re.search(usa_states_regex, user_loc_str) or re.search(usa_states_fullname_regex, user_loc_str)
    return is_usa_loc 
```

在 **on_status** 内，我调用一个助手函数: **get_is_usa_loc** 。在这个助手函数中，我首先检查以确保账户位置确实在 Tweet 中填充。如果是这样，我将 Account Location 转换为大写字符串，并对照两个正则表达式进行检查。一个正则表达式检查美国各州的双字符缩写。另一个正则表达式检查美国各州的全名。帐户位置只需匹配一个。:)

以下是我使用的正则表达式。第一个是州名缩写，第二个是州名全称和术语“美国”。

```
usa_states_regex = ‘,\s{1(A[KLRZ]|C[AOT]|D[CE]|FL|GA|HI|I[ADLN]|K[SY]|LA|M[ADEINOST]|N[CDEHJMVY]|O[HKR]|P[AR]|RI|S[CD]|T[NX]|UT|V[AIT]|W[AIVY])’ usa_states_fullname_regex = '(ALABAMA|ALASKA|ARIZONA|ARKANSAS|CALIFORNIA|'\
                            'COLORADO|CONNECTICUT|DELAWARE|FLORIDA|GEORGIA|HAWAII|'\
                            'IDAHO|ILLINOIS|INDIANA|IOWA|KANSAS|KENTUCKY|'\
                            'LOUISIANA|MAINE|MARYLAND|MASSACHUSETTS|MICHIGAN|'\
                            'MINNESOTA|MISSISSIPPI|MISSOURI|MONTANA|'\
                            'NEBRASKA|NEVADA|NEW\sHAMPSHIRE|NEWSJERSEY|'\
                            'NEW\sMEXICO|NEW\sYORK|NORTH\sCAROLINA|'\
                            'NORTH\sDAKOTA|OHIO|OKLAHOMA|OREGON|PENNSYLVANIA|'\
                            'RHODE\sISLAND|SOUTH\sCAROLINA|SOUTH\sDAKOTA|'\
                            'TENNESSEE|TEXAS|UTAH|VERMONT|VIRGINIA|'\
                            'WASHINGTON|WEST\sVIRGINIA|WISCONSIN|WYOMING|USA)'
```

就是这样！我刚刚使用一些简单的 Python 代码和正则表达式向您展示了通过帐户位置过滤 Twitter 数据的过程。

如果您发现这很有帮助，我鼓励您在自己的工作中扩展这个例子。请随时提出改进建议，或者让我知道这对您是否有帮助。

快乐过滤！