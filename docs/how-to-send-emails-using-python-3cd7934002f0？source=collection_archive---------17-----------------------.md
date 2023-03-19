# 如何使用 Python 发送电子邮件

> 原文：<https://towardsdatascience.com/how-to-send-emails-using-python-3cd7934002f0?source=collection_archive---------17----------------------->

## 向您展示使用 Python 与人交流的多种方式

![](img/0cd8c532e9d246de05213935a6207664.png)

来源:[https://unsplash.com/photos/3Mhgvrk4tjM](https://unsplash.com/photos/3Mhgvrk4tjM)

在本教程中，我将向您展示使用 Python 发送电子邮件的多种方式。这在许多项目或情况下非常有用，在这些项目或情况下，您需要以快速、简单和安全的方式与不同的人共享任何类型的信息。

## 传统方式:SMTPLIB

当使用 Python 发送电子邮件时，这个库是最受欢迎的。它创建了一个简单邮件传输协议(SMTP)会话对象，可用于向任何 internet 机器发送邮件。

在使用此模块发送电子邮件之前，您需要更改电子邮件帐户的设置。事实上，你需要在作为发件人的电子邮件中“启用对不太安全的应用程序的访问”。更多信息请点击这里:[https://support . Google . com/accounts/answer/6010255 # zippy = % 2c if-less-secure-app-access-on-for-your-account](https://support.google.com/accounts/answer/6010255#zippy=%2Cif-less-secure-app-access-is-on-for-your-account)

一旦这样做了，这里的代码将允许你立刻发送一封非常简单的电子邮件。

这传递了一个非常简单的信息。当然，您可以添加主题、附件等。但是我将要展示的另一种方法也可以用更简单的方式实现。

## 另一种选择:Yagmail

Yagmail 是一个 gmail 客户端，旨在使发送电子邮件变得更加简单。

我个人喜欢它，因为它非常容易给邮件添加内容(附件、主题、链接等)。)而我发现在使用 SMTPLIB 时并不那么直接。

下面是发送包含主题和附加图像的电子邮件的代码。还是那句话，超级简单！

在第 7 行中，`‘test’`实际上是邮件的主题，而在内容中，您可以添加消息和附件(图像、word 文档、音频文件等)。)超级轻松。

## 更具伸缩性的方式:使用 API

电子邮件服务现在已经存在，它可以用很少几行代码帮你发送尽可能多的电子邮件。

使用服务的好处是，它允许您跟踪您发送的内容。你可以很容易地知道是否收到电子邮件，何时何地发送了多少封。这种类型的信息并不总是必要的，但是随着项目的发展，您对这种类型的信息的需求也会增加。

这种服务可以是免费的。例如，如果你使用 Mailgun([https://www.mailgun.com/](https://www.mailgun.com/))，你每月可以免费发送多达 5000 封电子邮件。

此类服务的唯一问题是，你需要创建一个账户，如果你打算发送大量电子邮件，你最终可能需要花钱。

一旦你有了一个帐户，这里是你如何使用 Mailgun 和 Python 发送电子邮件。您自己的个人功能(具有正确的凭证)将很容易在您的帐户中使用。

我希望这能对你正在做的任何项目有所帮助，如果你有任何问题，请不要犹豫。

谢谢！