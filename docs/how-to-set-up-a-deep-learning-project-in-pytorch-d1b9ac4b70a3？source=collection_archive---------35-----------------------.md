# 如何在 Pytorch 中建立深度学习项目

> 原文：<https://towardsdatascience.com/how-to-set-up-a-deep-learning-project-in-pytorch-d1b9ac4b70a3?source=collection_archive---------35----------------------->

## 在 Pytorch 中建立深度学习项目的 3 个步骤

![](img/c59f8325222db2588365b6d2c9450a3b.png)

Graham Holtshausen 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这篇文章中，我将解释如何在 PyTorch 中建立一个深度学习项目。

任何 PyTorch 深度学习项目通常由 3 个基本步骤组成:

1.  设置数据集
2.  创建数据加载器
3.  创建培训、验证和测试循环

我不会讲述如何建立一个实际的模型，因为它相当简单，因任务而异，而且网上已经有很多资源可以学习如何做这件事。

## 1.设置数据集

为了创建可以与数据加载器一起工作的数据集，以创建可以发送到实际模型中的数据批，PyTorch 要求您为数据集创建一个特定的类，并覆盖它的两个函数:__getitem__()函数和 __len__()函数。

在大多数机器学习项目中，在数据实际发送到模型之前，通常需要对数据进行一些预处理。例如，在自然语言处理任务中，这可能涉及对文本进行标记并将其转换成数字格式。为此，我在同一个类中创建可以完成所有这些过程的函数，如 build_dataset()或 preprocess()函数，并将所有预处理数据存储在一个单独的文件中。

getitem 和 len 函数只是数据加载器用来构建小批量数据的函数。在下一节中，您将看到数据加载器如何使用和需要一个数据集对象/类，该数据集对象/类作为参数正确地覆盖了 getitem 和 len 函数，因此正确地实现这两个函数非常重要。

getitem 函数相当简单。想象您的数据集正在被索引。例如，如果您正在执行一项图像识别任务，并且正在处理图像，则每个图像都将从 0 到数据集大小/数据集中的图像数量进行索引。对于 getitem 函数，您所要做的就是，给定一个索引，返回存在于该特定索引的数据的 x，y 对(或输入输出对)。因此，重要的是要记住，您可能应该将数据存储在索引数据集中，如列表中，这样您就可以轻松地访问特定索引处的元素。len 函数只是一个返回数据集长度的函数。以前面将数据存储为列表的例子为例，您只需返回列表的长度。

```
class MyDataset:
   def __getitem__(self, index):
       return self.input[index], self.label[index]
   def __len__(self):
       return len(self.input)
```

需要注意的是，getitem 函数中的数据不一定是元组。您可以用任何类型的数据结构返回数据。如果您的模型有多个输入，您可以将数据作为字典返回。如果只有一个输入，可以将其作为元组返回。这真的没有关系，因为在训练循环中，您只需正确地从数据结构中检索输入。

```
class MyDataset:
   def __getitem__(self, index):
       return {"Input_1": self.input1[index], 
               "Input_2": self.input2[index], 
               "Input_3": self.input3[index], 
               "Label": self.label[index]}
   def __len__(self):
       return len(self.input1)
```

## 2.创建数据加载器

DataLoader 是 Pytorch 提供的一个工具，它使得创建批量数据并将其发送到模型中变得非常容易。它使用我们在上一部分中创建的 dataset 类，以及其他一些东西来创建批处理。下面是你如何编码它们:

```
from torch.utils.data import DataLoader
data = MyDataset(parameters here)
data.build_dataset()## A helper function to do the preprocessingdataloader = DataLoader(dataset = data, batch_size = batchsize, shuffle=True)
```

数据加载器提供的一个非常重要的功能是，它们允许我们在将每批数据发送到模型之前，对每批数据应用特定的函数。他们通过 collate 类来实现这一点。collate 是一个一次接收一批数据的类，可以修改该批数据并对数据执行任何特定于批的功能。它是这样工作的:

```
class MyCollate():
  def __call__(self, batch):
    ## do whatever operations you want here
    ## return the new batch of data
```

为了正确地创建 collate 类，您必须重写 __call__()函数。调用函数将接收一批数据，并返回一批新的、修改过的数据。collate 中 __call__()函数的输入批处理只是一个通过多次调用数据集中的 getitem 函数构建的列表，这是由数据加载器完成的。列表中的每个项目都是数据集中的一个项目或 x，y 对，可以从数据集类中的 __getitem__()函数中检索到。批处理的输出必须以稍微不同的方式构建。当您将一批数据发送到模型中时，例如，如果您的批量大小为 16，则模型的输入张量将被构造为一个列表/张量中的 16 个单独的输入，而标签/输出张量将是 16 个单独的标签。因为我们覆盖了 collate 函数，所以我们必须手动完成这个过程。如果我们没有覆盖 collate 函数，那么 Dataloader 将通过默认的 collate 函数自动完成这项工作。

collate 函数的输入结构如下:

```
(input1, label1), 
(input2, label2), 
(input3, label3), 
(input4, label4), 
(input5, label5)
```

如果批量大小为 5。我们从 collate 函数返回的输出应该是这样的结构:

```
(input1, input2, input3, input4, input5), 
(label1, label2, label3, label4, label5)
```

假设我正在做一个 NLP 任务。如果我试图将一批中的标记化句子填充到相同的长度(假设是该批中最长句子的长度)，那么我会这样做:

```
class MyCollate():
  def __call__(self, batch):
    ## find maximum length of a sentence
    max_len = 0
    for item in batch:
       max_len = max(max_len, len(item[0]))
    new_input = []
    new_label = [] 
    for item in batch:
       ## pad all items in batch to max_len
       val = item[0]
       val = pad(val, max_len) 
       ##pad is the function you should create
       new_input.append(val)
       new_label.append(item[1])
    new_input = torch.tensor(new_input)
    new_label = torch.tensor(new_label)
    return new_input, new_label
```

要将校对功能与您的数据加载器集成，只需执行以下操作:

```
data = MyDataSet(parameters here)
data.build_dataset()dataloader = DataLoader(dataset = data, batch_size = batchsize, shuffle=True, collate_fn = MyCollate())
```

shuffle=true 参数只是在创建批处理之前随机打乱数据集中的数据。通常的做法是混洗训练数据加载器的数据，而不是验证和测试数据加载器的数据。关于所有 PyTorch 机器学习项目，需要记住的一点是，模型只能接受张量形式的输入，这就是我在上面的代码片段中返回输入和标签列表之前，将它们都转换为张量的原因。

需要记住的重要一点是，用于在 getitem 函数和 collate 中存储数据的数据结构必须相同，并且它们的大小也必须相同。如果在 getitem 函数中返回一个包含 5 个键值对的字典，则必须在 collate 中返回相同的键。这些值会有所不同，因为您已经对该批数据做了一些预处理。正如我之前提到的，返回的批次的结构会有所不同。如果在 getitem 函数中使用了字典，调用函数的输入将是

```
{keys:values}, 
{keys:values}, 
{keys:values}, 
{keys:values}, 
{keys:values}
```

如果批量大小为 5。每批的密钥都是相同的。返回的输出结构将是

```
{key1: all values for key1}, 
{key2: all values for key2},
{key3: all values for key3},
{key4: all values for key4},
{key5: all values for key5}
```

在大多数机器学习项目中，您通常需要三个不同的数据加载器:一个用于训练循环、验证循环和测试循环。在 PyTorch 中实现这一点非常简单:

```
from torch.utils.data.dataset import random_splitdata = MyDataset()
data.build_dataset()train_len = int(0.7 * len(dataset))
test_len = len(dataset) - train_len
train_data, test_data = random_split(dataset, (train_len, test_len))
val_len = int(0.33 * len(test_data))
test_len = len(test_data) - val_len
test_data, val_data = random_split(test_data, (test_len, val_len))train_loader = DataLoader(dataset = train_data, batch_size = batchsize, shuffle = True, collate_fn = MyCollate())test_loader = DataLoader(dataset = test_data, batch_size = batchsize, shuffle = False, collate_fn = MyCollate())val_loader = DataLoader(dataset = val_data, batch_size = batchsize, shuffle = False, collate_fn = MyCollate())
```

您只需使用 PyTorch 提供的 random_split 函数将原始数据集分割成任意数量的部分。我将上面的数据集分成 70%的训练、20%的测试和 10%的验证。在创建每个数据集之后，您可以简单地将它们传递给自己的数据加载器，并定期使用它们。

## 3.创建培训、验证和测试循环

PyTorch 中的训练、验证和测试循环相当简单，也很相似。以下是创建训练循环的方法:

```
model = model.to(device)
model = model.train()
for index, batch in enumerate(train_loader):
    x = batch[0].to(device)
    y = batch[1].to(device)
    optimizer.zero_grad()
    output = model(x).to(device)
    loss = criterion(output, y).to(device)
    loss.backward()
    optimizer.step()
```

下面是如何创建一个验证/测试循环(它们是同一个东西，但是有不同的数据加载器)。

```
model = model.to(device)
model = model.eval()with torch.no_grad():
    for index, batch in enumerate(train_loader):
        x = batch[0].to(device)
        y = batch[1].to(device)
        output = model(x).to(device)
        loss = criterion(output, y).to(device) 
```

优化器是你选择的优化器(我通常选择 Adam)，准则是我通常给我的损失函数起的名字。您必须预先对它们进行初始化，以下是您的操作方法:

```
import torch.optim as optim
import torch.nn as nnlearning_rate = 1e-3optimizer = optim.Adam(model.parameters(), lr = learning_rate)
criterion = nn.BCEwithLogitsLoss()##you can use any loss function
```

我们必须在验证/测试循环中指定 with torch.no_grad()，以确保 PyTorch 不会计算反向传播的梯度，因为我们在这里不进行反向传播。如果你不包含这段代码，你的程序仍然可以工作，但是它将消耗更多的内存，因为 PyTorch 将计算和存储我们甚至不会使用的模型的梯度。

关于训练和测试循环，需要指出的一件重要的事情是，即使我将 x = batch[0]和 y = batch[1]，您也不需要像这样明确地构造模型的输入(作为一个元组)，特别是如果您使用了不同的数据结构。您只需要确保从在 collate 中的 getitem 函数和 call 函数中使用的数据结构中正确地检索数据。需要注意的一点是，某些损失函数需要输出的特定形状/尺寸和 y/标签，因此在从模型中获得输出后，请确保对其进行整形，然后将其发送到损失函数或我在上面命名的标准中。

还有一件重要的事情要记住，你需要把。除了优化器和训练循环中的损失。这将把你的张量和数据放到你指定的设备上。如果您不知道如何创建设备，以下是方法:

```
device = torch.device('cuda' if torch.cuda.is_available() else "cpu")
```

如果你有一个 GPU，那么设备将自动成为一个 GPU。不然就是 CPU 了。

完成 PyTorch 项目设置的最后一步是将前面的三个步骤结合起来。创建一个初始化所有需要的东西的函数，一个训练模型的函数，一个评估模型的函数。然后你只需要把所有的东西组合在一起，这相对来说比较简单。

您还应该记得将模型权重保存到文件中。除了将模型权重保存到文件中，我还保存了优化器权重。您可以像这样保存模型/优化器权重:

```
model = Model(parameters here
optimizer = optim.Adam(model.parameters(), lr = learning_rate)
save_dict = {'Optimizer_state_dict': optimizer.state_dict(), 
             'Model_state_dict': model.state_dict()}
torch.save(save_dict, file_path)
```

加载模型也非常容易。

```
load_dict = torch.load(file_path)
model.load_state_dict(load_dict['Model_state_dict'])
optimizer.load_state_dict(load_dict['Optimizer_state_dict'])
```

我希望你觉得这篇文章简单易懂，内容丰富。如果你有任何问题，请在下面的评论中提出。