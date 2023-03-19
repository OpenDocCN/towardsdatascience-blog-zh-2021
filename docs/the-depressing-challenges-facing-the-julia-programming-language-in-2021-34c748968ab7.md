# 2021 年 Julia 编程语言面临的令人沮丧的挑战

> 原文：<https://towardsdatascience.com/the-depressing-challenges-facing-the-julia-programming-language-in-2021-34c748968ab7?source=collection_archive---------3----------------------->

## Julia 要在 2021 年末取得数据科学优势需要克服的障碍。

![](img/cdee598330d6ac5905ee695ea26ceca5.png)

(src =[https://pixabay.com/images/id-1447233/](https://pixabay.com/images/id-1447233/)

# 介绍

最近，在精彩的编程世界中，Julia 编程语言已经成为一个相当有趣的对象，在我看来，这是有充分理由的。朱莉娅有很多其他语言所没有的可爱之处。然而，没有一种编程语言是没有问题的，而且作为数据科学领域的新手，Julia 需要克服许多不同的挑战，才能真正进入这个行业。

在我们深入研究 Julia 的问题以及这种语言目前存在的某些问题之前，让我们先来了解一下为什么这种语言值得使用和保存。首先，之前已经有人提出，所有的编程语言范例都已经被创建和使用，但是 Julia 证明事实并非如此。在我们深入 Julia 的范式之前，让我们首先简单地提醒自己什么是范式。编程范例只是一个用于类型和方法相互交互的系统。实际上，我有一整篇关于编程范例的文章，更深入一点，差不多有一年了，您可以在这里阅读:

[](/what-is-a-programming-paradigm-1259362673c2) [## 什么是编程范例？

### 编程范例和泛型概述

towardsdatascience.com](/what-is-a-programming-paradigm-1259362673c2) 

Julia 采用了编程范式的概念，并以通用编程概念多任务分派为中心。多重分派起源于 80 年代的一种叫做 ML 的小型函数式编程语言。虽然语言本身肯定没有在市场上引起巨大的轰动，但是在代码库中构思的一些天才的想法当然已经在各种各样的现代多范例应用程序中得到了充分的考虑。这就把我们带回到了 Julia，它把多分派和参数多态性泛型编程概念带到了一个全新的水平。Julia 驻留在它自己的新范例中，这个范例是用这种语言构想出来的，即多重分派范例。

坦率地说，Julia 开发人员想出的所有使用多重分派的好主意，比如内部和外部构造函数，以及抽象类型层次结构，都值得单独写一篇文章。然而，考虑到所有这些有趣的特性，Julia 是一种非常独特的编程语言。对我来说，主观上，多重分派感觉是一种非常自然和直观的编程方式。使用更像我们正在使用的类型的属性的方法，使用可以根据传递给它们的类型完全改变它们所做的事情的调用，感觉就像是最自然的编程方法。

除了是一种写起来很棒的语言之外，Julia 很快——真的很快。Julia 经常落后于 C 和 Rust 等低级语言，但写起来仍然简单得可笑。这是老掉牙的格言将再次露出其丑陋的头的地方，最好是最后一次…

> "走路像 Python，跑步像 c . "

你当然可以理解为什么选择这句格言。Julia 代码看起来与 Python 非常相似，而且非常容易使用。更好的是，代码在大多数情况下运行速度几乎和 C 一样快，但为什么这句格言很奇怪？因为 Julia 驻留在与 Python 完全不同的编程范式中，当然！不管怎样，这句格言仍然有意义，可以给人一个很好的关于朱莉娅的想法。既然我们已经理解了为什么 Julia 在许多方面都是无可争议的，以及这种语言是关于什么的，那么让我们深入研究这种语言今年在努力保持其在多个科学学科中的相关性时所面临的一些问题。

# 生态系统

毫无疑问，影响 Julia 新用户和整个 Julia 社区的最大因素是 Julia 的生态系统。举例来说，虽然 Julia 确实有能力调用 Python，并且有许多来自该语言和其他语言的不同移植包，但在许多情况下，从 Julia 调用这些语言是很奇怪的。虽然 PyCall 和其他语言的类似编程接口往往编程良好，并且实际上能够很好地处理数据类型，但用 Python 而不是 Julia 运行代码破坏了使用 Julia 的初衷。最终，这些包会比在 Python 中运行得更慢，所以为什么你会以这种方式使用 Julia 是一个非常奇怪的问题。

如果我要说出一些机器学习包的名字，我会推荐某人在 Julia 中使用，老实说，我很难提供可靠的例子。尽管如此，我还是会尽力为数据科学的每个部分提供一些我个人喜欢使用的优秀包。说到机器学习，对于神经网络，我肯定会推荐 Flux 库。Flux 库实际上非常健壮，并且已经开发了一段时间。

另一个你应该研究的伟大的库是车床，我是它的创造者。车床包括一些很好的机器学习模型，可能会派上用场，更多即将推出。然而，随着下一个长期不中断版本的发布，该包的最大焦点是预处理。在 Julia 中，真的没有那么好的预处理工具，编码器，缩放器，甚至训练-测试-分割函数或对象都很难得到。也就是说，车床包括所有这些，以及一个流水线接口。坦率地说，有很多工作要做，尤其是在一致性和文档部门。然而，随着 0.2.0 的发布，这种情况将在几周内得到改变，这将是软件包功能的一个重大进步。除了车床和通量，我会推荐 Knet 和 GLM。这里是我刚才提到的所有包的链接。GLM 是一个广义线性模型库，而 Knet 是一个鲁棒性较差的神经网络框架。

[](https://github.com/JuliaStats/GLM.jl) [## GitHub-Julia stats/glm . JL:Julia 中的广义线性模型

### Julia 中的广义线性模型。在 GitHub 上创建一个帐户，为 JuliaStats/GLM.jl 的开发做出贡献。

github.com](https://github.com/JuliaStats/GLM.jl)  [## 家庭流量

### Flux 是一个机器学习的库。它“包含电池”，内置许多有用的工具，但也让…

fluxml.ai](https://fluxml.ai/Flux.jl/stable/) [](https://github.com/ChifiSource/Lathe.jl) [## GitHub-chifi source/Lathe . JL:Lathe 是 Julia 的一个包容性预测学习模块

### Lathe 为 Julia 语言带来了完全不同的方法论。创建类型是为了遵守…

github.com](https://github.com/ChifiSource/Lathe.jl) [](https://github.com/denizyuret/Knet.jl) [## GitHub-denizyuret/knet . JL:kou 大学深度学习框架。

### Knet(发音为“kay-net”)是由 Deniz Yuret 和…在 Julia 中实现的 kou 大学深度学习框架

github.com](https://github.com/denizyuret/Knet.jl) 

我也有关于车床和 Flux 的介绍性文章，可以帮助你入门，如果你对 Julia 感兴趣，但不知道从哪里开始，我强烈推荐你。Flux 教程将介绍如何使用 Flux 创建图像分类器，而 Lathe 教程将带您完成预处理、编码和装配随机森林分类器，然后使用 Ubuntu 和 NGINX 构建、序列化甚至部署——这当然是一个简明的教程。

[](https://medium.com/chifi-media/radical-julia-pipelines-5afccc75a061) [## 激进的朱莉娅·皮普勒斯！

medium.com](https://medium.com/chifi-media/radical-julia-pipelines-5afccc75a061) [](/a-swift-introduction-to-flux-for-julia-with-cuda-9d87c535312c) [## Julia 对 Flux 的快速介绍(使用 CUDA)

### 用 Flux 在 Julia 中建立你的第一个渐变模型

towardsdatascience.com](/a-swift-introduction-to-flux-for-julia-with-cuda-9d87c535312c) 

在生态系统方面，让我们考虑一下统计数据。Julia 有一个相当强大和成熟的统计生态系统，实际上我认为大多数开发人员都不会觉得有问题。这个生态系统的唯一问题是，它会有一点分割，你可以预期为一个项目添加许多依赖项，这可能需要大量的测试等等。更进一步，这些统计包不一定是针对数据科学的，但是，Lathe 确实有一个较小的统计库，当涉及到数据科学友好的统计时，它有一些很好的功能来处理数据。这些软件包是 StatsBase，Statistics，Distributions，Lathe 等等。问题的一部分是我提到的，包无处不在，经常互相依赖。我认为这实际上很成问题的部分原因是 Julia 确实有一个叫做预编译的东西，其中包和它们各自的依赖项被预先编译——当每个包有 20 个依赖项要预编译时，这会导致语言中一些非常可怕的启动到执行时间。这实际上是 Julia 用户的一大抱怨，尤其是那些语言新手。

我认为问题在于朱莉娅有一个很棒的打字和扩展系统。使用不同的模块来扩展某个模块的能力和类型是很容易的。这通常是在各种各样的包中完成的。就我个人而言，我一直是一个保持低依赖性的人！在 Julia 中，预编译时间有时非常荒谬。

回到生态系统的话题，我认为 Julia 已经很好地覆盖了数据科学的数据可视化部分。牛虻和 VegaLite 等出色的软件包在多维度显示数据方面做了令人惊叹的工作，有时还提供令人惊叹的交互式可视化，Julia 在绘图方面确实有一些出色的选择。当然还有 Plots.jl 包，它带有 GR 和 Matplotlib。然而，我认为尽管这是新手的必备工具，而且容易使用，但也是一个可怕的选择。预编译时间简直是荒谬的。视觉表现也很糟糕，第一个情节的时间也很糟糕。然而，除了这个列表中的其他两个包，我们还有 Plot.ly 进入该领域。由于这是一个数据科学博客和数据科学出版物，我怀疑我是否需要详细介绍 Plot.ly 到底是什么。这是我提到的所有我喜欢的包的链接。

[](https://github.com/GiovineItalia/Gadfly.jl) [## GitHub-giovine Italia/牛虻. jl:为 Julia 制作的巧妙的统计图表。

### 文档构建状态帮助牛虻是一个用 Julia 编写的绘图和数据可视化系统。它受到了影响…

github.com](https://github.com/GiovineItalia/Gadfly.jl) [](https://github.com/queryverse/VegaLite.jl) [## GitHub-query verse/Vega Lite . JL:Julia 绑定到 Vega-Lite

### jl 是 julia 编程语言的绘图包。该软件包基于 Vega-Lite，它扩展了…

github.com](https://github.com/queryverse/VegaLite.jl) [](https://github.com/JuliaPlots/PlotlyJS.jl) [## GitHub - JuliaPlots/PlotlyJS.jl:用于使用 plotly.js 绘图的 Julia 库

### Julia 接口到 plotly.js 可视化库。这个包使用所有本地资源构建 plotly 图形…

github.com](https://github.com/JuliaPlots/PlotlyJS.jl) 

我还有一篇文章比较了 Julia 的可视化选项，不包括 Plot.ly，因为这篇文章发表时它还没有发布:

[](/julia-visualization-libraries-which-is-best-e4108d3eeaba) [## Julia 可视化库:哪个最好？

### Julia 语言中常用的可视化库概述。

towardsdatascience.com](/julia-visualization-libraries-which-is-best-e4108d3eeaba) 

最后，我们将转向可能是我最不喜欢的实现—数据管理。对于 Julia 中的数据管理，现在有一个典型的选项，data frames . JL。data frames . JL 是一个相当成熟的包，但在 Julia 中处理数据并不是非常成熟。这个包的部分问题是，它像我们接触过的许多其他包一样，与生态系统非常紧密地交织在一起。这意味着即时预编译时间很长，DataFrames.jl 的开发也相当缓慢，因为即使是最大的贡献者也在开发许多其他包，其中一些在 DataFrames.jl 的后端。

不用说，当谈到 DataFrames.jl 的许多依赖项时，DataFrames.jl 没有它们就无法前进。我还认为这个 API 使用起来有点乏味。当然，这是我的主观意见，我仍然喜欢这个包，但肯定有一些方式我想处理我的数据不同于 DataFrames.jl 处理它的方式。考虑到这一点，我实际上正在创建自己的 DataFrames 包 OddFrames.jl。虽然它仍处于早期版本，但我肯定会检查它，因为我最近在发布新版本的车床之前一直在努力，我计划在中包含对 OddFrames 的支持。对于感兴趣的人，这里有 DataFrames.jl 和 OddFrames.jl 的链接。可以把 OddFrames.jl 看作是一个类似熊猫的 API，在某些方面采用了一些 Julian 方法，还加入了一些独特的特性。它混合了典型的函数式编程和面向对象编程，以及一些索引技巧来获得一些非常有趣的结果！

[](https://github.com/ChifiSource/OddFrames.jl) [## GitHub-chifi source/odd frames . JL:面向对象的、基于 dict 的 DataFrames 包！

### jl 是 Julia 的一种新的数据管理和操作包。然而，许多类似的软件包…

github.com](https://github.com/ChifiSource/OddFrames.jl) [](https://github.com/JuliaData/DataFrames.jl) [## GitHub-Julia data/data frames . JL:Julia 中的内存表格数据

### 在 Julia 中处理表格数据的工具。安装:在朱莉娅 REPL，使用 PkgPkg.add("DataFrames")…

github.com](https://github.com/JuliaData/DataFrames.jl) 

此外，如果您对 DataFrames.jl 感兴趣，我有另一篇文章可以帮助您:

[](/how-to-manipulate-data-with-dataframes-jl-179d236f8601) [## 如何用 DataFrames.jl 操作数据

### 关于如何在 Julia with DataFrames.jl 中处理数据的快速教程

towardsdatascience.com](/how-to-manipulate-data-with-dataframes-jl-179d236f8601) 

# 证明文件

Julia 软件的另一个重要问题是文档，有些人可能没有想到。有许多 Julia 包完全没有文档。当然，这使得 Julian 软件与由几个开发人员不断维护的具有高级文档的更成熟的选项相比，非常难以使用。许多在各种领域的语言中普遍使用的 Julian 包，不仅仅是数据科学，只由很少一部分人维护。

也就是说，在自动化文档方面，Julia 确实有一个相当酷的实现。有一个名为 Documenter.jl 的包，它会自动为您创建文档网页，不用说，这太棒了！然而，这也是问题的一部分，因为很容易用 auto-doc 简单地调用 Documenter.jl 而不做任何其他事情——这给包带来了许多问题，因为很难找到您可能实际需要使用的文档。当只使用自动化文档并且没有组织文档网页时，尤其如此。如果你想了解更多关于 Documenter.jl 的知识，你可以看看我在这里写的一篇关于它的文章(是的，我在这里已经建立了一个很有用的目录。):

[](/how-to-automate-julia-documentation-with-documenter-jl-21a44d4a188f) [## 如何用 Documenter.jl 自动化 Julia 文档

### 没有文档，软件是没有用的，所以用 Documenter.jl 把文档的痛苦去掉吧

towardsdatascience.com](/how-to-automate-julia-documentation-with-documenter-jl-21a44d4a188f) 

> 哇，那是一年前的事了！

这种文档的另一个真正酷的地方是，自动化机器人既能生成它，又能提供页面。也就是说，这确实存在于美妙的朱莉娅·哈勃上。这是朱莉娅非常接近的事情之一，并且有许多非常酷的想法，但它们要么正在工作中，要么还没有完全实现。或者在许多情况下，没有充分利用他们的潜力。我有一整篇文章都在谈论朱莉娅·哈勃，以及它有多棒，我真的很喜欢写作——出于某种原因，我真的很喜欢我为它做的艺术作品。无论如何，你可以在这里查看:

[](/juliahub-the-greatest-approach-to-putting-together-packages-and-documentation-707688a472d2) [## JuliaHub:将包和文档放在一起的最好方法

### JuliaHub 如何自动化所有程序员讨厌做的事情。

towardsdatascience.com](/juliahub-the-greatest-approach-to-putting-together-packages-and-documentation-707688a472d2) 

# 这是为了什么？

我认为朱莉娅需要克服的最后一个困难就是找到自己在市场中的位置，从而获得巨大的成功。Julia Computing 明确表示，他们的目标不是取代 Python，而是与 Python 共存。然而，问题仍然存在，这到底意味着什么？正如我在本文中所表达的和过去测试过的，Julia 作为 Python 的后端并不是一个可行的选择。当我们考虑到 Python 与 C 语言有着密切的关系时，情况就更是如此了，C 语言作为后端语言往往运行得更快——坦率地说，虽然比 Python 和 Julia 更难，但它仍然不是一种很难编写的语言。

这就提出了一个问题，

> “朱莉娅到底属于哪里？”

作为编程语言范式的个人爱好者，以及该语言的许多方面，我希望看到它成为我的领域(数据科学)中经常使用的语言。朱莉娅似乎在过去一两年中获得了动力，但似乎也在去年年底达到了顶峰。我们可以从 *PYPL 指数*中看到，Julia 在不如 Perl 受欢迎后，即将屈服于 Haskell。

[](https://pypl.github.io/PYPL.html) [## PYPL 编程语言流行指数

### PYPL 编程语言流行指数是通过分析语言教程在网站上的搜索频率而创建的

pypl.github.io](https://pypl.github.io/PYPL.html) 

然而，了解这个行业——这些事情确实有起有落。也就是说，我确实认为这是语言生态系统改善的关键时刻。这种语言只需要更好的包，更好的文档，甚至更熟悉表达式。来到 Julia 这个奇怪的新世界是很痛苦的，不得不使用像

```
select!(df[!, Not(:B)])
```

从数据帧中删除一列。我的意思是这是一个非常朱利安的做法，但我认为那些来自世界各地的

```
df.drop()
```

可能会觉得这种令人困惑的事情令人不安。我认为朱莉娅在这方面需要有明确的方向感。如果目标是成为数据科学皇冠上的新宝石，那么我们必须建立一个生态系统来支持这样的事情。然而，如果目标是与 Python 一起工作，那么我们需要更健壮的 API 来实现。到目前为止，如果没有为 Julia 的未来开发新软件的目的，这种语言基本上只是一个能够做一些事情的话题。

# 结论

最后一段可能听起来很刺耳，但我只是担心。我真的很爱茱莉亚，也许你还没注意到。我喜欢它教给我的东西，我喜欢它的独特性和它自己令人敬畏的范式，我发现它具有难以置信的表现力。我喜欢这种感觉:语言永远不会碍事，而且我从来没有用其他语言写过任何东西。甚至当我写 Python 的时候，我发现自己有时会写 Julia，或者希望我现在正在写 Julia，这样我就可以做这个或那个来解决我正在处理的问题。

我从 2017 年开始使用 Julia，这是我在这门语言上大约四年的经验，我必须说——我肯定已经爱上它了。然而，我看不到这种语言或其生态系统的明确方向，这一事实让我真正感到不安。茱莉亚感觉就像是我的宝贝，它真的是我的第三代语言。你知道你如何到处编程，并最终找到一个？我曾经认为语言对我来说是 C++，但在进入 Julia 之后，我真正发现了我热爱的编程语言，Julia 当然就是这样。

如果你从事任何科学学科，或者甚至是通用程序员，我强烈建议给 Julia 一次机会。我相信每个程序员都应该努力从每种编程范式中学习至少一种编程语言。考虑到这一点，虽然 Julia 像许多其他现代编程语言一样，是多范式的，并利用了许多通用编程概念，但我认为它是在类似于 Python 的环境中学习函数式、系统化编程概念的一个很好的选择。非常感谢你看我的文章！请支持 Julia 的生态系统和语言，因为它确实是一个奇妙的——但仍然被低估了！祝您愉快！