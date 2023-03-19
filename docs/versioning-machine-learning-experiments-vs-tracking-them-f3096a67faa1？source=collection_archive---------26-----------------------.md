# 版本化机器学习实验与跟踪它们

> 原文：<https://towardsdatascience.com/versioning-machine-learning-experiments-vs-tracking-them-f3096a67faa1?source=collection_archive---------26----------------------->

## 了解如何利用 DVC 提高 ML 重现性

![](img/bdca11b3590ae1bffed2cad2a9c96668.png)

照片由[王思然·哈德森](https://unsplash.com/@hudsoncrafted?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在进行机器学习项目时，通常会运行大量实验来寻找算法、参数和数据预处理步骤的组合，从而为手头的任务产生最佳模型。为了跟踪这些实验数据，由于没有更好的选择，科学家们习惯于将它们记录到 Excel 表格中。然而，由于大部分是手动的，这种方法有其缺点。举几个例子，它容易出错，不方便，速度慢，而且完全脱离实际实验。

幸运的是，在过去的几年中，实验跟踪已经取得了很大的进步，我们已经看到市场上出现了许多工具，可以改善实验跟踪的方式，例如 Weights & Biases、MLflow、Neptune。通常这样的工具提供了一个 API，您可以从代码中调用它来记录实验信息。然后，它被存储在一个数据库中，您可以使用一个仪表板来直观地比较实验。有了它，一旦你改变了你的代码，你再也不用担心忘记写下结果——这是自动为你做的。仪表板有助于可视化和共享。

在跟踪已经完成的工作方面，这是一个很大的改进，但是……在仪表板中发现一个已经产生最佳指标的实验并不能自动转化为准备好部署的模型。很可能你需要先重现最好的实验。然而，您直接观察到的跟踪仪表板和表格与实验本身的联系很弱。因此，您可能仍然需要半手工地追溯您的步骤，以将准确的代码、数据和管道步骤缝合在一起，从而重现该实验。这能自动化吗？

在这篇博文中，我想谈谈版本化实验，而不是跟踪它们，以及这如何在实验跟踪的好处之上带来更容易的可重复性。

为了实现这一点，我将使用 [DVC](http://dvc.org) ，这是一个开源工具，在数据版本化的环境中最为人所知(毕竟它就在名字中)。然而，这个工具实际上可以做得更多。例如，您可以使用 DVC 来定义 ML 管道，运行多个实验，并比较指标。您还可以对参与实验的所有活动部件进行版本控制。

# 实验版本

为了开始对 DVC 进行版本控制实验，您需要从任何 Git repo 中初始化它，如下所示。注意，DVC 希望你的项目有一个合理的结构，你可能需要重新组织你的文件夹。

```
$ dvc exp init -i
This command will guide you to set up a default stage in dvc.yaml.
See [https://dvc.org/doc/user-guide/project-structure/pipelines-files](https://dvc.org/doc/user-guide/project-structure/pipelines-files).DVC assumes the following workspace structure:
├── data
├── metrics.json
├── models
├── params.yaml
├── plots
└── srcCommand to execute: python src/train.py
Path to a code file/directory [src, n to omit]: src/train.py
Path to a data file/directory [data, n to omit]: data/images/
Path to a model file/directory [models, n to omit]:
Path to a parameters file [params.yaml, n to omit]:
Path to a metrics file [metrics.json, n to omit]:
Path to a plots file/directory [plots, n to omit]: logs.csv
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
default:
  cmd: python src/train.py
  deps:
  - data/images/
  - src/train.py
  params:
  - model
  - train
  outs:
  - models
  metrics:
  - metrics.json:
      cache: false
  plots:
  - logs.csv:
      cache: false
Do you want to add the above contents to dvc.yaml? [y/n]: yCreated default stage in dvc.yaml. To run, use "dvc exp run".
See [https://dvc.org/doc/user-guide/experiment-management/running-experiments](https://dvc.org/doc/user-guide/experiment-management/running-experiments).
```

您可能还注意到，DVC 假设您将参数和指标存储在文件中，而不是用 API 记录它们。这意味着您需要修改代码，从 YAML 文件中读取参数，并将指标写入 JSON 文件。

最后，在初始化时，DVC 会自动创建一个基本管道，并将其存储在 dvc.yaml 文件中。有了它，您的培训代码、管道、参数和指标现在就存在于可版本化的文件中。

# 实验即代码方法的好处

## 干净的代码

当以这种方式设置时，您的代码不再依赖于实验跟踪 API。您不必在代码中插入跟踪 API 调用来将实验信息保存在中央数据库中，而是将其保存在可读文件中。这些在您的 repo 中总是可用的，您的代码保持干净，并且您有更少的依赖性。即使没有 DVC，您也可以使用 Git 读取、保存和版本化您的实验参数和度量，尽管使用普通的 Git 并不是比较 ML 实验的最方便的方式。

```
$ git diff HEAD~1 -- params.yaml
diff --git a/params.yaml b/params.yaml
index baad571a2..57d098495 100644
--- a/params.yaml
+++ b/params.yaml
@@ -1,5 +1,5 @@
 train:
   epochs: 10
-model:
-  conv_units: 16
+model:
+  conv_units: 128
```

## 再现性

实验跟踪数据库并不能捕获重现实验所需的所有信息。经常缺失的一个重要部分是端到端运行实验的管道。让我们看一下已经生成的管道文件“dvc.yaml”。

```
$ cat dvc.yaml
stages:
  default:
    cmd: python src/train.py
    deps:
    - data/images
    - src/train.py
    params:
    - model
    - train
    outs:
    - models
    metrics:
    - metrics.json:
        cache: false
    plots:
    - logs.csv:
        cache: false
```

这个管道捕获运行实验的命令、参数和其他依赖项、度量、绘图和其他输出。它只有一个“默认”阶段，但是您可以根据需要添加多个阶段。当将实验的所有方面都视为代码时，包括管道，任何人都可以更容易地复制实验。

## 减少噪音

在仪表板上，你可以看到你所有的实验，我是说所有的实验。在某一点上，你将会有如此多的实验，你将不得不对它们进行分类、标记和过滤以跟上进度。有了实验版本，你在分享什么和如何组织事情上有了更多的灵活性。

例如，您可以在一个新的 Git 分支中尝试一个实验。如果出了问题或者结果不令人振奋，可以选择不推分支。这样您可以减少一些不必要的混乱，否则您会在实验跟踪仪表板中遇到。

同时，如果一个特定的实验看起来很有希望，你可以把它和你的代码一起推送到你的 repo，这样结果就和代码和管道保持同步。结果与相同的人共享，并且已经使用您现有的分支机构名称进行了组织。你可以继续在那个分支上迭代，如果一个实验偏离太多，开始一个新的，或者合并到你的主分支，使它成为你的主要模型。

# 为什么用 DVC？

即使没有 DVC，您也可以更改代码，从文件中读取参数，向其他文件写入指标，并使用 Git 跟踪更改。然而，DVC 在 Git 上添加了一些特定于 ML 的功能，可以简化实验的比较和再现。

## 大型数据版本控制

在 Git 中不容易跟踪大型数据和模型，但是通过 DVC，您可以使用自己的存储来跟踪它们，但是它们是 Git 兼容的。初始化时，DVC 开始跟踪“模型”文件夹，让 Git 忽略它，但存储和版本化它，这样你就可以在任何地方备份版本，并在你的实验代码旁边检查它们。

## 单命令再现性

整理整个实验管道是实现可重复性的第一步，但是仍然需要用户来执行管道。有了 DVC，你只需一个命令就能重现整个实验。不仅如此，它还会检查缓存的输入和输出，并跳过重新计算之前生成的数据，这有时会节省大量时间。

```
$ dvc exp run
'data/images.dvc' didn't change, skipping
Stage 'default' didn't change, skippingReproduced experiment(s): exp-44136
Experiment results have been applied to your workspace.To promote an experiment to a Git branch run:dvc exp branch <exp> <branch>
```

## 更好的分支机构

虽然 Git 分支是一种灵活的组织和管理实验的方式，但是通常有太多的实验不适合任何 Git 分支工作流。DVC 跟踪实验，所以你不需要为每个实验创建提交或分支:

```
$ dvc exp show ┏━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━━┓ ┃**Experiment**               ┃ **Created**      ┃    **loss** ┃    **acc** ┃ ┡━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━━┩ │**workspace**                │ **-**            │ **0.25183** │ **0.9137** │ │**mybranch**                 │ **Oct 23, 2021** │       **-** │      **-** │ │├──9a4ff1c **[exp-333c9]**   │ 10:40 AM     │ 0.25183 │ 0.9137 │ │├──138e6ea **[exp-55e90]**   │ 10:28 AM     │ 0.25784 │ 0.9084 │ │├──51b0324 **[exp-2b728]** │ 10:17 AM     │ 0.25829 │ 0.9058 │ └─────────────────────────┴──────────────┴─────────┴────────┘
```

一旦您决定了这些实验中哪些值得与团队分享，就可以将它们转换成 Git 分支:

```
$ dvc exp branch exp-333c9 conv-units-64
Git branch 'conv-units-64' has been created from experiment 'exp-333c9'.
To switch to the new branch run:git checkout conv-units-64
```

这样你就可以避免在回购中产生混乱的分支，并且可以专注于比较那些有前途的实验。

# 结论

总的来说，实验版本化允许您以这样一种方式来编码您的实验，即您的实验日志总是连接到进入实验的确切数据、代码和管道。你可以控制哪些实验最终与你的同事分享以供比较，这可以防止混乱。

最后，复制一个版本化的实验变得像运行一个命令一样简单，如果一些管道步骤缓存了仍然相关的输出，它甚至可以比最初花费更少的时间。

谢谢你陪我到帖子结束！要了解更多关于管理 DVC 实验的信息，[查看文档](https://dvc.org/doc/user-guide/experiment-management)