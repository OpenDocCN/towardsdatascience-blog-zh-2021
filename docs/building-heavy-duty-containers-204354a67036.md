# 建造重型集装箱

> 原文：<https://towardsdatascience.com/building-heavy-duty-containers-204354a67036?source=collection_archive---------31----------------------->

## 重新启动标志、初始化和管理进程

![](img/55ca6caf13cfb753a82a0a7e632a030a.png)

由[帕特·惠伦](https://unsplash.com/@patwhelen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/heavy-duty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

有些情况下，软件在极少数情况下会失败，这种情况本质上是暂时的。虽然知道这些情况何时出现很重要，但通常至少尽快恢复服务也同样重要。

当容器中的所有进程都已退出时，该容器将进入 exited 状态。Docker 容器可以处于以下四种状态之一:

*   运转
*   暂停
*   重新启动
*   Exited(如果容器从未启动过，也可以使用)

从临时故障中恢复的一个基本策略是，当一个进程退出或失败时，它将自动重新启动。Docker 提供了一些监控和重启容器的选项。

## 自动重启容器

Docker 通过重启策略提供了对这一特性的支持。在创建容器时使用`—- restart`标志，您可以告诉 Docker 执行以下任一操作:

*   从不重启(默认)
*   检测到故障时尝试重启
*   当检测到故障时，尝试重新启动一段预定时间
*   无论情况如何，都要重启容器

Docker 并不总是试图立即重启容器。如果是这样的话，它将引发更多的问题，而不是解决问题。想象一个容器，它什么也不做，只是打印时间，然后退出。如果容器被配置为总是重启，而 Docker 总是立即重启，那么系统不会执行任何操作，只会重启容器。相反，Docker 使用指数补偿策略来计时重启尝试。

退避策略决定了连续重启尝试之间应该经过多长时间。指数退避策略将使等待每次连续尝试的时间加倍。例如，如果容器第一次需要重新启动，Docker 等待 1 秒，然后在第二次尝试时，它将等待 2 秒，第三次尝试将等待 4 秒，第四次将等待 8 秒，以此类推。初始等待时间较短的指数退避策略是一种常见的服务恢复技术。您可以看到 Docker 本身采用了这种策略，构建了一个总是重启并只打印时间的容器

```
docker run -d —-name backoff-sample --restart always busybox date
```

几秒钟后，查看日志，观察它的后退和重启

```
docker logs -f backoff-sample
```

您可能不希望直接采用该特性的唯一原因是容器在回退期间没有运行。等待重新启动的容器处于重新启动状态。为了进行演示，请尝试在回退示例容器中运行另一个进程

```
docker exec backoff-sample echo testing
```

运行该命令应该会导致一条错误消息

```
Error response from daemon: Container 4affc02f445dd426c0b7daf79efc31f83b1e6e3c7397323bd235a8cd80453bb1 is restarting, wait until the container is running
```

这意味着您不能执行任何需要容器运行的操作，例如在容器中执行其他命令。如果您需要在损坏的容器中运行诊断，这可能是一个问题。更完整的策略是使用运行 init 或 supervisor 进程的容器。

## 管理程序和启动程序

管理程序或初始化程序是用于启动和保持其他程序状态的程序。在 Linux 系统上，PID#1 是一个初始化过程。它启动所有其他系统进程，并在它们意外失败时重新启动它们。使用类似的模式来启动和管理容器中的流程是一种常见的做法。

如果目标进程(比如 web 服务器)失败并重启，在容器内部使用一个监管器将保持容器运行。容器中可以使用几个程序。最受欢迎的包括 init、systemd、runit、upstart 和 supervisord。

DockerHub 中有许多 Docker 映像，它们在一个容器中生成一个完整的 LAMP (Linux、Apache、MySQL PHP)堆栈。以这种方式创建的容器使用 supervisord 来确保所有相关的进程保持运行。启动一个示例容器

```
docker run -d -p 80:80 --name lamp-test mattrayner/lamp
```

您可以通过使用`docker top`命令来查看这个容器中正在运行的进程

```
docker top lamp-testPID                 USER                TIME                COMMAND5833                root                0:00                {supervisord} /usr/bin/python3 /usr/local/bin/supervisord -n6462                root                0:00                apache2 -D FOREGROUND6463                root                0:00                {pidproxy} /usr/bin/python3 /usr/local/bin/pidproxy /var/run/mysqld/mysqld.pid /usr/bin/mysqld_safe6464                1000                0:00                apache2 -D FOREGROUND6465                1000                0:00                apache2 -D FOREGROUND6466                1000                0:00                apache2 -D FOREGROUND6467                1000                0:00                apache2 -D FOREGROUND6468                1000                0:00                apache2 -D FOREGROUND6469                root                0:00                {mysqld_safe} /bin/sh /usr/bin/mysqld_safe6874                1000                0:00                /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib/mysql/plugin --user=www-data --log-error=/var/log/mysql/error.log --pid-file=/var/run/mysqld/mysqld.pid --socket=/var/run/mysqld/mysqld.sock --port=3306 --log-syslog=1 --log-syslog-facility=daemon --log-syslog-tag=
```

top 子命令将显示容器中每个进程的主机 PID。你会在运行程序列表中看到 supervisord，mysql，apache。现在容器正在运行，您可以通过手动停止容器中的一个进程来测试 supervisord 重启功能。

首先，让我们得到每个进程的 PID

```
docker exec lamp-test psPID TTY          TIME CMD1 ?        00:00:00 supervisord503 ?        00:00:00 apache2504 ?        00:00:00 pidproxy510 ?        00:00:00 mysqld_safe943 ?        00:00:00 ps
```

让我们终止`apache2`进程

```
docker exec lamp-test kill 503
```

运行这个命令将运行 lamp-test 容器中的 Linux kill 程序，并告诉 apache2 进程关闭。当 apache2 停止时，supervisord 进程将记录该事件并重新启动该进程。容器日志将清楚地显示这些事件:

```
docker logs lamp-testUpdating for PHP 7.4---------2021-02-16 12:58:42,878 INFO supervisord started with pid 1**2021-02-16 12:58:43,882 INFO spawned: 'apache2' with pid 503**2021-02-16 12:58:43,885 INFO spawned: 'mysqld' with pid 5042021-02-16 12:58:45,332 INFO success: apache2 entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)2021-02-16 12:58:45,332 INFO success: mysqld entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)**2021-02-16 13:08:42,334 INFO exited: apache2 (exit status 0; expected)****2021-02-16 13:08:43,345 INFO spawned: 'apache2' with pid 956****2021-02-16 13:08:44,385 INFO success: apache2 entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)**
```

使用 init 或 supervisor 程序的一个常见替代方法是使用启动脚本，该脚本至少检查成功启动所包含软件的先决条件。这些有时被用作容器的默认命令。Docker 容器在执行命令之前运行一个叫做*入口点*的东西。入口点是放置验证容器先决条件的代码的理想位置。

[启动脚本](https://docs.docker.com/config/containers/multi-service_container/)是构建持久容器的重要组成部分，并且总是可以与 Docker 重启策略相结合，以利用各自的优势。

## 竣工清理

易于清理是使用容器和 Docker 的最重要的原因之一。容器提供的隔离简化了停止进程和删除文件所需的所有步骤。使用 Docker，整个清理过程被简化为几个简单命令中的一个。在任何清理任务中，您必须首先确定要停止和/或删除哪个容器。

让我们先列出所有的集装箱

```
docker ps -a
```

由于不再使用为本文中的示例创建的容器，您应该能够安全地停止和删除所有列出的容器。如果您为您的活动创建了一个容器，请确保注意要清理的容器。

所有容器都使用硬盘空间来存储日志、容器元数据和已写入容器文件系统的文件。所有容器还消耗全局名称空间中的资源，例如容器名称和主机端口映射。在大多数情况下，不再使用的容器应该被删除。

让我们删除`lamp-test`容器:

```
docker rm -f lamp-test
```

由于这个容器正在运行，如果我们试图运行没有`-f`标志的前一个命令，Docker 将抛出一个错误。码头集装箱需要停止之前，试图删除它们。您可以通过运行`docker stop`命令来实现，或者不运行两个命令，而是在 remove 命令中添加 *force* 标志。