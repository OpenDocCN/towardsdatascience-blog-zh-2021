# Julia 网站开发的 4 个酷包

> 原文：<https://towardsdatascience.com/4-cool-packages-for-web-development-in-julia-cb99b0059025?source=collection_archive---------17----------------------->

## 用 Julia 比较不同 web 开发包的优缺点

![](img/08204e42c42d2f592253635f3061853e.png)

(src =[https://pixabay.com/images/id-921039/](https://pixabay.com/images/id-921039/)

# 介绍

W 随着 Julia 语言的广泛应用，尤其是在科学计算领域，web 开发很可能不是该语言的首要考虑因素。毕竟，有很多语言在 web 开发方面做得很好，而 Julia 真的不是其中之一。尽管如此，Julia 确实有一个中等规模的网络生态系统。虽然这些包中的大多数做更多的以数据为中心的事情，但是在 Julia 生态系统中有一些很棒的包可以用于传统的 web 开发。

有几个不同的原因可能会让你想在 Julia 中开发一个网站或页面。首先，总是有希望从传统的网络浏览器中与 Julian 数据、方法和工具进行交互的可能性。一个很好的例子就是用于分析的交互式仪表板。更进一步，有些人可能希望根据请求产生管道回报。幸运的是，Julia 确实有一个不错的生态系统，支持数据科学中的许多常见任务。

# №1:精灵

[](https://github.com/GenieFramework/Genie.jl) [## GenieFramework/Genie.jl

### Genie 是一个全栈 MVC web 框架，它为开发现代 web 提供了一个精简高效的工作流…

github.com](https://github.com/GenieFramework/Genie.jl) 

Genie.jl 是 Julia 中令人敬畏的 web 开发框架的一个经典例子。Genie 的伟大之处在于它已经存在了很长时间，并且确实有机会在某种程度上成熟。这个包本身非常可靠，通常用于在 Julia 中部署端点和处理 web 上的数据。然而，精灵当然不仅仅只有这些能力。

Genie 还使用 Project.toml 文件来处理在 gate 之外生成的依赖关系。这与列表中的许多其他包有很大的不同，因为 Genie 从一个项目模板开始，然后在这个模板上进行构建。真正将 Genie 与我们将要介绍的其他选项区分开来的是它的可靠性、多功能性和可部署性。我发现 Genie 项目非常容易归档。此外，该文档即使不是列表中最好的，也是最好的之一。

当然，Genie 并非没有缺陷。我在 NGINX 中使用 Genie.jl 时遇到了一些问题。有时 NGINX 无法判断是否有一个精灵服务器在运行，这至少是很乏味的。如果精灵安装包含在 Docker 映像中，这将变得更加乏味，因为有一个抽象层需要处理，很难导航。如果您想立即开始使用 Genie，我已经写了一篇文章，全面介绍了使用机器学习模型和 NGINX 部署端点。本文还包括使用 NGINX、Bash 和 Supervisors，因此如果您对这些信息感兴趣，它当然值得一读，您可以在下面这样做:

[](/a-z-julia-endpoint-with-genie-a2e0c2f1c884) [## 带精灵的 A-Z Julia 端点

### 介绍如何使用 Genie web-framework 的虚拟环境设置。

towardsdatascience.com](/a-z-julia-endpoint-with-genie-a2e0c2f1c884) 

# №2:虚线

[](https://github.com/plotly/Dash.jl) [## plotly/Dash.jl

### 从 Dash 的 v1.15.0 开始，Julia 组件可以与 Python 和 R 组件串联生成。有兴趣获得…

github.com](https://github.com/plotly/Dash.jl) 

最近，Dash.jl. Plot.ly 发布了一个 Julia 版本的绘图库(耶！)并发布了他们的 Plot.ly Dash 等价物。最棒的是，现在您可以在 pure Julia 中完整地使用 Plot.ly 生态系统。

在这个包之前，在 Julia 中确实没有一个好的方法来制作交互式仪表板。虽然我们很快将涉及的包肯定可以促进其中一些的创建，但这样做的难度意味着通常项目更侧重于 Dash 而不是分析。不用说，这可能会给分析带来问题。

当然，这个端口正是你所期望的 Plot.ly。这个包是健壮的，允许很容易地集成 Plot.ly 的图形，这使得在 Julia 中制作漂亮的仪表板变得非常容易。除了获得您熟悉和喜爱的 Plot.ly 功能，您还将获得 Plot.ly 拥有的令人难以置信的文档。这使得这个包比其他包更胜一筹。虽然 Genie.jl 在这方面非常接近，但我认为 Plot.ly 可能只是拿走了它的文档。

# №3:互动

[](https://github.com/JuliaGizmos/Interact.jl) [## JuliaGizmos/Interact.jl

### 玩茱莉亚代码，分享乐趣！这个包是一个基于网络的小部件的集合，你可以用它来:做快速…

github.com](https://github.com/JuliaGizmos/Interact.jl) 

另一个很棒的 web 开发包是 Interact.jl。这个包本身通常用于给笔记本增加交互性。也就是说，代码也可以通过 Mux.jl 包放入 web 服务器设置中。当涉及到与 Julia 生态系统合作时，Interact.jl 也有一些非常好的能力。对于 Plots.jl 来说尤其如此，它遵循了与 Plot.ly 的 Dash 与 Plot.ly 的绘图库的交互类似的路径。

也就是说，虽然我不会说交互对于网络开发来说是一个糟糕的选择，但以我的经验来看，它确实很难使用。这个包的一个特别的缺陷是它的文档，它在太少以至于包不可用和仅仅足够完成一些事情之间画了一条细线。因此，这个包很难使用。在使用这个包时，我甚至发现自己在查看源代码，试图了解关于这个包的更多信息，这当然是一个程序员不想做的事情。

此外，Mux.jl 的功能可能会有很多问题。如果可能的话，我肯定会避免这个包。问题是，这并不总是可能的，所以虽然从我的主观观点来看，这是一个值得了解的包，但对于生产中的任何东西来说，这都是一个很难推荐的包。

Interact.jl 的一个优点是它具有难以置信的可扩展性。该模块使用 Cairo 图形库，这是 Julia 中显示各种图形的一个非常流行的选择。这很棒，因为这意味着 Interact 能够立即查看来自完全不同的包的类型，这些包也使用 Cairo。您还可以创建自定义小部件来添加到表单中，这些小部件相对容易操作，并且遵循可组合的声明性语法。

# №4:眨眼

[](https://github.com/JuliaGizmos/Blink.jl) [## JuliaGizmos/Blink.jl

### Blink.jl 是围绕电子的 Julia 包装器。它可以在本地窗口中提供 HTML 内容，并允许…

github.com](https://github.com/JuliaGizmos/Blink.jl) 

本文中我想介绍的最后一个包是完全不同的东西，Blink.jl。与列表中的其他选项不同，Blink 不提供 web 服务器来交付基于 web 的内容。相反，Blink 更侧重于本地系统上的图形用户界面。

尽管如此，这个包还是派上了不少用场。我曾经想制作一个 GUI 来与我连接到计算机上的 CO 传感器进行交互，为此我使用了 Blink.jl。像这个列表中的其他一些包一样，Blink 的文档也足够好。虽然它不像 Interact 的那么可怕，但它甚至无法接近 Genie 的美丽，尤其是 Plot.ly 的 Dash。

尽管如此，我仍然认为这是一个非常好的工作和学习包。这个包确实派上了用场，这也是很多 Julia 项目依赖这个包来完成工作的部分原因。和 Interact 一样，这个包也使用了 Cairo。这提供了所有与 Interact 相同的兼容性优势，因此它肯定是值得研究的东西。

# 结论

Julia 生态系统中既有成熟的、已建立的 web 开发包，也有一些相对较新的、可能更难使用的包。幸运的是，有一些不同的例子，它们都有各自的缺点和优点。有些还有独特的用途，比如 Blink.jl。非常感谢您阅读本文，希望您喜欢！