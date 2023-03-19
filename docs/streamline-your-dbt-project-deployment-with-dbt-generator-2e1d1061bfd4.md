# 使用 dbt-generator 简化您的 dbt 项目部署

> 原文：<https://towardsdatascience.com/streamline-your-dbt-project-deployment-with-dbt-generator-2e1d1061bfd4?source=collection_archive---------11----------------------->

## 运行 50 个命令来生成基本模型？为基础模型编写 100 次相同的变换？这个软件包将为您简化这一过程。

![](img/7b2f40d7ba82db1341a3799bb6788db1.png)

照片由[莱尼·屈尼](https://unsplash.com/@lennykuhne?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

我们热爱 dbt，我们几乎在所有的咨询项目中都使用它。在 [dbt hub](https://hub.getdbt.com/) 中有许多很酷的预制软件包，可以显著提高您的工作效率。例如，Fivetran 为 Hubspot、Salesforce、Google Ads 和脸书广告等许多常见来源创建了预制的初始模型。Fishtown Analytics 也创建了一些很棒的启动包，比如 dbt-utils、audit-helper 和 code-gen。

当开始一个新客户的项目时，我们通常会经历相同的步骤来建立一个新的 dbt 项目。在将来自数据源的数据集成到数据仓库之后，我们使用 code-gen 包来生成包含所有表的源模式。我们还使用相同的包为我们的表生成基础模型，在这里我们可以重命名列并进行一些简单的转换。

对于较小的客户机或表较少的数据源，这完全没问题。然而，当您在一个源中有 10 个以上的表时，运行相同数量的命令来生成基本模型可能会令人望而生畏。此外，同一个源的列通常有相同的命名约定。我们的时间可以更好地用在其他地方，而不是把`write_at`列改成`modified_date`列 50 次。

# dbt 发生器

介绍`[dbt-generator](https://pypi.org/project/dbt-generator/)`🎉🎉。我写了一个简单的 Python 包来帮助解决这个问题。

这个包有助于生成基本模型并批量转换它们。对于拥有 10 个以上模型的源，这个包将通过批量生成基础模型并将它们转换为公共字段来为您节省大量时间。使用这个包是开始您的建模或加入新资源的好方法。

# 入门指南

要使用这个包，您需要安装 dbt 并配置一个概要文件。您还需要从 dbt Hub 安装 code-gen 包。将以下内容添加到 dbt repo 中的 packages.yml 文件，并运行`dbt deps`来安装依赖项。

```
packages:
  - package: fishtown-analytics/codegen
    version: 0.3.2
```

通过运行以下命令，在与 dbt 安装相同的环境中安装软件包:

```
pip install dbt-generator
```

这个包应该在您的 dbt repo 中执行。这个包的源代码可以在这里找到:

[](https://github.com/tuanchris/dbt-generator) [## GitHub - tuanchris/dbt-generator:为 dbt 生成和处理基本模型

### 这个包有助于生成基本模型并批量转换它们。对于有 10 个以上型号的信号源，此包…

github.com](https://github.com/tuanchris/dbt-generator) 

# 生成基础模型

要生成基础模型，使用`dbt-generator generate`命令。这是一个围绕`codegen`命令的包装器，它将生成基本模型。当您有很多模型，并且想要一次生成它们时，这尤其有用。

```
Usage: dbt-generator generate [OPTIONS] Gennerate base models based on a .yml sourceOptions:
  -s, --source-yml PATH   Source .yml file to be used
  -o, --output-path PATH  Path to write generated models
  --source-index INTEGER  Index of the source to generate base models for
  --help                  Show this message and exit.
```

## 例子

```
dbt-generator generate -s ./models/source.yml -o ./models/staging/source_name/
```

这将读入`source.yml`文件，并在`staging/source_name`文件夹中生成基础模型。如果您在`yml`文件中定义了多个源，使用`--source-index`标志来指定您想要为哪个源生成基础模型。

# 转换基础模型

对于同一个源，表之间通常有一致的命名约定。例如，`created_at`和`modified_at`字段通常在所有表格中被命名为相同的名称。将所有这些字段更改为不同数据源的通用值是一种最佳做法。然而，对 10 多个表中的所有日期列都这样做是一件痛苦的事情。

使用这个包，您可以编写一个将被读入的`transforms.yml`文件(`.yml`文件可以被命名为任何名称)。该文件将包含您想要应用到所有基础模型的转换。您可以重命名基本模型中的字段，或者对转换后的字段应用定制的 SQL select。

```
Usage: dbt-generator transform [OPTIONS] Transform base models in a directory using a transforms.yml fileOptions:
  -m, --model-path PATH       The path to models
  -t, --transforms-path PATH  Path to a .yml file containing transformations
  -o, --output-path PATH      Path to write transformed models to
  --drop-metadata BOOLEAN     The drop metadata flag
  --case-sensitive BOOLEAN    The case sensitive flag
  --help                      Show this message and exit.
```

## 例子

```
ID:
  name: ID
  sql: CAST(ID as INT64)
CREATED_TIME:
  name: CREATED_AT
UPDATED_TIME:
  name: MODIFIED_AT
DATE_START:
  name: START_AT
DATE_STOP:
  name: STOP_AT
```

这个`.yml`文件在应用于`staging/source_name`文件夹中的所有模型时，会将所有`ID`字段转换为 INT64，并将所有日期列重命名为`name`键中的值。例如，`CREATED_TIME`将被重命名为`CREATED_AT`,`DATE_START`将被重命名为`START_AT`。如果没有提供`sql`,包将只重命名字段。如果提供了`sql`，包将执行 SQL 并使用`name`键重命名字段。

```
dbt-generator transform -m ./models/staging/source_name/ -t ./transforms.yml
```

这将使用`transforms.yml`文件转换`staging/source_name`文件夹中的所有模型。您还可以通过将`drop-metadata`标志设置为`true`来删除元数据(删除列从`_`开始)。`--case-sensitive`标志将决定转换是否使用区分大小写的名称。

# 限制

以下是当前版本的一些限制。如果您想投稿，请打开一个问题或拉动请求。

*   转换只适用于由 code-gen 包生成的模型。
*   您不能转换已经转换的模型
*   您不能在转换的字段选择中使用通配符(如`*_id`)
*   还没有测试
*   还没有错误处理