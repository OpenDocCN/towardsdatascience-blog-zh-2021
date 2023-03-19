# 核捕鼠器

> 原文：<https://towardsdatascience.com/the-nuclear-mousetrap-e189236d7c51?source=collection_archive---------29----------------------->

![](img/73294aa5116367cbb610f032cdfaa776.png)

图片由来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6047218) 的 [Marco Schroeder](https://pixabay.com/users/schroeder75-19752325/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6047218) 拍摄

## 人工智能很难

## 在拿出大枪之前，先尝试简单的小解决方案

科学家们现在正在建立万亿个参数模型。像 GPT-3 这样的大型系统和像[武道 2.0](/gpt-3-scared-you-meet-wu-dao-2-0-a-monster-of-1-75-trillion-parameters-832cd83db484) 这样更大的型号正在下线。这些真正巨大的模型，它们更小的基于变形金刚的小兄弟，甚至从零开始的好的旧神经网络在媒体和实际应用中非常受欢迎。有些场景中，这些火箭筒非常有用，但许多其他情况可以通过极其简单的机器学习模型来解决。

**核捕鼠器**是我在工程学院第一年教我的一个思想实验。这个想法是热核爆炸能有效杀死老鼠，但也是制造捕鼠器的一种非常昂贵的方式。太过分了。出于某种原因，物理学家也喜欢谈论[核捕鼠器](https://aapt.scitation.org/doi/abs/10.1119/1.1990988?journalCode=ajp)，但在这种情况下，我们谈论的是与更简单的“可接受”解决方案相比，一个解决方案的过度工程化。人工智能充满了对微小问题的巨大解决方案。您需要小心，不要被愚弄而采用机器学习指标中的“最佳”执行模型(例如，精确度、召回率、f1 分数等)。)然后意识到你将为硬件支付更多的钱，并且你的推理的吞吐量将会更低。在现实生活中，从工程角度来看，一个[大小合适的](https://www.merriam-webster.com/dictionary/rightsize)解决方案比从你的机器学习指标中榨取最后一滴更重要。

让我们用一两个例子来说明这一点。这些例子的代码[可以在这里找到](https://gist.github.com/dcshapiro/54d649a6a2c3e6739cc0fc9919552f34)。

## 示例 1:识别手写数字“0”

MNIST 数据集是众所周知的手写数字的小灰度图像的集合。该数据集是一个用于测量和比较机器视觉解决方案有效性的示例。MNIST 性能最好的机器学习模型[可以在这里找到](https://paperswithcode.com/sota/image-classification-on-mnist)。不幸的是，性能最好的解决方案在计算上也很昂贵，并且有令人讨厌的长延迟，我们很快就会看到这一点。我们能做什么来得到一个快速和便宜的识别数字 0 的方法？

首先，让我们回忆一下[维度缩减](https://scikit-learn.org/stable/modules/unsupervised_reduction.html)在推理时有计算成本，并影响我们的模型性能。然而，简单地删除和缩减采样维度是没有这些成本的。让我们使用一系列技术来删除 MNIST 图像的部分，并疯狂地对其进行下采样，以最小化我们的成本。

该数据集分为 60，000 幅图像的训练集和 10，000 幅图像的测试集。每个 MNIST 图像由 784 个像素组成，每个像素包含 0(黑色)到 255(白色)之间的整数值。下面是这些图像的三个例子。

![](img/02661cfcff7936ef1222eaa535986873.png)

为了开始简化我们的数据，我们可以简单地将每个图像展平成一长串数字，完全忽略数据是一幅图片。接下来，我们将数字 0，1，2，… 9 的标签转换成一个标签，当数字是 0 时，该标签为“1”，否则为“0”。

在这一点上，我们应该检查一些非常简单的模型(如决策树)是否适合我们的数据集。该模型很好地拟合了训练数据，决策树捕获了测试数据中 94%的零。以下是这种“不费力”模式的分类报告的一部分:

```
NO EFFORT
            precision    recall  f1-score   support
Not 0       0.99         0.99    0.99       9020
Was 0       0.92         0.94    0.93       980
accuracy                         0.99       10000
```

这个初始模型在每张图片中使用所有 784 个[无符号 8 位](https://www.binaryconvert.com/convert_unsigned_char.html?decimal=050053053)数字(字节)。接下来，让我们取图片中的每隔一列，然后砍掉前 10 个像素和后 10 个像素，因为我们真的不需要它们来识别零。我们也把每个像素压缩成 1 或 0。以前，像素是灰度的，可以是 0 到 255 范围内的任何值。现在一个像素不是黑就是白。经过这些改变后，我们最终得到每张图片剩余 372 像素。接下来，我们可以检查图像的训练数据集，以删除其中样本很少的数组索引(无论写入的数字是多少，大部分都是 0 或 1)。我们只能查看训练数据，然后当我们决定删除哪些图像像素时，我们只需将相同的删除应用于测试图像。

经过下采样并移除低多样性像素后，我们现在检查哪些像素与我们创建的标签具有低相关性。与答案不相关的像素我们可以直接删除。经过这一改变，我们最终得到每张图片 281 像素。

下一步是识别彼此高度相关的像素，并从图像中移除两个相关像素中的一个。这样做之后，我们最终得到每幅图像 255 个二进制像素。我们的整个图像现在适合 32 字节，而不是原来的 784 字节。我们不仅节省了存储空间。我们在这个压缩的图像数据集上拟合了一个决策树，以下是这个较小的“中等努力”模型的分类报告的一部分:

```
MEDIUM EFFORT
            precision  recall  f1-score   support         
Not 0       0.99       0.99    0.99       9020        
Was 0       0.92       0.95    0.94       980      
accuracy                       0.99       10000
```

等等……什么？我们删除了每张图片 784 字节数据中的 752 字节！该模型如何在完全相同的性能水平下工作？仔细看的话其实好一点。

嗯，这就是为什么我们在变复杂之前先尝试简单…而且我们还没有完成。我们可以进一步删除 127 个特征，基本上没有任何后果。在 255 个剩余像素中只有 128 个像素的情况下，我们将决策树模型拟合到训练数据，并获得这个“微小”模型的以下测试结果:

```
TINY
         precision    recall  f1-score   support         
Not 0    1.00         0.99    0.99       9020        
Was 0    0.92         0.97    0.94       980
```

还是…不…崩溃！

现在让我们看看我们从这项工作中得到了什么:

![](img/ddcd359eea2e4c4852e88872f9727585.png)![](img/4cec4e930419251ec9e2abcea4e6a4b4.png)

我们应该总结一下我们现在的情况。我们找到了一种更快、存储效率更高的方法来检测手写 0 字符。但是这个技巧对其他东西也有效吗？

## 示例 2:识别凉鞋

已经表明我们的哑方法在手写数字上工作得很好，同样的方法在更复杂的数据上工作吗？一段时间以来，科学家们一直担心 MNIST 是一个过于简单的数据集，因此有一个更复杂的数据集，名为[时尚 MNIST](https://github.com/zalandoresearch/fashion-mnist) ，比手写数字更具挑战性。虽然时尚 MNIST 仍然有相同的尺寸(784 个无符号的 8 位 T4 数字代表一张 28x28 的灰度图片)，但它比 MNIST 有更复杂的物体，比如衬衫和钱包。数据集中的对象有:`'T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat', 'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot'`。

顺便说一句，我对时尚 MNIST 的形象嵌入非常感兴趣:

![](img/d51316857a9f47aaeecabeff8f139be4.png)

来自[https://github.com/zalandoresearch/fashion-mnist](https://github.com/zalandoresearch/fashion-mnist)的时尚 MNIST 数据集的嵌入

在我们将 MNIST 的方法重新应用到时尚 MNIST 之前，让我们先来看几张图片:

![](img/4f2b360d4ac306aef3ff889cc3de3a5e.png)

时尚 MNIST 数据集中的一条裙子和两只凉鞋

我们可以看到这些物体确实比手写数字复杂得多。

那么，我们的技巧在这个数据集上表现如何呢？结果如下:

![](img/55b74f6f4622f18c243ed55ffb157f31.png)![](img/4cec4e930419251ec9e2abcea4e6a4b4.png)

看起来棒极了！质量指标如何？这个模型“工作”得够好吗？

```
NO EFFORT FASHION
           precision  recall  f1-score   support    
Not Sandal 0.99       0.99    0.99       9000       
Sandal     0.89       0.89    0.89       1000      
accuracy                      0.98       10000MEDIUM EFFORT FASHION
           precision  recall  f1-score   support    
Not Sandal 0.99       0.98    0.99       9000       
Sandal     0.87       0.88    0.87       1000      
accuracy                      0.97       10000TINY FASHION
           precision  recall  f1-score   support    
Not Sandal 0.98       0.99    0.98       9000       
Sandal     0.87       0.85    0.86       1000      
accuracy                      0.97       10000
```

从上面的结果中我们可以看到，即使在更高级的环境中，这些技巧也能很好地工作。在我们的方法没有改变的情况下，我们生成了非常简单的决策树，正确地捕获了测试集中 89%、88%和 85%的凉鞋。我们没有真正探索设计空间。这整个事情是一个相当低的努力第一次看。在许多使用案例中，我们可以对模型的输出进行去抖，以便同一对象的一行中的几个预测增加我们对观察的信心，从而使精确度和召回率高于此处报告的单帧水平。

## 结论

我们在本文中看到，简单的低工作量和低复杂性方法可以在模型质量、延迟和存储需求方面实现合理的性能。如果您试图最大化您的解决方案的速度，最小化模型参数的数量(以适应小型设备或嵌入式平台)，或最小化推理成本(例如，您的带有保留 GPU 的 REST API 比基于 CPU 的保留计算能力花费更多)。巨大的预训练模型对于你关心的问题来说可能是多余的。记得检查你的“大”解决方案是否是核捕鼠器。

这篇文章的代码是[，可以在这里](https://gist.github.com/dcshapiro/54d649a6a2c3e6739cc0fc9919552f34)找到。

如果你喜欢这篇文章，那么看看我过去最常读的一些文章，比如“[如何给一个人工智能项目定价](https://medium.com/towards-data-science/how-to-price-an-ai-project-f7270cb630a4)”和“[如何聘请人工智能顾问](https://medium.com/towards-data-science/why-hire-an-ai-consultant-50e155e17b39)”还有嘿，[加入快讯](http://eepurl.com/gdKMVv)！

下次见！

——丹尼尔
[linkedin.com/in/dcshapiro](https://www.linkedin.com/in/dcshapiro/)
[丹尼尔@lemay.ai](mailto:daniel@lemay.ai)