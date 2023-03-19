# 手机深度学习:手机平台 PyTorch Lite 解释器

> 原文：<https://towardsdatascience.com/deep-learning-on-your-phone-pytorch-lite-interpreter-for-mobile-platforms-ae73d0b17eaa?source=collection_archive---------5----------------------->

## PyTorch 深度学习框架支持在移动设备上无缝运行服务器训练的 CNN 和其他 ML 模型。本文描述了如何在移动设备上优化和运行您的服务器训练模型。

![](img/b08019b40fd6a548f2f9b048c4374d29.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) 拍照

[PyTorch](https://pytorch.org/) 是一个深度学习框架，用于训练和运行机器学习(ML)模型，加快从研究到生产的速度。

通常，人们会在功能强大的服务器上训练模型(在 CPU 或 GPU 上)，然后将预训练的模型部署在移动平台上(这通常更受资源限制)。本文将重点讨论 PyTorch 为在移动设备/平台上运行服务器训练的模型提供了哪些支持。

[PyTorch Mobile](https://pytorch.org/mobile/home/) 目前支持在 [Android](https://pytorch.org/mobile/android/) 和 [iOS](https://pytorch.org/mobile/ios/) 平台上部署预训练模型进行推理。

# 高层次的步骤

1.  在服务器上训练模型(在 CPU 或 GPU 上)——本文不会讨论训练模型的细节。你可以在这里、[这里](/understanding-pytorch-with-an-example-a-step-by-step-tutorial-81fc5f8c4e8e)和[这里](https://www.kdnuggets.com/2020/06/fundamentals-pytorch.html)找到更多关于 PyTorch [训练的信息。这里有一篇关于 PyTorch](https://www.kdnuggets.com/2020/10/getting-started-pytorch.html) 入门的好文章。
2.  [可选]针对移动推理优化您的训练模型
3.  以 lite 解释器格式保存您的模型
4.  使用 PyTorch 移动 API 在您的移动应用程序中进行部署
5.  利润！

# 详细步骤

本文的其余部分假设您已经预先接受了培训。pt 模型文件，下面的示例将使用一个虚拟模型来遍历代码和工作流，以便使用 PyTorch Lite 解释器进行移动平台的深度学习。

## 在服务器上创建/训练模型

检查它是否运行，并产生合理的输出

应打印:

*张量(【4。, 11., 18.])*

## 生成脚本模型

## [可选]为移动推理优化模型

有关移动优化器的详细信息，请参见本页。

## 为移动推理保存模型

## 加载并运行移动模型进行推理(在移动设备上)

下面的代码直接使用 C++ API。根据你的需求，你可以选择使用 [Android (Java)](https://pytorch.org/mobile/android/) 或者[iOS](https://pytorch.org/mobile/ios/)API。您还可以查看[本文](https://www.kdnuggets.com/2021/11/deep-learning-mobile-phone-pytorch-c-api.html)开始使用 C++ API 在您的开发机器上以 x 平台的方式运行模型推理。

# optimize_for_mobile(模型)到底是做什么的？

移动优化过程对模型的图形执行许多更改。你可以在这里找到移动优化通道[的源代码。下面是这种优化的一个不全面的列表。](https://github.com/pytorch/pytorch/blob/master/torch/csrc/jit/passes/xnnpack_rewrite.cpp#L453)

1.  **模块冻结:**这将不会被模型变异的属性提升为常量。在后续过程中很有用。
2.  **函数内联**:如果从模型的 forward()方法中调用一个方法，那么被调用的方法在优化的模块中被内联，以消除函数调用开销。
3.  **操作符融合**:例如[融合](https://web.stanford.edu/class/cs245/win2020/slides/09-PyTorch.pdf)once 巴奇诺姆或 conv-雷鲁等……这允许在中间层重复使用进入 CPU 缓存的数据。
4.  **去除漏失** : [漏失](https://machinelearningmastery.com/dropout-for-regularizing-deep-neural-networks/)是一种近似并行训练大量不同架构神经网络的正则化方法。一旦模型进入 eval() only 模式，就不需要这样做了，删除它可以提高模型的推理性能。

# 比较移动优化前后的模型图

## 在优化之前

原始模型有两个方法:forward()和 helper()。forward()调用 helper()。此外，张量 t1 是模型属性。

*scripted.forward.graph*

将打印

*scripted.helper.graph*

将打印

## 优化后

优化的模型只有一个方法，forward()。此外，张量 t1 是模型常数。

*优化 _ 模型.转发.图形*

将打印

# lite 解释器模型与 TorchScript 模型有什么不同？

lite 解释器模型文件是一个常规的 TorchScript 模型文件，其中添加了特定于移动设备的字节码。您可以将移动模型作为普通的 PyTorch TorchScript 模型加载，也可以将其作为 lite-interpreter 模型加载。

当保存为 lite-interpreter(移动平台)时，PyTorch 为模型的图形保存了额外的字节码，与 TorchScript 相比，在设备上执行更有效。相对于运行 TorchScript，PyTorch 在编译的应用程序中使用的二进制文件更少。

下面是如何查看生成的 bytecode.pkl 文件。

# 结论

PyTorch 现在支持移动平台上的推理。很容易采用服务器训练的模型，并将其转换为在移动平台上运行优化的推理。模型语义以 TorchScript 和 Lite 解释器格式保存。

# 参考

1.  [PyTorch 手机](https://pytorch.org/mobile/home/)
2.  [通过示例了解 PyTorch:分步指南](/understanding-pytorch-with-an-example-a-step-by-step-tutorial-81fc5f8c4e8e)
3.  [深度学习 PyTorch 简介](https://www.kdnuggets.com/2018/11/introduction-pytorch-deep-learning.html)
4.  【PyTorch 入门
5.  [你应该知道的 PyTorch 最重要的基础知识](https://www.kdnuggets.com/2020/06/fundamentals-pytorch.html)
6.  [深度学习 PyTorch:入门](https://medium.com/data-folks-indonesia/pytorch-for-deep-learning-get-started-67c4c0a234e3)
7.  [什么是辍学？](https://machinelearningmastery.com/dropout-for-regularizing-deep-neural-networks/)
8.  [PyTorch 算子融合](https://web.stanford.edu/class/cs245/win2020/slides/09-PyTorch.pdf)
9.  [PyTorch 移动优化器](https://pytorch.org/docs/stable/mobile_optimizer.html)