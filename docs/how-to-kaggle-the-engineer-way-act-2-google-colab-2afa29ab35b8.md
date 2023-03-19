# 如何走工程师之路？第二幕:谷歌实验室

> 原文：<https://towardsdatascience.com/how-to-kaggle-the-engineer-way-act-2-google-colab-2afa29ab35b8?source=collection_archive---------23----------------------->

## 或者，如何设计出你的 Kaggle 开发环境:两幕剧。

![](img/201eb9b33a7d5a48f8d1c0aa919b40b2.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [krakenimages](https://unsplash.com/@krakenimages?utm_source=medium&utm_medium=referral) 拍摄的照片

这是该剧的第二幕，你可以在这里阅读第一幕。

我的整个 Kaggle 工作，包括本文中讨论的内容，都发表在这里。

## 序言/介绍

在故事的第一部分，我描述了如何为 Kaggle 设置带有容器的 VS 代码。对我来说，使用系统的方法是最重要的，不管是工程工作还是数据科学。因为这个信念，我在 Kaggle 比赛中的提升在很大程度上是为了将来节省时间。

在本文中，我将描述在我参加的第二次比赛中，我是如何改进我最初使用 VS 代码进行开发的方法的。如题所示，我转到了 Google Colab，但尽管如此，我仍然希望有适当的设置。

## 以前方法的缺点

在第一场比赛中，在用 Kaggle 容器设置了 VS 代码之后，我在 Kaggle 上进行本地开发和培训。我尽最大努力减少在不同环境间切换的重复性工作。然而，还有一些其他问题:

*   我仍然不得不在两种环境之间切换。
*   Kaggle 不允许使用数据集的特定版本(例如，当训练 5 倍时，我必须下载每一个并上传所有数据作为集合的数据集)。
*   包含依赖项是一件痛苦的事情——我不得不再次上传它们，作为一个数据集，然后映射它们的位置。

在本文中，我将描述我如何使用 Google Colab 来克服所有这些麻烦，并获得一些额外的好处，如更好的 GPU 和更好的运行时。

## 第 2 幕——谷歌与 GitHub 的 Kaggle 合作实验室

Google Colab 是一个为您的数据科学工作获取免费 GPU 的好方法。只需支付少量额外费用，你就可以获得配备 V100/P100 GPU 的 Colab pro，这比你在 Kaggle 上获得的 T4 GPU 快几倍。它还有其他不错的功能，比如映射 Google Drive 文件、使用 GitHub 等等——如下所述。

我选择这将是对 Kaggle 在训练速度和易用性上的改进，并且我不需要在本地开发。随着更多的研究，看起来我甚至不需要离开 Colab 环境，在里面做任何事情(有点像不离开我的公寓)。

## 在 Google Drive 中为 Colab 设置 GitHub

我做的第一件事是确保我仍然可以使用 Git，并且不必在环境之间切换。

糟糕的方法:你可以直接从你的私人或公共回购中打开文件，编辑它并提交回来，正如这里的所描述的。但是，有几个问题:

*   提交仅针对一个笔记本，即 Colab 仅在一个笔记本的上下文中工作。
*   由于上述原因，您不能拥有依赖关系。

好消息是，Colab 允许你[安装你的 Google Drive](https://www.marktechpost.com/2019/06/07/how-to-connect-google-colab-with-google-drive/) 。一旦它被映射，它就是运行 Colab 的 VM 实例上的一个常规文件夹。

那么，为什么不克隆你的 GitHub repo 呢？

*   创建 GitHub 个人访问令牌来代替您的密码— [link](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) 。
*   准备一些临时的笔记本，姑且称之为`terminal.ipynb`，存储在你的 Google Drive 中，并与你的驱动器进行映射:

```
from google.colab import drive
drive.mount("/content/drive")
```

*   在 Colab Pro 中，您可以打开一个终端实例— [link](https://stackoverflow.com/questions/59318692/how-can-i-run-shell-terminal-in-google-colab) 。
*   在任何 Colab 中，您都可以从笔记本单元格中运行终端命令:

```
%%bash
cd /content/drive/MyDrive/
git clone https://<username>:<PAT>@github.com/<username>/<repo name>.git
cd <repo name>
```

至此，您已经将所有代码 git 克隆到您的 Google Drive 中，并且您可以在其中打开任何笔记本。我还在笔记本[中设置了一些代码来使用 Git，但是我最终只使用了终端命令。](https://github.com/Witalia008/kaggle-public/blob/master/setup-colab-for-kaggle.ipynb)

唯一的缺点是，你必须在每个笔记本的开头映射 Google Drive，但我认为这相对来说没什么问题。

```
try:
    from google.colab import drive
    drive.mount("/content/drive")
    %cd /content/drive/MyDrive/Colab\ Notebooks/kaggle
except:
    print("Not in Colab")
```

我为自己创建了一个[模板笔记本](https://github.com/Witalia008/kaggle-public/blob/master/template-colab-kaggle-nb.ipynb)，我复制它作为任何其他笔记本的基准。

注意:一个额外的很酷的技巧，不需要每次都给你的驱动器 Colab 授权，你可以像这里描述的[一样为每个笔记本设置自动挂载。](https://stackoverflow.com/questions/52808143/colab-automatic-authentication-of-connection-to-google-drive-persistent-per-n)

## 为 Kaggle 设置

下一步是设置 Kaggle ( `/kaggle/input`、`/kaggle/working`)和 Kaggle API secrets 等所需的所有目录。这个设置必须在每个笔记本的开头完成，所以它必须作为脚本导入和函数调用。

您不能在 Colab 中真正编辑 Python 脚本，但是您可以让您的笔记本写入文件:

```
%%writefile setup_colab.py
def setup_colab_for_kaggle():
    ...
```

在该脚本中，让我们执行必要的设置:

*   映射和/或创建目录:

```
kaggle_dir = Path("/kaggle")
drive_content_dir = Path("/content/drive/MyDrive/kaggle")
(kaggle_dir / "working").mkdir()
target_content_dirs = ["input", "output"] + ([] if local_working else ["working"])
for content_dir in target_content_dirs:
    (kaggle_dir / content_dir).symlink_to(drive_content_dir / content_dir)
```

*   设置 Kaggle API 令牌。将你的`kaggle.json`文件上传到你的硬盘上(我把它放在了 repo 文件夹中，虽然你也这样做，**确保把它添加到** `**.gitignore**`)。

```
drive_sources_dir = Path("/content/drive/MyDrive/Colab Notebooks/kaggle")
kaggle_config = Path.home() / ".kaggle"
(kaggle_config / "kaggle.json").symlink_to(drive_sources_dir / "kaggle.json")
```

*   您可以使用相同的方法来设置 Weights & Biases API 键或任何其他键。
*   在该文件中有任何其他设置代码。

有相当多的附加逻辑，所以完整的代码在[笔记本](https://github.com/Witalia008/kaggle-public/blob/master/setup-colab-for-kaggle.ipynb)和[输出脚本](https://github.com/Witalia008/kaggle-public/blob/master/setup_colab.py)中。

现在，每台笔记本只需导入代码并运行设置功能:

```
from setup_colab import setup_colab_for_kaggle
setup_colab_for_kaggle(check_env=False, local_working=True)
```

使用我的脚本，输出将告诉我们最终的设置:

```
Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
/content/drive/MyDrive/Colab Notebooks/kaggle
Content of Drive Kaggle data dir (/content/drive/MyDrive/kaggle): ['/content/drive/MyDrive/kaggle/input', '/content/drive/MyDrive/kaggle/working', '/content/drive/MyDrive/kaggle/output']
Content of Kaggle data dir (/kaggle): ['/kaggle/working', '/kaggle/output', '/kaggle/input']
Content of Kaggle data subdir (/kaggle/input): ['/kaggle/input/vinbigdata']
Content of Kaggle data subdir (/kaggle/output): ['/kaggle/output/vbdyolo-out']
Content of Kaggle data subdir (/kaggle/working): []
Content of Kaggle config dir (/root/.kaggle): ['/root/.kaggle/kaggle.json']
Loaded environment variables from .env file: ['WANDB_API_KEY'].
```

酷，在这一点上，所有的设置都是模仿 Kaggle 内核的设置，以及前一篇文章中描述的本地环境，所以笔记本在两个环境之间切换时不会知道区别(如果你想知道的话)。但是，让我们将它设置得更好，这样我们就根本不用切换了。

## 从 Kaggle 获取数据

首先，我会将数据下载到本地，然后上传到我的驱动器，最后它会被映射到正确的位置，如上所示。然而，这并不太实际，最重要的是——*Colab 从 Google Drive* 中访问文件非常慢。事实证明，每次将数据集下载到 Colab 虚拟机的本地存储中要快得多。在你的笔记本上有:

```
!kaggle competitions download vinbigdata-chest-xray-abnormalities-detection -f train.csv -p {INPUT_FOLDER_DATA} --unzip
!kaggle datasets download xhlulu/vinbigdata-chest-xray-resized-png-1024x1024 -p {INPUT_FOLDER_PNG} --unzip
```

通过 Google Drive 训练有效网络 B3 的一个时期需要大约 1 小时，而下载 8GB 需要大约 8 分钟，训练一个时期需要大约 15 分钟。速度提高了 4 倍，不错！

## 版本化数据集和模型

这一节将阐明为什么我们需要设置 Kaggle API 令牌。

由于我对 Kaggle 竞赛的方法是系统的和有条理的，我开始思考如何组织我使用的数据和产生的模型。有点像模型和数据的 Git。事实上，我发现有[数据版本控制](https://dvc.org/)和其他一些工具。然而，对于 Kaggle 比赛，更简单的方法就可以了。一种选择是只存储在文件夹中，但这不是很整洁，数据可能会丢失，而且很难给每个版本附加注释。所以我选择把所有东西都作为 Kaggle 数据集偷走。

这可以像在每个笔记本末尾上传文件一样简单:

*   将您想要上传的文件放在一个文件夹中(比如说，`yolo_pred/`文件夹包含 YOLO 预测和`yolo.pt`文件)。
*   创建`dataset-metadata.json`文件——由 Kaggle API 解析和理解的东西:

```
with open(Path(folder_path) / "dataset-metadata.json", "w") as f:
    json.dump({
        "title": dataset_name,
        "id": f"{user_name}/{dataset_name}",
        "licenses": [{ "name": "CC0-1.0" }]
    }, f, indent=4)
```

*   使用 Kaggle API 命令在笔记本执行(如模型训练)结束时上传文件:如果是第一次:

```
!kaggle datasets create -p {OUTPUT_FOLDER_CUR} -r zip
```

否则:

```
!kaggle datasets version -m "{version_message}" -p {OUTPUT_FOLDER_CUR} -r zip
```

*   现在，在您的后处理中，您可以下载特定版本(特定的训练模式及其输出)用于提交:

```
!kaggle datasets download "username/dataset-name" -v {yolo_version} -p {version_data["path"]} --unzip --force
```

剧透:虽然，这在官方 Kaggle API 中不起作用——见下一段。

## 修复 Kaggle API 以满足我们的需求

作为一个偏离主题的话题，我需要在 Kaggle API 中添加/修复一些特性。推动这一点是因为在某一点上，我不能重现良好的结果，我获得了一些早期版本。那时，我已经对所有的东西进行了版本控制，并且记录了所有的设置，所以这几乎是不可能的……但是它正在发生。然后我注意到我选择的任何版本都得到了相同的结果。结果是，嗯，Kaggle API 默默地忽略了我的版本请求，总是下载最新的版本。

因此，他们的 GitHub repo 提到他们欢迎改变，因此我决定自己解决问题(我非常需要竞争的功能)。

*   首先，我需要确保 Kaggle 会下载一个特定的版本。我的 PR [在这里](https://github.com/Kaggle/kaggle-api/pull/335)。
*   第二，我已经准备好了，所以我做了一些修正，使得比赛中的一个文件可以正确下载(整个数据集大约 200GB，我只下载了带有火车标签的`.csv`文件，并从另一个数据集获得了大约 8GB 的相同图像)。我的 PR [这里](https://github.com/Kaggle/kaggle-api/pull/336)。

不过，很明显，将变更加入 Kaggle API 的过程很慢(他们有一个私有的回购协议，一些开发人员必须去合并变更……)。所以，这两个改动都暂时在我这里的[分叉](https://github.com/Witalia008/kaggle-api/tree/witalia-main)。

您可以从您的 Colab 笔记本上安装带有我的补丁的 Kaggle API，如下所示:

```
!pip install -U git+https://github.com/Witalia008/kaggle-api.git@witalia-main
```

## 臣服于 Kaggle

对于我参加的特定比赛，提交的只是一个`.csv`文件，所以我只需在我的后处理笔记本的末尾运行以下命令:

```
!kaggle competitions submit \
    vinbigdata-chest-xray-abnormalities-detection \
    -f {WORK_FOLDER}/submission.csv \
    -m "{submission_message}"
```

然后检查提交的结果:

```
!kaggle competitions submissions vinbigdata-chest-xray-abnormalities-detection
```

## 整个设置

您可以参考具有完整实现的文件:

*   [setup-colab-for-ka ggle . ipynb](https://github.com/Witalia008/kaggle-public/blob/master/setup-colab-for-kaggle.ipynb)—包含使用 Git 的代码以及 setup-colab 逻辑的笔记本。
*   [setup_colab.py](https://github.com/Witalia008/kaggle-public/blob/master/setup_colab.py) —设置脚本(由上述笔记本产生)，包含映射目录、配置密钥等功能。
*   [template-colab-ka ggle-nb . ipynb](https://github.com/Witalia008/kaggle-public/blob/master/template-colab-kaggle-nb.ipynb)—需要设置的模板笔记本，我将它用作其他笔记本的基础。
*   [vbd-yolov5.ipynb](https://github.com/Witalia008/kaggle-public/blob/master/vinbigdata-chest-xray-abnormalities-detection/vbd-yolov5.ipynb) —下载数据集、训练模型并将输出存储为数据集的示例笔记本。
*   [vbd-postprocess-yolo . ipynb](https://github.com/Witalia008/kaggle-public/blob/master/vinbigdata-chest-xray-abnormalities-detection/vbd-postprocess-yolo.ipynb)—示例笔记本，它下载以前笔记本输出的多个版本作为数据集，集成它们，并提交给 Kaggle。

## 进一步的改进

这种方法的问题实际上是失去了 IDE 的全部功能(比如，调试，或者处理笔记本之外的其他文件格式)。笔记本电脑中 pdb 的调试能力有限，人们可以通过终端用 Vim 或 Nano 编辑文件，但这不是很好。但我听说有一种方法可以将 VS 代码连接到 Google Colab ( [link](https://amitness.com/vscode-on-colab/) )，所以我一定会尝试一下，也许会有续集。

## 结语/总结

本文中描述的设置是对 Kaggle 开发环境的进一步改进，只需几个简单的步骤，就可以下载数据、进行数据科学工作、存储模型/输出并提交给竞赛，而无需离开 Colab 笔记本。

这可以通过以下步骤实现:

*   安装 Google Drive 并通过终端命令操作。
*   像在本地机器上一样克隆 Git repo 和跟踪文件。
*   在每个笔记本的开头创建一个脚本来运行设置命令，以模拟 Kaggle 环境、设置 API 键等等。
*   使用 Kaggle API 将数据集和比赛数据从笔记本下载到 Colab VM。
*   使用 Kaggle API 将模型版本及其输出存储为数据集。
*   使用 Kaggle API 直接从笔记本提交竞赛。

我希望你会发现这个设置很有用，它可以让你花更少的时间来配置一切，而花更多的时间来解决 Kaggle 比赛。祝你好运:)