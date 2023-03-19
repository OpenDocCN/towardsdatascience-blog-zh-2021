# 从源代码构建 Keras:后续指南

> 原文：<https://towardsdatascience.com/building-keras-from-source-a-follow-along-guide-2bcc4cea3aec?source=collection_archive---------20----------------------->

## 构建 Keras 的分步指南。

![](img/7bf70049d554718103df488c363f5dee.png)

*照片由*[劳拉·奥克](https://unsplash.com/@viazavier) *上* [*下*](https://unsplash.com/)

# 前奏:

Keras 最近通过将代码库托管在一个单独的存储库中，朝着改善开发者体验迈出了一大步。正如在 [RFC](https://github.com/tensorflow/community/blob/master/rfcs/20200205-standalone-keras-repository.md) 中提到的，主要目标之一是消除由于核心 [TensorFlow](https://www.tensorflow.org/) 库的长构建时间而导致的冗长反馈循环。由于这一变化，现在可以在非常可行的时间内运行测试。

这篇博文旨在成为 Keras 构建和测试过程的实践指南。让我们开始吧！

# 从源构建:

对于这里所有的新手，我想花点时间解释一下“从源代码构建”到底是什么意思。这可能意味着很多事情(在这里最好的解释是[这里是](https://stackoverflow.com/questions/1622506/programming-definitions-what-exactly-is-building))，但是在我们的例子中，它意味着以下内容:

将源代码编译成可安装包，并将所有模块链接到各自的端点。[1]"

注意，即使在这次迁移之后，仍然可以通过调用`from tensorflow import keras`来访问 Keras。这是由黄金 API 实现的。这些是 Keras 库为 TensorFlow 库提供的端点。因此，即使 Keras 是单独开发的，对于用户来说，它仍然停留在`tf.keras`。你可以在这篇[帖子](https://stackoverflow.com/questions/1622506/programming-definitions-what-exactly-is-building)中了解到这一点。实现这一点的代码可在[这里](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/api_template.__init__.py)获得。

我假设您是在 Linux 机器上这样做的。额外的好处是，这可以与支持 TPU 的云虚拟机完美配合。

以下所有命令都是直接采用或受官方 Keras 贡献的[指南](https://github.com/keras-team/keras/blob/master/CONTRIBUTING.md)的启发。请在打开 PR 之前进行同样的操作。

我还创建了一个 [Colab 笔记本](https://colab.research.google.com/github/AdityaKane2001/keras_build_test/blob/main/Keras_build_test_notebook.ipynb)，你可以用它来轻松地构建和测试代码！

## 第 1 部分:设置环境

就像 TensorFlow 一样，Keras 使用了[Bazel](https://bazel.build/)【2】，一个基于图形的构建管理系统。这意味着您可以构建一次 Keras，后续的构建将重用自上一次构建以来没有更改的部分。因此，重建所需的时间大大减少。下面是我们设置环境的方法:

安装巴泽尔。

接下来，我们建立一个虚拟 python 环境。如果您在开发机器上工作，建议这样做。在 Colab 中，在基础环境中重新安装 Keras 就可以了。

设置虚拟环境。

接下来，我们克隆我们的开发分支。我们还安装了 TensorFlow 的夜间版本，这确保了我们与主 TensorFlow repo 保持同步。

克隆存储库并更新环境。

## 第 2 部分:更新 golden APIs(如果有的话)

需要更新的文件的文件结构。

这部分仅适用于您添加了一些新文件的情况。您需要在以下文件中添加它们的名称。这确保您的模块将被构建，并在以后可供用户访问。

## 第 3 部分:构建和安装

现在是过程的关键。在这里，我们构建并安装我们的 Keras 版本。为此，请使用以下命令:

使用 Bazel 构建。

执行上述命令后，Keras 将根据您的更改重新安装。

如果你只想进行测试，那么你可以用`bazel test`代替`bazel build`。在这种情况下，您可以修改代码并再次运行`bazel test`。您不需要像我们使用`bazel build`那样手动安装软件包。

示例:

```
bazel test keras/layers/convolutional_test
```

在这里，您可以运行任意多的测试。您可以使用以下命令运行所有测试:

```
bazel test — test_timeout 300,450,1200,3600 — test_output=errors — keep_going — define=use_fast_cpp_protos=false — build_tests_only — build_tag_filters=-no_oss — test_tag_filters=-no_oss keras/…
```

# 常见错误:

```
ERROR: /home/jupyter/.cache/bazel/_bazel_jupyter/ebc81b3ee71ff9bb69270887ebdc0d7b/external/bazel_skylib/lib/unittest.bzl:203:27: name ‘analysis_test_transition’ is not defined# OR ERROR: error loading package ‘’: Extension ‘lib/unittest.bzl’ has errors
ERROR: error loading package ‘’: Extension ‘lib/unittest.bzl’ has errors
INFO: Elapsed time: 3.557s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (0 packages loaded)
```

重新启动虚拟机。这种情况发生在安装之后，因为 path 不会在整个系统中更新。

```
ImportError: cannot import name ‘saved_metadata_pb2’ from ‘keras.protobuf’ (unknown location)
```

请更改目录，然后重试。这是由于本地和全球环境的混合造成的。

# 承认

我感谢 TPU 研究云(TRC)[3]对这个项目的支持。TRC 在该项目期间允许 TPU 进入。谷歌通过提供谷歌云信用来支持这项工作。感谢 Keras 团队的 Scott Zhu 在整个过程中对我的指导。

# 离别的思绪

Keras 是一个用于深度学习的通用而灵活的库。它被成千上万的开发者使用，是一个巨大的开源项目。如果你发现了一个 bug 或者想要在 Keras 中实现一个特性，自己动手吧！没有什么比看着自己的代码被无数人使用更令人高兴的了。随着能够轻松构建 Keras，每个人都有能力改进代码库，让 Keras 成为更好的产品。

# 参考

*【1】Greg Mattes 关于 StackOverflow 的回答(*[)https://stack overflow . com/questions/1622506/programming-definitions-what-just-is-building](https://stackoverflow.com/questions/1622506/programming-definitions-what-exactly-is-building)*)*

[2]“Bazel——一个快速、可伸缩、多语言和可扩展的构建系统”([https://bazel.build/](https://bazel.build/))

[3] TPU 研究云([https://sites.research.google/trc/about/](https://sites.research.google/trc/about/))