# 设置 Conda 在您的苹果 M1 硅机器上运行 PyData 堆栈

> 原文：<https://towardsdatascience.com/setting-up-conda-to-run-pydata-stack-on-your-apple-m1-silicon-machine-55e0b8320f65?source=collection_archive---------22----------------------->

## 使用 mambaforge 为您的 ARM64 芯片安装诸如 Dask、XGBoost 和 Coiled 等 PyData 库的分步指南

![](img/24401bf6e58d0daae132bf5d43d55852.png)

Michael Dziedzic 通过 [Unsplash](https://unsplash.com/@lazycreekimages) 拍摄的图片

# TL；博士；医生

在苹果 M1 机器上运行 PyData 库需要你使用一个 ARM64 定制版本的 conda-forge。本文提供了如何在您的机器上设置它的分步指南。然后讨论了一些特定于运行 Dask、Coiled 和 XGBoost 的问题。我希望这能对遇到类似问题的人有所帮助。

# M1 的 PyData

苹果 M1 硅芯片是一项了不起的创新:它低功耗、高效率、电池寿命长，而且价格便宜。但是，如果您使用默认版本运行 PyData 堆栈，您可能会遇到一些奇怪的行为。

当您安装为英特尔 CPU 架构编写的库时，代码将通过 Rosetta 仿真器运行。这通常很有效，但可能会给特定的 PyData 库带来意想不到的问题。例如，当导入一个<100MB dataset.

Since the internals of Rosetta is proprietary, there’s no way to know exactly why this is happening.

# Installing PyData Libraries using Mambaforge

But fear not, there *是*的一种前进方式时，我个人遇到了 Dask 的内存使用爆炸。您可以通过安装 ARM64-native 版本的 PyData 库来解决这种奇怪的行为。虽然对于大多数安装，我通常会推荐 Anaconda/miniconda，但是目前， **mambaforge** 部署似乎拥有最平滑的 ARM 安装过程。

下面是安装这个版本的 conda 并使用它来安装您最喜欢的 PyData 库的分步指南。

**1。检查您的 conda 版本**

*   要检查您的 conda 安装，请在您的终端中键入`conda info`并检查`platform`键的值。这个应该说`osx-arm64.`
*   如果没有，继续删除包含您的 conda 安装的文件夹。请注意，这将删除您的软件环境，因此请确保将这些环境另存为。yml 或者。txt 文件。您可以通过在终端中键入`conda list - export > env-name.txt`或`environment.yml`来导出活动软件环境的“配方”。

**2。下载曼巴福吉安装程序**

**3。运行安装程序。**

*   这应该会问你是否希望它为你运行`conda init`。选择“是”。
*   如果由于某种原因它没有这样做(在我的例子中就发生了)，您将不得不使用/path/to/mamba forge/bin/conda init zsh 自己运行`conda init`。请注意，`zsh`只有在您使用 `zsh`外壳时才有效。根据需要使用其他 shell 标志。

重启终端后，你现在应该可以像往常一样使用`conda`了。例如，你可以从使用`conda env create -f <path/to/environment.yml>`重建你的软件环境开始。

# 运行 Dask、Coiled 和 XGBoost 时的具体问题

下面是我在 Coiled 上尝试使用 Dask 和 XGBoost 运行工作负载时遇到的一些问题的进一步说明:

```
import coiled 
from dask import Client 
import dask.dataframe as dd cluster = coiled.Cluster() 
client = Client(cluster) 
ddf = dd.read_parquet("<path/to/100MB/file/on/s3>") 
ddf = ddf.persist()
```

我遇到了三个问题，我想分享我的解决方案，因为我可以想象其他人可能也会遇到这种情况:

1.  Dask 的内存使用量在导入小(<100MB) dataset with an error like the one below:

```
distributed.nanny - WARNING - Worker exceeded 95% memory budget. Restarting
```

2\. In some software environments, my kernel was crashing when calling **导入盘绕**时爆炸

3.切换到 mambaforge 部署后，我无法安装 XGBoost。

以下是我想出的解决办法。

**1。Dask**

第一个问题通过切换到 mambaforge 并从那里安装所有需要的 PyData 库得到了解决，如上面的分步指南所述。

**2。盘绕的**

第二个问题是由于 **python-blosc** 库引起的冲突。由于某种未知的原因，当导入带有分段错误的 **python-blosc** (例如作为`import coiled`的一部分)时，苹果 M1 机器上的内核崩溃。为了避免这种情况，请确保您当前的软件环境中没有安装 python-blosc。如果是，运行`conda uninstall python-blosc`将其移除。

**3。XGBoost**

最后，如果你想在你的苹果 M1 机器上运行 XGBoost，你需要跳过一些额外的环节，因为目前 conda-forge 上没有 arm64 兼容版本的 XGBoost。这不是问题，因为 XGBoost 本身实际上可以在你的苹果 M1 机器上运行；问题在于 XGBoost 安装的依赖项，特别是 numpy、scipy 和 scikit-learn。这些依赖项需要与 arm64 兼容，我们将通过完成以下步骤来确保这一点:

1.创建一个包含 Python、numpy、scikit-learn 和 scipy 的新 conda 环境。在这里使用 conda 而不是 pip 来安装这些依赖项是至关重要的。

```
conda create -n xgboostconda activate xgboostconda install python=3.8.8 numpy scipy scikit-learn
```

2.安装必要的库来编译 XGBoost

```
conda install cmake llvm-openmp compilers
```

3.现在我们已经有了正确的依赖项，我们可以从 pip 安装 XGBoost 了。这很好，因为我们已经安装了所有合适的 arm-64 依赖项。

```
pip install xgboost
```

4.带它去兜一圈！

注意:上面的步骤是由 [Fabrice Daniel](https://medium.com/u/926442548db0) 从[这本很棒的教程](/install-xgboost-and-lightgbm-on-apple-m1-macs-cb75180a2dda)中总结出来的，并在 [Isuru Fernando](https://twitter.com/isuru_f) 的帮助下进行了更新。

# 走向

正如任何技术突破一样，随着行业重新校准并适应苹果转向 M1 芯片所引起的连锁反应，将会有一些阵痛。看到像 Anaconda 和 QuanStack 这样的公司以及 Conda-Forge 社区继续改进对 ARM 硬件的支持是令人兴奋的，我对 PyData 的强大未来充满信心！

这一次到此为止！我希望这篇文章有助于让你的 PyData 工作流程在你的苹果 M1 机器上顺利有效地运行。如果您有任何反馈或问题，或者有更适合您的不同设置，请联系我们！

*原载于 2021 年 8 月 19 日*[*https://coiled . io*](https://coiled.io/blog/apple-arm64-mambaforge/)*。*