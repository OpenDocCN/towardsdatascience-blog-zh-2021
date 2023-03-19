# 作为一名高中生，我在构建基于 NLP 的产品时遇到的挑战

> 原文：<https://towardsdatascience.com/my-challenges-in-building-an-nlp-based-product-as-a-high-school-student-f0b8f2e85edc?source=collection_archive---------39----------------------->

## 以及我是如何设法解决这些问题，而没有花掉我根本不存在的积蓄中的一毛钱

![](img/8c8793aa13f9438b215a06f67f611269.png)

文森佐·迪乔尔吉在 [Unsplash](https://unsplash.com/s/photos/challenge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 介绍

作为一名 18 岁的学生和 NLP &深度学习的热情爱好者，学习曲线对我来说是真正无价的。我积累并获得了大量的技术技能。然而，这段旅程并不是一条直路；它充满了颠簸、弯路和碰撞。

对于这篇文章，我想强调我所面临的一些主要困难，特别是选择构建一个大量包含人工智能(确切地说，是深度学习和自然语言处理)的辅助项目。虽然这是一个被过度使用的时髦词，可能会给你那些一无所知的朋友留下深刻印象，但我决定利用 NLP &深度学习，因为我看到这是一种解决我试图解决的问题的合适方法。

然而，作为一名破产的学生，深度学习融入初创公司的成本非常高，这也于事无补。因此，本条的目的是:

1.  帮助人们评估将深度学习模型融入他们的辅助项目的好处和挑战
2.  向边项目布道者和独立黑客展示你如何通过一些巧妙的策略来克服这些挑战

事不宜迟，我们开始吧！

# 挑战 1:基础设施成本

![](img/3619d5cd29775b39d1599958296c5fe9.png)

照片由[伊万·班杜拉](https://unsplash.com/@unstable_affliction?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/infrastructure?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

这个问题可能是很多 AI 创业公司面临的最常见也是最麻烦的挑战。训练深度学习模型是一回事；确保它们在生产中正确快速地运行是另一个问题。对于深度学习模型，大多数时候需要 GPU 来获得快速响应时间，如果你不知道，GPU 服务器可能会变得极其昂贵，平均每小时执行 GPU 的成本为 0.8 美元。无法解决的问题，对吧？良好的..

# 我的解决方案:无服务器容器(GCP 云运行)

![](img/819d307a486573d89b32e96d9b9112d2.png)

弗兰克·麦肯纳在 [Unsplash](https://unsplash.com/s/photos/container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

是的，没错，无服务器容器。构建在 [Knative](https://www.signalfx.com/blog/observability-for-knative-workloads-introducing-signalfx-integration-with-google-cloud-run/) 之上，Cloud Run 本质上允许你将所有代码&打包成一个无状态的容器，然后通过 HTTP 请求调用它。与云功能不同，云功能只是作为一个功能来部署，并且语言支持有限，您可以使用您选择的任何语言，并且可以通过简单地将代码封装到容器中来轻松部署。大概 Cloud Run 的伟大之处在于，你每月可以获得**200 万个免费请求。多棒啊。**

通过利用 Cloud Run，我能够轻松地将我的自然语言处理模型及其依赖项部署到 GCP 上，为自己节省了购买 GPU 硬件的巨大成本。

## 限制

当然，云跑也有弊端。首先，它是无服务器的，这意味着如果它在一段时间内不活动。这意味着，如果没有请求需要处理，云运行将在一段时间后终止未使用的容器，导致冷启动仍然可以。

此外，如果你正在将一个大型深度学习模型包装到一个容器中，这显然会导致延迟，特别是如果你在 CPU 上。

## 可能的解决方案

不要害怕，因为你总是可以尝试缓解这些问题！

对于冷启动问题，您可以设置一个云调度程序 cron 作业来定期 ping 端点。或者，您可以设置一个“热容器”，或者一个总是保持一个容器活动的选项。然而，这将产生额外的费用。

不幸的是，对于推理时间，您必须为运行云运行的 GPU 付费。因此，我真心推荐 Cloud Run 来建立一个激情项目/MVP，但是如果你真的对你的产品很认真，那么你将需要投入一些比索。

## 资源

*   [谷歌云运行主页](https://cloud.google.com/run)
*   [比较谷歌的无服务器产品](https://www.splunk.com/en_us/blog/devops/gcp-serverless-comparison.html)
*   [优化谷歌云响应时间的 3 种方法](https://cloud.google.com/blog/topics/developers-practitioners/3-ways-optimize-cloud-run-response-times)

# 挑战 2:为工作寻找合适的模型

![](img/4ab3b6972cc35dfde1fb27c255a6fa38.png)

安德烈·德·森蒂斯峰在 [Unsplash](https://unsplash.com/s/photos/right?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

为了更好地展示我的产品，我的想法是让学生拍摄笔记的图像，让我的模型扫描文本并产生问题。我的挑战是找到一个好的模型来解决问题生成的挑战，这是 NLP 中相对未探索的领域，即使用 NLP 技术从文本块中生成问题的过程。

不幸的是，我找不到太多关于这个主题的博客或文章，也没有太多关于流行的 NLP 库(如 Huggingface)的工作。所以，朋友，你做了什么？

# 我的解决方案:研究论文

是的，我知道，枯燥冗长的论文充满了复杂的词汇和看起来复杂的公式。我不是说它们容易阅读。但是，我确实相信它是一种很好的技能，而且，像大多数技能一样，它可以通过练习和努力获得。

最初，我努力阅读关于自然语言处理的研究论文。然而，随着我开始阅读越来越多的论文，我开始进一步理解所解释的过程和方法，并开始实现论文中描述的问题生成模型的原型。我做到这一点的一个方法是阅读一段文字，并尝试用我自己的话总结它所描绘的内容。你也可以建立一个 NLP 总结模型来做这件事(看，有一个很酷的激情项目！)

这使我不仅能够实现论文中的数学并将其转换成代码，而且我还能够发现 Github 仓库中隐藏的宝石，其中一些研究人员已经编写了他们方法的概念证明，并允许我使用并进一步优化他们的代码以满足我的需求。多酷啊，对吧！

## 限制

当然，研究论文也有局限性，但这些局限性更容易减轻。

首先，值得注意的是，这些是研究人员，而不是软件工程师，因此坚实的原则和代码质量并不总是他们的主要关注点。

此外，一旦他们实现了概念验证，他们往往会离开代码，导致过时的代码&不能被正确使用的包。

最后，一些研究人员忽略了正确地记录他们的代码，所以可能很难在您的机器上得到一个工作的演示。

## 可能的解决方案

对于第一个问题，我发现这不是一个障碍，而是一个挑战，看看我能优化代码有多好，我鼓励你也这样做，因为这将帮助你提高你的编码技能，以及你消化他人代码的能力。

对于第二个问题，您总是可以创建一个虚拟环境，并使用旧的包和代码来启动和运行一个工作演示，稍后您可以相应地更新代码

对于第三点，为什么不通过扫描代码库来尝试自己记录代码呢！每一次挑战都是一次机会！

## 资源

*   [Arxiv](https://arxiv.org/)
*   [关于如何阅读研究论文的好文章](/how-to-read-scientific-papers-df3afd454179)

# 挑战三:模特培训

![](img/020b73cf4210d74c4589303f9c5c3946.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com/s/photos/practice?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我找到了一个日期设置，我相信我可以根据研究论文对我创建的现有模型进行微调和优化。然而，我很快意识到模特培训不是一个便宜的问题；事实上，这比我面临的任何问题都要昂贵。对我来说，期望的结果是能够在 GPU 上训练我的模型，并且不用花一分钱就能节省重量…

# 我的解决方案是:Google Colab

对于深度学习任务，尤其是训练 NLP 模型，Google Colab 是一个真正高超的工具。它本质上是一个托管的 Jupyter 笔记本，不需要设置，有一个优秀的**免费版本**，可以免费访问谷歌计算资源，如…GPU 和 TPUs！我以为我找到了 21 世纪的终极黑客；只需将模型的代码转储到 Colab 笔记本上，训练&在 GPU 上保存重量，并且不花一分钱！唉，要是有那么容易就好了！

## 限制

Colab 最大的限制可能是它在长时间不活动后会断开连接。它应该在 12 小时后断开连接，但我发现它在不活动 2 小时后就断开了我的连接。我想你不会想知道当它在本该节省重量的代码单元前断开我的模型时我是什么感觉！

## 可能的解决方案

通过创建 JavaScript 函数来模拟用户点击，有许多“黑客”方法来欺骗 Colab 认为你是活跃的。然而，即使使用了这些功能，我仍然无法连接。

我认为解决这个问题的三个办法是要么坚持 12 个小时，但我相信你有更好的事情去做！

你也可以使用 Kaggle，我在那里连续训练了我的模型 9 个小时。这可能是最好的解决方案，因为他们也有 GPU 支持。

最后，你可以随时升级到 Colab Pro，这对于他们提供的价格来说是一笔很大的交易！

## 资源

*   [卡格尔](https://www.kaggle.com/)
*   [Google Colab](https://colab.research.google.com/)
*   [使用 JavaScript](https://stackoverflow.com/questions/57113226/how-to-prevent-google-colab-from-disconnecting) 阻止 Google Colab 断开连接

# 结论

本文的目的不是向您展示我在尽一切可能避免支付额外资源方面有多吝啬，而是揭示我在构建基于 NLP 的辅助项目时所面临的一些有趣的挑战。我相信，这些方法肯定可以让那些为了不花太多钱而只是尝试和构建 NLP 辅助项目的人受益，但是，如果你更认真地构建一个产品，那么我建议你为额外的资源付费。

我希望你觉得我的发现很有见地，如果你在使用创新解决方案时遇到任何挑战，请在评论中告诉我！

直到下一次，和平！

![](img/c427a9505c6b599862558dc5f8115899.png)

照片由[克里斯蒂安·威迪格](https://unsplash.com/@christianw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/peace?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄