# 我在 15 分钟内为数据科学家总结了 100 多个命令行工具

> 原文：<https://towardsdatascience.com/i-summarized-100-command-line-tools-for-data-scientists-in-15-minutes-976def544ffe?source=collection_archive---------17----------------------->

## 数据科学家

## 所有命令行工具的概述

![](img/696e19049493a1a13679fed4aebed55e.png)

那是一个旧的命令行工具

命令行界面是以文本行的形式处理计算机程序命令的程序。在这篇精彩的文章中，我将介绍 100 多种在数据科学中广泛使用的命令行工具。

# 别名和 unalias

别名工具是内置的 Z shell。

```
$ type alias
alias is a shell builtin
```

对于命令行，语法是:

```
$ alias name=’string’
```

我们可以用它创建自己的命令。下面是将 Python 3 设置为默认的正确方法。

```
$ type python
python is /usr/bin/python$ python --version
Python 2.7.16$python3 --version
Python 3.9.4$ alias python=python3
$ python --version
Python 3.9.4$ type python
python is an alias for python3
```

要删除别名，可以使用 unalias 命令，如下所示。

```
$ unalias python
$ type python
python is /usr/bin/python
```

# Awk、sed 和 grep

为了便于讨论，让我们定义一个文本文件 list.txt:

```
$ cat list.txt
John Daggett, 341 King Road, Plymouth MA 
Alice Ford, 22 East Broadway, Richmond VA 
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK 
Terry Kalkas, 402 Lans Road, Beaver Falls PA 
Eric Adams, 20 Post Road, Sudbury MA 
```

## awk

awk 工具是一种模式扫描和文本处理语言，由 Mike D. Brennan 和 Thomas E. Dickey 开发。

```
$ type awk 
awk is /usr/bin/awk
```

awk 工具为每行输入执行一组指令。对于命令行，语法是:

```
awk 'instructions' files
```

例如，我们有一个包含姓名和地址列表的`list.txt`文件。我们编写一条指令，打印输入文件中每行的第一个字段。

```
$ awk ‘{ print $1 }’ list.txt
John
Alice
Orville
Terry
Eric
```

*阅读更多:*[*【https://invisible-island.net/mawk】*](https://invisible-island.net/mawk)

## 一项 Linux 指令

sed 工具是一个流编辑器，用于过滤和转换文本，作者是杰伊·芬拉森、汤姆·洛德、肯·皮齐尼和保罗·邦兹尼。

```
$ type sed
sed is /usr/bin/sed
```

对于命令行，语法是:

```
$ sed [-e] ‘instruction' file
```

只有在命令行上提供多条指令时，才需要`-e`选项。它告诉 sed 将下一个参数解释为指令。

以下示例使用`s`命令进行替换，将“MA”替换为“Massachusetts”

```
$ sed 's/MA/Massachusetts/' list.txt
John Daggett, 341 King Road, Plymouth Massachusetts 
Alice Ford, 22 East Broadway, Richmond VA 
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK 
Terry Kalkas, 402 Lans Road, Beaver Falls PA 
Eric Adams, 20 Post Road, Sudbury Massachusetts 
```

*阅读更多:*[*https://www.gnu.org/software/sed*](https://www.gnu.org/software/sed)

## 可做文件内的字符串查找

grep 命令搜索与正则表达式模式匹配的行，并将这些匹配的行打印到标准输出。目前由吉姆·梅耶林维护。

```
$ type grep
grep is /usr/bin/grep
```

对于命令行，语法是:

```
$ grep [options] pattern [file...]
```

*   **模式**是一个正则表达式模式，它定义了我们想要在由 FILE 参数指定的文件内容中找到什么。
*   **选项**可选参数是修改 grep 行为的标志。

假设我们想从 list.txt 中提取包含“John”文本的行。

```
$ grep “John” list.txt
John Daggett, 341 King Road, Plymouth Massachusetts
```

*阅读更多:*[*https://www.gnu.org/software/grep/*](https://www.gnu.org/software/grep/)

# 尝试

bash shell 是由 Brian Fox 和 Chet Ramey⁴.开发的 GNU Bourne-Again shell

```
$ type bash
bash is /bin/bash
```

它包含了 C shell 的大部分主要优点以及 Korn shell 的特性和一些自己的新特性。

*阅读更多:*[*【https://www.gnu.org/software/bash】*](https://www.gnu.org/software/bash)

## 历史

***历史*** 命令显示自会话开始以来输入的命令列表。目前由布莱恩·福克斯和切特·雷米维护。

```
$ type historyhistory is a shell builtin
```

例如，您现在应该会看到一个列表，其中快速显示了最近使用的 500 条命令。

```
$ history
496  ls -la
497  ls
498  history
499  ls
500  cd domains
501  cd ..
502  ls
503  history
504  cd ls
505  ls
506  cd data
507  ls
508  cd ..
509  cd domains
510  ls
511  cd ..
512  history
```

使用该命令还有更多方法。

# 公元前

bc 工具是一种支持任意精度数字和交互式执行语句的语言，作者是菲利普·a·nelson⁵.

```
**$** type bc 
bc is /usr/bin/bc
```

对于命令行，语法是:

```
$ bc [ -hlwsqv ] [long-options] [ file ... ]
```

下面将把“pi”的值赋给变量 pi。

```
$ pi=$(echo "scale=10; 4*a(1)" | bc -l)
$ echo $pi
3.1415926532
```

*阅读更多:*[*https://www.gnu.org/software/bc*](https://www.gnu.org/software/bc)

# 镉

cd 命令是 Z shell 内置的。它用来改变你正在工作的当前目录。

```
$ type cd 
cd is a shell builtin
```

对于命令行，语法是:

```
cd [option(s)] directory
```

若要导航到您的个人目录，请使用“cd ~”:

```
$ cd ~ 
$ pwd
/Users/dangtrunganh
```

要导航到根目录，请使用“cd /”:

```
$ cd /
$ pwd
/
```

若要向上导航一级目录，请使用“cd ..”：

```
$ pwd
/Users/junryo$ cd ..
$ pwd 
/Users
```

# Util Linux

***util-linux*** 库是由 linux 内核组织发布的标准包，用作 Linux 操作系统的一部分。

*阅读更多:【https://www.kernel.org/pub/linux/utils/util-linux】<https://www.kernel.org/pub/linux/utils/util-linux>*

## *圆柱*

*Linux 中的 column 命令用于以列的形式显示文件的内容。它由卡雷尔·扎克维护。*

```
*$ type column
column is /usr/bin/column*
```

*对于命令行，语法是:*

```
*column [-entx] [-c columns] [-s sep] [file ...]*
```

*例如，您希望将由特定分隔符分隔的条目排序到不同的列中。*

```
*$ column -t -s “|” list.txt
1 Anh Dang
2 Linh Hong*
```

## *发动机的旋转*

*逐字符反转线条。*

*例如:*

```
*$ echo ‘name: Anh Dang’ | rev 
gnaD hnA :eman*
```

# *考赛*

*这个工具是托尼·门罗开发的一个程序，可以生成带有信息的奶牛的 ASCII 艺术图片。*

```
*$ type cowsay 
cowsay is /usr/bin/cowsay*
```

*Unix 命令 ***fortune*** 也可以通过管道传输到 ***cowsay*** 命令中:*

```
*$ fortune | cowsay
 ________________________________________
/ You have Egyptian flu: you're going to \
\ be a mummy.                            /
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||*
```

*在 GPLv3 许可证下可以获得 ***cowsay*** 工具，您可以在 GitHub 上找到 Perl 源代码。*

**阅读更多:*[*https://github.com/tnalpgge/rank-amateur-cowsay*](https://github.com/tnalpgge/rank-amateur-cowsay)*

# *科里蒂尔斯*

*GNU 核心工具是 GNU 操作系统的基本文件、外壳和文本操作工具。这些是每个操作系统中都应该存在的核心实用程序。*

**阅读更多:*<https://www.gnu.org/software/coreutils>**。***

## **chmod**

**chmod 命令用于更改权限，由大卫·麦肯齐和吉姆·meyerin⁶.维护**

```
**$ type chmod 
chmod is /usr/bin/chmod**
```

**只有文件或文件夹的所有者才能更改其模式。**

## **丙酸纤维素**

*****cp*** 命令用于复制文件或文件组或目录。它由托尔比约恩·格兰伦德、大卫·麦肯齐和吉姆·迈耶林维护。**

```
**$ type cp 
cp is /usr/bin/cp**
```

**这里有五个命令可以让您在 Linux 终端中查看文件的内容，如下所示:cat、nl、less、head 和 tail。**

## **猫**

**这是 Torbjorn Granlund 和 Richard M. Stallman⁶.编写的在 Linux 中查看文件的最简单也可能是最流行的命令**

```
**$ type cat 
cat is /usr/bin/cat**
```

**下面是如何保存一个日期范围内的所有日志文件，如下所示。**

```
**$ cat localhost_log_file.2017-{09-{03..30},10-{01..08}}.txt > totallog.csv**
```

**cat 命令的大问题是它在屏幕上显示文本。**

## **北纬**

**nl 命令几乎和 cat 命令一样，由斯科特·bartram⁷.维护**

```
**$ type nl 
nl is /usr/bin/nl**
```

**唯一的区别是，在终端中显示文本时，它会预先考虑行号。**

```
**$ nl main.js 
1 console.log([…[1, 2, 3], …[4, 5, 6]])%**
```

**有时，您并不需要某个命令的所有输出。您可能只需要前几行或后几行。**

## **头**

*****head*** 命令打印文件的前 10 行。**

```
**$ ls /usr/bin | head -n 5
2to3-
2to3–2.7
AssetCacheLocatorUtil
AssetCacheManagerUtil
AssetCacheTetheratorUtil**
```

## **尾巴**

**一个 ***尾*** 命令打印最后 10 行。**

```
**$ ls /usr/bin | tail -n 5
zipsplit
zless
zmore
znew
zprint**
```

## **限位开关（Limit Switch）**

*****ls*** 命令可能是使用最多的命令。它列出了目录内容。**

```
**$ type ls
ls is /bin/ls**
```

**例如，我们列出了用户的主目录(用~符号表示)和/usr 目录。**

```
**$ ls ~ /usr**
```

## **切口**

*****cut*** 程序用于从一行中提取一段文本，并将提取的部分输出到标准输出。它可以接受多个文件参数或来自标准输入的输入。**

```
**$ type cut
cut is /usr/bin/cut**
```

## **回声**

*****echo*** 命令用于显示作为参数传递的文本/字符串行。**

```
**$ type echo 
echo is a shell builtin**
```

**对于命令行，语法是:**

```
**$ echo [option] [string]**
```

**例如:**

```
**$ echo "TowardsDataScience"**
```

## **包封/包围（动词 envelop 的简写）**

**显示当前环境或设置执行命令的环境。**

```
**$ type env 
env is /usr/bin/env**
```

**对于命令行，语法是:**

```
**$ env [ -i | — ] [*Name*=*Value* ]… [*Command* [ *Argument* … ] ]**
```

**例如，要在运行 **date** 命令时更改 **TZ** 环境变量，请键入:**

```
**env TZ=MST7MDT date**
```

## **折叠**

**Linux 中的 *fold* 命令将输入文件中的每一行换行以适应指定的宽度，并将其打印到标准输出。默认情况下，它以最大 80 列的宽度换行，但这是可以配置的。**

```
**$ type fold 
fold is /usr/bin/fold**
```

**对于命令行，语法是:**

```
**$ fold [  -b ] [  -s ] [  -w Width] [ File... ]**
```

**例如，要将名为 longlines 的文件的行折叠成宽度 72(72)，请输入:**

```
**$ fold -w 72 longlines**
```

## **mkdir**

**创建一个或多个新目录。**

```
**$ type mkdir 
mkdir is /usr/bin/mkdir $ man mkdir** 
```

**对于命令行，语法是:**

```
**$ mkdir [-e] [ -m *Mode* ] [ -p ] *Directory …***
```

**例如:**

```
**$ mkdir -p /mybook/chapter{01..10}**
```

# **Dsutils**

*****dsutils*** 是用于数据科学的命令行工具列表。作者是耶鲁安·扬森斯和李维·吉列。**

***阅读更多:*[*https://github.com/jeroenjanssens/dsutils*](https://github.com/jeroenjanssens/dsutils)。**

## **身体**

*****body*** 命令允许您将任何命令行工具应用于 CSV 文件的主体。**

```
**$ type body 
body is /usr/bin/dsutils/body**
```

**例如:**

```
**$ echo -e “value\n8\n2\n6\n3” | body sort -n 
value 
2 
3 
6 
8**
```

## **关口**

**此工具允许您对列的子集应用命令。**

```
**$ type cols
cols is /usr/bin/dsutils/cols**
```

## **csv2vw**

*****csv vvw***工具将 CSV 转换为 Vowpal Wabbit 格式。**

```
**$ type csv2vw 
csv2vw is /usr/bin/dsutils/csv2vw**
```

## **dseq**

**命令 ***dseq*** 生成一个日期序列。**

```
**$ type dseq 
dseq is /usr/bin/dsutils/dseq** 
```

**例如，你使用***dseq***⁠command 生成一个`dates.txt`文件。**

```
**$ dseq 5 > dates.txt
2021–08–01 
2021–08–02 
2021–08–03
2021-08-04
2021-08-05**
```

## **页眉**

*****标题*** 命令用于添加、替换和删除 CSV 文件的标题行。**

```
**$ type header 
header is /usr/bin/dsutils/header**
```

**对于命令行，语法是:**

```
**$ header [-adr] text**
```

**下面举个例子，如何统计下面 CSV 的正文的行。**

```
**$ seq 5 | header -a count | body wc -l 
count 
5**
```

## **中国人民银行(People's Bank of China)**

*****pbc*** 命令是并行 ***bc*** 。**

```
**$ type pbc 
pbc is /usr/bin/dsutils/pbc** 
```

**例如:**

```
**$ seq 5 | pbc ‘{1}²’ 
1 
4 
9
16
25**
```

## **servewd**

**该工具使用一个简单的 HTTP 服务器提供当前工作目录。**

```
**$ type servewd 
servewd is /usr/bin/dsutils/servewd** 
```

**例如:**

```
**$ cd /data && servewd 8080**
```

## **整齐**

*****修剪*** 命令用于限制输出到给定的高度和宽度。**

```
**$ type trim 
trim is /usr/bin/dsutils/trim**
```

**例如:**

```
**$ echo {a..z}-{0..9} | fold | trim 5 60 
a-0 a-1 a-2 a-3 a-4 a-5 a-6 a-7 a-8 a-9 b-0 b-1 b-2 b-3 b-4… 
c-0 c-1 c-2 c-3 c-4 c-5 c-6 c-7 c-8 c-9 d-0 d-1 d-2 d-3 d-4… 
e-0 e-1 e-2 e-3 e-4 e-5 e-6 e-7 e-8 e-9 f-0 f-1 f-2 f-3 f-4… 
g-0 g-1 g-2 g-3 g-4 g-5 g-6 g-7 g-8 g-9 h-0 h-1 h-2 h-3 h-4… 
i-0 i-1 i-2 i-3 i-4 i-5 i-6 i-7 i-8 i-9 j-0 j-1 j-2 j-3 j-4… 
… with 8 more lines**
```

## **解除…的负担**

*****解包*** 工具用于提取 tar、rar、zip 等常见文件格式。**

```
**$ type unpack 
unpack is /usr/bin/dsutils/unpack**
```

**例如:**

```
**$ unpack logs.tar.gz**
```

**请记住，它会查看您想要解压缩的文件的扩展名，并调用适当的命令行工具。**

# **Csvkit**

*****csvkit*** repo 是一套命令行工具，用于转换和处理 CSV，表格文件格式之王。目前由 Christopher Groskopf 维护。**

***阅读更多:*[*https://csvkit.rtfd.org*](https://csvkit.rtfd.org)*。***

*****csvkit*** 工具让你的生活更轻松。**

*   **将 Excel 转换为 CSV:**

```
**$ in2csv data.xls > data.csv**
```

*   **将 JSON 转换为 CSV:**

```
**$ in2csv data.json > data.csv**
```

*   **打印列名:**

```
**$ csvcut -n data.csv**
```

*   **选择列的子集:**

```
**$ csvcut -c column_a,column_c data.csv > new.csv**
```

*   **重新排序列:**

```
**$ csvcut -c column_c,column_a data.csv > new.csv**
```

*   **查找具有匹配单元格的行:**

```
**$ csvgrep -c phone_number -r "555-555-\d*{4}*" data.csv > new.csv**
```

*   **转换为 JSON:**

```
**$ csvjson data.csv > data.json**
```

*   **生成汇总统计数据:**

```
**$ csvstat data.csv**
```

*   **使用 SQL 查询:**

```
**$ csvsql --query "select name from data where age > 30" data.csv > new.csv**
```

*   **导入 PostgreSQL:**

```
**$ csvsql --db postgresql:///database --insert data.csv**
```

*   **从 PostgreSQL 提取数据:**

```
**$ sql2csv --db postgresql:///database --query "select * from data" > new.csv**
```

# **卷曲**

**一个 ***curl*** 是一个命令行工具和库，用于通过 url 传输数据。这是一个免费的开源软件，它的存在要感谢成千上万的贡献者**

```
**$ type curl 
curl is /usr/bin/curl**
```

**例如:**

```
**curl https://www.keycdn.com**
```

***阅读更多:*[*https://curl . se*](https://curl.se)**

# **出口**

**将环境导出到随后执行的程序。它是一个内置的 Z 外壳。**

```
**$ type export 
export is a reserved word**
```

**对于命令行，语法是:**

```
**$ export [-f] [-n] [name[=value] ...]** 
```

**例如:**

```
**$ export $PATH:$HOME/bin**
```

**查看当前 shell 中所有导出的变量。**

```
**$ export -p**
```

**查看所有导出的变量。**

```
**$ export**
```

# **足球俱乐部（Football Club）**

*****fc*** 命令是 Z shell 内置的。这是一个命令行实用程序，用于列出、编辑和重新执行以前输入到交互式 shell 中的命令。**

```
**$ type fc
fc is a shell builtin**
```

# **发现**

**find 命令在目录层次结构中搜索文件。它是由埃里克·b·德克尔、詹姆斯·杨曼和凯文·戴利写的。**

```
**$ type find 
find is /usr/bin/find**
```

**例如:**

```
**find /Volumes/MyData -type f -name ‘*.csv’ -size -3**
```

***阅读更多:*[*https://www.gnu.org/software/findutils*](https://www.gnu.org/software/findutils)**

# **为**

**命令的 ***是 Z shell 内置的。它允许代码重复执行。*****

```
**$ type for
for is a reserved word**
```

**例如:**

```
**$ for i in $(seq 1 2 20)
do
   echo “Welcome $i times”
done
Welcome 1 times
Welcome 3 times
Welcome 5 times
Welcome 7 times
Welcome 9 times
Welcome 11 times
Welcome 13 times
Welcome 15 times
Welcome 17 times
Welcome 19 times**
```

# **外汇（foreign exchange 的缩写）**

*****fx*** 库是 Anton Medvedev 开发的命令行工具和终端 JSON 查看器。**

```
**$ type fx 
fx is /usr/local/bin/fx**
```

**下面介绍如何使用 ***fx*** 命令。**

*   **启动[交互模式](https://github.com/antonmedv/fx/blob/master/DOCS.md#interactive-mode)而不传递任何参数。**

```
**$ curl ... | fx**
```

*   **或者将文件名作为第一个参数传递。**

```
**$ fx data.json**
```

*   **传递几个 JSON 文件。**

```
**cat foo.json bar.json baz.json | fx .message**
```

*   **充分利用 JavaScript 的力量。**

```
**$ curl ... | fx '.filter(x => x.startsWith("a"))'**
```

*   **使用[访问所有 lodash(或 ramda 等)方法。fxrc](https://github.com/antonmedv/fx/blob/master/DOCS.md#using-fxrc) 文件。**

```
**$ curl ... | fx '_.groupBy("commit.committer.name")' '_.mapValues(_.size)'**
```

*   **使用 spread 运算符更新 JSON。**

```
**$ echo '{"count": 0}' | fx '{...this, count: 1}'
{
  "count": 1
}**
```

*   **从地图中提取值。**

```
**$ fx commits.json | fx .[].author.name**
```

*   **将格式化的 JSON 打印到 stdout。**

```
**$ curl ... | fx .**
```

*   **通过管道将 JSON 日志流导入 fx。**

```
**$ kubectl logs ... -f | fx .message**
```

*   **试试这个:**

```
**$ fx --life**
```

***阅读更多:*[*https://github.com/antonmedv/fx*](https://github.com/antonmedv/fx)**

# **饭桶**

**愚蠢的内容跟踪器。**

```
**$ type git 
git is /usr/bin/git**
```

**对于命令行，语法是:**

```
**$ git [--version] [--help] [-C <path>] [-c <name>=<value>]
    [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
    [-p|--paginate|-P|--no-pager] [--no-replace-objects] [--bare]
    [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
    [--super-prefix=<path>] [--config-env=<name>=<envvar>]
    <command> [<args>]**
```

***阅读更多:*[*https://git-scm.com*](https://git-scm.com)**

# **格朗**

**让 JSON greppable！它是由汤姆·哈德森维护的。**

```
**$ type gron 
gron is /usr/bin/gron**
```

**对于命令行，语法是:**

```
**$ gron [options] [file|URL|-]**
```

**例如:**

```
**$ gron "https://api.github.com/repos/tomnomnom/gron/commits?per_page=1" | fgrep "commit.author"
json[0].commit.author = {};
json[0].commit.author.date = "2016-07-02T10:51:21Z";
json[0].commit.author.email = "mail@tomnomnom.com";
json[0].commit.author.name = "Tom Hudson";**
```

***阅读更多:*[*https://github.com/TomNomNom/gron*](https://github.com/TomNomNom/gron)**

# **主机名**

**hostname 命令显示或设置系统的主机名。它是由彼得·托拜厄斯，贝恩德·艾肯费尔斯和迈克尔·梅斯克斯写的。**

```
****$** type hostname
hostname is /usr/bin/hostname**
```

**例如:**

```
**$ hostname
Trungs-MacBook-Pro.local**
```

***阅读更多:*[*https://sourceforge.net/projects/net-tools/*](https://sourceforge.net/projects/net-tools/)**

# **JQ**

*****jq*** 库是一个轻量级且灵活的命令行 JSON 处理器，由 Stephen Dolan 编写。**

```
**$ type jq
jq is /usr/bin/jq**
```

**例如:**

```
**$ curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq '.[0]'**
```

***阅读更多:*[*https://stedolan.github.io/jq*](https://stedolan.github.io/jq)**

# **Json2csv**

**将换行符分隔的 json 数据流转换为 csv 格式。**

```
**$ type jq 
jq is /usr/bin/jq**
```

**对于命令行，语法是:**

```
**$ json2csv
    -k fields,and,nested.fields,to,output
    -i /path/to/input.json (optional; default is stdin)
    -o /path/to/output.csv (optional; default is stdout)
    --version
    -p print csv header row
    -h This help**
```

**例如**

*   **要转换:**

```
**{“user”: {“name”:”jehiah”, “password”: “root”}, “remote_ip”: “127.0.0.1”, “dt” : “[20/Aug/2010:01:12:44 -0400]”}
{“user”: {“name”:”jeroenjanssens”, “password”: “123”}, “remote_ip”: “192.168.0.1”, “dt” : “[20/Aug/2010:01:12:44 -0400]”}
{“user”: {“name”:”unknown”, “password”: “”}, “remote_ip”: “76.216.210.0”, “dt” : “[20/Aug/2010:01:12:45 -0400]”}**
```

*   **收件人:**

```
**"jehiah","127.0.0.1"
"jeroenjanssens","192.168.0.1"
"unknown","76.216.210.0"**
```

*   **您可以:**

```
**$ cat input.json | json2csv -k user.name,remote_ip > output.csv**
```

***阅读更多:*[*https://github.com/jehiah/json2csv*](https://github.com/jehiah/json2csv)**

# **制造**

**GNU Make 是一个工具，它控制从程序的源文件生成程序的可执行文件和其他非源文件。**

```
**$ type make
make is /usr/bin/make**
```

***阅读更多:*[*https://www.gnu.org/software/make/*](https://www.gnu.org/software/make/)**

# **男人**

**系统参考手册的界面。**

```
**$ type man 
man is /usr/bin/man**
```

**例如:**

```
**$ man ls**
```

***阅读更多:*[*https://nongnu.org/man-db/*](https://nongnu.org/man-db/)**

# **毫微；纤（10 的负九次方）**

**GNU nano 被设计成 Pico 文本编辑器的免费替代品，Pico 文本编辑器是华盛顿大学 Pine 电子邮件套件的一部分。**

```
**$ type nano 
nano is /usr/bin/nano**
```

***阅读更多:*[*https://nano-editor.org*](https://nano-editor.org)**

# **平行的**

**GNU ***parallel*** 是使用一台或多台计算机并行执行作业的 shell 工具。**

```
**$ type parallel 
parallel is /usr/bin/parallel** 
```

**例如:**

```
**$ seq 3 | parallel “echo Processing file {}.csv” 
Processing file 1.csv 
Processing file 2.csv 
Processing file 3.csv**
```

***阅读更多:*[*https://www.gnu.org/software/parallel/*](https://www.gnu.org/software/parallel/)**

# **点**

**它是一个安装和管理 Python 包的工具。**

```
**$ type pip 
pip is /usr/bin/pip**
```

**例如:**

```
**$ pip install EvoCluster$ pip freeze | grep Evo
EvoCluster==1.0.4**
```

# **小狗**

*****pup*** 库是处理 HTML 的命令行工具。它从标准输入中读取，打印到标准输出，并允许用户使用 CSS 选择器过滤页面的部分内容。**

```
**$ type pup 
pup is /usr/bin/pup**
```

**对于命令行，语法是:**

```
**$ cat index.html | pup [flags] '[selectors] [display function]'**
```

# **计算机编程语言**

**Python 是一种编程语言，可以让您快速工作并更有效地集成系统。**

```
**$ type python
python is /usr/bin/python**
```

**对于命令行，语法是:**

```
**$ python [-bBdEhiIOqsSuvVWx?] [-c command | -m module-name | script | - ] [args]**
```

***阅读更多:*[*https://www.python.org*](https://www.python.org)**

# **稀有**

**r 是一个用于统计计算和图形的自由软件环境。它可以在多种 UNIX 平台、Windows 和 MacOS 上编译和运行。**

```
**$ type R 
R is /usr/bin/R**
```

***阅读更多:*[*https://www.r-project.org*](https://www.r-project.org)**

# **冲**

*****rush*** 库是一个 R 包，可以让你直接从 shell 中运行表达式，创建 plots，安装包。**

```
**$ type rush 
rush is /usr/local/lib/R/site-library/rush/exec/rush**
```

**例如:**

```
**$ rush run 6*7
42**
```

***阅读更多:*[*https://github.com/jeroenjanssens/rush*](https://github.com/jeroenjanssens/rush)**

# **样品**

**在给定的延迟和一定的持续时间内，根据某种概率从标准输入中过滤掉行。**

```
**$ type sample 
sample is /usr/local/bin/sample**
```

**例如:**

```
**$ time seq -f "Line %g" 1000000 | sample -r 1% -d 1000 -s 5
Line 71
Line 250
Line 305
Line 333
Line 405
Line 427
seq -f "Line %g" 1000000  0.01s user 0.00s system 0% cpu 5.092 total
sample -r 1% -d 1000 -s 5  0.06s user 0.02s system 1% cpu 5.091 total**
```

***阅读更多:*[*https://github.com/jeroenjanssens/sample*](https://github.com/jeroenjanssens/sample)**

# **XML2json**

**使用 xml 映射 npm 模块将 XML 输入转换为 JSON 输出的命令。**

```
**$ type xml2json 
xml2json is /usr/local/bin/xml2json**
```

**对于命令行，语法是:**

```
**$ xml2json < input.xml > output.json**
```

***阅读更多:*[*https://github.com/Inist-CNRS/node-xml2json-command*](https://github.com/Inist-CNRS/node-xml2json-command)**

# **XMLStarlet**

*****XML stallet***库是一组命令行实用程序(工具),使用一组简单的 shell 命令来转换、查询、验证和编辑 XML 文档和文件，与使用 UNIX grep、sed、awk、diff、patch、join 等实用程序处理文本文件的方式类似。**

```
**$ type xmlstarlet 
xmlstarlet is /usr/bin/xmlstarlet**
```

# **xsv**

**用 Rust 编写的快速 CSV 命令行工具包。**

```
**$ type xsv 
xsv is /usr/bin/xsv**
```

**例如:**

```
**$ curl -LO [https://burntsushi.net/stuff/worldcitiespop.csv](https://burntsushi.net/stuff/worldcitiespop.csv)$ xsv headers worldcitiespop.csv
1   Country
2   City
3   AccentCity
4   Region
5   Population
6   Latitude
7   Longitude**
```

***阅读更多:*[*https://github.com/BurntSushi/xsv*](https://github.com/BurntSushi/xsv)**

# **祖蒂尔斯**

**Zutils 是一个工具集，能够透明地处理任何压缩和未压缩文件的组合。**

**更多阅读:[https://www.nongnu.org/zutils/zutils.html](https://www.nongnu.org/zutils/zutils.html)**

## **zcat**

**解压缩并将文件复制到标准输出。**

## **zcmp**

**解压缩并逐字节比较两个文件。**

## **zdiff**

**逐行解压缩并比较两个文件。**

## **zgrep**

**解压缩并在文件中搜索正则表达式。**

## **ztest**

**测试压缩文件的完整性。**

## **zupdate**

**将文件重新压缩为 lzip 格式。**

# **zsh**

**Zsh 是一个为交互使用而设计的 shell，尽管它也是一种强大的脚本语言。**

```
**$ type zsh 
zsh is /usr/bin/zsh**
```

***阅读更多:*[*https://www.zsh.org*](https://www.zsh.org)**

**太神奇了！100 多个命令。对于那些每天使用命令行工具处理数据的人来说，这篇文章非常有用。**