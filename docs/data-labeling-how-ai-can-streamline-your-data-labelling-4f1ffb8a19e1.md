# 数据标签:人工智能如何简化你的数据标签

> 原文：<https://towardsdatascience.com/data-labeling-how-ai-can-streamline-your-data-labelling-4f1ffb8a19e1?source=collection_archive---------18----------------------->

## Colab 上的 BYO ML 辅助标签工具，并尝试 Datature 的改变游戏规则的无代码数据标签工具

![](img/b0eb4e81393ac86913d3ae94cc6ef0f3.png)

照片由来自 [Pexels](https://www.pexels.com/photo/gray-small-bird-on-green-leaves-70069/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 daniyal ghanavati[拍摄。图片作者。](https://www.pexels.com/@daniyal-ghanavati-10741?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

读完这篇文章后的三大收获:

1.  如何在 Colab 上使用 PixelLib 创建自己的机器学习标签工具？
2.  了解 [Intellibrush](https://datature.io/intellibrush) 如何帮助您更快、更好地完成标签制作，并且无需任何代码。
3.  决定构建还是购买人工智能数据标签工具时的关键考虑因素

# 人工智能+人工注释=快速注释过程

典型的数据科学产品开发流程如下:

![](img/5bc12efaab613345c9401f9d8b891832.png)

典型的产品开发过程。图片作者。

作为整个产品流程的一部分，数据标签占据了大部分时间。当谈到数据标记时，我们使用人工标注器来帮助标记大量的非结构化数据，如图像或文本。不太关心隐私的公司可能会将他们的贴标工作外包给第三方贴标商。然而，如果标记的数据涉及敏感数据，如客户的个人信息或公司知识产权，外包不再是一种选择，公司面临着在内部建立自己的数据标记团队，这带来了一系列全新的挑战。

团队通常由多个数据标注者和一个数据工程师组成，其中标注者负责标注数据，并确保数据被清理并准备好用于模型训练。另一方面，数据工程师必须熟悉机器学习应用的最终用例，以提供贴标机要达到的标签一致性和基准的高度概述，并确保偏差保持在最低水平。他们可能会通过设置标签指南或跨不同的贴标机执行共识检查来建立基线标准，因为毕竟，垃圾进来就是垃圾出去，在这种情况下，像对象检测器这样的机器学习模型的性能在很大程度上取决于标签数据的质量。

但是，人工智能如何帮助标记数据，特别是对于药物研究这样需要高技能和训练有素的研究科学家的用例？这就是人工智能工具发挥作用的地方，传统的统计模型与预先训练的机器学习模型结合使用，以加快注释过程。智能标记工具的一个例子是 IntelliBrush，如下面的视频所示，只需单击一下，即可完成像素完美的掩膜注释，比常规的非智能工具快 10 倍以上！

照片由来自 [Pexels](https://www.pexels.com/photo/gray-small-bird-on-green-leaves-70069/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的丹尼尔·加纳瓦蒂[拍摄。由](https://www.pexels.com/@daniyal-ghanavati-10741?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[数据公司](https://datature.io/)提供的 ML 辅助工具(智能画笔)。作者视频。

# 构建您自己的 ML 辅助标签工具

现在我们已经看到了人工智能工具的能力，让我们试着构建我们自己的 ML 辅助工具。为了说明，我将使用图像分割作为一个例子。同样的概念也可以应用于其他机器学习任务。

这里，我们将使用 Colab 和 PixelLib。下面这篇文章给了我灵感，让我在 Colab 上构建一个图像分割工具，并使用 PixelLib 快速分割图像中的对象。

</how-we-built-an-easy-to-use-image-segmentation-tool-with-transfer-learning-546efb6ae98>  </real-time-image-segmentation-using-5-lines-of-code-7c480abdb835>  

## 概观

这里是整个管道的总结。在普通管道中，人工注释者可以通过标记接口直接检查图像。为了使机器学习成为标签过程的一部分，我们必须添加一个名为 ML 辅助标签模块的模块，该模块使用户能够直接在标签界面上修改机器预测的标签。

![](img/ef1d46c17faa751c146fab739576dc65.png)

我们的 ML 辅助标记工具的简单管道。图片作者。

## 图像分割模型

这个演示将使用 PixelLib，一个用于分割图像和视频中的对象的库。我选择 PixelLib 是因为它易于使用，并提供快速检测，这有助于减少花费在 ML 推理上的时间。

下面是使用 PixelLib 进行推理的代码

```
# install pixellib
pip install pixellib# download model pretrained weights
wget -N 'https://github.com/ayoolaolafenwa/PixelLib/releases/download/0.2.0/pointrend_resnet50.pkl"# instantiate model and load model weights
ins = instanceSegmentation()
ins.load_model("pointrend_resnet50.pkl", detection_speed='rapid')# inference
result = ins.segmentImage(img_path,show_bboxes=False)
```

可以从结果中提取遮罩，并将其应用于原始图像。

![](img/bd9ee16984122d5a6c39e068bd618b48.png)

照片由[像素](https://www.pexels.com/photo/animal-avian-beak-bird-416179/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[皮克斯拜](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## Colab 演示

下面是在 Colab 上构建 ML 辅助标签工具的代码

下面的 gif 展示了运行代码后用户界面的样子。你可以用套索选择器给鸟贴标签。

![](img/38fd6d2bd08963f7be3f836733c7775e.png)

作者 Gif。

或者，您可以单击“ **ml 辅助的**按钮，机器学习模型将自动选择该鸟。然后，您可以使用套索选择器工具继续添加缺失的部分。

![](img/9df534fdfc3a2665aa1a18806ea33416.png)

作者 Gif。

您可以使用演示来了解 ML 辅助工具如何帮助您最小化标记工作，尽管如果您的团队有多个贴标机，这可能不太容易扩展。

根据演示，您可以看到 ML 辅助工具如何帮助您最大限度地减少标记工作。然而，对该工具的进一步实验使我总结出以下缺点。

1.  由于每个成员都必须单独加载和保存他们的原始图像文件和注释，因此实施是不可扩展的，特别是对于内部团队。
2.  PixelLib 使用 Mask R-CNN 模型和 COCO 数据集进行训练，因此可能无法准确检测到您的使用案例特有的自定义对象，如果该工具无法检测到自定义对象或公共数据集中不常见的对象，则无法实现拥有内部标签团队的目的。
3.  无法共享对数据集和标签的访问-如果您将 PixelLib 用于只有您需要访问图像和标签的辅助项目，则 PixelLib 可能会起作用-但是，在数据工程师、贴标签员和项目经理共同努力推动项目成功的组织中，情况往往并非如此。这可能会在模型迭代阶段造成问题，因为调试标签和图像无疑是乏味的。

你会注意到，我高度强调了工具的可用性和协作性——因为 ML 的制作通常是团队的努力，在孤岛中工作通常会导致延迟。请继续阅读，找出我在寻找数据标签工具时的三大考虑因素！

# 无需任何代码的快速注释

一些公司可能没有时间或专业知识来建立自己的标签软件，因此他们寻找现成的解决方案。

我被一家名为 [**Datature**](https://datature.io/) 的公司开发的产品吸引住了。Datature 是一个无代码的 MLOps 平台，为数据标注和模型训练提供基于云的工作流。该公司最近推出了一款名为 [**IntelliBrush**](https://datature.io/intellibrush) 的产品，这是一款人工智能数据标签工具，旨在帮助公司提高标签生产率和效率，以减少开发一个完全可用的计算机视觉模型所需的时间。我有机会试用他们的最新产品，我渴望与你分享我自己的经验。

如果您迫不及待地想试用这款最新产品，请在此注册:

<https://datature.io/intellibrush#sign-up>  

## 什么是 IntelliBrush？

IntelliBrush 是 Datature 的 Nexus 平台上的内置功能。它使用机器学习模型来预测您选择的对象的轮廓。使用此功能，用户只需点击 1-2 次即可快速获得像素完美的遮罩/边界框注释，而不是像使用正多边形工具那样需要在图像的边界上点击多次，或者在误差范围较大的情况下跟踪对象的轮廓。此外，我喜欢 IntelliBrush 的一点是，它会不断调整，以确保它随着时间的推移而改进，我甚至可以使用 Intelli-Settings 来选择粒度级别，当我的图像只包含一个对象或包含多个较小的对象时，这非常有用。点击此处查看各种物品:

以下是 IntelliBrush 自适应模式的一些亮点。作者视频。

如果你是一家公司或小型创业公司，正在寻找一个平台，使用你的定制数据来标记和训练计算机视觉模型，你可能想知道在投资人工智能辅助标记工具之前，你应该考虑哪些因素。以下是我认为需要考虑的 3 个重要标准，以及我为什么认为 Datature 的 IntelliBrush 是一个值得考虑的绝佳选择:

1.  **成本**。每当我们在内部开发软件系统时，我们必须考虑成本。这种成本的一个例子是雇用一组软件工程师开发标签工具，以及一组 UX 设计师设计一个用户友好的界面。此外，如果你希望建立一个像 IntelliBrush 这样的工具，可能有必要雇佣一个数据科学家团队来建立机器学习模型。因此，在选择标签平台时，考虑构建和维护软件的成本是必不可少的。Datature 的 Nexus 平台(有或没有 IntelliBrush)计划迎合各种业务，无论您是刚刚开始使用计算机视觉模型，还是有一个专门的贴标机团队正在寻找一个平台来处理大量的数据标签任务。他们确实有一个免费计划，附带了对 IntelliBrush 的有限访问，这对那些喜欢“先试后买”的团队来说非常好。
2.  **易用性**。市场上已经有 ML 辅助的解决方案，但有些需要你开发自己的机器学习模型来支持这些功能，这是另一个挑战，可以很容易地将你的开发时间表推迟几周或几个月。另一个需要考虑的重要事实是，专业的人工注释者往往时间紧张，没有能力花一整天的时间来标记数据——这就是为什么尽可能精简的界面会大大提高效率。
3.  **灵活性**。根据您的团队正在开发的模型类型，标注工具还应支持复杂多边形和边界框。此外，该工具不应该局限于常见对象，而是底层算法应该能够检测出以前从未见过的新数据。这是 IntelliBrush 的一个亮点，因为不需要预先训练，这意味着它也可以在任何自定义对象上工作！正如我上面提到的，演示只展示了 IntelliBrush 如何分割图像，但也可以支持其他几个任务，例如为对象检测模型生成边界框。

## 如何使用 IntelliBrush？

1.  登录您的[账户](https://datature.us.auth0.com/u/login?state=hKFo2SBhNXVtQV9YNkxxaUJuc2dPX1JWeUdPb2ZnOEJqN0I3c6Fur3VuaXZlcnNhbC1sb2dpbqN0aWTZIFdOcUlMbldPYUpraF9xN2xxbDZ6WnhIUnEzWDNFUXg3o2NpZNkgaXg2endaS2hkVmRsRlFLdjdMZjBvV2h3SldhOUtDYzU)，创建一个新项目并上传您的图片
2.  打开基于 web 的注释器并创建您的第一个标签。
3.  选择右侧面板上的 IntelliBrush 或使用热键“T”。(IntelliBrush 应该在注册时激活。如果没有，可以在这里[申请](https://datature.io/intellibrush#sign-up)提前接入。)
4.  通过左键单击感兴趣对象的中心，该对象将立即被屏蔽。
5.  如果您对生成的遮罩不满意，可以通过右键单击来表示“不感兴趣”的区域，从而对遮罩进行编辑，并且可以根据需要进行多次细化。
6.  一旦您对遮罩满意，您可以按空格键提交标签。

看看下面的视频，看看它是如何工作的。我还用这个工具尝试了不同的图像！

如何在各种情况下使用 IntelliBrush 的示例。作者视频。

# 结论

如果您的团队经常因缺乏高质量的标注数据而停滞不前，那么 ML 辅助标注工具可能有助于提高您团队的生产力。如果你正在寻找一个快速准确的“现成”工具，IntelliBrush 是一个理想的候选人，因为它不需要事先的模型训练，甚至可以处理从未见过的图像。此外，该公司正在积极改进该工具，并计划在现有的协作标签功能的基础上继续进行 QA 检查的计划发布。最后，如果你对构建自己的计算机视觉模型感兴趣，这里有一些来自 [Datature](https://datature.io/) 的视频，可以帮助你使用 Nexus 平台启动自己的计算机视觉项目——所有这些都没有代码。

# 关于作者

Woen Yon 是新加坡的一名数据科学家。他的经验包括为几家跨国企业开发先进的人工智能产品。

Woen Yon 与少数聪明人合作，为当地和国际初创企业主提供网络解决方案，包括网络爬行服务和网站开发。他们非常清楚构建高质量软件的挑战。如果你需要帮助，请不要犹豫，给他发一封电子邮件到 wushulai@live.com。

他喜欢交朋友！请随时在 [LinkedIn](https://www.linkedin.com/in/woenyon/) 和 [Medium](https://laiwoenyon.medium.com/) 上与他联系

<https://laiwoenyon.medium.com/> 