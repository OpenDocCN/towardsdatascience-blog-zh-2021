# 如何对接现有的 Ruby on Rails 应用程序

> 原文：<https://towardsdatascience.com/how-to-dockerize-an-existing-ruby-on-rails-application-3eb6d16ec392?source=collection_archive---------4----------------------->

## 轻松运行 rails 应用程序

![](img/b7d1b908ca98c9196658ababe2a8056d.png)

照片由[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

R uby on Rails 是一个基于 Ruby 的开源 web 应用开发框架。许多大公司使用 Rails 来构建他们的产品，如 GitHub、Shopify、Airbnb 和 Twitch。

通过这篇文章，您将了解如何将现有的 Ruby on Rails 应用程序进行 dockerize，从而使开发更快更容易。

# 设置

首先，我们需要安装 [Docker](https://docs.docker.com/engine/install/) 并从 [GitHub](https://github.com/lifeparticle/Rails-Docker/tree/main) 下载一个 git 库。对于这个设置，我使用的是 macOS。

现在，我们将创建一个包含 Ruby on Rails 的 Docker 映像。让我们分解一下 [**Dockerfile**](https://github.com/lifeparticle/Rails-Docker/blob/main/Dockerfile) 文件的各个成分。

```
FROM ruby:3.0.0

RUN apt-get update -qq \
&& apt-get install -y nodejs postgresql-client

ADD . /Rails-Docker
WORKDIR /Rails-Docker
RUN bundle install

EXPOSE 3000

CMD ["bash"]
```

这里我们使用`From`指令来定义父图像。我们使用来自 Docker Hub 的预建的官方图像。之后，我们使用`RUN`指令安装所有的依赖项。现在我们需要使用`ADD`指令将当前文件夹中的所有内容添加到镜像中名为 **Rails-Docker** 的目录中。`WORKDIR`指令将工作目录设置为 **Rails-Docker** 。我们再次使用`RUN`指令来安装我们的 ruby gems。之后，我们使用 expose 指令公开容器的端口 3000。默认情况下，Rails 运行在端口 3000 上。最后，`CMD`指令用于在容器启动时运行命令。在本例中，它用于打开 bash shell。

现在，我们需要运行 **db** 和 **web** 容器。为此，我们将创建一个 Docker 合成文件。我们来分解一下[**docker-compose . yml**](https://github.com/lifeparticle/Rails-Docker/blob/main/docker-compose.yml)文件的各个成分。

```
version: '3.8'
services:
  db:
    image: postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres:/var/lib/postgresql/data
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/Rails-Docker
    ports:
      - "3000:3000"
    depends_on:
      - db
volumes:
  postgres:
```

**版本**标签用于定义合成文件格式。**服务**标签用于定义我们的应用程序想要使用的服务。这里，我们有两个服务叫做 **db** 和 **web** 。

在 **db** 服务下，我们使用预先构建的 postgres 映像来构建映像。然后我们有环境变量。最后，我们有一个名为 **postgres** 的命名卷来保存数据。

在 **web** 服务下，我们使用之前创建的 docker 文件来构建图像。**命令**标签用于运行我们的应用程序。我们使用“0.0.0.0 ”,因此服务器将监听所有可用的 IP 地址。**端口**标签用于定义主机和容器端口。它将主机上的端口 3000 映射到容器上的端口 3000。最后， **volumes** 标签用于将文件夹从主机挂载到容器。

现在从 **docker-compose.yml** 文件所在的目录运行以下命令。此命令将启动并运行整个应用程序。

```
docker compose up
```

如果遇到错误“active record::nodatabaserror ”,请运行以下命令创建数据库。

```
docker-compose run web rake db:create
```

恭喜你！现在，您可以通过访问 URL[http://localhost:3000/](http://localhost:3000/)来访问 Rails 应用程序。

我希望这能帮助你开始使用 Rails 和 Docker。现在使用 Rails 创建你的下一个大东西。编码快乐！

# 相关职位

</how-to-dockerize-an-existing-sinatra-application-3a6943d7a428>  </how-to-dockerize-an-existing-flask-application-115408463e1c> 