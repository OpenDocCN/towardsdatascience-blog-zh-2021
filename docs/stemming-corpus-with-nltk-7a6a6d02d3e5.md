# 用 NLTK 对文本进行词干分析

> 原文：<https://towardsdatascience.com/stemming-corpus-with-nltk-7a6a6d02d3e5?source=collection_archive---------7----------------------->

## 词干分析是文本规范化中使用最广泛的技术之一。在这篇文章中，我们将探索 nltk 的三个最著名的词干分析器

![](img/5e5ecaa09131432fcaf31ee109e1a9be.png)

马库斯·斯皮斯克的照片

T 处理文本数据的能力是一项巨大的技术成就，使公司和组织能够处理非结构化信息，并根据过去极难收集和存储的信息支持决策。

自然，由于计算机不像人类那样真正理解字符和单词，这带来了许多新的挑战，这些挑战可以通过使用专门处理文本数据的特定技术来解决，如词干化或词汇化。

**在这篇文章中，我们将探讨词干的概念，以及词干如何帮助你挖掘文本管道。**

## 词干定义

词干提取，也称为后缀剥离，是一种用于减少文本维数的技术。**词干化也是一种文本标准化类型，使您能够将一些单词标准化为特定的表达式，也称为词干。**

让我们从一个句子的例子开始:

> 这绝对是一个争议，因为律师称该案件“极具争议”

如果我们想将这个文本表示为一个数组(查看本系列的另一篇文章 )单词*争议*和*争议**会被认为是不同的标记，并在您可能构建的任何数组中有不同的位置。*

*词干有助于我们标准化有相同后缀的单词，这些单词通常是语法相似单词的派生词。*

*但是，像往常一样，在几个文本挖掘管道中，有许多可能导致信息丢失的选择。请记住，考虑到我们今天能够在 NLP 管道中应用的计算能力和更高级的模型(例如深度神经网络)，在 NLP 领域中词干提取是否真的是一个好主意，这是一个正在进行的讨论。*

*词干提取对于处理大量维度的算法来说可能是个好主意。回想一下，词干分析是一种标准化技术，但是像大多数标准化问题(例如主成分分析)一样，它会导致你丢失文本中的一些原始信息。对于 NLP 管道来说，这是一个好的还是坏的应用程序，这实际上取决于您使用的模型、您保存数据所需的空间量(尽管现在空间应该不是问题)以及您想要使用的算法的特征。*

*鉴于此，让我们探索一下 *NLTK(自然语言工具包)*库中最常见的词干分析器。*

## *波特的斯特梅尔*

*波特的斯特梅尔实际上是计算机科学中最古老的词干分析器应用程序之一。它第一次被提及是在 1980 年 Martin Porter 的论文 [*中的一种后缀剥离*](https://www.cs.toronto.edu/~frank/csc2501/Readings/R2_Porter/Porter-1980.pdf) 算法，它是***【nltk】***中广泛使用的词干分析器之一。*

***波特的斯特梅尔应用一套五个连续的规则(也称为阶段)来确定句子中常见的后缀。**举例来说，第一阶段首先从句子中移除复数和过去分词，例如单词 *cats* 被转换成 *cat* :*

> *猫->猫*

*从第二阶段到第五阶段，波特的算法根据前面辅音和元音的不同组合来阻止常见的词尾。*

*每个阶段的详细描述见[此处](http://facweb.cs.depaul.edu/mobasher/classes/csc575/papers/porter-algorithm.html)。*

*porter stemmer 最重要的概念之一是以下公式中的 *m* 的概念:*

*![](img/d3a48924cbf01c37f469556d8a439a5d.png)*

*波特的斯特梅尔算法辅音和元音公式*

*每个 *C* 都被认为是一个包含一个或多个辅音的字符串。每个 *V* 都被认为是一个包含一个或多个元音的字符串。元音字母被认为是字母 *a，e，I，o，u* 和 *y* 有一定的限制*。**

**m* 是波特词干分析器中最重要的概念之一，因为它定义了对特定单词应用特定规则的阈值。让我们来看一些单词的例子:*

*   *树->这个词可以用表示法 ***CV*** 来表示，因为 TR 被认为是 ***C*** 表示法而 ee 被认为是 ***V*** 表示法。在这种情况下， *m* 是零，因为这个单词只适合格式[C][V]*
*   *创作->这个词可以用 *CVCVC* 来表示。为了符合波特公式，可以用[C][VC]来表示，因此*m*等于 2。*
*   *燕麦->这个词可以用符合波特公式的 VC 来表示为[VC]。在这种情况下 *m* 等于 1。*

*请记住这个“T2”的概念，因为它对下面的一些解释很重要。*

*我们在 *nltk* 图书馆有波特的《斯特梅尔》:*

```
*from nltk.stem import PorterStemmer*
```

*要使用它，必须在导入后声明 PorterStemmer 的实例:*

```
*porter = PorterStemmer()*
```

*定义了 *PorterStemmer* 的实例后，我们可以根据算法中定义的规则对单词进行词干处理:*

```
*porter.stem('cats')*
```

*这个命令产生单词:*‘cat’—*单词 cats 的词干版本。**我们来看看更常见的例子:***

```
*porter.stem('amazing') returns ‘amaz’*
```

*出现这个词干是因为 *ing* 在英语单词中是一个如此常见的词尾，以至于单词 *amazing* 被词干化为单词 *amaz。*这个 *amaz* 词干也是由下面这些词产生的，惊叹、拍案叫绝、惊叹:*

```
*porter.stem('amazement') returns ‘amaz’
porter.stem('amaze') returns ‘amaz’
porter.stem('amazed') returns ‘amaz’*
```

****惊异*、*惊异*、*惊异*和*惊异*这四个单词在经过词干处理后，在我们的文本中被认为是同一个单词——这就是为什么我们称之为文本规范化！***

*作为一个反例，单词 *amazon* 并不是源于 *amaz* :*

```
*porter.stem('amazon') returns ‘amazon‘*
```

*为什么*亚马逊*中上的*没有被移除？这是因为某些词尾，比如上的**，在波特的算法中是不被考虑的——只有当它们前面有 *i* 的时候，比如*民族*、*预感、*等等。让我们测试这两个例子的词干:****

```
*porter.stem('nation') returns ‘nation‘
porter.stem('premonition') returns 'premonit'*
```

*现在一个新的问题出现了——为什么预感被阻止了，而国家却没有？*

***某些词尾只考虑某个类似字长的阈值**(我们的 *m* ！).如果你查看波特规则，你会看到后缀 **ION** 或 **TION** 仅在基于 *m* 的特定条件下应用于第二和第四阶段。*

*让我们一步一步地了解每个阶段，真正理解波特的斯特梅尔背后的规则。当词干分析器被顺序应用时，可以应用不止一个[规则](http://facweb.cs.depaul.edu/mobasher/classes/csc575/papers/porter-algorithm.html)，例如，单词 ***条件* (m 等于 4)** 在斯特梅尔阶段中遵循以下流程:*

*   *阶段 1:它不匹配该步骤认为的任何模式，例如以 **IES** 或 **SSES** 结尾，以及阶段 1b 和 1c 中存在的其他规则。*
*   *阶段 2:当 *m* 大于 0 并且单词以*国家*结尾时，词干分析器应用后缀 *al* 的裁剪，并且我们的单词成为 ***条件。****
*   *阶段 3:我们的单词*条件*(注意，现在我们将我们的单词视为*条件*，而不是原始的*条件*，因为我们已经在阶段 2 中应用了一些东西)。*
*   *阶段 4:这个单词以第*个第*个第*个第*个第>个第 1，所以后缀第*个第*个第*个第*个第*个第*个第*个第*个第*个第*个第*个第*个第*个第*个】个第*个】个第*个第【20】个第【18】个第【19】个】个第【18】个第【19】个】个第【***
*   *阶段 5:我们的单词 *condit* 与阶段 5 中可以剥离的任何模式都不匹配，因此我们的最终词干是 ***condit*** 。*

*正如您在上面的过程中看到的，在词干提取过程的各个阶段，我们的单词可能会被词干提取多次。*

*如果您需要检查每个阶段应用的规则列表，请返回此[链接](http://facweb.cs.depaul.edu/mobasher/classes/csc575/papers/porter-algorithm.html)。*

*波特的斯特梅尔是自然语言处理中最常用的词干技术之一，但自从它首次实现和开发以来已经过去了近 30 年，马丁·波特开发了一个名为 Porter2 的更新版本，由于它的 *nltk* 实现，它也被称为雪球斯特梅尔。*

## *雪球斯特梅尔*

*Snowball stemmer(正式名称为 Porter2)是波特的斯特梅尔的更新版本，引入了新规则，修改了波特的斯特梅尔中已经存在的一些现有规则。*

*雪球/搬运工 2 的详细规则在这里找到[。](http://snowball.tartarus.org/algorithms/english/stemmer.html)*

***其逻辑和过程与波特的斯特梅尔完全相同，单词在词干分析器的五个阶段中被依次词干化。***

*Snowball 和 Porter 之间的另一个相关区别是，Snowball 还详细说明了其他语言的规则——例如，您可以在这里访问葡萄牙语的规则。*

*你可以很容易地在 nltk 中访问 Snowball，就像我们访问波特的斯特梅尔一样——这里我们也同时定义了实例，但是我们必须将语言定义为一个参数:*

```
*from nltk.stem import SnowballStemmer
snowball = SnowballStemmer(language='english')*
```

*除了广泛回顾雪球的规则，让我们用一些例子来检查波特和雪球之间的一些重要区别:*

```
*porter.stem('fairly') -> returns fairli
snowball.stem('fairly') -> returns fair*
```

*在下面的例子中，斯诺鲍在规范副词*方面做得更好，*让它产生词干 *fair，*，而波特产生词干 *fairli。*这样做使得单词*的词干相当*与形容词*尚可，*相同，从规范化的角度来看似乎是有意义的。*

*这个规则也适用于其他副词:*

```
*porter.stem('loudly') -> returns loudli
snowball.stem('loudly') -> returns loud*
```

*Snowball 的另一个改进是它的规则防止了一些越界(从单词中去掉太多后缀)。让我们看看一般的*和*慷慨的*这两个词的例子——从波特的第一个开始:**

```
*porter.stem('generically') -> returns gener
porter.stem('generous') -> returns gener*
```

*根据波特的斯特梅尔，这些表达完全不同意思的词将被归结为同一个表达 *gener。**

*对于 Snowball，这种情况不会发生，因为两个单词的词干不同:*

```
*snowball.stem('generically') -> returns generical
snowball.stem('generous') -> returns generous*
```

*总之， **SnowballStemmer 被广泛认为是波特的《斯特梅尔》的改进版本。***

****nltk*** 的实现基于 Porter2，是对马丁·波特开发的原始算法的改进。*

## *兰卡斯特斯特梅尔*

*兰开斯特·斯特梅尔是兰开斯特大学的克里斯·佩斯在论文 [*中提出的另一个斯特梅尔*](https://dl.acm.org/doi/abs/10.1145/101306.101310) 中开发的词干分析器。*

*它的规则比波特和雪球更具侵略性，它是最具侵略性的词干分析器之一，因为它倾向于超越许多单词。*

*作为一般的经验法则，认为兰开斯特的斯特梅尔的规则试图减少单词到尽可能短的词干。你可以在这里查看兰卡斯特的一些规则[。](https://github.com/nltk/nltk/blob/develop/nltk/stem/lancaster.py#L62)*

*下面是 ***nltk 的*** 实现兰卡斯特的斯特梅尔:*

```
*from nltk.stem import LancasterStemmer
lanc = LancasterStemmer()*
```

*让我们来看一些例子，看看兰开斯特的斯特梅尔是如何词干化的，并将结果与斯诺鲍·斯特梅尔的方法进行比较——从单词*salt*开始:*

```
*snowball.stem('salty') returns 'salti'
lanc.stem('salty') returns 'sal'*
```

*现在对比一下销售这个词:*

```
*snowball.stem('sales') returns 'sale'
lanc.stem('sales') returns 'sal'*
```

*正如你所看到的，Lancaster 把单词 *salty* 和 *sales* 放在同一个词干中，这对于两个单词的意思来说没有多大意义。发生这种情况是因为 Lancaster 倾向于过度词干，这是我们说 Lancaster 词干是最积极的词干分析器之一的主要原因。*

## *评估词干分析器的影响*

*既然我们已经理解了词干是如何通过裁剪后缀来规范化文本的，那么我们如何理解这种规范化在我们的语料库中的影响呢？*

*由于词干化是一种导致信息丢失的标准化技术，最直接的方法之一是了解词干化前后文本长度之间的比率。这将给出一个近似值，即词干化后保留的信息的百分比。*

*对于这个例子，让我们使用维基百科中欧盟定义文章的第一段和第二段来评估我们的词干分析器:*

```
*eu_definition = 'The European Union (EU) is a political and economic union of 27 member states that are located primarily in Europe. Its members have a combined area of 4,233,255.3 km2 (1,634,469.0 sq mi) and an estimated total population of about 447 million. The EU has developed an internal single market through a standardised system of laws that apply in all member states in those matters, and only those matters, where members have agreed to act as one. EU policies aim to ensure the free movement of people, goods, services and capital within the internal market; enact legislation in justice and home affairs; and maintain common policies on trade, agriculture, fisheries and regional development. Passport controls have been abolished for travel within the Schengen Area. A monetary union was established in 1999, coming into full force in 2002, and is composed of 19 EU member states which use the euro currency. The EU has often been described as a *sui generis* political entity (without precedent or comparison).The EU and European citizenship were established when the Maastricht Treaty came into force in 1993\. The EU traces its origins to the European Coal and Steel Community (ECSC) and the European Economic Community (EEC), established, respectively, by the 1951 Treaty of Paris and 1957 Treaty of Rome. The original members of what came to be known as the European Communities were the Inner Six: Belgium, France, Italy, Luxembourg, the Netherlands, and West Germany. The Communities and their successors have grown in size by the accession of new member states and in power by the addition of policy areas to their remit. The United Kingdom became the first member state to leave the EU on 31 January 2020\. Before this, three territories of member states had left the EU or its forerunners. The latest major amendment to the constitutional basis of the EU, the Treaty of Lisbon, came into force in 2009.'*
```

*让我们通过我们的搬运工的斯特梅尔，看看结果。为了在 *nltk 的*实现上传递整个文本，我们必须使用 *word_tokenize* 函数来标记我们的文本:*

```
*from nltk.stem import PorterStemmer
from nltk.tokenize import word_tokenizetokenized_eu = word_tokenize(eu_definition)porter_eu = [porter.stem(word) for word in tokenized_eu]*
```

***让我们检查最终的词干版本:***

```
*'the european union ( EU ) is a polit and econom union of 27 member state that are locat primarili in europ . it member have a combin area of 4,233,255.3 km2 ( 1,634,469.0 sq mi ) and an estim total popul of about 447 million . the EU ha develop an intern singl market through a standardis system of law that appli in all member state in those matter , and onli those matter , where member have agre to act as one . EU polici aim to ensur the free movement of peopl , good , servic and capit within the intern market ; enact legisl in justic and home affair ; and maintain common polici on trade , agricultur , fisheri and region develop . passport control have been abolish for travel within the schengen area . A monetari union wa establish in 1999 , come into full forc in 2002 , and is compos of 19 EU member state which use the euro currenc . the EU ha often been describ as a sui generi polit entiti ( without preced or comparison ) .the EU and european citizenship were establish when the maastricht treati came into forc in 1993 . the EU trace it origin to the european coal and steel commun ( ecsc ) and the european econom commun ( eec ) , establish , respect , by the 1951 treati of pari and 1957 treati of rome . the origin member of what came to be known as the european commun were the inner six : belgium , franc , itali , luxembourg , the netherland , and west germani . the commun and their successor have grown in size by the access of new member state and in power by the addit of polici area to their remit . the unit kingdom becam the first member state to leav the EU on 31 januari 2020 . befor thi , three territori of member state had left the EU or it forerunn . the latest major amend to the constitut basi of the EU , the treati of lisbon , came into forc in 2009 .'*
```

*这是使用波特词干分析器对 *eu_definition* 文本进行词干分析的版本。这对于人类来说是不可能的，但是请记住，词干提取的主要目的是规范化文本，以提高机器学习算法的泛化能力。我们的词干版本包含 1430 个字符，不包括空格。我们可以通过以下方式获得:*

```
*len(''.join(porter_eu))*
```

***我们的原始句子包含 1591 个字符，**我们可以使用:*

```
*len(''.join(word_tokenize(eu_definition)))*
```

***信息保留的百分比是这些数字之间的比值(1430/1591)，约为 89.88%。***

***如果我们对 Snowball 和 Lancaster** 的相同文本应用相同的逻辑，我们会得到以下值:*

*   *雪球:90.88%*
*   *兰卡斯特:78.44%*

*不出所料，保留较少信息的词干分析器是兰卡斯特·斯特梅尔。*

*如果您想在项目中使用词干，这里有一个小要点和代码:*

*就是这样！词干是最常用的文本规范化技术之一，当您想要减少将在代码中使用的词汇(独特的单词)时，它可能适合您的自然语言管道。但是请记住，与任何规范化一样，每次应用词干都会丢失语料库中的一些原始信息。*

*尽管如此，词干分析对于数据科学家来说是一个很好的工具，因为它仍然广泛应用于整个 NLP 应用程序，并且可能对监督和非监督方法都有好处。*

> **感谢您花时间阅读这篇文章！随意在 LinkedIn 上加我(*[【https://www.linkedin.com/in/ivobernardo/】](https://www.linkedin.com/in/ivobernardo/)*)查看我公司网站(*[*https://daredata.engineering/home*](https://daredata.engineering/home)*)。**
> 
> *如果你有兴趣获得分析方面的培训，你也可以访问我在 Udemy([https://www.udemy.com/user/ivo-bernardo/](https://www.udemy.com/user/ivo-bernardo/))上的页面*

****这个例子摘自我在 Udemy 平台*** ***上为绝对初学者开设的*** [***NLP 课程——该课程适合初学者和希望学习自然语言处理基础知识的人。该课程还包含 50 多个编码练习，使您能够在学习新概念的同时进行练习。***](https://www.udemy.com/course/nlp_natural_language_processing_python_beginners/?couponCode=LEARN_NLP)*

*<https://ivopbernardo.medium.com/membership> *