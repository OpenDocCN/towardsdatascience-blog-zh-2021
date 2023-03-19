# 用 Python 构建健壮的 Google BigQuery 管道:第三部分

> 原文：<https://towardsdatascience.com/build-robust-google-bigquery-pipelines-with-python-part-iii-e19c6da566f3?source=collection_archive---------31----------------------->

## 谷歌云平台

## BigQuery 和 Google Sheet 隐藏的复杂性

![](img/fb0ef064bd9aad2529a663c57bb87406.png)

由 [Ashkan Forouzani](https://unsplash.com/@ashkfor121?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/jigsaw-puzzle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

这是关于在 Python 中与 BigQuery 交互以构建数据管道的三篇文章系列的第三部分。

*   [第一部分:Apache Beam 与 Google BigQuery API](/build-robust-google-bigquery-pipelines-with-python-part-i-1ac8ca11391a)
*   [第二部分。Python 中的 big query](/build-robust-google-bigquery-pipelines-with-python-part-ii-2883423a1c8a)`[STRUCT](/build-robust-google-bigquery-pipelines-with-python-part-ii-2883423a1c8a)`
*   第三部分。BigQuery 和 Google Sheet 隐藏的复杂性

在 Spotify，Google Sheet 是一款非常受欢迎的办公工具。BigQuery 的优势之一是它能够与存储在 Google Sheet 中的数据进行交互。您可以使用新的 BigQuery 数据连接器 Connected Sheets 访问、分析、可视化和共享电子表格中的数十亿行数据。当数据科学家或工程师与不太懂技术的用户合作时，这非常方便。然而，当涉及到 Google Sheet 时，BigQuery 中有一些意想不到的行为，起初它们可能看起来并不直观。本文旨在深入探讨 BigQuery 中 Google Sheet 的不同用例，并帮助避免由于这些意外行为而造成的混淆，尤其是在 Python 管道环境中。

# I .从 Google Sheet 到 BigQuery 创建表格

在这种情况下,“创建表”一词会产生误导，更准确的说法应该是“创建表连接”BigQuery 正在建立一个活动连接，而不是将数据集实际导入 BigQuery。但是 BigQuery console 使用这个术语，所以我们现在坚持使用它。

以下是从 Google Sheet 创建表格需要做的工作概述:

1.  为`Create table from`选择“驱动”
2.  将 URI 工作表复制并粘贴到下一个框中
3.  为`File format`选择谷歌表单
4.  记下表的元数据，如项目、数据集和表名。为表定义模式—我建议手动输入模式，因为自动检测并不总是正确的
5.  定义`Header rows to skip`，如果没有标题，则为 0，如果有 n 个标题行要跳过，则为 n

![](img/78793c70d69d511f609421717cac3b16.png)

从 Google 工作表创建表格

一旦创建了表，您将能够在 BigQuery 中查询它。然而，一个常见的错误是认为数据现在作为一个单独的副本存储在 BigQuery 中。事实并非如此。BigQuery 在这里所做的仅仅是创建一个到 Google Sheet 的活动连接，其含义如下:

1.  如果在 Google Sheet 上放置任何过滤器，BigQuery 将无法显示整个数据集；Google Sheet 中的过滤器实际上会立即反映出来，并影响数据在 BigQuery 中的外观。因此，如果您想访问 BigQuery 中的所有数据，请始终对单个视图使用过滤器。
2.  如果有任何模式更改，如列名更改、列类型更改，则不能在 BigQuery 中编辑现有模式。删除当前的 BigQuery 表并重做`Create table`过程。

# 二。用 Python 阅读 BigQuery Google Sheet 数据

如果您尝试使用服务帐户来运行作业，请确保您将服务帐户添加为 Google Sheet 的编辑器。

将 Google 工作表设置为 BigQuery 表的最重要步骤是在 Python BigQuery API 中修改 BigQuery 客户机的范围。

```
import pandas as pd
from google.cloud import bigqueryscopes = (
    'https://www.googleapis.com/auth/drive',
    'https://www.googleapis.com/auth/drive.file',
    'https://www.googleapis.com/auth/spreadsheets',
)
bigquery.Client.SCOPE += scopesclient = bigquery.Client()
```

然后，这里有一行代码将您的查询结果转换成 pandas `dataframe`。

```
gs = client.query("""SELECT * FROM `YOUR_PROJECT.YOUR_DATASET.{table}`""".format(table=table_name)).result().to_dataframe()
```

# 三。用 Python 写 Google Sheet

有几个包可以让你用 Python 写 Google Sheet。我将以`pygsheets`为例。如果您不打算在管道中运行作业，这个包非常简单。但是，如果您计划在管道中使用包，则需要一个变通方法；出于安全原因，我们不能在我们的云服务器中存储任何服务帐户密钥文件，因此常规功能将不起作用。让我们看看这两个场景中的一些例子。

假设您计划为此作业使用服务帐户密钥。这里是服务帐户的快速介绍。

> 服务帐户是与电子邮件相关联的帐户。该帐户使用一对公钥/私钥进行身份验证，这比其他选项更安全——只要私钥保持私有。— Pysheets

要创建服务帐户，请按照下列步骤操作:

1.  转到“凭据”选项卡并选择“创建凭据>服务帐户密钥”。
2.  接下来选择服务帐户为“App Engine default ”,密钥类型为 JSON，然后单击创建:

![](img/2e84df82aa0362a63dd2268eeb5ff175.png)

3.接下来点击下载按钮下载`client_secret[…].json`文件；请务必记住您将它保存为本地`json`文件的位置:

![](img/70d7839c481f00a83de897f5a8c9e45f.png)

现在我们已经准备好了服务帐户密钥文件，让我们导入所需的包:

```
import requests
import json
import pygsheets
import styx_secrets
from google.oauth2 import service_account
from google.cloud import bigquery
import pandas as pd
import numpy as np
```

授权过程很简单:

```
gc = pygsheets.authorize(client_secret='path/to/client_secret[...].json')
```

然而，如果您有一个管道作业，这个过程会稍微复杂一些，因为我们需要使用一个字典作为我们的解决方法。

首先，我们需要加密我们的服务帐户密钥。在 Spotify，我们有一个名为`styx-secrets`的包用于加密。请注意，为了让`pygsheets`识别这个密钥，需要对`utf-8`进行解码。

```
import styx_secretskey1 = styx_secrets.decrypt("FIRST_HALF_OF_ENCRYPTED_KEY_STRING").decode("utf-8")
key2 = styx_secrets.decrypt("SECOND_HALF_OF_ENCRYPTED_KEY_STRING").decode("utf-8")
```

我不得不把密匙分解成两串，因为原来的密匙串太长了，`styx-secrets`不接受。

然后我们可以在一个`dictionary`中准备密钥文件，这基本上是`json`密钥文件的一个`copy`和`paste`，除了用你的加密字符串交换真实的密钥(注意解密后还需要做一些格式上的改变):

```
sc_key = {
    "type": "service_account",
    "project_id": "YOUR_PROJECT",
    "private_key_id": "YOUR_PRIVATE_KEY_ID",
    "private_key": ("-----BEGIN PRIVATE KEY-----" + key1 + key2 + "-----END PRIVATE KEY-----\n").replace("\\n", "\n"),
    "client_email": "YOUR_SERVICE ACCOUNT_EMAIL",
    "client_id": "SERVICE_ACCOUNT_CLIENT_ID",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/xxxxxxxx"
}credentials = service_account.Credentials.from_service_account_info(
    sc_key, 
scopes=[
'https://www.googleapis.com/auth/spreadsheets',
'https://www.googleapis.com/auth/drive',
'https://www.googleapis.com/auth/drive.file', ])gc = pygsheets.authorize(custom_credentials=credentials)
```

现在我们完成了授权，我们可以玩一玩了！要写入 Google 工作表，这里有一个例子。

假设您有一个跟踪管道事件的 Google Sheet，这样您的团队就可以轻松地分析数据:

```
# open the google spreadsheet 
sh = gc.open('Data Incident Tracker')

# select the first sheet
wks = sh[0]# import previous events
track_prev = client.query("""SELECT * FROM `YOUR_PROJECT.YOUR_DATASET.{table}`""".format(table=table_name)).result().to_dataframe()

# current event
track_now = [{
'incident_date': date_param,
'num_of_tables': len(missing_list),
'table_list': ', '.join(missing_list),
'alert': True,
'root_cause': root_cause}, ]

# create new data frame with all incidents

tracker_df = pd.concat([track_prev, pd.DataFrame(track_now)])

# update the first sheet with tracker_df

wks.set_dataframe(tracker_df.replace(np.nan, '', regex=True), 'A1')
```

最后，请注意，如果您的表中有任何`None`值，它们实际上将作为“NaN”字符串上传到 Google Sheet 中。为了避免这种情况，使用`replace(np.nan, ‘’, regex=True)`将它们替换为空字符串，单元格将作为实际的`Null`上传到 Google 工作表中。

这是 Python 系列中 Google BigQuery 的最后一篇文章。我希望它们对你有用。如果您有任何问题，请随时联系我们或发表评论。干杯！