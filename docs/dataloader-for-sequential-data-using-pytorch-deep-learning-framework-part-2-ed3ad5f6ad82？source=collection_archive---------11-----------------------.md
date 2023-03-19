# 使用 Pytorch 深度学习框架的序列和图像数据集的数据加载器

> 原文：<https://towardsdatascience.com/dataloader-for-sequential-data-using-pytorch-deep-learning-framework-part-2-ed3ad5f6ad82?source=collection_archive---------11----------------------->

## 教程使您能够使用 PyTorch 为任何类型的数据集编写数据加载器

![](img/1cc99d4312844c5c6178a1cc5e9f5bc7.png)

塔米姆·汗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

深度学习和机器学习算法正在统治这个世界。PyTorch 是最常用的深度学习框架之一，用于实现各种深度学习算法。另一方面，基于学习的方法本质上需要一些带注释的训练数据集，模型可以使用这些数据集来提取输入数据和标签之间的关系。为了向神经网络提供数据，我们定义了一个数据加载器。在这篇博客中，我们将看到如何在 PyTorch 框架中为不同的数据集编写数据加载器。

# 影像数据集的数据加载器

我们将致力于狗和猫的图像分类问题。我们必须对给定的图像是猫还是狗进行分类，数据集可以从[这里](https://www.kaggle.com/c/dogs-vs-cats)下载。训练数据集总共包含 25，000 幅图像。由于这是一个分类问题，狗的标签是“ **0** ”，猫的标签是“ **1** ”。

让我们从导入所有需要的库开始。

```
import os
from PIL import Image
import torch
from torch.utils.data import DataLoader, Dataset
import torchvision.transforms as transforms
import torch.nn as nn
```

PyTorch 框架的数据集类被定义为一个类，其基本结构如下

```
class data(Dataset):
   def __init__(self, param1, param2):
        #the function is initialised here

   def __len__(self):
        #the function returns length of data 

   def __getitem__(self, index):
        #gives one item at a time
```

*   这个类的最终目的是使用函数 *__getitem__，一次提供一个数据点。*这是通过使用*索引*完成的，索引在内部传递给函数，使用 Dataloader 中定义的采样器函数(将在接下来的博客中讨论)。
*   当初始化数据集的对象时，调用函数 *__init__* 。在这里，您可以传递多个对编写 *__getitem__* 有用的参数。
*   *__len__* 函数用于返回数据集的总长度。在此基础上，将生成*索引*，然后提供给 *__getitem__* 。

狗与猫数据集的格式如下:

```
data/
   - dog_1.jpg
   - dog_2.jpg
    ...
    ...
    ...
   - cat_1.jpg
   - cat_2.jpg
    ...
    ...
    ...
```

现在，我们已经了解了编写数据加载器所需的组件，让我们更深入地研究我们的用例。

```
class data(Dataset):   
   def __init__(self, path, transform):
        self.files = os.listdir(path)
        self.transform = transform
        self.path = path   def __len__(self):
        return len(self.files)   def __getitem__(self, index):
       filename = self.files[index]
       input = Image.open(os.path.join(self.path, filename))
       label = 0 if filename.find("dog")>=0 else 1
       img_as_tensor = self.transform(input)
       return img_as_tensor, labeltransformations = transforms.Compose(
         [transforms.Resize((224,224)),transforms.ToTensor()]
                 )
path = "./data"
train_dataset = data(path, transformations)
dataloader = DataLoader(train_dataset, batch_size=Train_Batch_Size, shuffle=True)
```

*   我们先来了解一下 *__init__* 这个函数。类*数据*用两个参数初始化，*路径*和*转换*，它们作为参数传递给 *__init__* 。当我们声明这个类的一个对象时，它在内部调用 *__init__* ，其中存储了文件名列表 *self.files* 、 *self.transform* 中的转换和 *self.path* 中的路径。
*   由于 *__len__* 用于返回整个数据集的长度，所以我使用了 *len(self.files)* 来返回相同的长度。
*   函数 *__getitem__* 是最关键的，它加载图像，然后调整大小，再转换成张量。这里要注意的一件重要事情是，提供给神经网络的数据应该总是被归一化。我们使用*转换来处理规范化。ToTensor()* 。最后， *__getitem__* 返回两个东西，对应数据点的 *image_as_tensor* 和 *label* 。

在初始化类*数据*之后，我们使用一个数据加载器函数，该函数自动将所有数据批处理到一个定义的批处理大小。因此，如果您的原始数据点大小为(3，224，224)(从 *__getitem__* 获得)，则数据加载器的每一项都将具有大小( *batch_size* ，3，224，224)，即它会自动采样 *batch_size* 个数据点。这在我们的例子中是可能的，因为图像的大小是恒定的，所以 DataLoader 函数能够自动创建批处理。然而，在像自然语言处理这样的情况下，当大小不恒定时，我们需要编写自己的批处理函数。

# 顺序数据集的数据加载器

让我们处理顺序数据集，即句子、时间序列、音频等。现在。这里 *__getitem__* 将不再给我们相同大小的数据点。例如，考虑情绪分类的任务(这里解释)，那么一句话可以是“航班服务非常好”，另一句话可以是“我没有把行李放在传送带上，可怜的服务。”这里两个句子的长度不同。

为了解决这个问题，让我们先回答三个问题。

1.  什么是批处理？—批处理意味着将多个数据点的张量合并成一个张量
2.  为什么我们需要批处理？—批处理用于加快计算速度，因为通过批处理，我们可以一起处理多个数据点，而不是一次只处理一个。
3.  批处理是如何完成的？—因为我们在这里合并了多个张量，所以张量在每个维度上的大小需要相同。因为我们的数据点大小不一，所以我们面临一个问题。

我们现在主要需要解决配料问题。

出于我们在这里讨论的目的，我们将使用 IMDB 数据集，它是一个审查数据集，可以从[这里](http://ai.stanford.edu/~amaas/data/sentiment/)下载。由于我们在这里处理的是句子，处理数据集的方式会有所不同。由于神经网络只理解数字，而不是单词，我们必须将每个单词转换成数字。为了做到这一点，我们必须建立一个词汇表，如下面的代码所示。

*   函数 *reader* 用于读取全部数据，它返回所有句子和标签的列表，负面评论为“ **0** ，正面评论为“ **1** ”。
*   函数 *build_vocab* 将数据和最小字数作为输入，并将每个单词到唯一数字的映射(名为“ *word2id* ”)作为输出。对于向前的每个未知单词，对应的数字将是 1。

继续为顺序数据集编写数据集类。我们的目标是在给定索引的情况下，一次输出一个项目。

由于上面已经讨论了不同功能的功能，我将简要回顾一下。

*   函数 *__init__* 采用 *word2id* 映射和 *train_path* 。然后 *__init__* 调用*阅读器*获取句子对应的数据和标签。
*   函数 *__len__* 返回整个数据集的长度，即自身数据
*   函数*预处理*将输入的句子转换成一个数字张量，其中每个数字对应于句子中的单词。
*   函数 *__getitem__* 用于在*索引*的帮助下，一次输出一个已处理的数据点。

接下来解决每个数据点大小不同的问题。以下代码定义了 *collate_fn* 。该函数用于处理不同大小的批处理数据点。

```
train_dataset = Dataset_seq(word2id, train_path)
train_dataloader = DataLoader(dataset=train_dataset, batch_size=batch_size, shuffle=True,collate_fn=collate_fn)
```

这里要注意的一点是，在元组列表中，每个元组可以具有不同的大小，但是在张量中，沿着所有维度的大小需要相同，以便合并它们。

自动给 *collate_fn* 一个名为*data*的输入，这是一个长度等于批处理大小的元组列表。每个元组包含数字的十进制数( *seq* 从 *__getitem__* 返回)和它们对应的标签。为了简单起见，我们将分别称它们为*序列*和*标签*。因此，最终我们必须转换每个*序列*，使它们的大小保持不变。为此，我们执行零填充，如上面的代码所示。由于零填充统一用于整个数据集，因此模型知道它没有多大用处，它只是表示浪费值。

我们已经达成了一个解决方案，但问题仍然存在，这是一个最佳的解决方案吗？如果所有*序列*的原始大小相差很大，或者换句话说差异很大，我们最终会浪费大量填充零的 GPU 内存，这最终是没有用的。必须有一个更好的方法来最小化零填充的需求！这个问题及其解决方案在这里[讨论。](https://medium.com/@AnveeNaik/batch-sampler-for-sequential-data-using-pytorch-deep-learning-framework-part-3-df19f449f24e)

*成为* [*介质会员*](https://medium.com/@AnveeNaik/membership) *解锁并阅读介质上的许多其他故事。关注我们的* [*中的*](https://medium.com/@AnveeNaik) *，阅读更多此类博文*。