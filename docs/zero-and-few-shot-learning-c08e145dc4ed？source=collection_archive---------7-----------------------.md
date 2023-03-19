# 零投和少投学习

> 原文：<https://towardsdatascience.com/zero-and-few-shot-learning-c08e145dc4ed?source=collection_archive---------7----------------------->

## 使用 FlairNLP 和 Huggingface 的低资源印尼语示例！

![](img/b9e9ead845c4aba94d3c46da1f19131f.png)

照片由 [Abdullah Ahmad](https://unsplash.com/@thefinalshot?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

NLP 领域每天都变得越来越令人兴奋。直到几年前，我们还不能充分利用网上的大量数据来源。随着无监督学习方法和迁移学习的惊人成功，NLP 社区已经建立了作为多个 NLP 任务的知识库的模型。

然而，我们仍然依赖带注释的数据对下游任务进行微调。通常情况下，获取带标签的数据并不容易，而且是一项相对昂贵且耗时的工作。如果我们没有任何标记的数据或者数据非常少，我们能做什么？这个问题的答案是零炮和少炮学习。

零炮和少炮方法没有单一的定义。相反，可以说它的定义是任务相关的。

*零镜头分类*是指我们在一些类上训练一个模型，并对一个新的类进行预测，这个模型以前从未见过。显然，类名需要存在于类的列表中，但是这个类没有训练样本。

> 在更广泛的意义上，零镜头设置指的是使用一个模型去做一些它没有被训练去做的事情。

让我们假设我们在一个大的文本语料库上训练一个语言模型(或者使用一个像 GPT-2 这样预先训练好的语言模型)。我们的任务是预测一篇给定的文章是关于体育、娱乐还是科技。通常情况下，我们会将此作为一个带有许多标记示例的微调任务，并在语言模型之上添加一个用于分类的线性层。但是对于 zero shot，我们直接使用语言模型(没有任何显式的微调)。我们可以给出文章以及解释性标签名称，并得到一个预测。

现在听起来还很困惑吗？不要烦恼！并深入这些概念背后的直觉，让事情变得更清晰。此外，最后的例子将确保对其可用性有更深的理解。

# 零投和少投学习背后的直觉

作为人类，我们储存了大量的信息，我们从每一个资源中学习，无论是书籍，新闻，课程，还是仅仅是经验。

如果我们被要求做以下任务:
*“从英语翻译成法语”:*你好吗？- >？

从任务的描述中，我们很清楚这里要做什么。我们已经使用我们的知识库来推断翻译的意思。

另一个任务可以如下:
“我爱这部电影！”- >快乐还是悲伤

阅读自我解释的任务解释(快乐或悲伤)，我们明白这是一个分类任务。我们的知识库也帮助我们理解句子并推断它是快乐的！

这正是零射击分类的工作原理。我们有一个预先训练好的模型(如语言模型)作为知识库，因为它已经在许多网站的大量文本上进行了训练。对于任何类型的任务，我们给出相关的类描述符，并让模型推断任务实际上是什么。

不用说，我们提供的标签数据越多，结果就越好。而且很多时候零拍都不太管用。如果我们有一些标签数据的样本，但不足以进行微调，那么可以选择少量拍摄。正如在 GPT-3 中所使用的，“语言模型很少使用学习者”，作者证明了与较小的模型相比，非常大的语言模型可以用更少的标记数据在下游任务中有竞争力地执行。

少数镜头只是零镜头的扩展，但用几个例子来进一步训练模型。

# 拍打和拥抱脸来救援！

FlairNLP 和 Huggingface 都有针对英语的零镜头分类管道(因为它们使用 bert 作为模型)。尽管 flairNLP 使用 bert-base-uncased for english 作为它的基本模型，但它对于简单的印度尼西亚文本却出奇地好用。在下面的例子中，我将带你在印度尼西亚文本上使用 flairNLP 中的 [TARS 模型完成零镜头和少镜头学习的步骤。](https://github.com/flairNLP/flair/blob/master/resources/docs/TUTORIAL_10_TRAINING_ZERO_SHOT_MODEL.md)

huggingface 实现的零镜头分类管道有一些很优秀的文章和演示。看看这个优秀的[博客](https://joeddav.github.io/blog/2020/05/29/ZSL.html)和这个[现场演示](https://huggingface.co/zero-shot/)关于拥抱脸的零镜头分类。

由于我使用的是印尼语，所以我不会使用 huggingface 零射击管道，而是使用 GPT-2 模型(由 [cahya](https://huggingface.co/cahya/gpt2-small-indonesian-522M) 在印尼语上培训，并在 huggingface 上可用)并实现零射击分类。

# 例子

## FlairNLP

使用 zero shot 分类器( [TARS](https://kishaloyhalder.github.io/pdfs/tars_coling2020.pdf) )模型来分类句子是关于体育(olahraga)还是政治(politik)

```
from flair.models.text_classification_model import TARSClassifier
from flair.data import Sentence# 1\. Load our pre-trained TARS model for English
tars = TARSClassifier.load('tars-base')# 2\. Prepare a test sentence
sentence1 = Sentence("Inggris mengalahkan Australia dengan 4 gawang")
sentence2 = Sentence("Para menteri kabinet memainkan permainan yang tidak menyenangkan")# 3\. Define some classes that you want to predict using descriptive names
classes = ["olahraga","politik"]#4\. Predict for these classes
tars.predict_zero_shot(sentence1, classes, multi_label=False)
tars.predict_zero_shot(sentence1, classes, multi_label=False)# Print sentence with predicted labels
print(sentence1)
print(sentence2)
```

输出

```
Sentence: "Inggris mengalahkan Australia dengan 4 gawang"   [− Tokens: 6  − Sentence-Labels: {'label': [olahraga (0.6423)]}]
Sentence: "Para menteri kabinet memainkan permainan yang tidak menyenangkan"   [− Tokens: 8  − Sentence-Labels: {'label': [olahraga (0.0951)]}]
```

英语模式在印度尼西亚语上出奇地好用！虽然它错误地预测了第二个例子，但是“内阁大臣们正在玩一场肮脏的游戏”这句话已经被归类为体育而不是政治。大概是因为*这个词打*混淆了型号吧！让我们用 4 个例子(每个班 2 个)让它学习，并使用少数镜头分类对同一个例子进行重新评估。

```
from flair.data import Corpus
from flair.datasets import SentenceDataset# training dataset consisting of four sentences (2 labeled as "olahraga" (sports) and 2 labeled as "politik" (politics)
train = SentenceDataset(
    [
        Sentence('Pemilu akan dimulai bulan depan').add_label('olahraga_atau_politik', 'politik'),
        Sentence('Perdana menteri mengumumkan dana bantuan banjir besar-besaran').add_label('olahraga_atau_politik', 'politik'),
        Sentence('Olimpiade akan diadakan di Tokyo').add_label('olahraga_atau_politik', 'olahraga'),
        Sentence('Asian Games 2018 merupakan ajang multi sport yang diadakan di Jakarta').add_label('olahraga_atau_politik', 'olahraga')
    ])# test dataset consisting of two sentences (1 labeled as "olahraga" and 1 labeled as "politik")
test = SentenceDataset(
    [
        Sentence('Para menteri kabinet memainkan permainan yang tidak menyenangkan').add_label('olahraga_atau_politik', 'politik'),
        Sentence('Ayo pergi dan bermain basket').add_label('olahraga_atau_politik', 'olahraga')
    ])# make a corpus with train and test split
corpus = Corpus(train=train, test=test)
```

根据新数据训练模型

```
from flair.trainers import ModelTrainer# 1\. load base TARS
tars = TARSClassifier.load('tars-base')# 2\. make the model aware of the desired set of labels from the new corpus
tars.add_and_switch_to_new_task("OLAHRAGA_POLITIK", label_dictionary=corpus.make_label_dictionary())# 3\. initialize the text classifier trainer with your corpus
trainer = ModelTrainer(tars, corpus)# 4\. train model
trainer.train(base_path='resources/taggers/olahraga_politik', # path to store the model artifacts
              learning_rate=0.02, 
              mini_batch_size=1, 
              max_epochs=10, 
              )# 5\. Load few-shot TARS model
tars = TARSClassifier.load('resources/taggers/olahraga_politik/final-model.pt')# 6\. Prepare a test sentence
sentence = Sentence("Para menteri kabinet memainkan permainan yang tidak menyenangkan")# 7\. Predict for olahraga and politik
tars.predict(sentence)
print(sentence)
```

输出

```
Sentence: "Para menteri kabinet memainkan permainan yang tidak menyenangkan"   [− Tokens: 8  − Sentence-Labels: {'label': [politik (0.9968)]}]
```

## 使用在印度尼西亚语上训练的 GPT-2 进行零射击分类

```
from torch.nn import functional as F
from transformers import GPT2Tokenizer, GPT2Model# 1\. Load pertained GPT-2 model and tokenizer
model_name='cahya/gpt2-small-indonesian-522M'
tokenizer = GPT2Tokenizer.from_pretrained(model_name)
model = GPT2Model.from_pretrained(model_name)# 2\. Prepare a test sentence and labels
sentence = 'Para menteri kabinet memainkan permainan yang tidak menyenangkan'
labels = ['olahraga', 'politik']# 3\. Since there is no padding token in this tokenizer, add a token. # A separate pad token can be added using the add_special_token
# function
tokenizer.pad_token = tokenizer.eos_token# 4\. Concatenate sentence with lables
inputs = tokenizer([sentence]+labels, return_tensors='pt',padding='longest')input_ids = inputs['input_ids']
attention_mask = inputs['attention_mask']
output = model(input_ids, attention_mask=attention_mask)[0]
sentence_rep = output[:1].mean(dim=1)
label_reps = output[1:].mean(dim=1)# now find the labels with the highest cosine similarities to
# the sentence
similarities = F.cosine_similarity(sentence_rep, label_reps)
closest = similarities.argsort(descending=True)
for ind in closest:
    print(f'label: {labels[ind]} \t similarity: {similarities[ind]}')
```

输出

```
label: politik 	 similarity: 0.5492470860481262
label: olahraga 	 similarity: 0.48411038517951965
```

# 结论

零炮和少炮学习方法正在减少对注释数据的依赖。GPT-2 和 GPT-3 模型已经显示出显著的结果来证明这一点。然而，对于像印度尼西亚语这样的低资源语言，它仍然是一个活跃的研究领域。然而，感谢像 [cahya](https://huggingface.co/cahya/gpt2-small-indonesian-522M) 这样的宝贵资源，我们有了专门为印尼人训练的 GPT-2 模型。然而，由于印度尼西亚语的数据并不庞大，因此少击比零击学习更受欢迎。尽管如此，我们只用几个例子就能取得的成就绝对是了不起的，flairNLP 和 huggingface 这样的库是如何让实现前沿模型变得如此容易的！

干杯！

Eram