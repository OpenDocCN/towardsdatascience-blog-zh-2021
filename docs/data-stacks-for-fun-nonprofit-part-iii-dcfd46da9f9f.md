# 面向娱乐和非营利组织的数据堆栈—第三部分

> 原文：<https://towardsdatascience.com/data-stacks-for-fun-nonprofit-part-iii-dcfd46da9f9f?source=collection_archive---------17----------------------->

## 在本教程中，我们将介绍 DS4FNP 堆栈的每一步。

面向娱乐和非营利组织的数据堆栈是一系列文章和教程，旨在使用开源和开放核心组件设计和实现现代数据堆栈。DS4FNP 堆栈旨在为教育目的提供足够的可访问性(在成本和笔记本电脑可部署性方面)，但作为一个成熟的分析架构，适合各种规模的组织进行扩展。

在之前的一篇文章中，我描述并概述了由 Postgres、Meltano、Airflow、dbt 和 Apache 超集组成的本地可部署堆栈背后的基本原理。Postgres 数据库可以替换为其他数据库引擎，如 SQLite 和 MySQL，而不会有太大的影响。这种设置允许我们在本地环境中使用完整的数据栈，然后当我们准备好扩展到像 BigQuery 或 Snowflake 这样的云托管数据仓库时，这样做只是简单地切换一些配置文件。

我们现在准备好了一个实践教程，演示如何设置堆栈的每个组件和一些示例内容。我们将遍历堆栈中每个组件的设置，提取数据并将其加载到数据仓库中(好吧，现在实际上只是一个本地数据库)，并设计一些基本的分析仪表板。

# 找一些数据！

在开始之前，我们需要想出某种目的或场景来激励我们选择数据源和目标。这总是为如此多样的潜在用例设计教程的最困难的部分，因为你想尝试呈现一些与所有用例相关的东西。我认为一个理想的场景应该是涉及连续生成的数据，如体育统计数据或服务器日志，因为这些数据将允许我们根据定期安排的工作流和纵向图进行思考。然而，为了在我们跑之前学会走路，让我们把这些场景留到以后的教程中，现在集中在一些移动较慢的数据上。

过去几个月一直被关于 2020 年美国大选的新闻所占据，最终的回报现在可以从通常的来源获得，所以这似乎是一个及时而合适的主题。从分析的角度来看，让我们把兴趣放在比较最近和历史的选举结果上。

也许毫不奇怪，事实证明美国选举的数据分散得令人难以置信，在哪里找到或如何获得并不总是显而易见的，也缺乏任何一种共同的标准。有一些公共机构和学术研究人员勇敢地收集和发布了一些数据集，其中许多我已经开始在 https://github.com/andrewcstewart/awesome-democracy-data 进行分类和策划。如果你对与选举、选举改革和民主政治制度有关的数据感兴趣，我鼓励你去看一看！

我们将在本教程中使用的主要数据源来自[麻省理工学院选举数据+科学实验室](https://electionlab.mit.edu/data)。具体来说，我们对收集美国众议院、参议院和总统选举团候选人的历史选举结果感兴趣。

# 使用 Meltano 收集数据

本教程需要以下内容:

*   下载并安装 [PostgreSQL](https://www.postgresql.org/download/) 。
*   你可以使用 SQL 客户端:我推荐 [TablePlus](https://tableplus.com/) 或者 [pgcli](https://github.com/dbcli/pgcli) 。
*   栈中的大多数组件都依赖于 [Python3](https://www.python.org/) 。
*   我还推荐使用 IDE，比如 [VS Code](https://code.visualstudio.com/) (以及 [vscode-dbt](https://marketplace.visualstudio.com/items?itemName=bastienboutonnet.vscode-dbt) 扩展。)

我们开始吧！如果你只是想跟着做，在本教程中生成的所有代码/配置都可以在[https://github.com/andrewcstewart/ds4fnp](https://github.com/andrewcstewart/ds4fnp)找到。由于我们将要安装的许多(如果不是全部)组件都是通过 python 实现的，我强烈建议创建一个虚拟环境。

```
mkdir ds4fnp
cd ds4fnp                   
python3 -m venv .venv       # create your virtual environment
source .venv/bin/activate   # activate your virtual environment
```

我们将使用 Meltano 来管理我们的 ELT(而不是 ETL！)管道，每个管道将从某个源提取数据，并将数据加载到目标数据库中。如果我们手工开发自己的 ETL 解决方案，我们可能会为每个数据提取和加载过程编写大量的定制 python 代码，但幸运的是，开源的 [Singer](https://www.singer.io/) 标准将这些过程组合成可互换的“tap”(数据源)和“targets”(加载目的地)。Meltano 有助于抽象出许多配置细节，并提供了一个方便的执行层。Meltano 还可以处理下游的转换(通过 dbt)和编排(通过 Airflow)步骤，使它成为整个项目的方便的顶层控制器，但是出于说明的目的，我们将只使用 Meltano 进行提取和加载步骤。

## 设置 Meltano

设置 Meltano 很简单，只需安装它并创建一个新项目。

```
pip3 install meltano
meltano init ./meltano
cd ./meltano
```

如果你做了一个`ls`，你会注意到已经创建了几个子目录，还有一个`meltano.yml`文件。这个文件是我们要做大部分工作的地方，还有 Meltano 的 CLI。我建议浏览一下 Meltano 的文档，以便更好地了解项目配置文件是如何工作的。在大多数情况下，我们将使用 CLI 在`meltano.yml`中生成部分，然后根据需要手动编辑配置选项。

## 配置用于提取的源

现在我们已经建立了一个项目，我们可以添加和配置一些提取器(taps)和一个加载器(target)。Meltano 提供 Singer taps，从 Google Analytics、Zendesk、Salesforce 等，到简单的 CSV 文件或 SQL 数据库；然而，也有数不清的其他第三方歌手水龙头在野外为各种不同的数据来源。在本教程中，我们只是从 URL 中提取一些 CSV 文件，所以我们只需要将`[tap-spreadsheets-anywhere](https://meltano.com/plugins/extractors/spreadsheets-anywhere.html#getting-started)` [插件](https://meltano.com/plugins/extractors/spreadsheets-anywhere.html#getting-started)添加到我们的项目中。这是一个非常通用的 tap，可以让我们从许多来源提取 CSV 和 Excel 文件，包括 http/s、s3、sftp 等等。

```
meltano add extractor tap-spreadsheets-anywhere
```

这个命令将在我们的项目中安装插件，并向我们的`meltano.yml`添加一个配置部分。有些 tap 在 CLI 中配置起来很简单，但是对于`tap-spreadsheets-anywhere`，我们需要在`tables:`下创建很多嵌套的项目，所以手动编辑文件可能更容易。tap 的[文档](https://github.com/ets/tap-spreadsheets-anywhere)描述了如何配置 tap 以读取特定源。

为了演示，我们将从 MIT Dataverse 添加三个数据集的 URL，以便 tap 从中提取:

*   美国众议院选举:https://dataverse.harvard.edu/api/access/datafile/4202836
*   美国参议院选举:https://dataverse.harvard.edu/api/access/datafile/4300300
*   美国总统选举:https://dataverse.harvard.edu/api/access/datafile/4299753

这些 URL 中的每一个都解析为一个制表符分隔的文件，所以我们将告诉 tap 期待一个“csv”文件，但是带有“\t”分隔符。我们的`meltano.yml`看起来像这样:

```
version: 1
send_anonymous_usage_stats: true
project_id: xxx
plugins:
  extractors:
  - name: tap-spreadsheets-anywhere
    variant: original
    pip_url: git+https://github.com/ets/tap-spreadsheets-anywhere.git
    capabilities:
    - catalog
    - discover
    - state
    config:
      tables:   
      - path: [https://dataverse.harvard.edu/api/access/datafile/](https://dataverse.harvard.edu/api/access/datafile/)
        name: mit__house_elections
        pattern: "4202836"
        start_date: '2010-01-01T00:00:00Z'
        key_properties: []
        format: csv
        delimiter: "\t"
      - path: [https://dataverse.harvard.edu/api/access/datafile/](https://dataverse.harvard.edu/api/access/datafile/)
        name: mit__senate_elections
        pattern: "4300300"
        start_date: '2010-01-01T00:00:00Z'
        key_properties: []
        format: csv
        delimiter: "\t"
      - path: [https://dataverse.harvard.edu/api/access/datafile/](https://dataverse.harvard.edu/api/access/datafile/)
        name: mit__president_elections
        pattern: "4299753"
        start_date: '2010-01-01T00:00:00Z'
        key_properties: []
        format: csv
        delimiter: "\t"
```

## 配置要加载的目标

接下来，我们需要添加一个 Singer 目标来告诉 Meltano 我们希望它将数据加载到的 Postgres 数据库。在此之前，我们需要在 Postgres 中创建一个数据库和权限。出于本教程的目的，我们将创建一个名为“ds4fnp”的 db，以及一个同名的用户。

```
create database ds4fnp;
create user ds4fnp with password 'ds4fnp';
grant all privileges on database ds4fnp to ds4fnp;
```

然后我们可以将`target-postgres`加载器添加到我们的项目中，类似于我们添加 taps 的方式。

```
meltano add loader target-postgres --variant meltano
```

安装插件后，`meltano.yml`现在包含了一个`loaders`部分，包含了我们的特定目标。我们可以将目标配置为使用我们刚刚创建的数据库和凭证。

```
loaders:
  - name: target-postgres
    pip_url: git+https://github.com/meltano/target-postgres.git
    config:
      user: ds4fnp
      host: 127.0.0.1
      port: 5432
      dbname: ds4fnp
```

## 运行 ELT 工作流

提取器和加载器都准备好了，我们现在可以定义和运行管道了。使用 Meltano 的 CLI，我们只需运行`meltano elt [TAP] [TARGET]`，就像这样:

```
meltano elt tap-spreadsheets-anywhere target-postgres
```

这将运行一段时间，因为 Meltano 从我们的源中提取所有数据并加载到我们的数据库中。在我们的`ds4fnp`数据库中，我们现在有了一个以 tap 命名的新模式，它包含了我们在`meltano.yml`中指定的所有目标表。比如`ds4fnp.tap_spreadsheets_anywhere.mit__house_elections`。为了验证一切顺利，我们可以使用我们选择的 SQL 浏览器在我们的仓库中查找这些表。

我们可以使用`pgcli`通过运行以下命令来浏览数据库:

```
pgcli postgresql://ds4fnp:ds4fnp@127.0.0.1/ds4fnp
```

或者如果你喜欢更直观的体验，我们可以使用 TablePlus:

![](img/20ec1690b7be4b869891f4771a97b294.png)

Meltano (TablePlus SQL 客户端)新加载的数据。

太好了！我们现在有一些原始数据加载到我们的数据库中。我们称之为“原始”数据，因为它包含许多不完美和不确定的质量，最终我们希望将其提炼为更直接有用的产品。整个原油管道的类比在这里非常适用。这就是转型的切入点。我们希望通过数据转换来处理的问题类型的一些示例:

*   `mit__house_elections`表格中的所有字符串字段都是大写的，而在其他表格中则不是。我们应该与大写保持一致。
*   有些字段使用“名字，姓氏”，而有些字段使用“姓氏，名字”。我们应该保持名称格式的一致性。
*   在分析过程中，有许多我们可能不关心的元数据字段。我们可以把这些过滤掉。
*   这些选举结果的粒度级别非常低，而我们可能希望处理更高级别的聚合数字。

然而，我们将在下一步处理转换。实际上，将原始数据保持为最初加载的状态，并将转换视为原始数据的衍生物是有价值的。这样做有助于我们保存数据的来源(T2 ),一个可审计的数据历史记录。它还允许我们重新访问和修改我们的转换任务，如果我们不保存原始数据，我们就不能这样做。

正如我前面提到的，Meltano 实际上可以将 dbt 作为一个转换层合并到它自己的项目配置中，但是现在我们将自己一步一步地完成 dbt。

# 使用 dbt 转换和建模数据

[dbt](https://www.getdbt.com) 代表“数据构建工具”。它是一个开源项目，为定义和管理数据仓库中的分析工作流提供了一个开发环境。Singer(通过 Meltano)从一个或多个外部源将数据加载到数据库中，而 dbt 将数据从一个或多个现有的表(例如，原始数据表)转换到一个新的表中。这是通过用基本的 SQL 查询定义数据模型来实现的，这些查询利用 Jinja 模板系统来动态引用其他表。如果您曾经在 Flask 这样的框架中创建过 web 应用程序，您可以在其中以编程方式填充内容模板化的 HTML，这与 SQL 的情况非常相似。当您运行 dbt 时，所有这些模板化的查询和元数据都被编译成表/视图定义，并在我们的目标数据仓库上执行。花点时间在这里阅读更多。

## 设置 dbt

设置 dbt 和梅尔塔诺很像。我们从 pip 安装，并用 dbt CLI 创建一个新项目。

```
cd ..                   # go back to the ds4fnp base pathpip3 install dbt        # install dbt
dbt init dbt            # create a new dbt project called 'dbt'
cd dbt                  #
```

我们现在有了一个新的项目框架，包括几个子目录和一个类似于`meltano.yml`配置文件的`dbt_project.yml`配置文件。简而言之，dbt 项目由以下组件组成:

*   **sources** 是对我们的数据仓库中现有位置的引用，我们的加载流程将原始数据存放在这些位置。这些在`models`目录下定义。
*   **模型**是模板化的查询，它定义了 dbt 将在我们的数据仓库中具体化为新表/视图的选择和转换。模型可以从源中选择，也可以从其他模型中选择。这些也在`models`目录下定义。
*   `**data**`是一个包含 CSV 格式的平面文件的目录，dbt 会自动将这些文件加载到我们的数据仓库中；这对于小型不可变的查找表很有用。
*   `**macros**`包含模板引擎在定义查询时可以使用的函数。我们不会在这里过多地讨论宏，但是它们提供了 dbt 背后的真正力量。
*   `**analysis**`查询类似于模型，只是 dbt 只将它们编译成呈现的 SQL，而不在数据仓库中执行它们。我发现这些对于测试新的查询想法很有用，而不需要将它们合并到我的整体数据模型中，并且可以预先生成查询，用于下游分析和 BI 平台。
*   `**docs**`包含 dbt 生成的完整静态文档网站。这是 dbt 的一个非常棒的特性，也是让同事能够访问您的数据仓库的关键。
*   `**tests**`提供一种将单元测试概念应用到项目设计中的方法。
*   `**snapshots**`提供一种捕获和保存不维护自身历史的源的历史记录的方法。
*   `**target**`是 dbt 在对目标数据仓库执行之前编写已编译查询的地方。

在大多数情况下，我们将只触及其中的几个组件；即来源和模型。

我们还需要配置一个`profiles.yml`文件，它将包含我们的数据库凭证，默认情况下位于项目目录之外的`~/.dbt/profiles.yml`下。我建议阅读关于配置文件配置的[文档。](https://docs.getdbt.com/dbt-cli/configure-your-profile/)

您可以配置多个概要文件，然后轻松地针对多个目标部署您的 dbt 项目。例如，出于开发目的，您可以部署到本地 postgres 数据库，然后部署到 BigQuery 作为生产数据仓库。这里有一个例子`profiles.yml`让我们开始:

```
# For more information on how to configure this file, please see:
# [https://docs.getdbt.com/docs/profile](https://docs.getdbt.com/docs/profile)ds4fnp:
  outputs:
    local:
      type: postgres
      host: 127.0.0.1
      user: ds4fnp
      pass: ds4fnp
      port: 5432
      dbname: ds4fnp
      schema: public
  target: local
```

有了我们的概要文件设置，接下来我们要做的事情是转向项目配置。在 dbt 项目中，我们将基本上处理三个不同的文件:

*   `dbt_project.yml` -该文件通知 dbt 项目的结构，以及一些通用的配置选项。
*   `schema.yml` -数据模型是通过 yaml 和 sql 文件的目录结构定义的。`schema.yml`文件列举并描述了每个模型的元数据。
*   `*.sql` -每个模型由一个 jinja 模板化的 SQL 文件定义。除了对数据库对象的直接引用将被模板引擎使用的宏所取代之外，这些文件看起来与您可能见过的大多数 SQL 文件非常相似。

与其一步一步地设计整个项目配置，不如让我们看一个完整的配置。我将把`dbt_project.yml`文件的基本概述留给正式的[文档](https://docs.getdbt.com/reference/dbt_project.yml)。

```
# Name your project! Project names should contain only lowercase characters
# and underscores. A good package name should reflect your organization's
# name or the intended use of these models
name: 'ds4fnp'
version: '1.0.0'
config-version: 2# This setting configures which "profile" dbt uses for this project.
profile: 'ds4fnp'# These configurations specify where dbt should look for different types of files.
# The `source-paths` config, for example, states that models in this project can be
# found in the "models/" directory. You probably won't need to change these!
source-paths: ["models"]
analysis-paths: ["analysis"]
test-paths: ["tests"]
data-paths: ["data"]
macro-paths: ["macros"]
snapshot-paths: ["snapshots"]target-path: "target"  # directory which will store compiled SQL files
clean-targets:         # directories to be removed by `dbt clean`
    - "target"
    - "dbt_modules" # Configuring models
# Full documentation: [https://docs.getdbt.com/docs/configuring-models](https://docs.getdbt.com/docs/configuring-models)# In this example config, we tell dbt to build all models in the example/ directory
# as tables. These settings can be overridden in the individual model files
# using the `{{ config(...) }}` macro.
models:
  ds4fnp:
    staging:
        +schema: staging
        +materialized: view
    # marts:
    elections:
        +schema: elections
        +materialized: view
```

除了几处编辑之外，这几乎是 dbt 在初始化期间为我们生成的常用配置文件。我们更改的主要内容是将`profile:`指向我们上面创建的概要文件，以及`models:`部分下的所有内容。有几种方法来构建您的数据模型，但是这里我们将保持事情相当简单。我只定义了两个模式:

*   `staging`将包含对我们数据源的引用。
*   `elections`将包含我们将在下游使用的转换模型。

## 定义来源和临时表

首先让我们在`models/staging/schema.yml`中定义我们的分段模式。从 Meltano，我们将三个不同的来源加载到我们的仓库中(众议院、参议院和总统选举结果)。

```
version: 2sources:
  - name: mit
    loader: meltano
    database: ds4fnp
    schema: tap_spreadsheets_anywhere
    tables:
      - name: mit__house_elections
        description: MIT house election data
      - name: mit__senate_elections
        description: MIT senate election data
      - name: mit__president_elections
        description: The data file `1976-2016-president` contains constituency (state-level) returns for elections to the U.S. presidency from 1976 to 2016\.  The data source is the document "[Statistics of the Congressional Election](http://history.house.gov/Institution/Election-Statistics/Election-Statistics/)," published biennially by the Clerk of the U.S. House of Representatives.
        columns:
          - name: candidate
            description: name of the candidate 
          - name: candidatevotes
            description: votes received by this candidate for this particular party
          - name: district
            description: None
        ...
models:
  - name: stg_us_house_election_results
    description: US House election results
  - name: stg_us_senate_election_results
    description: US Senate election results
  - name: stg_us_president_election_results
    description: US President election results
```

在这个文件中，我们在`sources:`下定义了源表的位置，但是我们也在`models:`下定义了一组相应的“staging”模型。每个`name:`字段的值必须对应一个`.sql`文件，我们也将创建这个文件。我们为每个数据源定义分段模型的原因是，我们可能希望在多个数据模型中使用同一个数据源，但是我们不一定希望在每个实例中定义相同的过滤和数据清理。有了分段模型，我们可以在一个地方一次性定义这些转换，这样对数据源的任何修改都可以反映在引用它的每个下游模型中。`schema.yml`文件还允许我们描述和记录表中的字段，这将在以后派上用场。

## 定义数据模型

对于模型定义，我只使用众议院选举作为例子，因为其他两个看起来相似，完整的代码可以从前面提到的 Github 库中获得。我们将在`models/staging/stg_us_house_election_results.sql`中定义众议院选举结果的阶段模型:

```
select 
    to_date(year::VARCHAR, 'YYYY') as year,
    INITCAP(candidate) as candidate,
    INITCAP(party) as party,
    INITCAP(state) as state,
    state_po,
    COALESCE(district, '1') as district,
    candidatevotes::INTEGER as candidate_votes,
    totalvotes as total_votes,
	ROUND(candidatevotes::numeric / totalvotes::numeric * 100, 2) as vote_pct
from {{ source('mit','mit__house_elections') }}
where writein::BOOLEAN is False
```

从这个分段模型中我们可以看到，dbt 中的模板化 SQL 与常规 SQL 非常相似，只是我们在查询的`from`子句中使用了一个`{{ source() }}`宏。当 dbt 运行时，它将使用项目配置中的适当信息填充宏。在这个 staging 模型中，我们基本上是按原样选择源表，只是我们限制了返回的字段，并对其中的一些字段执行了一些转换。例如:

*   我们已经将`date`字段转换为实际的日期戳类型。
*   我们用`INITCAP()`函数修正了很多字段的全大写字符串。
*   我们在`district`列中插入了一些缺失的值。

仅举几个例子。然后，我们对参议院和总统选举数据来源重复这一过程。既然我们已经定义了我们的源和阶段模型，我们可以开始设计我们计划在未来的分析和超集中使用的数据模型。原始数据提供了每一个候选人(包括非推荐候选人、小党派等)和每个州/地区的选举结果，让我们在党派层面总结结果。对于这些模型，我们将在`models/elections/schema.yml`下创建一个新的模式:

```
version: 2models:
  - name: us_house_party_seats
    description: Seats in the US House of Representatives by party.
  - name: us_senate_party_seats
    description: Seats in the US Senate by party.
  - name: us_president_popular_vote
    description: Popular vote in US presidential elections by party.
```

这应该类似于我们上面定义的阶段模型。为了总结众议院的结果，让我们定义一个数据模型查询，该查询根据收到的选票对候选人进行排名和筛选，然后最终汇总每个选举年每个政党赢得的席位数。

```
WITH 
sub1 AS (
    SELECT 
        *,
        rank() OVER (PARTITION BY year,
        state,
        district ORDER BY candidate_votes DESC) AS rank
    FROM {{ ref('stg_us_house_election_results') }}
),
sub2 AS (
    SELECT 
        *, 
        rank = 1 AS winner
    FROM sub1
)SELECT 
	year,
	party,
	count(*) as seats
FROM sub2
WHERE winner = True
GROUP BY year, party
```

我们的查询看起来与 staging 模型中使用的非常相似，主要区别是使用了`{{ ref() }}`宏而不是`{{ source() }}`宏。其他查询可以在 Github 存储库中找到。

## 运行 dbt

配置好项目后，我们现在可以运行 dbt 了！请注意，在实践中，您可能希望在验证您的工作时经常运行项目。为此，我们只需从命令行运行`dbt run`。我将在下面的代码片段中包含终端输出:

```
dbt runRunning with dbt=0.18.1
Found 6 models, 0 tests, 0 snapshots, 1 analysis, 138 macros, 0 operations, 0 seed files, 3 sources14:08:49 | Concurrency: 1 threads (target='local')
14:08:49 | 
14:08:49 | 1 of 5 START view model dbt_staging.stg_us_house_election_results.... [RUN]
14:08:49 | 1 of 5 OK created view model dbt_staging.stg_us_house_election_results [CREATE VIEW in 0.23s]
14:08:49 | 2 of 5 START view model dbt_staging.stg_us_senate_election_results... [RUN]
14:08:49 | 2 of 5 OK created view model dbt_staging.stg_us_senate_election_results [CREATE VIEW in 0.08s]
14:08:49 | 3 of 5 START view model dbt_staging.stg_us_president_election_results [RUN]
14:08:49 | 3 of 5 OK created view model dbt_staging.stg_us_president_election_results [CREATE VIEW in 0.08s]
14:08:49 | 4 of 5 START view model dbt_elections.us_house_party_seats........... [RUN]
14:08:50 | 4 of 5 OK created view model dbt_elections.us_house_party_seats...... [CREATE VIEW in 0.09s]
14:08:50 | 5 of 5 START view model dbt_elections.us_senate_party_seats.......... [RUN]
14:08:50 | 5 of 5 OK created view model dbt_elections.us_senate_party_seats..... [CREATE VIEW in 0.07s]
14:08:50 | 
14:08:50 | Finished running 5 view models in 1.26s.Completed successfullyDone. PASS=5 WARN=0 ERROR=0 SKIP=0 TOTAL=5
```

厉害，我们的项目编译运行成功！您可以在 dbt 运行时跟踪进度，如果出现问题，它通常会提供有用的诊断信息。在我们深入数据仓库查看 dbt 生成的所有内容之前，让我们快速回顾一下生成 dbt 的文档。我们只需要另外两个命令就可以做到:

```
dbt docs generate
dbt docs serve
```

这将生成您的数据仓库的文档，并启动一个临时 web 服务器，以便您可以查看它。花几分钟时间在浏览器中浏览生成的站点。

![](img/c581bda5fbf8238b56a61b4dccee0cb9.png)

dbt 的自动生成文档。

dbt 的一个很棒的特性是站点右侧的图形，它允许您可视化各种视图和表之间的关系。注意，与您可能在其他地方看到的 ERD(实体关系图)不同，这里的表之间的关系是沿袭的；即哪些表是其他表的源。随着项目复杂性的增加，这些图表和自动生成的文档通常会变得非常有价值。

![](img/66f216c6a3b99e0f427a78bd410ebd28.png)

dbt 自动生成的文档中的谱系图。

此时，您还可以检查在本地 postgres 数据仓库中生成的表。我们将在下一节中更直接地研究它们。

# 使用超集分析和可视化数据

最后，我们准备好对新填充的数据仓库做一些事情。通常情况下，任何分析项目都会有某种 BI 层，具有交互式数据可视化，可以根据我们仓库中的数据进行动态更新。当然，在本教程中，我们有意使用不经常更新的数据源(美国大选每两年才举行一次)。然而，我们可以[设置 Apache 超集](https://superset.apache.org/docs/installation/installing-superset-from-scratch)来演示如何完成这个一般任务。

我们将再次在终端运行一系列命令:

```
pip3 install apache-superset                # install superset
superset db upgrade                         # initialize superset's db
superset fab create-admin                   # create an admin user
superset init                               # set a bunch of defaults 
superset run -p 8088 --with-threads --reload --debugger     # run it!
```

其中一些步骤可能涉及到一些交互，之后您应该有一个正在运行的超集实例，您可以从浏览器的 [http://localhost:8088](http://localhost:8088) 访问它。此时，我还建议快速浏览一下[超集的文档](https://superset.apache.org/docs/intro)。因为本教程的其余部分与命令行无关，所以我将首先列出在超集中创建仪表板的步骤，然后我们将逐一介绍。如果你使用过商业 BI 工具，比如 Tableau 或者 Looker，这看起来会非常相似。

*   `Configure a data source`
*   `Create datasets`
*   `Create charts`
*   `Create dashboards`

如果你想直接跳到成品，你可以导入我已经导出到 Github 库的`superset`目录下的数据和仪表板配置:

```
superset import-datasources -p datasources.yml
superset import-dashboards -p dashboards.json
```

## 配置数据源

我们只需要建立到仓库的连接作为数据源。您可以通过“数据”菜单下的“数据库”添加新的连接。超集使用 SQLAlchemy 来管理数据库连接，因此您的连接字符串将如下所示:`postgresql+psycopg2://ds4fnp:ds4fnp@localhost:5432/ds4fnp`

![](img/438461a081baa110002b679757c3b81d.png)

在超集中配置数据源连接。

在我们进入数据可视化之前，值得指出的是，超集有一个方便的内置 SQL 客户端，称为“SQL Lab”。一旦添加了数据源，就可以使用 SQL 实验室来浏览数据库表并运行 SQL 查询。这对于运行特别分析非常有用，也有助于 Meltano 和 dbt 的开发。

![](img/67ea0f1a3fd5ff577e93c43d3c6f0a55.png)

超集中的 SQL 实验室接口。

## 创建数据集

“数据”菜单下还有“数据集”，通常是从您的仓库中选择的表。要创建一个，您需要选择数据库、模式和表，然后单击 save。

![](img/48137b378051aa1d707761ae48d26453.png)

在超集中设置数据集。

## 创建图表

图表是超集的核心。创建一个数据集包括指定要使用的数据集，然后将各种视觉美感映射到数据集中的字段。我们将在这里创建一个非常简单的示例:

*   创建新图表。
*   选择“dbt _ elections . us _ house _ party _ seats”作为数据集，选择“面积图”作为图表类型。
*   选择`year`作为时间列，以及时间粒度。
*   将时间范围设置为“无过滤器”。
*   在 metrics 下，选择`seats`作为列，选择`SUM`作为聚合。
*   选择`party`进行分组。

我还把 Customize 下的配色改成了“Google Category 10c”，大多是把民主党映射成蓝色，共和党映射成红色。

![](img/1ac37f0d0c8a6dbd7e05153719b964cf.png)

超集的图表编辑器。

重复此过程，为参议院和总统选举结果创建类似的图表。

注意:是的，我意识到在总统选举图表中，民主党是红色的，共和党是蓝色的。为了解决这个问题，我们可能希望在表中创建一个额外的标准化 party 字段，以便控制分组和排序的工作方式。

## 创建仪表板

超集中的仪表板提供了一个画布，用于将图表和其他工件组装到一个方便用户查看和交互的页面中。超集使得创建新的仪表板和将图表拖放到画布上变得非常容易。查看超集仪表板上的 [Preset.io 文档](https://docs.preset.io/v1/docs/en/about-dashboards)，了解如何定制所有各种[仪表板功能](https://docs.preset.io/docs/dashboard-features)的更多信息。

对于本教程，我简单地创建了一个新的仪表板，并将我们刚刚创建的每个图表拖到它上面。

![](img/8abebda3d51a309f059ae921bf74f791.png)

超集仪表板编辑器。

一旦你有了你想要的仪表板，只需保存它并点击“草稿”开关将仪表板设置为已发布。

![](img/0fc5f91d8f27e75a990fc7d1bfa2823a.png)

我们在超集中发布的仪表板！

这看起来是一个很好的开始！从这里开始，您可以尝试创建更多的图表和仪表板。您可能还想回到您的数据模型，并创建可以支持其他类型图表的附加表/视图。如果您觉得特别大胆，您甚至可以尝试在 Meltano 中添加额外的加载器，并在您的 dbt 项目中组合多个数据源。

# 包扎

概括地说，在本教程中，我们有:

*   安装并配置了 DS4FNP 堆栈的每一部分。
*   创建了一个 postgres 数据库作为我们的本地开发仓库。
*   用 Meltano 从外部来源提取的数据加载我们的仓库。
*   使用 dbt 将原始数据转换成精确的数据模型。
*   从超集中的数据模型创建可视化。

希望您现在已经很好地掌握了堆栈中的技术，以及它们是如何结合在一起创建现代数据分析系统的。我们所涵盖的绝不是开发一个完整的生产质量系统所涉及的范围，但它确实建立了一个基础。

在本教程中，我们只是使用了一个运行在本地机器上的简单 postgres 实例，这样做对于本地开发目的来说确实是完全有效的，但是在某些时候，我们希望利用一个可扩展的基于云的仓库，比如 BigQuery 或 Snowflake。幸运的是，从本地开发仓库到云生产仓库的过渡非常容易。假设我们有一个在某处运行的预配实例，我们只需将它的连接和凭证添加到 Meltano 和 dbt 中的项目配置中。这些工具使得在部署目标之间切换变得非常容易。

对于真实的生产环境，您可能也不想从本地机器上运行这些工具。相反，您会希望使用基于云计算的方式来执行您的管道。你可以手工推出自己的解决方案，但幸运的是，现有的云平台易于使用。例如，dbt 的创建者将 [dbt Cloud](https://www.getdbt.com/product/) 作为托管服务提供，该服务“配备了对调度作业、CI/CD、服务文档、监控&警报和集成开发环境(IDE)的交钥匙支持。”

说到 CI/CD，通过配置和代码定义数据仓库的一个好处是，您可以使用版本控制系统(如 git)来管理这些文件，并将管道的执行视为类似于以仓库为构建目标的构建过程。如果你现在还不明白这些是什么意思，我们将在后面的教程中讨论一些 DevOps 主题。

虽然您的数据管道只能在需要时运行(无论是触发的还是)，但您很可能希望超集连续运行，以便可以在需要时访问它。在生产环境中，您可能希望将它部署为托管 web 服务。类似于 dbt Cloud 如何提供运行 dbt 的托管服务， [Preset.io](https://preset.io/) 提供运行超集的托管服务。

我们在本教程中使用的示例有些过于简单，以便呈现整个堆栈和过程的概览，但后续教程将进一步深入到特定方面，并包括其他主题。其中包括配置 CI/CD 工作流、更高级的数据源提取、工作流与气流的协调、设计指标以及使用云仓库。

希望你觉得这个教程有用！