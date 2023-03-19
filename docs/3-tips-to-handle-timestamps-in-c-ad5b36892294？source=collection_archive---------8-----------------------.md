# 处理 C 中时间戳的 3 个技巧

> 原文：<https://towardsdatascience.com/3-tips-to-handle-timestamps-in-c-ad5b36892294?source=collection_archive---------8----------------------->

## 处理时间序列中时区的简明指南

![](img/5217440e3cc58aa23095b0b5acc113b4.png)

来自[维基媒体](https://en.wikipedia.org/wiki/UTC%2B08:45#/media/File:Entering_Central_Western_Time_Zone.jpg)的图像。许可证 CC BY-SA 4.0

无论是谁在时间序列分析中处理了带有时区的时间戳，总是会遇到一些令人头痛的问题。无论我处理时间序列多少次(在我的例子中不变地涉及处理时区)，我总是必须重新审视它是如何工作的，因为它通常是反直觉的。

# 需求

处理时间序列通常涉及处理不同的时区。即使只使用一个时区，您可能仍然希望可视化您当地时区的数据，这可能会根据用户所在的国家而变化。

这种情况使得使用世界协调时作为参考很方便。为什么？因为它不会经历夏令时变化，并且很容易转换到任何时区。

就如何表示时间戳而言，它可以是:

*   语言特定的日期时间对象，
*   基于整数的人类可读格式(即日期为 20210126，时间为 1530)，
*   人类可读文本格式(即 1999–01–01t 00:00:29.75)，
*   UNIX 纪元时间。

我更喜欢最新的，因为它简化了时间的计算。要迭代一分钟，你只需要将你所处理的时间光标增加 60。它还精确地定义了时间中的某个时刻。如果您有数百 Gb 或 Tb 的数据，纯数字表示是可行的方法。

我将为这种情况提供一些例子，所有的例子都可以在文档和问答论坛中找到——这里没有什么新的或奇特的东西——但是这些信息经常被隐藏在其他解释中，通常是像将特定的本地化时间戳转换为 UNIX Epoch 这样简单的事情，而相反的事情并不像应该的那样简单。

# 时间戳需要时区

如果你说是【2021 年 3 月 28 日 09:30 并且你认为你完成了，那你就错了。接下来的答案是，好的，是【2021 年 3 月 28 日 09:30 ，但是 ***在哪里*** ？每个时间戳必须有一个位置或时区，可以是隐式的(按照惯例，该数据库中的所有时间戳都使用法兰克福时间),也可以是显式的。所以，如果你提供数据，帮你自己和其他人一个忙，做一个明确说明包含时间戳的时区的练习。这可能是显而易见的，但我向您保证，我所看到的涉及时间戳的数据集有一半以上都缺少这种信息。

# 时区不是给定地点的时间

我在处理这个问题时发现的第一个错误是很容易混淆概念。您经常会听到诸如“法兰克福比纽约早 5 个小时”或“纽约时区是美国东部时间”之类的错误说法。

法兰克福有时比纽约早 5 个小时，但在其他地方则是 6 个小时，具体时间取决于您所处理的具体年份。

纽约不是东部时间，纽约有一半时间是东部时间，另一半时间是东部时间。过渡发生的时间每年都不一样，甚至在不久的将来会消失。

东部时间不是东部时间，而是东部标准时间(当然也不是我听说的“复活节时间”)。如果你想说东部时间，那就是东部时间。

犯这样的错误是可以的，因为我们都是从那里开始的，但是这些错误需要尽快改正，否则你将会做错误的计算。

我发现处理与特定地点相关的时间序列(在我的行业中，是在给定交易所交易的金融数据)的最简单方法是处理实际的地区/城市。因此，说“纽约时间”意味着它将是美国东部时间或美国东部时间，这取决于具体的年份和日期，但时间戳是正确定义的。这通常也是存储数据的正确方法，因为无论发生什么情况，在给定的时间都会发生许多事件。股票交易所通常在每天 9:30 开门，你在每天下午的同一时间为你的孩子跑步，他们不会根据是否遵守夏令时来修改它。

指定时区可能无法唯一地定义某个时刻，因为在夏令时调整期间，可能不清楚您指定的确切时刻，但所有夏令时调整都发生在周末。如果您的数据缺少周末(比如来自许多业务相关领域的数据)，那么您就不会关心这个细节。

如果夏令时的变化很重要，因为您的数据可能发生在周末，那么您必须始终使用 UTC，因为它不会经历任何变化。这就是为什么军方使用祖鲁语时间的原因，因为这是一种全球共享的指定时间的方式。

> **提示 1** :当你指定一个时间戳的时候，一定要指定这个时间属于哪个特定的地点(城市)。如果您的时间序列涉及周末，请使用 UTC。

# 使用 UNIX 纪元

UNIX 纪元时间被定义为自 1970 年 1 月 1 日 00:00 UTC 以来经过的秒数。请注意，UNIX Epoch 是 UTC，因此它可以准确无误地识别特定的时刻。永远不要询问 UNIX epoch 时间戳的时区，根据定义它是 UTC。

关于 UNIX 纪元的一个特别的警告是闰秒，但是除非您必须处理闰秒，否则 UNIX 纪元很好。还要记住，当使用 32 位有符号整数时，UNIX epoch 可以表示多达 2038 个时间戳的时间。有关详细信息，请参见[1],但通常这些限制不会对您的应用产生影响。

> **提示 2** : UNIX epoch 时间戳是一种存储给定时刻的便捷方式，它们通常足够准确，并且允许做一些简单的事情，比如加 60 来得到下一分钟。它们也很有效，因为只有 4 个字节可以用来存储时间戳。作为 UTC 的定义，不需要传递时区，您总是知道如何将时间戳转换成任何时区。

UNIX epoch 给出的精度是一秒，尽管经常使用备用版本处理毫秒。

# ANSI C

我们将开始回顾 C 如何处理时区和时间戳。在 C 语言中，这个标准从来没有覆盖不同的时区，但是从 80 年代开始，它已经覆盖了所有的 UNIX 系统。

可以使用 32 位整数存储 UNIX 纪元时间，在标准中，该类型是传统上为整数的 *time_t* 。该标准没有定义具体的类型(在我的 64 位 FreeBSD 框中使用 clang 是一个*长的*)。

因此，如果您想指定一个 UNIX 纪元，您可以这样做:

```
/* 1616927900 Epoch is Sunday, 28 March 2021 10:38:20 GMT 
   1616927900 Epoch is Sunday March 28, 2021 06:38:20 (am) in time zone America/New York (EDT)
*/time_t epoch_time = 1616927900;
```

在 POSIX 系统(所有 UNIX/Linux/BSD 系统)中，您可以使用 TZ 变量将它转换成任何带有时区的时间戳。假设您想知道纽约特定 UNIX 纪元是什么时间。然后你做:

```
/* 1616927900 Epoch is Sunday, 28 March 2021 10:38:20 GMT 
   1616927900 Epoch is Sunday March 28, 2021 06:38:20 (am) in time zone America/New York (EDT)
*/time_t epoch_time = 1616927900;/* CONVERT TO struct tm */
struct tm *converted_time;
converted_time = localtime(&epoch_time);/* GET SYSTEM TIME ZONE */
tzset();/* DISPLAY TIME */
char buffer[26];
strftime(buffer, 26, "%Y-%m-%d %H:%M:%S", converted_time);
printf("Timestamp for %ld is %s according to system timezone.\n", epoch_time, buffer);/* DISPLAY TIME ZONE */
printf("Timezone for %ld is %s.\n", epoch_time, tzname[converted_time->tm_isdst]);---$ TZ='America/New_York' ./test
Timestamp for 1616927900 is 2021-03-28 06:38:20 according to system timezone.
Timezone for 1616927900 is EDT.
```

`struct tm`有一个名为`tm_isdst`的字段，可以是-1、0 或 1。

-1 用于指定一个给定的时间戳(我将在后面介绍这种情况)，0 表示没有实行夏令时，1 表示实行夏令时。

`tzset`将检索计算机的特定区域，当使用`localtime`时，localtime 将使用它来填充`tm_isdst`，以便 struct 的字段给你正确的 struct。`tzset`还设置外部数组`tzname`，可用于获取时区名称。

如果您想使用不同于系统使用的时区，您有两种选择:

1.  你可以在你的程序前面加上正确的 TZ 变量:`$TZ='America/New_York' ./my_program`。这不是 C 的一部分，而是 UNIX shell 在调用命令时定制环境的方式。
2.  在调用`tzset`之前，可以用`putenv("TZ=America/New_York");`显式设置 TZ 变量。

如果您想做相反的事情，即获得给定本地化时间戳的 UNIX 纪元(问题将是:*哪个 UNIX 纪元对应于纽约 2021 年 3 月 28 日星期日 06:38:20？*)这是您将使用的代码:

```
/* 1616927900 Epoch is Sunday, 28 March 2021 10:38:20 GMT 
   1616927900 Epoch is Sunday March 28, 2021 06:38:20 (am) in time zone America/New York (EDT)
*//* GET SYSTEM TIME ZONE, MUST BE NEW YORK */
tzset();/* DEFINE THE TIMESTAMP struct tm */
struct tm localised_time;
localised_time.tm_year = 2021 - 1900;
localised_time.tm_mon = 3 - 1;
localised_time.tm_mday = 28;
localised_time.tm_hour = 6;
localised_time.tm_min = 38;
localised_time.tm_sec = 20;
localised_time.tm_isdst = -1;
time_t epoch_time = mktime(&localised_time);/* NOTICE HOW mktime FIXES tm_isdst */
printf("Fixed tm_isdst is %d.\n", localised_time.tm_isdst);/* DISPLAY TIME ZONE */
printf("Timezone is %s.\n", tzname[localised_time.tm_isdst]);/* DISPLAY EPOCH */
printf("Epoch timestamp is %ld.\n", epoch_time);---$ TZ='America/New_York' ./test2
Fixed tm_isdst is 1.
Timezone is EDT.
Epoch timestamp is 1616927900.
```

请注意，在`struct tm`中，年份被指定为自 1900 年以来的年数，一月的月份为 0。我们对`tm_isdst`使用-1，`mktime`使用`tzset`检索的时区来确定这个值，并确定在纽约的那一天和时间是美国东部时间还是美国东部时间。

很明显，只有当你的计算机位于美洲时区，或者你明确地为程序设置或传递了 TZ 时，这种方法才有效。另请注意，这不适用于 Microsoft Windows。

> **提示 3** :在 C UNIX/POSIX 时区资源和基本 struct tm 中，mktime 和 localtime 函数可以用来处理时区。它在大多数情况下都能很好地工作，并且可以与所有现成的 C/C++标准一起工作。

我在 C 语言中使用这些简单的方法成功地处理了时区。在其他情况下，这种简单性可能还不够。

C++还有其他处理时区的方法，最相关的是 C++20 标准的 *chrono* [2】库部分(该库也可以为旧标准编译)。Boost【3】也包含了一个处理时区的库，但是显然，它缺少一些时区或者有限制——我没有试过——。还有 ICU 库[4]来处理本地化，这似乎是详尽和完整的，还有许多其他开源解决方案。所有这些的问题是，你给你的系统增加了另一个依赖，我发现它们不容易使用。

我发现上面的方法对我的需求来说足够简单，但是它只对 UNIX 系统有效。它适用于任何 C/C++标准。

[1][https://en.wikipedia.org/wiki/Unix_time](https://en.wikipedia.org/wiki/Unix_time)

[2][http://www.cplusplus.com/reference/chrono/](http://www.cplusplus.com/reference/chrono/)

[3][https://www . boost . org/doc/libs/1 _ 74 _ 0/libs/locale/doc/html/dates _ times _ time zones . html](https://www.boost.org/doc/libs/1_74_0/libs/locale/doc/html/dates_times_timezones.html)

[http://site.icu-project.org/](http://site.icu-project.org/)