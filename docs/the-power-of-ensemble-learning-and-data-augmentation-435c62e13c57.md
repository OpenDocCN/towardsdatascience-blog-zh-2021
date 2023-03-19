# 集成学习和数据扩充的力量(使用 MNIST 数据集)

> 原文：<https://towardsdatascience.com/the-power-of-ensemble-learning-and-data-augmentation-435c62e13c57?source=collection_archive---------30----------------------->

## MNIST 数据集上的完整代码示例，VGG16 | ResNet50 | FG-UNET |多数投票|单次可转移投票|即时决胜投票

# 目录

1.  [简介](#0a2f)(MNIST 数据集和目标)
2.  [合奏中包含的模特](#2f65)
    一、 [VGG16](#2198)
    二。 [ResNet50](#a27e)
    iii。 [FG-UNET](#d6b5)
3.  [数据扩充](#98a9)
4.  [集成学习方法](#0e40)
    一、[多数表决](#9e71)
    二。[岭回归合奏](#081f)
    iii。[单一可转让票(STV)](#cbd1)
    iv。[即时决胜投票(IRV)](#1671)
5.  [关键外卖](#01ab)

# 介绍

作为这篇博客的第一篇文章，我决定从一些简单的东西开始——写一个我早期在 Kaggle 上做的关于数字识别的老项目。这个任务很简单(**手写数字上的数字[0–9]识别**)并且已经被 ML 社区很好地解决了，但是它是一个很好的玩具数据集来启动[我的 Kaggle 投资组合](https://www.kaggle.com/socathie)。

要了解更多关于 MNIST 数据集的信息，您可以访问[它在 TensorFlow 数据集上的页面，这里是](https://www.tensorflow.org/datasets/catalog/mnist)。简而言之，MNIST 数据集包含需要被分别分类成 10 个数字的手写数字的图像。

Kaggle 有一个永久运行的[玩具竞赛](https://www.kaggle.com/c/digit-recognizer)，它为任何人提供了一个方便的平台，通过设置好的编程环境和模型评估来测试他们的数据科学技能。使用的度量是**分类准确度**，即正确图像预测的百分比。

下面是[数据描述](https://www.kaggle.com/c/digit-recognizer/data):

> 数据文件 train.csv 和 test.csv 包含手绘数字的灰度图像，从 0 到 9。
> 
> 每幅图像高 28 像素，宽 28 像素，总共 784 像素。每个像素都有一个与之关联的像素值，表示该像素的亮度或暗度，数字越大表示越暗。该像素值是 0 到 255 之间的整数，包括 0 和 255。

![](img/d95cbf7d970455539710e2049ebbf991.png)

手写数字仅供参考(照片由 [Pop &斑马](https://unsplash.com/@popnzebra?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄)

请注意，图像尺寸仅为 28×28 像素，这对于 [tf.keras.applications](https://www.tensorflow.org/api_docs/python/tf/keras/applications) 中的许多模型来说太小了。在本文的其余部分，我们将假设数据已经被预处理成形状为**(样本大小，32，32，1)** 的数组，方法是[在图像](https://numpy.org/doc/stable/reference/generated/numpy.pad.html)的每一侧填充 2 行 2 列零。Kaggle 上的 MNIST 数据集有 42，000 个训练样本和 28，000 个测试样本。

# 这套服装中的模特

# 一、VGG16(准确率 98.80%)

这是在 MNIST 数据集上实现 VGG16 (增加了数据)的完整的 [Kaggle 笔记本。](https://www.kaggle.com/socathie/mnist-w-vgg16/)

VGG16 是由 [Simonyan 和 Zisserman (2014)](https://arxiv.org/abs/1409.1556) 作为提交给 [ILSVRC2014](https://www.image-net.org/challenges/LSVRC/2014/) 的，在 [ImageNet](https://www.image-net.org/about.php) 中实现了 92.7%的 top-5 测试准确率。数字 16 代表网络的层数。这个模型还有一个变种， [VGG19](https://keras.io/api/applications/vgg/) ，它的网络改为 19 层。

Tensorflow 的 Keras API 有一个预训练的 VGG16 模型，它只接受 224x224 的输入大小。对于 MNIST 数据集，我们将使用 Keras API 创建一个输入大小为 32x32 的 VGG16 网络，并从头开始训练，如以下代码所示。

```
from tf.keras.applications import VGG16
from tf.keras import Model
from tf.keras.layers import Densevgg  = VGG16(include_top=False, weights=None, input_shape=(32,32,3), pooling="max")x = vgg.layers[-1].output
x = Dense(10, activation='softmax', name='predictions')(x)
model = Model(inputs=vgg.layers[0].output,outputs=x)
```

10%的样品留作验证之用。该模型使用 [Adam 优化器](https://keras.io/api/optimizers/adam/)对[分类交叉熵](https://www.tensorflow.org/api_docs/python/tf/keras/losses/CategoricalCrossentropy)进行 100 个时期的训练，使用[提前停止](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/EarlyStopping)标准，即验证损失的耐心为 10。

除了将在下面[描述的数据扩充，VGG16 在测试集上实现了 98.80%的分类准确率。](#98a9)

# 二。ResNet50 (99.17%的准确率)

这是在 MNIST 数据集上实现 ResNet50 (带有数据扩充)的完整 [Kaggle 笔记本。](https://www.kaggle.com/socathie/mnist-w-resnet-and-data-augmentation/)

ResNet 由[何等(2016)](https://arxiv.org/abs/1512.03385) 提出，其一个变种在 [ILSVRC2015](https://image-net.org/challenges/LSVRC/2015/) 获得第一名。ResNet 的特点是*跳过连接*，网络中跳过几层的捷径，旨在解决[消失梯度](/the-vanishing-gradient-problem-69bf08b15484)的问题。数字 50 再次表示网络的层数。

Tensorflow 的 Keras API 也有一个预训练的 ResNet50 模型，它同样只接受 224x224 的输入大小。与 VGG16 类似，我们将使用下面的代码在 MNIST 数据集上从头开始训练模型。

```
from tf.keras.applications import ResNet50res  = ResNet50(include_top=False, weights=None, input_shape=(32,32,3), pooling="max")x = res.layers[-1].output
x = Dense(10, activation='softmax', name='predictions')(x)
model = Model(inputs=res.layers[0].output,outputs=x)]
```

使用上述相同的训练参数，ResNet50 在测试集上实现了 99.17%的分类准确率。

# 三。FG-UNET (97.93%的准确率)

这是在 MNIST 数据集上实现 FG-UNET 的完整的 [Kaggle 笔记本。](https://www.kaggle.com/socathie/mnist-w-fg-unet/)

我在为一个客户的卫星图像项目工作时遇到了 FG-UNET，并发现它特别有趣。该模型基于 [U-Net](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/#:~:text=The%20u%2Dnet%20is%20convolutional,and%20precise%20segmentation%20of%20images.&text=U%2Dnet%20architecture%20(example%20for,on%20top%20of%20the%20box.) 架构，该架构由几个下采样和上采样路径组成，用于对生物医学图像进行逐像素分类。FG-UNET 试图借用这种架构，同时通过添加完全连接的路径而不进行任何下采样或上采样来保留细粒度信息。

修改 FG-UNET 对 MNIST 数据集进行分类真的只是一个有趣的实验。特别是，我们不再需要按像素分类，因此修改了最终图层以展平 2D 影像并输出到 10 个类别的密集图层。具体的内核大小和使用的层可以在下面的代码中找到。

在没有数据扩充的情况下，FG-UNET 在测试集上实现了 97.93%的分类准确率。

# 数据扩充

数据扩充是[一种在有限数据集下增强模型性能的成熟技术](https://journalofbigdata.springeropen.com/articles/10.1186/s40537-019-0197-0)。通过旋转、移动和剪切输入图像，网络将能够学习手写数字的更好的内部表示，而不是过度适应训练图像。

Keras API 有一个 [ImageDataGenerator](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator) ，它可以方便地提供一个生成器，为模型生成增强的输入图像。

```
from tf.keras.preprocessing.image import ImageDataGeneratorgen = ImageDataGenerator(rotation_range=30, width_shift_range=2, height_shift_range=2, shear_range=30, validation_split=val_rate)
```

# 集成学习方法

为什么要集成学习？嗯，*三个臭皮匠胜过一个*(这里是三个臭皮匠)。

假设我们有上面的三个模型，它们的误差率分别为 1.20%、0.83%和 2.07%。他们未能正确预测的图像集可能不会完全重叠。一个模型可能难以区分 1 和 7，而另一个模型可能难以区分 3 和 8。如果我们能够以某种方式将所有三个模型的学习结合起来，我们应该会得到一个比这三个单独的模型更好的模型！

我们如何组合这些模型？事实证明，有许多方法可以使用集成学习，我们将在下面讨论其中的几种。这里是 Kaggle 上的[全合奏学习笔记本](https://www.kaggle.com/socathie/multiple-model-ensemble)。

# 一、多数表决(准确率 99.35%)

最直接的合奏方法就是让模特们“投票”。如果三个模型中的两个或更多个预测了相同的数字，则它们是正确的可能性很高。

当没有多数时，多数表决的问题就出现了，特别是考虑到我们有 10 路分类。当没有多数时，有许多可能的解决方案，但这里我们使用一个简单的解决方案，如果没有多数，我们使用 ResNet50，因为它具有最高的单一模型精度。

在 28，000 个测试样本中，只有 842 个(3.01%)没有一致的预测，需要投票。在 842 次“选举”中，只有 37 次(0.13%)没有获得多数票。这意味着该例外仅适用于极少数样本。

通过多数投票，MNIST 数据集上的分类准确率提高到 99.35%，高于三个模型中的任何一个。

# 二。岭回归集成(99.33%的准确率)

岭回归通常被称为[一种正则化普通线性回归的技术](/ridge-and-lasso-regression-a-complete-guide-with-python-scikit-learn-e20e34bcbf0b)。但是，也可以通过将单个模型预测作为输入，将分类标签作为输出，使用岭回归来创建集成。

这三种模型的输出都是 10 位数的概率分布。通过连接 3x10 个概率，我们得到一个 30 维的向量 *X* 。对于输出向量 *y* ，我们简单地将类标签编码成[单热点向量](https://www.tensorflow.org/api_docs/python/tf/keras/utils/to_categorical)。通过对输入 *X* 和输出 *y* 进行岭回归拟合，回归器将预测 10 个类别的*伪*-概率向量(*伪*，因为该向量可能未被归一化)。

岭回归集成将 MNIST 数据集上的分类准确率提高到 99.33%。

# 四。单一可转移投票(STV，99.36%的准确率)

对于接下来的两种集成方法，我们将使用一个叫做 [PyRankVote](https://pypi.org/project/pyrankvote/) 的便捷模块。PyRankVote 由 Jon Tingvold 于 2019 年 6 月创建，实施单一可转让投票(STV)、即时决胜投票(IRV)和优先集团投票(PBV)。

在 STV，每个模型按照预测概率的降序排列，为他们排序的选择投下一张“选票”。在每一轮，最不受欢迎的“候选人”被淘汰。只要有可能，投票会投给模型的首选，但是如果他们的首选被排除，那么投票会投给他们的第二、第三、第四选择，等等。该过程递归运行，直到剩下一个获胜者。

使用 PyRankVote，我们可以用几行代码实现 STV:

```
import pyrankvote
from pyrankvote import Candidate, Ballotcandidates = [Candidate(i) for i **in** range(10)]
ballots = []
for j **in** range(3):
    ballot = np.argsort(X_pred[i,:,j]) #input is the predicted probability distribution
    ballot = np.flip(ballot) #flipping to descending order

    ballots.append(Ballot(ranked_candidates=[candidates[i] for i **in** ballot]))

election_result = pyrankvote.single_transferable_vote(candidates, ballots, number_of_seats=1)winners = election_result.get_winners()
```

STV 在 MNIST 数据集的 Kaggle 测试集上取得了 99.36%的分类准确率。

# 动词 （verb 的缩写）即时决胜投票(IRV，99.35%的准确率)

与 STV 相似，每个模特也为 IRV 的一个排名列表投票。如果一名“候选人”在第一轮投票中获得简单多数(超过半数)，该候选人获胜。否则，最不受欢迎的“候选人”将被淘汰，而那些首先投票给最不受欢迎的候选人的人的选票将被计入他们的下一个选择。

根据我们使用[多数投票](#9e71)方法的经验，我们知道 28，000 个测试样本中只有 37 个没有简单多数胜出者。因此，只有这 37 个样本会受到 IRV 的影响。

IRV 获得了与多数投票法相同的 99.35%的分类准确率。

# 关键外卖

下表总结了所有型号的性能:

我们观察到集合模型性能都是相似的，范围在 99.3%到 99.4%之间。这篇文章的关键要点不是一种集成方法是否比另一种更好，而是所有的集成模型都可以比三个单独的模型中的任何一个达到更高的精度。

在学术研究中，由于发表的压力，我们经常专注于为特定的机器学习任务找到最佳模型，而独立的最佳表现模型的优雅更容易被期刊接受。在行业中，没有最终部署在最终产品中的模型的先前迭代通常被认为是沉没成本。如果资源允许，部署几个表现良好的模型的组合可能会比单个表现最好的模型产生更好的性能，并帮助公司更好地实现其业务目标。

# 参考

**VGG16** :西蒙扬，k .，&齐塞曼，A. (2014)。用于大规模图像识别的非常深的卷积网络。 *arXiv 预印本 arXiv:1409.1556* 。

何，王，张，徐，任，孙，孙(2016)。用于图像识别的深度残差学习。IEEE 计算机视觉和模式识别会议论文集*(第 770–778 页)。*

**FG-UNET** :斯托伊安，a .，普莱恩，v .，英格拉达，j .，普贡，v .，&德克森，D. (2019)。用高分辨率卫星图像时间序列和卷积神经网络制作土地覆盖图:业务系统的适应和限制。*遥感*， *11* (17)，1986 年。

**数据扩充** : Shorten，c .，& Khoshgoftaar，T. M. (2019)。面向深度学习的图像数据增强综述。*大数据杂志*， *6* (1)，1–48。

*关于作者:我是一名自由数据科学家和 ML 工程师。我为贵公司的数据科学和机器学习需求提供定制解决方案。更多信息请访问 www.CathieSo.com*[](https://www.CathieSo.com)**。**