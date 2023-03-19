# 使用 Python 从 Redshift 到 Google 电子表格

> 原文：<https://towardsdatascience.com/from-redshift-to-google-spread-sheet-using-python-fd4b50131940?source=collection_archive---------14----------------------->

## 读取、写入和格式化电子表格的简化工作流程

![](img/8c0259055319fa4866e3c169f494422b.png)

作者图片

最近，我试图从亚马逊红移数据库中提取数据，并使用 Python 脚本将其推送到谷歌电子表格中。该脚本的目的是从 Amazon redshift 数据库中读取数据，应用一些业务规则，并将其写入 google 电子表格。

我已经在我的 python 脚本中使用了 [**gspread**](https://docs.gspread.org/en/v3.7.0/) 库，它只不过是用于 google sheets 的 python API。使用它，我们可以非常容易地读取、写入和格式化电子表格。

要与 Google API 交互，第一步是在 [Google 开发控制台](https://console.cloud.google.com/apis/dashboard?ref=https:%2F%2Fwww.google.com%2F&pli=1&project=test-rebike&folder=&organizationId=)中创建一个项目并启用 API。

1.  创建一个项目([点击这里](https://console.cloud.google.com/projectcreate?previousPage=%2Fiam-admin%2Fserviceaccounts%2Fdetails%2F109825980442063387913;edit%3Dtrue%2Fkeys%3Fproject%3Dglossy-protocol-321412%26supportedpurview%3Dproject&folder=&organizationId=0&supportedpurview=project)):

![](img/4e0b93f7eae499d10acd87f2083bd3cc.png)

作者图片

2.给项目命名:

![](img/8afe4f1bb3b9555a6133bd5a495ad3de.png)

作者图片

3.转到项目仪表板，点击+启用 API 和服务:

![](img/e25af5cbebe2ae05a41f66624cd7bc3e.png)

作者图片

4.搜索 Google Drive API，点击它并启用它。执行相同的过程来启用 Google 电子表格 API:

![](img/1f2b1a62f8940d99affaa71ffb11faba.png)

作者图片

![](img/67e47f2c18a9e24962933dad368ee412.png)![](img/af2c4f99e4aceafecbb093418bebe5de.png)

5.点击创建凭证:

![](img/a37c2010a4899733d60182039ce232ab.png)![](img/ef22407b818d644d65d74c9ce896a2a0.png)

作者图片

6.选择参数并点击**我需要什么凭证？**

![](img/c056c4d43f83b87e0554bff4d34dc70c.png)

作者图片

7.输入服务帐户名并选择角色:

![](img/0088fb8ce161266013d03b055e1d12d1.png)

作者图片

8.现在单击 KEYS 并创建一个 json 格式的新密钥:

![](img/5e0e9e6bf891740927a68db8fbed2c76.png)

作者图片

将下载一个 JSON 文件。我们的脚本中需要这个 JSON 文件。因此将该文件重命名为***client _ secret key . JSON***。

现在转到你的 Google Drive，创建一个 Google Sheet，命名为**redshift-to-Google-Sheet**。从上面下载的 JSON 文件中复制 ***client_email*** 值，并将该 Google Sheet 共享给此 ***client_email*** 并具有编辑权限。

至此，我们已经设置好了一切，现在让我们编写 Python 脚本。要使用 python 脚本与电子表格交互，我们必须安装 gspread Python 库。因此，打开终端并运行以下命令。oauth2client 是一个 python 库，用于访问受 OAuth 2.0 保护的资源。

```
pip install gspread
pip install oauth2client
```

现在创建一个 Python 文件，并将其命名为 redshift_to_spradsheet **。** py。复制并粘贴下面的代码。

```
import gspread
from oauth2client.service_account import ServiceAccountCredentialsscope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/spreadsheets",
         "https://www.googleapis.com/auth/drive.file", "https://www.googleapis.com/auth/drive"]credentials = ServiceAccountCredentials.from_json_keyfile_name('client_secretkey.json', scope)
client = gspread.authorize(credentials)
```

现在让我们添加连接到红移数据库的代码。我使用了 **sqlalchemy** 库，它方便了 Python 程序和数据库之间的通信。

要安装 sqlalchemy，请键入以下内容:

```
pip install sqlalchemy
```

现在将 sqlalchemy 导入到 python 脚本中，并使用 create_engine 建立数据库连接。在连接字符串之后，编写 select 查询(或任何其他 DML)来获取数据并将其存储到 pandas 数据帧中。

```
from sqlalchemy **import**  create_engine
import pandas as pd*# Connecting to postgresql*
engine = create_engine('postgresql://user:password@host:5439/database')
data_frame = pd.read_sql('SELECT * FROM article_sales;', engine)
```

要将此数据框写入 google 电子表格，我们必须安装 df2gspread python 库。要安装 df2gspread，请键入以下内容:

```
pip install df2gspread
```

现在将 df2gspread 导入到 python 脚本中，并创建一个名为 write_googlesheet 的函数，如下所示:

```
from df2gspread import df2gspread as d2gdef write_googlesheet(df, spreadsheet_key, sheet_title, starting_cell, overwrite):
    d2g.upload(df, spreadsheet_key, sheet_title, credentials=credentials, col_names=overwrite, row_names=True, start_cell = starting_cell, clean=overwrite)
```

这个 d2g.upload 函数将给定的熊猫数据帧上传到 Google Drive，并返回 gspread 工作表对象。这里我已经从电子表格的 url 传递了电子表格键，工作表标题作为电子表格名称(即工作表 1、工作表 2)。要了解更多关于函数参数的信息，请[点击此处](https://df2gspread.readthedocs.io/en/latest/examples.html)。

github 链接: [redshift_spreadsheet.py](https://github.com/FrennyMacwan/redshift_to_spreadsheet/blob/main/redshift_spreadsheet.py)

现在运行 Python 脚本，在浏览器中打开**红移到 Google-Sheet** Google Sheet。您将看到您的 Google 表单已更新了内容。

# 结论

这篇文章的目的是让人们了解这种类型的实现。可以有许多不同的策略，但我发现它不太复杂，对于天真的开发人员来说容易理解。