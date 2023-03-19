# 如何对现有的 Sinatra 应用程序进行 Dockerize

> 原文：<https://towardsdatascience.com/how-to-dockerize-an-existing-sinatra-application-3a6943d7a428?source=collection_archive---------17----------------------->

## 轻松运行 Sinatra 应用程序

![](img/4ccc90bb484534e5871b3758a06b4850.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

W 什么是[辛纳特拉](http://sinatrarb.com/)？它是一种使用 Ruby 创建基于 web 的应用程序的特定领域语言。我们可以用 Sinatra 更快更省力地创建一个 web 应用程序。

Dockerized 应用程序通过最大限度地减少环境管理等任务来提高开发速度，从而节省时间和精力。通过这篇文章，您将了解如何对现有的 Sinatra 应用程序进行 Dockerize。

# 设置

首先，我们需要安装 [Docker](https://docs.docker.com/engine/install/) 并从 [GitHub](https://github.com/lifeparticle/Sinatra-Docker) 下载一个 git 库。对于这个设置，我使用的是 macOS。

现在，我们将创建一个包含 Ruby 的 Docker 映像。让我们来分解一下 [**Dockerfile**](https://github.com/lifeparticle/Sinatra-Docker/blob/main/Dockerfile) 文件的各个成分。

```
FROM ruby:3.0.0

ADD . /Sinatra-Docker
WORKDIR /Sinatra-Docker
RUN bundle install

EXPOSE 4567

CMD ["/bin/bash"]
```

`From`指令用于定义父图像。这里我们使用的是 Docker Hub 上预建的 Ruby 的官方图片。通过使用`ADD`指令，我们可以将当前文件夹中的所有内容添加到图像中名为 **Sinatra-Docker** 的目录中。`WORKDIR`指令会将工作目录设置为 **Sinatra-Docker** 。我们再次使用`RUN`指令来安装我们的 ruby gems。之后，我们使用`EXPOSE`指令为我们的容器公开端口 4567。默认情况下，Sinatra 在端口 4567 上运行。最后，`CMD`指令用于在容器启动时运行命令，这里我们打开 bash shell。

现在，我们将创建一个 Docker 合成文件，使用我们刚刚创建的 Docker 映像运行 Docker 容器。我们来分解一下[**docker-compose . yml**](https://github.com/lifeparticle/Sinatra-Docker/blob/main/docker-compose.yml)文件的各个成分。

```
version: "3.8"
services:
  app:
    build: .
    command: bundle exec rackup --host 0.0.0.0 -p 4567
    ports:
      - "4567:4567"
    volumes:
      - .:/Sinatra-Docker
```

在 **docker-compose.yml** 文件中，我们有 version、services、app、build、command、ports 和 volumes 标签。**版本**标签用于定义合成文件格式。 **services** 标签用于定义我们希望应用程序使用的服务。在这种情况下，我们只有一个名为 **app** 的服务。**构建**用于使用我们之前创建的 Docker 文件构建我们的 Docker 映像。**命令**标签用于运行我们的应用程序。我们使用“0.0.0.0 ”,这样服务器将监听所有可用的 IP 地址。**端口**标签用于定义主机和容器端口。它将主机上的端口 4567 映射到容器上的端口 4567。最后，**卷**标签用于将文件夹从主机挂载到容器。

现在从 **docker-compose.yml** 文件所在的目录运行`docker compose up`命令。此命令将启动并运行整个应用程序。

```
docker compose up
```

现在，您可以通过访问 URL[http://localhost:4567/](http://localhost:4567/)通过您最喜欢的网络浏览器访问 Sinatra 应用程序。

现在，让我们分解我们的 Sinatra 应用程序。我们可以使用一个`config.ru`文件来服务一个模块化的应用程序。你可以在这里阅读更多。

```
require './main'
run MyApp
```

在开发模式下，`sinatra/reloader`会在代码改变时自动重新加载 Ruby 文件。未设置环境变量时，默认为`APP_ENV`环境变量(`ENV['APP_ENV']`)的值，未设置`APP_ENV`环境变量时，默认为`:development`。这里可以阅读更多[。最后，对于我们的应用程序，我们只有一个端点。](http://sinatrarb.com/configuration.html)

```
require 'sinatra/base'
require "sinatra/reloader"
class MyApp < Sinatra::Base
configure :development do
  register Sinatra::Reloader
 end
get '/' do
  'Hello world!'
 end
end
```

现在您知道了如何在 Docker 容器中运行 Sinatra 应用程序。希望这能帮助你入门 Sinatra 和 Docker。编码快乐！

# 相关职位

</how-to-dockerize-an-existing-flask-application-115408463e1c>  </how-to-mount-a-directory-inside-a-docker-container-4cee379c298b>  </how-to-run-a-python-script-using-a-docker-container-ea248e618e32> 