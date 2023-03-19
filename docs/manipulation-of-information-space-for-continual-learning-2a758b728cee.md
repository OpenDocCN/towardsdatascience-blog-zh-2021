# 为持续学习而操纵信息空间

> 原文：<https://towardsdatascience.com/manipulation-of-information-space-for-continual-learning-2a758b728cee?source=collection_archive---------41----------------------->

## VeriMedi 项目动机文章—持续学习、深度度量学习、调整信息理论

让我们从一些问题开始:

假设我有两个概念，比如巧克力曲奇和布朗尼。如果我说巧克力曲奇的值是 0，布朗尼的值是 1，这是不是一个错误的说法？

给出不同的值有助于我区分巧克力曲奇和核仁巧克力饼。如果我用相同的价值来代表两者，当我品尝它们的时候，我能认出它们的不同吗？如果我不能区分它们，那它们还会是信息吗？

这个帖子是关于信息的表示和信息操作的实现。本文中产生和呈现的想法和方法是为我在工作中参与的一个创新项目——药丸识别项目而设计的:[链接到预印本](https://www.researchgate.net/publication/350995592_VeriMedi_Pill_Identification_using_Proxy-_based_Deep_Metric_Learning_and_Exact_Solution)
所以，让我们从信息开始。信息是当存在两个或两个以上不同事实时出现的现象。当信息所代表的事实能够被感知它的人获得和区分时，信息就能够被识别和获得。所以，要创造信息，就应该有不同的事实，而且它们需要被感知它的人感知为两个或两个以上不同的事实。这是森林故事里的经典树。如果一棵树在森林里倒下了，这是信息吗？既然没人观察，没人知道，没人听见，那就不是。

![](img/86d92de63cbb24366c148459ccd08a18.png)

[帕特·惠伦](https://unsplash.com/@patwhelen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

信息是可以测量的，而且是通过信息的熵来测量的。根据 Shannon 的说法，信息的熵不是由感知者感知的数据量，而是由感知者感知的不同数据量。那么，什么是信息熵呢？信息熵是定义信息的概念的分布。想象一个向量，一个箭头，从你的电脑到厨房的水槽。这个向量代表一个概念。如果这是该空间中唯一的向量，则该信息不存在。当从你的电脑到门有第二个矢量，那么信息就存在了。因为我们可以说，电脑和水槽之间的矢量，和电脑和门之间的矢量是不一样的。信息不是矢量之间的差。信息是当这两个向量之间存在差异时出现的现象。

我们如何测量两个向量之间的差异？我们可以测量它们之间的欧几里德距离。此外，向量的方向可以不同，这意味着两个向量之间的余弦相似性可以帮助我们测量它们之间的差异。如果我们想用一个向量来表示一个概念呢？我们如何做到这一点？我们可以为这个概念指定任何向量。在主流分类模型中，表示概念的向量是二元向量。例如:概念 1 是 0001，而概念 2 是 0010，概念 3 是 0100，概念 4 是 1000。但问题是“我们能用非二进制的向量来表示概念吗？”。是的，我们可以。基于代理的深度度量学习模型正是这样做的。他们为每个类定义代理多维向量，然后尝试使输入数据适合这些代理。深度度量学习中使用了两种基于代理的损失函数:代理 NCA 损失([链接](https://arxiv.org/pdf/1703.07464.pdf#:~:text=Proxy%2DNCA%20converges%20about%20three,than%20to%20the%20dissimilar%20one.))和代理锚点损失([链接](https://arxiv.org/abs/2003.13911))。不幸的是，这两个损失函数都与代理的初始化无关。因此，当代理的向量大小不够大时(相对于向量所代表的代理的数量)，这些代理有可能彼此接近。此外，我们知道，当代表代理的向量足够明显时，信息就出现了。所以，我们定义的代理向量应该尽可能地远离彼此，这样我们才能更好地表示信息。我们需要能够操纵这个代表性的信息空间。为了做到这一点，我开发了一种优化方法，它将一组向量(代理)作为输入，并通过使用梯度下降来分解它们，以便它们的方向将是不同的。

要使这种方法奏效，需要遵循几个步骤:

*   定义正态分布的代理
    定义一个线性层来分配代理。
*   将所有代理注册为参数，以便更新它们。
*   计算每个二进制代理组合中代理之间的余弦相似度。
*   计算余弦相似度和零向量之间的损失。
*   反向传播丢失，以便更新代理。

很简单，对吧？

代码如下:

首先，让我们定义 l2_norm 和计算向量的二进制组合之间的余弦相似性的相似性函数:

```
import itertoolsdef l2_norm(self, x):
    input_size = x.size()
    buffer = torch.pow(x, 2)
    normp = torch.sum(buffer, 1).add_(1e-12)
    norm = torch.sqrt(normp)
    _output = torch.div(x, norm.view(-1, 1).expand_as(x))
    output = _output.view(input_size)
    return outputdef sim_func(base_proxies):
    layers = l2_norm(base_proxies)
    combinations = list(itertools.combinations(np.arange(0,len(layers)), 2))
    sim_mat = torch.nn.functional.linear(layers, layers)
    similarity_vector = torch.zeros(len(combinations))
    for cnt,comb in enumerate(combinations):
        similarity_vector[cnt] = sim_mat[comb[0],comb[1]]
    return similarity_vector
```

现在，让我们定义稍后将处理分解操作的代理和类。我将把 sim_func 和 l2_norm 放在这个类中。

```
class ProxyOperations(torch.nn.Module):def __init__(self, base_proxies = None):
        super(ProxyOperations, self).__init__()
 if base_proxies == None:
            base_proxies = torch.randn(4, 2)
            torch.nn.init.kaiming_normal_(base_proxies, mode='fan_out')
        self.base_proxies = torch.nn.Parameter(base_proxies) def l2_norm(self, x):
        input_size = x.size()
        buffer = torch.pow(x, 2)
        normp = torch.sum(buffer, 1).add_(1e-12)
        norm = torch.sqrt(normp)
        _output = torch.div(x, norm.view(-1, 1).expand_as(x))
        output = _output.view(input_size)
        return output def sim_func(self):
        layers = l2_norm(self.base_proxies)
        combinations = list(itertools.combinations(np.arange(0,len(layers)), 2))
        sim_mat = torch.nn.functional.linear(layers, layers)
        similarity_vector = torch.zeros(len(combinations))
        for cnt,comb in enumerate(combinations):
            similarity_vector[cnt] = sim_mat[comb[0],comb[1]]
        return similarity_vector
```

现在，让我们在某个地方定义类，然后开始分解过程。

```
POP = ProxyOperations()
optimizer = torch.optim.Adam(POP.parameters(), lr=0.1)
lossfunc = torch.nn.MSELoss()
POP.train()cnt = 0
max_iter = 100
desired_sim_score = 0.1
loss_max = 1.while loss_max>desired_sim_score:
    optimizer.zero_grad()
    distance_vector = POP.sim_func()
    loss_mse = lossfunc(distance_vector, torch.zeros(distance_vector.shape)) loss_max = torch.max(torch.abs(distance_vector-torch.zeros(distance_vector.shape)))
    loss_mse.backward()
    optimizer.step()
    cnt += 1
    if cnt+1>max_iter:
        break
```

好的，我们的分解过程将最多运行 100 次迭代。我仅为本文创建了这段代码。不幸的是，我不能分享原始代码，因为知识产权和版权的东西。如果你出于非商业原因想要实施这个系统，你可以通过阅读论文来实现:)如果你是一家需要这个系统(持续学习模式)的公司，你需要通过我在论文中写的电子邮件再次联系我。

让我们继续这个话题:

使用这种方法，我们定义的代理在它们的方向上彼此远离。在代理被定义和优化之后，我们可以用新的代理来训练我们的网络。

现在该说说第二种方法了:代理添加。

因为我们可以优化代理并操纵信息空间，所以我们也可以向信息空间添加新的向量，以便我们可以向模型引入新的类，使其能够预测。想象一个分类模型，当需要一组新的类时，您不必从头开始重新创建和重新训练。不错吧。

所以现在，让我们想象一下我们如何做到这一点。我们是否可以创建一组数量更大的新代理，然后优化它们？这可能行得通，但不太合适。如果我们从头开始定义新的代理，那么模型将会很混乱，因为它已经学会了将你的输入图像映射到现有的代理(与以前的不同)。因为当你定义一个新的更大的代理集合时，你失去了之前的代理，在那里你的模型已经用来学习你之前输入的图像。

我们是否可以在现有的代理中添加新的代理，然后优化所有要分解的代理？这个想法比第一个好，但还不够好。因为我们添加的新代理将是随机的，当其他代理与这些新代理一起被分解时，它们将受到极大的影响。这意味着模型所学的将毫无意义，因为我们的初始代理已经再次发生了很大变化。此外，新的代理可能会比模型在当前状态下产生的代理更加不同。这又会增加训练的复杂性。

另一个想法是:如果我们生成属于新类别的图像的嵌入向量(使用初始代理预先训练的模型)，然后将这些嵌入向量用于新代理，会怎么样？这是可行的，让我们继续这个想法。因此，在生成嵌入向量之后，我们可以计算每个新类的平均向量，然后将这些平均向量作为新的代理添加到先前的代理集中。通过这样做，我们的旧代理不受影响，新代理是模型已经生成的向量。因此，我们不会增加这些新代理的训练复杂性。这听起来不错。但是，如果新生成的代理接近现有的代理呢？这里还有一个问题，我们来解决一下。我们需要优化这些新代理，使它们彼此远离，也远离旧代理。因为我们的新代理的起点将接近模型的当前状态，所以我们的训练复杂性将尽可能小。这说起来容易，但是在项目过程中很难实现。

实现这一想法有几个步骤:

*   生成属于新类别的图像的嵌入向量。
*   计算每个类别的平均向量，并将这些平均向量定义为候选代理。
*   将候选代理附加到旧代理，并仅将新代理注册为要更新的参数。
*   按照前面分解新代理的方法运行优化。

耶，我们成功了！但是等等，还有提升的空间。

在我们适当地扩大我们的代理空间之后，我们仍然可以通过优化这个新的、更大的代理集来尽可能地相互分解，从而增强我们的代理空间。所以，我们基本上可以再次运行第一次优化。现在，我们准备好了，☺

详细信息请阅读论文预印本(链接在上面)。

我们在本文中还应用了一项创新。如果你读过我以前的一篇文章([conv·托奇求解文章](/backpropagation-in-convolution-layers-using-system-solution-torch-solve-a00139da00db))，你就会知道，利用线性代数的精确解，只需一步就可以训练出一个卷积层。有趣的巧合是，澳大利亚的研究人员发表了一篇做类似事情的论文([链接到论文](https://link.springer.com/chapter/10.1007%2F978-3-030-63823-8_63))。基本上，他们用梯度下降法训练 CNN 只需几次迭代。然后，他们采用倒数第二层，并为其计算最佳权重，以最佳方式将输入映射到输出。我强烈建议你读读那篇论文，它太棒了！

所以，回到正题。我们所做的是，在我们的识别解决方案中，我们实施了经过精确解决方案训练的全连接层。我们不是一开始训练一个经典的 CNN，而是训练一个深度的度量学习模型来生成嵌入。我们认为，我们可以使用精确的解决方案来训练完全连接的层，以将生成的嵌入向量映射到输出向量，输出向量是表示类的二进制向量。你知道吗，这招很管用。

请查看论文中的实验部分，查看我们所做的不同实验的结果。

现在，我们有了一个可以像人类一样不断学习的模型。在用新的类训练后，它甚至可以对以前学习的类给出更好的预测。这是有意义的，因为它有能力更好地解释知识。

请查看论文中的进一步研究部分(链接在上面)。这个新系统还有很多工作要做。

我的下一篇文章将是关于我在进一步研究部分提到的方法。

为大家干杯，我希望你们喜欢这篇文章和报纸。

## [链接到纸质预印本](https://www.researchgate.net/publication/350995592_VeriMedi_Pill_Identification_using_Proxy-_based_Deep_Metric_Learning_and_Exact_Solution)