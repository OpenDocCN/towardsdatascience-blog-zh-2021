# 为意大利语培训上下文拼写检查器

> 原文：<https://towardsdatascience.com/training-a-contextual-spell-checker-for-italian-language-66dda528e4bf?source=collection_archive---------11----------------------->

![](img/a95d6e463ef3b22516d98c58cdc8b0b5.png)

图片鸣谢:Unsplash。

## 介绍

在[之前的帖子](https://medium.com/spark-nlp/applying-context-aware-spell-checking-in-spark-nlp-3c29c46963bc)中，我们已经介绍了拼写检查，以及在生成更正时需要考虑上下文线索。我们还讨论了 Spark-NLP 库中用于拼写检查的预训练模型的可用性。虽然，预先训练的模型在许多情况下都很方便，但有时你需要针对特定的领域，如金融或特定的语言，如德语。在这篇文章中，我们将从头开始探索训练一个上下文拼写检查器。

我们将深入研究创建上下文拼写检查模型所涉及的训练过程和数据建模。我们将使用通用意大利语作为我们的目标语言，但是这些步骤可以应用于您感兴趣的任何其他领域或语言。

我们将使用[PaisàCorpus](http://www.corpusitaliano.it/)作为我们的训练数据集，我们将添加自定义的单词类来处理像地点和名称这样的事情；最后，我们将看到如何使用 GPU 和 Spark-NLP 来加速训练。我们开始吧！。

## 本文的代码

您可以在下面的笔记本中找到本文的完整代码以及一些附加示例，

[训练上下文拼写检查器——意大利语。](https://github.com/JohnSnowLabs/spark-nlp-workshop/blob/master/tutorials/blogposts/5.TrainingContextSpellChecker.ipynb)

# 数据准备

您将从这个 URL 下载语料库文件。接下来，我们将应用一些清理原始文本，以消除一些噪音。

让我们从添加一些基本导入开始，

```
from sparknlp.annotator import *
from sparknlp.common import *
from sparknlp.base import *
import sparknlp
spark = sparknlp.start()
```

接下来让我们加载语料库数据。

```
from pyspark.sql.functions import *
paisaCorpusPath = “/path/to/paisa.raw.utf8”# do some brief DS exploration, and preparation to get clean text
df = spark.read.text(paisaCorpusPath)
df = df.filter(~col(‘value’).contains(‘</text’)).\
 filter(~col(‘value’).contains(‘<text’)).\
 filter(~col(‘value’).startswith(‘#’)).\
 limit(10000)df.show(truncate=False)
```

这就是我们得到的，

```
+--------------------+
|               value|
+--------------------+
|Davide Guglielmin...|
|Avete partecipato...|
|Otto mesi di rita...|
|Il prezzo dei big...|
|Dopo la “Shopping...|
|Il nome è tutto u...|
|Ovvero? Ovvero do...|
|Il patto ha lo sc...|
|La tre giorni org...|
|Quanto pagate per...|
|Per contro, nell’...|
|Non è poco, ovvia...|
|Dopo una lunga as...|
|La serata sarà un...|
|I primi 4 negozi ...|
|La Chaingang Rota...|
|La Chaingang Rota...|
|Tra le molte rami...|
|Un primo corollar...|
|E' proprio nel di...|
+--------------------+
only showing top 20 rows
```

我们有 7555997 个段落，每个段落包含几个句子，由于训练不需要带标签的数据，这就是我们所需要的。

# 基本管道设置

现在我们已经有了良好的数据，我们将设置一个基本的管道来进行处理。这是典型的 Spark-NLP 管道，数据在注释器中作为注释流动。

```
assembler = DocumentAssembler()\
 .setInputCol(“value”)\
 .setOutputCol(“document”)\tokenizer = RecursiveTokenizer()\
 .setInputCols(“document”)\
 .setOutputCol(“token”)\
 .setPrefixes([“\””, ““”, “(“, “[“, “\n”, “.”, “l’”, “dell’”, “nell’”, “sull’”, “all’”, “d’”, “un’”])\
 .setSuffixes([“\””, “””, “.”, “,”, “?”, “)”, “]”, “!”, “;”, “:”])spellChecker = ContextSpellCheckerApproach().\
    setInputCols("token").\
    setOutputCol("corrected").\
    setLanguageModelClasses(1650).\
    setWordMaxDistance(3).\
    setEpochs(10).\
    addVocabClass('_NAME_', names).\
    updateRegexClass('_DATE_', date_regex)
```

我们从我们的 DocumentAssembler 开始，为它输入我们在上一步中准备好的数据。然后我们继续我们的 RecursiveTokenizer，其中我们设置了一些特殊的选项来处理意大利语的一些细节。

最后，我们有我们的拼写检查。前两个方法调用是管道中连接输入和输出的常用方法。然后，我们在拼写检查器 setLanguageModelClasses()中为语言模型设置了一个依赖于词汇大小的设置，模型将使用它来控制语言模型中的因子分解。

基本上，因式分解背后的思想是，我们不会孤立地对待单词，而是将它们分组，给每个单词分配一个特定的类，以及该类中的一个 id。这有助于加快训练和推理过程中的内部处理。

接下来是 setWordMaxDistance(3)，它简单地告诉拼写检查器我们的错误最多在编辑距离 3 处。

历元数是您从神经网络中了解到的典型梯度下降参数，它将由拼写检查器中的神经语言模型使用。

最后但同样重要的是，我们增加了一个特殊的词类，在这个例子中是用来处理名字的。让我们进入更多的细节！。

# 添加特殊类别

这一节是可选的，它解释了我们在注释器设置代码末尾进行的 addVocabClass()和 addRegexClass()调用。如果您不想添加特殊的类或者您想稍后再来，只需注释掉这两行。

在这一点上，我们有一个相当大的语料库，足以让我们很好地接触到不同风味的意大利语。

尽管如此，除了我们在语料库中看到的，我们可能还想有另一种机制来构建我们的模型词汇表。这就是特殊类发挥作用的地方。

有了这个，我们就可以释放包含在特定数据集中的知识的力量，如名称词典或地名词典。

它们不仅允许我们教会拼写检查器保留一些单词，还可以提出更正建议，而且与主词汇表中的单词不同，一旦模型经过训练，您就可以更新它们。例如，如果在模型被训练之后，您仍然希望能够支持一个新名称，您可以通过更新 names 类来实现，而不需要从头开始重新训练。

当你像我们在这里做的那样从头开始训练拼写检查器时，你会得到两个预定义的特殊类:_DATE_，和 _NUM_，分别用于处理日期和数字。您可以覆盖它们，也可以添加更多的类。

按照我们关于名字的例子，我们将在 Spark-NLP 的上下文拼写检查器中添加意大利名字作为一个特殊的类，

```
import pandas as pd
import io
import requests# Get a list of common Italian names
url=”[https://gist.githubusercontent.com/pdesterlich/2562329/raw/7c09ac44d769539c61df15d5b3c441eaebb77660/nomi_italiani.txt](https://gist.githubusercontent.com/pdesterlich/2562329/raw/7c09ac44d769539c61df15d5b3c441eaebb77660/nomi_italiani.txt)"
s=requests.get(url).content# remove the first couple of lines (which are comments) & capitalize first letter
names = [name[0].upper() + name[1:] for name in s.decode(‘utf-8’).split(‘\n’)[7:]]# visualize
names
```

这是我们得到的结果(被截断)，

```
['Abaco',
 'Abbondanza',
 'Abbondanzia',
 'Abbondanzio',
 'Abbondazio',
 'Abbondia',
 'Abbondina',
 'Abbondio',
 'Abdelkrim',
 'Abdellah',
 'Abdenago',
 'Abdon',
 'Abdone',
 'Abela',
 'Abelarda',
 'Abelardo',
 'Abele',
```

这就是我们在前面清单中注释器设置期间传递给拼写检查器的*名称*变量。

如果您关注了我们如何设置注释器，您可能还会注意到，我们使用 updateRegexClass()函数调用为数字配置了第二个特殊的类。

这与为修正候选项添加额外来源的目的是一样的，但是这次是针对日期，所以我们需要传递一个正则表达式来描述我们想要覆盖的日期。

这里我们将集中讨论一种特定的格式，但是您也可以添加自己的格式，

**格式:**日/月/年

**正则表达式:**

([0–2][0–9]|30|31)\\/(01|02|03|04|05|06|07|08|09|10|11|12) \\/(19|20)[0–9]{2}

理解我们刚刚创建的正则表达式的幕后发生的事情很重要。首先，我们创建的正则表达式是一个有限的正则表达式，也就是说，它匹配的词汇表或字符串集是有限的。这很重要，因为拼写检查器将枚举正则表达式，并用它创建一个 Levenshtein 自动机。

## 训练模型

现在我们已经准备好了管道，我们将调用 fit()，并传递我们的数据集。

```
pipeline = Pipeline(
 stages = [
    assembler,
    tokenizer,
    spellChecker
 ])
model = pipeline.fit(df)
```

## 使用 GPU

你会注意到训练过程不是很快。那是因为到目前为止我们只使用了 CPU。我们可以通过用 spark-nlp-gpu 替换 spark-nlp 库来显著加快训练速度。

您几乎不需要做任何更改就可以做到这一点，只需像这样将正确的标志传递给 Spark-NLP start()函数，

```
sparknlp.start(gpu=True)
```

## 玩模型

让我们看看这个模型能做什么！。

```
lp.annotate(“sonno Glorea ho lasciatth la paterte sul tavolo acanto allu fruttu”)[‘corrected’]['sono',
 'Gloria',
 'ho',
 'lasciato',
 'la',
 'patente',
 'sul',
 'tavolo',
 'accanto',
 'alla',
 'frutta']
```

我们可以看到，模型确实在生成考虑上下文的校正，例如,“allu”被校正为“alla ”,而其他选项如“alle”或“allo”也在离输入单词 1 的相同编辑距离处，然而模型做出了正确的选择。我们还可以看到名字的词汇化有助于模型理解和纠正专有名称“Gloria”。

总的来说，这就是我们从这个模型中所能期待的，当多个相似的、距离相等的候选对象都有可能时，采取正确的选择。该模型可以做出一些更冒险的决定，比如实际替换词汇表中存在的输入单词。

这更多的是语法检查的工作，例如，从语法上来看。尽管如此，如果您想探索您可以在多大程度上扩展这个模型来执行这些修正，在本文的配套笔记本中有更多的例子。

## 通过模型中心访问模型

虽然这个模型并不完美，但我已经把它上传到了[模型中心](https://nlp.johnsnowlabs.com/models)，并在社区上公开，以防有人觉得有用。您可以通过以下方式访问它，

```
ContextSpellChecker.pretrained(“italian_spell”, “it”)
```

你也可以训练你自己的模型，并按照[这些步骤](https://modelshub.johnsnowlabs.com/)与社区分享。

## 关于标记化的一句话

这一部分是可选的，但是推荐给那些试图为特定语言或领域微调模型的人。

在许多 NLP 任务中，标记化部分是至关重要的，拼写检查也不例外。您将从标记化中获得的单元将非常重要，因为这些单元将定义您的模型检测错误、生成更正以及利用上下文对不同的更正进行排序的级别。

这里，我们使用 RecursiveTokenizer，之所以这样叫是因为它将继续递归地应用它的标记化策略，也就是说，应用到前面的标记化步骤的结果，直到该策略不能再应用为止。

RecursiveTokenizer 中的默认标记化是在空白上分割。为了进一步标记化，您可以指定中缀模式、前缀模式和后缀模式。

中缀模式是可以在标记中间找到的字符，您希望在其上进行拆分。相应地，前缀模式和后缀模式是您想要拆分的字符，它们将分别出现在每个标记的开头和结尾。

您可以在文档中找到这个 RecursiveTokenizer 的更多选项。Spark-NLP 库中也有其他的记号赋予器。

## 结论

我们已经探索了如何训练我们自己的拼写检查器，如何准备我们的数据和管道，如何为特殊类提供规则，如何使用 GPU，以及如何调整标记化以更好地满足我们的特定需求。

Spark-NLP 库提供了一些合理的默认行为，但是也可以针对特定的领域和用例进行调整。

希望你可以开始训练和分享你自己的模型，在博卡阿尔卢波！！