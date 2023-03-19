# 快速 Docker 构建带有缓存(不仅仅是)的 Python

> 原文：<https://towardsdatascience.com/fast-docker-builds-with-caching-for-python-533ddc3b0057?source=collection_archive---------6----------------------->

## 在 Docker build 中安装应用程序依赖项需要很长时间？CI/CD 限制 Docker 缓存的有效性？使用私人回购？…您听说过新的 BuildKit 缓存特性吗？

![](img/9d9162d0a1693a63930e0eda166158ea.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Cameron Venti](https://unsplash.com/@ventiviews?utm_source=medium&utm_medium=referral) 拍摄的照片

作为一名开发人员，对我的生产力来说最重要的事情之一是*做出改变——构建——测试*循环的速度。它目前涉及到用 Python 构建几个 Docker 映像。事实证明，用 Python 快速构建 Docker 并不那么简单，也没有太多好的文章。在我迈向高效构建的旅程中，我学到了比我想要的更多的关于 Docker 缓存的知识，我想在这里与你分享我的发现。

我将首先解释 Docker 为提供的**不同的缓存选项，以及新的 **BuildKit** 后端(Docker 18.09+)，并逐步展示**如何将它们组合起来**，这样您就不会在等待构建管道时多花一秒钟。不耐烦的可以直接跳到**文末**的**完整解决方案**。**

虽然代码示例显示了一个安装了[poems](https://python-poetry.org/)、**的 Python 应用程序，但是大多数技术也适用于其他语言**或者应用程序在 Docker 中的构建阶段需要很长时间并且缓存会有所帮助的情况。

# 挑战

有几件事会使快速构建变得具有挑战性:

*   应用程序有许多依赖项，下载和安装需要很长时间。
*   CI/CD 作业在我们控制有限的机器上运行。特别是，我们不能依赖于 Docker 缓存的存在。
*   有些依赖项是从私有存储库安装的，需要秘密凭据进行身份验证。
*   生成的图像如此之大，以至于从注册表中推送和提取都要花费不可忽略的时间。

我们想要的是尽可能快的构建、映像拉取和映像推送，即使有这些限制。但是在我们找到解决方案之前，让我们看看我们有哪些工具可以使用。

# 工具

![](img/077125312d58ba00c7cd30741814543c.png)

图片来自[makeameme.org](https://makeameme.org/meme/cache-cache-everywhere)

## Docker 层缓存

每个人都知道 Docker [图层](https://docs.docker.com/glossary/#layer)和缓存——除非图像图层的输入发生变化，否则 Docker 可以重用本地缓存的图层。只需仔细排序 Dockerfile 命令，以避免缓存无效。

## 外部缓存源

如果我们没有可用的本地缓存，例如在 CI/CD 代理上，该怎么办？解决这个问题的一个不太为人所知的特性是[外部缓存源](https://docs.docker.com/engine/reference/commandline/build/#specifying-external-cache-sources)。您可以使用`build`命令的`**--cache-from**`标志在注册表中提供先前构建的映像。Docker 将检查图像的清单，并提取任何可以用作本地缓存的层。

要让它发挥作用，有几个注意事项。[需要 BuildKit](https://docs.docker.com/engine/reference/builder/#buildkit) 后端——这需要 Docker 版本≥18.09，并在调用`docker build`之前设置一个`DOCKER_BUILDKIT=1`环境变量。源图像也应该用`--build-arg BUILDKIT_INLINE_CACHE=1`构建，这样它就嵌入了缓存元数据。

## 建造坐骑

当谈到在 Docker 构建中使用缓存目录时，有人可能会认为我们可以从主机挂载它。很简单，对吧？除了[不支持](https://stackoverflow.com/a/26053710)。幸运的是，BuildKit 增加了另一个有帮助的特性:[构建挂载](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#build-mounts-run---mount)。它们支持在单个`RUN`指令的持续时间内从各种来源挂载一个目录:

```
RUN --mount=type=cache,target=/var/cache/apt \
  apt update && apt-get install -y gcc
```

有几种类型的安装，例如，

*   `bind` mount 允许您从映像或构建上下文中挂载一个目录；
*   mount 挂载一个目录，其内容将在两次构建之间被本地缓存。

然而，挂载的内容对于 docker 文件中后面的任何指令都是不可用的。

为了使构建挂载工作，需要在 Dockerfile `# syntax=docker/dockerfile:1.3`中包含一个特殊的第一行，并使用`DOCKER_BUILDKIT`环境变量启用 BuildKit。

# 逐步改进构建

现在我们知道了 Docker with BuildKit 提供了什么，让我们将它与一些最佳实践结合起来，尽可能地改进我们的构建时间。

![](img/8301f257e94e4390d6c1e996a567d241.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Viktoria Niezhentseva](https://unsplash.com/@niezhentseva?utm_source=medium&utm_medium=referral) 拍摄

## 首先是依赖项，然后是应用程序代码

经常为 Python 图像推荐的第一个技巧是重新排序指令，以便应用程序代码中的更改不会使已安装依赖关系的图层的缓存失效:

现在，只有当`pyproject.toml`或锁定文件改变时，具有相关性的层才会被重建。顺便说一下，有些人建议[用 pip](https://stackoverflow.com/a/57886655) 而不是 poem 安装依赖项，但是也有[的理由不要](https://github.com/python-poetry/poetry/issues/1301#issuecomment-523915318)。

## 多阶段构建和虚拟环境

另一件显而易见的事情是利用[多阶段构建](https://docs.docker.com/develop/develop-images/multistage-build/)，以便最终映像只包含必要的生产文件和依赖关系:

你可以看到我们在最后阶段使用了一个较小的基础图像(`3.8-slim`)和`--no-dev`诗歌选项来使结果更小。

我们还添加了一个 Python 虚拟环境。虽然在一个已经被隔离的容器中它看起来是多余的，但是它提供了一种干净的方法来在构建阶段之间转移依赖关系，而不需要不必要的系统包。激活它所需要的只是设置变量`PATH`和`VIRTUAL_ENV`(一些工具用它来检测环境)。venv 的替代方法是[构建车轮文件](https://testdriven.io/blog/docker-best-practices/#use-multi-stage-builds)。

关于诗歌的一个警告是，你应该小心`virtualenvs.in-project`的设置。以下是*不*做什么的简化示例:

```
COPY ["pyproject.toml", "poetry.lock", "/app/"]
RUN poetry config virtualenvs.in-project true && poetry install 
COPY [".", "/app"]FROM python:3.8-slim as finalENV PATH="/app/.venv/bin:$PATH"
WORKDIR /app
COPY --from=build-stage /app /app
```

这将使生成的映像像以前一样小，构建速度也一样快，但应用程序文件和依赖关系将最终出现在一个最终的映像层中，打破了对拉/推依赖关系的缓存。之前显示的正确版本*允许*在远程注册表中缓存有依赖关系的层。

## 传递存储库机密

诗歌通过`POETRY_HTTP_BASIC_<REPO>_PASSWORD`等环境变量接受[凭证。传递 PyPI 存储库凭证的一个简单的解决方案是用`--build-arg`传递它们。不要这样做。](https://python-poetry.org/docs/repositories/#configuring-credentials)

第一个原因是安全。变量将[保持嵌入](https://docs.docker.com/engine/reference/builder/#arg)在图像中，你可以用`docker history --no-trunc <image>`验证。另一个原因是，如果你使用临时凭证(例如，由你的 CI/CD 提供)，在 `**--build-arg**` 中传递凭证或者通过`**COPY**` 指令**将使缓存的具有依赖关系的层无效！**

BuildKit 再次出手相救。新推荐的方法是使用`[secret](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#run---mounttypesecret)`建造坐骑。

*   首先，准备一个包含您的凭证的`auth.toml`文件，例如:

```
[http-basic]
[http-basic.my_repo]
username = "my_username"
password = "my_ephemeral_password"
```

*   将它*放在 Docker 上下文的*之外，或者将其排除在`.dockerignore`之外(否则缓存仍然会失效)。
*   更新 docker 文件，将`# syntax=docker/dockerfile:1.3`作为第一行，并将`poetry install`命令改为

*   最后，用`DOCKER_BUILDKIT=1 **docker build** **--secret id=auth,src=auth.toml** ...`构建镜像。

缓存逻辑不考虑构建装载的内容，因此即使凭据发生变化，也将重用已安装依赖项的层。

## 没有本地缓存的缓存

我们的要求之一是在 CI/CD 作业中也利用 Docker 缓存，这些作业可能没有本地缓存可用。这就是前面提到的[外部缓存源](https://docs.docker.com/engine/reference/commandline/build/#specifying-external-cache-sources)和`**--cache-from**`可以帮助我们的时候。如果您的远程存储库是`my-repo.com/my-image`，您的构建命令将变成:

```
DOCKER_BUILDKIT=1 docker build \
  --cache-from my-repo.com/my-image \
  --build-arg BUILDKIT_INLINE_CACHE=1 \
  ...
```

这对于单阶段构建来说很好。不幸的是，我们还需要为构建阶段缓存层，这[需要](https://stackoverflow.com/questions/52646303/is-it-possible-to-cache-multi-stage-docker-builds)为它构建和推送一个单独的映像:

注意，我们在第一个构建命令中使用了`[--target](https://docs.docker.com/develop/develop-images/multistage-build/#stop-at-a-specific-build-stage)`来停止在`build-stage`阶段，并且第二个构建命令引用了`build-stage`和`latest`图像作为缓存源。

将缓存图像推送到远程注册表的另一种方法是[使用](https://github.com/linkerd/linkerd2/pull/891/files) `[docker save](https://github.com/linkerd/linkerd2/pull/891/files)`并将它们作为文件管理。

最后一件事:现在你也推你的构建阶段图像，这是一个好主意，使它也更小。设置`PIP_NO_CACHE_DIR=1` `ENV`变量可以有所帮助。

## 使用。dockerignore

最重要的是从您的构建上下文中省略不必要的文件，并使用`[.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)`排除生成 docker 映像。这里有一个你可能想忽略的例子。

## 获取 docker 构建中的缓存目录

我将提到最后一个技巧，尽管我不推荐它，除非你真的需要它。到目前为止，我们成功地避免了使用 Docker 层缓存*重复安装依赖项，除非*您更改了依赖项定义(`pyproject.toml`或`poetry.lock`)。如果我们想重用以前安装的包，即使我们改变了一些依赖关系(就像 does 在本地运行时所做的那样)，该怎么办？在`poetry install`运行之前，您需要将缓存的 venv 目录放到`docker build`容器中。

最简单的解决方案是使用`[cache](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#run---mounttypecache)`构建挂载。缺点是缓存只能在本地使用，不能跨机器重用。还要记住，构建挂载只在单个`RUN`指令期间可用，因此您需要在`RUN`指令完成之前将文件复制到映像中的不同位置(例如，使用`cp`)。

如果您自己在构建主机上管理缓存目录，那么您可以使用`[bind](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#run---mounttypebind-the-default-mount-type)`构建挂载来挂载它。同样的警告只适用于单个`RUN`指令。

另一种方法是`COPY`来自另一映像的高速缓存目录，例如，先前构建的`build-stage`映像。你可以把它拉成另一个阶段(`FROM my-image:build-stage as cache`)。棘手的部分是解决先有鸡还是先有蛋的问题:在缓存源可用之前，您的构建第一次就需要工作；而且 Dockerfile 里没有`if`。解决方案是[参数化缓存阶段所基于的图像](https://stackoverflow.com/a/54245466):

```
ARG VENV_CACHE_IMAGE=python:3.8FROM $VENV_CACHE_IMAGE as cache
RUN python -m venv /venvFROM python:3.8 as build-stage
# ...
COPY --from=cache /venv /venv
RUN poetry install --remove-untracked
```

如果您已经有一个可用的`build-stage`映像，将`VENV_CACHE_IMAGE`构建参数指向它。否则，使用一些其他可用的图像作为默认图像，`RUN python -m venv /venv`指令将确保一个空的`/venv`目录可用，这样`COPY`就不会失败。

# 完整的解决方案

![](img/9a2540584e55b2335c8d562dbce4166d.png)

照片由[迈克·多尔蒂](https://unsplash.com/@mikedoherty?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

让我们总结一下您可以采取哪些步骤来加快构件速度并缩小图像:

*   对 Dockerfile 指令进行重新排序，以便只有`COPY`的依赖项规范位于依赖项安装之前。稍后复制应用程序文件。
*   利用[多阶段构建](https://docs.docker.com/develop/develop-images/multistage-build/)，仅将虚拟环境(或轮子)的必要依赖项复制到最终映像，使其变小。
*   不要将具有依赖关系的应用程序代码与`virtualenvs.in-project = true`混在一起。
*   使用一个`[secret](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#run---mounttypesecret)`构建挂载来传递存储库凭证。
*   如果本地缓存不可用，使用`[--cache-from](https://docs.docker.com/engine/reference/commandline/build/#specifying-external-cache-sources)`重用注册表中的图像。这可能需要将构建阶段的一个单独的映像推送到注册表中。
*   如果你绝对需要在`docker build`期间获得一个缓存目录到一个容器，使用`[cache](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#run---mounttypecache)`或`[bind](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#run---mounttypebind-the-default-mount-type)`构建挂载，或者`COPY`从另一个映像获取它作为额外的构建阶段。请记住，要正确实施所有选项都有点棘手。

由此产生的`Dockerfile`可能是这样的:

为 Python 应用程序定制的 Docker 文件，用于优化 Docker 缓存

(出于安全原因，我还添加了指令，使应用程序不能在`root`下运行。你可以在这篇伟大的文章中找到这个和其他有用的提示。)

在您的配置项/光盘中构建和推送映像可能如下所示:

使用构建阶段缓存支持构建上述 docker 文件的脚本

如您所见，在 Docker 中使用 Python 进行适当的缓存并不简单。但是当你知道如何使用新的 BuildKit 特性时，Docker 可以为你工作，而不是与你作对，并做许多繁重的工作。