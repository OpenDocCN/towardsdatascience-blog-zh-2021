# 使用 Python 生成密码

> 原文：<https://towardsdatascience.com/highly-secure-password-generation-with-python-852da86565b9?source=collection_archive---------12----------------------->

## 在大约 5 分钟内为所有重要文件和网上交易生成安全密码

![](img/4da17811c42ef3875a96f5a96df9edca.png)

照片由[飞:D](https://unsplash.com/@flyd2069?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

安全性是当今时代最重要的问题之一，确保每台设备、设备、社交媒体账户、银行对账单或任何其他类似的重要信息的安全变得至关重要。出于高安全性的目的，我们利用密码锁定必要的数据，以便只有授权用户才能访问相关信息。

大多数人可能已经注意到，密码通常由星号(*)表示，代表字符数。大多数现代电子邮件或其他加密服务拒绝允许弱密码。你不再被允许只输入几个字母或几个数字，因为这些密码很容易被暴力破解。

我们甚至可以注意到，像 Gmail 这样的一些服务甚至为用户提供了一个选项来选择一个强密码的建议。在本文中，我们将利用 Python 编程来实现以下强密码生成任务。使用 Python 进行项目开发的多功能性和案例将使我们能够有效地实现预期的结果。

我们现在将直接进入这个项目，为个人和专业应用程序构建高度安全的密码。但是，如果您不太熟悉 Python，我建议您在继续学习之前先阅读下面的文章。

</how-to-write-code-effectively-in-python-105dc5f2d293>  

***来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*

# 生成高度安全密码的 Python 项目:

为了构建这个项目，我们将研究几个方法来执行下面的任务。在第一种方法中，我们将从头开始构建整个项目。在第二种方法中，我们将查看一个可以帮助我们更有效地完成预期任务的库。然而，了解如何从零开始构建这样的项目以在编程和最终数据科学中发展是至关重要的。让我们开始导入一些基本的库。

```
import random
import string
```

随机库对于我们从特定字符串的序列中随机选取字母、数字或特殊字符是必不可少的。为了获得生成密码所需的字符，我们将使用导入的字符串模块。键入以下命令来接收可能性字符串。

```
print(string.printable)
```

确保您删除了双引号或在它们前面加了一个反斜杠，这样您就可以在项目中使用它们。但是，您可以删除一些您不希望密码包含的不必要的字符。另外，请注意，有些网站可能对特殊字符有特殊要求。确保满足这些要求，并且没有任何不必要的序列。下一个代码块如下。

```
char_seq = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!#$%&'()*+,-./:;<=>?@[\]^_`{|}~"
print(type(char_seq))
```

在下一步中，我们将允许用户选择他们希望在各自的密码中包含的字符数。但是，我们将设置一个最小的八位密码限制，因为这通常是大多数网站对强密码的最小要求。我们还将设置一个最大 16 个字符的阈值，因为任何超过这个长度的字符都可能会很奇怪。下面是解释以下内容的代码片段。

```
print("Enter the required length of the password ranging from 8 to 16: ")
length = int(input())
```

现在我们已经允许用户输入所需的密码长度，我们可以继续构建下一个代码块，它将使我们能够计算生成的密码。我们将使用 if 和 else 语句来确保用户输入的长度要求在 8 到 16 个字符的限制范围内。我们将用一个空字符串初始化一个密码变量，这个空字符串将存储我们将生成的随机字符。我们将针对提到的长度运行 for 循环迭代。在每次迭代中，我们将使用随机库中的 choice 函数从我们的字符序列中选择一个随机字母，并将其添加到密码变量中。

通常，第一个过程应该足以创建一个由随机字符组成的强密码。然而，我们将通过给这个新生成的密码增加更多的随机性来使它稍微更安全。因此，我们将把密码从字符串格式转换成一个列表，并从随机库中运行 shuffle 操作。一旦我们打乱了随机生成的密码，我们将把列表中的字母重新组合成字符串格式，并打印出用户选择的强密码。下面提供了执行以下操作的完整代码块。

```
if length >= 8 and length <= 16:
    password = ''
    for len in range(length):
        random_char = random.choice(char_seq)
        password += random_char

    # print(password)
    list_pass = list(password)
    random.shuffle(list_pass)
    final_password = ''.join(list_pass)
    print(final_password)
```

最后，我们将通过完成 if-else 条件语句来结束我们的代码。如果范围不合适，我们将返回一个命令，告诉用户输入一个合适的范围。您可以对该项目进行一些改进，使其更有成效和效率。我建议观众多尝试一下这样的方法！

```
else:
    print("Enter a suitable range")
```

现在，我们将看看第二种方法，在这种方法中，我们可以构建同一个项目。为了完成这个任务，我们可以用一个简单的 pip 命令安装下面的库。查看安装页面，从这个下载库[获得关于这个库的更多信息。](https://pypi.org/project/random-password-generator/)

```
pip install random-password-generator
```

为了生成所需的密码，导入已安装的库并调用函数，同时将它存储在一个随机变量中。现在，您可以相应地生成所需的密码。执行所需操作的代码片段如下所示。

```
from password_generator import PasswordGeneratorpwo = PasswordGenerator()
pwo.generate()
```

有了这个库，您可以实现更多的事情，值得查看官方文档。因此，请随意参观。您可以从这个下载[库](https://pypi.org/project/random-password-generator/)查看安装页面以获得关于这个库的更多信息。

# 结论:

![](img/d920fe63dfa3c44cefed8a4051b13943.png)

照片由[杰瑞米·贝赞格](https://unsplash.com/@jeremybezanger?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

*“因为没什么好隐瞒而辩称自己不在乎隐私权，和因为无话可说而说自己不在乎言论自由没什么区别* ***。”
-* 爱德华·斯诺登**

随着安全成为最近几乎所有活动的主要关注点，应用增强的方法来保护您的数据和信息以防止您被网络钓鱼或被类似的黑客方法利用变得至关重要。生成您自己的强密码来保护私人信息是对抗和处理这些现有威胁的最佳方式之一。

在本文中，我们利用 Python 编程语言编写了一个简单的代码，该代码将解决语言如何解决强密码生成这一任务。我建议看看其他技术和方法，以获得更有成效的结果。

如果你想在我的文章发表后第一时间得到通知，请点击下面的[链接](https://bharath-k1297.medium.com/membership)订阅邮件推荐。如果你希望支持其他作者和我，请订阅下面的链接。

<https://bharath-k1297.medium.com/membership>  

我建议尝试许多这样的项目来适应 Python 编程。看看我的其他一些文章，你可能会喜欢读！

</generating-qr-codes-with-python-in-less-than-10-lines-f6e398df6c8b>  </5-best-python-projects-with-codes-that-you-can-complete-within-an-hour-fb112e15ef44>  </7-best-ui-graphics-tools-for-python-developers-with-starter-codes-2e46c248b47c>  

谢谢你们坚持到最后。我希望你们都喜欢这篇文章。祝大家有美好的一天！