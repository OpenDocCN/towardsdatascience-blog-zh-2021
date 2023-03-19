# 如何使用来自拥抱脸变压器库的伯特

> 原文：<https://towardsdatascience.com/how-to-use-bert-from-the-hugging-face-transformer-library-d373a22b0209?source=collection_archive---------2----------------------->

## 如何使用来自拥抱脸变形库的 BERT 完成四项重要任务

![](img/a59921fdae3ba0c168bebba22bc223e2.png)

eberhard grossgasteiger 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在这篇文章中，我将演示如何使用 BERT 的拥抱面部变形库来完成四个重要的任务。我还将向您展示如何配置 BERT 来执行您可能希望使用它的任何任务，除了它旨在解决的标准任务之外。

请注意，本文写于 2021 年 1 月，因此拥抱脸库的早期/未来版本可能会略有不同，本文中的代码可能不一定有效。

## 伯特建筑的快速回顾

BERT 是一个双向转换器，使用屏蔽语言建模和下一句预测的组合进行预训练。BERT 的核心部分是来自 transformer 模型的堆叠双向编码器，但在预训练期间，在 BERT 上添加了屏蔽语言建模和下一句预测头。当我说“头”时，我的意思是在 BERT 上添加几个额外的层，可以用来生成特定的输出。BERT 的原始输出是来自堆叠双向编码器的输出。这个事实特别重要，因为它允许您使用 BERT 做任何事情，您将在本文后面看到这样的例子。

BERT 可以解决拥抱脸提供的许多任务，但我将在本文中讨论的是掩蔽语言建模、下一句预测、语言建模和问题回答。我还将演示如何配置 BERT 来完成除上述任务和拥抱脸提供的任务之外的任何任务。

在讨论这些任务之前，我将描述如何使用 BERT 记号赋予器。

## 伯特记号赋予器

BERT 记号赋予器是一个与 BERT 一起工作的记号赋予器。对于任何类型的令牌化任务，它都有许多功能。您可以使用这行代码下载令牌化器:

```
from transformers import BertTokenizertokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
```

与 BERT 模型不同，您不必为每个不同类型的模型下载不同的标记器。您可以对 hugging face 提供的各种 BERT 模型使用相同的标记器。

给定一个文本输入，下面是我在项目中通常如何标记它:

```
encoding = tokenizer.encode_plus(text, add_special_tokens = True,    truncation = True, padding = "max_length", return_attention_mask = True, return_tensors = "pt")
```

由于 BERT 一次只能接受 512 个令牌作为输入，因此我们必须将截断参数指定为 True。add special tokens 参数只是让 BERT 添加标记，如开始、结束、[SEP]和[CLS]标记。Return_tensors = "pt "只是让 tokenizer 返回 PyTorch 张量。如果您不希望这种情况发生(也许您希望它返回一个列表)，那么您可以移除该参数，它将返回列表。

在下面的代码中，你会看到我没有添加上面列出的所有参数，这主要是因为这是不必要的，因为我没有对真实项目的文本进行标记。在一个真实的机器学习/NLP 项目中，您会想要添加这些参数，尤其是截断和填充，因为我们必须在一个真实的项目中对数据集中的每个批次进行此操作。

tokenizer.encode_plus()专门返回值的字典，而不仅仅是值的列表。因为 tokenizer.encode_plus()可以返回许多不同类型的信息，如 attention_masks 和令牌类型 id，所以所有内容都以字典格式返回，如果您想检索编码的特定部分，可以这样做:

```
input = encoding["input_ids"][0]
attention_mask = encoding["attention_mask"][0]
```

此外，因为记号赋予器返回一个不同值的字典，而不是像上面显示的那样找到这些值，并分别将它们传递到模型中，我们可以像这样传递整个编码

```
output = model(**encoding) 
```

关于记号赋予器，需要知道的另一件非常重要的事情是，如果需要，您可以指定检索特定的记号。例如，如果您正在进行掩码语言建模，并且您想要在模型解码的位置插入一个掩码，那么您可以像这样简单地检索掩码标记

```
mask_token = tokenizer.mask_token
```

您可以通过将它与您的输入文本连接起来，简单地将其插入到您的输入中。

您还可以用同样的方式检索许多其他标记，比如[SEP]标记。

我通常使用 tokenizer.encode_plus()函数来标记我的输入，但是还有另一个函数可以用来标记输入，这个函数就是 tokenizer.encode()。这是一个例子:

```
encoding = tokenizer.encode(text, return_tensors = "pt")
```

tokenizer.encode_plus()和 tokenizer.encode()的主要区别在于 tokenizer.encode_plus()返回更多信息。具体来说，它返回实际的输入 id、注意掩码和令牌类型 id，并在一个字典中返回所有这些内容。tokenizer.encode()只返回输入 id，并根据参数 return_tensors = "pt "以列表或张量的形式返回。

## 掩蔽语言建模

屏蔽语言建模是对句子中的屏蔽标记进行解码的任务。简单来说就是填空的任务。

我将演示如何获取掩码标记的前 10 个替换单词，而不是仅获取最佳候选单词来替换掩码标记，下面是您可以如何做:

```
from transformers import BertTokenizer, BertForMaskedLM
from torch.nn import functional as F
import torchtokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertForMaskedLM.from_pretrained('bert-base-uncased',    return_dict = True)text = "The capital of France, " + tokenizer.mask_token + ", contains the Eiffel Tower."input = tokenizer.encode_plus(text, return_tensors = "pt")
mask_index = torch.where(input["input_ids"][0] == tokenizer.mask_token_id)
output = model(**input)
logits = output.logits
softmax = F.softmax(logits, dim = -1)
mask_word = softmax[0, mask_index, :]
top_10 = torch.topk(mask_word, 10, dim = 1)[1][0]
for token in top_10:
   word = tokenizer.decode([token])
   new_sentence = text.replace(tokenizer.mask_token, word)
   print(new_sentence)
```

拥抱脸是这样设置的，对于它有预训练模型的任务，你必须下载/导入那个特定的模型。在这种情况下，我们必须下载用于屏蔽语言建模模型的 Bert，而正如我在上一节中所说的，标记器对于所有不同的模型都是相同的。

屏蔽语言建模的工作方式是，在您希望预测最佳候选单词的位置插入一个屏蔽标记。您可以简单地插入掩码标记，就像我上面所做的那样，将它连接到输入中所需的位置。用于屏蔽语言建模的 Bert 模型预测其词汇表中替换该单词的最佳单词/记号。在 softmax 激活函数应用于 BERT 的输出之前，logits 是 BERT 模型的输出。为了获得 logits，我们必须在初始化模型时在参数中指定 return_dict = True，否则，上面的代码将导致编译错误。在我们将输入编码传递到 BERT 模型中之后，我们可以通过指定 output.logits 来获得 logit，这将返回一个张量，然后我们可以最终将 softmax 激活函数应用到 logit。通过对 BERT 的输出应用 softmax，我们得到了 BERT 词汇表中每个单词的概率分布。具有较高概率值的单词将是掩码标记的更好的候选替换单词。为了获得 BERT 词汇表中所有单词的 softmax 值的张量以替换屏蔽令牌，我们可以指定屏蔽令牌索引，这是使用 torch.where()获得的。因为在这个特定的例子中，我正在检索掩码标记的前 10 个候选替换单词(通过相应地调整参数，可以得到 10 个以上)，所以我使用了 torch.topk()函数，它允许您检索给定张量中的前 k 个值，并且它返回包含这些前 k 个值的张量。在此之后，过程变得相对简单，因为我们所要做的就是迭代张量，并用候选标记替换句子中的掩码标记。下面是上面代码编译的输出:

```
The capital of France, paris, contains the Eiffel Tower. 
The capital of France, lyon, contains the Eiffel Tower. 
The capital of France, lille, contains the Eiffel Tower. 
The capital of France, toulouse, contains the Eiffel Tower. 
The capital of France, marseille, contains the Eiffel Tower. 
The capital of France, orleans, contains the Eiffel Tower. 
The capital of France, strasbourg, contains the Eiffel Tower. 
The capital of France, nice, contains the Eiffel Tower. 
The capital of France, cannes, contains the Eiffel Tower. 
The capital of France, versailles, contains the Eiffel Tower.
```

您可以看到 Paris 确实是 mask token 的首选替换单词。

如果您只想获得第一个候选单词，可以这样做:

```
from transformers import BertTokenizer, BertForMaskedLM
from torch.nn import functional as F
import torch
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertForMaskedLM.from_pretrained('bert-base-uncased',    return_dict = True)
text = "The capital of France, " + tokenizer.mask_token + ",
contains the Eiffel Tower."
input = tokenizer.encode_plus(text, return_tensors = "pt")
mask_index = torch.where(input["input_ids"][0] == tokenizer.mask_token_id)
logits = model(**input)
logits = logits.logits
softmax = F.softmax(logits, dim = -1)
mask_word = softmax[0, mask_index, :]
top_word = torch.argmax(mask_word, dim=1)
print(tokenizer.decode(top_word))
```

我们不使用 torch.topk()来检索前 10 个值，而是使用 torch.argmax()，它返回张量中最大值的索引。代码的其余部分与原始代码基本相同。

## 语言建模

语言建模的任务是在给定句子中所有单词的情况下，预测跟随或继续句子的最佳单词。

```
import transformersfrom transformers import BertTokenizer, BertLMHeadModel
import torch
from torch.nn import functional as F
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertLMHeadModel.from_pretrained('bert-base-uncased',
return_dict=True, is_decoder = True)
text = "A knife is very "
input = tokenizer.encode_plus(text, return_tensors = "pt")
output = model(**input).logits[:, -1, :]
softmax = F.softmax(output, -1)
index = torch.argmax(softmax, dim = -1)
x = tokenizer.decode(index)
print(x)
```

语言建模的工作方式与屏蔽语言建模非常相似。首先，我们必须下载特定的 BERT 语言模型 Head Model，它本质上是一个 Bert 模型，上面有一个语言建模 Head。在实例化该模型时，我们必须指定的一个额外参数是 is_decoder = True 参数。如果我们想要使用这个模型作为预测序列中下一个最佳单词的独立模型，我们必须指定这个参数。代码的其余部分与屏蔽语言建模中的代码相对相同:我们必须检索模型的逻辑，但我们不必指定屏蔽标记的索引，我们只需获取模型最后一个隐藏状态的逻辑(使用-1 索引)，计算这些逻辑的 softmax，找到词汇表中的最大概率值，解码并打印这个标记。

## 下一句预测

下一句预测是预测一个句子是否跟随另一个句子的任务。这是我的代码:

```
from transformers import BertTokenizer, BertForNextSentencePrediction
import torch
from torch.nn import functional as Ftokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertForNextSentencePrediction.from_pretrained('bert-base-uncased')prompt = "The child came home from school."next_sentence = "He played soccer after school."encoding = tokenizer.encode_plus(prompt, next_sentence, return_tensors='pt')
outputs = model(**encoding)[0]
softmax = F.softmax(outputs, dim = 1)
print(softmax)
```

下一句预测是预测一个句子对给定句子的下一句有多好的任务。在这种情况下，“孩子放学回家。”是给定的句子，我们试图预测“他放学后是否踢足球。”就是下一句。为此，BERT tokenizer 自动在句子之间插入一个[SEP]标记，表示两个句子之间的分隔，特定的 BERT For Next sense 预测模型预测该句子是否为下一句的两个值。Bert 在一个张量中返回两个值:第一个值表示第二个句子是否是第一个句子的延续，第二个值表示第二个句子是否是随机序列或者不是第一个句子的良好延续。与语言建模不同，我们不检索任何逻辑，因为我们不试图在 BERT 的词汇表上计算 softmax 我们只是试图计算下一句预测的 BERT 返回的两个值的 softmax，这样我们就可以看到哪个值具有最高的概率值，这将代表第二句话是否是第一句话的下一句话。一旦我们得到了 softmax 值，我们就可以简单地通过打印出来来查看张量。以下是我得到的值:

```
tensor([[0.9953, 0.0047]])
```

因为第一个值大大高于第二个指数，所以 BERT 认为第二句话跟在第一句话后面，这就是正确答案。

## 抽取式问题回答

抽取式问题回答是在给定一些上下文文本的情况下，通过输出答案在上下文中所处位置的开始和结束索引来回答问题的任务。以下是我回答问题的代码:

```
from transformers import BertTokenizer, BertForQuestionAnswering
import torch
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertForQuestionAnswering.from_pretrained('bert-base-
uncased')
question = "What is the capital of France?"
text = "The capital of France is Paris."
inputs = tokenizer.encode_plus(question, text, return_tensors='pt')
start, end = model(**inputs)
start_max = torch.argmax(F.softmax(start, dim = -1))
end_max = torch.argmax(F.softmax(end, dim = -1)) + 1 ## add one ##because of python list indexing
answer = tokenizer.decode(inputs["input_ids"][0][start_max : end_max])
print(answer)
```

与其他三个任务类似，我们从下载特定的用于问题回答的 BERT 模型开始，我们将两个输入标记化:问题和上下文。与其他模型不同，该模型的过程相对简单，因为它输出标记化输入中每个单词的值。正如我之前提到的，抽取式问题回答的工作方式是通过计算答案在上下文中所处位置的最佳开始和结束索引。该模型返回上下文/输入中所有单词的值，这些值对应于它们对于给定问题的起始值和结束值有多好；换句话说，输入中的每个单词接收表示它们是答案的好的开始单词还是答案的好的结束单词的开始和结束索引分数/值。这个过程的其余部分与我们在其他三个程序中所做的非常相似；我们计算这些分数的 softmax 以找到值的概率分布，使用 torch.argmax()检索开始和结束张量的最高值，并在输入中找到对应于这个 start : end 范围的实际记号，解码它们并打印出来。

## 使用 BERT 完成您想要的任何任务

尽管文本摘要、问题回答和基本语言模型特别重要，但人们通常希望使用 BERT 来完成其他不确定的任务，尤其是在研究中。他们的方法是获取 BERT 堆叠编码器的原始输出，并附加自己的特定模型，通常是一个线性层，然后在特定数据集上微调该模型。当在 Pytorch 中使用 Hugging Face transformer 库执行此操作时，最好将其设置为 Pytorch 深度学习模型，如下所示:

```
from transformers import BertModel
class Bert_Model(nn.Module):
   def __init__(self, class):
       super(Bert_Model, self).__init__()
       self.bert = BertModel.from_pretrained('bert-base-uncased')
       self.out = nn.Linear(self.bert.config.hidden_size, classes)
   def forward(self, input):
       _, output = self.bert(**input)
       out = self.out(output)
       return out
```

如您所见，我没有下载已经为特定任务(如问答)设计的特定 BERT 模型，而是下载了原始的预训练 BERT Model，它没有附带任何头部。

要获得原始 BERT 输出的大小，只需使用 self.bert.config.hidden_size，并将它附加到您希望线性图层输出的类的数量。

要使用上面的代码进行情感分析，这是一个令人惊讶的任务，没有下载/已经在拥抱脸变压器库中完成，您可以简单地在线性层的末尾添加一个 sigmoid 激活函数，并将类指定为等于 1。

```
from transformers import BertModel
class Bert_Model(nn.Module):
   def __init__(self, class):
       super(Bert_Model, self).__init__()
       self.bert = BertModel.from_pretrained('bert-base-uncased')
       self.out = nn.Linear(self.bert.config.hidden_size, classes)
       self.sigmoid = nn.Sigmoid() def forward(self, input, attention_mask):
       _, output = self.bert(input, attention_mask = attention_mask)
       out = self.sigmoid(self.out(output))
       return out
```

我希望您觉得这些内容很容易理解。如果你认为我需要进一步阐述或澄清什么，请在下面留言。

## 参考

[拥抱变脸库](https://huggingface.co/transformers/index.html)