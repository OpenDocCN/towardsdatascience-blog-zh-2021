# 用 Jupyter 内核避免 DLL 地狱

> 原文：<https://towardsdatascience.com/avoid-dll-hell-with-jupyter-kernels-4447cf16dc34?source=collection_archive---------49----------------------->

![](img/35ba97e313b00d2136f30a0e96d48f34.png)

斯特凡诺·波利奥在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 类似于 *Docker 容器*，内核是确保你的 ML 应用程序可以在任何地方运行的关键。

# 介绍

以前在 Windows 上有一个叫做 *DLL Hell* 的东西。简而言之，过去你电脑上的所有应用程序都*共享*已安装库的相同*版本*——所以每次你安装一个新的应用程序，你都冒着更新底层库和破坏先前安装的应用程序的风险。

对于习惯于*容器化*他们的应用程序的 *Docker* 爱好者来说，这是很好理解的:运行时环境需要*虚拟化*以便应用程序可以移植，并且对特定版本库的依赖可以被隔离到容器本身。这确保了应用程序可以在任何人的计算机上运行，包括云。

# 蟒蛇

Anaconda 个人版让打包和分发 ML 应用变得轻而易举:

<https://www.anaconda.com/products/individual>  

中的*到底是什么由你决定——你可以使用 *Python* 或 *R* ，并包含你的项目所需的任何库/框架。当你完成后，你就有了一个可移植的内核，它可以跟随你的应用程序，并确保它总是看到相同的运行时环境。不管你的同事使用的是 *Windows* 、 *MacOS* 还是 *Linux* 都没关系——你可以轻松地与他们分享你的项目，并确保他们在他们的机器上看到的结果与你在自己的机器上看到的一样。*

# 康达环境

这个食谱背后的秘方是环境的概念。安装完 *Anaconda* 之后，你将能够使用一个叫做 *Anaconda Prompt* 的漂亮的命令窗口。在管理*环境*时，它会很快成为你新的最好的朋友。

这是你可以用*康达环境*做的所有酷事情的官方清单，但在下一节，我将带你经历一个典型的场景:

  

# GPU 上的张量流

![](img/8118531e30f0099840e79120f4c95ec1.png)

斯蒂芬·科斯马在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

或者…*曲速*！在 CPU 上构建神经网络是*所以*昨天，所有酷孩子都在使用*支持 CUDA 的 Nvidia GPU*来运行他们的！比方说，我们有一些新的想法来提高我们一直在做的 CNN 的准确性。首先要做的是旋转一个 *conda env* 并加载我们需要的所有东西(从 *Anaconda 提示符*):

```
conda create -n tf-warp-speed tensorflow-gpu=2.1 python
```

我们将把我们的 conda *容器命名为* **tf-warp-speed** ，并用支持 GPU 的版本 *TensorFlow* 对其进行初始化。我们现在有了一个*沙盒*来玩——从现在开始我们在里面做的任何事情都不会影响我们所依赖的操作系统。

步入沙盒很容易，只需*激活*它:

```
conda activate tf-warp-speed
```

我们已经安装了 *Python* 和支持 GPU 的 *TensorFlow* 版本，但是您可以在这里为您的项目加载额外的库/框架。

对于这篇文章，我们将使用 *rogue* 和 version-up(通过*pip*)*tensor flow*:

```
pip install tensorflow-gpu==2.3.1
```

忽略你可能看到的任何依赖错误，让我们继续让我们的新 *conda env* 对 *Jupyter 笔记本*可见！

```
conda install ipykernel
```

我们上面安装的是一个*交互式 Python 内核*，它为 *Jupyter 笔记本*提供了一个执行后端。现在我们需要给它贴上消费标签:

```
python -m ipykernel install --user --name=tf-warp-speed
```

*瞧*！启动一个 *Jupyter 笔记本*，进入**内核- >改变内核**，你应该会看到一个 *tf-warp-speed* 的选项！选择那个，我们就可以开始了！在新笔记本的*代码*单元中输入以下内容:

```
import tensorflow as tf
from tensorflow.python.client import device_libprint("TensorFlow version: ", tf.**version**)
print("TensorFlow built with CUDA: ",tf.test.is_built_with_cuda())
print("GPUs Available: ", len(tf.config.list_physical_devices('GPU')))!python --version
```

当您*执行它* (CTRL-ENTER)时，您应该看到类似这样的内容:

```
TensorFlow version:  2.3.1
TensorFlow built with CUDA:  True
GPUs Available:  1
Python 3.7.9
```

让我们带它出去兜一圈，看看它能做什么！

```
import timeitdef cpu():
  with tf.device('/cpu:0'):
    random_image_cpu = tf.random.normal((100, 100, 100, 3))
    net_cpu = tf.keras.layers.Conv2D(32, 7)(random_image_cpu)
    return tf.math.reduce_sum(net_cpu)def gpu():
  with tf.device('/device:GPU:0'):
    random_image_gpu = tf.random.normal((100, 100, 100, 3))
    net_gpu = tf.keras.layers.Conv2D(32, 7)(random_image_gpu)
    return tf.math.reduce_sum(net_gpu)

# We run each op once to warm up
cpu()
gpu()# Run the op several times
print('Time (s) to convolve 32x7x7x3 filter over random 100x100x100x3 images '
      '(batch x height x width x channel). Sum of 100 runs.')
print('CPU (s):')
cpu_time = timeit.timeit('cpu()', number=100, setup="from __main__ import cpu")
print(cpu_time)
print('GPU (s):')
gpu_time = timeit.timeit('gpu()', number=100, setup="from __main__ import gpu")
print(gpu_time)
print('GPU speedup over CPU: {}x'.format(int(cpu_time/gpu_time)))
```

如果此时一切正常，您应该会看到如下内容:

```
Time (s) to convolve 32x7x7x3 filter over random 100x100x100x3 images (batch x height x width x channel). Sum of 100 runs.
CPU (s):
6.590599500000053
GPU (s):
0.8478618999999981
GPU speedup over CPU: 7x
```

等等…*什么*？！？在 GPU 上运行我的 CNN***比我的 CPU 快七倍*** ？！？现在*那就是*的曲速！

# 共享

在这一点上，我对我的新内核非常兴奋，但是我有一个朋友在 Ebay 上花了太多的钱买了两张 3090 卡，我真的很想听听它是什么感觉。

轻松点。我可以简单地将我的 *conda env* 导出到一个 *yaml* 文件中，并通过电子邮件发送该文件(连同我的*。ipynb 文件)给我的朋友。他们可以重新创建我的内核，重新运行我的笔记本，让嘲讽开始(因为我还在运行一台 GTX 1080)！

```
conda env export > environment.yml
```

运行完以上程序后，我有了我的 *yaml* 文件，其中包含了我的朋友构建他们的新 *conda env* 所需的一切:

```
conda env create -f environment.yml
```

# 结论

使用 *Anaconda* 来管理您的虚拟环境是维护您的 ML 项目的完整性的关键。库和框架的版本来来去去就像日落一样快——明智的做法是为您的每个项目拍摄运行时环境的快照。