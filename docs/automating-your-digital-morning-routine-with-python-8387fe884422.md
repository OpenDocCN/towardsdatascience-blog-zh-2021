# 使用 Python 自动化您的数字早晨程序

> 原文：<https://towardsdatascience.com/automating-your-digital-morning-routine-with-python-8387fe884422?source=collection_archive---------8----------------------->

![](img/bc24ee6c5d8e9f141b97f0287a027fa3.png)

照片由[努贝尔森·费尔南德斯](https://unsplash.com/@nubelsondev?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## 一个简单的 python 脚本开始您的一天

当你早上第一次使用笔记本电脑/台式机时，你可能会打开很多应用程序。每天都这样做可能会令人厌倦，所以我写了一个脚本来自动化在我的桌面上开始一天的过程，在这个教程中我将向你展示它是如何工作的。

**【2021 年 11 月 22 日更新】**

我制作了这篇文章的视频版本，你可以在 Youtube 上查看:

## 简单地说，这个剧本

这个脚本简化了我早上启动电脑后的日常工作，处理了我在开始工作前必须完成的大量基本手动任务。

这个例程包括类似 ***的过程，打开特定的浏览器标签，显示例程提醒，启动跟踪脚本等等。***

让我们一行一行地过一遍:

**1。导入依赖关系**

```
import webbrowser
import time
import os
import subprocess
from datetime import datetime
```

**2。写一个通知函数**

```
def sendNotificationOnLinux(message):
    subprocess.Popen(["notify-send", message])
    return
```

这里我使用内置库`subprocess`调用我的 Linux 机器上的通知系统并发送一个简单的消息。这是一个让一整天都自动和自己对话的好方法。

这样我就不用为我每天做的任务设置每日自定义闹钟了。下面我也提供了一个类似的 Windows 选项:

```
def sendNotificationOnWindows(msg="", delay=2):
    t = 0
    notify = ToastNotifier()
    while t < delay:
        notify.show_toast("Notification",msg)
        time.sleep(1)
        t+=1
```

**3。写一个延续日函数**

```
def continueDay():
    cont = input("Press to continue")
```

这里我只是包装了 python 内置的 input()函数给它一个更合适的名字，我用它来做逐行控制脚本的执行。

**4。当我打开和关闭电脑时记录日志**

```
os.system("bash /home/lucassoares/Desktop/projects/self_track/scripts/logSystemOnOff.sh")0
```

在这里，我运行一个 bash 脚本来记录我的计算机最后一次打开和关闭的时间，以跟踪我何时开始工作，何时停止工作。这个 bash 脚本如下所示:

```
# logSystemOnOff.sh
echo "$(who -b)" >> path/to/file/systemOnOff.txt
```

**5。打印关于我正在运行的跟踪器的提醒**

现在我给自己留一个提醒，我正在为我的键盘和应用程序窗口运行跟踪脚本。

```
print("Running keyfrequency and active window loggers with sudo")Run keyfrequency and active window loggers with sudo
```

这些与使用 python 的`multiprocessing`包的脚本并行运行:

```
# call_Start_and_Logs.py
import os                                                                       
from multiprocessing import Pool                                                

processes = ('/home/lucassoares/Desktop/automated/logWin.py', 
             '/home/lucassoares/Desktop/automated/logKeys.py',
             '/home/lucassoares/Desktop/automated/startday.py')                                    

def run_process(process):                                                             
    os.system('python {}'.format(process))                                       

pool = Pool(processes=3)  
pool.map(run_process, processes)
```

在这里，我运行一个 python 脚本来记录我在键盘上输入的所有键(因为我在我的 linux 机器上做了一点自我跟踪，这是另一篇文章的主题)以及我使用的应用程序。上面提到的`startday.py`剧本就是我们现在正在讨论的。

6。打印开始跟踪焦点的提醒

```
sendNotificationOnLinux("Turn on Focus")
continueDay()
```

这里有一个提醒我打开 focus 的提示，这是另一个跟踪我聚焦时间的脚本。现在我正在使用一个名为`logFocus.sh`的定制 bash 脚本在终端上做这件事。如果您对以下 bash 脚本感到好奇:

```
logfile="/path/to/text/file/focus.txt"
start="$(date +%s)"
echo "Started logging focus"
read stop
echo "Stopped recording focus"
echo "Calculating focus time"
end="$(date +%s)"
echo "$start $end" >> $logfile/path/to/python/executable/python 
/path/to/python/script/to/calculate/focused/time/calculateTodaysFocusTime.py
```

我最近发现这个名为 [timetrap](https://github.com/samg/timetrap) 的终端工具在这方面做得更好，所以我可能会在不久的将来改变它。

计算和存储聚焦时间的 python 脚本如下:

```
from utils import loadFocus
import pandas as pd
from datetime import datetimetoday = datetime.strftime(datetime.today(),format="%Y-%m-%d")
timeStartsStamp,dateStartsStamp,durations = loadFocus()
df = pd.DataFrame(dict(dates=dateStartsStamp, durations=durations))
todayFocus = df[df["dates"]==today]["durations"].sum()
print(todayFocus)
```

**7。Spotify 提醒**

```
sendNotificationOnLinux("Turn on white noise")
continueDay()
```

这里是另一个提醒，启动 Spotify 并打开[白噪音](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5638812/)(是的，我一天的大部分时间都在听白噪音)。

**8。打开谷歌日历**

```
# Google calendar events
sendNotificationOnLinux("Check your calendar")
webbrowser.open("https://calendar.google.com/calendar/u/0/r/day")
```

这个很简单，我只需使用 python 的 webbrowser 模块在浏览器上打开谷歌日历。

```
# Touch typing practice
print("TYPE")
os.system('"mlt random "0)-_=+[{]};:\'\|//?"') #pinky finger
os.system("mlt random 123456!@#$%^qwertasdfgzxcvbZXCVBASDFGQWERT~") #left hand
os.system("python /home/lucassoares/Desktop/automated/logTyping.py") #general practice
continueDay()
```

这部分是我每天的触摸打字练习套路。我使用 [mltype](https://github.com/jankrepl/mltype) ，一个很棒的命令行工具，来改进命令行输入！我用这个工具写了三种不同的练习:

*   `logtyping.py`:标准实践，从我保存的数据集中抽取几行 python 脚本，其中包含我感兴趣的模块和脚本
*   一个是专注于练习我的左手(所以只有键盘上的左手键)
*   另一个是给我右手小指的，我发现这是我打字速度的瓶颈

我在日常实践中调用的 python 脚本是这样的:

```
# logTyping.py
import os
import pathlib
import random
import pandas as pd
from datetime import datetime
import sysdef updateTypingPerformance(df,datedPerformance):
    if "Unnamed: 0" in df.columns:
        df.drop("Unnamed: 0", axis=1, inplace=True)
    df.loc[len(df)] = datedPerformance
    return dfdef typePractice(typingDataset):
    files = list(typingDataset.iterdir())
    file = random.choice(files)
    f = open(file, "r")
    numberOfLines = len(f.readlines())
    print(file)
    print(numberOfLines)
    if numberOfLines>10:
        numberOfLines=10 os.system(f"mlt file  --n-lines {numberOfLines} {file}")typingDatasetFolder = "path/to/folder/typingDataset"typingDataset = pathlib.Path(typingDatasetFolder)typingPerformance = "path/to/typing/performance/file/typingPerformance.csv"df = pd.read_csv(typingPerformance)
typePractice(typingDataset)performance = []date = str(datetime.now())wpm = input("What was the wpm?")
acc = input("Waht was the accuracy?")
cont = input("Continue typing?")
performance.append(date)
performance.append(wpm)
performance.append(acc)updateTypingPerformance(df,performance)
while cont=="y":
    performance = []
    typePractice(typingDataset)
    date = str(datetime.now())
    wpm = input("What was the wpm?")acc = input("Waht was the accuracy?")performance.append(date)
performance.append(wpm)
performance.append(acc)updateTypingPerformance(df,performance)

    cont = input("Continue typing?")if len(sys.argv)>1:
    if sys.argv[1]=="plot":
        df["wpm"].plot()
        plt.show()
```

它基本上是用 mlt 命令行工具设置培训，并更新一个. csv 文件，我用我的触摸输入性能保存它。

**9。学习普通话提醒**

```
print("MANDARIN")
webbrowser.open("www.duolingo.com")
continueDay()
```

这是另一个不言而喻的例子:我每天练习普通话，所以我会自动在浏览器上打开它，进行每天 10 分钟的练习。

**10。在浏览器上打开项目 euler 进行晨间编码例程**

```
print("CODE")
webbrowser.open("https://projecteuler.net/archives")
# start Anki
```

在练习了普通话之后，我练习了一些解决问题的方法，所以我打开 [project euler](https://projecteuler.net/) 作为我的日常编码练习。我喜欢每天只做一项运动来暖脑。

我更喜欢在终端上有一些提醒，在系统的通知弹出窗口上有一些提醒，但这只是个人喜好，根据你的喜好调整它。

**11。检查通信信道**

做完一天的热身活动后，我喜欢先检查我使用的基本沟通渠道，即:

*   松弛(网络)
*   团队(工作)
*   电子邮件(工作)
*   Whatsapp(社交)

```
# teams, mail, slack, whatsapp
sendNotificationOnLinux("Open Mail, Teams, Slack and Whatsapp")
os.system("teams")
os.system("mailspring")
os.system("slack")
webbrowser.open("https://web.whatsapp.com/")
```

一旦我完成了检查(我一天做三次，早上一次，下午一次，晚上一次)，我就检查我的中等状态，以了解我在平台上做得如何。

```
# medium
webbrowser.open("https://medium.com/me/partner/dashboard")
print("Check your stats on Medium")
continueDay()
```

**12。最终提醒和结束**

```
sendNotificationOnLinux(message="Leave an Ipython window Open")
```

这是为了让我记住总是打开一个 ipython 窗口，这样我就可以在需要快速编写脚本时随时使用它。

```
sendNotificationOnLinux(“Be grateful, no negative self-narratives and have a great day!”)
```

这里有一个很好的日常小提醒，让我专注于重要的事情，永远心存感激，避免消极的适得其反的自我叙述，这样我就不会分心。

这个项目的源代码可以在[这里](https://github.com/EnkrateiaLucca/Morning-Routine-Automation)找到。

# 关于桌面日常自动化的思考

我喜欢这种自动化脚本，因为它们允许我通过简化所有的手动过程来更快地执行我的例程，即使是像打开标签页这样简单的事情。

这篇文章的重点是 ***展示 python 和 bash 脚本结合的威力，通过自动化我们每天必须完成的烦人的手动任务来提高您的生产力*** 。

我试着用一个原则来写这些:如果事情可以自动化，就不要重复做。

如果您有兴趣学习 bash 脚本来优化您的开发工作流，请查看 Udemy 的这些课程:

> 这些是附属链接，如果你使用它们，我会得到一小笔佣金，干杯！:)

*   [Bash 脚本和 Shell 编程(Linux 命令行)](http://seekoapp.io/61351f9698becc000a8e2958)
*   [Linux Shell 脚本:基于项目的学习方法](http://seekoapp.io/61351f9798becc000a8e295a)

如果你喜欢这篇文章，请在 [Twitter](https://twitter.com/LucasEnkrateia) 、 [LinkedIn](https://www.linkedin.com/in/lucas-soares-969044167/) 上联系我，并在 [Medium](https://lucas-soares.medium.com/) 上关注我。谢谢，下次再见！:)