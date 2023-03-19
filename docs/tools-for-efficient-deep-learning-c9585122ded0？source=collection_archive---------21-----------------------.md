# 高效深度学习的工具

> 原文：<https://towardsdatascience.com/tools-for-efficient-deep-learning-c9585122ded0?source=collection_archive---------21----------------------->

## 鉴于 PyTorch 和 Keras 的便利性，深度学习很有趣，但如果我们不能高效、聪明地进行实验，它也会令人厌倦。

做深度学习已经一年多了。PyTorch 和 Keras 之类的库使得实现深度模型变得更加容易，并提供了许多深度架构的标准实现，如 CNN、LSTM、soft max、embeddings 等。如果我们可以可视化或提出一个基于深度学习的问题解决方案，那么给定数据集，就可以很容易地证明给定的主张是否有意义。进行深度学习可能既有趣又令人厌倦，因为缺乏高质量的 GPU 导致训练缓慢，有时不可预测，难以调试，耗时，并且依赖于数据集的规模。根据我的经验，我发现下面的库非常方便有效地执行实验。

![](img/b2e1872b610e1489863213926063a78e.png)

深度学习很好玩。不是吗？(图片由:Pixabay，[链接](https://www.pexels.com/photo/time-lapse-photography-of-blue-lights-373543/))

[**九头蛇**](https://hydra.cc/docs/intro/) —为整个实验提供一个配置文件。我们可以设置不同的参数。当我们想与其他人共享我们的代码或者在不同的机器上运行实验时，它会非常有帮助。它为我们提供了设置所需配置的灵活性，如学习速率、模型隐藏层大小、时期、数据集名称等。而不暴露某人对实际代码的更改。

当我们想要将代码移植到不同的机器上并安装所有的依赖项时，这证明是非常有用的。它帮助我们创建一个 python 依赖项列表，以及我们当前工作代码正在使用的版本，并将它们保存在一个文件中，该文件可以很容易地安装到其他任何地方。

```
>> pipreqs
INFO: Successfully saved requirements file in D:\blogs\requirements.txt 
```

要在不同的机器上成功安装这些依赖项，请复制 requirements.txt 文件并运行

```
>>pip install -r requirements.txt
```

**G**[**etopt**](https://docs.python.org/3/library/getopt.html)—可用于通过命令行参数传递参数。它为我们提供了设置强制参数和参数默认值的灵活性。argparse 库是另一个流行的选择。

运行以下命令，并将参数作为命令行参数传递。

```
>>python getopt_demo.py -r demo_experiment -d ./ -t CNN -l 0.4
```

[**tensor board**](https://pytorch.org/tutorials/intermediate/tensorboard_tutorial.html)**—**它用于通过记录和绘制训练损失、训练准确度等指标来测量我们实验进度的实时图。张量板日志是 Summarywriter 对象的一部分，它跟踪某些变量，如训练损失、验证损失、训练精度、验证精度等。并把它绘制得很漂亮，便于可视化。当我们想要实时或在稍后的时间点可视化我们的结果时，这就很方便了。

要查看 cmd 中运行的实时进度

```
>> tensorboard --logdir=runs
Serving TensorBoard on localhost; to expose to the network, use a proxy or pass --bind_all
TensorBoard 2.4.1 at [http://localhost:6006/](http://localhost:6006/) (Press CTRL+C to quit)
```

[**Tqdm**](https://tqdm.github.io/)**——**作为初学者，我曾经手动记下一个历元的训练时间，真的很繁琐。Tqdm 与循环一起使用时(这里我们使用 torch.utils.data.DataLoader 对象上的循环),可以很好地描述每个梯度步骤或时期的时间，这可以帮助我们设置不同结果的记录频率或保存模型，或者了解如何设置验证间隔。

[**logu ru**](https://loguru.readthedocs.io/en/stable/api/logger.html)**—它提供了记录配置、实验名称和其他训练相关数据的记录器功能。当我们做多个实验并想要区分不同实验的结果时，这被证明是相当有帮助的。因此，如果我们记录实际的配置和结果，那么就更容易将适当的设置映射到我们得到的输出。**

**[**Pickle**](https://docs.python.org/3/library/pickle.html) —可用于保存和加载 python 类或 PyTorch 模型以供重用。例如，在涉及文本预处理的实验中，我们需要存储训练数据集的词汇和相应的嵌入，以及 word2index 和 index2word 字典。我们可以对对象进行 pickle，并在将来加载它，以节省预处理的时间。**

**这些库节省了大量的时间，帮助我们保持实现的整洁，并使实验结果更有说服力。让我们让深度学习变得有趣和简单。**