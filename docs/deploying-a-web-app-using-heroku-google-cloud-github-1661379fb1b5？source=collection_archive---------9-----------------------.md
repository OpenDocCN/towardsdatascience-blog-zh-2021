# 使用 Heroku、Google Cloud 和 GitHub 部署 Web 应用程序

> 原文：<https://towardsdatascience.com/deploying-a-web-app-using-heroku-google-cloud-github-1661379fb1b5?source=collection_archive---------9----------------------->

## 允许用户从 Google Cloud 交互式查询数据，而不暴露您的凭据。

我最近玩了一下应用程序部署，做了一个关于芝加哥地区犯罪的应用程序。点击这里查看！

顾名思义，我希望将源代码保留在 GitHub 上，使用 Heroku 进行部署，因为它是免费的，并允许用户从芝加哥警察局实时查询数据，以减少存储成本和维护。安全地做到这一点比我预期的要复杂得多，所以我认为这篇文章可能对其他人有所帮助。

![](img/e0fad7955c73646e23fab0dd7055695d.png)

由[费伦茨·阿尔马西](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 为什么是这些工具？

GitHub :我想把我的代码都放在 GitHub 上。出于几个原因，我不想在多个位置存储不同的项目。第一，很乱。我不想为了维护我的项目而在平台之间切换。第二，我记性很差。如果我没有东西放在一个地方，很可能我会忘记一些曾经存在过的东西。

**Google Cloud:** 那是我获得[数据](https://cloud.google.com/bigquery/public-data)的地方。要构建一个几乎不需要维护并保持最新的应用程序，我知道我需要一些东西，允许我在需要时从那里查询数据。还有，数据是海量的！光是加载就够给熊猫添麻烦了，所以把数据存储在 GitHub 上不太实际。

Heroku:我想我本可以继续使用谷歌云，使用他们的应用引擎。也许吧。我还没有深入调查过，所以不能确定。它要求我放下一张信用卡，所以我停下来。我还没有富到让公司拿我的信用卡做人质的地步。此外，我有使用 GitHub & Heroku 部署应用程序的经验。老实说，对我来说，Heroku 的界面比谷歌云服务更友好。

# 那么挑战是什么？

**安全问题:**要使用 BigQuery(数据在 Google Cloud 上)，我必须在每次想要查询数据时向应用程序提供我的凭证。有一个简单明了的计划，就是把所有东西都发布到 GitHub 上，然后让 Heroku 接手。

**试验 1:** 上传凭证到 GitHub 上的私有库，在主项目的库中创建子模块，引用私有库。听起来可行，对吧？[没有](https://devcenter.heroku.com/articles/github-integration)！Heroku 不加载 GitHub 的子模块。我希望我在尝试之前能看到这个…

**试炼 2** : [GitHub 秘笈](https://docs.github.com/en/actions/reference/encrypted-secrets)！我认为将我的凭证复制粘贴为存储库秘密是可行的。然后，当 Heroku 从 GitHub 部署时，我可以使用 [GitHub Actions](https://github.com/features/actions) 将秘密作为一个变量进行广播，我可以使用`os`从 Python 脚本中获得这个变量。老实说，我仍然不知道这是否可行。我写了一个‘动作’剧本(你是这么说的吗？)似乎通过了 GitHub 的 sniff 测试。但是在我的 Python 脚本中，我根本无法访问这个变量。如果你知道，请告诉我。

**试验三:**然后我想到也许我可以在 Heroku 这边做一些类似的事情？也许我可以直接告诉 Heroku 我的资历。而我[找到了](https://devdojo.com/bryanborge/adding-google-cloud-credentials-to-heroku)我的解决方案！

# 第三次是一种魅力

因此，步骤是这样的(我不会经历完全复制我的项目的所有步骤，只是那些我认为对于实现使用来自 Google Cloud 的数据从 GitHub 部署 Heroku 应用程序的类似目标是必要的):

1.  创建一个 Heroku 帐户，然后在您的控制面板中，单击“新建”创建一个新应用程序。我选择了 Python。
2.  按照这些[步骤](https://devcenter.heroku.com/articles/github-integration)来集成 GitHub 和 Heroku。我不建议允许自动部署，除非你确定一切正常。当 Heroku 给你的收件箱发邮件说‘这没用!’时，你会感到焦虑。！!'当它每 5 分钟尝试部署你的应用时。
3.  在 GitHub repo 中创建一个 Procfile、setup.sh(由 GitHub[复制并粘贴)和 requirements.txt(包含一个 python 包的列表以使您的应用程序工作)文件。](https://github.com/vo-h/Chicago-Crime)
4.  按照这些[步骤](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-client-libraries)为 Google BigQuery 设置一个 Python 客户端。不过，你不需要使用虚拟环境。我用康达。我认为 google-cloud-bigquery 确实与一些 python 包冲突，所以我必须为这个项目创建一个新的 conda 环境。最后，您应该得到一个包含您的凭证的. json 文件。还有，第一个 1TB 是每月免费的，所以我没有设置计费账户。
5.  按照这些[步骤](https://devdojo.com/bryanborge/adding-google-cloud-credentials-to-heroku)让 Heroku 知道你的证书。

# streamlit——网络的上帝——开发无能

我唯一真正懂的语言是 Python。过去，我曾尝试过 MATLAB、Java 和 R，主要是为了协作项目或课程。当然，如果情况需要，我可以学习新的语言。但如果是为了个人项目，我会尽量把自己对 Python 的理解推得更远。我只是在走阻力最小的路。

想象一下，当我发现我不必学习 Javascript 或 CSS 之类的东西时，我有多轻松。按照以下步骤安装 [streamlit](https://docs.streamlit.io/en/stable/) 。我会用`conda install -c conda-forge streamlit`来代替。点击此[链接](https://docs.streamlit.io/en/stable/api.html)查看他们的小工具选项。我没有足够的经验来判断这些选项是否足以满足你的所有需求，但它们肯定足以满足我的需求。

# 我如何构建应用程序的精简演练

首先，我给这个应用起了一个时髦的名字，吸引眼球，比如

```
import streamlit as st
st.title("Chicago Crime")
```

那里！然后，我将所有东西包装在一个`if`语句中，以确保代码只在我拥有 Google Cloud 凭证的情况下运行。

然后，我想要一个侧边栏，让用户指定他们的偏好。

当你打开我的网络应用程序时，我想这些功能显而易见。然后，根据`question`变量返回的内容，执行一条`if`语句。

我认为 streamlit 的可爱之处在于，我不必担心将所有东西都包装在一个不断获取用户输入的循环中。例如，如果用户首先选择“普遍犯罪”，则相应的 if 语句被激活。一个普通的 Python 程序会在一次输入后结束。但是有了 streamlit，用户可以随心所欲地多次输入他们的选项，脚本就可以“重新运行”,而无需我为这个过程显式编码。

为了可视化数据，我使用了 plotly。这个过程就像一个人通常会做的那样，但是为了形象化，像这样传递它来简化它

```
st.plotly_chart(fig1)
```

# 总结

本文讨论了构建一个从 Google Cloud 查询数据的 web 应用程序并使用 Heroku & GitHub 部署它的必要步骤。在 Heroku 上部署应用程序肯定有更快更简单的方法。事实上，我想要 GitHub 上的所有东西，这让整个过程变得复杂。

由此产生的应用程序也存在延迟问题，因为它是从大型数据集实时查询的。我还注意到应用程序启动需要一点时间，这让我想知道是否有比 Heroku 更好的平台。

# 保持联系

我喜欢写关于数据科学和科学的文章。如果你喜欢这篇文章，请在 [Medium](https://medium.com/@h-vo) 上关注我，加入我的[电子邮件列表](https://medium.com/subscribe/@h-vo)，或者[成为 Medium 会员](https://medium.com/@h-vo/membership)(如果你使用这个链接，我将收取你大约 50%的会员费)，如果你还没有的话。下一篇帖子再见！😄