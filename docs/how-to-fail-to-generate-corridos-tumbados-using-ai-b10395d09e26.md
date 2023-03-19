# 如何使用人工智能生成“corridos tumbados”失败

> 原文：<https://towardsdatascience.com/how-to-fail-to-generate-corridos-tumbados-using-ai-b10395d09e26?source=collection_archive---------41----------------------->

## “Corridos tumbados”和机器学习

![](img/04ab2dc20d26975ddd148bfb582026bb.png)

俄罗斯摄影师在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> Eliud Gilberto Rodríguez Martínez、Hugo Francisco Cano Beyliss、Martin joséVega Noriega。
> 
> 索诺拉大学，埃莫西约，索诺拉，
> 
> [mjvnor@outlook.com](mailto:mjvnor@outlook.com)，slabxcobra@gmail.com，eliud.giroma@gmail.com。

**摘要。**该项目旨在使用神经网络为 Julio Waissman Vilanova 教授的神经网络课程生成“corridos tumbados”。我们承担了使用不同方法生成歌曲的任务，使用不同的神经网络，如 RNNs、WaveNets 和合成器。在这里，我们解释了为什么我们能够执行这项任务，但不便之处在于，由于执行这项任务时出现的各种问题，如数据集有大量噪声(他们有不止一台仪器)，以及没有处理大型网络所需的设备，结果不如预期。

**关键词:** corridos tumbados，神经网络，lstm，wavenet，n-synth，keras

# 为什么我们决定做这个项目

我们决定做这个项目是因为 Julio Waissman Vilanova 教授在神经网络课上分配给我们的一项任务，这项任务是关于使用递归神经网络(RNN)创作音乐，我们决定借此机会创作主要在墨西哥北部非常流行的“corridos tumbados”。

## 什么是 corrido tumbado？

Corridos tumbados 是墨西哥地区的变体，是传统 corrido 的变体，但包含了 hip-hop 和 trap，他们的歌手通常穿着 trap 的特色服装。你绝对应该听听。

# 我们所做的…

## 我们尝试的第一件事

在我们的例子中，没有 corridos tumbados 的 MIDIs 数据集，所以我们开始创建一个。首先，我们想用这些类型歌曲的所有乐器来生成 corridos tumbados，但我们意识到这太费时且难以实现，所以我们使用了“SpleeterGui”软件来获取我们手头的 corridos tumbados 的 requintos。为什么要求？requinto 是 corridos tumbados 的主要乐器，没有这个乐器就不是 corridos tumbados。我们使用谷歌提供的一个名为“Onsets and Frames”的人工智能，这是一个带有钢琴和鼓模型的自动音乐转录框架，它将帮助我们将安魂曲转换为 MIDIs。

*   Colab:

[](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/Onsets_and_Frames.ipynb) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/Onsets_and_Frames.ipynb) 

我们生成了大约 241 个 MIDI 格式的 corridos tumbados，在一个名为“music21”的库的帮助下，我们从中提取音符和和弦，并将它们用作我们网络的输入。然后，我们用一个长短期记忆递归神经网络(LSTM)来产生谎言运行，这是结果:

*   Colab:

[](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/GeneratorMusicLstm.ipynb) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/GeneratorMusicLstm.ipynb) 

## 我们失败的原因

我们失败了，因为我们的数据集不够好，它包含了太多的噪音和除了 requintos 以外的其他乐器。我们还需要做更多的 epoch 来更好地生成歌曲，这是一个问题，因为如果我们给它更多的 epoch，它需要超过 15 个小时才能完成，当你超过这个时间时，google colab 不再允许你使用他们的 gpu。

## 我们尝试的第二件事

我们尝试的第二件事是使用一个名为 N-SYNTH 的 magenta 项目，这是一个神经合成器。N-SYNTH 合成器操纵数据而不是操纵声音；分析音频文件，了解数据之间的关系，然后从头开始制作新的音频文件。要听质量接近现实生活的音乐，需要 48000 个样本。这个合成器基于上面的几千个样本，以及经过训练的音频，一次生成一个样本，听起来有点差，因为使用了 16，000 个样本，而不是 48，000 个样本，这仍然是相当多的。

*   结果:

*   Colab:

[](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/nsynth.ipynb) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/nsynth.ipynb) 

该算法用高音声音训练，它用来自一千种不同乐器的 30 万个音符训练。当您组合两种完全不同的声音时，合成器会更加复杂，因为它必须处理更多的声音，而不仅仅是单个音符，除了合成器在高音方面受过训练，所以它会尝试将任何其他频率解释为高音，这意味着，除了通过使用 16，000 个样本来创建低质量的旋律之外，由于可能缺乏乐器，并希望将频率提高，从而产生看起来像恐怖电影一样的音频畸变。

## 我们尝试的最后一件事

我们最后尝试使用的是波网。使用 WaveNet 架构的简化版本，而不添加剩余连接或跳过连接，因为这些层的作用是提高快速收敛，并且 WaveNet 将原始音频波作为输入，但是在我们的情况下，输入将是一组节点和和弦，因为我们正在生成音乐。

结果:

少年 H(最佳 5 名)| 50 岁

少年 H(最佳 5 名)| 500 纪元

完整数据集| 50 个纪元

完整数据集| 200 个历元

*   Colab:

[](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/AutoMusicGen.ipynb) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/github/MJVNOR/DatasetMidiCorridosTumbadosAcustico/blob/main/AutoMusicGen.ipynb) 

## 我们失败的原因

有几种可能性，但我们认为该模型未能提取歌曲的重要特征，这是因为该模型仅适用于一种类型的乐器，并且由于我们的数据集不如我们希望的那样干净，这引入了影响学习阶段的噪声，此外，该模型可能需要更多的时期来训练或更多的歌曲。

# 什么是 LSTM？

长短期记忆(LSTM)网络是一种递归神经网络，它扩展了递归神经网络的记忆，它们有短期记忆问题，所以它们只使用以前的信息，所以它们没有神经节点可用的所有上述信息的列表。由于它们特有的行为，它们被用于解决语音识别问题、手写识别、音乐创作等。递归神经网络无法解决的复杂问题。

# 什么是 Wavenet？

WaveNet 是一种称为深度卷积神经网络的神经网络，用于生成逐个样本的音频。该技术能够通过使用直接来自真实人类语音样本的经过训练的神经网络直接对波形进行建模来生成逼真的声音，例如人类语音。Wavenet 生成波形的能力允许对任何类型的音频进行建模，包括音乐。在 WaveNet 中，网络采样信号作为输入，并逐个采样输出进行合成。

# 什么是插值？

插值包括利用特定数据集的信息进行有根据的猜测。根据你手头的信息，这是一个“最佳猜测”。插值是我们经常使用的人工智能。把数据输入计算机，让它为我们做出有根据的猜测。理论上，内插法对于提取有关情况的数据和利用已知经验将知识扩展到不熟悉的领域也很有用。然而，这通常被称为外推。

# 结论

我们的结论是，我们必须努力改进我们的数据集，如果它更干净，我们第二次尝试的输出就会有很大的改进。除了 google colab 之外，我们还应该使用另一个系统来长时间运行模型。

# Github 知识库

[](https://github.com/MJVNOR/DatasetMidiCorridosTumbadosAcustico) [## MJVNOR/DatasetMidiCorridosTumbadosAcustico

### 通过在 GitHub 上创建一个帐户，为 MJVNOR/DatasetMidiCorridosTumbadosAcustico 开发做出贡献。

github.com](https://github.com/MJVNOR/DatasetMidiCorridosTumbadosAcustico) 

# 参考

霍桑，c .、埃尔森，e .、宋，j .、罗伯茨，a .、西蒙，I .、拉弗尔，c .、… &埃克，D. (2017)。双目标钢琴改编曲。arXiv 预印本 arXiv:1710.11153。

[](https://deepai.org/machine-learning-glossary-and-terms/interpolation) [## 插入文字

### 插值是利用特定数据集内的信息进行有根据的猜测。这是一个“最佳猜测”,使用…

deepai.org](https://deepai.org/machine-learning-glossary-and-terms/interpolation)  [## 维基百科，自由百科全书

### 波网是一个红色的神经元深度超过一般的音频 muesta 一 muesta。企业投资研究中心

es.wikipedia.org](https://es.wikipedia.org/wiki/WaveNet) [](https://hub.packtpub.com/what-is-lstm/) [## 什么是 LSTM？包装中心

### LSTM 代表长期短期记忆。它是一种有效“扩展”内存的方法或架构…

hub.packtpub.com](https://hub.packtpub.com/what-is-lstm/)