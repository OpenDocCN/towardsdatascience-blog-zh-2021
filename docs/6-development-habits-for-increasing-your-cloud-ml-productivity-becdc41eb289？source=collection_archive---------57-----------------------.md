# 提高云 ML 生产力的 6 个开发习惯

> 原文：<https://towardsdatascience.com/6-development-habits-for-increasing-your-cloud-ml-productivity-becdc41eb289?source=collection_archive---------57----------------------->

## 回归基础:重新思考云计算时代的开发最佳实践

![](img/31c535964698399932c95945e9824970.png)

费利佩·费塔多在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我的[以前的帖子](https://chaimrand.medium.com/)主要是技术性的，涵盖了一系列关于云培训和高级 TensorFlow 开发的主题。这个帖子不一样。你可能会认为这更像是一篇观点文章。

在我的职业生涯中，当讨论软件开发实践的话题时，我很少看到比这更紧张的了。即使是最内向的工程师也会突然活跃起来。同事们会陷入激烈的争论，否则他们会步调一致地工作。我曾与[牛仔程序员](https://en.wikipedia.org/wiki/Cowboy_coding#:~:text=Cowboy%20coding%20is%20software%20development,tools%2C%20frameworks%20and%20coding%20style.)和 [Scrum](https://en.wikipedia.org/wiki/Scrum_(software_development)) masters 一起工作过，与那些将[敏捷](https://en.wikipedia.org/wiki/Agile_software_development)作为生活方式的人一起工作过，与那些宁愿退出也不愿再参加一次[站立会议](https://en.wikipedia.org/wiki/Stand-up_meeting#:~:text=A%20stand%2Dup%20meeting%20(or,to%20keep%20the%20meetings%20short.)的人一起工作过，与那些将他们的功能名称大写的人一起工作过，与那些将这样的实践视为死罪[的人一起工作过。](https://www.dictionary.com/browse/capital-offense)

说实话，大多数常见的开发实践都植根于坚实的常识。虽然开发人员可能对是否以及如何采用 [Scrum](https://en.wikipedia.org/wiki/Scrum_(software_development)) 有强烈的感觉，但是现代开发团队应该能够快速适应变化的环境，这是常识。虽然具体的细节可能有争议，但是团队采用[编码风格](https://en.wikipedia.org/wiki/Programming_style#:~:text=Programming%20style%2C%20also%20known%20as,help%20to%20avoid%20introducing%20errors.)以增加代码可读性和可维护性是常识。虽然[测试驱动的开发](https://en.wikipedia.org/wiki/Test-driven_development)可能看起来势不可挡，但是在不同级别和不同开发阶段引入测试是常识。当在团队中工作时，执行[持续集成](https://en.wikipedia.org/wiki/Continuous_integration)包括自动化集成测试是常识。即使是最好的开发人员有时也会犯错误，在他们被推到主存储库并最终毁掉每个人的一天之前抓住他们是常识。进行[同行代码评审](https://en.wikipedia.org/wiki/Code_review)是常识，可能只是防止像内存泄漏或数据类型溢出这样的小疏忽搞垮你的公司。

```
Never underestimate the capacity of a person, no matter how stupid, to do something genius. More importantly, never underestimate the capacity of a person, no matter how genius, to do something monumentally stupid. --Chaim Rand
```

另一个基于常识的主张是，我们的开发习惯应该适应我们正在进行的项目的细节，包括团队、时间表，以及重要的是**开发环境**。这是事实，尤其是在云中训练机器学习模型时。

# 云 ML 的优势

在云中训练机器学习模型的趋势越来越强，云服务提供商为你能想象到的任何机器学习相关任务推出了不断扩展的服务。不难看出云中培训的吸引力。

构建现代机器学习基础设施可能很难。首先，是硬件基础设施。GPU 和替代的机器学习加速器**很贵**，如果你想保持最新最好的状态，你需要定期更新它们。由于您可能希望在多台机器上分布您的训练，并且可能希望一次运行多个训练实验，因此您可能需要多个不同规格的 GPU。机器学习通常需要复杂且可扩展的存储基础设施来存储不断扩展的数据集，以及支持数据存储和训练机器之间的高流量的网络基础设施。

然后是软件基础设施。要让您的培训发挥作用，需要适当的 BIOS 设置、定期的驱动程序更新、复杂的访问权限管理以及仔细的环境变量配置。您通常会需要一个很长的软件包列表，这些软件包可能会有相互冲突的依赖关系，您需要对它们进行分类。您可能需要 Docker 或其他一些虚拟化层，这带来了一系列全新的潜在问题。

另一方面，当你在云中训练时，生活是容易的。硬件、存储、网络和软件基础架构都已准备就绪，随时可以使用。您有各种各样的 GPU(和其他加速器)可供选择，以及不同类型和版本的软件框架。在云中，你可以自由地扩展，运行尽可能多的并行实验，也可以扩展独立的训练课程，在许多 GPU 核心(或其他加速器)上并行运行。云中的存储容量几乎是无限的，让您不必再为满足不断增长的存储需求而头疼。此外，云产品通常包括基于其定制硬件或网络拓扑的独特优化。除了已经在管理自己的数据中心的大型组织之外，创建自己的经济高效的基础架构，并具有云中提供的相同的灵活性和复杂性，几乎是不可思议的。查看[此处](https://medium.com/@julsimon/making-amazon-sagemaker-and-tensorflow-work-for-you-893365184233)了解更多将培训迁移到云的优势和挑战。

[](https://julsimon.medium.com/making-amazon-sagemaker-and-tensorflow-work-for-you-893365184233) [## 让 Amazon SageMaker 和 TensorFlow 为您服务

### 这是 Mobileye 的机器学习算法开发人员 Chaim Rand 的客座博文。它构建于 AIM410R 之上…

julsimon.medium.com](https://julsimon.medium.com/making-amazon-sagemaker-and-tensorflow-work-for-you-893365184233) 

## 管理成本

虽然**成本**可能是使用云 ML 服务的主要考虑因素之一，但很容易看出，如果没有**的适当治理**，成本也可能成为主要陷阱。我相信，每个开发人员的 DNA 中都有一种与生俱来的倾向，那就是使用最大、最新、最快和最强的计算资源。如果我们可以使用一个强大的(而且*昂贵的* ) GPU(或其他加速器)系统，为什么要满足于一个更便宜的呢？如果我们可以在 1024 个内核上分布我们的培训，为什么要满足于 8 个内核呢？如果我们可以并行运行 1000 个实验，为什么要把自己限制在 4 个呢？如果我们能进行 20 天的训练，为什么要限制在 1 天呢？

显然，为了确保向云的成功过渡，管理人员必须为如何以及何时使用不同的云服务制定明确的政策。应定义指导原则，包括要使用的培训资源的数量和类型、培训课程应持续多长时间、何时支付云资源的全价以及何时使用更便宜的“低优先级”资源，如 [spot 实例](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc)或[可抢占虚拟机](https://cloud.google.com/compute/docs/instances/preemptible)(冒着作业提前终止的风险)。应监控使用模式，并不断进行调整，以最大限度地提高生产率。

虽然定义和执行这样的策略是管理人员的责任，但是我们，开发人员，有责任使我们的开发过程适应这个新的工程时代。我们需要调整我们的习惯，以便最大限度地利用云资源，并最大限度地减少浪费、闲置时间和资源利用不足。

# 提高云培训生产力的开发习惯

为了最大化效率，我想提出 6、**常识**，我认为在云中训练机器学习模型时应该强调的开发实践。

## 1.在云中运行之前进行本地测试

众所周知，我们编写的几乎*的应用程序从来不会按照我们期望的方式运行，直接开箱即用。我们经常会访问未定义的变量，使用错误的数据类型，越界访问列表，错误地使用 API，等等。在云中解决这些问题是低效、耗时和浪费的。常识告诉我们要建立一个本地的(基于 CPU 的)环境，其中有一小部分训练数据，在这个环境中，您要尽可能地验证一切都按预期运行。*

## 2.让您的调试工具和技术适应云培训

不幸的是，在云中调试失败的应用程序，很少仅仅是在调试模式下运行您喜欢的 IDE。同时，我们快速识别和修复云运行中的错误的能力会对成本和生产率产生巨大影响。常识要求重新审视我们的调试过程，并使它们适应新的开发范式。应该对代码进行修改，以支持错误的快速再现。例如，保存中间模型检查点可能会缩短重新生成的时间，捕获输入张量可能有助于识别导致错误的输入和权重的精确组合，以及用于运行更详细的模型训练的专用调试标志可能有助于查明问题的根本原因。有关有效 TensorFlow 调试的更多详细信息，请参见此处的。

[](/debugging-in-tensorflow-392b193d0b8) [## TensorFlow 中的调试

### 如何在不失去理智的情况下调试 TensorFlow 训练程序

towardsdatascience.com](/debugging-in-tensorflow-392b193d0b8) 

## 3.进行持续、深入的性能分析和优化

为了最大限度地利用您的培训资源，您应该将绩效分析和优化作为您发展过程中不可或缺的一部分。通过分析训练执行的速度(例如，以每秒迭代次数来度量)和我们利用系统资源的方式，我们寻求确定提高效率和降低总成本的机会。虽然*任何未被充分利用的*培训资源都是提高培训效率的潜在机会，但首先也是最重要的是，对于 GPU(或其他培训加速器)，通常是系统中最昂贵和最强大的资源。你的目标应该是尽可能提高 GPU 的利用率。

典型的性能优化策略包括反复执行以下两个步骤，直到您对训练速度和资源利用率感到满意:

1.概述培训绩效，以确定流程中的瓶颈和未充分利用的资源

2.解决瓶颈并提高资源利用率

有许多不同的工具和技术来进行性能分析，这些工具和技术根据开发环境的具体情况而有所不同。关于分析 TensorFlow 模型性能的更多细节，请参见[此处](/tensorflow-performance-analysis-314b56dceb59)。

[](/tensorflow-performance-analysis-314b56dceb59) [## 张量流性能分析

### 如何从您的培训资源中获得最大价值

towardsdatascience.com](/tensorflow-performance-analysis-314b56dceb59) 

## 4.在代码中引入容错

想象一下，在 32 台 GPU 机器上运行一个高度分布式的培训会话两天，结果只有一台机器在模型似乎已经收敛时崩溃。虽然机器也可能在本地培训环境中发生故障，但有两个原因可以解释为什么在云中这可能是一个更大的问题。首先是分布式培训更加普遍。一般来说，我们训练的机器数量越多，我们就越容易受到系统故障的影响。第二个原因是，降低云中培训成本的最常见方法之一是使用“低优先级”资源，(如[点实例](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc)，或[可抢占虚拟机](https://cloud.google.com/compute/docs/instances/preemptible))，如果出现更高优先级的请求，这些资源容易被提前终止。因此，将容错引入到您的开发需求中只是常识。

你应该做的第一件事是确保你定期捕获你的机器学习模型的快照(也称为检查点)，以便在提前终止的情况下，你可以从最近捕获的模型恢复，而不是从头开始训练。捕获快照的时间间隔应通过权衡捕获快照的开销与在出现故障时您将损失的预期培训时间来确定。

另一件要考虑的事情是实现一种从单个系统的故障中恢复的机制，而不需要重新开始培训会话。在分布式培训环境中，实现这一点的一种方法是使用 [Elastic Horovod](https://horovod.readthedocs.io/en/stable/elastic_include.html) ，它支持根据系统可用性动态扩展工作人员的数量。

## 5.对您的培训工作进行自动监控

监控学习过程指的是在训练期间跟踪不同指标的艺术，以便评估训练如何进行，并确定调整哪些超参数以改进训练。虽然监控学习过程是训练机器学习模型的关键部分，但一般来说，这在云中尤为重要，因为我们的目标是尽可能减少浪费。一旦我们发现一个失败的训练实验，或者一个已经停止学习的训练环节，我们的目标应该是尽快停止训练，以避免浪费。

因为我们不可能在一天的每时每刻都跟踪每一个训练实验，常识告诉我们应该尽可能自动监控。我们可以编程规则来检测常见的训练失败，例如爆炸或消失梯度、非递减损失等，并采取适当的行动，无论是终止训练还是调整一些超参数。

查看[此处](/upgrade-your-dnn-training-with-amazon-sagemaker-debugger-d302fab5ee61)和[此处](/the-tensorflow-keras-summary-capture-layer-cdc436cb74ef)了解更多关于 TensorFlow 中监控和自动监控的主题。

[](/upgrade-your-dnn-training-with-amazon-sagemaker-debugger-d302fab5ee61) [## 使用 Amazon SageMaker 调试器升级您的 DNN 培训

### 如何在云中培训时提高效率并降低成本

towardsdatascience.com](/upgrade-your-dnn-training-with-amazon-sagemaker-debugger-d302fab5ee61) [](/the-tensorflow-keras-summary-capture-layer-cdc436cb74ef) [## TensorFlow Keras 汇总捕获图层

### 在以前的帖子中，我告诉过你我在 Mobileye 的团队(正式名称是 Mobileye，一家英特尔公司)是如何…

towardsdatascience.com](/the-tensorflow-keras-summary-capture-layer-cdc436cb74ef) 

## **6。采用先进的超参数调谐技术**

尽管无论培训地点在哪里，成功的培训通常都需要超参数调整，但我们希望最大限度地减少云中的浪费，这要求我们尽可能以最具成本效益的方式进行调整。虽然我们可能满足于在本地环境中进行手动或网格搜索调优，但我们应该致力于在云中使用更高级的技术，这些技术已经被证明可以更快地收敛并降低调优阶段的总体成本。同样，有许多超参数调整的解决方案，根据训练环境的不同而不同。努力选择一个提供现代的、基于贝叶斯的算法支持的(例如 [Ray Tune](/fast-hyperparameter-tuning-at-scale-d428223b081c) 和[Amazon SageMaker hyperparameter tuner](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html))。

# 摘要

向云计算的过渡是一种范式的转变，它要求我们的开发过程朝着提高生产力和效率的目标调整和收紧。虽然细节肯定会因团队和项目而异，但核心原则是常识。成功掌握在我们手中。