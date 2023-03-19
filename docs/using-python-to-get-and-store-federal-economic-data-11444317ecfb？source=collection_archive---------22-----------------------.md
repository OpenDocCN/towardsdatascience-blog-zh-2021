# 使用 Python 获取和存储联邦经济数据

> 原文：<https://towardsdatascience.com/using-python-to-get-and-store-federal-economic-data-11444317ecfb?source=collection_archive---------22----------------------->

## 从数据获取流程的一些最初步骤开始数据之旅。

![](img/6378f66ece8e1a4295e3ec29ee4050e3.png)

关于金钱的数据——还有比这更好的吗？来自 [Pexels](https://www.pexels.com/photo/eagle-printed-on-bill-of-america-4386151/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Karolina Grabowska](https://www.pexels.com/@karolina-grabowska?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

在每个分析师的一生中，都会有这样一个时刻，他们必须获取、处理和存储数据，以便获得分析阶段所需的数据。可悲的是，并不是所有的东西都是干净的，都是从股票数据集中打包出来的。如果你不是一个有经验的数据工程师或不习惯 ETL 过程，这可能是一个挑战，并提出一些你必须解决的概念问题。

我想，既然我经历了这个过程，而且记忆犹新，我会写一篇文章，希望能帮助那些面临挑战的人。我要警告你，这不是高质量的代码。它在我的情况下是有效的，但却被展示出来，毫无保留。我做了很少的清理或优化，所以请记住这一点。

让我们跳进来。

# 要求

根据它需要什么，我把它保持得非常简单:

— Python(我用的是 3.9，因为它在我的开发工作站上运行)

—熊猫

— psycopg2(用于数据库访问的 Python 库)

— Datapungi_fed(该库允许轻松访问美联储数据库)

— PostgreSQL(我目前使用的是版本 13，但几乎任何最新版本都适用于我们在这里做的事情)

一点也不难。

注意:对于这些数据，您需要向美联储注册以获得 API 密钥。这些在 https://research.stlouisfed.org/docs/api/api_key.html[有售](https://research.stlouisfed.org/docs/api/api_key.html)

# 数据

首先说一下我们会收到的数据。

美联储维护着一个庞大的数据库，其中包含各种经济数据集，分布在众多数据库中。几乎所有这些都是时间序列集，根据各种报告标识符以日期和数据的形式出现。

关于使用的简要说明:美联储使数据集可用于个人、非商业用途，因此您可以按照自己选择的长度进行构建和分析。但是，如果你想通读所有的条款和条件，这里有法律常见问题的链接:[https://fred.stlouisfed.org/legal/](https://fred.stlouisfed.org/legal/)

datapungi_fed 库简化了实际检索过程，并处理了所有繁重的工作。我们简单地通过名字调用一个报告，然后库返回给我们一个系列，我们可以使用 pandas 来操作。

我发现在我的日常使用中，我对 API 的使用超过了我真正喜欢的程度。我想确保 A)我将我的 API 访问限制在可接受的数量。毕竟，我是一个有礼貌的数据收集者。b)我希望数据更接近我的最终目的地，并防止出现连接问题、服务器维护问题和其他连接问题。我还希望能够在必要时进行更多的后端处理，并减轻前端的负担。因为这个原因，我把这个放进了数据库。

我想从美联储获取的另一个数据是发布的每日报告列表。这有点不同，合并了一行，包含 ID、开始和结束日期、报告名称、链接、新闻发布标志和注释。

这第二次拉进了一些数据净化，以使它正确加载，但我们也将解决这个问题。

# 数据库ˌ资料库

我为数据库创建的简单模式由四个表组成:

1.  美联储 _ 报告
2.  美联储 _ 报告 _tmp
3.  美联储 _ 释放
4.  美联储 _ 发布 _tmp

普通表和 _tmp 表本质上都是彼此的副本。它们的作用是在 _tmp 表中对数据库进行初始插入，然后只将新数据复制到主表中，此时 _tmp 表被清空。

表 1 和表 2 的结构是:

```
CREATE TABLE public.fed_reports (
  report_date DATE NOT NULL,
  data NUMERIC NOT NULL,
  report_name VARCHAR NOT NULL,
  hash VARCHAR NOT NULL PRIMARY KEY
);
```

类似地，表 3 和表 4 的结构是:

```
CREATE TABLE public.fed_reports (
  release_date DATE NOT NULL,
  report_name VARCHAR NOT NULL,
  report_link VARCHAR NOT NULL,
  notes VARCHAR NOT NULL,
  hash VARCHAR NOT NULL PRIMARY KEY
);
```

这为流程中不同点的数据提供了四个容器。现在，我们需要一些方法来确保事情能够正常进行。

前两个函数用于为 _tmp 表的内容分配一个哈希值。执行此操作是因为我只想要新数据。通过对内容进行散列，我为每一行定义了一个惟一的键，这给了我在插入时匹配的内容。当我进入实际的检索代码时，我将进一步讨论这个问题，所以请耐心等待。

```
CREATE OR REPLACE FUNCTION public.rehash_fed_reports()
  RETURNS integer
  LANGUAGE 'plpgsql'
  VOLATILE
  PARALLEL UNSAFE
  COST 100AS $BODY$
    begin
      UPDATE fed_reports_tmp
      SET hash = md5(
        cast(report_date as VARCHAR) ||
        cast(data as VARCHAR) ||
        cast(report_name as VARCHAR)
      ) return 0;
  end;
  $BODY$;CREATE OR REPLACE FUNCTION public.rehash_fed_releases()
  RETURNS integer
  LANGUAGE 'plpgsql'
  VOLATILE
  PARALLEL UNSAFE
  COST 100AS $BODY$
  begin
    UPDATE fed_releases
    SET = md5(
      cast(release_date as VARCHAR) ||
      cast(report_name as VARCHAR) ||
      cast(report_link as VARCHAR) ||
      cast(notes as VARCHAR)
    ) return 0;
  end;
  $BODY$;
```

这两种功能基本相似。它们对表中的行执行定义的更新，并返回一个整数 0。没有真正的错误检查或任何其他容错，因为如果他们的过程失败，函数将返回一个错误，事务将默认回滚。它使这个应用程序变得简单。

注意，为了使用 md5 函数，我将所有数据转换成文本格式，并将结果连接起来。只要始终使用相同的方法，这就不是问题。

最后，我定义了一个函数来执行数据移动。

```
CREATE OR REPLACE FUNCTION public.fed_report_update()
  RETURNS integer
  LANGUAGE 'plpgsql'
  VOLATILE
  PARALLEL UNSAFE
  COST 100AS $BODY$
  begin

    INSERT INTO fed_releases (
      release_date, 
      report_name, 
      report_link, 
      notes, 
      hash)
    SELECT * FROM 
      fed_releases_tmp 
    WHERE 
      hash NOT IN (
        SELECT hash FROM fed_releases
    );

    INSERT INTO 
      fed_reports (
        report_date, 
        data, 
        report_name, 
        hash)
    SELECT * FROM 
      fed_reports_tmp 
    WHERE 
      hash NOT IN (
        SELECT hash FROM fed_reports);return 0;
    end;
  $BODY$;
```

这个函数只使用 INSERT INTO 语句根据子选择的结果执行插入。在本例中，我从 each _tmp 表中选择主表中不存在散列的所有内容。这确保了只添加新数据。

或者，我可以每次只删除主表中的数据，然后重新插入所有数据，而不需要所有的散列步骤，但是在发布报告的情况下，我想要一个每日历史记录，并且我不能再次获得该数据，所以这是必要的。如果我只为一个人做，为什么不使用复制粘贴的魔法，为两个人都做呢？

最后，我唯一没有放在这里的是清除 _tmp 表的函数。我将它们打包到一些其他的进程中，作为更大的应用程序的一部分，但是给定代码示例，您应该能够自己轻松地完成这些工作。

这就是数据库。很简单，对吧？

# 检索代码

现在我们开始有趣的部分 python 代码。

出于几个原因，这不是一个完整的脚本，因为有些东西在我自己的代码中是不相关的(我太懒了，不会为本文重写一个工作模板)，但没有什么重要的东西会被遗漏。但是，如果你被卡住了，就留下你的评论，我会试着给你一个答案——不像一些作者，我会检查并试着回复。

首先，让我们做重要的事情:

```
import pandas as pd
import datapungi_fed as dpfdata = dpf.data("put your API code here")# Note: please change your database, username & password as per your own values
conn_params_dic = {
  "host": "address of database",
  "database": "name of database",
  "user": "user of database",
  "password": "password of database"
}
```

我们已经做了两件事:1)我们为 API 键设置了一个变量，2)为数据库定义了一个连接字符串。这两者都允许访问我们需要的两个外部资源。

```
def get_report(report_name):
  try:
    df = data.series(report_name)
    df.rename(columns={report_name: 'data'}, inplace=True)
    df['report'] = report_name
    df.reset_index(drop=False, inplace=True)
  except:
    df = pd.DataFrame(columns = ['date', 'data', 'report'])
    print(report_name + ' Empty Set')
  return df
```

接下来，我创建了一个快速实用函数，它将一个报告名称作为输入，并使用创建的数据对象来检索报告，该报告以一系列名为 date 和 report_name 的列作为列。因为我将所有的报告存储在一个带有报告名称标识符的大表中，所以我想保持一致，所以我将 report_name 列重命名为“data”。然后，我创建“report”列来保存名称，并删除索引，默认情况下是日期。

这就把所有的东西都放到了正确的格式中。

如果有一个空报告(这种情况时有发生),我只需返回一个带有标题的空数据帧并打印一个警告。

准备工作结束后，让我们先做例外，也就是发布的报告，并把它们处理掉。

```
released = data.releases()released.drop(columns=['id', 'realtime_end', 'press_release'], inplace = True)released.rename(columns={'realtime_start': 'Date'}, inplace=True)
released.rename(columns={'name': 'Report Name'}, inplace=True)
released.rename(columns={'link': 'Link'}, inplace=True)
released.rename(columns={'notes': 'Notes'}, inplace=True)released['Report Name'] = released['Report Name'].str.replace(r'"', '')
released['Report Name'] = released['Report Name'].str.replace(r',', '')
released['Link'] = released['Link'].str.replace(r'"', '')
released['Link'] = released['Link'].str.replace(r',', '')
released['Notes'] = released['Notes'].str.replace(r'"', '')
released['Notes'] = released['Notes'].str.replace(r',', '')
released['Notes'] = released['Notes'].str.replace(r'\n', '')
released['Notes'] = released['Notes'].str.replace(r'\r', '')
released['Notes'] = released['Notes'].str.replace(r'\\', '')released['hash'] = released.apply(lambda x: hash(tuple(x)), axis=1)
```

这是一段可怕的代码，但是完成了工作。如果我更有雄心，或者这不仅仅是一个一次性的，我可能会做更多的事情来将其容器化，使其更便于携带，但它就是这样。

在这里，我们获取新的报告发布(目的地为 fed_releases 表)。这存储在“发布”中。接下来，我删除了对我不重要的列，比如 ID、realtime_end 和 press_release。

接下来，我重命名剩余的列。这并不是绝对必要的，而是作为我使用这些数据的一种方法的延续，所以它被留在了这个过程中。

之后，需要清理内容。对这些数据没有太多的关注，所以会有很多错误的格式出现。一些简单的替换就可以解决问题。

因为在使用 COPY 命令将数据复制到数据库之前，首先要将数据移动到 CSV 中，所以大多数问题都与转换有关。我们删除了逗号、引号、反斜杠字符，当然还有换行符。任何这些都会很快破坏你的一天，并导致一些非常无用的错误信息。

最后一步是使用 python 散列库进行散列。

但是，Brad，我们刚刚在上一节中讲述了使用 md5 函数对数据库中的数据进行哈希运算的整个过程，这是怎么回事呢？

简短的回答是，有时候，这个实例中 python 散列函数的结果是不一致的。我发现欺骗给我带来了痛苦，所以这是一次学习的经历。

我把这个留在这里是有原因的。我的表上的哈希值被定义为主键。我可以放弃它，使字段为空，或者我可以做一些其他的工作，但是我意识到在导入后用一个新的散列来更新这些值要简单得多。我选择了最省力的方法。如果你想继续下去，你可能会有更好的结果，但我知道我目前的设置是可靠的。

跳过这一步，我们现在准备进行第一次插入。我倾向于使用我为此定义的一些其他函数，但我也将它们放在这里供您参考:

```
conn = dbf.connect(conn_params_dic)
conn.autocommit = True
dbf.copy_from_dataFile(conn, released, 'fed_releases_tmp')
```

dbf 引用的是我拥有的一个单独的模块，它只包含我使用的数据库函数。我有连接函数和复制数据文件函数。以下是这些函数供参考:

```
def connect(conn_params_dic):
  conn = None
  try:
    conn = psycopg2.connect(**conn_params_dic)
    if debug == 1:
      print("Connection successful..................")
  except OperationalError as err:
    show_psycopg2_exception(err)
    # set the connection to 'None' in case of error
    conn = None
  return conndef copy_from_dataFile(conn, df, table):
  tmp_df = "/tmp/insert_data.csv"
  df.to_csv(tmp_df, header=False, index=False)
  f = open(tmp_df, 'r')
  cursor = conn.cursor()
  try:
    cursor.copy_from(f, table, sep=",")
    conn.commit()
    print("Data inserted successfully....")
    cursor.close()
  except (Exception, psycopg2.DatabaseError) as error:
    os.remove(tmp_df)
    # pass exception to function
    show_psycopg2_exception(error)
    cursor.close()
  os.remove(tmp_df)
```

此外，还使用这两个函数:

```
def show_psycopg2_exception(err):
  # get details about the exception
  err_type, err_obj, traceback = sys.exc_info()
  line_n = traceback.tb_lineno
  print("\npsycopg2 ERROR:", err, "on line number:", line_n)
  print("psycopg2 traceback:", traceback, "-- type:", err_type)
  print("\nextensions.Diagnostics:", err.diag)
  print("pgerror:", err.pgerror)
  print("pgcode:", err.pgcode, "\n")def query_return(query):
  conn = connect(conn_params_dic)
  try:
    query_1 = pd.read_sql_query(query, conn)
    df = pd.DataFrame(query_1)
  except (Exception, psycopg2.Error) as error:
    print("Error while fetching data from PostgreSQL", error)
  finally:
    if conn:
      conn.close()
  return df
```

总的来说，这些函数允许建立 psycopg2 连接、通过 copy 命令插入数据、处理异常和进行选择查询。

这些可以合并到您的主获取文件中，或者像我所做的那样，放在一个单独的模块中进行访问。这完全取决于您构建应用程序的内容和方式。

# 中途总结

哇，我们已经谈了很多了。让我们先喘口气，回顾一下。

我们已经创建了数据库，并设置了保存数据的模式。我们定义了在后端操作数据的函数，并处理所有数据，因此我们有增量更新。

在我们的数据获取过程中，我们已经从美联储建立了到数据库和 API 的连接。我们已经获得了新发布报告的列表，对它们进行了不良字符处理，并运行了将它们插入数据库的函数。

现在，您的 fed_releases_tmp 表应该已经塞满了准备处理的新报告。

让我们进入这个旅程的最后一部分，使用一个报告标识符列表来获取实际的时间序列报告数据，将它们插入到数据库中，然后完成，只留下填充的主表。

# 检索代码第 2 部分

我们的下一步是定义我们想要的报告列表。确定你感兴趣的是你的事，你需要花一些时间在 https://fred.stlouisfed.org/浏览他们的报告清单，以确定你想要什么。但是，一旦你有了这份清单，你的生活将会更加充实，有最新的经济数据可以戳戳戳，直到你心满意足。

以下是我的清单的一部分:

```
report_list = [
  'WM1NS', # M1 Supply
  'WM2NS', # M2 Supply
  'ICSA', # Unemployment
  'CCSA', # Continued Unemployment
  'JTSJOL', # Job Openings: Total Nonfarm
  'PAYEMS', # Non-Farm Employment
  'RSXFS', # Retail Sales
  'TCU', # Capacity Utilization
  'UMCSENT', # Consumer Sentiment Index
  'BUSINV', # Business Inventories
  'INDPRO', # Industrial Production Index
  'GACDFSA066MSFRBPHI', # Philidelphia Fed Manufacturing Index
  'GACDISA066MSFRBNY', # Empire State Manufacturing Index
  'BACTSAMFRBDAL', # Current General Business Activity; Diffusion Index for Texas
  'IR', # Import Price Index
  'IQ', # Export Price Index
  'PPIACO', # Producer Price Index - all
  'CPIAUCSL', # Consumer Price Index - all
  'CPILFESL', # Consumer Price Index (Core)
  'MICH', # University of Michigan: Inflation Expectation
  'CSCICP03USM665S', # Consumer Opinion Surveys: Confidence Indicators: Composite Indicators: OECD Indicator for the United States
]
```

显然，这是所有的地图，但显示了很多共同的经济指标。但是，我们需要处理这个:

```
# Pull all the reports into a big honking dataframe
all_rep = []
for i in report_list:
  df1 = get_report(i)
  all_rep.append(df1)df = pd.concat(all_rep)
df['hash'] = df.apply(lambda x: hash(tuple(x)), axis=1)
```

非常简单，我创建 all_rep 列表作为一个空的占位符。从那里，我读取我的 report_list 列表，并为每个报告调用 get_report 函数，将结果放入 df1。我取结果并将 df1 附加到 all_rep。

我做了最后的连接，然后将结果数据散列到一个名为“hash”的新列中。参见我之前对这个问题的讨论。

现在，我们有了可以输入到函数中并插入的最终系列:

```
conn = dbf.connect(conn_params_dic)
  conn.autocommit = True
  dbf.copy_from_dataFile(conn, df, 'fed_reports_tmp')
```

这与我们对美联储新闻稿所做的一样。

至此，我们应该已经填充了两个 _tmp 表，并准备好进行处理。

这将引导我们进入最后一步:

```
# Final Cleanuptry:
  cursor = conn.cursor()
  query = "SELECT rehash_fed_reports_tmp()"
  cursor.execute(query)
  return_val = cursor.fetchall()
  conn.commit() cursor = conn.cursor()
  query = "SELECT rehash_fed_releases_tmp()"
  cursor.execute(query)
  return_val = cursor.fetchall()
  conn.commit() cursor = conn.cursor()
  query = "SELECT fed_report_update()"
  cursor.execute(query)
  return_val = cursor.fetchall()
  conn.commit() cursor = conn.cursor()
  query = "SELECT symbol_cleanup()"
  cursor.execute(query)
  return_val = cursor.fetchall()
  conn.commit() print("Records updated and cleaned up.......")except (Exception, dbf.psycopg2.Error) as error:
  print("Error while fetching data from PostgreSQL", error)finally:
  # closing database connection.
  if conn:
    cursor.close()
    conn.close()
    print("PostgreSQL connection is closed")
```

在这一步中，我们执行最后的 select 语句来运行我们之前定义的函数。前两个重写了 _tmp 表。第三个函数从 _tmp 表中更新主表。最后一个函数清除 _tmp 表，并为下一次运行做好准备。

最后的过程只是关闭我们的数据库连接，并通过一些友好的消息退出，让我们知道一切正常。

# 行程安排

我的最后一点是运行这个无人值守。为此，我有一个基本的 cron 条目，它在工作日每 6 小时运行一次该进程:

```
* */6 * * 1-5 /Library/Frameworks/Python.framework/Versions/3.9/bin/python3 ./data_miners/pull_fed_data.py 2>&1
```

# 结论

很难说是写整个过程还是这篇解释过程的文章花了更长的时间。这并不是说这篇文章花了很多时间，而是说一旦你掌握了节奏，获取数据、清理数据并将其放入数据库进行分析就相当容易了。

我鼓励完成这些类型的流程。最好的老师是逆境和排除错误。这在哪里可以改进？您看到了哪些错误？你有更好的解决办法吗？

但是，在数据科学和数据分析的世界里，获取数据只是第一步。以这篇文章为指导，你现在有一大堆热门的相关时间序列数据可以利用。你会用它做什么？