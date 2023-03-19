# 关于如何在机器学习项目中使用 Hydra 的完整教程

> 原文：<https://towardsdatascience.com/complete-tutorial-on-how-to-use-hydra-in-machine-learning-projects-1c00efcc5b9b?source=collection_archive---------3----------------------->

## 学习你需要知道的关于如何在你的机器学习项目中使用 Hydra 的一切。Hydra 的所有特性都将通过一个虚拟 ML 示例进行讨论。

为了提高 PyTorch 生态系统的标准化，脸书·艾在最近的[博客文章](https://ai.facebook.com/blog/reengineering-facebook-ais-deep-learning-platforms-for-interoperability/)中表示，他们将利用脸书的开源 [Hydra 框架](https://hydra.cc/)来处理配置，并提供与 [PyTorch Lightning](https://www.pytorchlightning.ai/) 的集成。这个帖子是关于九头蛇的。

如果你正在阅读这篇文章，那么我假设你熟悉什么是配置文件，为什么它们有用，以及它们如何增加可再现性。而且你也知道什么是噩梦 [argparse](https://docs.python.org/3/library/argparse.html) 。一般来说，通过配置文件，你可以将所有的超参数传递给你的模型，你可以定义所有的全局常量，定义数据集分割，等等，而不需要接触你的项目的核心代码。

在[九头蛇网站](https://hydra.cc/docs/intro)上，以下列出了九头蛇的主要特征:

*   可从多个来源组合的分层配置
*   可以从命令行指定或覆盖配置
*   动态命令行制表符结束
*   在本地运行您的应用程序，或者启动它以远程运行
*   用一个命令运行具有不同参数的多个作业

在这篇文章的其余部分，我将通过一个用例的例子来逐一介绍 Hydra 的特性。所以跟着走吧，这将是一次有趣的旅程。

# 了解 Hydra 设置流程

安装 Hydra(我用的是版本`1.0`)

```
pip install hydra-core --upgrade
```

对于这篇博文，我将假设下面的目录结构，其中所有的配置都存储在一个`config`文件夹中，主配置文件被命名为`config.yaml`。为了简单起见，假设`main.py`是我们项目的所有源代码。

```
src
├── config
│   └── config.yaml
└── main.py
```

让我们从一个简单的例子开始，它将向您展示使用 Hydra 的主要语法，

```
*### config/config.yaml*

batch_size: 10
lr: 1e-4
```

以及相应的`main.py`文件

```
*### main.py* 
import hydra
from omegaconf import DictConfig

**@**hydra.main(config_path**=**"config", config_name**=**"config")
**def** **func**(cfg: DictConfig):
    working_dir **=** os.getcwd()
    **print**(f"The current working directory is {working_dir}")

    *# To access elements of the config
*    **print**(f"The batch size is {cfg.batch_size}")
    **print**(f"The learning rate is {cfg['lr']}")

**if** __name__ **==** "__main__":
    func()
```

运行该脚本将得到以下输出

```
> python main.py    
The current working directory is src/outputs/2021-03-13/16-22-21

The batch size is 10
The learning rate is 0.0001
```

> ***注*** *:路径被缩短，不包括从根开始的完整路径。同样，您可以将* `*config.yaml*` *或* `*config*` *传递给* `*config_name*` *。*

发生了很多，我们一个一个解析吧。

*   `omegaconf`默认安装有`hydra`。它仅用于为`func`中的`cfg`参数提供类型注释。
*   `@hydra.main(config_path="config", config_name="config")`这是当任何函数需要配置文件中的内容时使用的主装饰函数。
*   **当前工作目录被更改**。`main.py`存在于`src/main.py`中，但输出显示当前工作目录是`src/outputs/2021-03-13/16-22-21`。这是使用九头蛇时最重要的一点。解释如下。

## 九头蛇如何处理不同的运行

每当使用`python main.py`执行一个程序时，Hydra 将在`outputs`目录中创建一个新文件夹，其命名方案如下`outputs/YYYY-mm-dd/HH-MM-SS`，即文件执行的日期和时间。想一想这个。Hydra 为您提供了一种维护每次运行日志的方法，而您不必为此担心。

执行`python main.py`后的目录结构是(现在先不要担心每个文件夹的内容)

```
src
├── config
│   └── config.yaml
├── main.py
├── outputs
│   └── 2021-03-13
│       └── 17-14-24
│           ├── .hydra
│           │   ├── config.yaml
│           │   ├── hydra.yaml
│           │   └── overrides.yaml
│           ├── main.log
```

实际上发生了什么？当你运行`src/main.py`时，hydra 将这个文件移动到`src/outputs/2021-03-13/16-22-21/main.py`然后运行它。您可以通过检查`os.getcwd()`的输出来验证这一点，如上例所示。这意味着如果你的`main.py`依赖于某个外部文件，比如说`test.txt`，那么你将不得不使用`../../../test.txt`来代替，因为你不再运行`src`目录中的程序。这也意味着您保存到磁盘的所有内容都是相对于`src/outputs/2021-03-13/16-22-21/`保存的。

Hydra 提供了两个实用函数来处理这种情况

*   **hydra . utils . Get _ original _ CWD()**:获取原当前工作目录，即`src`。

```
orig_cwd **=** hydra.utils.get_original_cwd()
path **=** f"{orig_cwd}/test.txt"*# path = src/test.txt*
```

*   **hydra . utils . to _ absolute _ path(文件名):**

```
path **=** hydra.utils.to_absolute_path('test.txt')

*# path = src/test.txt*
```

让我们用一个简短的例子来概括一下。假设我们想读取`src/test.txt`并将输出写入`output.txt`。完成此操作的相应函数如下所示

```
**@**hydra.main(config_path**=**"config", config_name**=**"config")
**def** **func**(cfg: DictConfig):
    orig_cwd **=** hydra.utils.get_original_cwd()

    *# Read file
*    path **=** f"{orig_cwd}/test.txt"
    **with** open(path, "r") **as** f:
        **print**(f.read())

    *# Write file
*    path **=** f"output.txt"
    **with** open(path, "w") **as** f:
        f.write("This is a dog")
```

在运行`python main.py`之后，我们可以再次检查目录结构。

```
src
├── config
│   └── config.yaml
├── main.py
├── outputs
│   └── 2021-03-13
│       └── 17-14-24
│           ├── .hydra
│           │   ├── config.yaml
│           │   ├── hydra.yaml
│           │   └── overrides.yaml
│           ├── main.log
│           └── output.txt
└── test.txt
```

文件被写入 hydra 创建的文件夹。当你在开发一些东西的时候，这是一个保存中间结果的好方法。您可以使用此功能保存具有不同超参数的模型的精度结果。现在，您不必花费时间手动保存用于运行脚本的配置文件或命令行参数，也不必为每次运行创建一个新文件夹来存储输出。

> ***注意*** *:每个* `*python main.py*` *都运行在一个新的文件夹中。为了保持上面的输出简短，我删除了之前运行的所有子文件夹。*

主要的一点是使用`orig_cwd = hydra.utils.get_original_cwd()`来获得原始的工作目录路径，这样你就不必担心 hydra 在不同的文件夹中运行你的代码。

## 每个子文件夹的内容

每个子文件夹都有以下子结构

```
src/outputs/2021-03-13/17-14-24/
├── .hydra
│   ├── config.yaml
│   ├── hydra.yaml
│   └── overrides.yaml
└── main.log
```

*   `config.yaml` -传递给函数的配置文件的副本(如果您传递了`foo.yaml`也没关系，该文件仍将被命名为`config.yaml`)
*   `hydra.yaml`-hydra 配置文件的副本。我们稍后将看到如何更改 hydra 使用的一些默认设置。(您可以在此指定`python main.py --help`的消息)
*   `overrides.yaml` -您通过命令行提供的任何参数的副本，它改变了一个缺省值，将被存储在这里
*   `main.log` -记录器的输出将存储在此处。(对于`foo.py`，该文件将被命名为`foo.log`)

## 如何使用日志记录

有了 Hydra，你可以在代码中轻松使用 Python 提供的[日志](https://docs.python.org/3/library/logging.html)包，无需任何设置。日志的输出存储在`main.log`中。下面显示了一个使用示例

```
import logging

log **=** logging.getLogger(__name__)

**@**hydra.main(config_path**=**"config", config_name**=**"config")
**def** **main_func**(cfg: DictConfig):
    log.debug("Debug level message")
    log.info("Info level message")
    log.warning("Warning level message")
```

在这种情况下，`python main.py`的日志将会是(在`main.log`中)

```
[2021-03-13 17:36:06,493][__main__][INFO] - Info level message
[2021-03-13 17:36:06,493][__main__][WARNING] - Warning level message
```

如果您想将`DEBUG`也包括在内，那么覆盖`hydra.verbose=true`或`hydra.verbose=__main__`(即`python main.py hydra.verbose=true`)。在这种情况下，`main.log`的输出将是

```
[2021-03-13 17:36:38,425][__main__][DEBUG] - Debug level message
[2021-03-13 17:36:38,425][__main__][INFO] - Info level message
[2021-03-13 17:36:38,425][__main__][WARNING] - Warning level message
```

# OmegaConf 快速概述

OmegaCong 是一个基于 YAML 的分层配置系统，支持合并来自多个来源(文件、CLI 参数、环境变量)的配置。你只需要知道 YAML 就能使用九头蛇。OmegaConf 是九头蛇在后台用的，帮你处理一切。

您需要知道的主要内容显示在下面的配置文件中

```
server:
  ip: "127.0.0.1"
  port: ???       *# Missing value. Must be provided at command line*
  address: "${server.ip}:${server.port}" *# String interpolation*
```

现在在`main.py`中，您可以如下访问服务器地址

```
**@**hydra.main(config_path**=**"config", config_name**=**"config")
**def** **main_func**(cfg: DictConfig):
    server_address **=** cfg.server.address
    **print**(f"The server address = {server_address}")

*# python main.py server.port=10
# The server address = 127.0.0.1:10*
```

从上面的例子中你可以猜到，如果你想让某个变量和另一个变量取相同的值，你应该使用下面的语法`address:${server.ip}`。我们稍后将看到一些有趣的用例。

# 在 ML 项目中使用 Hydra

现在你知道了 hydra 的基本工作原理，我们可以专注于使用 Hydra 开发一个机器学习项目。查看这篇文章之后的 hydra [文档](https://hydra.cc/docs/intro)，了解这里没有讨论的一些内容。我也不会在这篇文章中讨论 [*结构化配置*](https://hydra.cc/docs/tutorials/structured_config/intro)(YAML 文件的替代文件)，因为没有它们你也可以完成所有的事情。

回想一下，我们项目的`src`目录有如下结构

```
src
├── config
│   └── config.yaml
└── main.py
```

我们有一个单独的文件夹来存储我们所有的配置文件(`config`)，我们项目的源代码是`main.py`。现在让我们开始吧。

## 资料组

每个 ML 项目都是从收集数据和创建数据集开始的。在进行影像分类项目时，我们会使用许多不同的数据集，如 ImageNet、CIFAR10 等。每个数据集都有不同的相关超参数，如批次大小、输入影像的大小、类的数量、用于特定数据集的模型的层数等等。

我没有使用一个特定的数据集，而是使用一个随机的数据集，因为它可以使事情变得通用，你可以将这里讨论的东西应用到你自己的数据集上。此外，我们不要担心创建数据加载器，因为它们是一回事。

在讨论细节之前，让我向您展示代码，您可以很容易地猜到发生了什么。本例中涉及的 4 个文件是

*   `src/main.py`
*   `src/config/config.yaml`
*   `src/config/dataset/dataset1.yaml`
*   `src/config/dataset/dataset2.yaml`

```
*### src/main.py ###* 
import torch
import hydra
from omegaconf import DictConfig

**@**hydra.main(config_path**=**"config", config_name**=**"config.yaml")
**def** **get_dataset**(cfg: DictConfig):
    name_of_dataset **=** cfg.dataset.name
    num_samples **=** cfg.num_samples

    **if** name_of_dataset **==** "dataset1":
        feature_size **=** cfg.dataset.feature_size
        x **=** torch.randn(num_samples, feature_size)
        **print**(x.shape)
        **return** x

    **elif** name_of_dataset **==** "dataset2":
        dim1 **=** cfg.dataset.dim1
        dim2 **=** cfg.dataset.dim2
        x **=** torch.randn(num_samples, dim1, dim2)
        **print**(x.shape)
        **return** x

    **else**:
        **raise** ValueError("You outplayed the developer")

**if** __name__ **==** "__main__":
    get_dataset()
```

相应的配置文件是，

```
*### src/config/config.yaml*
defaults:
  - dataset: dataset1

num_samples: 2 *### src/config/dataset/dataset1.yaml*

*# @package _group_*
name: dataset1
feature_size: 5 *### src/config/dataset/dataset1.yaml*

*# @package _group_*
name: dataset2
dim1: 10
dim2: 20
```

老实说，这几乎是你在项目中使用 hydra 所需要的一切。让我们看看上面的代码中实际发生了什么

*   在`src/main.py`中，您将看到一些通用变量，即`cfg.dataset`和`cfg.num_samples`，它们在所有数据集之间共享。这些是在主配置文件中定义的，我们使用命令`@hydra.main(...)`传递给 hydra。
*   接下来，我们需要定义一些特定于每个数据集的变量(比如 ImageNet 和 CIFAR10 中的类的数量)。为了在 hydra 中实现这一点，我们使用以下语法

```
defaults:
  - dataset: dataset1
```

*   这里的`dataset`是文件夹的名称，该文件夹将包含每个数据集的所有对应的 *yaml* 文件(即本例中的`dataset1`和`dataset2`)。所以目录结构看起来像这样

```
config
  ├── config.yaml
  └── dataset
      ├── dataset1.yaml
      └── dataset2.yaml
```

*   就是这样。现在，您可以在上述每个文件中定义特定于每个数据集的变量，这些变量彼此独立。
*   这些被称为[配置组](https://hydra.cc/docs/tutorials/basic/your_first_app/config_groups)。每个配置文件都独立于文件夹中的其他配置文件，我们只能选择其中一个配置文件。为了定义这些配置组，您需要在每个文件`# @package _group_`的开头包含一个特殊的注释。

> *我们只能从*`*dataset1.yaml*`*`*dataset2.yaml*`*中选择一个配置文件作为* `*dataset*` *的值。为了告诉 hydra 这些是配置组，我们需要在这些文件的开头加上特殊的注释* `*# @package _group_*` *。**
> 
> ****注*** *:在九头蛇 1.1 中，* `*_group_*` *将成为默认的* `*package*` *，无需添加特殊注释。**

*   *什么是**默认值**？在我们的主配置文件中，我们需要某种方法来区分普通的字符串值和配置组值。就像在这种情况下，我们希望将`dataset: dataset1`解释为一个配置组值，而不是一个字符串值。为此，我们在`defaults`中定义了所有的配置组。正如您所猜测的，您为它提供了一个默认值。*

> ****注意*** *:* `*defaults*` *需要一个列表作为输入，所以需要每个名字都以一个* `*-*` *开头。**

```
*defaults:
  - dataset: dataset1 *# By default use `dataset/dataset1.yaml*

*## OR*

defaults:
  - dataset: ???  *# Must be specified at command line**
```

*我们可以检查上面代码的输出。*

```
*> python dataset.py
torch.Size([2, 5])*
```

*和*

```
*> python dataset.py dataset=dataset2
torch.Size([2, 10, 20])*
```

*现在停下来想一想。您可以使用同样的技术为所有优化器定义超参数值。只需创建一个名为`optimizer`的新文件夹，并写入`sgd.yaml`、`adam.yaml`文件。而在主`config.yaml`中，你只需要多加一行*

```
*defaults:
  - dataset: dataset1
  - optimizer: adam*
```

*您还可以使用它来创建学习率调度器、模型、评估指标和几乎所有其他东西的配置文件，而不必在主代码库中实际硬编码任何这些值。您不再需要记住用于运行该模型的学习率，因为用于运行 python 脚本的配置文件的备份始终存储在 hydra 创建的文件夹中。*

## *模型*

*有一种特殊情况你也需要知道。当使用 ImageNet vs CIFAR10 时，如果您希望您的 ResNet 模型具有不同的层数，该怎么办？天真的解决方案是在每个数据集的模型定义中添加`if-else`条件，但这是一个糟糕的选择。如果明天你添加一个新的数据集。现在您必须修改您的模型`if-else`条件来处理这个新的数据集。因此，我们在配置文件中定义一个值`num_layers`，然后我们可以使用这个值来创建我们想要的层数。*

*假设我们使用两个模型，resnet 和 vgg。根据上一主题中的讨论，我们将为每个模型创建一个单独的配置文件。`config`文件夹的目录结构应该是*

```
*config
├── config.yaml
├── dataset
│   ├── cifar10.yaml
│   └── imagenet.yaml
└── model
    ├── resnet.yaml
    └── vgg.yaml*
```

*现在，假设我们希望 resnet 模型在使用 CIFAR10 时有 34 个层，而其他数据集有 50 个层。在这种情况下，`config/model/resnet.yaml`文件应该是*

```
**# @package _group_*
name: resnet
num_layers: 50 *# As 50 is the default value**
```

*现在，我们希望在用户指定 CIFAR10 数据集时设置值`num_layers=34`。为此，我们可以定义一个新的配置组，在其中我们可以定义所有特殊情况的组合。在主要的`config/config.yaml`中，我们将进行以下更改*

```
*defaults:
  - dataset: imagenet
  - model: resnet
  - dataset_model: ${defaults.0.dataset}_${defaults.1.model}
    optional: true*
```

*这里我们创建了一个名为`dataset_model`的新配置组，它采用由`dataset`和`model`指定的值(就像`imagenet_resnet`、`cifar10_resnet`)。这是一个奇怪的语法，因为`defaults`是一个列表，所以你需要在名字前指定索引，例如`defaults.0.dataset`。现在我们可以在`dataset_model/cifar10_resnet.py`中定义配置文件*

```
**# @package _global_*
model:
  num_layers: 5*
```

> ****注*** *:这里我们用* `*# @package _global_*` *代替* `*# @package _group_*` *。**

*我们可以如下测试代码，这里我们简单地打印出配置文件返回的特性数量*

```
***@**hydra.main(config_path**=**"config", config_name**=**"config")
**def** **main_func**(cfg: DictConfig):
    **print**(f"Num features = {cfg.model.num_layers}")> python main.py dataset=imagenet
Num features = 50> python main.py dataset=cifar10
Num features = 34*
```

*我们必须指定`optional: true`，因为如果没有它，我们将需要指定`dataset`和`model`的所有组合(如果用户输入值`dataset`和`model`，这样我们就没有该选项的配置文件，那么 Hydra 将抛出一个缺少配置文件的错误)。*

*本主题的[文档](https://hydra.cc/docs/patterns/specializing_config)。*

*流程的其余部分是相同的，为优化器、学习率调度程序、回调、评估指标、损失、培训脚本创建单独的配置组。就创建配置文件并在项目中使用它们而言，这是您需要知道的全部内容。*

# *随机的事情*

## *显示配置文件*

*在不运行函数的情况下，打印传递给函数的配置文件。用法`--cfg [OPTION]`有效`OPTION`有*

*   *`job`:你的配置文件*
*   *`hydra`:九头蛇的配置*
*   *`all` : `job` + `hydra`*

*当您想要检查传递给函数的内容时，这对于快速调试非常有用。例子，*

```
*> python main.py --cfg job
# @package _global_
num_samples: 2
dataset:
  name: dataset1
  feature_size: 5*
```

## *多次运行*

*这是九头蛇的一个非常有用的特性。查看[文档](https://hydra.cc/docs/tutorials/basic/running_your_app/multi-run)了解更多详情。主要思想是你可以使用一个命令运行不同学习率值和不同权重衰减值的模型。下面显示了一个示例*

```
*❯ python main.py lr**=**1e-3,1e-2 wd**=**1e-4,1e-2 **-**m
[2021**-**03**-**15 04:18:57,882][HYDRA] Launching 4 jobs locally
[2021**-**03**-**15 04:18:57,882][HYDRA]        *#0 : lr=0.001 wd=0.0001* [2021**-**03**-**15 04:18:58,016][HYDRA]        *#1 : lr=0.001 wd=0.01* [2021**-**03**-**15 04:18:58,149][HYDRA]        *#2 : lr=0.01 wd=0.0001* [2021**-**03**-**15 04:18:58,275][HYDRA]        *#3 : lr=0.01 wd=0.01**
```

*Hydra 将使用`lr`和`wd`的所有组合运行您的脚本。输出将存储在一个名为`multirun`(而不是`outputs`)的新文件夹中。该文件夹也遵循将内容存储在日期和时间子文件夹中的相同语法。运行上述命令后的目录结构如下所示*

```
*multirun
└── 2021-03-15
    └── 04-21-32
        ├── 0
        │   ├── .hydra
        │   └── main.log
        ├── 1
        │   ├── .hydra
        │   └── main.log
        ├── 2
        │   ├── .hydra
        │   └── main.log
        ├── 3
        │   ├── .hydra
        │   └── main.log
        └── multirun.yaml*
```

*它和`outputs`一样，除了这里为运行创建了四个文件夹，而不是一个。您可以查看[文档](https://hydra.cc/docs/tutorials/basic/running_your_app/multi-run)，了解指定运行脚本的变量值的不同方式(这些被称为*扫描*)。*

*此外，这将在本地按顺序运行您的脚本。如果您想在多个节点上并行运行您的脚本或者在 AWS 上运行它，您可以查看以下插件的文档*

*   *[作业库](https://hydra.cc/docs/plugins/joblib_launcher) —使用[作业库。平行](https://joblib.readthedocs.io/en/latest/parallel.html)*
*   *[Ray](https://hydra.cc/docs/plugins/ray_launcher) —在 AWS 集群或本地集群上运行作业*
*   *[RQ](https://hydra.cc/docs/plugins/rq_launcher)*
*   *[提交](https://hydra.cc/docs/plugins/submitit_launcher)*

## *给终端添加颜色*

*你可以通过安装这个插件来增加 Hydra 终端输出的颜色*

```
*pip install hydra_colorlog **--**upgrade*
```

*然后在配置文件中更改这些默认值*

```
*defaults:
  - hydra/job_logging: colorlog
  - hydra/hydra_logging: colorlog*
```

## *指定帮助消息*

*您可以查看您的一次运行的日志(在`.hydra/hydra.yaml`下，然后转到`help.template`)来查看 hydra 打印的默认帮助信息。但是您可以在主配置文件中修改该消息，如下所示*

```
**### config.yaml*hydra:
  help:
    template:
      'This is the help message'> python main.py --help
This is the help message*
```

## *输出目录名*

*如果您想要更具体的东西，而不是 hydra 用来存储所有运行输出的**日期/时间**命名方案，您可以在命令行指定文件夹名称*

```
*python main.py hydra.run.dir=outputs/my_runORpython main.py lr=1e-2,1e-3 hydra.sweep.dir=multirun/my_run -m*
```

*今天就到这里。希望这有助于您在项目中使用 Hydra。*

*[twitter](https://twitter.com/Kkushaj) ， [linkedin](https://www.linkedin.com/in/kushaj/) ， [github](https://github.com/KushajveerSingh)*

**原载于 2021 年 3 月 16 日*[*https://kushajveersingh . github . io*](https://kushajveersingh.github.io/blog/general/2021/03/16/post-0014.html)*。**