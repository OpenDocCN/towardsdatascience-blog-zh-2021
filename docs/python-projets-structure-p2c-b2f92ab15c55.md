# Python 项目的结构 P2C

> 原文：<https://towardsdatascience.com/python-projets-structure-p2c-b2f92ab15c55?source=collection_archive---------31----------------------->

![](img/d0a2167bad247a7baab2a54f4fc2b563.png)

在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 从管道到代码

作为一名程序员，我发现编码是一门艺术。当从事一个复杂的项目时，为了开发一个连贯、可靠和可持续的代码，需要遵循许多步骤，这些代码可以被其他贡献者阅读和恢复:

![](img/fad8ffcf87584c9bb9601197dd5ff946.png)

作者图片

*   首先，为了满足正确的需求，理解问题是至关重要的
*   这个项目可以被分割成多个子项目，这些子项目有助于完成任务，尤其是协作
*   您的管道是您的子项目的直接结果
*   代码的结构应该遵循相同的分段，这样您就可以拥有相同的管道和技术逻辑
*   为了有效地开发您的代码，可以在每个片段中遵循循序渐进的步骤

在这篇文章中，我们将看到如何从一个业务需求到一个简单易懂的全功能 **python** 代码。

# 目录

摘要如下:

1.  项目示例
2.  设计机构
3.  Python 中的导入

# 项目示例

为了便于说明，我们将考虑以下业务需求:

> 作为一名**高速公路经理**，我希望使用给定的路线对车辆进行**每日计数。为了回答这一需求，一个数据科学团队被投入到该项目中，并决定使用一个固定的摄像头，并计数**车牌的唯一编号。** (其中一个建议)**

这个想法可以看作是几个步骤的顺序*(其中一个简化的分解)*:

*   车辆检测
*   车牌检测
*   车牌的光学字符识别

因此，下面的管道:

![](img/dad3830965f9def64a47c11ee1032d06.png)

作者图片

# 项目的组织

给定上面的管道，可以如下组织代码:

```
|--**data/** *#useful to store temporary files for instance*
|--**Tests/** *#hosts the functional unit testings of your code/api*
|--**notebooks/** *#helpful for testing and developping and debugging*
|--|--develop.ipynb
|--**weights/** #weights are kept a part for easier manipulation|--**counthighwaypy/** *#folder hosting your entire code* |--|--***detectvehicle/*** *#1st brick of your pipeline*
|--|--|--detect_vehicle_main.py
|--|--|--detect_vehicle_utils.py
|--|--|--detect_vehicle_conf.py
|--|--|--Tests/ #independant unit testings relative to 1st brick
|--|--***detectlicenseplate/*** *#2nd brick of your pipeline*
|--|--|--licence_plate_main.py
|--|--|--licence_plate_utils.py
|--|--|--licence_plate_conf.py
|--|--|--Tests/ #independant unit testings relative to 1st brick
|--|--***ocrlicenseplate/*** *#3rd brick of your pipeline*
|--|--|--ocr_license_main.py
|--|--|--ocr_license_utils.py
|--|--|--ocr_license_conf.py
|--|--|--Tests/ #independant unit testings relative to 1st brick

|--|--utils.py
|--|--conf.py *#! very  important file (see below)*
|--|--main.py *#* orchestrator of the different bricks
|--|--Tests/ *#E2E technical unit testings*+--README.md
+--app.py *#hosts your API and calls the main.py*
+--packages.txt *#python environment*
+--launch_tests.sh *#functional unit testings*
+--pytest.ini
+--Dockerfile
```

如前一节所述，存储库的结构遵循管道中相同的逻辑。

*   每个**brick**hosts:
    **+utils**文件:包含您的 brick
    **+ conf** 文件的所有辅助函数:包含所有常量参数(变量名称、目录、超参数值……)
    ++main 文件:通常包含一个函数，该函数集合了 *utils* 文件
    **+ Tests** 文件夹:包含[单元测试](/data-scientists-starter-pack-32ef08f7829c)[这是实现更快、更有效调试的基本原则。](/data-scientists-starter-pack-32ef08f7829c)
*   当处理机器学习算法时，最好将潜在的**权重**存储在项目的根，因为在开发过程中它们可能会经常被替换。
*   使用**笔记本可以轻松尝试新功能。**根据给定的结构，每个都应该包含以下 python 代码，以便能够“看到”和导入模块 **counthighwaypy:**

```
import os
import sys
sys.path.insert(0, os.path.abspath("../")) #visible parents folders
from counthighwaypy import ...### your code
```

*   在模块 **counthighwaypy** 中，重要的是要有一个 **utils** 文件、 **conf** 和 **main** 文件，**在不忘记 E2E **测试**的情况下协调**不同的砖块
*   **conf** 文件非常重要，因为它设置了项目的**根目录及其不同的**子模块**和**目录**。可以这样写:**

```
import os
PROJECT_ROOT = os.path.realpath(os.path.join(os.path.realpath(__file__), "../.."))##### directories
DATA = os.path.join(PROJECT_ROOT, "data/")
NOTEBOOK= os.path.join(PROJECT_ROOT, "notebooks")SRC = os.path.join(PROJECT_ROOT, "counthighwaypy/")
WEIGHTS = os.path.join(PROJECT_ROOT, "weights/")MODULE_DETECT_VEHICLE = os.path.join(SRC, "detectvehicle/")
MODULE_DETECT_LICENCE_PLATE = os.path.join(SRC, "detectlicenseplate/")
MODULE_OCR_LICENSE_PLATE = os.path.join(SRC, "ocrlicenseplate/")
```

*   **app** 文件将您的项目封装到一个 [API](/api-flask-fastapi-http-postman-e1673d672596) 中，该 API 可供其他用户和服务使用
*   附加文件如 **packages.txt** 、 **pytest.ini、**和 **Dockerfile** 放在项目的根目录下

一旦代码的结构设置好了，最好以独立于其他模块的**独立格式**开发**每个模块**。也就是说，这里有一些你可以遵循的指导方针:

*   设置**格式**的**输入&输出**每块砖，其中输出的砖**I是输入的砖**I+1****
*   用简单的方式编写**代码的画布**(空函数)当你阅读它的时候，你会立刻明白这个脚本是做什么的
*   不要忘记**的签名**和**的评论**
*   使用 code v **版本工具**， [git](/all-you-need-to-know-about-git-github-gitlab-24a853422ff3) 进行更有效的协作
*   使用 [**代码格式器**和**代码转换器**](/keep-your-code-clean-using-black-pylint-git-hooks-pre-commit-baf6991f7376) 保持代码整洁
*   对于跨团队协作，将您的代码/包公开为可消费的 [API](/api-flask-fastapi-http-postman-e1673d672596)

# Python 中的导入

从 Python 3.3 开始，文件夹 **folderame** 被认为是一个模块(**，不需要****_ _ init _ _。py** 文件)并且可以简单地**导入**到 python 文件中，只要它是可见的，即在**相同的树级**上，通过使用:

```
import foldername
```

假设我们有以下结构:

```
|--**FOLDER1/** 
|--|--file1.py
|--**FOLDER2/** 
|--|--file2.py
|--main.py
```

*   在 main.py 中，我们可以

```
import FOLDER1.file1
import FOLDER2.file2
```

*   要在文件 1 中导入文件 2:

```
import os
import sys#make FOLDER2 visible to file1 (one step up in the tree)
sys.path.insert(0, os.path.abspath("../"))from FOLDER2 import file2
```

在一个复杂的 python 项目中，为了保持你的**导入** **的一致性，**建议从你代码的源**开始**所有的**。在我们的例子中，在任何。包含以下内容的 py 文件:**

```
from counthighwaypy.xxx.xxx import xxx
```

# 结论

还有其他方法来构建您的 python 项目，但是我发现本文中描述的方法简单易懂，易于掌握。也可以应用于 Python 以外的语言。

我希望你喜欢阅读这篇文章，它将帮助你在未来更好地组织你的工作。欢迎所有的评论和建议！