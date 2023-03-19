# 替代分布语义方法

> 原文：<https://towardsdatascience.com/alternative-distributional-semantics-approach-e40fe1370c13?source=collection_archive---------22----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 解决歧义不再是歧义了

如果你在这里登陆，这意味着你有足够的好奇心去学习更多关于在 NLP/NLU 中解决歧义的不同方法。

背景信息是机器产生歧义的原因。这种模棱两可的信息产生于人类在交流中使用的自然语言。将这种语言“翻译”成一种全面的机器人工语言的过程可能会产生歧义。这可以用人类语言本身固有的非正式性和模糊性来解释。

传统的分布式语义方法是基于词向量化的地址语义。这里显示的替代方案是基于一个知识图，直接请求解决词汇歧义。

*本教程将强调一些我们可以用来解决这个问题的歧义消解任务，通过一个简单方便的应用程序，因此，一个自然语言 API (NL API)* 。

![](img/01b980d33c82744dd9bc9a2840f14aed.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[paweczerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral)拍摄的照片

机器本身既不能解释也不能理解文本；要做到这一点，并解决语言歧义，他们需要通过多层次的语言分析对文本进行注释。处理歧义的现象称为“歧义消除”。这是帮助机器检测文本中的意思(语义)的过程。考虑上下文、句法和单词关系来确定含义。

接下来的文章将强调可以用来帮助机器减少歧义和揭示文本理解的不同方法，如词汇化、词性标注等。

这项工作将基于自然语言 expert.ai NL API 的使用。

## Expert.ai NL API？

expert.ai 自然语言 API 是一个能够通过几行代码在文本中提供多层次信息的应用程序。为了构建 NLP 模块，API 提供了深入的语言理解。它显示了执行深层语言分析(标记化、词元化、词性标注、形态/句法/语义分析)的特征子集。最重要的是，该库允许解决命名实体识别(NER)、这些实体之间的语义关系和情感分析等问题。文档分类也可以通过现成的分类法来实现。

## Python 如何使用 Expert.ai NL API？

***安装库***

首先，您需要使用以下命令安装客户端库:

> pip 安装专家-nlapi

一旦你在[**developer . expert . ai**](https://developer.expert.ai/ui/)门户上创建了你的凭证，该 API 就可用了。Python 客户端代码希望您的开发人员帐户凭据被指定为环境变量:

*   **Linux:**

> 导出 EAI _ 用户名=您的用户
> 导出 EAI _ 密码=您的密码

*   **视窗**:

> SET EAI _ USERNAME = YOUR _ USER
> SET EAI _ PASSWORD = YOUR _ PASSWORD

您的用户是您在注册时指定的电子邮件地址。
您还可以在代码中定义凭证:

# 2 深入的语言分析

语言学把对语言的分析分成不同的部分。所有这些分支都是相互依赖的，一切都用语言联系在一起。
文档经过多级文本分析处理；每个文本被分割成句子，这些句子被解析成记号、词条和词类，找到句法成分和谓词之间的关系，并解释句法以建立完整的依存树。

要检索这些信息，首先要导入库的客户端部分:

让我们举一个例子来说明这些操作:

“索菲亚是一个由香港汉森机器人公司开发的社交人形机器人。索菲亚于 2016 年 2 月 14 日被激活。”

## a/文本细分

该操作允许将文本从最长的形式划分到最小的形式，在这种情况下，从段落级别开始，经过句子和短语，直到标记级别。当令牌是一个*搭配*(复合词)时，文本细分可以在分析中变得更深，直到*原子级别*为止，它不能被进一步划分。

一旦导入了库并实例化了客户端，就应该设置文本的语言和 API 的参数:

在 API 请求里面，你要提到 ***正文*** 里面要分析的句子和 ***params*** 里面的语言。**资源**参数与您需要对文本执行的操作相关，例如，这里的*消歧*基于 expert.ai NL API 提供的多级文本分析。

这种多层次的文本分析一般分为三个阶段:
1。一个**词法分析**:允许文本被分解成基本实体(记号)的文本细分阶段。
2。一个**句法分析:**在于识别构成句法实体的词位组合(包括词性标注)。
3。A **语义分析:**消歧发生在这个层面，它根据交际语境和实体之间可能的关系来检测这些实体的含义。

词法分析从第一个细分开始:

```
Paragraphs: Sophia is a social humanoid robot developed by Hong Kong-based company Hanson Robotics. Sophia was activated on February 14, 2016.
```

由于我们的文本已经是一个段落(这里是两个句子)，细分的输出提供了与输入相同的文本。让我们试着把段落分解到句子层面，在这种情况下，我们只需要修改元素**。段落**至**。句子**。最常见的句子定界方式是基于点**(。)**:

```
Sentences: Sophia is a social humanoid robot developed by Hong Kong-based company Hanson Robotics.
Sentences: Sophia was activated on February 14, 2016.
```

结果我们确实有两个句子。让我们更深入地细分检索短语级别。我们使用与上面相同的过程，修改元素**。带**的句子**。短语**:

```
Phrases: Sophia              
Phrases: is a social humanoid robot
Phrases: developed           
Phrases: by Hong Kong-based company Hanson Robotics
Phrases:.                   
Phrases: Sophia              
Phrases: was activated       
Phrases: on February 14, 2016
Phrases:.
```

我们注意到，一旦我们深入细分，我们会在结果中获得更多的元素。我们也可以得到文本中短语的数量:

```
phrases array size:  10
```

## b/标记化

此外，我们可以将短语级别分解成更小的单元，即记号。这是**“标记化”**任务，在 NLP 中很常见。它帮助机器理解文本。为了用 Python 执行标记化，我们可以使用**。【split()【功能如下图所示:**

例如，考虑这个句子:

“美国消费者新闻与商业频道评价了索菲亚栩栩如生的皮肤和她模仿 60 多种面部表情的能力。”

```
These are the tokens of the sentence ['CNBC', 'has', 'commented', 'on', 'the', "robot's", 'lifelike', 'skin', 'and', 'her', 'ability', 'to', 'emulate', 'more', 'than', '60', 'facial', 'expressions.']
```

如果不在 split()中指定分隔符，文本将根据空格分隔。使用 expert.ai NL API，我们也可以执行标记化，并具有附加功能。换句话说，API 提供了不同的单词级令牌分析；API 产生的标记可以是单词、字符(如缩写)甚至标点符号。
让我们看看如何用 API 执行这个任务，我们使用与上面相同的过程，修改元素**。短语**与**。代币**:

```
TOKEN                
----                
CNBC                
has                 
commented           
on                  
the                 
robot               
's                  
lifelike            
skin                
and                 
her                 
ability             
to                  
emulate             
more                
than                
60                  
facial expressions  
.
```

我们注意到这些记号要么是像**皮肤、能力、仿效**这样的单词，要么是像**的**这样的缩写，要么是数字: **60** ，甚至是像**点这样的标点(。)**。

标记化会产生搭配，就像使用 split()函数无法实现的面部表情一样。API 能够根据单词的位置和上下文来检测句子中的复合词。这种搭配可以进一步细分到原子级别，这是我们可以拥有的最后一个小词汇单位:

```
CNBC                
has                 
commented           
on                  
the                 
robot               
's                  
lifelike            
skin                
and                 
her                 
ability             
to                  
emulate             
more                
than                
60                  
facial expressions  
	 atom: facial              
	 atom: expressions         
.
```

## c/ PoS 标签

标记化导致 NLP 中的第二个过程，即**词性标注**(词性标注)，它们一起工作，以便让机器检测文本的意思。在这一阶段，我们介绍包括词性标注任务的句法分析。后者包括给每个单词分配一个词性或语法类别。词性表征了每个词素的形态句法性质。这些归属于文本元素的标签可以反映文本的一部分意义。我们可以列出英语中常用的几个词类:**限定词**、**名词、副词、动词、形容词、介词、连词、代词、感叹词** …

同形异义词[(同形异义词)](https://en.wikipedia.org/wiki/Homograph)可以有不同的含义[(多义词)](https://en.wikipedia.org/wiki/Polysemy)。这个单词可以有不同的词性，即使它有相同的形式。语法类别取决于单词在文本中的位置及其上下文。让我们来考虑这两句话:

> 这项活动的目的是为慈善机构筹款。很多人将**反对**书。

从语言学的角度来看，在第一句中，*宾语*是名词，而在第二句中，*宾语*是动词。词性标注是消除歧义的关键步骤。根据这种标记，从上下文、从单词的形式(例如，专有名词开头的大写字母)、从位置(SVO 词序)等推断单词的意思。因此，单词之间产生语义关系；根据关系的类型将每个概念相互联系起来，构建一个知识图。
让我们尝试使用我们的 API 为前面两个句子生成词性标记:

我们首先导入库并创建客户端，如下所示:

我们必须声明与每个句子相关的变量，**宾语 _ 名词**用于单词*宾语*是名词的句子，而**宾语 _ 动词**用于带有动词的句子:

单词**宾语**在两个句子中具有相同的形式，但是具有不同的词性。以便向专家展示。Ai NL API，我们需要调用后者。

首先，我们指定要进行词性标注的文本，对于第一个句子，它是宾语名词，对于第二个句子，它是宾语动词。然后是例子的语言，最后是资源；这与所执行的分析相关，在这种情况下是歧义消除，如下所示:

一旦我们设置了这些参数，就需要对标记进行一次迭代，以便为每个示例分别分配一个 POS

```
 **Output of the first sentence :** 

**TOKEN                POS** 
The                  DET   
object               NOUN  
of                   ADP   
this                 DET   
exercise             NOUN  
is                   VERB  
to                   PART  
raise                VERB  
money                NOUN  
for                  ADP   
the                  DET   
charity              NOUN  
.                    PUNCT 
	 **Output of the second sentence : ** 

**TOKEN                POS** 
A lot of             ADJ   
people               NOUN  
will                 AUX   
object               VERB  
to                   ADP   
the                  DET   
book                 NOUN  
.                    PUNCT
```

一方面，**宾语**确实是名词，前面加了冠词/限定词【DET】*。另一方面，单词*宾语*实际上是连接主语“很多人”和宾语“书”的动词。*

*自然语言处理中使用的传统词性标注工具通常使用相同类型的信息来标注文本中的单词:上下文和词法。专家系统中词性标注的特殊功能。Ai NL API 不仅仅是为每一个 token 标识一个语法标签，更是引入了意义。*

*换句话说，一个词可以和其他词有相同的形式，但它包含几个意思(一词多义)。每个意思都在一个概念中传达，与其他概念相联系，创建一个知识图。上面看到的单词 *object* 有不止一个含义，因此，它属于不同语义的**概念**，我们在 expert.ai NL API 的知识图中称之为 **"Syncons"** 。词性标注可以揭示同一个词的不同标签，从而产生不同的含义。这就是我们可以用 API 检查的内容:*

```
 ***Concept_ID for object when NOUN** 

**TOKEN                POS             ID** 
The                  DET                 -1 
object               NOUN             26946 
of                   ADP                 -1 
this                 DET                 -1 
exercise             NOUN             32738 
is                   VERB             64155 
to                   PART                -1 
raise                VERB             63426 
money                NOUN             54994 
for                  ADP                 -1 
the                  DET                 -1 
charity              NOUN              4217 
.                    PUNCT               -1 
	 **Concept_ID for object when VERB ** 

**TOKEN                POS             ID** 
A lot of             ADJ              83474 
people               NOUN             35459 
will                 AUX                 -1 
object               VERB             65789 
to                   ADP                 -1 
the                  DET                 -1 
book                 NOUN             13210 
.                    PUNCT               -1*
```

*可以注意到，名词*对象*属于 ID 为 **26946** 的概念。这一概念包括其他具有相同含义的词[(同义词)](https://en.wikipedia.org/wiki/Synonym)。相比之下，它在第二句中的同形异义词与 ID **65789 相关。**这些 ID 是知识图中每个概念的标识。*

> *因此，不同的词性导致不同的意思，即使我们有相同的词形。*

****请注意*****以-1 为 ID 的词如 ADP (Adposition 指介词和后置)、PUNCT(表示标点)、*限定词)*等，在知识图中是不存在的，因为它们本身没有语义。***

## **d/词汇化**

**这是自然语言处理中的另一个核心任务，称为词汇化。除了标记化和词性标注，这是执行信息提取和文本规范化的重要步骤。对于观点挖掘和情感检测特别有用，词条允许在文档中出现主要的语义趋势。**

**词汇化是一种将某些标记组合在一起的语言资源。简而言之，它将每个标记与字典中代表它的标准形式相关联:**

*   **动词的不定式:wear，wear->wear/ran，running，runs - > run**
*   ****的单数形式**为名词:mouse->mouse/die->dice**
*   **等等。…**

**这个概念(或 Syncon)可以包含许多引理 [(lexemes)](https://en.wikipedia.org/wiki/Lexeme) 。在消除歧义的过程中，文本中识别的每个标记都返回到其基本形式，去除屈折词缀。每个引理与知识图中的一个概念相关联。因此，引理化使得能够将不同记号的集合减少到不同引理的集合。这可以通过这个例子来说明；**

**起初，一个词位“ **living** ”的听者几乎是无意识地就能辨别出这个词的意思。这对于人类来说是可能的，通过基于世界的知识等进行推论。如果上下文不存在，这对于机器来说是不可能的。**

**对于机器来说，预测一个单词的几个意思，具有相同的拼写和相同的发音，词汇化是处理词汇歧义的关键解决方案。**

**我们可以用 expert.ai NL API 来执行这个任务。让我们考虑这两个例子:**

> **她正在过着她最美好的生活。你靠什么为生**？****

```
 ******Output of the first sentence :** 

**TOKEN                LEMMA           POS** 
She                  she             PRON   
's                   's              AUX    
living               live            VERB   
her                  her             PRON   
best                 good            ADJ    
life                 life            NOUN   
	 **Output of the second sentence : ** 

**TOKEN                LEMMA           POS** 
What                 what            PRON   
do                   do              AUX    
you                  you             PRON   
do                   do              VERB   
for                  for             ADP    
a                    a               DET    
living               living          NOUN   
?                    ?               PUNCT****
```

****如上所述，“living”属于两个不同的引理，取决于上下文及其在句子中的位置。在第一个例子中， **living** 对应于引理**“live”**，它是句子的动词。相反，第二个句子中的 **living** 是一个名词，有一个引理**“living”**。意思也不一样，第一个引理描述的是*【生存】*的概念，然而，作为名词的生存属于*【收入或挣钱的手段】*的概念。****

****因此，词汇化有助于机器推断同形异义词的意思。****

# ****结论****

****一个表达或单词可能有一个以上的意思，因此，机器的语言理解存在问题。这要归功于非常基本的自然语言处理任务，比如词汇化、词性标注等。，以及几行代码，我们可以解决这种模糊性，这就是我在本文中想要分享的内容。****

****希望解决歧义现在不那么模糊了…****