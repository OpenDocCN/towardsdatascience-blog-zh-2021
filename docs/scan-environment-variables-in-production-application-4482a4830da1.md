# 扫描生产应用程序中的环境变量

> 原文：<https://towardsdatascience.com/scan-environment-variables-in-production-application-4482a4830da1?source=collection_archive---------31----------------------->

![](img/45a0f77e60dbcbb1ca3744d4d169c5d3.png)

照片由[埃琳娜·莫日维洛](https://unsplash.com/@miracleday?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/keys?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

没有环境变量，就没有完整的生产应用程序。大多数项目将环境变量作为事实保存在`.env`中。随着应用程序中环境变量的增加，很难在生产中准确无误地跟踪它们。

在部署应用程序时，在生产环境中定义或更新 env 变量并不是一种理想的方式。

为此，我创建了一个简单的 **npm 包** `scan-env`。这个简单的包扫描应用程序环境变量，并打印缺少环境变量的报告。

自己去试试[链接](https://www.npmjs.com/package/scan-env)。

在下一节中，我将创建一个虚拟项目来使用它。

# 入门指南

创建一个`scanenv-demo`项目。

使用`npm init -y`初始化项目。

## 安装扫描环境

```
npm install scan-env
```

## 项目结构

```
scanenv-demo
|- index.js
|- .env
|- .env.example
```

在项目中创建 3 个文件`index.js`、`.env`和`.env.example`。

## **index.js**

将下面的代码复制并粘贴到`index.js`中。

```
const scan = require("scan-env");// scan env
const status = scan();if (!status) {
  console.error("Missing Envs.");
  process.exit(1);
}console.log(
  `${process.env.GREETING} ${process.env.REGION} from ${process.env.FROM}.`
);
```

## **。环境**

从这个文件中，应用程序将使用环境变量。

将以下按键复制并粘贴到`.env`中。

```
GREETING=Namaste
REGION=World
```

## **env . example**

列出应用程序所需的所有环境变量。

复制并粘贴`.env.example`中的以下按键。

```
GREETING=Anything
REGION=Anything
FROM=Anything
```

> 注意:`.env.example`的语法必须和`.env`一样。

## 运行应用程序

`FROM`键在`.env`中丢失。我们在等一份丢失的报告。

```
node index.js
```

**输出**

```
Env Scan Report
--------------------------------------------------------------------
Total Missing Environment Variables:
1Missing Environment Variables:
FROM
--------------------------------------------------------------------
```

在`.env`中增加`FROM`键

```
GREETING=Namaste
REGION=World
FROM=India
```

使用`node index.js`运行应用程序。

**输出**

```
Namaste World from India.
```

## **忽略环境变量**

当您在不同的环境中运行应用程序时，您可能希望忽略一些环境变量。

为此使用`.envignore`文件。

创建一个`.envignore`文件并粘贴下面的密钥。

```
REGION=Anything
```

默认情况下，`scanenv`检查`.envignore`是否存在。如果它存在，那么它会忽略这些键。

运行应用程序`node index.js`。

**输出**

```
Namaste undefined from India.
```

## 结论

如果文件名不是`.env`、`.env.example`和`.envignore`，则以相同的顺序将文件名传递给函数。

感谢阅读。