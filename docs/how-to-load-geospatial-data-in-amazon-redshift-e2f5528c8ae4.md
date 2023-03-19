# 如何在亚马逊红移中加载地理空间数据

> 原文：<https://towardsdatascience.com/how-to-load-geospatial-data-in-amazon-redshift-e2f5528c8ae4?source=collection_archive---------25----------------------->

## 使用开源地理空间库和 Amazon 红移数据 API 的 pythonic 方法

![](img/d85986a9de7b73747fd0e21720ac32f5.png)

Pawel Czerwinski 在 [Unsplash](https://unsplash.com/images/stock/creative-common?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

亚马逊红移[于 2019 年 11 月宣布](https://aws.amazon.com/about-aws/whats-new/2019/11/amazon-redshift-announces-support-spatial-data)支持空间数据。这种支持包括新的几何数据类型和 40 多种空间函数。它在 2020 年 9 月得到了增强，增加了新的空间函数，并在使用 JDBC/ODBC 驱动程序时支持几何类型。

如果您有地理空间数据，那么在 Amazon Redshift 中加载数据有两个选项，都涉及到 COPY 命令:

*   [CSV](https://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-spatial-data.html) 文件，带有 [EWKB](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry#Format_variations) 格式的几何图形
*   [形状文件](https://docs.aws.amazon.com/redshift/latest/dg/spatial-copy-shapefile.html)

如果您已经有了这种格式的数据，使用 shapefiles 是一个很好的选择，但它是专有的，需要多个文件，属性名称限制为 10 个字符，大小限制为 2 GB。

如今，使用更新、更高级的格式(如 GeoJSON 或 GeoPackage)来查找地理空间数据是很常见的。如果您想要加载以这些格式存储的数据，您需要使用带有 EWKB 几何图形的 CSV 文件选项。

在本文中，我将向您展示如何使用利用开源地理空间包和 AWS SDK 的 Python 脚本，以更常见的格式将地理空间数据加载到 Redshift。

该脚本有三个主要步骤:

1.  从输入文件创建带有 EKWB 几何图形的 CSV 文件
2.  将创建的文件上传到 S3 存储桶
3.  从 S3 文件导入红移表中的数据

# 创建包含 EWKB 几何图形的 CSV 文件

第一步是读取输入地理空间文件，然后用 EWKB 格式的几何创建一个新的 CSV 文件。

我们将使用最初由 Sean Gillies 开发的两个众所周知的地理空间包:

*   菲奥娜。基于 [GDAL/OGR](https://www.osgeo.org/projects/gdal/) ，提供了用于读写地理空间文件的 Python API。我们将使用 Fiona 来读取输入文件。
*   [身材匀称的](https://github.com/Toblerity/Shapely)。基于[几何](https://trac.osgeo.org/geos/) / [JTS](https://locationtech.github.io/jts/) ，提供操作和分析平面几何对象的功能。我们将使用 Shapely 创建 EWKB 几何图形。

首先我们用 Fiona 打开输入文件，然后使用 Python [csv 模块](https://docs.python.org/3/library/csv.html)创建一个新文件。我们添加带有一个 *geom* 列的标题和来自输入文件的属性名( *f["properties"])。keys()* )作为第一行。然后，我们使用 Shapely[*WKB . dumps*](https://shapely.readthedocs.io/en/stable/manual.html#shapely.wkb.dumps)函数在输入文件中写入特征行，该函数允许我们使用*十六进制*参数以 EWKB 格式写入几何图形。

如果输入文件使用 EPSG 代码定义空间参考，本文末尾链接的完整源代码支持将 EPSG 代码(空间参考 ID-SRID)添加到输出 EWKB 几何。

# 把文件上传到 S3

Redshift 中的 *COPY* 命令要求我们将想要加载的文件存储在 S3 桶中。我们将使用 Python 的[AWS SDK](https://aws.amazon.com/sdk-for-python/)(boto 3)来上传我们在上一步中创建的 CSV 文件。

将文件上传到 S3 的代码非常简单，正如您在下面看到的，但是我们需要有适当的权限来写入 S3 存储桶。

您需要登录到 [AWS IAM 控制台](https://console.aws.amazon.com/iam/home)，创建您的凭证，并确保您在*权限*部分附加了“ *AmazonS3FullAccess* ”策略。

Boto3 将在环境变量(AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY)或您的主文件夹( *~/中的凭证文件中查找访问密钥。用于 macOS/Linux 的 aws/credentials* 或 *C:\Users\USER_NAME\。Windows 版的 aws\credentials* )。您可以自己创建这个文件，也可以安装 [AWS CLI](https://aws.amazon.com/cli/) 并运行 *aws configure* 命令。

配置好凭据后，您应该能够运行以下代码将包含 EWKB 几何图形的 CSV 文件上传到 S3。重要的是，你上传文件到 S3 桶在同一个 AWS 地区，你有你的红移集群。

# 将数据导入红移

现在我们在 S3 有了 CSV 文件，我们将使用 *COPY* 命令将数据加载到 Redshift 中。在 Redshift 中，我们有不同的选项来执行 SQL 命令；其中一些如下:

*   我们可以使用 PostgreSQL 的 [psycopg](https://www.psycopg.org/) 驱动程序。虽然 Redshift 不完全兼容 PostgreSQL，但是我们可以用这个驱动连接 Redshift，执行 SQL 语句。这个选项只允许我们使用传统的数据库认证。
*   另一种选择是使用红移 ODBC 驱动程序和 [pyodbc](https://pypi.org/project/pyodbc/) 模块。这是一个类似于 psycopg 的解决方案，具有红移特有的特性，也只提供传统的数据库认证。
*   更好的选择是使用 [Python 红移连接器](https://github.com/aws/amazon-redshift-python-driver)。它是特定于红移的，由 AWS 开发，并支持红移特定的功能，如 IAM 身份验证。
*   我们最终选择的选项是[红移数据 API](https://docs.aws.amazon.com/redshift/latest/mgmt/data-api.html) 。它使用 boto3 包进行异步 HTTP 调用。代码稍微复杂一点(您需要[执行语句](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/redshift-data.html#RedshiftDataAPIService.Client.execute_statement)，等待它结束，然后检索结果)，但是它提供了 IAM 和 Secrets Manager 认证，并且避免了管理连接的需要。

要使用红移数据 API，我们需要我们的凭证来附加[AmazonRedshiftDataFullAccess](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html)策略。如上所述，使用 AWS IAM 控制台附加策略。

在加载数据之前，我们需要创建一个表，数据将被导入到这个表中。我们需要准备带有输入文件中的列名和数据类型的 SQL 语句。Fiona 提供了一个*模式*属性，我们可以使用它将从 Fiona 读取的 Python 数据类型映射到[红移数据类型](https://docs.aws.amazon.com/redshift/latest/dg/c_Supported_data_types.html)。请看一下*get _ create _ table _ statement*和 *get_field_mappings* 函数来理解这个过程。

为了在 Redshift 上执行 SQL 语句，我们需要提供数据库凭证来访问数据库。该脚本使用存储在 [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) 中的凭证。这是连接到 Redshift 的推荐选项之一，它为循环、管理和检索数据库凭证提供了非常有用的功能。我们还需要提供这个秘密的 ARN。

一旦我们创建了表，我们就可以使用 *COPY* 命令导入数据。除了数据库凭证之外，我们还需要为我们的红移集群附加一个角色，以允许对 S3 进行读访问。一旦我们附加了这个角色，我们将在 COPY 命令中使用该角色的 Amazon 资源名称(ARN)。

您可以在下面看到 *execute_statement* 函数的语法。除了集群标识符和数据库之外，我们还用数据库凭证和 SQL 语句指定了秘密的 ARN。在 SQL 语句中，我们需要指明 S3 中 CSV 文件的路径以及附加到 Redshift 集群的 IAM 角色，该角色对 S3 具有读取权限。

请查看完整的源代码，看看我们如何检查 SQL 语句执行是否成功，以及我们需要使用的其他选项，如 *COPY* 命令中的 *TIMEFORMAT* 参数，以正确解释日期/时间字段。

# 现在呢？

所以你有我们的红移地理空间数据，现在你想工作的空间信息。您可以从红移[空间函数](https://docs.aws.amazon.com/redshift/latest/dg/geospatial-functions.html)开始，这些空间函数提供空间分析的基本功能，如要素相交或计算给定区域内的要素。

但是您可能希望使用这些数据集创建地图来理解信息并执行高级空间分析，如计算零售选址解决方案的双区域或计算包裹递送的最佳路径。

这就是像 [CARTO](https://carto.com) 这样的云原生地理空间平台有用的地方(免责声明:我目前是 CARTO 的产品经理)。CARTO 允许您导入红移表，使用[生成器](https://carto.com/builder/)创建令人惊叹的可视化效果，并通过使用来自[数据观测站](https://carto.com/data-observatory/)的公共和优质数据集丰富您的信息来执行高级分析。您还可以使用 React 的 [CARTO 和 awesome](https://docs.carto.com/react) [deck.gl](https://deck.gl) 可视化库的 CARTO [模块](https://docs.carto.com/deck-gl)创建自己的自定义空间应用程序。

除了这些功能之外，CARTO 即将推出的针对 Redshift 的空间分析扩展将允许您执行高级空间分析，而无需将数据从数据库中删除，这一新的云原生功能将允许您直接从 Redshift 集群创建 2D 和 3D 地图。

# 源代码

您可以在这个 GitHub [资源库](https://github.com/borja-munoz/cloud-native-geo-importers)中找到 Python 脚本的源代码。脚本( [*geo2rs.py*](https://github.com/borja-munoz/cloud-native-geo-importers/blob/master/geo2rs.py) )可以独立运行，也可以作为模块使用。如果独立运行，它使用 [argparse](https://docs.python.org/3/library/argparse.html) 库来解析命令参数。你可以在 [README.md](https://github.com/borja-munoz/cloud-native-geo-importers/blob/master/README.md) 文件中找到关于命令行参数的更多信息。您还可以使用 [AWS Lambda](https://aws.amazon.com/lambda/) 修改代码，使其无服务器运行。