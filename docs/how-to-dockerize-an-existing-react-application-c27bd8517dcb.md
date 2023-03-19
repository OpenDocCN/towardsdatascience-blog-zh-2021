# 如何对现有的 React 应用程序进行 Dockerize

> 原文：<https://towardsdatascience.com/how-to-dockerize-an-existing-react-application-c27bd8517dcb?source=collection_archive---------16----------------------->

## 轻松运行 React 应用程序

![](img/dd1432493ec5eacdc73053ee7592bb6d.png)

来自 [Pexels](https://www.pexels.com/photo/man-people-woman-relaxation-6860464/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

eact 是一个创建用户界面的 Javascript 库。React 不会将不同文件中的标记和逻辑分开。它使用包含两者的可重用组件来分离关注点。这些组件可用于构建可伸缩的 web 应用程序。

在这篇文章中，我将说明如何对现有的 React 应用程序进行 dockerize。你想这么做可能有很多原因。例如，与开发人员合作。通过使用 docker，其他开发人员不必担心管理开发环境。相反，他们可以专注于构建应用程序。

# 设置

对于第一步，您需要安装 [Docker](https://docs.docker.com/engine/install/) 并从 [GitHub](https://github.com/lifeparticle/netlify_react) 下载一个 git 库用于第二步。这个 Git 存储库包含一个演示 React 应用程序。在这个存储库中，我还添加了一个 **Dockerfile** 和 **docker-compose.yml** 。对于这个设置，我使用的是 macOS。

现在，让我们来分解一下 [**Dockerfile**](https://github.com/lifeparticle/netlify_react/blob/main/Dockerfile) 文件的各个成分。

```
FROM node:16-slim

ADD . /netlify_react
WORKDIR /netlify_react
RUN npm install
```

首先，`From`命令用于定义父图像。这里我们使用的是 Docker Hub 预建的官方图片 [Node.js](https://hub.docker.com/_/node) 。之后，`ADD`命令用于将当前文件夹中的所有内容添加到镜像中一个名为 **netlify_react、**的目录中，后跟`WORKDIR`命令，用于将工作目录设置为 **netlify_react** 。最后，我们使用`RUN`命令运行`npm install`命令。

现在，让我们分解一下[**docker-compose . yml**](https://github.com/lifeparticle/netlify_react/blob/main/docker-compose.yml)文件的各个成分。

```
version: '3.8'
services:
  web:
    build: .
    command: npm start
    volumes:
      - .:/netlify_react
    ports:
      - "3000:3000"
```

首先，我们有**版本**标签，用于定义合成文件格式。然后，我们有了 **services** 标签，它用于定义我们的应用程序想要使用的服务。在这种情况下，我们有一个名为 **web** 的服务。之后， **build** 命令用于构建我们的 Docker 映像，它将基于我们的**Docker 文件**创建一个 Docker 映像。我们可以使用**命令**标签运行 React 应用程序。为了定义主机和容器端口，我们可以使用 **ports** 标签。对于我们的应用程序，它将主机上的端口 3000 映射到容器上的端口 3000。默认情况下，React 在端口 3000 上运行。最后，我们有 **volumes** 标签，用于将文件夹从主机挂载到容器。

现在，让我们从 **docker-compose.yml** 文件所在的目录运行下面的命令。以下命令将启动并运行整个应用程序。

```
docker compose up
```

太棒了。我们已经通过在 Docker 容器中运行成功地将 React 应用程序进行了 Docker 化。现在，您可以通过访问 URL[http://localhost:3000](http://localhost:3000)来访问 React 应用程序。

管理环境是一项挑战。当涉及到协作时，问题就更多了。通过使用 Docker，我们可以省略这些问题，节省时间和精力。如果你想了解更多关于 Docker 的知识，请阅读下面的帖子，我在其中介绍了如何将 Ruby on Rails、Sinatra 和 Flask 应用程序 dockerize。编码快乐！

# 相关职位

</how-to-dockerize-an-existing-ruby-on-rails-application-3eb6d16ec392>  </how-to-dockerize-an-existing-sinatra-application-3a6943d7a428>  </how-to-dockerize-an-existing-flask-application-115408463e1c> 