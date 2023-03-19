# 如何在 Azure 中做数据科学

> 原文：<https://towardsdatascience.com/data-science-azure-8785c8f718ac?source=collection_archive---------44----------------------->

## Azure services 将你的机器学习项目带到云端

![](img/c0c6d3379d7890ea58507b9361dea8cf.png)

照片由[达拉斯里德](https://unsplash.com/@dallasreedy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## ⚠️读了我在⚠️博客中的[原帖](https://anebz.eu/data-science-azure)

如今，在自己的计算机上进行数据科学研究变得越来越困难。当然，普通计算机处理数据探索、分析和可视化没有问题。但是当涉及到模型训练时，除非你拥有一个 GPU 或者你使用的是经典的机器学习模型而不是神经网络，否则你的计算机可能会很难训练一个模型。包括 RAM 和计算长度。

一些人购买更多的内存、更强的处理器或 GPU。其他人使用已经预先训练好的模型，例如来自 Huggingface 的模型，这是一种非常用户友好的与大模型互动的方式。有些人甚至会买个 GPU，训练出大模型，发布在 Huggingface 上玩玩。但大部分人没有这些资源，于是转向云解决方案。

在这篇文章中，我将具体解释如何在 Azure 的帮助下完成不同的数据科学项目。虽然我在 AWS Sagemaker 方面的经验较少，但我将来也可能会在 AWS 上发表另一篇文章。

# 使用通用云资源

当你使用云服务时，你可以简单地使用一个虚拟机，并且只为你使用的时间付费。所有的云提供商都是如此，包括 AWS、Azure 和 GCP (Google)。您可以指定所需的计算能力和 RAM 的数量，当然成本也会相应变化。虚拟机属于 IaaS 类型，即基础架构即服务。云提供商负责底层基础设施:硬件、网络、冷却、通风、安全补丁。你只要拿着服务器，把它用在你的项目上，不用担心这些事情。

这项服务提供了很大的自由和灵活性，你可以安装任何你想要的操作系统，不同版本的软件，但这也意味着你必须知道如何安装库，框架等等。假设您安装了 PyTorch 和其他 Python 库，当您尝试训练您的模型时，您会得到 PyTorch 和另一个库之间的不兼容错误。没有来自 Azure 的人会帮助你，你必须自己找到答案。

这是与 IaaS 的权衡:它不是非常用户友好，您需要自己设置大部分的东西。您需要对您正在构建的工具有所了解。但如果你能做到这一点，你就拥有了最大的自由。

# 使用特定的云资源

近年来，云提供商为不同的应用开发了特定的云资源。大多数提供商提供网络应用、物联网、数据科学、数据库、大数据等服务。这些属于 PaaS 类型:平台即服务。它们比 IaaS 更加用户友好，云提供商为您安装软件，您主要选择您的应用程序类型、您想要的编程语言版本等等。在终端上打字更少，在网站上点击和选择选项更多。

这些服务对用户更加友好，例如，对机器学习知之甚少的人可以创建一个图像分类项目。有大量的教程，帮助按钮和整体指导，使过程尽可能简单和舒适。缺点是，这些服务不太灵活。您可能会发现自己正在使用这些服务中的一个，并且您想要查看您的数据的某个特定部分，但是该服务不允许这样做。确保可以为您使用的每个服务导出数据或模型。

特定的云资源非常适合进行大项目的试运行，或者快速实现小的个人项目。在这篇文章中，我将告诉你关于机器学习的 Azure PaaS 服务。

## 1. [Azure 定制视觉](https://www.customvision.ai/)

自定义视觉是计算机视觉非常好用的服务，是用图像进行深度学习。你至少要上传 20-30 张图片，给它们贴上标签(或者已经上传了标签的图片)，然后训练一个模型。它包括一个非常有用的 REST API，这样你就可以向模型发出请求，并且可以从任何其他应用程序获得预测。

不幸的是，图像的标签网站不是很好，Azure 机器学习(下一节)的网站更具可定制性。但是对于一个快速的解决方案，自定义视觉是非常强大的。

有很多教程，比如关于[物体检测](https://docs.microsoft.com/en-us/azure/cognitive-services/Custom-Vision-Service/get-started-build-detector)。该服务有预先训练的模型，这意味着在上传很少的图像后，模型可以很快适应你的训练数据。

## [2。Azure 机器学习](https://azure.microsoft.com/en-us/services/machine-learning/#product-overview)

Azure Machine Learning 是 Azure 为机器学习提供的一个更加可定制、更加完整的解决方案。它仍然是一种 PaaS 服务，但比定制的 vision 更复杂。

[关于如何使用 AML](https://docs.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-ml) 的文章很多。您可以使用 SDK 或直接通过门户来构建 ML 项目。例如，[这篇文章](https://docs.microsoft.com/en-us/azure/machine-learning/tutorial-first-experiment-automated-ml)解释了如何在 Azure Machine Learning studio 中用无代码 AutoML 训练分类模型。

您可以使用文本构建 NLP 项目，或者使用图像构建计算机视觉项目。AML 包括为您标注数据的标签，以及在其上训练机器学习模型的能力。

在我看来，AML 的一个最好的特点是标签可以是 ML 增强的。当你给图像加标签时，有一个机器学习模型在后台学习你的标签。一旦你标记了足够多的图像，该服务将向你推荐标签，使标记更快更容易。你将不再需要从头开始贴标签，你只需要纠正自动化服务所犯的一些错误，并可能添加丢失的标签。这在进行对象检测项目时特别有用，因为每个图像有许多标签。加速贴标签是很大的帮助。

## 3. [Azure 文本分析](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/)

该服务是用于 NLP 的 API，特别是情感分析、关键短语提取、命名实体识别和语言检测。比如你可以[搭建一个 Flask 翻译 app](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis) 。

# 结论

Azure 有三个主要的机器学习服务:自定义视觉和文本分析非常用户友好，快速且易于使用。前者用于图像，后者用于文本。然而，如果您希望您的项目有更多的功能，它们可能太简单了。在这种情况下，我建议改用 AML。你有更多的权力来决定如何标记数据，以及如何训练数据。我特别喜欢 AML 的贴标服务。即使您不使用 AML 的模型训练方面，您仍然可以在那里标记您的图像，然后再导出。

如果您想要更强大的功能和对应用程序的控制，那么我推荐使用虚拟机。这需要你知道如何自己安装框架，但你将拥有最大的自主权。

感谢您的阅读！我很高兴在推特上聊天