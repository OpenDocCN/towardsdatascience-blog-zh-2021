# 如何将 XML 转换为平面表(Python)

> 原文：<https://towardsdatascience.com/how-to-convert-xml-to-a-flat-table-python-f51576f569ad?source=collection_archive---------18----------------------->

![](img/785c0ae4ec9db4ec56ed2a3df64e6fd3.png)

图片由 Pixabay 提供

*-这个故事描述了如何使用 REST API 将高度嵌套的 XML 转换为平面数据帧。-*

在这个故事中，我们将探索如何使用这个免费的 REST API 将任何简单的嵌套 XML 转换成平面表。这个 REST API 利用一个作为概念验证开发的脚本，它推断任何嵌套 XML 的结构，组织数据，然后以清晰的结构化格式返回数据。

目前，API 只能将 XML 数据作为 Pandas 数据帧返回，但是您可以将数据帧导出/保存为您希望的任何数据格式。

# 它是如何工作的

用户登录和注册由 Amazon Cognito 管理，它处理用户凭证的生成和验证。这允许我们在限制 REST API 访问的同时轻松管理用户。

REST API 结合了亚马逊的 API 网关、AWS Lambda 和亚马逊 S3。Lambda 存放了我们的 XML 解释器代码，而 API Gateway 充当了我们调用 Lambda 的安全访问点。Lambda 还跟踪我们的用户请求。这种情况每天都会被跟踪，并以定制的日志格式(与我们的 CloudWatch 日志分开)记录到 S3，这样 Lambda 就可以验证任何用户每天调用 XML 解释器的次数都不能超过 4 次。

提供的 Python 脚本处理发送有效请求所需的 REST API 令牌的获取。由于 REST API 是受限制的，我们使用我们的 Cognito 登录凭证来生成我们的关联令牌，然后脚本自动将它传递给我们的 REST API 请求。然后，该脚本还处理 XML 编码、REST API 请求/响应处理，并将响应解码成数据帧。

# 如何使用 REST API

## 第一步:注册

您需要注册 REST API 凭证。你可以在这里这样做[。滚动到页面底部，然后单击“注册”。](http://bitsyl.com/xml_converter.html)

注册需要一个有效的电子邮件地址和您选择的密码。请务必点击您通过电子邮件收到的“验证”链接。

## **第二步:警告和注意事项**

REST API 是免费提供的，因此随后受到限制。这意味着作为一个公共用户，我们每天只能发出 4 个请求，XML 解释器将只接受遵循以下准则的 XML:

*   XML 字符限制为 4000 个字符(大约是本文后面提供的示例 XML 的两倍)。
*   XML 文件必须**而不是**包含实体或其他 XML[dtd](https://www.w3schools.com/xml/xml_dtd.asp)。
*   建议您的 XML 只包含属性和文本，如下面提供的示例所示。嵌套元素和属性的数量没有限制。

API/Lambda 已配置为在处理通过请求接收的任何 XML 之前扫描上述要求。

## **第三步:准备 XML 文件**

请务必在上传之前检查您的 XML 文件，确保其格式正确并符合上面列出的要求。

如果没有 XML 文件，但想测试 XML 解释器，可以使用下面提供的示例 XML 代码。样本来自 [Adobe 开源](https://opensource.adobe.com/Spry/samples/data_region/NestedXMLDataSample.html)。随意编辑 XML，添加额外的嵌套元素、属性等。

## 步骤 4:创建您的 REST API Python 脚本

为了使用 REST API，我们需要使用我下面提供的脚本准备一个请求。只需将脚本复制并粘贴到 Python 文件或笔记本中，然后执行脚本:

**注意**:一定要用你的 XML 文件路径的路径替换 XML 文件路径。

该脚本将提示您输入之前创建的用户名和密码。如果登录凭证有效，它将自动编译 REST API 请求，编码您的 XML 数据，将其发送给解释器，然后返回您的平面熊猫数据帧。

恭喜，您刚刚使用 REST API 将您的 XML 转换成一个漂亮的平面数据帧！现在，您可以在 Pandas 中进一步操作您的数据，或者导出到您选择的文件类型。