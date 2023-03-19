# 我的干净整洁数据清单

> 原文：<https://towardsdatascience.com/my-clean-and-tidy-checklist-for-clean-and-tidy-data-fbdeacb3736c?source=collection_archive---------45----------------------->

## 数据争论(获取、清理和转换数据)并不总是数据科学中有趣的部分，但它是在现实世界中处理数据的主要部分

![](img/5b776c8d0ab9c7423e25e2f41ba4e0a7.png)

穿着得体。来自 [Pexels](https://www.pexels.com/photo/photo-of-person-disinfecting-the-table-4099462/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[马蒂尔达·艾文](https://www.pexels.com/@matilda-wormwood?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

在我们开始使用我们刚刚学习的令人敬畏的新算法之前，我们需要确保我们拥有数据，我们已经修改或删除了任何有问题的条目，并且我们已经将数据转换为我们可以使用的形式。有什么比使用一个方便的清单更好的方法呢？因为说真的，谁不喜欢清单呢？看，没有人。

我的清单由 3 个主要部分组成，文档部分贯穿所有部分:

*   首先，我们必须关注我们的外部资源，那些我们无法真正控制的东西，这样它们就不会阻碍我们前进。
*   接下来，我们需要了解我们面对的是什么。
*   之后我们就可以自由地做实际的工作了！

→一定要记得全程记录——把它当成你现在和未来的自己。

![](img/e75d51d2bff57a01efcb5de43e83cbba.png)

作者图片

# 外部因素

![](img/8540a3e49e85a6657be46ac36606e65b.png)

他们就在我们中间。迈克尔·赫伦在 [Unsplash](https://unsplash.com/s/photos/ufo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

1.  **验证您的数据。在这个项目中，我的初始数据来自组织的另一个部门，他们已经做了一些计算。在工作了一段时间后，我注意到有些东西不对劲，这让我进行了一次彻底的 QA，在 QA 中我发现大多数计算基本上都是错误的。我应该从现实检查开始，但是我得到了教训——不要相信别人的数据！**
2.  验证您的系统。如果您的工作依赖于来自组织内部或(尤其是)外部的 API，您的第一步行动应该是检查它是否已启动并运行，您是否可以访问所有内容，等等。我们被延迟了一个月，因为一个合作伙伴试图让他们的 API 启动并运行，当你有一个客户截止日期时，这并不好玩。镜像之前的情绪——**不要相信别人的系统。**
3.  **重读你的资料。**如果你正在从其他人的笔记/笔记本/代码中学习，确保在你的项目工作一段时间后回到他们那里。一旦你更熟悉项目/数据/工具，你会注意到并理解你第一次错过的东西。

# 谅解

![](img/da9513832216d319d15a10cdc2e1b02f.png)

由 [Rob Schreckhise](https://unsplash.com/@robschreckhise?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

1.  **积累你的领域知识。很有可能你正在研究一个你不太了解的课题。对我来说，这是零售。什么是 [UPC](https://en.wikipedia.org/wiki/Universal_Product_Code) ？它看起来像什么，不同的数字是什么意思？不同的零售商如何组织他们的货架？
    一些信息可以在维基百科中找到，在其他情况下，你需要钻研你的项目数据或相关数据(在我的例子中，是书架图片)。我发现熟悉这个领域在整个过程中帮助了我——清理数据、编写代码(和发现错误),以及向客户展示最终的解决方案。**
2.  **了解你的交付成果是什么。**你试图用你的数据回答哪些问题？你打算怎么回答他们？您是要运行模型并在与客户的会议上展示结果，还是给他们发送 csv 文件，还是要编写一个将在生产中运行的算法？
    这将帮助您决定如何处理您的数据——如何准备数据，如何处理丢失的数据(删除/完成数据),在所有转换后您希望数据采用何种格式。
3.  **探索数据。** [这里的](/5-and-a-half-lines-of-code-for-understanding-your-data-with-pandas-aedd3bec4c89)是一些让你入门的好代码行。每个(字符串)列中的唯一值是什么？每种有多少？查看来自列的随机数据样本，以了解数据的外观。
    例如，我想保存一些带前导零的数字，所以我将它们作为对象存储。但是后来在这个过程中，我将数据转换为 int，并对我拥有的组的数量感到惊讶。事实证明，同一个数字可能有不同数量的前导零。哎呀。

# 进入正题

![](img/441fbf9e4110a98cef927b0b7b92c1c3.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

1.  清理那些数据。你需要处理缺失值、离群值、重复值等等。这本身就是一个完整的世界，[这些](/data-cleaning-in-python-the-ultimate-guide-2020-c63b88bf0a0d)是一个[开始](/data-cleaning-with-python-and-pandas-detecting-missing-values-3e9c6ebcf78b)的好地方。
2.  **验证您的数据转换。当你转换或合并数据时，总是要停下来检查结果是否如你所愿。例如，结果数据帧具有预期的行数(而不是您认为它应该具有的行数的 3 倍)。举几个例子，手动检查结果是否正确。这很烦人，但比以后意识到错误要少得多。
    合并时您可以使用的一个有用工具是[指示器标志](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)，它告诉您结果数据是从左侧、右侧还是两个数据帧合并而来。**
3.  **构建一个小沙箱进行实验**:当你试图对大型数据帧进行复杂运算时，不要试图在真实数据集上计算出你的算法。只是混乱又缓慢。就像你对代码进行单元测试一样，把一个小例子和相关的用例放在一起，然后继续工作。一旦成功了，就在真实数据上试试——正如我们之前提到的，确保验证你的结果。

## 证明文件

![](img/2ca7c20a811065cbb924ea1aedfc4aae.png)

但是要让它可读。马克·拉斯姆森在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**文档！**保留一个文档，在其中写下任何见解、想法、相关数据的链接等。你所遇到的。当你想最终得到一个演示文稿时，或者如果你想把项目交给某人，或者如果客户几个月后有问题时，它会很有帮助。使用笔记本上的 markdown 来解释你正在做的事情。既然都是商业逻辑，过几个月就会有人(或者未来的你)感谢你。

# 轮到你了

你可能会注意到，在所有这些艰苦的工作之后，我们差不多是 Kaggle 比赛开始的地方。它们是研究算法的一种很好的方式，但它们往往带有干净的数据，不太像现实世界中生产系统的模糊现实。

如果你想动手实践数据清理，这里有一个非常棒的数据集列表[需要一些爱来为工作做准备，以及需要做些什么来让你前进的亮点。](https://makingnoiseandhearingthings.com/2018/04/19/datasets-for-data-cleaning-practice/)

# 让我总结一下

## 这是我们最后的清单

就像我们的数据一样整洁:

*   验证您的数据
*   验证您的系统
*   重读你的资料
*   建立你的领域知识
*   理解你的交付物是什么
*   探索数据
*   清理数据
*   验证您的数据转换
*   建造一个小沙箱进行实验
*   文档！

现在你的数据已经整理好了，你可以进入大多数人最喜欢的部分——算法。只是不要忘记，没有闪亮的算法会完全弥补糟糕的数据！