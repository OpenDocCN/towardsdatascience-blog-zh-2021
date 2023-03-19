# 在服务器上全面部署 Jupyter 笔记本电脑，包括 TLS/SSL 和加密功能

> 原文：<https://towardsdatascience.com/full-deployment-of-jupyter-notebooks-on-a-server-including-tls-ssl-with-lets-encrypt-b30cc758881e?source=collection_archive---------8----------------------->

## 在服务器或云上完成 Jupyter 笔记本电脑或 JupyterLab 的设置和安装

![](img/f6685697bf7c064082714f646f361d71.png)

图片来自[维基共享资源](https://commons.wikimedia.org/wiki/File:Leitstand_2.jpg)

Jupyter Notebook 是一个强大的工具，但是你如何在服务器上使用它呢？在本教程中，你将看到如何在服务器上设置 Jupyter notebook，如[数字海洋](https://m.do.co/c/cd7e4dd5ee1f)、[海兹纳](https://hetzner.cloud/?ref=FkpdQcqbGXhP)、 [AWS](https://aws.amazon.com/) 或其他大多数可用的主机提供商。此外，您将看到如何通过 SSH 隧道或 TLS/SSL 使用 Jupyter notebooks 和[让我们加密](https://letsencrypt.org/)。本文原载[此处](https://janakiev.com/blog/jupyter-notebook-server/)。

Jupyter 是一个开源的 web 应用程序，支持从浏览器进行交互式计算。您可以创建包含实时代码的文档、带有 Markdown 的文档、等式、可视化甚至小部件和其他有趣的功能。Jupyter 来自支持的三种核心语言:Julia、Python 和 r。Jupyter 连接到使用特定语言的内核，最常见的是 IPython 内核。它支持各种各样的内核，你应该能找到你需要的大多数语言。本教程写于 [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) ，Jupyter 笔记本的下一步发展:

![](img/21124ee269b337d47ede0ed7c9a5674f.png)

在本教程中，我们将使用 Ubuntu 18.04/20.04 服务器，但大多数步骤应该与 Debian 9/10 发行版相当相似。我们将首先创建 SSH 密钥，在服务器上添加一个新用户，并使用 [Anaconda](https://www.anaconda.com/) 安装 Python 和 Jupyter。接下来，您将设置 Jupyter 在服务器上运行。最后，你可以选择在 SSH 隧道上运行 Jupyter 笔记本，或者在 SSL 上运行[让我们加密](https://letsencrypt.org/)。

# 创建 SSH 密钥

我们从一个全新的服务器开始，为了在访问您的服务器时增加更多的安全性，您应该考虑使用 SSH 密钥对。这些密钥对由上传到服务器的公钥和保存在机器上的私钥组成。一些主机提供商要求您在创建服务器实例之前上传公钥。要创建新的 SSH 密钥，您可以使用 [ssh-keygen](https://www.ssh.com/ssh/keygen/) 工具。要创建密钥对，您只需键入以下命令:

```
ssh-keygen
```

如果您愿意，这将提示您添加文件路径和密码。还有其他选项参数可供选择，如公钥算法或文件名。你可以在这里找到一个非常好的教程[关于如何用 ssh-keygen 为 Linux 或 macOS 创建一个新的 SSH 密钥。如果您使用的是 Windows，您可以使用 PuTTYgen 创建 SSH-keys，如这里的](https://www.ssh.com/ssh/keygen/)[所述](https://www.ssh.com/ssh/putty/windows/puttygen)。如果您的主机提供商在创建前不需要公钥，您可以使用 [ssh-copy-id](https://www.ssh.com/ssh/copy-id) 工具复制公钥:

```
ssh-copy-id -i ~/.ssh/jupyter-cloud-key user@host
```

最后，您可以通过以下方式连接到您的服务器:

```
ssh -i ~/.ssh/id_rsa root@host
```

其中`~/.ssh/id_rsa`是您的 ssh 私有密钥的路径，而`host`是您的服务器实例的主机地址或 IP 地址。

# 添加新用户

在一些服务器中，您是作为根用户开始的。直接使用根用户被认为是一种不好的做法，因为它有很多特权，如果某些命令是意外执行的，这些特权可能是破坏性的。如果您已经有用户，可以跳过这一部分。请注意，您可以将以下所有命令中的`cloud-user`替换为您想要的用户名。首先创建一个新用户:

```
adduser cloud-user
```

这个命令会问你几个问题，包括密码。接下来，您需要向该用户授予管理权限。您可以通过键入以下命令来完成此操作:

```
usermod -aG sudo cloud-user
```

现在你可以用`su cloud-user`切换到新用户，或者用`ssh cloud-user@host`连接到你的服务器。或者，您可以将 root 用户的 SSH 密钥添加到新用户中，以提高安全性。否则，您可以跳到下一节如何安装 Anaconda。现在，如果您有根用户的现有 SSH 密钥，您可以将公钥从根主文件夹复制到用户主文件夹，如下所示:

```
mkdir /home/cloud-user/.ssh
cp /root/.ssh/authorized_keys /home/cloud-user/.ssh/
```

接下来，您需要更改文件夹和公钥的权限:

```
cd /home/user/
chmod 700 .ssh/
chmod 600 .ssh/authorized_keys
```

如果您正在为您的用户使用密码，您需要更新`/etc/ssh/sshd_config`:

```
nano /etc/ssh/sshd_config
```

在这里，您希望找到行`PasswordAuthentication no`，并将`no`更改为`yes`，以允许密码验证。最后，您希望通过键入`service ssh restart`来重启 SSH 服务。对于其他发行版，请看一下这个指南，在那里你也会看到如何设置防火墙。

# 安装 Anaconda

Anaconda 是 Python(和 R)的开源发行版，用于科学计算，包括包管理和部署。有了它，你就有了你需要的大部分工具，包括 Jupyter。要安装 Anaconda，请转到 linux 的[下载](https://www.anaconda.com/download/#linux)，并复制最新 Python 3.x 版本的 Linux 安装程序链接。然后你可以用`wget`下载安装程序:

```
wget [https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh](https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh)
```

接下来，您可以使用`bash`安装 Anaconda，如下所示:

```
bash Anaconda3-5.2.0-Linux-x86_64.sh
```

在安装过程中，当安装过程中出现以下提示时，键入`yes`非常重要:

```
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/user/.bashrc ? [yes|no]
```

安装完成后，您希望通过 Anaconda 用以下命令初始化`conda`命令行工具和包管理器:

```
source .bashrc 
conda update conda
```

这两个命令在您的服务器上设置 Anaconda。如果您已经用 sudo 运行了 Anaconda bash 文件，您将得到一个`Permission denied`错误。你可以通过输入`sudo chown -R $$USER:$$USER /home/user/anaconda3`来解决这个[问题](https://stackoverflow.com/questions/49181799/conda-update-conda-permission-error)所示的问题。这将使用 [chown](https://en.wikipedia.org/wiki/Chown) 命令将该文件夹的所有者更改为当前用户。

# 正在启动 Jupyter 笔记本服务器

Jupyter 与 Anaconda 一起安装，但是我们需要做一些配置以便在服务器上运行它。首先，您需要为 Jupyter 笔记本创建一个密码。您可以通过用`ipython`启动 IPython shell 并生成一个密码散列来实现这一点:

```
from IPython.lib import passwd
passwd()
```

暂时保存这个结果散列，我们稍后会用到它。接下来，您想要生成一个配置文件，您可以通过键入。

```
jupyter-notebook --generate-config
```

现在用`sudo nano ~/.jupyter/jupyter_notebook_config.py`打开配置文件，将下面的代码复制到文件中，并用您之前生成的代码替换这个代码片段中的散列:

```
c = get_config()  # get the config object
# do not open a browser window by default when using notebooks
c.NotebookApp.open_browser = False
# this is the password hash that we generated earlier.
c.NotebookApp.password = 'sha1:073bb9acaa67:b367308802ab66cb1d7654b6684eafefbd61d004'
```

现在你应该设置好了。接下来，您可以决定是使用 SSH 隧道还是使用 SSL 加密并通过您自己的域名访问您的 jupyter 笔记本。

# Linux 或 MacOS 上的 SSH 隧道

您可以通过在负责端口转发的`ssh`命令中添加`-L`参数来隧道连接到您的服务器。第一个`8888`是您将在本地机器上访问的端口(如果您已经为另一个 juypter 实例使用了这个端口，那么您可以使用端口 8889 或不同的开放端口)。你可以用`localhost:8888`在你的浏览器上访问它。第二部分`localhost:8888`指定从服务器访问的跳转服务器地址。因为我们希望在服务器上本地运行笔记本，所以这也是 localhost。这意味着我们从服务器通过端口转发到我们机器上的`localhost:8888`来访问`localhost:8888`。下面是该命令的样子:

```
ssh -L 8888:localhost:8888 cloud-user@host
```

如果您的本地机器上已经运行了另一个 Jupyter 笔记本，您可以将端口更改为`8889`，这将导致命令:

```
ssh -L 8889:localhost:8888 cloud-user@host
```

现在，您可以在服务器上为您的项目创建一个笔记本文件夹，并在其中运行 Jupyter notebook:

```
mkdir notebook
cd notebook/
jupyter-notebook
```

你也可以使用 [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) 来代替，这是一个更强大的接口，它也预装了 Anaconda。你可以通过输入`jupyter-lab`而不是`juypter-notebook`来启动它。

# 让我们加密的 SSL 加密

也可以对你的 jupyter 笔记本使用 SSL 加密。这使您能够通过互联网访问您的 Jupyter 笔记本，从而方便地与您的同事分享结果。要做到这一点，您可以使用[让我们加密](https://letsencrypt.org/)，这是一个免费的[认证中心(CA)](https://en.wikipedia.org/wiki/Certificate_authority) ，它为 TLS/SSL 证书提供了一种简单的方法。这可以用他们的 [certbot](https://certbot.eff.org/) 工具全自动完成。要找到您系统的安装指南，请查看此[列表](https://certbot.eff.org/all-instructions)。对于 Ubuntu 18.04，[安装](https://certbot.eff.org/lets-encrypt/ubuntubionic-apache.html)如下所示:

```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python-certbot-apache
```

现在，您可以为您拥有的域运行 certbot:

```
sudo certbot certonly -d example.com
```

完成提示后，您应该会看到以下输出:

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2019-05-09\. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by: Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    [https://eff.org/donate-le](https://eff.org/donate-le)
```

太好了！您已经准备好了证书和密钥文件。现在，您可以使用 jupyter 笔记本配置文件中的证书和密钥文件。在此之前，您需要更改证书和密钥文件的所有者(用您自己的用户名更改`user`):

```
sudo chown user /usr/local/etc/letsencrypt/live
sudo chown user /usr/local/etc/letsencrypt/archive
```

接下来，您可以将以下代码添加到`~/.jupyter/jupyter_notebook_config.py`配置文件中:

```
# Path to the certificate 
c.NotebookApp.certfile = '/etc/letsencrypt/live/example.com/fullchain.pem' 
# Path to the certificate key we generated
c.NotebookApp.keyfile = '/etc/letsencrypt/live/example.com/privkey.pem' 
# Serve the notebooks for all IP addresses
c.NotebookApp.ip = '0.0.0.0'
```

最后，你可以通过`https://example.com:8888`安全地访问 Jupyter 笔记本。只是确保使用`https://`而不是`http://`。如果您犯了任何错误，您可以用`sudo certbot delete`或`sudo certbot delete --cert-name example.com`删除 certbot 证书。如果您使用的是防火墙，请确保端口`8888`是打开的。这里有一个关于使用[简单防火墙(UFW)](https://en.wikipedia.org/wiki/Uncomplicated_Firewall) 防火墙的很好的[指南](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)。

# 结论

您已经从头到尾学习了如何为服务器设置 Jupyter。随着您设置的每一台服务器，这项任务变得越来越容易。请务必深入研究 Linux 服务器管理的相关主题，因为一开始使用服务器可能会令人生畏。使用 Jupyter，您可以访问各种各样的内核，这些内核使您能够使用其他语言。所有可用内核的列表可以在[这里](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)找到。我希望这是有帮助的，如果你有任何进一步的问题或意见，请在下面的评论中分享。

我在[之前的教程](https://janakiev.com/til/jupyter-virtual-envs/)中提到了如何在 Jupyter 笔记本中使用虚拟环境。还有一个将 Jupyter 作为 Docker 容器运行的选项。例如，您可以使用[jupyter/data science-notebook](https://hub.docker.com/r/jupyter/datascience-notebook/)容器。你可以在本指南的[中阅读更多关于如何使用 Jupyter 和 Docker 的信息。关于进一步的安全考虑，请看一下 Jupyter 笔记本服务器](https://jupyter-docker-stacks.readthedocs.io/en/latest/index.html)中的[安全性。以下是我从中学到的更多链接，可能对你也有用:](https://jupyter-notebook.readthedocs.io/en/stable/security.html)

*   [初始服务器设置](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04)
*   [运行笔记本服务器](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html)
*   [如何为 Python 3 设置 Jupyter 笔记本](https://www.digitalocean.com/community/tutorials/how-to-set-up-jupyter-notebook-for-python-3)
*   [如何使用 Certbot 独立模式检索加密 SSL 证书](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-ubuntu-16-04)
*   [UFW 基础:通用防火墙规则和命令](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)
*   [在 Jupyter 笔记本和 Python 中使用虚拟环境](https://janakiev.com/til/jupyter-virtual-envs/)
*   [用 Jupyter 笔记本制作幻灯片](https://janakiev.com/blog/creating-slides-with-jupyter-notebook/)

# 更多参考

*   [如何在 Jupyter 和 IPython 中使用 letsencrypt 证书](https://perrohunter.com/how-to-use-letsencrypt-certificates-in-jupyter/)
*   [向 Jupyter Hub 添加 SSL 和域名](https://pythonforundergradengineers.com/add-ssl-and-domain-name-to-jupyterhub.html)