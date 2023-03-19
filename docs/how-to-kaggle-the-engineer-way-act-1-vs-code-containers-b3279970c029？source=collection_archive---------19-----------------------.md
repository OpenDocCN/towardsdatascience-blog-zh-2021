# 如何走工程师之路——第一幕:VS 代码容器

> 原文：<https://towardsdatascience.com/how-to-kaggle-the-engineer-way-act-1-vs-code-containers-b3279970c029?source=collection_archive---------19----------------------->

## *或者，如何设计出你的 Kaggle 开发环境:一部两幕剧*

![](img/f2f8d1d6846218a1d1c02f7ce9322942.png)

安托万·佩蒂特维尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我的整个 Kaggle 工作，包括本文中讨论的内容，都发表在这里。

## 简介和动机

我是一名软件工程师，不久前决定开始新的挑战，成为一名数据科学家。作为其中的一部分，我开始参加 Kaggle 上的比赛(到目前为止我只参加了两次)。然而，在开始时，我遇到了一个相当麻烦的上升阶段。前两次比赛的大部分时间，我都在搭建我的开发环境。我只是无法忍受所有那些手动和重复的任务:下载数据集，硬编码路径，通过网站手动提交，最重要的是迷失在模型的所有输入和输出中(即跟踪版本)。

所以，我已经决定，如果我花一些时间在一开始就把事情做好，也就是说，采取软件工程的方式，我会为自己节省很多时间。

这是一个两幕的故事:

*   Act 1 是我用本地开发容器设置的 VS 代码，以模仿 Kaggle 内核。
*   第二幕是我设置 Google Colab 独立运行，但与 Kaggle 合作。

在第一幕结束时，你将能够在 Kaggle 上进行本地开发和训练，而无需任何手动步骤。第二幕将在下一篇文章中介绍。

## 第 1 幕—VS 代码中的本地集装箱化环境

在我参加的第一场比赛(一个图像分类问题)中，我只是通过在线使用 Kaggle 内核开始，因为我的笔记本电脑没有 GPU，训练模型需要花费我很长时间。

然而，很快内核就不够了:

*   首先，我想跟踪 GitHub 上的所有东西，所以我最终会在本地下载所有东西，提交等等。，每次我做出改变。在工具之间切换是不可接受的。
*   其次，我希望能够调试我的代码。我相信调试是一个强大的工具，这只是一个事实——对于那些喜欢使用 Vim 输入代码的人来说是一个“眼中钉”。

## VS 代码远程概述:容器

如果你不想一遍又一遍地做设置步骤，容器是一个很好的工具。它们允许您定义一次配置步骤，并且可以在任何时候使用该环境。或者，更好的是，使用其他人预先定义的，例如 Kaggle。

你可以在这里找到关于如何使用 VS Code: Remote extension 在容器[中工作的大量信息。](https://code.visualstudio.com/docs/remote/containers)

## 获取 Kaggle 容器图像

他们网站上的 Kaggle 内核正是以这种方式工作的——他们有一个预定义的容器映像，可以为每个用户的笔记本加载，从而隔离他们的环境。

对我们来说幸运的是，他们的照片被公开了。

*   [这里](https://github.com/Kaggle/docker-python)他们有关于他们的 Python 图像的信息。
*   你可以偷看他们的 [Dockerfile](https://github.com/Kaggle/docker-python/blob/master/Dockerfile) 里面，看看他们在里面安装了什么。
*   这些图片被发布在谷歌容器注册表的 [CPU 专用](gcr.io/kaggle-images/python)或 [GPU](gcr.io/kaggle-gpu-images/python) 上

## 配置 VS 代码

*   首先你需要安装 [VS 代码远程开发扩展包](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)。
*   其次，创建`.devcontainer/devcontainer.json`文件来存储您的环境定义([更多信息](https://code.visualstudio.com/docs/remote/containers#_create-a-devcontainerjson-file)):

```
{
    "name": "Kaggle Dev CPU",
    "image": "gcr.io/kaggle-images/python", // Remember to pull latest before rebuilding.
    "extensions": [
        "ms-python.python",
    ],
    "settings": {
        "terminal.integrated.shell.linux": "/bin/bash",
        "python.pythonPath": "/opt/conda/bin/python"
    },
    "devPort": 8888,
    "shutdownAction": "none"
}
```

*   运行 VS 代码命令`Remote-Containers: Rebuild and Reopen in Container`。
*   你的 VS 代码窗口标题现在应该显示`... [Dev Container: Kaggle Dev CPU] - ...`。

## 模拟 Kaggle 环境

即使现在我已经用 VS 代码完成了开发，由于 GPU 的原因，我仍然希望在 Kaggle 上进行培训。而且，我再次希望必须做最少的手动任务来从本地运行切换到 Kaggle。所以，我决定*在本地模拟* Kaggle 环境，这样脚本/笔记本甚至不会知道其中的区别。

我已经运行了同一个容器，所以我只需要模拟文件夹位置:

*   `/kaggle/input`:数据集映射到的文件夹。
*   `/kaggle/working`:存储输出的文件夹(以及笔记本/脚本的当前工作目录)。

要实现这一点:

*   在方便的地方创建`input`和`working`文件夹。我在我的工作区中选择了`data/`文件夹。
*   在`devcontainer.json`中为容器创建映射:

```
"mounts": [
    "type=bind,source=${localWorkspaceFolder}/data/input,target=/kaggle/input",
    "type=bind,source=${localWorkspaceFolder}/data/output,target=/kaggle/output",
],
```

上面的配置从路径`/kaggle/input`下的容器内我的本地工作区文件夹中映射了`data/input`文件夹——就像它在 Kaggle 上一样。它还映射了一个额外的文件夹`data/output <-> /kaggle/output`，这样笔记本就可以在容器之外保存数据。

*   创建一个脚本`.devcontainer/setup.sh`，它将在容器创建后由 VS 代码执行:

```
#!/bin/bash
mkdir /kaggle/working
```

不要忘记使它可执行:`chmod +x .devcontainer/setup.sh`。

并告诉 VS 代码运行它(在`devcontainer.json`):

```
"postCreateCommand": ".devcontainer/setup.sh",
```

## 从 Kaggle 获取数据集

起初，我会简单地手动下载数据集(到一个文件夹`data/input`)，并给它们起一个与 Kaggle kernel 相同的名字。然而，当我开始使用不同的附加数据集或附加库等时。，我开始寻找 Kaggle 是否有一些 API 或工具来自动化这个过程。幸运的是[它做到了](https://github.com/Kaggle/kaggle-api)。

所以，我决定设置 VS 代码任务(你可以在这里了解更多)来运行下载数据集、文件等的命令。：

*   按照这里的描述[获取您的 Kaggle API 凭证。](https://github.com/Kaggle/kaggle-api#api-credentials)
*   我把`kaggle.json`放在我的工作区文件夹中(**确保把它添加到** `**.gitignore**` **！**):
*   然后，我们需要确保它在容器的主目录中，以便能够在其中运行(不必切换到本地模式)。

添加一个名为`.devcontainer/setup-mounted.sh`的脚本(这个脚本将在代码挂载后运行):

```
#!/bin/bash
# Set up a link to the API key to root's home.
mkdir /root/.kaggle
ln -s /workspaces/kaggle/kaggle.json /root/.kaggle/kaggle.json
chmod 600 /root/.kaggle/kaggle.json
```

并告诉 VS 代码在附加到容器后运行这个脚本:

```
"postAttachCommand": ".devcontainer/setup-mounted.sh",
```

*   Kaggle API 设置完成后，添加 VS 代码任务(在`.vscode/tasks.json`):

```
"tasks": [
  {
      "label": "kaggle dataset download",
      "type": "shell",
      "command": "kaggle datasets download ${input:userName}/${input:datasetName} -p ${input:datasetName} --unzip --force",
      "options": {
          "cwd": "/kaggle/input"
      },
      "problemMatcher": []
  }
]
```

上面的任务将把格式为`<username>/<dataset name>`的数据集下载到目录`/kaggle/input`中，就像在 Kaggle 内核中一样。

## 运行特定于环境的逻辑(如有必要)

如果您的机器上只有 CPU，并且只能运行 2 个时期的训练，这可能是有用的，但是当在 Kaggle 上运行时，您希望运行完整的训练(例如，30 个时期)。

为此，我使用了一个只能在 VS 代码中设置的环境变量。

*   告诉 VS 代码在`devcontainer.json`中定义这个环境变量:

```
"containerEnv": {
    "KAGGLE_MODE": "DEV"
},
```

*   在代码中使用它来检查您是否正在本地运行:

```
import os
DEVMODE = os.getenv("KAGGLE_MODE") == "DEV"
print(f"DEV MODE: {DEVMODE}")
EPOCHS = 2 if DEVMODE else 30
```

这一步是我最不喜欢的，因为我必须在每台笔记本上重复这一步…然而，这只是在创建笔记本时进行的，所以仍然比每次希望在不同环境之间切换时都必须手动完成所有事情要好得多。

## 额外的

您可以为您的工作启用许多有用的扩展。在`devcontainer.json`中:

```
"extensions": [
    "ms-python.python",
    "ms-python.vscode-pylance",
    // Editing/dev process
    "streetsidesoftware.code-spell-checker",
    "wayou.vscode-todo-highlight",
    "janisdd.vscode-edit-csv",
    "davidanson.vscode-markdownlint",
    // VCS helpers
    "donjayamanne.githistory",
    "eamodio.gitlens"
],
```

并且不要忘记一堆对你的 VS 代码环境有用的[设置](https://github.com/Witalia008/kaggle-public/blob/master/.vscode/settings.json)(比如格式化和林挺你的代码)。

## 整个设置

您可以参考这些文件:

*   [。devcontainer](https://github.com/Witalia008/kaggle-public/tree/master/.devcontainer) 文件夹。
*   [。vscode](https://github.com/Witalia008/kaggle-public/tree/master/.vscode) 文件夹。
*   [cassava_inference.py](https://github.com/Witalia008/kaggle-public/blob/master/cassava-leaf-disease-classification/cassava-inference.py) —示例 python 脚本。

## 进一步的改进

一些我没有做过的事情，但是在 Kaggle API 中是可能的:

*   它允许上传笔记本并运行它们([阅读更多](https://github.com/Kaggle/kaggle-api#kernels))，所以你不必手动进入网站。
*   它还允许提交竞争(将在法案 2 中涉及)。
*   它还有其他一些可能对你有用的功能，比如列表排行榜等。

## 摘要

按照本文中描述的步骤，可以在本地机器上建立与 Kaggle 上非常相似的开发环境，并增加了版本控制系统和调试的额外功能(以及使用 IDE 的其他优势)。

这可以通过几个步骤来实现:

*   配置 VS 代码在 Kaggle 容器中开发。
*   设置类似 Kaggle 的目录结构，映射到本地机器的存储。
*   使用 Kaggle API 下载数据集和更多。
*   使用环境变量在代码中包含特定于环境的逻辑。

我希望这个设置可以帮助你在参加 Kaggle 比赛时减轻工作负担。