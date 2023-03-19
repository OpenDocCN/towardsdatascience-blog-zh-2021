# 使用 Python 在可视化中研究图像和自定义颜色

> 原文：<https://towardsdatascience.com/investigating-images-and-customizing-colors-in-visualizations-with-python-5226c834cb65?source=collection_archive---------33----------------------->

## 使用 Alteryx Designer 中的图像配置文件工具快速了解您的图像数据集，然后添加几行 Python 代码，使用自定义调色板创建数据可视化

![](img/56e76ce3cdaff31ccbb048740ecc7e76.png)

[*萨姆比斯利*](https://unsplash.com/@sam_beasley?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *上* [*下*](https://unsplash.com/s/photos/colors?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

看电视侦探注意细节并通过环顾犯罪现场来破案总是很有趣。他们的观察力是戏剧性的，令人印象深刻(尽管有时他们会面临一些幽默的挑战，这取决于节目)。

![](img/ef027ae25374537aff360b3b21499215.png)

图像通过 [GIPHY](https://media.giphy.com/media/XLIWxLU3KsmNM3Wt8w/giphy.gif?cid=ecf05e47osgzaefdzahe8iob9yiy107zldm26750bu4xhmv7&rid=giphy.gif&ct=g)

新的[图像轮廓工具](https://help.alteryx.com/20213/designer/image-profile)也非常擅长快速观察图像的细节。Alteryx Intelligence Suite 的计算机视觉工具组的新成员可以快速分析图像，使您可以将对图像的见解整合到更大的工作流程中。您可以获得关于每张图像的格式、颜色、拍摄地点的信息(如果 [EXIF](https://photographylife.com/what-is-exif-data) 数据可用并包含 GPS 细节)，以及关于图像的各种汇总统计数据。(后者的一个例子是找到图像像素值的标准偏差，以量化图像中的对比度水平。)

你可以将这些信息用于各行各业，从农业到零售业到制造业。也许你想知道哪种产品颜色在你的顾客中最受欢迎。也许你想通过图像的附加位置数据来绘制图像，以寻找地理模式，并使用 Designer 的[分配](https://help.alteryx.com/20213/designer/demographic-analysis)工具来构建人口统计细节。也许您想在推荐引擎或基于图像的搜索系统中使用图像数据。您可以从图像中收集的“证据”可以增强许多项目。

![](img/96206db0d55146e4f59231de09a43123.png)

*图片经由* [*GIPHY*](https://media.giphy.com/media/1AdZhmcdYrLVopELHh/giphy.gif?cid=790b761143b03a9507b7051e0459e646db7fca60ab7c894b&rid=giphy.gif&ct=g)

无论您的动机是什么，我将向您快速介绍该工具及其选项，并且我将为我们的 Python 爱好者提供一个奖励:一种使用 Stitch Fix 发布的开源 Python 包为图像最常见的颜色指定人类友好名称的方法。另外，我们将探索一种方法来生成这些颜色的自定义可视化。开始调查吧！

![](img/9fa3058b309492abea1e5f935247d93d.png)

*图片经由* [*GIPHY*](https://media.giphy.com/media/YOjW8IpbWneVLNkRhe/giphy-downsized.gif?cid=ecf05e47yltlts8bnbt4fckygcat5bmyhj82d0qpkc94ipik&rid=giphy-downsized.gif&ct=g)

# 如果鞋子合脚:准备服装数据集

在这个演示中，我使用了 3781 张不同服装在普通背景上的图片。我使用一个目录工具将图像引入工作流，然后使用一个 Regex 工具从每个目录的名称中提取服装的类型，认为这可能对以后的排序和分析有用。然后，我使用一个图像输入工具开始工作流程的图像部分。

在进入剖析步骤之前，我使用了一个[图像处理工具](https://community.alteryx.com/t5/Data-Science/Picture-Perfect-Inside-Image-Processing/ba-p/767828?utm_content=815532&utm_source=tds)。我对这些图像的初步探索表明，令人惊讶的是，很大一部分图像以不同深浅的灰色为主色调。许多衣服是在灰色背景上拍摄的。我使用图像处理工具将图像裁剪为中心区域的 200 像素的正方形，以试图将焦点集中在实际拍摄的服装上。这不是一个完美的策略。裤腿和鞋子之间的间隙可能会稍微扭曲最终结果。这个过程中的一个[物体检测](https://machinelearningmastery.com/object-recognition-with-deep-learning/)步骤有助于将分析集中在衣服上。但是在增加这一步后，我看到了更多“丰富多彩”的不同结果，所以这似乎有所帮助。

![](img/23371fba531a74285f7bb466aaa5aedd.png)

*图像通过* [*GIPHY*](https://media.giphy.com/media/YRVgSMuu8QBuM9oKxN/giphy.gif?cid=ecf05e47wfrlat8ouef1jswgtubgzy520bjaewnxxrjvkma5&rid=giphy.gif&ct=g)

# 询问图像:工作中的图像轮廓

最后，调查者进入场景:图像轮廓工具，它需要最少的配置。只需告诉它哪个字段包含您的图像，以及您想要为每个图像检索哪个(些)简档，或此处[描述的一组细节](https://help.alteryx.com/20213/designer/image-profile)。

![](img/34b087acbc279ab6e5189af949b0349c.png)

作者图片

运行工作流会提供每个图像的关键细节。基本配置文件包括如下所示的字段以及更多内容。下面是显示图像中最常见颜色的字段，以 RGB 和十六进制格式表示，以及暗像素和亮像素的数量。

![](img/01abda7e6a118e3a12472c393c748b9f.png)

作者图片

# 自我解释:将颜色结果转化为人类术语

从来没有人说:“我最喜欢的颜色是#afada6！”或者“我想要一件[37，150，190]色调的衬衫。”那些是什么颜色？

您可能对 RGB 和/或十六进制代码感到满意。例如，您可以使用这些 RGB 细节来聚类图像，或者使用最近邻来匹配新图像。但是如果把你的颜色结果翻译成人类术语并想象它们的频率会有所帮助，请继续阅读。

![](img/ca05d0afdea2b919faa13014960465ab.png)

*图片经由* [*GIPHY*](https://media.giphy.com/media/1lyPd9XMwZpfUS5hxp/giphy-downsized.gif?cid=ecf05e47fc1plxh44z8sdisz16tqyzwwjyedcbocezgwnlqq&rid=giphy-downsized.gif&ct=g)

像往常一样， [xkcd](https://xkcd.com/1882/) 给我们指路。由网络漫画的创作者进行的颜色命名调查的结果被集成到开源 Python 包 colornamer 中并得到增强，该包由 Stitch Fix 的数据科学团队开发。这些数据科学家尤其需要确保他们在推荐服装时对颜色进行了细微的区分。为此，他们创建了一个颜色层次，具有特定的、人类可读的名称和不同级别的区别，调色板选项的大小从 900 多种命名颜色到两种选项(“颜色”或“中性”)。他们的过程和调色板的所有细节都显示在 [the Stitch Fix 博客帖子](https://multithreaded.stitchfix.com/blog/2020/09/02/what-color-is-this/)中，同时还有一个颜色的交互式图形。

使用 colornamer 和 Python 工具中的几行代码，我能够为每张图像最常见的颜色生成友好的名称，并将其添加到我的数据集中。例如，看看下面的图片和它的主色。

![](img/df55e1556ffc4a47c73c1461f5f654b0.png)

作者图片

图像配置文件工具告诉我们，最常见的颜色的 RGB 值是[70.72，28.02，37.88]，该颜色的十六进制代码是#461c25。那种颜色显示在右上方。使用 colornamer，我们可以检索这些值的名称，从最具体到最不具体:

> xkcd 颜色:深栗色
> 
> 设计颜色:深酒红色
> 
> 常见颜色:栗色
> 
> 颜色系列:红色紫色
> 
> 颜色类型:深色
> 
> 彩色或中性:彩色

这些颜色名称可以帮助您以易于理解的方式对图像进行筛选或分组，然后在文档中使用这些图像或使用报告工具自动生成 PowerPoint 幻灯片。

![](img/58362721211cb3771efcd38bf153bb23.png)

*图片经由* [*GIPHY*](https://media.giphy.com/media/Qy7JZwgK1MFlEHHvHC/giphy.gif?cid=ecf05e47xjo229lxbbx3s7yul6dr6vuv5qwp5tkmfg113t3v&rid=giphy.gif&ct=g)

# 在可视化中使用自定义颜色

从这一点来看，绘制一个图表来显示每种颜色在图像数据集中占主导地位的频率是非常简单的。但就我个人而言，我发现在绘图中看到所有颜色的名称都用一种默认颜色来描绘令人不安。(这是一个很好的 Stroop 效应的例子，在这个效应中，我们的大脑努力处理不一致的刺激！)

幸运的是，根据最常出现的主要图像颜色创建自定义调色板并在绘图中使用它们并不太难。然后，我们可以简单地使用 pandas 内置的绘图功能来生成一个条形图，并将其位置输出到我们的工作流中。(我[在这里](https://community.alteryx.com/t5/Data-Science/Plot-Twist-Using-the-Python-Tool-for-Plotting/ba-p/584670?utm_content=815532&utm_source=tds)写了关于从 Python 工具中获取绘图的博客。)从那里，很容易查看和/或保存情节。

![](img/1148f2a544cc398f1c214bf156206643.png)

作者图片

我绘制了这个数据集中前 10 种主色出现的频率。仍然有很多灰色，但通过一些灰色占主导地位的图片可以证实，实际的衣服，而不仅仅是背景，确实经常是灰色的。(在这里，我认为我自己的灰色主题衣柜是一个异数。)

请记住，自定义调色板可能是也可能不是色盲友好的。你可以在这篇博文中阅读更多关于这个问题的内容，并找到一些工具和资源。

![](img/e77d82f38f542a812d4f44ad7a7e564f.png)

*图像通过* [*GIPHY*](https://media.giphy.com/media/t9lPSqrGSc1IOnajTz/giphy.gif?cid=790b7611654a178fbea0d767628f504fe65fa43c803b5006&rid=giphy.gif&ct=g)

# 用图像求解

图像配置文件工具提供了将图像的有趣信息引入工作流程的绝佳机会。享受您自己的图像调查，配备这一新的检查工具。我希望你能找到一些激动人心的、引人注目的结果！

想试试这个吗？[下载数据集](https://github.com/alexeygrigorev/clothing-dataset-small)，解压缩它，并获取[这篇文章所附的工作流程，如最初在 Alteryx 社区上发布的](https://community.alteryx.com/t5/Data-Science/Investigate-Your-Images-with-Image-Profile/ba-p/815532)。使用目录工具将数据集引入工作流。(也请确保在工作流程结束时更新渲染工具中的文件路径。)您需要以管理员身份运行 Designer，以便 Python 工具可以为您安装 colornamer 包。

*原载于* [*Alteryx 社区数据科学博客*](https://community.alteryx.com/t5/Data-Science/Investigate-Your-Images-with-Image-Profile/ba-p/815532?utm_content=815532&utm_source=tds) *。*