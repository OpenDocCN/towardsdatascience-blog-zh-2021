# 使用 Azure 函数部署您的机器学习 Python 代码

> 原文：<https://towardsdatascience.com/deploy-python-codes-with-azure-functions-45a9b4485760?source=collection_archive---------23----------------------->

![](img/edfc2efeeaf57a5a48132677e3cdae69.png)

由[罗曼五世](https://unsplash.com/@lebrvn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Azure 函数是代码触发的事件，由于它们的无服务器功能，伸缩性很好。触发机制多种多样——基于时间的(CRON 函数)、http、存储触发器(blob、队列)等。函数也是独一无二的，因为它们可以用多种语言编写，如 C#、Python、Javascript 等。在本文中，我将演示一个简单的 ML python 脚本，它是使用 VS 代码通过 Azure 函数触发的。你可以在这里查看我的 git 回购。

**要求**:

*   加载 Azure 扩展的 VS 代码
*   Azure 订阅(提供免费点数)
*   Azure 函数 CLI 已安装。请关注[https://docs . Microsoft . com/en-us/azure/azure-functions/functions-run-local？tabs = windows % 2c csharp % 2c bash](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=windows%2Ccsharp%2Cbash#install-the-azure-functions-core-tools)
*   一个腌 ML 模型。在这里，我选择了一个简单的模型，它使用回归算法来预测一块房地产的价格，基于的因素包括建筑的年龄、离城市的距离、周围商店的数量、纬度和经度。关注的焦点是模型在 Azure 函数中的实现，而不是模型本身的准确性。

将 python 代码和 pkl 文件放在单独的文件夹中。建议使用虚拟环境来安装必要的文件。冻结同一文件夹中的需求文件。

启动 VS 代码。从扩展中启动功能应用程序，选择 http trigger 作为触发器，选择 Anonymous 作为触发器类型。启动存储 python 代码和 pkl 文件的工作空间和文件夹——自动创建需求文件。进一步添加了运行代码所需的必要文件，包括运行 pkl 文件所需的包。启动该函数会自动创建一个 init.py 和一个 function.json 文件，其他助手除外。

让我们更深入地了解一下 init.py 文件实际上在做什么——在这个基本代码中，init.py 文件是包含函数 app 的主要代码的脚本，它触发 http 并具有 post 和 get 请求。在对模型进行预测之后，json 转储的结果作为 numpy 数组返回。function.json 文件只是一个指向 init.py 文件的绑定器(会根据使用的触发器而改变)。请注意使用的匿名认证级别。

根据您的要求，您可以更改这些设置。例如，如果您基于 blob 的文件更改进行访问，请确保您已经准备好了安全的 SAS 密钥。

差不多就是这样，真的。现在是时候测试自己的功能 app 了。在命令提示符下，使用以下命令初始化该函数:

```
func init
func host start
```

您应该能够看到一些 ascii 字符。您将被定向到本地的一个链接。顺其自然吧。这说明功能 app 其实一直到现在都在无瑕疵的工作。

打开 git bash 并运行以下命令:

```
curl -w '\n' '[http://localhost:7071/api/real_estate?Age=25&Dist=251&Num_stores=5&Lat=24.9756&Long=121.5521](http://localhost:7071/api/real_estate?Age=25&Dist=251&Num_stores=5&Lat=24.9756&Long=121.5521)'
```

您当然可以更改年龄、距离等的值。在运行这个 bash 命令时，python 终端中应该显示成功，以及 bash 终端中的预测值。

通过 vscode 将其部署到 Azure 是最简单的方法。这非常简单——按照 Functions 扩展的提示使用部署命令。你的应用应该准备好了。使用部署的 weblink，并将相同的部分添加到 http link(？age = 25 & Dist = 251 & Num _ stores = 5 & Lat = 24.9756 & Long = 121.5521)。

你应该在你的网页链接中看到同样的回复。

好了，差不多了！！！—现在，这是一个使用 vscode 和 azure 函数触发 python 脚本的端到端管道。Azure 函数非常通用，可以与 datafactory 放在一个管道中，用于更复杂的工作。