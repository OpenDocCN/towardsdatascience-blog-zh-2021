# 为您的下一个 NLP 项目准备的 5 个鲜为人知的 Python 库

> 原文：<https://towardsdatascience.com/5-lesser-known-python-libraries-for-your-next-nlp-project-ff13fc652553?source=collection_archive---------2----------------------->

## 带有代码示例和解释。

![](img/03001a561ee3ebe311ed92857a8272bf.png)

图片由[有意识设计](https://unsplash.com/@conscious_design)来自 [Unsplash](https://unsplash.com/)

当我第一次开始阅读 Medium 时，我最喜欢的文章类型是那些向我介绍新的 Python 库的文章。我了解的几个库现在是我日常项目的一部分。

为了传播它，我想分享我在过去几年做各种自然语言处理(NLP)工作中发现的 5 个很棒的 Python 库。

# **1)宫缩**

当然，你可以写一长串正则表达式来扩展文本数据中的缩写*(即不要* → *不要；不能* → *不能；还没有* → *还没有)。*但是为什么不省点力气，利用 Python 库来帮你完成繁重的工作呢？

缩写是一个易于使用的库，它将扩展常见的英语缩写和俚语。它速度快，效率高，并且可以处理大多数边缘情况，比如丢失撇号。

## **安装**

```
pip install contractions
```

## **示例**

```
import contractionss = "ive gotta go! i'll see yall later."
text = contractions.fix(s, slang=True)
print(text)
```

## **结果**

```
ORIGINAL: ive gotta go! i’ll see yall later.OUTPUT: I have got to go! I will see you all later.
```

## 用例

文本预处理的一个重要部分是创建一致性，并在不丢失太多含义的情况下削减唯一单词的列表。例如，词袋模型和 TF-IDF 创建了大型稀疏矩阵，其中每个变量都是语料库中不同的词汇。扩展缩写可以进一步降低维数，甚至有助于过滤掉停用词。

[文献](https://github.com/kootenpv/contractions)

# **2)蒸馏标点符号**

恢复丢失的标点符号到纯英语文本…听起来很简单，对吗？对计算机来说，要做到这一点肯定要复杂得多。

Distilbert-punctuator 是我能找到的唯一一个执行这项任务的 Python 库。而且超级准确！这是因为它使用了一个精简版本的 BERT，这是一个由谷歌提供的最先进的预训练语言模型。它进一步微调了超过 20，000 篇新闻文章和 4，000 篇 TED 演讲的文字记录，以检测句子的边界。当插入句尾标点符号时，例如句号，模型还会适当地大写下一个起始字母。

## **安装**

```
pip install distilbert-punctuator
```

> 专业提示:这个库需要相当多的依赖项。如果你在安装上有问题，那么在 Google Colab 上试试，在这种情况下，你需要运行！皮普安装蒸馏标点符号代替。

## **例子**

```
from dbpunctuator.inference import Inference, InferenceArguments
from dbpunctuator.utils import DEFAULT_ENGLISH_TAG_PUNCTUATOR_MAPargs = InferenceArguments(
        model_name_or_path="Qishuai/distilbert_punctuator_en",
        tokenizer_name="Qishuai/distilbert_punctuator_en",
        tag2punctuator=DEFAULT_ENGLISH_TAG_PUNCTUATOR_MAP
    )punctuator_model = Inference(inference_args=args, 
                             verbose=False)text = [
    """
however when I am elected I vow to protect our American workforce
unlike my opponent I have faith in our perseverance our sense of trust and our democratic principles will you support me
    """
]print(punctuator_model.punctuation(text)[0])
```

## 结果

```
ORIGINAL: 
however when I am elected I vow to protect our American workforce
unlike my opponent I have faith in our perseverance our sense of trust and our democratic principles will you support meOUTPUT:
However, when I am elected, I vow to protect our American workforce. Unlike my opponent, I have faith in our perseverance, our sense of trust and our democratic principles. Will you support me?
```

## **用例**

有时，您只是希望您的文本数据在语法上更加正确和更具可读性。无论任务是修复混乱的 Twitter 帖子还是聊天机器人消息，这个库都是为你准备的。

[文档](https://pypi.org/project/distilbert-punctuator/)

# **3)文本统计**

Textstat 是一个易于使用的轻量级库，它提供了关于文本数据的各种指标，如阅读水平、阅读时间和字数。

## **安装**

```
pip install textstat
```

## **例子**

```
import textstattext = """
Love this dress! it's sooo pretty. i happened to find it in a store, and i'm glad i did bc i never would have ordered it online bc it's petite. 
"""# Flesch reading ease score
print(textstat.flesch_reading_ease(text))
  # 90-100 | Very Easy
  # 80-89  | Easy
  # 70-79  | Fairly Easy
  # 60-69  | Standard
  # 50-59  | Fairly Difficult
  # 30-49  | Difficult
  # <30    | Very Confusing# Reading time (output in seconds)
# Assuming 70 milliseconds/character
print(textstat.reading_time(text, ms_per_char=70))# Word count 
print(textstat.lexicon_count(text, removepunct=True))
```

## 结果

```
ORIGINAL:
Love this dress! it's sooo pretty. i happened to find it in a store, and i'm glad i did bc i never would have ordered it online bc it's petite.OUTPUTS:
74.87 # reading score is considered 'Fairly Easy'
7.98  # 7.98 seconds to read
30    # 30 words
```

## **用例**

这些指标增加了一层额外的分析。比方说，你正在从一本八卦杂志中寻找一组名人新闻文章。使用 textstat，你可能会发现更快更容易阅读的文章更受欢迎，保留率也更高。

[文档](https://pypi.org/project/textstat/)

# **4)胡言乱语检测器**

这个低代码库的主要目的是检测乱码(或难以理解的单词)。它使用一个在大型英语单词语料库上训练的模型。

## **安装+** 培训

```
pip install gibberish-detector
```

接下来，您还需要在您的终端上训练模型，但这非常简单，只需要一分钟。只需遵循以下步骤:

1.  从[这里](https://github.com/rrenaud/Gibberish-Detector/blob/master/big.txt)下载名为 big.txt 的训练语料库
2.  打开您的 CLI 和 *cd* 到 big.txt 所在的目录
3.  运行下面的:*胡言乱语-检测器训练。\big.txt >胡言乱语-detector.model*

一个名为*的文件将会在你当前的目录下创建。*

## **示例**

```
from gibberish_detector import detector# load the gibberish detection model
Detector = detector.create_from_model('.\gibberish-detector.model')text1 = "xdnfklskasqd"
print(Detector.is_gibberish(text1))text2 = "apples"
print(Detector.is_gibberish(text2))
```

## 结果

```
True  # xdnfklskasqd (this is gibberish)False # apples (this is not)
```

## **用例**

我过去曾使用过 gibbish-detector 来帮助我从数据集中删除不良的观察结果。

它也可以被实现用于用户输入的错误处理。例如，如果用户在你的 web 应用程序上输入了无意义、无意义的文本，你可能想要返回一个错误消息。

[文档](https://pypi.org/project/gibberish-detector/)

# **5) NLPAug**

我把最好的留到了最后。这个多用途的图书馆确实是一个隐藏的宝石。

首先，什么是数据增强？它是通过添加现有数据的稍微修改的副本来扩展训练集大小的任何技术。当现有数据多样性有限或不平衡时，通常使用数据扩充。对于计算机视觉问题，增强用于通过裁剪、旋转和改变图像的亮度来创建新的样本。对于数字数据，可以使用聚类技术创建合成实例。

但是如果我们处理的是文本数据呢？

这就是 NLPAug 的用武之地。该库可以通过替换或插入语义相关的单词来扩充文本。

它通过采用像 BERT 这样的预训练语言模型来实现这一点，这是一种强大的方法，因为它考虑了单词的上下文。根据您设置的参数，前 *n* 个相似单词将用于修改文本。

预训练的单词嵌入，如 Word2Vec 和 GloVe，也可以用于用同义词替换单词。点击[此处](https://www.analyticsvidhya.com/blog/2021/08/nlpaug-a-python-library-to-augment-your-text-data/)阅读一篇展示该库全部功能的精彩文章。

## **安装**

```
pip install nlpaug
```

## **例子**

```
import nlpaug.augmenter.word as naw# main parameters to adjust
ACTION = 'substitute' # or use 'insert'
TOP_K = 15 # randomly draw from top 15 suggested words
AUG_P = 0.40 # augment 40% of words within textaug_bert = naw.ContextualWordEmbsAug(
    model_path='bert-base-uncased', 
    action=ACTION, 
    top_k=TOP_K,
    aug_p=AUG_P
    )text = """
Come into town with me today to buy food!
"""augmented_text = aug_bert.augment(text, n=3) # n: num. of outputsprint(augmented_text)
```

## 结果

```
ORIGINAL:
Come into town with me today to buy food!OUTPUTS:
• drove into denver with me today to purchase groceries!
• head off town with dad today to buy coffee!
• come up shop with mom today to buy lunch!
```

## **用例**

假设您正在一个数据集上训练一个监督分类模型，该数据集有 15k 个正面评论，只有 4k 个负面评论。像这样严重不平衡的数据集将在训练期间产生对多数类(正面评论)的模型偏差。

简单地复制少数类的例子(负面评论)不会给模型增加任何新的信息。相反，利用 NLPAug 的高级文本增强功能来增加少数民族类别的多样性。该技术已被[证明](/powerful-text-augmentation-using-nlpaug-5851099b4e97)可改善 AUC 和 F1 评分。

[文档](https://github.com/makcedward/nlpaug)

# **结论**

作为数据科学家，Kaggle 的竞争对手，或者一般的程序员，我们在口袋里放尽可能多的工具是很重要的。我们可以利用这些库来解决问题，增强我们的数据集，并花更多的时间来思考解决方案，而不是编写代码。

如果你对 NLP 感兴趣，可以看看我最近写的关于命名实体识别(NER)的文章。我将展示如何使用 spaCy——另一个强大的 NLP 库——来构建一个模型，自动识别非结构化文本中与家庭装修服务相关的单词。

<https://levelup.gitconnected.com/auto-detect-anything-with-custom-named-entity-recognition-ner-c89d6562e8e9>  

感谢阅读！