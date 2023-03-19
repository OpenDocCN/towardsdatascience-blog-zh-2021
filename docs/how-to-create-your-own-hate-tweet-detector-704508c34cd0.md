# 如何创建自己的仇恨推特探测器

> 原文：<https://towardsdatascience.com/how-to-create-your-own-hate-tweet-detector-704508c34cd0?source=collection_archive---------13----------------------->

## 一步一步的教程，关于开发一个机器学习分类算法来检测 Python 中的仇恨推文

![](img/565049750c4aad7d3b7609f2efc224a6.png)

乔恩·泰森在 [Unsplash](https://unsplash.com/s/photos/hate?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# ***发布一条仇恨的推文会有什么后果？***

在推特层面，推特可以采取以下行动:

1.  将推文标记为包含有争议或误导的信息
2.  要求您删除该推文，然后才能再次发布
3.  隐藏违规的推文，等待删除

在账户层面，这些行动是针对惯犯或做了特别恶劣事情的人采取的:

1.  要求编辑配置文件或媒体内容，并使其在编辑前不可用
2.  将帐户设置为只读模式会阻止推文、转发或类似功能
3.  要求验证帐户所有权，以检测拥有多个帐户的匿名用户
4.  永久暂停

Twitter 不同的“地狱圈”旨在确保没有用户会因为无意中发布了冒犯性的消息而受到过于严厉的惩罚。因此，除非你是一个惯犯，否则你不太可能删除你的帐户或将其置于只读模式。

# 仇恨言论真的那么普遍吗？

近年来，仇恨推特业务已经远远超出了柠檬水摊位。早在 2018 年第一季度，脸书就对 250 万条仇恨言论内容采取了行动，但在 2020 年最后一个季度，**脸书对高达****2690 万条仇恨言论**采取了行动。所以每个月大约有 900 万英镑左右。

脸书和推特都已经开发了检测仇恨言论的专有算法，因为它们看到了仇恨言论的恶毒和阴险性质，特别是在社交媒体平台上，因为它能够煽动暴力。能够开发这样的检测模型将被证明对任何初创公司都是有用的，特别是社交媒体，或者任何希望监控 Reddit 等在线论坛或公司内部通信中的仇恨言论的公司。

# 开发部署模型的步骤

![](img/eebcb34b909d043c81c33d000b611910.png)

马尔科·比安切蒂在 [Unsplash](https://unsplash.com/s/photos/steps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

下面是我对从原始数据到准备部署的模型的进展的粗略概述:

1.  环境设置
2.  数据收集
3.  数据准备
4.  模型训练和评估
5.  超参数调谐
6.  模型预测法
7.  模型部署

您将在以下笔记本中找到该项目的完整代码:

[](https://github.com/datascisteven/Medium-Blogs/blob/main/Hate-Tweet-Detector/Hate_Tweet_Detector.ipynb) [## Medium-Blogs/Hate _ Tweet _ detector . ipynb at main datassist even/Medium-Blogs

### 我的中型博客的代码库。通过在…上创建一个帐户，为 datassisteven/Medium-Blogs 的发展做出贡献

github.com](https://github.com/datascisteven/Medium-Blogs/blob/main/Hate-Tweet-Detector/Hate_Tweet_Detector.ipynb) 

# 环境设置

![](img/bc16ad7df23c01ccc76632a1fe630517.png)

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/hate?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

要创建环境，请将`environment.yml`放在您想要创建项目的文件夹中，并从终端中的同一个文件夹运行以下代码:

```
$ conda env create -f environment.yml
```

# 数据收集

开发任何有监督的机器学习算法机器的第一步是数据收集，即获得有标签的数据集。我从 Davidson 数据集开始，因为它已经包含了数据库中实际的 tweet 文本，我可以开始这个项目了，但是大多数带标签的数据库只提供 tweet ID。

由于数据集呈现出巨大的阶级不平衡，我进行了彻底而狂热的搜索，寻找额外的带标签的数据集来支持我的少数阶级，并偶然发现了这个网站:[hatespeechdata.com](https://hatespeechdata.com/)。我最终决定了来自哥本哈根[大学](https://github.com/ZeerakW/hatespeech)、[亚里士多德大学](https://github.com/ENCASEH2020/hatespeech-twitter)和 [HASOC 2019 共享任务](https://hasocfire.github.io/hasoc/2019/dataset.html)数据集的数据集。

最初的戴维森数据集总共包括 24783 条推文，其中 23353 条(或 94.2%)被标记为非仇恨，1430 条(或 5.8%)被标记为仇恨。在将额外的少数民族实例合并到数据集之后，我通过将少数民族类实例增加到 7025 条(或 30.1%)仇恨推文(总共 30378 条推文)来解决类不平衡问题。

## Twitter API

让我们来看看如何从 Twitter 上下载自己的一组仇恨推文。

对于只有 tweet IDs 的数据集，您需要申请一个开发人员帐户来访问 Twitter API 以获取 tweet 文本。公平的警告，它可能不是一个立竿见影的过程。Twitter 联系我，向我详细介绍我的项目细节。

完成后，创建一个应用程序从 Twitter 获取 API 键，然后将它们放在一个`config.py`文件中，无论它们是被赋给变量，还是作为值放在字典中，等等。

将这个`config.py`文件放入`.gitignore`以保持 API 密匙私有。要使用密钥，请将模块导入您的笔记本电脑:

```
from config import keys
```

如果`keys`是一个字典，我们可以很容易地用字典的不同键来检索 API 键:`keys[‘key_value’]`。

Twitter 要求以特定的格式提交 id，每批不超过 100 个，以字符串格式提交，id 之间用逗号分隔，不能有空格。

第一个函数创建一个字符串列表，其中每个字符串是一批 100 个逗号分隔的 tweet IDs:

这段代码是在 Postman 代理的帮助下生成的，当前一个函数生成的列表作为`tweets_ids:`传递给下面的函数时，第二个函数生成请求字段的数据帧

为了您的方便，我已经在 repo 中包含了我的组合数据集`combined.csv`,以防您想跳过这些数据收集步骤。

# 数据准备

为了在机器学习算法中使用自然语言，我们必须将单词转换为数字，这是算法可以识别的形式。

## 推文与文本

让我们想想一条推文与任何一篇文学文本有何不同:

1.  可能语法不正确
2.  拼写可能不正确(即缩写、组合词、重复字母，如 FML、F$ckMyLife、Fuuuuuu$$ my life)
3.  使用特殊字符，如#或@和表情符号

这里的目标是删除任何特殊字符，用户名，URL 链接，转发，任何不会增加句子语义的东西。

所以在这里进行一些无耻的自我宣传，但如果你需要快速刷新正则表达式，请查看我的帖子: [***To RegEx or Not To RegEx，第一部分***](http://tinyurl.com/regex-blog) 和[***To RegEx or Not To RegEx，第二部分***](http://tinyurl.com/regex2-blog) 。

## 用于预处理推文的代码

我将简要讨论我在创建用于预处理推文的函数时的思维过程:

第一个函数用于词汇化，它返回每个单词的根形式，即“dimension”、“dimensional”和“dimensionality”都作为“dimension”返回。第二个功能是创建一个标记化和词条化的 tweets 列表，其中停用词和少于 3 个字符的词使用模块`gensim`进行标记化，这会自动降低所有词的大小写。

## 预处理功能的组件

1.  转发是 Twitter 上转发的消息，包含 RT 标签和转发的 tweet 文本，有时还包含用户名。我决定保留转发的文本，但删除“RT @username ”,因为用户名没有增加语义值。
2.  推文通常包含其他网站、推文、在线媒体等的 URL 链接。我删除了 http 和 bit.ly 链接，后者是一个 URL 缩短服务。
3.  Unicode characters 有一个 HTML 代码，以下面两个字符`&#`开头，后面跟着一个数字，可以指表情符号、符号、标点符号等。除了常规的标点符号之外，我还删除了这些，但在删除标点符号之前，我删除了 URL 链接。
4.  Hashtags 是前面有一个哈希(`#`)符号的单词或短语，用于标识它与特定主题相关，我决定保留这个短语，因为它通常具有一些语义值。
5.  我将任何 1+的空白改为单个，并删除了任何前导和尾随空白。
6.  我按照内部大写的指示，将所有连接的单词(如 AaaaBbbbCccc)分开。
7.  我删除了所有大于 2 的重复字母，因为在英语中没有一个字母重复两次以上的例子。

我们最终得到的是一个小写单词的字符串，这些单词要么是词干化的，要么是词干化的，由空格分隔，并且在数据帧的每一行中都删除了停用词、1 个和 2 个字母的单词、特殊字符、用户名、标点符号、额外的空格和 URL 链接:

```
“lemma space separate lowercase tweet”
```

# 模型训练和预测

让我们通过下面的步骤创建一个管道，`TfidfVectorizer()`，它结合了`CountVectorizer()`和`TfidfTransformer()`的功能，前者将文本转换为令牌计数矩阵，后者将该矩阵转换为规范化的 tf-idf 表示。

然后，我们在训练集上拟合模型，这里的`X-train`是预处理的 tweet 文本。我们不需要使用`fit_transform`，因为`TfidfVectorizer()`包含一个变压器:

一旦我们符合这个模型，我们就可以继续进行预测，并将任何推文分类为讨厌或不讨厌:

我们必须通过`preprocess()`函数传递推文，并使用经过训练的管道进行预测。这条推文最终被归类为仇恨推文。

注意:通过比较 6 种不同的算法并使用 GridSearchCV 执行超参数调整，确定这是最好的。结果位于以下报告中:

[](https://www.github.com/datascisteven/Automated-Hate-Tweet-Detection) [## GitHub-datassisteven/Automated-Hate-Tweet-Detection:开发一个分类模型来检测…

### 作者:Steven Yan 这个项目建立在我和 Ivan Zarharchuk 的联合项目之上

www.github.com](https://www.github.com/datascisteven/Automated-Hate-Tweet-Detection) 

# 下一步是什么？

1.  您可以包含额外的标注数据集来改善类别不平衡。
2.  您可以对预处理函数进行额外的调整。
3.  您可以尝试不同的分类算法，包括 Doc2Vec 或神经网络。

在下一篇博客中，我将仔细看看最后一步:部署。我将看看我们作为数据科学家可以部署模型的不同方式。

*在*[*Github*](https://github.com/datascisteven)*[*LinkedIn*](https://www.linkedin.com/in/datascisteven)*上与我联系，或者通过* [*邮箱*](mailto: datascisteven@gmail.com) *。在下面的* [*网站*](https://datascisteven.github.io) *看看我以前的博客和转帖。**