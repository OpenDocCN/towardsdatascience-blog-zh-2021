# 不要丢失数据:通过 CLI 保存 MySQL 数据库备份

> 原文：<https://towardsdatascience.com/dont-lose-your-data-saving-a-mysql-database-backup-via-cli-68d87e8c237e?source=collection_archive---------28----------------------->

## 如何轻松备份 MySQL 数据库

![](img/d1fa81793dc3d3b02b6f2b829d50839b.png)

马库斯·温克勒[@马库斯·温克勒](http://twitter.com/markuswinkler)在 Unsplash 的照片

本文将快速描述如何通过 CLI 使用 **mysqldump** 保存数据库的转储文件。在那里，您可以找到以下主题的代码和解释:

*   保存数据库转储文件
*   保存所有数据库或数据库列表
*   保存表格
*   如何在 Windows 上使用 mysqldump CLI
*   错误 1044 拒绝访问-锁定表

如果你曾经使用过数据库，你就会知道备份数据有多重要。数据丢失发生在你没有预料到的时候，你必须做好准备。保存数据库中的转储文件是防止数据丢失的一种方法，甚至更多。

转储文件的主要用途是在数据丢失的情况下备份数据，但除此之外，转储文件还可以用于多种其他用途，如复制数据库并将其用于实验、将数据库的副本发送给某人、作为副本的数据源等等。

# 保存数据库转储文件

要保存数据库转储文件，首先必须打开终端并键入以下命令。在调用 **mysqldump** 命令时，必须向数据库的服务器传递一个具有读取权限的用户，数据库的名称，并在“>之后传递转储文件的位置和名称。

```
mysqldump -h <server> -u <user> -p <database>  > file.sql
```

运行这行代码后，它会询问用户密码，并将您的文件保存在传递的目录中。

# 保存所有数据库或数据库列表

如果你想一次保存所有的数据库，你可以使用命令“all-databases”和服务器及用户。

```
mysqldump -h <server> -u <user> --all-databases > file.sql
```

如果你想一次保存多个数据库，你可以使用命令“databases ”,传递一个你想保存的数据库列表，后面跟服务器和用户。

```
mysqldump -h <server> -u <user> --databases <db1> <db2> <db3> > file.sql
```

在运行这些行之后，它将询问用户密码，并将您的文件保存在传递的目录中。

# 保存表格

如果只想保存一个表，只需在命令中的数据库后面添加表名。

```
mysqldump -h <server> -u <user> -p <database> <tablename> --single-transaction > file.sql
```

# 如何在 Windows 上使用 mysqldump CLI

要在 Windows 上使用它，您只需打开命令行并添加 MySQL 工作台的路径，然后传递命令保存您的数据库。

```
set path=C:\Program Files\MySQL\MySQL Workbench 6.3 CEmysqldumpmysqldump -h <server> -u <user> -p <database>  > file.sql
```

# 错误 1044 拒绝访问-锁定表

如果您有一个锁定的表，您会得到“1044 拒绝访问错误”和以下消息:

> "获得错误:1044:使用锁表时，用户' user'@'% '对数据库' database '的访问被拒绝"

要解决这个错误，您只需在代码中添加命令“single-transaction”。单个事务命令将强制它在发出 BEGIN 时转储数据库的一致状态，而不会阻塞任何其他应用程序。

```
set path=C:\Program Files\MySQL\MySQL Workbench 6.3 CEmysqldump -h <server> -u <user> -p <database> --single-transaction > file.sql
```

如果您想了解更多关于 **mysqldump** 命令的信息，或者如果您正面临我所介绍的不同场景，请访问以下链接中的 MySQL 文档:

  

非常感谢您的阅读！我希望这些技巧能帮助你保存 MySQL 数据库的备份，防止项目中的数据丢失。

有任何问题或建议可以通过 LinkedIn 联系我:【https://www.linkedin.com/in/octavio-b-santiago/ 

# 更多阅读

</powerbi-the-tool-that-is-beating-excel-8e88d2084213> [## power bi——击败 Excel 的工具

towardsdatascience.com](/powerbi-the-tool-that-is-beating-excel-8e88d2084213) </hacking-chess-with-decision-making-deep-reinforcement-learning-173ed32cf503>  </how-to-build-the-perfect-dashboard-with-power-bi-28c35d6cd785> 