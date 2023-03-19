# 使用 Docker 构建图像的完整教程

> 原文：<https://towardsdatascience.com/complete-tutorial-on-building-images-using-docker-1f2be49ea8a6?source=collection_archive---------16----------------------->

## 通过一个设置 Ubuntu、默认用户、Miniconda、PyTorch 的例子来了解使用 Docker 编写 Dockerfile 和运行容器所需的一切。

Docker 提供了一种在主机操作系统上运行你的程序的方法。 [Dockerfile](https://docs.docker.com/engine/reference/builder/) 提供了关于如何构建映像的说明，然后这些映像作为容器运行。这篇文章讨论了创建 Dockerfile 所需的所有东西，并以设置 [Ubuntu](https://ubuntu.com/) 、 [Miniconda](https://docs.conda.io/en/latest/miniconda.html) 和 [PyTorch](https://pytorch.org/) 为例。

# 建立形象

`docker build`命令用于从`Dockerfile`和*上下文*(路径或 URL)构建图像。*上下文*是指构建命令中指定的所有文件。构建映像的步骤如下

*   由*上下文*指定的所有文件都被发送到 docker 守护进程。因此，您应该在空目录中创建 Dockerfile，以避免不必要的传输。
*   检查 docker 文件是否有语法错误
*   Docker 守护程序通过读取来自`Dockerfile`的指令开始构建映像。

## 指定上下文

*上下文*可以是本地文件系统上的`PATH`或引用 Git 存储库的远程`URL`。下面显示了一个指定构建上下文的示例

```
> docker build .> docker build /path/to/context
```

## 用 URL 指定上下文

Docker 还为您提供了从 Git URL 构建图像的能力。这主要用于持续集成管道。要使用 URL 构建映像，docker 文件应该位于指定的 URL(而不是本地文件系统)。`docker build`从 Github URL 构建图像时，将自动执行以下步骤

```
> git clone {GITHUB_URL}
> git checkout {BRANCH}
> cd repo
> docker build .
```

构建映像的命令如下

```
> docker build {GITHUB_URL}#{BRANCH}> docker build https://github.com/KushajveerSingh/Dockerfile.git#master
```

> ***注意:-*** *由于指定位置没有 Dockerfile，上述命令将失败。*

## 指定 Dockerfile 文件的位置

您可以使用`-f`标志来指定 Dockerfile 的路径。默认情况下，假定 Dockerfile 位于上下文的根位置。

```
> docker build -f /path/to/Dockerfile .> docker build -f ubuntu_conda_pytorch/Dockerfile https://github.com/KushajveerSingh/Dockerfile.git#master
```

Docker 会自动将`cd`放入 Github 存储库，所以路径不应该包含存储库的名称。

## 指定存储库和标签

考虑下面的 Dockerfile 文件

```
FROM ubuntu:18.04LABEL PURPOSE = "test Dockerfile"
```

当我们构建一个映像时，docker 分配一个提交散列作为`IMAGE ID`。

您可以为每个 docker 图像指定一个存储库和标签，然后使用`-t`标志轻松访问该图像。`REPOSITORY`可以被认为是你的 Github 库的名字，而`TAG`是用来指定你的镜像版本的。比如，`ubuntu:18.04`，`ubuntu:latest`。

您也可以指定多个标签

```
> docker build -t test:0.1 -t test:latest .
```

## Docker 图像层

Docker 守护进程从上到下逐个运行指令。并且大多数指令的结果(`FROM`、`ADD`、`COPY`)被提交给新的图像。由于这个原因，你需要小心使用这些指令，因为每一次使用它们都会导致创建一个新的图像，从而增加最终图像的大小。

Docker 为什么要这么做？考虑下面的 Dockerfile 文件

```
FROM ubuntu:18.04RUN COMMAND_1
RUN COMMAND_2
RUN COMMAND_1
```

现在，当我们建立的形象，我们将创建以下层

*   来自`ubuntu:18.04` Dockerfile 的层
*   `RUN COMMAND_1`将创建一个新层
*   `RUN COMMAND_2`将创建一个新层
*   `RUN COMMAND_3`将创建一个新层

层基本上是图像或中间图像上的变化。每当我们运行一条指令(如`FROM`、`RUN`、`COPY`)时，我们都在对之前的图像进行修改。这些变化导致新层的创建。拥有中间层有助于构建过程。如果您在 Docker 文件中进行了更改，那么 Docker 将只构建被更改的层和之后的层。这样可以节省很多时间。

> ***注意:-*** *创建新图层时要小心，因为这会增加图像的尺寸。*

## 构建工具包

Docker 支持 [moby/buildkit](https://github.com/moby/buildkit) 后端构建映像。与 Docker 提供的默认实现相比，BuildKit 提供了许多好处

*   检测并跳过未使用的构建阶段
*   并行构建独立的构建阶段
*   在两次构建之间，仅增量传输构建上下文中已更改的文件
*   检测并跳过在您的构建上下文中传输未使用的文件
*   使用具有许多新特性的外部 Dockerfile 实现
*   避免其他 API(中间图像和容器)的副作用
*   为自动修剪设置构建缓存的优先级

要使用 BuildKit 后端，您需要设置`DOCKER_BUILDKIT=1`环境变量。

```
> DOCKER_BUILDKIT=1 docker build .
```

运筹学

```
> export DOCKER_BUILDKIT=1
> docker build .
```

## 摘要

总之，要构建 Docker 映像，可以使用以下命令

# 转义字符

这是一个解析器指令的例子。解析器指令是在 Dockerfile 的第一行中指定的，不会向构建中添加层。您可以使用这个来指定在 Dockerfile 中换行的字符。

```
# escape=\
FROM ubuntu:18.04RUN INSTRUCTION_1 \
    INSTRUCTION_2
```

在 Windows 上`\`用于指定路径。因此，将它更改为类似于反勾号的东西会很有用。

```
# escape=`
FROM ubuntu:18.04RUN INSTRUCTION_1 `
    INSTRUCTION_2
```

# 从

所有 Dockerfile 文件都以`FROM`指令开始。`FROM`初始化新的构建阶段，并为后续指令设置基础映像。一般语法是

```
FROM [--platform=<platform>] <image>[:tag] [AS <name>]
```

这里的[…]表示可选。您可以从一个零映像开始，并在此基础上构建一切

```
FROM scratch
```

或者你可以建立在一个公共形象之上(比如 [Ubuntu](https://hub.docker.com/_/ubuntu) 、 [PyTorch](https://hub.docker.com/r/pytorch/pytorch) 、 [nvidia/cuda](https://hub.docker.com/r/nvidia/cuda) )

```
FROM ubuntu:18.04
```

在这篇文章中，我建立在 Ubuntu 图像之上。您可以使用以下命令构建映像并尝试运行它

```
> DOCKER_BUILDKIT=1 docker build -t test:0.1 .
> docker run -it --name temp test:0.1
```

您会看到这是一个基本安装。它没有任何用户或 sudo。它为我们提供了 Linux 内核，我们必须在此基础上构建一切。

在接下来的几节中，我们将查看 docker 文件中的所有指令，然后我们将使用所有这些指令构建一个 Ubuntu/Miniconda/PyTorch 映像。

# 参数和环境

环境变量通常用于在脚本中声明变量，或者设置一些在容器运行时会持续存在的变量。Docker 允许我们用两种方式设置变量:`ARG`和`ENV`。

*   `ARG`指令定义了一个变量，用户将在编译时通过使用`--build-arg <name>=<value>`标志的`docker build`命令传递该变量。这些将只在 Dockerfile 文件中使用。
*   `ENV`指令设置 Dockerfile 文件中的环境变量，当从结果图像运行容器时，环境变量将持续存在。

## 银

我们可以将 Ubuntu 的版本指定为`ARG`(docker file 的代码如下所示)

```
ARG UBUNTU_VERSION
FROM ubuntu:$UBUNTU_VERSION
```

然后我们可以在构建映像时指定 ubuntu 的版本

```
> DOCKER_BUILDKIT=1 docker build -t test --build-arg UBUNTU_VERSION=18.04 .
```

我们还可以为`ARG`指定一个默认值，如下所示

```
ARG UBUNTU_VERSION=18.04
```

要访问`ARG`的值，可以使用`$UBUNTU_VERSION`或`${UBUNTU_VERSION}`语法。当您想要访问字符串中的值`ARG`时，第二种方法很有用。

使用`ARG`

*   对于只在 docker 文件中需要而在容器运行时不需要的变量，使用`ARG`。在这种情况下，当容器运行时，不需要 Ubuntu 的版本。
*   在`FROM`之前使用的`ARG`只能在`FROM`中使用
*   在`FROM`之后使用的`ARG`可以在 docker 文件中的任何地方使用(在多阶段构建的情况下有一个例外，即当我们在同一个 docker 文件中使用多个`FROM`指令时)

## 包封/包围（动词 envelop 的简写）

这与`ARG`相同，除了当容器从结果图像运行时`ENV`将持续。这方面的一个例子包括

```
ENV PYTORCH_VERSION 1.9.0
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64# Setting PATH variables
ENV PATH  /home/default/miniconda3/bin:$PATH
```

# 贴上标签并曝光

这两个说明可以被认为是文档说明。Dockerfile 中的这些指令对图像没有影响，它们只是用来提供元数据信息。

## 标签

`LABEL`可用于指定 Dockerfile 的作者等相关信息

```
LABEL author = "Kushajveer Singh"
LABEL email = "kushajreal@gmail.com"
LABEL website = "kushajveersingh.github.io"
```

构建图像后，您可以获得标签列表，如下所示

## 揭露

`EXPOSE`指令通知 Docker 容器在运行时监听指定的网络端口。它实际上并不发布端口。它只是作为构建映像的人和运行容器的人之间的一种文档，说明打算发布哪些端口。

```
EXPOSE 8889
EXPOSE 80/udp
EXPOSE 80/tcp
```

现在，运行容器的人可以使用`-p`标志指定端口，如下所示

```
> DOCKER_BUILDKIT=1 docker build -t test .
> docker run -p 80:80/tcp -p 80:80/udp test
```

# 添加并复制

我们在开始时讨论过，在读取 Docker 文件之前，会将一个*上下文*传递给 Docker 守护进程。现在，要从上下文向图像添加文件，我们可以使用`ADD`或`COPY`。两个指令相似，但是`ADD`做了一些额外的事情(你需要小心)。两个命令的语法是相同的

```
ADD [--chown=<user>:<group>] <src>... <dest>COPY [--chown=<user>:<group>] <src>... <dest>
```

一个使用示例是

```
COPY --chown=default:sudo /file/on/local/drive /path/on/imageCOPY --chown=default:sudo script.sh /home/default
```

`<dest>`路径要么是绝对路径，要么是相对于`WORKDIR`的路径，我们将在后面讨论。

现在让我们看看每条指令之间的区别。

## 复制

```
COPY [--chown=<user>:<group>] <src>... <dest>
```

`COPY`指令从`<src>`复制文件或目录，并将它们添加到位于`<dest>`的容器文件系统中。除非您指定了`--chown`，否则这些文件是用默认的 UID 和 GID 0 创建的。就是这样。`COPY`会将一个文件从`<src>`复制到`<dest>`。

## 注意缺陷障碍 (Attention Deficit Disorder)

```
ADD [--chown=<user>:<group>] <src>... <dest>
```

`ADD`指令也像`COPY`一样将文件或目录从`<src>`复制到`<dest>`，但它也做一些额外的事情

*   `<src>`可以是远程文件的 URL
*   如果`<src>`是一个 *tar* 文件(identity，gzip，bzip2，xz ),那么它将被解压为一个目录。如果 tar 文件是一个远程 URL，那么它不会被解压缩

> ***注意:-*** *由于* `*ADD*` *的这些额外特性，建议您使用* `*COPY*` *，除非您知道* `*ADD*` *到底在给图像添加什么。*

## 工作方向

`WORKDIR`指令为 docker 文件中跟随它的任何`RUN`、`CMD`、`ENTRYPOINT`、`COPY`、`ADD`指令设置工作目录。您可以根据需要多次使用该选项来设置工作目录。

```
WORKDIR /home/default
RUN ...# You can also provide path relative to previous WORKDIR
WORKDIR ../home
```

# 奔跑

该命令的语法是

```
RUN <command>
```

它将在一个 shell 中运行该命令(在 Linux 上默认为`/bin/sh -c`)。每个`RUN`指令将创建一个新层，因此，您应该尝试将多个`RUN`指令组合成一个逻辑组。

要对多个`RUN`指令进行分组，可以使用分号或`&&`。

最好使用`&&`而不是`;`。原因是当您使用分号将多个命令分组时，无论前一条指令是否出错，下一条指令都会运行。`&&`的情况并非如此。如果一个命令失败，那么执行将停止，并且下一个命令将不会被执行。

这些都是我们创建 Docker 映像所需的说明。我留下了一些说明，因为我们不需要它们，但你可以查看完整的[文档参考](https://docs.docker.com/engine/reference/builder/)以了解关于这些命令的信息(如`ENTRYPOINT`、`CMD`、`VOLUME`)。

# 所有命令的摘要

*   `FROM` -每个 docker 文件都以这条指令开始，它提供了我们构建映像的基础映像
*   `ARG`——我们可以使用`--build-arg`指定命令行参数，只在 docker 文件中使用
*   `ENV` -将在 Dockerfile 中使用并在容器运行时保持的环境变量
*   `LABEL` -指定元数据，如作者姓名，...
*   `EXPOSE` -记录集装箱运行时计划使用的端口
*   `COPY` -将文件或目录从上下文添加到图像(仅复制文件)
*   `ADD` -将文件或目录从上下文添加到图像(可以从远程 URL 复制，自动解压缩 tar 文件)
*   `WORKDIR` -指定使用 path 的其他指令的工作目录
*   `RUN` -从 shell 运行任何命令
*   `USER` -设置容器运行时的默认用户

# PyTorch Dockerfile，Ubuntu

## 基础图像

我们将使用 [Ubuntu](https://hub.docker.com/_/ubuntu) 作为基础图片。如前所述，它为我们提供了一个基本的 Linux 发行版，我们必须设置我们需要的一切。

```
ARG UBUNTU_VERSION=18.04
FROM ubuntu:$UBUNTU_VERSION
```

## 更新 ubuntu 并安装实用程序

在讨论正在发生的事情之前，让我们先检查一下文档

第一步是更新 Ubuntu。注意 Ubuntu 上的默认用户是`root`。之后，我们设置`sudo`和一个`default`用户，然后我们将不得不在这些指令后面加上`sudo`。这是通过使用

`--fix-missing`是可选的。它在依赖关系破裂的情况下使用，在大多数情况下，使用此标志可以帮助我们解决问题。因为我们是从全新安装开始的，所以这个标志没有什么作用。

`apt install -y --no-install-recommends`。`-y`标志帮助我们绕过是/否提示。Ubuntu 中的每个包都有三个依赖项

*   主要依赖关系
*   推荐的软件包
*   建议的套餐

默认情况下，Ubuntu 将安装主软件包和推荐软件包(要安装推荐软件包，您需要向`apt install`提供`--install-suggests`标志)。我们的主要目标是保持 docker 映像的大小最小，由于这个原因，我们不想浪费空间安装推荐的包。`--no-install-recommends`标志会这样做，因此我们只安装主要的依赖项。

现在你可以安装你可能需要的任何其他包，比如`ca-certificates`(需要`curl`)、`sudo`、`curl`、`git`。

第二步是清理不再需要的包，并清除所有本地缓存。这是通过使用

```
RUN apt clean && \
    rm -rf /var/lib/apt/lists/*
```

当我们安装包时，Ubuntu 在`/var/cache`中维护了一个包的缓存。这样做的原因是，如果升级时出现问题，我们无法访问网络连接，那么我们可以恢复到缓存中的旧版本来降级软件包。但是我们不需要 Docker 图像的缓存，所以我们可以使用`apt clean`删除它。具体来说，`apt clean`将删除`/var/cache/apt/archives/`和`/var/cache/apt/archives/partial`中的文件，留下一个`lock`文件和`partial`子目录。

`/var/lib/apt`存储与 apt 包管理器相关的数据。我们每次运行`apt update`时都会自动下载这些数据，因此没有必要存储这些数据，我们可以安全地删除这些数据，以使用`rm -rf /var/lib/apt/lists/*`减小图像大小。

> ***注意:-*** *要在* `*rm -rf /var/lib/apt/lists/**` *之后安装一个软件包，你必须先运行* `*apt update*` *，然后才可以安装你需要的软件包。*

## 设置 sudo 和默认用户

下一步是设置 root 帐户和默认用户。这样做的 docker 文件的内容如下所示

让我们看看在`useradd`每面旗都做了什么

*   `-r`用于创建系统帐户，即操作系统在安装过程中创建的帐户或 root 帐户
*   `-m`用于创建一个主目录，如果它不存在的话
*   `-d /home/default`用于提供主目录的位置
*   `-s /bin/bash`用于指定用户登录 shell 的名称。如果你愿意，可以跳过这个。
*   `-g root -G sudo`这个有意思。`-g`标志用于指定用户所属的组，而`-G`用于提供用户所属的其他组的列表。

默认情况下，`root`用户不是组`sudo`的成员，我们需要显式设置这一点。`root`拥有系统上的所有权限，但我们仍然需要一个`sudo`组。当用户属于`sudo`组时，这意味着用户可以使用他们的用户密码来执行`sudo`命令。

当我们在一个系统上有多个用户时，拥有一个`sudo`组是很有用的，每个用户都可以通过使用他们自己的用户密码获得`root`特权。

*   `-u 1000`用于提供用户 ID
*   `$USERNAME`是将要创建的用户的名称

默认情况下，在 Ubuntu 中，root 帐户没有设置密码。要设置密码，我们可以使用以下命令

```
echo "${USERNAME}:${PASSWORD}" | chpasswd
```

在完成上述步骤后，我们完成了以下工作

*   增加了一个用户`default`
*   用户`default`可以使用`sudo su`并使用`default`作为密码获得 root 权限

这里的[讨论了`sudo`的一个错误](https://github.com/sudo-project/sudo/issues/42)，每次你试图用`sudo`执行一个命令时，你都会得到下面的警告信息

```
> sudo hello > /dev/null
sudo: setrlimit(RLIMIT_CORE): Operation not permitted
```

这个问题已经在最新的补丁中解决了，但是 Ubuntu 没有附带，所以要停止这个烦人的警告，你可以使用下面的命令

```
echo "Set disable_coredump false" >> /etc/sudo.conf
```

当你运行这个容器时，你会看到一个关于`sudo`的提示，如下所示

```
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

当你第一次用`sudo`运行一个命令时，这个消息将被删除，或者你可以通过添加`~/.sudo_as_admin_successful`文件来删除它。

```
touch /home/$USERNAME/.sudo_as_admin_successful
```

就这样，您已经设置了`sudo`和一个`default`用户。当你运行容器时，默认情况下你会是`root`。但是您可以使用下面的命令将默认用户设置为`default`

```
USER $USERNAME
WORKDIR /home/$USERNAME
```

# 到目前为止的 Dockerfile 文件摘要

我们的 docker 文件将包含以下内容

我们可以将所有更新 Ubuntu、设置`sudo`和默认用户的命令放在一个`RUN`命令中以节省空间。

## 访问 Nvidia GPU

要访问 Docker 容器中的主机 GPU，可以在运行容器时指定`--gpus`标志

```
> docker run --gpus all -it --name temp test:0.1> docker run --gpus 0,2 -it --name temp test:0.1
```

这只需要您在主机上安装 Nvidia 驱动程序，然后您可以使用 Docker 容器中的`nvidia-smi`来检查是否正在检测 GPU。

## 安装 Miniconda

[Miniconda](https://docs.conda.io/en/latest/miniconda.html) 可以用于一个简单的 python 设置。只需从文档页面(在本例中是 Python 3.9)获取最新的 Linux 安装程序的链接，并指定要安装 Miniconda 的位置

Miniconda 的设置非常简单

*   设置`MINICONDA_DOWNLOAD_LINK`和`MINICONDA_INSTALL_PATH`变量
*   设置环境变量`ENV PATH ${MINICONDA_INSTALL_PATH}/miniconda3/bin:$PATH`，这将允许我们从文件系统中的任何地方运行 conda
*   使用`bash Miniconda.sh -b -p ./miniconda3`安装 miniconda
*   使用`rm Miniconda.sh`移除`.sh`文件以节省空间
*   `conda init`可以跳过，因为我们已经设置了`ENV PATH`
*   使用`conda update -y --all`将 conda 软件包更新至最新版本

## 安装 PyTorch

现在我们准备安装 PyTorch。前往 [PyTorch 安装页面](https://pytorch.org/get-started/locally/)获取您想要用来安装 PyTorch 的命令。

> ***注意:-*** *由于某种原因* `*numpy*` *在我运行* `*conda install*` *命令时没有被安装，所以我必须添加* `*pip install numpy*` *命令。*

## 移除 conda/pip 缓存

Conda 存储索引缓存、锁文件、tarballs 和未使用包的缓存。使用命令`conda clean -afy`可以安全移除这些以节省空间。所以把这个作为 Dockerfile 中的最后一个 conda 命令添加进去。

`pip`将缓存存储在`~/.cache/pip`中，该文件夹可以使用命令`rm -rf ~/.cache/pip`安全移除。

## 建立形象

现在我们已经准备好构建映像了。

```
> DOCKER_BUILDKIT=1 docker build -t test:0.1 .
```

然后我们可以运行一个容器来测试图像

```
> docker run --gpus all -it --name temp test:0.1
default@a7f862b6bf73:~$
```

我们可以尝试`nvidia-smi`来检查是否正在检测 GPU

正如我们所看到的，GPU 和主机的 Nvidia 驱动程序一起被检测到。接下来，我们可以尝试运行一些 PyTorch 命令

我们已经成功地安装了 PyTorch，并且拥有了一个正常工作的 Ubuntu 环境。下一节讨论构建映像、运行容器和将映像推送到`dockerhub`所需的各种`docker`命令。

# 有用的 Docker 命令

## 构建图像

我们已经在第一部分讨论了从 Dockerfile 构建图像的命令。

此处可访问`docker build`的参考[。](https://docs.docker.com/engine/reference/commandline/build/)

## 列出所有图像

`docker image ls`可用于获取本地文件系统上所有图像的列表。

您可以使用这个命令来检查图像的`SIZE`并获取`IMAGE ID`，以防您忘记标记图像。Docker 将图像存储在 Linux 上的`/var/lib/docker/`中，但是弄乱这个文件夹的内容并不是一个好主意，因为 Docker 的存储很复杂，并且它取决于正在使用的[存储驱动](https://github.com/moby/moby/blob/990a3e30fa66e7bd3df3c78c873c97c5b1310486/daemon/graphdriver/driver.go#L37-L43)。

`docker image ls`的参考值可在处访问[。](https://docs.docker.com/engine/reference/commandline/image_ls/)

## 删除图像

通过指定标签或`IMAGE ID`，可以使用`docker image rm`删除图像。

`docker image rm`的参考值可在处访问[。](https://docs.docker.com/engine/reference/commandline/image_rm/)

如果您有许多未标记的图像，您可以使用`docker image prune`删除所有图像，而不是手动删除每一张图像。

`docker image prune`的参考可在处访问[。](https://docs.docker.com/engine/reference/commandline/image_prune/)

## 列出所有容器

`docker container ls -a`可用于列出所有容器(运行和停止)。如果您想只列出正在运行的容器，请使用`docker container ls`。

我们可以得到所有集装箱的状态。在上述示例中，`temp_1`未运行，而`temp_2`正在运行。我们还可以看到容器没有使用任何`PORTS`。

`docker container ls`的参考可在处访问[。](https://docs.docker.com/engine/reference/commandline/container_ls/)

## 启动容器

`docker start`可用于启动一个或多个停止的容器(`docker container start`也可用于此)。

```
> docker start temp_1
temp_1
```

`docker start`的参考可在处访问[。](https://docs.docker.com/engine/reference/commandline/start/)

## 附加到容器

要打开一个新的终端会话，可以使用 docker 容器`docker attach`。

```
> docker attach temp_1
```

此处可访问`docker attach`的参考值[。](https://docs.docker.com/engine/reference/commandline/attach/)

## 停止集装箱

`docker stop`或`docker kill`可用于停止多个运行中的集装箱。`docker stop`可以认为是优雅的一站。这两个命令的区别在于

*   `docker stop`。停止正在运行的容器。主进程将接收到`SIGTERM`，在一段宽限期后，将接收到`SIGKILL`。
*   `docker kill`。杀死一个正在运行的容器。主进程将接收`SIGKILL`或用户使用`--signal`指定的任何信号

```
> docker stop temp_1
> docker kill temp_1
```

此处可访问`docker stop`的参考[，此处](https://docs.docker.com/engine/reference/commandline/stop/)可访问`docker kill`的参考[。](https://docs.docker.com/engine/reference/commandline/kill/)

## 删除容器

`docker rm`可用于删除停止的集装箱。

```
> docker rm temp_1
```

在某些情况下，您可能需要删除一个不存在的容器，同时不抛出错误。这可以按如下方式完成

```
> docker rm temp_1 || true
```

`docker rm`的参考可在处访问[。](https://docs.docker.com/engine/reference/commandline/rm/)

## 运行容器

`docker run`命令用于从图像中创建一个容器。使用该命令时，有许多有用的标志

```
> docker run -it --gpus all --name temp -p 8888:8888 test:0.1
```

*   `-it`将打开一个与容器连接的终端
*   `--gpus all`用于指定哪些 GPU 可以访问容器
*   `--name temp`容器的名称
*   `-p 8888:8888`发布端口。格式为`{HOST_PORT}:{CONTAINER_PORT}`。要在 docker 中使用 jupyter 笔记本，您需要发布一个端口。

此处可访问`docker run`的参考[。](https://docs.docker.com/engine/reference/run/)

## 将新码头连接到集装箱

如果您想要将一个新的终端连接到一个正在运行的容器，那么您可以使用下面的命令

```
> docker exec -it {container_name} bash
```

`docker attach {container_name}`无法创建新的终端会话。它将连接到旧会话。

## 删除所有容器/图像

要删除系统上的所有 docker 容器和映像，您可以使用以下两个命令(按照指定的顺序)

```
# Remove all containers
> docker rm -vf $(docker ps -a -q)# Remove all images
> docker rmi -f $(docker images -a -q)
```

## 将图像推送到 Dockerhub

使用以下命令将图像推送到 [Dockerhub](https://hub.docker.com/)

下面显示了一个工作示例

第一个参数(`test:0.1`)到`docker tag`是本地文件系统上映像的名称，第二个参数是 Dockerhub 上您想要将映像推到的位置。

上述命令的参考资料

*   `docker login` [链接](https://docs.docker.com/engine/reference/commandline/login/)
*   `docker tag` [链接](https://docs.docker.com/engine/reference/commandline/tag/)
*   `docker push` [链接](https://docs.docker.com/engine/reference/commandline/push/)

# 链接

*   [kushayveersingh/Dockerfile](https://github.com/KushajveerSingh/Dockerfile)—您可以从这个存储库中访问 docker file 来构建您自己的映像。在[自述文件](https://github.com/KushajveerSingh/Dockerfile/blob/main/README.md)中提供了所有可用 docker 文件的文档。
*   所有的图片都可以在这个链接中找到。如果你想使用一个图像而不是图像的内容，那么你可以从上面的 [Github](https://github.com/KushajveerSingh/Dockerfile) 链接访问用于创建图像的 Dockerfile 并构建图像。

[twitter](https://twitter.com/Kkushaj) ， [linkedin](https://www.linkedin.com/in/kushaj/) ， [github](https://github.com/KushajveerSingh)

*原载于 2021 年 10 月 3 日*[*https://kushajveersingh . github . io*](https://kushajveersingh.github.io/blog/docker)*。*