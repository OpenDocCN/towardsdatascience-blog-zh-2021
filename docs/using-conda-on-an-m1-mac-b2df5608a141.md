# 在 M1 Mac 电脑上使用 conda

> 原文：<https://towardsdatascience.com/using-conda-on-an-m1-mac-b2df5608a141?source=collection_archive---------3----------------------->

## 运行多个 conda 发行版，从两个世界获得最佳效果。

![](img/b0882ecace2767d3b2a5e21ee4095721.png)

照片由[派恩瓦特](https://unsplash.com/@pinewatt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/merge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

如果你最近在工作中购买或获得了一台新的 M1 Mac，并且你正在使用 Python 开发或从事数据科学项目，你可能已经浪费了一些时间来运行一些包。我仍然在我的 Mac 上与 Python、Docker 和 conda 作斗争，但是我找到了一种让许多包在 conda 环境中运行的方法。

由于 M1 是基于 ARM 的系统，许多 Python 包无法正确安装，因为它们是为在 AMD64 (x86)上运行而构建的。像许多其他人一样，我使用 conda 为我的项目建立环境——最好是使用 Anaconda 或 Miniconda。

当我第一次想在我的 Mac 上安装 Tensorflow 时，我偶然发现了 mini forge([https://github.com/conda-forge/miniforge](https://github.com/conda-forge/miniforge))，它可以与 Miniconda 相媲美，但 conda-forge 是默认通道，并专注于支持各种 CPU 架构。Tensorflow 与 Miniforge 一起安装时工作正常。

但是当我需要安装我工作中使用的某些包时——比如 SimpleITK(现在也有 M1 Python wheel！)— Miniforge 无法安装它。这有点像赌博。在某个时候，我意识到我可以在同一个系统上同时安装和使用 Miniforge 和 Miniconda。

**编辑:正如**[**Lucas-Raphael müller**](https://medium.com/u/7e72236d2b4b?source=post_page-----b2df5608a141--------------------------------)**给我指出的，你不需要两个都装，Miniconda 和 Miniforge。你可以像这里说的一样选择是使用针对 Intel 芯片编译的包还是针对 Apple Silicon 编译的包:**[**https://github . com/Haydnspass/miniforge # Rosetta-on-MAC-with-Apple-Silicon-hardware**](https://github.com/Haydnspass/miniforge#rosetta-on-mac-with-apple-silicon-hardware)**。**

安装 Miniforge 后，初始化命令将在您的。bashrc/。zshrc:

```
# >>> conda initialize >>>
# !! Contents within this block are managed by ‘conda init’ !!
__conda_setup=”$(‘/Users/xyz/miniforge3/bin/conda’ ‘shell.zsh’ ‘hook’ 2> /dev/null)”
if [ $? -eq 0 ]; then
 eval “$__conda_setup”
else
 if [ -f “/Users/xyz/miniforge3/etc/profile.d/conda.sh” ]; then
 . “/Users/xyz/miniforge3/etc/profile.d/conda.sh”
 else
 export PATH=”/Users/xyz/miniforge3/bin:$PATH”
 fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

这将使用 Miniforge 初始化 conda。你只需要复制你的。bashrc/。zshrc 文件，并将 miniforge3 更改为 miniconda3，并选择默认情况下要使用的文件。更改非常简单，只需运行`source .bashrc`进行所需的 conda 初始化。

例如，我正在做一个项目，在这个项目中，我需要 SimpleITK 来预处理图像，需要 Tensorflow 来训练一个模型。我无法让两者在 M1 的同一个小型锻造环境中工作。所以我把预处理和训练分成两个环境，一个利用 Miniconda 运行 SimpleITK，一个 Miniforge 环境运行 Tensorflow。

它的好处是你可以通过运行`conda env list`同时看到 Miniconda 和 Miniforge 环境。唯一的区别是，您将看不到用其他安装程序构建的环境的名称，只能看到路径。初始化 Miniconda 后，您需要使用 Miniforge 环境的完整路径运行`conda activate`。

这在 bash 脚本中仍然很容易管理，以便使用由多个 conda 发行版构建的多个环境来运行脚本。

我希望这只是一个临时的解决办法，直到越来越多的软件包可以在 M1 上工作，但是我确信这需要一些时间。

希望这能帮助一些在 M1 上使用 conda 处理 Python 包的人。