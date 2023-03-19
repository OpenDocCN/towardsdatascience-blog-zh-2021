# 为数据项目使用环境:初学者指南

> 原文：<https://towardsdatascience.com/using-environments-for-data-projects-a-guide-for-beginners-e2a825c40441?source=collection_archive---------25----------------------->

![](img/5c63acf00277c662fb96b2ff7fafea0e.png)

照片由[克里斯里德](https://unsplash.com/@cdr6934?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 Unsplash 上拍摄

## 通过一些简单的步骤改善您和您的团队的编码体验

# 介绍

当我开始用 Python 编程时，对我来说比较困惑的问题之一是如何恰当地管理我安装在计算机上的包。我通常的工作流程是，当我需要一个新的包时，我将它安装在默认的 Python 系统中，而不知道我在做什么。因此，由于包之间的不兼容问题，我的 Python 开始出现错误。此外，我的代码在我同事的机器上无法运行，因为他们没有我使用的相同版本的软件包。几个月后，我发现了虚拟环境这个神秘的概念。当我发现这个工具以及如何使用它们时，它完全改善了我的编码体验。

虚拟环境基本上是一个隔离的设置，您可以在其中指定与依赖项及其版本相关的所有功能，以开发特定的项目。简而言之，就是在你的电脑上安装一个新版本的软件(比如 Python，Rstats，Javascript ),它不会与默认版本或其他环境共享任何东西。在这种情况下，虚拟环境允许您:

*   根据您正在进行的每个项目的需求，拥有几个版本的 Python(或 R)。例如，由于 Python 开发人员在发布新版本时会增加和减少特性，这将有助于避免版本错误和不兼容性。
*   您可以精确地指定每个项目需要哪些包以及哪些版本。当您定义需求时，您的合作者也可以复制您的环境，避免由于不同机器上的不同规范而导致的不兼容性。

为 Python 创建虚拟环境有两种定义明确且有据可查的方法:`[virtualenv](https://docs.python.org/3/library/venv.html)`和。一方面，我们有`[virtualenv](https://docs.python.org/3/library/venv.html)`，一个允许我们创建和控制我们的环境的环境管理器。安装软件包最简单的方法是通过。对于 Stata 用户来说，这相当于 scc。另一方面，我们有`conda`，他既是环境经理也是包经理。

在这篇文章中，我将教你如何用这两种工具创建环境，并利用这一神奇的工具！

# 目录

*   使用`venv.`创建和管理环境
*   使用`conda`创建环境。
*   我的个人工作流程。
*   结束语

# 使用`venv`创建和管理环境。

```
# 1\. Update pip package manager: 
#  Mac/Linux:  
$ (base) python -m pip install --user --upgrade pip
# Windows: 
$ (base) python -m pip install --upgrade pip # 2\. Install virtualenv. 
# Mac/Linux:  
$ (base) python -m pip install --user virtualenv
# Windows:  
$ (base) python -m pip install --user virtualenv# 3\. Using your terminal, go to the folder of the project where you are working:
$ (base) cd path/to/your/cool/project# 4\. Now, you can create a virtual environment using  $ (base) python -m venv your-env-name # 5\. Activate the environment: 
# Mac/Linux: 
$ (base) source your-env-name/bin/activate 
# Windows:
$ (base) your-env-name\Scripts\activate.ps1
```

恭喜你。！你只是创造了一个环境。如果您使用的是 Anaconda，您的终端可能看起来像这样:

```
$ (base)(your-env-name)
```

现在，我们可以开始在虚拟环境中安装软件包了。为了便于说明，我们将使用一个最关键的包来执行科学计算:`[NumPy](https://numpy.org)`。

```
# Check the installed packages
$ (base)(your-env-name) pip freeze# Install the latest version of numpy
$ (base)(your-env-name) pip install numpy# Install a specific version:
$ (base)(your-env-name) pip install numpy==1.17.2 # Install a version greater than or equal to a specific one: 
$ (base)(your-env-name) pip install numpy>=1.17.2 # Upgrade a package to a newer version: 
$ (base)(your-env-name) pip install --upgrade numpy
```

使用`pip`安装包有很多变化和命令。以下是[文档](https://packaging.python.org/tutorials/installing-packages/)的链接。

为一个项目安装多个包是很常见的。除了`numpy`，让我们想象你需要处理数据帧(`pandas`)和图形(`NetworkX`)。您可以指定一个`requirements.txt`文件来管理您需要的所有包。

```
# Location of this file: path/to/your/cool/project/requirements.txt networkx>=2.4
pandas>=1.1.0 
numpy>=1.17.2
```

使用以下命令安装`requirements.txt`中的所有软件包:

```
$ (base)(your-env-name) pip install -r requirements.txt
```

最后，要停用或删除环境:

```
# Deactivate the environment
$ (base)(your-env-name) deactivate# Delete the environment
# Mac/Linux:
$ (base) rm -rf your-env-name
# Windows:
$ (base) Remove-Item -LiteralPath "your-env-name" -Force -Recurse
```

# 使用`conda`创建环境。

我们已经知道如何使用`venv`和`pip`来管理环境和包。另一种广泛使用的方式是使用`conda`。正如我们前面所讨论的，`conda`既是一个环境又是一个包管理系统，所以您可以使用`conda`创建环境和安装包。根据你的操作系统，点击[这里](https://conda.io/projects/conda/en/latest/user-guide/install/index.html)安装`conda`。

```
# 1\. Check if conda was installed correctly 
# This command will show all the installed packages in base ... 
$ (base) conda list # ... and this will show all the environments 
$ (base) conda env list # 2\. Create a new environment with an specific python version 
$ (base) conda create -n your-env-name python=3.8 # You can create the environment with some packages 
$ (base) conda create -n your-env-name python=3.8 networkx pandas>=1.1.0 # Activate (deactivate) the environment 
$ (base) conda activate your-env-name  
$ (my-env) conda deactivate
```

默认情况下，您的所有环境都位于您的`conda`目录中。例如，在我的机器中，环境保存在`/Applications/anaconda3/envs/your-env-name`中。这种方法与`venv`后面的方法不同，因为后者在项目的同一个文件夹中创建环境。

```
# Create and env in an specific directory 
$ (base) cd path/to/your/project
$ (base) conda create -p ./your-env-name # or alternatively
$ (base) conda create -p path/to/your/project/your-env-name # To activate this environment 
$ (base) conda activate -p path/to/your/project/your-env-name
```

作为一个包管理器，`conda`默认安装了来自 [Anaconda 仓库](https://repo.anaconda.com)的包。你也可以从第三方软件仓库安装软件包( [Conda-Forge](https://conda-forge.org) 是最流行的一个)，也可以从`pip`安装。这就是它的工作原理！

```
# IMPORTANT! # Remember to activate the environment before installing packages $ (base) conda activate -n your-env-name   # -f path/to/your/project/your-env-name # Install a package from the Anaconda repo 
$ (your-env-name) conda install numpy # Install a package from conda forge 
$ (your-env-name) conda install -c conda-forge numpy # ... and add the channel to the configuration 
$ (your-env-name) conda config --append channels conda-forge  
# you can also define which channel to prioritize 
$ (your-env-name)  conda config --set channel_priority strict # Try to avoid pip if you are using conda! 
$ (your-env-name)  pip install numpy # Install a requirements.txt file 
$ (your-env-name) conda install --file requirements.txt
```

我发现用`conda`管理环境的一个惊人之处是，您可以在单个`.yml`文件中指定配置的每个方面。例如，让我们假设您有一个`environment.yml`配置:

```
name: your-env-name 
channels:   
- conda-forge   
- defaults 
dependencies: 
- python=3.7 
- pytest 
- ipykernel 
- ipython 
- pip:     
   - autopep8==1.5.4     
   - dropbox==10.4.1     
   - paramiko==2.7.2     
   - redis==3.5.3     
   - twilio==6.41.0
```

通过该文件，您可以使用以下方式创建环境:

```
# To create the env 
$ (base) conda env create -n your-env-name -f /path/to/environment.yml # To update the env 
$ (base) conda env update -n conda-env -f /path/to/environment.yml
```

同样，如果你想在`.yml`中保存一个环境的规格，你可以这样做！

```
$ (base) conda activate your-env-name 
$ (your-env-name) conda env export > your-env-name.yml
```

更多详细信息，请阅读文档！

# 我的个人工作流程。

![](img/5863a0ac6455086827b1c5245a7c3aa1.png)

来源:照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 Unsplash 上拍摄

在我使用的任何环境中，有几个包是我一直想要的。这里是规范:)！

```
name: null 
channels:   
- conda-forge   
- defaults 
dependencies: 
- python>=3.7 #  Specify your desire version 
- ipython # To develop and run my code 
- pytest # To perform testing to the code  
- autopep8 # Code formatter for the PEP8 guidelines 
- mypy # Static typing 
- flake8 # For linting (spell and grammar checker of python code) prefix: path/to/env/conda-env
```

使用`venv`，该工作流程如下:

```
$ (base) cd path/to/your/project 
$ (base) python -m venv venv 
$ (base) source venv/bin/activate 
$ (venv) pip install ipython pytest autopep8 mypy flake8 --upgrade pip 
$ (venv) pip install -r requirements.txt # This is where I specify all the packages I'm gonna use!
```

# 结束语

使用虚拟环境将有助于您避免许多因不兼容问题导致的未知错误给自己和同事带来的麻烦。如果你喜欢这个帖子或者你有一些意见，请在 Twitter 上联系我！开始为您的项目使用环境吧！

# 参考资料和进一步阅读

*   [**康达环境权威指南**](/a-guide-to-conda-environments-bc6180fc533)
*   [**Python 虚拟环境**](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/26/python-virtual-env/)
*   [**数据科学最佳实践:Python 环境**](/data-science-best-practices-python-environments-354b0dacd43a)
*   [**GCPy: Python 支持的地质化学分析/可视化**](http://danielrothenberg.com/gcpy/getting_started.html)
*   [**Python 打包用户指南:教程，安装包**](https://packaging.python.org/tutorials/installing-packages/)
*   [**Python 环境**](https://rabernat.github.io/research_computing_2018/python-environments.html)

*最初发布于*[*https://Ignacio riveros1 . github . io*](https://ignacioriveros1.github.io/coding/2021/03/15/using-envs-for-data-projects.html)*。*