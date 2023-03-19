# 在 Python 中本地存储和读取凭证的 3 种方法

> 原文：<https://towardsdatascience.com/3-ways-to-store-and-read-credentials-locally-in-python-5195fc1881b6?source=collection_archive---------24----------------------->

## 秘密被称为秘密是有原因的。我们不希望其他人知道它们，无论是在现实生活中还是在 Python 中

![](img/576e0697c1637d26faec93c49ac7ef0d.png)

作者图片

> *原帖*[*【realpythonproject.com】*](https://www.realpythonproject.com/3-ways-to-store-and-read-credentials-locally-in-python/)
> 
> *在*[*LinkedIn*](https://www.linkedin.com/in/rahulbanerjee2699/)*，* [*Twitter*](https://twitter.com/rahulbanerjee99) 上与我联系

我们将讨论使用 Python 存储和读取凭证的 3 种不同方式。

*   将它们存储为系统变量
*   将它们作为变量存储在虚拟环境中
*   将它们存储在. env 文件中
*   最便捷的方式

吉姆·梅德洛克的这篇文章对环境变量、它们的用例以及如何使用它们进行了深入的解释。

<https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa>  

# 将它们存储为全局环境变量

如果凭据存储为全局环境变量，则在您 PC 上的任何环境中运行的任何脚本都可以访问它们。

要创建一个全局环境变量，请在您的终端中运行

```
export varName=varValue
```

确保“=”之间没有空格。如果您得到一个错误“zsh: Bad Assignment”，这可能是由于“=”之间的空格引起的。

让我们创建几个全局环境变量

```
export globalSecretUser=global_rahul1999
export globalSecretKey = global_xy76hUihk
```

在 Windows 中，您可能必须使用“设置”而不是“导出”

上面的脚本需要在终端中运行。现在，让我们尝试访问我们之前创建的变量。

```
import os
print(os.environ) 
# This will print a dictionary with 
# all the global environment variables
```

我们可以使用字典访问方法来读取环境变量，即使用。get()或[ ]

```
print(os.environ.get('globalSecretUser'))
print(os.environ.get('globalSecretKey'))'''
global_rahul1999
global_xy76hUihk
'''
```

要更新环境变量，只需再次运行 export 语句

```
export globalSecretUser=Updated_rahul1999
```

让我们再次尝试访问 Python 中的变量

```
import os print(os.environ.get('globalSecretUser'))'''
Updated_rahul1999
'''
```

要删除环境变量，我们使用 unset 关键字

```
unset globalSecretUser
```

如果您尝试在 Python 中访问该变量，根据您用来访问字典中的值的方法，您将要么得到一个 KeyError，要么没有。

# 将它们作为变量存储在虚拟环境中

这些环境变量只能由在创建变量的虚拟环境中运行的脚本访问。我们可以将它们视为“局部”环境变量。

首先，我们必须创建一个虚拟环境

在 Mac 上

```
python3 -m venv venv
```

在 Windows 上

```
python -m venv venv
```

这将在您的当前目录中创建一个虚拟环境。bin 文件夹中应该有一个名为“activate”的文件。如果您使用的是 Windows，它应该在 Scripts 文件夹中。每当我们激活虚拟环境时，这个脚本就会运行。

我们将在这个脚本中添加变量。因此，每次我们激活虚拟环境时，都会创建环境变量。打开文件。

在创建变量之前，我们需要添加一些代码，以确保一旦虚拟环境被停用，这些变量就不存在了。

我们将使用 unset 关键字，转到 activate 脚本中的函数 deactivate()并在函数的开头取消设置变量

```
deactivate () {
    unset localSecretUser
    unset localSecretKey
    # Some Code
}
```

现在，我们将使用 export 关键字来定义变量。这段代码应该位于激活脚本末尾的停用函数之外

```
deactivate(){
     # Some Code
}
# Some More Code
export localSecretUser=local_secret_rahul199
export localSecretKey=local_secret_xyudJIk12AA
```

在 Windows 中，您可能必须使用“设置”而不是“导出”

现在我们需要激活我们的虚拟环境。如果您已经激活了您的虚拟环境，您可以使用 deactivate 关键字来停用您的虚拟环境

```
deactivate
```

在终端中键入以上内容。

现在让我们激活我们的虚拟环境。
在苹果电脑上

```
source venv/bin/activate
```

在 Windows 上

```
venv/Scripts/activate
```

您可以在终端中使用以下命令列出当前环境的变量

```
printenv
```

现在让我们尝试访问我们刚刚创建的变量。这类似于我们访问全局环境变量的方式

```
import os print(os.environ.get('localSecretUser'))
print(os.environ.get('localSecretKey'))'''
local_secret_rahul199
local_secret_xyudJIk12AA
'''
```

现在，让我们停用虚拟环境，并尝试再次运行 python 脚本

```
deactivate
python3 main.py
```

它应该不返回任何值。

要了解更多关于 Python 中虚拟环境的知识，请查看达科塔·莉莉艾的这篇文章

<https://medium.com/@dakota.lillie/an-introduction-to-virtual-environments-in-python-ce16cda92853>  

# 将它们存储在. env 文件中

在您的根文件夹(包含您的虚拟环境的文件夹)中，创建一个名为“. env”的文件。在该文件中添加变量

```
#inside file named .env
secretUser = "secret_user_rahul1999"
secretKey = "secret_key_adfdsaUj12"
```

我们需要安装一个 python 库来读取变量

```
pip3 install python-dotenv
```

让我们来阅读 Python 中的变量

```
from dotenv import load_dotenv
import os load_dotenv()
print(os.environ.get('secretUser'))
print(os.environ.get('secretKey'))'''
secret_user_rahul1999
secret_key_adfdsaUj12
'''
```

不要忘记添加。的. env 文件。gitignore 文件。这将确保您的凭证不会被推送到您的 git 存储库。

```
# Inside .gitignore.env
```

# 最方便的方法

在我看来，最方便的方法是将它们存储在. env 文件中。

*   它们位于您项目的本地
*   您不需要担心“取消设置”它们
*   如果您将它们添加到您的。gitignore 文件，它对外界是安全的
*   如果您将脚本部署为无服务器函数，您将需要使用“load_dotenve()”来读取 Azure/AWS 环境的配置文件中存储的环境变量。

虽然它有很多优点，但是一个缺点是当我们在。与全局环境变量同名的 env 文件。在 Python 中访问变量将返回全局值，因为我们没有使用“unset”来取消变量值的设置，也没有使用“export”来为变量赋值。

让我知道你是否喜欢任何其他方法，或者是否认为其中一个比另一个更安全。