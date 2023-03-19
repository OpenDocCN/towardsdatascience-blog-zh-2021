# 如何在 2021 年建立一个通用的 Lisp IDE

> 原文：<https://towardsdatascience.com/how-to-set-up-common-lisp-ide-in-2021-5be70d88975b?source=collection_archive---------10----------------------->

## 使用 Roswell 在 Windows 10、MacOS 或 Ubuntu 中快速启动 Common Lisp

![](img/b45b2e6b71d66aee14e0c53a061d2c81.png)

克里斯蒂娜·莫里路穿着 Pexels 的照片

如果“以错误的方式”建立一个公共 Lisp (CL)开发环境是乏味且耗时的。

它涉及手动安装和设置:

1.  一个常见的 Lisp 实现——通常是`sbcl`
2.  `emacs`—Common Lisp 中编码的首选非商业编辑器，
3.  `quicklisp` —通用 Lisp 的黄金标准包管理器，以及
4.  `slime`—`emacs`和 CL 实施`sbcl`之间的功能胶水

这个过程可能会让人不知所措，而且充满陷阱，尤其是对初学者来说。

不过，很幸运，有一个度假村:`Roswell`！

`Roswell`是一个现代的虚拟环境和 Common Lisp 的包管理器，它不仅允许安装 Common Lisp 的不同实现(如`sbcl`、`ecl`、`allegro`等)。)而且将它们中的每一个的不同版本放入单独的虚拟环境中。`Roswell`允许在不同的实现和版本之间切换，这对于普通的 Lisp 包开发者来说是必不可少的。因此，Roswell 是任何认真的 Common Lisp 开发人员的最好朋友。

此外，Roswell 允许建立一个测试环境并集成 CI(见[此处](https://roswell.github.io/Travis-CI.html))。

## 在 Windows 10 中安装 Roswell(PowerShell)

在 Windows 10 (PowerShell)中安装 Roswell 非常简单:

```
# 0\. install scoop - which is kind of `apt` for windows
iwr -useb get.scoop.sh | iex# 1\. install roswell using scoop
scoop install roswell
```

仅此而已！接下来的所有命令也可以在 PowerShell 上完成——尽管我在代码行的开头写了`$`,表示通常是来自 Linux 或 MacOS 的 bash shell(在 PowerShell 中安装 scoop 之后，您也可以在 cmd.exe 中运行 scoop——但是我总是在 Windows 中使用 PowerShell)。

在后面的阶段，可能会出现以下错误

```
ros install sbcl
```

可以肯定的是`scoop install`单独运行，它安装了一个二进制版本的`sbcl (sbcl-bin)`，但是它不支持多线程。

我意识到

```
ros install msys2
```

然后运行初始配置

```
msys2
```

之后，重新启动 powershell，然后输入

```
ros install sbcl
```

应该有效——但事实并非如此。问题出在罗斯威尔的`msys2`安装脚本中。如果它使用`scoop install`而不是像它实际做的那样从 sourceforge 拉`msys2`的源代码，它会工作。所以目前`ros`相当破。然而，`ros install`至少安装了不需要`msys2`的`sbcl-bin`。但是，一个人只有 aked `sbcl-bin`。一个人甚至不能安装`ros install slime`也不能安装`ros install clisp`。因为那些是需要`msys2`的。很抱歉，罗斯威尔的作者必须修复`msys2`的安装。在此之前，您只能通过激活 windows 中的`wsl2`并在其中安装`ubuntu`来使用 roswell。并使用 Linux 的指令。

## 在 MacOS 中安装 Roswell

简单如:

```
$ brew install roswell
```

## 在 Ubuntu 20.04 LTS 版(以及更老的 Ubuntu 版本)中安装 Roswell

```
# Following instructions in github of roswell:# system-wide installation of roswell:sudo apt-get -y install git build-essential automake libcurl4-openssl-dev
git clone -b release https://github.com/roswell/roswell.git
cd roswell
sh bootstrap
./configure
make
sudo make install
ros setup# local installation of roswell:git clone -b release https://github.com/roswell/roswell.git
cd roswell
sh bootstrap
./configure --prefix=$HOME/.local
make
make install
echo 'PATH=$HOME/.local/bin:$PATH' >> ~/.profile
PATH=$HOME/.local/bin:$PATH 
ros setup
```

或者，您也可以遵循以下步骤:

```
# install dependencies for ubuntu
$ sudo apt install libcurl4-openssl-dev automake# download Roswell installation script $ curl -L [https://github.com/roswell/roswell/releases/download/v19.08.10.101/roswell_19.08.10.101-1_amd64.deb](https://github.com/roswell/roswell/releases/download/v19.08.10.101/roswell_19.08.10.101-1_amd64.deb) --output roswell.deb# or just for the latest debian package:
$ curl -sOL `curl -s https://api.github.com/repos/roswell/roswell/releases/latest | jq -r '.assets | .[] | select(.name|test("\\\.deb$")) | .browser_download_url'`# run the installation
$ sudo dpkg -i roswell.deb# add roswell to PATH to your ~/.bashrc 
# (important for scripts to run correctly!)
export PATH="$HOME/.roswell/bin:$PATH"# don't forget to source after modifying your ~/.bashrc:
$ source ~/.bashrc
```

## 安装罗斯威尔后

一旦 Roswell 安装完毕，我们可以通过以下方式检查可用的公共 Lisp 实现:

```
$ ros install # this prints currently:Usage:To install a new Lisp implementaion:
   ros install impl [options]
or a system from the GitHub:
   ros install fukamachi/prove/v2.0.0 [repository... ]
or an asdf system from quicklisp:
   ros install quicklisp-system [system... ]
or a local script:
   ros install ./some/path/to/script.ros [path... ]
or a local system:
   ros install ./some/path/to/system.asd [path... ]For more details on impl specific options, type:
   ros help install implCandidates impls for installation are:
abcl-bin
allegro
ccl-bin
clasp-bin
clasp
clisp
cmu-bin
ecl
mkcl
sbcl-bin
sbcl
sbcl-source
```

## 安装不同的公共 Lisp 实现和版本

```
# these are examples how one can install specific implementations and versions of them:
$ ros install sbcl-bin      # default sbcl
$ ros install sbcl          # The newest released version of sbcl
$ ros install ccl-bin       # default prebuilt binary of ccl
$ ros install sbcl/1.2.0    # A specific version of sbcl
```

我推荐安装最新的`sbcl`，因为`sbcl-bin`似乎不支持多线程:

`$ ros install sbcl`

要列出已经安装的实现和版本，请执行以下操作:

```
$ ros list installed  # Listing all installed implementations
```

## 在不同的实现及其版本之间切换

检查当前活动的实现/版本:

```
$ ros run -- --version      # check which implementation is used
SBCL 1.2.15
```

切换到另一个实现/版本:

```
$ ros use sbcl/2.1.7 # change the version number if newer available!
```

## 启动 REPL(使用“rlwrap ”)

```
$ ros run
# or better:
$ rlwrap ros run # it starts sbcl or whatever implementation 
                 # recently determined by `$ ros use` command.
```

`rlwrap` -ing `sbcl`很有帮助，因为`sbcl` REPL 的“裸机”不允许行内跳转或者其他有用的编辑命令哪一个是使用 ubuntu shell REPL 时习惯的。

## 使用 Roswell 安装公共 Lisp 包

对于 quicklisp 的`ql:quickload`功能可用的任何包，您现在都可以使用当前激活的实现的`roswell`从命令行安装:

```
# from the quicklisp package repository for CL
$ ros install rove # rove is a test package for CL# or from github:
$ ros install fukamachi/rove# later update your package by:
$ ros update rove
```

## 安装 emacs 和 slime 并与 Roswell 连接

## A.简单的方法(但目前已被打破):

通常，emacs 可以在连接并运行 quicklisp】的情况下，通过`Roswell`中的单个命令进行安装和设置，quicklisp 是日本开发者`cxxxr`为`CL`开发的一种特殊模式。

```
$ ros install cxxxr/lem
# and then start `lem` by:
$ lem
```

不幸的是，目前`lem`安装存在问题。

因此，必须手动安装`emacs`，并手动将其与罗斯威尔连接。

## B.手动方式(在罗斯威尔的帮助下):

0.全球安装 emacs:

```
$ sudo apt install emacs
# then start emacs in the background
$ emacs &
```

1.  在罗斯威尔内部安装史莱姆和斯旺克:

```
$ ros install slime
$ ros install swank
```

看来这还不够。用户还必须通过`M-x install-package`并选择`slime`来安装到 emacs `slime`中。

2.通过配置`~/.emacs.d/init.el`中 emacs 的标准配置文件，在 emacs 中配置通过 slime 到 Roswell 的连接。

在 emacs 中，通过按下`C-x C-f`，键入`~/.emacs.d/init.el`，并按下回车键`RET`，打开`~/.emacs.d/init.el`。

(Emacs 快捷键符号:

*   `C`是`Ctrl`
*   `M`是`Alt`
*   `S`是`Shift`
*   `SPC`是`Space`
*   `RET`就是`Return`。
*   像 Ctrl 键被按下的同时按键`x`的组合将是`C-x`，并且

`C-x C-f`例如:按住 Ctrl 的同时按下`x`，然后松开两者，然后按住 Ctrl，同时`f`，然后松开两者)。

打开`~/.emacs.d/init.el`后，按照[维基中针对罗斯威尔](https://github.com/roswell/roswell/wiki/Initial-Recommended-Setup)的说明，我们编写:

```
;; initialize/activate emacs package management
(require 'package)
(setq package-enable-at-startup nil)
(setq package-archives '());; connect with melpa emacs lisp package repository
(add-to-list 'package-archives '("melpa"     . "[http://melpa.org/packages/](http://melpa.org/packages/)") t);; initialization of package list
(package-initialize)
(package-refresh-contents);; Ensure `use-package` is installed - install if not                                                                                        
(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package));;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;                                                                                       
;; slime for common-lisp                                                                                               
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;; to connect emacs with roswell
(load (expand-file-name "~/.roswell/helper.el"));; for connecting slime with current roswell Common Lisp implementation
(setq inferior-lisp-program "ros -Q run");; for slime;; and for fancier look I personally add:
(setq slime-contribs '(slime-fancy));; ensure correct indentation e.g. of `loop` form
(add-to-list 'slime-contribs 'slime-cl-indent);; don't use tabs
(setq-default indent-tabs-mode nil);; set memory of sbcl to your machine's RAM size for sbcl and clisp
;; (but for others - I didn't used them yet)
(defun linux-system-ram-size ()
  (string-to-number (shell-command-to-string 
                     "free --mega | awk 'FNR == 2 {print $2}'")))(setq slime-lisp-implementations 
   `(("sbcl" ("sbcl" "--dynamic-space-size"
                     ,(number-to-string (linux-system-ram-size))))
     ("clisp" ("clisp" "-m"
                       ,(number-to-string (linux-system-ram-size))
                       "MB"))
     ("ecl" ("ecl"))
     ("cmucl" ("cmucl"))));;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; slime for common-lisp using use-package
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;(use-package slime
    :ensure t
    :config
    (load (expand-file-name "~/.roswell/helper.el"))
    ;; $ ros config
    ;; $ ros use sbcl dynamic-space-size=3905
    ;; query with: (/ (- sb-vm:dynamic-space-end sb-vm:dynamic-space-start) (expt 1024 2));; set memory of sbcl to your machine's RAM size for sbcl and clisp
    ;; (but for others - I didn't used them yet)
    (defun linux-system-ram-size ()
      (string-to-number (shell-command-to-string "free --mega | awk 'FNR == 2 {print $2}'")))
    ;; (linux-system-ram-size) (setq inferior-lisp-program (concat "ros -Q dynamic-space-size="     
                                      (number-to-string (linux-system-ram-size)) 
                                      " run")) ;; and for fancier look I personally add:
    (setq slime-contribs '(slime-fancy)) ;; ensure correct indentation e.g. of `loop` form
    (add-to-list 'slime-contribs 'slime-cl-indent) ;; don't use tabs
    (setq-default indent-tabs-mode nil))
```

通过以下方式在 emacs 中保存并关闭文件

*   `C-x C-s`(保存当前缓冲区/文件)和
*   `C-x C-c`(关闭 emacs)。

3.最后，重新启动 emacs:

```
$ emacs &
```

要尝试 emacs 和当前 Roswell 激活的实现之间的连接，创建一个`test.lisp`文件:

*   (在 emacs 中打开/创建文件)`C-x C-f`然后输入:`test.lisp`并按下`RET`。

(打开现有的 lisp 文件:`C-x C-f`，输入 lisp 文件的路径，按`RET`)。

在 emacs 中的 lisp 内部，通过按下`M-x`并键入:slime 来启动`slime`，然后按下`RET`来执行命令。

当前`test.lisp`文件(emacs 缓冲区)下的一个新标签(emacs 缓冲区)应在 emacs 中打开，并且粘液 REPL 应可见:

```
; SLIME 2.26.1 (or whatever version number you have ...)
CL-USER>
```

这是 Roswell 中当前激活的 lisp 实现的交互式 REPL(可通过`$ ros use`命令随时切换，但不适用于已经运行的 emacs 会话；每次`$ ros use`切换后，可能需要重启 emacs)。

最好的事情是，现在，`test.lisp`已经(通过`slime`)与 Roswell 中当前的 CL 实现相连接。您可以将指针指向`test.lisp`文件中任何 lisp 表达式的末尾，然后按下`C-x C-c` : emacs 会将该表达式转发给 Common Lisp 的连接实现(在我们的例子中是`sbcl`)并执行该表达式。

通过`vim`或`atom`而不是`emacs`进行连接，查看罗斯威尔 GitHub 网站的`wiki`。

在下文中，我们希望了解如何创建公共 Lisp 包(或项目)，如何设置测试，尤其是 Travis CI 的自动化测试，以及如何监控测试的代码覆盖率。

# 使用 cl-projects 启动公共 Lisp 包/项目

可以使用`cl-project`自动生成一个包主干。

通过`Roswell:`安装

```
$ ros install fukamachi/cl-project
```

进入`Roswell`的`local-projects`文件夹，因为我们想先把包保存在本地机器上。

```
$ cd ~/.roswell/local-projects
```

现在创建你的项目主干——让我们称这个项目为`my-project`,并假设它依赖于包`alexandria`和`cl-xlsx`

```
$ make-project my-project --depends-on alexandria cl-xlsx
```

`tree`它列出它的组成部分:

```
$ tree my-project
my-project
├── my-project.asd
├── README.markdown
├── README.org
├── src
│   └── main.lisp
└── tests
    └── main.lisp2 directories, 5 files
```

由通用 Lisp 标准包系统 ASDF(另一个系统定义工具)构建的 meta-info 项目文件`my-project/my-project.asd`的内容是:

```
(defsystem "my-project"
  :version "0.1.0"
  :author ""
  :license ""
  :depends-on ("alexandria"
               "cl-xlsx")
  :components ((:module "src"
                :components
                ((:file "main"))))
  :description ""
  :in-order-to ((test-op (test-op "my-project/tests"))))(defsystem "my-project/tests"
  :author ""
  :license ""
  :depends-on ("my-project"
               "rove")
  :components ((:module "tests"
                :components
                ((:file "main"))))
  :description "Test system for my-project"
  :perform (test-op (op c) (symbol-call :rove :run c)))
```

Common Lisp 中的测试被组织成独立的包——因此，`.asd`文件将项目的测试准备成一个独立的包，并将`:rove`添加为它的依赖项之一。

填写作者、许可证和描述部分。

在您的项目中，当引入新的包时，它们应该首先包含在两个包系统的`:depends-on`列表中。

`:components`部分列出了属于这个打包系统的所有文件和文件夹(列出的文件名没有以`.lisp`结尾！).所以每当你添加一个新的文件或文件夹时，你应该更新这个文件中的两个`defsystem`!

`my-projcet/src/main.lisp`的内容是:

```
(defpackage my-project
  (:use :cl))
(in-package :my-project);; blah blah blah.
```

你把你的包码写到`;; blah blah blah.`里。

每当您需要依赖项时，将它们添加到`(:use :cl)`列表中。

在这种情况下，用`(:use :cl :alexandria :cl-xlsx).`来完成它是合理的

或者，如果您从这些包中导入单个函数，您可以通过以下方式仅显式导入这些函数:

```
(defpackage my-project
  (:use #:cl
        #:cl-xlsx) ;; this package's entire content is imported
  (:import-from #:alexandria ;; package name
                #:alist-hash-table) ;; a function from :alexandria
  (:export #:my-new-function))
```

在`:export`子句中，您声明了哪些函数应该被导出并公开——从该文件外部可见。

# 使用 Roswell 和 rove 设置测试

在 Common Lisp 中测试通常用`FiveAM`或`prove`来完成。

`rove`是和`Roswell`玩得好的`prove`的继任者。

通过以下方式安装:

```
$ ros install fukamachi/rove
```

在您的解释器中，首先通过以下方式加载它:

```
(ql:quickload :rove)
```

通过以下方式输入其名称空间:

```
(use-package :rove)
```

测试的语法非常简单。

## 阳性和阴性检查

通常，当你进行测试时，你会检查真假。

`rove`为您提供`ok`和`ng`(否定)检查功能。

```
;; expect true
(ok (= (+ 1 2) 3));; expect false
(ng (eq 'a 'a));; add error message
(ok (= 1 1) "equality should be true")
(ng (< 3 2) "3 and 2 should not be equal")
```

您可以检查错误、标准输出和宏扩展:

```
(ok (signals (/ 1 0) 'division-by-zero)) 
;; tests for a specific, expected error(ok (signals (/ 1 0)))                   
;; tests for error occurrence itself without exact specification
```

控制台的输出:

```
(ok (outputs (format t "hello") "hello"))
```

以及一个带有愚蠢宏的宏展开示例。

```
(defmacro my-sum (&rest args) `(+ ,@args))(ok (expands '(my-sum 1 2 3) '(+ 1 2 3)));; which is:
(ok (expands '<macro-call> '<expected macroexpand-1 result>))
```

## 将几个检查分组到一个测试名称中，并对它们进行注释

你应该写下为什么/做什么，因为你正在做检查。有人说“测试是最精确的文档形式。”—所以它们也应该为人类读者而写。

```
(deftest <test-name>
  (testing "what the test is for"
    (ok <expr>)
    (ok <expr>)
    (ng <expr>))
  (testing "another subpoint"
    (ok <expr>)
    (ng <expr>)
    (ng <expr>)))
```

下面是一个函数及其测试示例:

```
(defun my-absolute (x) (if (< x 0) (* -1 x) x))
```

这就是考验:

```
(deftest my-absolute
  (testing "negative input should give positive output"
    (ok (eq (my-absolute -3) 3))
    (ng (eq (my-absolute -3) -3)))
  (testing "positive input should give positive output"
    (ok (eq (my-absolute 3.1) +3.1))
    (ng (eq (my-absolute 2.3452) + 2.4352)))
  (testing "zero should always give zero"
    (ok (zerop (my-absolute 0))
        (zerop (my-absolute 0.0000000))))
```

使用以下命令从控制台运行测试:

```
(run-test 'my-absolute)
```

## 将一个或多个测试分组到测试包(测试套件)中

想象一下，您创建了一个名为`my-package`的项目或包。

在 package/project 文件夹中应该有一个包含这个包的测试文件(测试套件)的`tests`文件夹。

`cl-projects`已经创建了骨架，在我们的例子中是`tests/main.lisp`。

它自动生成的内容是:

```
(defpackage my-project/tests/main
  (:use :cl
        :my-project
        :rove))
(in-package :my-project/tests/main);; NOTE: To run this test file, execute `(asdf:test-system :my-project)' in your Lisp.(deftest test-target-1
  (testing "should (= 1 1) to be true"
    (ok (= 1 1))))
```

每当我们需要包作为新的依赖项或它们的单一功能时，我们应该完成这个`defpackage`定义，类似于我们为`my-project/my-project/main`的`defpackage`表达式所做的。

如果我们创建几个测试文件/套件，每个文件都必须在`.asd`文件的`defsystem`的`:components`部分注册。这样`(asdf:test-system :my-project)`就包含了所有的测试服。

## 增加准备和善后程序

在测试软件包之前，`setup`函数会对其列出的命令进行一次评估。例如，这对于建立数据库或创建临时目录非常有用。

测试运行后，可以使用`teardown`命令清理临时目录或其他生成的文件。

```
;; after the defpackage and in-package declarations:(setup
  (ensure-directories-exist *tmp-directory*));; all the tests of the package(teardown
  (uiop:delete-directory-tree *tmp-directory* validate t 
                              :if-does-not-exist :ignore))
```

对于应该在每次测试之前或之后运行的命令，请使用:

```
(defhook :before ...)
(defhook :after ...)
```

## 常规设置

为了让测试具有特殊的风格，您可以指定全局变量:

```
(setf *rove-default-reporter* :dot)  ;; other: :spec :none
(setf *rove-debug-on-error* t)
```

## 跑步测试服

```
(asdf:test-system :my-project)(rove:run :my-project/tests);; you can modify their style when calling to run
(rove:run :my-project/tests :style :spec) ;; other :dot :none
```

`FiveAM`，另一套测试服，非常相似。

# 使用 Travis-CI 自动化测试

由于在`[https://raw.githubusercontent.com/roswell/roswell/release/scripts/install-for-ci.sh](https://raw.githubusercontent.com/roswell/roswell/release/scripts/install-for-ci.sh.)`中有一个脚本，它负责安装`Roswell`中所有可用的实现，所以设置 Travis CI 被大大简化了。

首先，你必须在 Travis CI 注册一个账户。

将 Travis CI 与 GitHub 或 Bitbucket 或 GitLab 连接在这里的[和这里的](https://docs.travis-ci.com/user/tutorial/?utm_source=help-page&utm_medium=travisweb)和[中进行了解释。](https://roswell.github.io/Travis-CI.html)

简而言之:你必须点击你的个人资料图片，然后点击*设置，并选择你希望 Travis CI 监控的存储库。(在我们的案例中，这将是 GitHub repo 窝藏* `*my-project*` *)。*

接下来要做的就是创建一个名为`.travis.yml`的文件，并将它放在项目根文件夹中(由 git 运行)。一旦您 git 添加新的`.travis.yml`文件并 git 提交和推送它，Travis CI 将开始“照顾”您的存储库。

对于只使用最新的 SBCL 二进制文件的测试，一个非常简短的`.travis.yml`就足够了:

```
language: common-lisp
sudo: required

install:
  - curl -L https://raw.githubusercontent.com/roswell/roswell/release/scripts/install-for-ci.sh | sh

script:
  - ros -s prove -e '(or (rove:run :my-project/tests) (uiop:quit -1))'
```

通过将`sudo:`字段设置为`false`，用户可以激活 Travis CI 来使用新的`container-based infrastructure`(比旧系统更快的测试启动时间)。

下面的`.travis.yml`脚本让`Roswell`在每次提交和 git 推送代码时运行两个实现的漫游测试。

```
language: common-lisp
sudo: false

env:
  global:
    - PATH=~/.roswell/bin:$PATH
    - ROSWELL_INSTALL_DIR=$HOME/.roswell
  matrix:
    - LISP=sbcl-bin
    - LISP=ccl-bin

install:
  - curl -L https://raw.githubusercontent.com/roswell/roswell/release/scripts/install-for-ci.sh | sh
  - ros install rove
  - ros install gwangjinkim/cl-xlsx script:
  - ros -s rove -e '(or (rove:run :my-project/tests :style :dots) (uiop:quit -1))'
```

`env:`下面的`matrix:`部分列出了要测试的 Common Lisp 的不同版本和实现。

这是一个成熟的`.travis.yml`文件，它测试了更多的 Common Lisp 实现。

```
language: common-lisp
sudo: false

addons:
  apt:
    packages:
      - libc6-i386
      - openjdk-7-jreenv:
  global:
    - PATH=~/.roswell/bin:$PATH
    - ROSWELL_INSTALL_DIR=$HOME/.roswell
  matrix:
    - LISP=sbcl-bin
    - LISP=ccl-bin
    - LISP=abcl
    - LISP=clisp
    - LISP=ecl
    - LISP=cmucl
    - LISP=alisp

matrix:
  allow_failures:
    - env: LISP=clisp
    - env: LISP=abcl
    - env: LISP=ecl
    - env: LISP=cmucl
    - env: LISP=alisp

install:
  - curl -L https://raw.githubusercontent.com/roswell/roswell/release/scripts/install-for-ci.sh | sh
  - ros install fukamachi/rove
  - ros install gwangjinkim/cl-xlsx

cache:
  directories:
    - $HOME/.roswell
    - $HOME/.config/common-lisp

script:
  - ros -s rove -e '(or (rove:run :my-project/tests :style :dots) (uiop:quit -1))'
```

因为`clisp`、`abcl`、`alisp`和/或`cmucl`需要`openjdk-7-jre` 和`clisp`需要 `libc-i386`，所以`addons:`和`apt:`部分必须存在，它们必须使用`apt`安装。

在`cache:`下面列出的目录被缓存(为了更快的启动)。

`install:`部分列出了当其中一个组件丢失时需要为您的系统进行的安装，按照给定的顺序。

实现是由`https://raw.githubusercontent.com/roswell/roswell/release/scripts/install-for-ci.sh`脚本准备的，因此单个实现的安装不必明确列在`install:`下。如果没有`Roswell`,测试不同的实现将需要更多的工作。`Roswell`安装并测试最新版本的实现。

如果您的测试包本身有一些更简单的命令行命令，那么`script:` 命令看起来会更简单。

查看 Travis CI 文档，了解更多信息(在不同操作系统上进行测试等。)

## 工作完成后，让 Travis CI 通过电子邮件或电报通知您

添加到`.travis.yml`一个:

```
notifications:
  email:
    - my-email@gmail.com
```

会让它在每次作业运行时通知您。

更好一点的是通过电报发出通知，如下文[所述](https://testdriven.io/blog/getting-telegram-notifications-from-travis-ci/):

您添加到`.travis.yml`:

```
after_script:
  - bash ./telegram_notification.sh
```

并将一个名为`telegram_notification.sh`的脚本放到您的项目根文件夹中:

```
#!/bin/sh

# Get the token from Travis environment vars and build the bot URL:
BOT_URL="https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage"

# Set formatting for the message. Can be either "Markdown" or "HTML"
PARSE_MODE="Markdown"

# Use built-in Travis variables to check if all previous steps passed:
if [ $TRAVIS_TEST_RESULT -ne 0 ]; then
    build_status="failed"
else
    build_status="succeeded"
fi

# Define send message function. parse_mode can be changed to
# HTML, depending on how you want to format your message:
send_msg () {
    curl -s -X POST ${BOT_URL} -d chat_id=$TELEGRAM_CHAT_ID \
        -d text="$1" -d parse_mode=${PARSE_MODE}
}

# Send message to the bot with some pertinent details about the job
# Note that for Markdown, you need to escape any backtick (inline-code)
# characters, since they're reserved in bash
send_msg "
-------------------------------------
Travis build *${build_status}!*
\`Repository:  ${TRAVIS_REPO_SLUG}\`
\`Branch:      ${TRAVIS_BRANCH}\`
*Commit Msg:*
${TRAVIS_COMMIT_MESSAGE}
[Job Log here](${TRAVIS_JOB_WEB_URL})
--------------------------------------
"
```

一个电报机器人，你通过从你的电报账户向`@Botfather` : `/newbot`写信来初始化它——它要求你给它一个名字，其他任何机器人都不会给它。如果你的名字通过了，你会得到一个机器人。然后向`@Botfather`写:`/token`，它会问你向哪个机器人要令牌。这个令牌必须用于脚本中的`TELEGRAM_TOKEN`。

# 使用工作服为 Travis 添加代码覆盖率

为此，我们遵循`Roswell`的[文档](https://roswell.github.io/Coveralls.html)或 [Boretti](https://borretti.me/article/lisp-travis-coveralls) 的博客，他使用了一个`Roswell`独立的脚本和一个使用`FiveAM`作为测试系统的脚本

与 Travis CI 类似，您必须在[工作服](https://coveralls.io/)中创建一个帐户。

然后，您在`.travis.yml`文件中标记应该评估代码覆盖率的实现:

```
env:
  matrix:
    - LISP=sbcl COVERALLS=true
    - LISP=ccl
```

还应规定工作服的安装命令:

```
install:
  # Coveralls support
  - ros install fukamachi/cl-coveralls
```

最后，脚本调用必须修改为:

```
 - ros -s rove 
        -s cl-coveralls
        -e '(or (coveralls:with-coveralls (:exclude (list "t"))
                (rove:run :quri-test))
                (uiop:quit -1))'
```

# 结束——也是开始！

现在，我们已经为 2021 年建立了一个完整的公共 Lisp IDE 系统。

祝 Lisp Hacking 快乐！

*(欢迎评论区的建议和反应！—关于设置*`*Roswell*`*`*cl-projects*`*以及用* `*rove*` *进行测试的部分，我从用日语写了一本书的日本朋友那里了解到:* `*SURVIVAL COMMON LISP*` *—作者有:* `*Masatoshi Sano*` *，* `*Hiroki Noguchi*` *，* `*Eitaro Fukamachi*` *，* `*gos-k*`， `*Satoshi Imai*` *，* `*cxxxr*` *然而，许多信息都可以在包和工具的文档中找到。)**