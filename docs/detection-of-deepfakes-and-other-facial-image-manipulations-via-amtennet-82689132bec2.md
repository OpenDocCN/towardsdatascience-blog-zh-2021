# 通过 AMTENnet 检测深度伪造和其他面部图像操作

> 原文：<https://towardsdatascience.com/detection-of-deepfakes-and-other-facial-image-manipulations-via-amtennet-82689132bec2?source=collection_archive---------25----------------------->

## 一种检测数字面部操作的新方法

![](img/761fca54ace65605528036ff4f057bd1.png)

杰佛森·桑多斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

最先进的图像处理技术允许任何人改变图像和视频来交换一个人的身份。像 Deepfake 和 Face2Face 这样的技术是最近流行起来的两种相对新颖的技术。除了这些技术之外，各种基于 [GAN 的人脸交换](/gender-swap-and-cyclegan-in-tensorflow-2-0-359fe74ab7ff)方法也已经发布，并附有代码。

DeepFakes 和 Face2Face 的使用有一个伦理因素；随着这些技术的不断改进，与之相关的伦理风险也在不断增加。因此，为了应对这种新出现的道德威胁，[脸书研究](https://medium.com/u/25aae929dbb1?source=post_page-----82689132bec2--------------------------------)创建了“ [DeepFake 检测挑战(DFDC)数据集](https://ai.facebook.com/datasets/dfdc)”和一个 [Kaggle 竞赛](https://ai.facebook.com/datasets/dfdc)来提高检测 DeepFakes 的准确性。在这场比赛中，黑盒环境中的最高准确率约为 65%。然而，更高的精度已经实现。

这个主题已经成为学术界的趋势，因此在学术界(以及学术界以外)围绕面部操作检测领域进行了大量的研究。来自中国湖南和南京大学的研究人员最近发表的一篇[期刊论文](https://arxiv.org/abs/2005.04945)提出了一种通过自适应操纵轨迹提取网络(AMTEN)检测 DeepFakes 操纵的新方法。

## 阿姆滕解释道

AMTEN 作为预处理模块工作，用于抑制图像内容的副作用。典型的面部操纵取证试图预测痕迹的操纵并提取它们。然而，AMTEN 使用输入图像和输出特征图之间的差异来提取这些操作轨迹。这种新的方法似乎可以很好地检测出倾向于篡改检测任务的操纵痕迹。

AMTEN 模块与卷积神经网络的集成产生了 AMTENnet，这是一种深度神经网络，旨在寻找图像和视频中使用的面部图像处理技术。

最近关于假脸检测的工作集中在二进制分类上，即找出一幅图像是假的还是真的。除了二进制分类，AMTENnet 还考虑了面部图像处理技术的多种分类。

人脸图像处理技术可以分为三类，即身份处理、表情处理和属性转移。

*   身份操纵:这是一种生成完全虚构的人的假脸图像的行为，以及用另一个人的脸替换一个人的脸(通过 DeepFake 或 FaceSwap)。
*   表情操纵:是指将面部表情从源人脸转移到目标人脸。
*   属性转移:当人脸图像的风格改变时，例如，改变性别、头发颜色或年龄。

AMTENnet 的网络架构在发表它的期刊论文中有充分的解释(包括图表)，因此我鼓励读者看一看这篇开放存取的期刊论文，以了解关于该架构的更多细节。为了准确起见，我将引用下面的 AMTENnet 论文来解释网络架构是如何工作的:

> 给定一幅输入的 RGB 图像，我们使用 AMTEN 中的 Conv 1 来获取图像的特征图。然后，从 Conv 1 中的特征图中减去原始图像，提取低级操纵痕迹 **Fmt** 。此外，通过重用 **Fmt** 获得稳定的高级操纵轨迹，即 **Freu** 。接下来，将 **Freu** 传递到后续卷积层进行分层特征提取，以获得高级取证特征。最后，我们使用全连接层和 softmax 函数对图像进行分类。

## AMTENnet 的准确性和优越性

AMTENnet 针对以下最先进的基线模型进行了测试:

*   [Meso-4](https://ieeexplore.ieee.org/abstract/document/8630761)
*   [中见-4](https://ieeexplore.ieee.org/abstract/document/8630761)
*   [手工制作的分辨率](https://dl.acm.org/doi/abs/10.1145/3206004.3206009?casa_token=xK35O740hf8AAAAA%3AucylQZoDeYfz2Wh7Y9jGhbStatADeJ8aZ3C9jSi5LifCCAbONOttWFimnk1sh_qzUuOlhg7-82g)
*   [MISLnet](https://ieeexplore.ieee.org/abstract/document/8335799)
*   [异常网络](https://openaccess.thecvf.com/content_ICCV_2019/html/Rossler_FaceForensics_Learning_to_Detect_Manipulated_Facial_Images_ICCV_2019_paper.html)
*   模型基础:本文中使用的基本模型，不包括 AMTEN 模块

此外，其他最先进的模块被用来取代 AMTEN 模块，从而产生了几个混合模型。这些模块分别是[ [周等，2018](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhou_Learning_Rich_Features_CVPR_2018_paper.html) ]的 SRM 滤波器核、[【巴亚尔和斯塔姆，2018 ]的约束 Conv 和[ [莫等，2018](https://dl.acm.org/doi/abs/10.1145/3206004.3206009?casa_token=c9MGW0lwBjYAAAAA:IHpjdH7_nhDZi9Fy83xZq3hVkKPrwDHI7pQoA_E8RE55jJQtnoRDMNfJfilEchjy5jRSZIo7nOA) ]的手工制作的特征提取器。

两个数据集用于训练、验证和测试所有网络，可在 GitHub [**此处**](https://github.com/EricGzq/Hybrid-Fake-Face-Dataset) 和 [**此处**](https://github.com/deepfakes/faceswap) 找到。对两种情况进行了训练、验证和测试；首先，人脸图像处理技术的分类；第二，人脸图像操纵的二元分类(图像是不是假的)。

实验结果表明，AMTENnet 具有较高的检测精度和泛化能力，特别是对于多分类的人脸图像处理技术，检测精度提高了 7.61%。对于二元分类数据集，AMTENnet 有大约 1%的增长，虽然很低，但仍然是更好的。用数字来透视多重分类问题，MISLnet 达到了 74.03%的准确率，而 AMTENnet 达到了 81.13%的准确率。我鼓励读者在这里查看完整的文章[](https://arxiv.org/abs/2005.04945)**来检查所有的结果。**

## **结束语**

**AMTENnet 提高了检测 DeepFakes 的准确性，但也展示了一种方法，如果正确遵循，可以被研究社区复制，并用于推动知识的边界，甚至比 AMTEN 更进一步。我敢肯定，到今年年底，DeepFakes 检测模型的准确性将进一步提高。计算机科学的研究速度比以往任何时候都快，这在很大程度上要归功于脸书人工智能研究所、OpenAI 和 DeepMind 的数据集和研究论文。开源被证明是加速知识扩展的最佳方式。**

**感谢您的阅读，感谢您对本文的任何反馈。你可以在我的 [GitHub 页面](https://github.com/manuelsilverio)上看到我在机器学习方面的公开工作，并随时关注我或通过 [LinkedIn](https://www.linkedin.com/in/manuelsilverio/) 联系我。**