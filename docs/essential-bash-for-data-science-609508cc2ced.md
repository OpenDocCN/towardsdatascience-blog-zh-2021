# 数据科学的重要盛会

> 原文：<https://towardsdatascience.com/essential-bash-for-data-science-609508cc2ced?source=collection_archive---------42----------------------->

## 通过与贝壳相处来提高你的生产力

![](img/91d50d7daad7f7399365a69247ec4368.png)

[https://unsplash.com/@ngeshlew](https://unsplash.com/@ngeshlew)

作为一名数据专业人员，您可能已经了解 Python，这是最强大的通用语言之一，它的库几乎可以方便地做任何事情。那么，为什么要学习 Bash 和 Unix shell 呢？我在工作中发现的一些用例:

*   本地/远程使用文件系统
*   本地/远程监控系统资源
*   远程检查数据文件(在本地，我发现用 Pandas 启动 Python 控制台更快)
*   自动化任何使用不同命令行工具的作业
*   整合复杂的预处理步骤，包括编译软件或与数据库同步(可能通过 Docker)
*   在查询/预处理/训练/部署阶段之间建立粘合剂
*   将定制的实用程序整合在一起，这些实用程序利用了您无论如何都会从 CLI 使用的工具(例如 Git)
*   Makefile 经常被用作数据科学的 DAG 框架，例如在 cookiecutter 项目中

# 设置

注意，在 Mac 上，默认情况下你会被 BSD 工具所困，它们没有 GNU 那么强大。然而，你可以使用`brew install coreutils`，然后加上前缀`g`，例如`split`变成了`gsplit`。

你可能还想用`brew install bash`更新 Mac Bash 版本。

如果有任何命令不存在，那么在 Mac 上你通常可以运行`brew install command`。

在 Mac 上，你可能想安装 [iTerm2](https://iterm2.com/) 而不是默认的终端。

# 基础

用`#!/bin/bash`或`#!/usr/bin/env bash`启动脚本(在 Mac 上，您可能有多个 Bash - `which -a bash`版本)。

用双引号将`"$VARIABLE"`括起来以保留值。

`set -euox pipefail`是一个很好的默认配置(在脚本中)，它提供了:

*   在脚本中的任何命令失败时引发错误
*   运行命令时打印命令
*   在未设置变量的情况下引发错误

使用终端中的`Ctrl+R`搜索命令历史，使用`history`查看全部命令历史。

使用`info command`或`man command`或`help command`或`command --help`获得命令帮助。`whatis command`还打印来自`man`手册的第一行描述。

# 别名

别名是一种快捷方式，可用于冗长乏味的命令，例如

连接到远程机器:`alias remote="ssh user@bigawesomemachine.cloud`

连接到数据库:`alias db="psql -h database.redshift.amazonaws.com -d live -U database_user -p 5439"`

激活 Conda 环境:`alias pyenv="source activate py_39_latest`

甚至只是纯粹的懒惰:`alias jn="jupyter notebook"`

`alias repo="cd /Users/john/work/team_repo"alias l="ls -lah"`

`alias l=”ls -lah”`

将这些添加到`~/.bash_profile`中，以便它们在 shell 会话中保持不变。

# 检查文件

使用`tree`以树状格式列出目录的内容。

用`head -n 10 file`或`tail -n 10 file`读取文件的一端

观察动态变化的文件，如日志

`tail -f file.log`

在当前(嵌套)目录中查找文件

`find . -name data.csv`

浏览一个. csv 文件，其中“，”是列分隔符

`cat data.csv | column -t -s "," | less -S`

从明文配置中获取密钥值，这在 Bash 中不一定是可理解的`sed -n 's/^MODEL_DATA_PATH = //p' model_conf.py`

您可以使用`cut`对. csv 列进行 Pandas 风格的值计数，例如对分号分隔的文件中的第五列进行计数

`cut -d ";" -f 5 data.csv | sort | uniq -c | sort`

用`diff file1 file2`或`cmp file1 file2`比较文件，用`diff <(ls directory1) <(ls directory2)`比较目录。

使用`grep`在文件中搜索(正则表达式)模式。这个命令有很多有用的选项，所以最好读一下`info grep`。grep+cut 是一种强大的模式，可以根据模式过滤行并获得您关心的列，例如获得最新的提交散列:

`git log -1 | grep '^commit' | cut -d " " -f 2`

`sort`和`uniq`可以一起使用来处理重复，例如计算一个. csv 文件中重复的行数

`sort data.csv | uniq -d | wc -l`

# 控制结构

快速 if-then-else:

`[[ "$ENV" == prod ]] && bash run_production_job.sh || bash run_test_job.sh`

检查文件是否存在:`[[ -f configuration.file ]] && bash run_training_job.sh || echo "Configuration missing"`

数字比较:

`[[ "$MODEL_ACCURACY" -ge 0.8 ]] && bash deploy_model.sh || echo "Insufficient accuracy" | tee error.log`

Bash 是一种编程语言，所以它支持常规的`for` / `while` / `break` / `continue`结构。例如，要创建备份文件:

`for i in `ls .csv`; do cp "$i" "$i".bak ; done`

在 Bash 中定义函数也很简单，对于更复杂的逻辑来说，它可能是别名的有用替代物

```
view_map() {
    open "https://www.google.com/maps/search/$1,$2/"
}
```

例如，上面用`view_map 59.43 24.74`在塔林打开了一个谷歌地图浏览器窗口

使用`xargs`为多个参数执行一个命令:例如删除所有已经合并到 master 的本地 Git 分支。

`git branch --merged master | grep -v "master" | xargs git branch -d`

# IO 基础知识

添加到文件中

`python server.py >> server.log`

将 stdout 和 stderr 写入文件和控制台

`python run_something.py 2>&1 | tee something.log`

将所有输出写入 void(丢弃它)

`python run_something.py > /dev/null 2>&1`

# 操纵字符串

你可以用括号扩展生成一个字符串列表:`mkdir /var/project/{data,models,conf,outputs}`

Bash 内置了一些漂亮的字符串操作功能，比如

`${string//substring/replacement}`

对于字符串，使用转换将小写替换为大写

`cat lower_case_file | tr 'a-z' 'A-Z'`。

然而，还有更强大的语言，如`awk`和`sed`内置来处理任何字符串操作。您还可以将 Perl 或 Python 与正则表达式一起用于任意复杂性的字符串操作。

为了开始使用正则表达式，我发现有用的是交互式教程，如 [RegexOne](https://regexone.com/) 和操场，如 [regexr](https://regexr.com/) 。

# 操作文件

确保文件存在:

```
if [[ ! -f stuff/parent_of_files/files/necessary.file ]]; then
	mkdir -p stuff/parent_of_files/files
	touch files/necessary.file
fi
```

压缩和解压缩目录

`tar -cvzf archive_name.tar.gz content_directory`

`tar -xvzf archive_name.tar.gz -C target_directory`

合并两个。具有相同索引的 csv 文件

`join file1.csv file2.csv`

用单独的头文件合并 N .csv 文件

`cat header.csv file1.csv file2.csv ... > target_file.csv`

使用`split`将文件分成块，例如. csv 文件(保留文件头)

```
tail -n +2 file.csv | split -l 4 - split_
for file in split_*
do
    head -n 1 file.csv > tmp_file
    cat "$file" >> tmp_file
    mv -f tmp_file "$file"
done
```

从大样本中随机抽取一个样本。csv 文件(这会将它加载到内存中):

`shuf -n 10 data.csv`如果您需要保留标题，但将子样本写入另一个文件:

```
head -n 1 data.csv > data_sample.csv
shuf -n 10000 <(tail -n +2 data.csv) >> data_sample.csv
```

用`sedsed 's/;/,/g' data.csv > data.csv`改变文件分隔符

将文件发送到远程机器

`scp /Users/andrei/local_directory/conf.py user@hostname:/home/andrei/conf.py`

# 管理资源和作业

是什么占用了我所有的磁盘空间？`ncdu`提供交互式视图。不需要额外安装的替代方案是`du -hs * | sort -h`

是什么/谁让整个机器变慢了？`htop`为多彩版，`top`否则。

`uptime` -不言自明。

让一个长时间运行的模型/脚本在服务器的后台执行，并将输出写到`model.log` `nohup python long_running_model.py > model.log &`这也打印出你可以用来终止进程的 PID，如果过了一段时间你可以用`ps -ef | grep long_running_model.py`找到 PID

向本地 Flask 服务发送带有(嵌套)有效负载的请求

```
curl \
  -H 'Content-Type: application/json' \
  -X POST \
  -d '{"model_type": "neuralnet", "features": {"measurement_1": 50002.3, "measurement_2": -13, "measurement_3": 1.001}}' \
  http://localhost:5000/invoke
```

一个进程占用一个端口吗？`lsof -i :port_number`

要[干掉](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_12_01.html#sect_12_01_02)岗位，先试`kill PID`，最后再用`kill -9 PID`。

# 变量

# 基础

使用`printenv`查看您的环境变量。要设置一个永久的环境变量，将其添加到您的`~/.bash_profile` : `export MLFLOW_TRACKING_URI=http://mlflow.remoteserver`中，现在您可以在 Python 中访问`os.environ["MLFLOW_TRACKING_URI"]`。

使用`env`在自定义环境中运行命令:

```
env -i INNER_SHELL=True bash
```

使用`local`在函数中声明局部范围变量。

# 特殊参数

有几个，但是一些有用的

`$?` -最后一条命令的退出状态

`$#` -脚本的位置参数数量

`$$` -过程 ID

`$_` -外壳的绝对路径

`$0` -脚本的名称

`[[ $? == 0 ]] && echo "Last command succeeded" || echo "Last command failed"`

# 变量操作

您可以使用变量的一些技巧:

如果未设置参数，则使用默认值:`${MODEL_DIR:-"/Users/jack/work/models"}`或将其设置为另一个值:`${MODEL_DIR:=$WORK_DIR}`或显示一条错误消息:`${MODEL_DIR:?'No directory exists!'}`

# 命令行参数

`$#` -命令行参数的数量

您总是可以使用简单的`$1`、`$2`、`$3`来使用与脚本一起传递的位置参数。

对于简单的单字母命名参数，您可以使用内置的`getopts`。

```
while getopts ":m:s:e:" opt; do
    case ${opt} in
        m) MODEL_TAG="$OPTARG"
        ;;
        s) DATA_START_DATE="$OPTARG"
        ;;
        e) DATA_END_DATE="$OPTARG"
        ;;
        \?) echo "Incorrect usage!"
        ;;
    esac
done
```

# 巴利安

Python 代码可以在管道中内联运行——这意味着如果有必要，你可以用它来替换`awk` / `sed` / `perl`:

```
echo "Hello World" | python3 -c "import sys; import re; input = sys.stdin.read(); output = re.sub('Hello World', 'Privet Mir', input); print(output)"
```

或者做任何事情:

```
model_accuracy=$(python -c 'from sklearn.svm import SVC; from sklearn.multiclass import OneVsRestClassifier; from sklearn.metrics import accuracy_score; from sklearn.preprocessing import LabelBinarizer; X = [[1, 2], [2, 4], [4, 5], [3, 2], [3, 1]]; y = [0, 0, 1, 1, 2]; classif = OneVsRestClassifier(estimator=SVC(random_state=0)); y_preds = classif.fit(X, y).predict(X); print(accuracy_score(y, y_preds))';)
```

向命令提供多行输入的另一种方法是使用 here 文档，例如:

```
python <<HEREDOC
import sys
for p in sys.path:
  print(p)
HEREDOC
```

其中`HEREDOC`作为 EOF 编码，脚本仅在满足 EOF 后执行。

使用`jq`命令在命令行上使用 JSON。[例题](https://www.datascienceatthecommandline.com/2e/chapter-5-scrubbing-data.html#working-with-xmlhtml-and-json)。

# 资源

初学者 Bash 指南——介绍脚本的不错资源

[GNU Coreutils 手册](https://www.gnu.org/software/coreutils/manual/coreutils.html) —了解你可以用默认工具做什么

[命令行的数据科学](https://www.datascienceatthecommandline.com/)——如果你非常喜欢 shell，想在那里做 EDA/建模:)

感谢马克·考恩的一些提示和技巧，他实际上知道一些 Bash:)

*最初发表于 2021 年 6 月 1 日 https://mlumiste.com**的* [*。*](https://mlumiste.com/peripherals/bash-cheat-sheet/)