# 如何有效地管理部署的模型

> 原文：<https://towardsdatascience.com/how-to-effectively-manage-deployed-models-6c35ced55f92?source=collection_archive---------40----------------------->

## 通过 Tensorflow 服务管理您的模型的生命周期

![](img/fb15675e2642e66fdd47cc87ba1a80bc.png)

来源( [Unsplash](https://unsplash.com/) )

大多数型号从未投入生产。我们之前看了[使用 Tensorflow 服务部署 Tensorflow 模型](/putting-your-models-into-production-5ae3191722b9)。一旦这一过程完成，我们可能会认为我们的工作已经全部完成。事实上，我们刚刚开始了管理模型生命周期的新旅程，并确保它保持最新和有效。

像软件中的大多数东西一样，需要持续的开发和改进。一旦模型被部署，管理它的任务是一个经常被忽视的任务。在这里，我们将看看如何有效地做到这一点，并使我们的模型管道更有效。

**多型号配置**

当服务模型时，最简单的方法是指定`MODEL_NAME`环境变量，如这里的[所示](/putting-your-models-into-production-5ae3191722b9#12d0)。但是，如果我们想为多种型号服务呢？不同的配置选项，比如使用多少个线程或轮询频率，会怎么样？在这种情况下，我们可以使用如下所示的模型服务器配置文件。

模型服务器配置文件

我们使用模型配置列表选项，它是模型配置协议缓冲区的列表。每个 ModelConfig 都应该指定一个要服务的模型。上面我们看到两个模型，分别命名为`first_model`和`second_model`。我们还需要指定一个`base_path`，服务器将在这里寻找模型的版本以及`model_platform`。我们的示例展示了在 TensorFlow 上运行的两个模型。

一旦我们有了 ModelServer 配置文件，我们就可以使用如下所示的命令运行 docker 容器。

```
docker run --rm -p 8501:8501 -v "$(pwd)/models/:/models/" \
-t tensorflow/serving **--model_config_file=/models/models.config \
--model_config_file_poll_wait_seconds=60**
```

在之前，我们已经看过这个`docker run`命令[。唯一的区别是我们在最后增加了`--model_config_file`和`--model_config_file_poll_wait_seconds`选项。`--model_config_file`选项设置为我们的模型服务器配置文件，而`--model_config_file_poll_wait_seconds`选项设置为 60。这意味着服务器每分钟都会在指定位置检查新的配置文件，并根据需要进行更新。](/putting-your-models-into-production-5ae3191722b9#7fac)

**型号版本和版本标签**

当 Tensorflow Serving 搜索一个模型的`base_path`时，其默认行为是总是根据版本号服务于*最新的*模型。如果在我们的路径中有版本 1 到 5，版本 5 将总是被服务。如果我们想提供更早的版本呢？还是为同一型号的多个版本服务？在这种情况下，我们可以在配置文件中指定 model_version_policy，并将其设置为`specific`，提供我们想要提供的版本号。

```
model_version_policy {
  specific {
    versions: 2
  }
}
```

通过将上面的代码片段添加到我们的配置文件中，我们的服务器将知道提供我们模型的版本 2，而不是最新的版本。如果我们想一次提供多个版本，我们可以简单地添加更多的版本。

```
model_version_policy {
  specific {
    versions: 2
    versions: 3
    versions: 4
  }
}
```

我们还可以给模型版本分配字符串标签，这样我们可以更容易地记住它们。

```
model_version_policy {
  specific {
    versions: 2
    versions: 3
    versions: 4
  }
}
version_labels {
  key: 'stable'
  value: 2
}
version_labels {
  key: 'canary'
  value: 3
}
version_labels {
  key: 'dev'
  value: 4
}
```

上面我们已经给我们的模型版本分配了标签`stable`、`canary`和`dev`。如果我们想将版本 3 升级到稳定版本，我们可以简单地将`stable`的值从 2 改为 3。类似地，如果我们想让版本 4 成为新的 canary 版本，我们只需要改变相应的值。

**A/B 型测试**

一个常见的业务场景是模型 A/B 测试。总有一天，我们会想看看开发中的模型与当前生产中的模型相比如何。有了 Tensorflow 的服务，A/B 测试变得容易多了。

在上面的代码片段中，我们创建了一个名为`get_request_url`的助手函数，它返回一个我们可以发送 post 请求的 URL。关于这一点，我们可以设置一些阈值(本例中为 0.05)，将一定比例的流量路由到不同的模型版本。您还可以对此进行归纳，以便我们可以将流量路由到任意数量的模型版本。

如果我们想慢慢地扩大金丝雀模型版本，我们可以在我们认为合适的时候增加这个阈值，直到金丝雀版本处理大部分传入流量。可以想象，使用这种类型的设置进行回滚也非常简单。

**迷你批处理推论**

Tensorflow 服务的一个非常有用的特性是能够批量处理请求以增加吞吐量。如果我们一次发出一个请求，我们的服务器将会被占用，在接受任何新的请求之前等待每个请求完成。类似于我们在训练模型时如何批量训练示例，我们也可以将几个推理请求批量在一起，这样模型就可以一次性处理它们。为此，我们将创建一个`batch_parameters.txt`文件来保存我们的批处理配置。

一个简单的批处理配置文件

在上面的文件中，我们将一个批处理中的最大请求数设置为 256，最大入队批处理数设置为 1000000，最大并发线程数设置为 4。更详尽的可用选项列表可以在[这里](https://github.com/tensorflow/serving/blob/master/tensorflow_serving/batching/README.md)的文档中找到。

一旦我们有了这个文件，我们就可以通过向我们的`docker run command`添加两个选项来启用批处理。

```
docker run --rm -p 8501:8501 -v "$(pwd)/models/:/models/" \
-t tensorflow/serving --model_config_file=/models/models.config \
--model_config_file_poll_wait_seconds=60 **--enable_batching=true \
--batching_parameters_file=/models/batching_parameters.txt**
```

我们只需将`--enable_batching`选项设置为真，并将我们的文件传递给`--batching_parameters_file`选项。就这么简单！通过在我们的服务器中启用批处理，我们可以显著提高吞吐量，从而可以处理更大的流量。不需要额外的服务器！

**结论**

我们研究了 Tensorflow 服务的不同特性，这些特性使得管理模型部署和维护变得更加容易。当我们想要一次运行多个模型(或者并行运行同一模型的多个版本)时，我们使用一个 **ModelServer 配置文件**来指定每个模型的配置。我们还研究了**为特定模型**分配字符串值，以便更容易跟踪生产中的产品和非生产中的产品。我们还讨论了 **A/B 测试**，并看到将流量路由到我们模型的不同版本来比较结果是多么容易。最后，我们看了一下**批处理**，这是一种并发处理大量推理请求的简单方法，因此我们可以增加吞吐量。

感谢您的阅读！

您可以通过以下渠道与我联系:

*   [中等](https://zito-relova.medium.com/)
*   [领英](https://www.linkedin.com/in/zrelova/)。
*   [Github](https://github.com/zitorelova)
*   [卡格尔](https://www.kaggle.com/zitorelova)