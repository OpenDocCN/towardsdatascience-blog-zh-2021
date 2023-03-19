# Docker 卷完全指南

> 原文：<https://towardsdatascience.com/the-complete-guide-to-docker-volumes-1a06051d2cce?source=collection_archive---------0----------------------->

## 了解 docker 卷的基础知识

![](img/50fb9b80cfb891713bb6f4137fa8e0d7.png)

照片由[克里斯汀·休姆](https://unsplash.com/@christinhumephoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在我们重启或移除容器后，容器生成和使用的数据不再持久。因此，我们可以使用 [Docker 卷](https://docs.docker.com/storage/volumes/)和[绑定挂载](https://docs.docker.com/storage/bind-mounts/)来管理 Docker 容器中的数据，以解决这个问题。我们可以用它来持久化容器中的数据或者在容器之间共享数据。从这篇文章中，你将学习如何在你的项目中使用 Docker 卷和绑定挂载。

# 设置

Docker 使用以下类型的卷和绑定挂载来保存数据。对于这个设置，我使用的是 macOS。

1.  匿名卷
2.  命名卷
3.  绑定安装

在这篇文章中，我们将运行一个 MySQL 服务器并执行一些命令。默认情况下，MySQL 会将其数据文件存储在容器的`/var/lib/mysql`目录中，Docker volumes 会帮助我们持久存储这些数据。

我们有三个 docker-compose.yml 文件来演示卷和绑定挂载。要启动这些文件，您需要使用以下命令。

```
docker compose up
```

一旦我们的容器开始运行，我们可以使用下面的命令在容器中创建一个表用于测试。

```
# Access the container
docker exec -it mysql_db_1 bash
# Connect to MySQL server
mysql -uroot -proot
# Run MySQL commands
USE test_db;
SHOW TABLES;
CREATE TABLE users (
       user_id int NOT NULL AUTO_INCREMENT,
       name VARCHAR(20),
       PRIMARY KEY (user_id)
);
SHOW TABLES;
```

## 1.匿名卷

如果我们运行下面的 docker-compose.yml 文件，将会创建一个匿名卷。如果我们重启我们的容器，数据将是可见的，但不是在我们移除容器之后。此外，其他容器也无法访问它。如果我们想暂时保存数据，这是很有帮助的。这些卷是在`/var/lib/docker/volume`本地主机目录中创建的。

```
version: '3.8'
services:
  db:
    image: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: test_db
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql
```

正如我们所看到的，我们不必指定主机目录。我们只需要指定容器内的目录。

如果我们从 docker-compose.yml 文件中删除 volume 指令，容器将默认创建一个匿名卷，因为它是在 MySQL [Dockerfile](https://github.com/docker-library/mysql/blob/master/8.0/Dockerfile.debian) 中指定的。因此，MySQL 映像确保了我们在不提供任何卷信息的情况下仍然可以访问数据。

```
VOLUME /var/lib/mysql
```

现在，我们有一个带有随机标识符的匿名卷。

```
docker volume ls
DRIVER    VOLUME NAME
local  4e679725b7179e63e8658bc157a1980f320948ab819f271fd5a44fe94c16bf23
```

让我们检查一下我们的码头集装箱。

```
docker inspect mysql_db_1
.
.
.
"Mounts": [
            {
                "Type": "volume",
                "Name": "4e679725b7179e63e8658bc157a1980f320948ab819f271fd5a44fe94c16bf23",
                "Source": "/var/lib/docker/volumes/4e679725b7179e63e8658bc157a1980f320948ab819f271fd5a44fe94c16bf23/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
.
.
.
```

我们可以使用下面的[命令](https://docs.docker.com/engine/reference/commandline/rm/)删除容器及其相关的匿名卷。

```
docker rm -v mysql_db_1
```

如果我们不将匿名卷和容器一起删除，它将成为悬挂卷。

```
docker rm mysql_db_1
```

我们可以使用以下命令列出并删除所有悬挂卷。

```
docker volume ls -qf dangling=true
docker volume rm $(docker volume ls -qf dangling=true)
```

## 2.命名卷

在我们重新启动或删除容器后，命名卷可以保存数据。此外，其他容器也可以访问它。这些卷创建在`/var/lib/docker/volume`本地主机目录中。

```
version: '3.8'
services:
  db:
    image: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: test_db
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
volumes:
  db_data:
```

这里，第一个字段是主机上卷的唯一名称。第二部分是容器中的路径。

此外，如果我们使用以下命令删除容器，我们将仍然拥有该卷，这与匿名卷不同。

```
docker rm -v mysql_db_1
```

## 3.绑定安装

在我们重启或移除容器后，绑定装载可以持久化数据。正如我们所看到的，命名卷和绑定装载是相同的，只是命名卷可以在特定的主机目录下找到，而绑定装载可以在任何主机目录下。

```
version: '3.8'
services:
  db:
    image: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: test_db
    ports:
      - "3306:3306"
    volumes:
      - $PWD/data:/var/lib/mysql
```

这里，我们正在装载一个主机文件夹。第一部分是主机中的路径。第二部分是容器中的路径。

# 命令

现在，让我们列出 volume 指令的所有可用命令。

```
docker volume --help

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes
```

我们可以使用这些命令来管理匿名卷和命名卷。

```
# Creat a volume
docker volume create test-vol
# test-vol

# Inspect a volume
docker inspect test-vol
# [
#     {
#         "CreatedAt": "2021-07-17T07:23:25Z",
#         "Driver": "local",
#         "Labels": {},
#         "Mountpoint": "/var/lib/docker/volumes/test-vol/_data",
#         "Name": "test-vol",
#         "Options": {},
#         "Scope": "local"
#     }
# ]

# List all volumes
docker volume create test-vol-2
docker volume ls
# DRIVER    VOLUME NAME
# local     test-vol
# local     test-vol-2

# Remove all volumes
docker volume prune
# WARNING! This will remove all local volumes not used by at least one container.
# Are you sure you want to continue? [y/N] y
# Deleted Volumes:
# test-vol
# test-vol-2

# Remove volumes
docker volume create test-vol-3
docker volume rm test-vol-3
# test-vol-3

docker volume create test-vol-4
docker volume create test-vol-5
docker volume rm test-vol-4 test-vol-5
# test-vol-4
# test-vol-5
```

我希望你对 Docker 卷和装订线有一个清晰的了解。它将帮助您为 Docker 项目持久化数据。编码快乐！

# 相关职位

[](/how-to-mount-a-directory-inside-a-docker-container-4cee379c298b) [## 如何在 Docker 容器中挂载目录

### 专注于编写代码，无需担心环境管理

towardsdatascience.com](/how-to-mount-a-directory-inside-a-docker-container-4cee379c298b) [](/the-ultimate-markdown-cheat-sheet-3d3976b31a0) [## 终极降价备忘单

### 写一篇精彩的自述

towardsdatascience.com](/the-ultimate-markdown-cheat-sheet-3d3976b31a0)