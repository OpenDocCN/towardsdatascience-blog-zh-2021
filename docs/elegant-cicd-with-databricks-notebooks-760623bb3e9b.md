# 优雅的 CICD，配有 Databricks 笔记本

> 原文：<https://towardsdatascience.com/elegant-cicd-with-databricks-notebooks-760623bb3e9b?source=collection_archive---------14----------------------->

## 如何用 Azure DevOps 发布 Databricks 笔记本神器

卢克·范·德·维尔登

从数据科学探索到生产中的 ETL 和 ML，笔记本是数据块的主要运行时。这种对笔记本的强调要求我们改变对生产质量标准的理解。我们必须消除对凌乱的笔记本的犹豫，并问自己:我们如何将笔记本转移到我们的生产流水线中？我们如何在笔记本上执行单元测试和集成测试？我们可以将笔记本视为 DevOps 管道中的工件吗？

# 作为一等公民的数据砖笔记本

选择 Databricks 作为计算平台时，您的最佳选择是在您的生产环境中运行笔记本电脑。这个决定是由对笔记本运行时和经典 python 脚本的压倒性支持决定的。我们认为，人们应该完全接受笔记本电脑方法，并选择最佳方法在生产环境中测试和部署笔记本电脑。在这篇博客中，我们使用 Azure DevOps 管道通过瞬态数据块集群和笔记本工件注册进行笔记本(单元、集成)测试。

# 笔记本:python 包的入口点

笔记本可以独立存在，但是我们更喜欢将它们作为 Git 存储库的一部分，它具有以下结构。它包含一个笔记本目录，用于将数据块笔记本作为源文件签入；一个 Python 包(“my_model”)，包含要在笔记本中导入的功能；一个测试目录，包含 Python 包的单元测试；一个 Azure DevOps 管道和一个 cluster-config.json，用于配置我们的瞬态数据块集群。此外，我们使用诗歌进行基于 pyproject.toml 规范的 Python 依赖管理和打包。

```
notebooks/
-        run_model.py  # Databricks notebook checked in as .py file
my_model/
-        preprocessing.py  # Python module imported in notebook
tests/
azure-pipelines.yml
cluster-config.yml
pyproject.toml
...
```

通过将 Git 存储库链接到 Databricks 工作区中的笔记本，或者通过手动将笔记本导出为*源文件*，可以将笔记本提交到 Git 存储库中。在这两种情况下，存储库中的笔记本都是带有数据块标记命令的 Python 文件。我们存储库的笔记本入口点如下所示。请注意，它从包含的存储库中安装并导入 Python 包‘my _ model’build。包版本将在后面详细说明。任何笔记本逻辑都在 main 函数中被捕获。执行主函数 dbutils.notebook.exit()后，调用该函数，表示成功完成，并允许将结果值返回给调用者。

```
# Databricks notebook source
dbutils.widgets.text("package_version", defaultValue='')
package_version = dbutils.widgets.get("package_version")

# COMMAND ----------

devops_pat = dbutils.secrets.get("devops_scope", "devops-artifact-read")
%pip install my_model==$package_version --index=[https://build:$devops_pat@pkgs.dev.azure.com/organization/project/_packaging/feed/pypi/simple/](https://build:$devops_pat@pkgs.dev.azure.com/organization/project/_packaging/feed/pypi/simple/)

# COMMAND ----------

from my_model.preprocessing import do_nothing

# COMMAND ----------

# define the main model function
def main(spark):
  do_nothing(spark)

# COMMAND ----------

# run the model
from loguru import logger

with logger.catch(reraise=True):
  main(spark)

# COMMAND ----------

dbutils.notebook.exit("OK")
```

# 笔记本拉取请求管道

在开发笔记本及其支持的 Python 包时，开发人员提交开发分支，并创建一个 Pull 请求供同事审阅。图 1 显示了支持我们的拉请求的管道步骤。Pull 请求自动触发 Azure DevOps 管道，该管道必须在最近一次提交时成功。首先，我们运行 Python 包的单元测试，成功后构建它并将开发构建包发布到 Azure 工件。这个开发构建包的版本字符串被传递给笔记本输入小部件“package_version ”,以便在我们的登台环境中进行笔记本集成测试。管道验证笔记本是否成功运行(是否调用了 dbutils.notebook.exit ),并提供关于 Pull 请求的反馈。

![](img/99039ebbdf631e051260d614310260ff.png)

图 1:用于持续集成的 PR 笔记本管道，图片由作者提供

# 瞬态集群上的集成测试

目标是在来自 Azure DevOps 管道的数据块上执行这个笔记本。为了灵活性，我们选择数据块池。这些池的优势在于，当许多不同的作业需要在即时集群上运行时，它们可以减少集群的启动和自动扩展时间。对于笔记本的执行(以及对可选数据源的访问)，我们使用 Azure 应用程序注册。此 Azure 应用程序注册将拥有管理数据块群集和执行笔记本的权限。流水线的基本步骤包括数据块集群配置和创建、笔记本的执行以及集群的最终删除。我们将详细讨论每个步骤(图 2)。

![](img/aa1557c9acee39d5479a16de349aa0e2.png)

*图 Databricks 笔记本的集成测试管道步骤，作者图片*

为了使用 Azure DevOps 管道来测试和部署 Databricks 笔记本，我们使用由 Data heedy Ltd 开发的 [Azure DevOps 任务](https://marketplace.visualstudio.com/publishers/DataThirstLtd)来创建集群，并使用来自[微软 DevLabs](https://marketplace.visualstudio.com/publishers/Microsoft%20DevLabs) 的任务来执行笔记本。由于他们的任务集还不支持所有需要的操作，我们也使用他们为数据块开发的 PowerShell 工具。任务和 PowerShell 工具都是围绕 Databricks API 的包装器。

## 应用程序注册的数据块权限

作为准备，我们创建了一个可用于集成测试的数据块池。我们使用 Azure 应用程序注册作为主体在实例池上执行笔记本。应用程序注册被注册为 Databricks 服务主体，在 Databricks 池上具有“可以附加到”权限以创建群集。

![](img/c8631e8533b3636cb70d89ed6d18c985.png)

*数据块实例池配置，作者图片*

## 准备管道机密

CI/CD 管道的第一步是获取所有需要的机密。为简单起见，我们将应用注册客户端 id、secret、tenant-id 和 Databricks 池 id 存储在一个密钥库中。秘密是使用 AzureKeyVault 任务收集的。

```
# azure-pipelines.yml excerpt
jobs:
- job: integration_test
  displayName: Test on databricks
  pool:
    vmImage: "windows-latest"
  steps:
  - task: AzureKeyVault@1
    inputs:
      azureSubscription: "Azure DevOps Service Connection"
      keyVaultName: "keyvault-test-environment"
      secretsFilter: "appreg-client-id,appreg-client-secret,tenant-id,databricks-pool-id"
```

## 数据块工作空间连接

为了与数据块交互，我们需要从 Azure DevOps 连接到工作区。我们使用 Data heart 中的两个 Azure Devops 任务来生成 Databricks 的访问令牌并连接到工作区。令牌存储在 BearerToken 变量中，并为我们在 Databricks 中授予权限的应用程序注册生成。可以在 Databricks 资源上的 Azure 门户中找到工作区 URL。

```
# azure-pipelines.yml excerpt
- task: databricksDeployCreateBearer@0
  inputs:
    applicationId: $(appreg-client-id)
    spSecret: $(appreg-client-secret)
    resourceGroup: "DatabricksResourceGroup"
    workspace: "DatabricksWorkspace"
    subscriptionId: "AzureSubscriptionId"
    tenantId: $(tenant-id)
    region: "westeurope"
- task: configuredatabricks@0
  inputs:
    url: "[https://adb-000000000000.0.azuredatabricks.net](https://adb-000000000000.0.azuredatabricks.net)"
    token: $(BearerToken)
```

请注意，使用 databricksDeployCreateBearer 任务存在潜在的安全问题，我们已经在实时管道中解决了这个问题。该任务的当前版本创建了没有截止日期的不记名令牌，不幸的是，无法使用该任务设置截止日期。或者，也可以使用 Data hedge 的 Powershell Databricks 工具。通过连续调用 Connect-Databricks 和 New-databrickbearertoken，可以创建具有有限生存期的令牌。

## 创建瞬态测试集群

在建立到数据块的连接之后，我们在数据块中创建一个专用集群，用于由这个管道执行的集成测试。集群配置只包含一个工作人员，这对于集成测试来说已经足够了。当我们在 ADLS 第二代存储帐户上存储笔记本电脑所需的测试数据时，我们设置了 ADLS 传递，以允许应用程序注册通过存储帐户进行验证。为了达到最佳效果，我们**而不是**直接在集群配置中插入应用注册客户端密码，因为这将在数据块中可见。相反，我们在集群配置中使用 Databricks Secret 作用域及其模板标记，这在运行时填充。

```
// cluster-config.json
{
    "num_workers": 1,
    "cluster_name": "",
    "spark_version": "",
    "spark_conf": {
        "fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "",
        "fs.azure.account.oauth2.client.secret": "{{secrets/zpz_scope/appreg-client-secret}}",
        "fs.azure.account.oauth2.client.endpoint": "",
        "spark.hadoop.fs.permissions.umask-mode": "002"
    },
    "ssh_public_keys": [],
    "custom_tags": {},
    "spark_env_vars": {
        "PYSPARK_PYTHON": "/databricks/python3/bin/python3",
        "NSK_ENV": ""
    },
    "autotermination_minutes": 10,
    "cluster_source": "API",
    "init_scripts": [],
    "instance_pool_id": ""
}
```

我们提交集群配置文件的上述模板，并在运行时使用 Linux‘jq’命令从 Azure Key Vault 中填写详细信息，如池 id 和应用注册客户端 id。集群名称基于当前的 devops 构建 ID 和其他参数，cluster-config.json 被呈现并写入磁盘。

```
# azure-pipelines.yml excerpt
- bash: |
    jq -c ".cluster_name = \"${CLUSTER_NAME}\""`
         `"| .spark_conf.\"fs.azure.account.oauth2.client.id\" = \"$(ar20zpz001-client-id)\""`
         `"| .spark_conf.\"fs.azure.account.oauth2.client.endpoint\" = \"[https://login.microsoftonline.com/$(tenant-id)/oauth2/token\](https://login.microsoftonline.com/$(tenant-id)/oauth2/token\)""`
         `"| .spark_version = \"${RUNTIME}\""`
         `"| .instance_pool_id = \"${INSTANCE_POOL_ID}\""`
         `"| .spark_env_vars.NSK_ENV = \"${NSK_ENV}\"" cluster-config.json > tmp.$$.json
    mv tmp.$$.json cluster-config.json
    echo "Generated cluster-config.json:"
    cat cluster-config.json
  displayName: "Generate cluster-config.json"
    env:
      CLUSTER_NAME: "integration-build-$(Build.BuildId)"
      RUNTIME: "7.5.x-scala2.12"
      INSTANCE_POOL_ID: $(main-pool-id)
      NSK_ENV: "test"
```

Data heedy 中的 databricksClusterTask 使用呈现的 cluster-config.json 在我们的暂存环境中创建和部署一个集群，其中包含从 Databricks 池中获取的资源。

```
- task: databricksClusterTask@0
  name: createCluster
  inputs:
    authMethod: "bearer"
    bearerToken: $(appreg-access-token)
    region: "westeurope"
    sourcePath: "cluster-config.json"
```

## 执行笔记本

最后，我们可以上传笔记本来测试和执行它。databricksDeployScripts 任务将笔记本上载到 Databricks，该任务使用 Microsoft DevLabs 的 executenotebook 任务执行。笔记本存储在包含 devops 构建 ID 的路径中，以便在以后需要时识别(并删除)它。如果笔记本使用小部件，executionParams 输入用于传递带有输入参数的 JSON 字符串。在我们的例子中，Python 包开发版本字符串作为“package_version”传递，用于受控集成测试。最后，我们等待笔记本的执行完成。如果在笔记本运行期间调用了 Databricks builtin dbutils . notebook . exit(" return value ")，则 executenotebook 任务将成功完成。

```
# azure-pipelines.yml excerpt
- task: databricksDeployScripts@0
  inputs:
    authMethod: "bearer"
    bearerToken: $(appreg-access-token)
    region: "westeurope"
    localPath: "notebooks/"
    databricksPath: "/test/package_name/$(Build.BuildId)"
    clean: false       
- task: executenotebook@0
  inputs:
    notebookPath: "/test/package_name/$(Build.BuildId)/$(notebook_name)"
    executionParams: '{"package_version":"0.0.0-dev"}'
    existingClusterId: $(createCluster.DatabricksClusterId)
- task: waitexecution@0
  name: waitForNotebook
```

# 删除集群

最后，我们删除集群。不幸的是，不存在来自数据渴求的 Azure DevOps 任务来删除集群，所以我们安装了他们的 Powershell Databricks 工具，并使用 Remove-DatabricksCluster 命令来删除集群。

```
# azure-pipelines.yml excerpt
- task: PowerShell@2
  condition: always()
  inputs:
    targetType: "inline"
    script: |
      Install-Module -Name azure.databricks.cicd.tools -force -Scope CurrentUser
- task: PowerShell@2
  condition: always()
  inputs:
    targetType: "inline"
    script: |
      Remove-DatabricksCluster -BearerToken $(BearerToken) -Region 'westeurope' -ClusterId $(createCluster.DatabricksClusterId)
  displayName: "Delete Databricks integration cluster"
```

# 笔记本神器发布

测试成功的笔记本准备与主分支合并。合并后，我们希望将笔记本电脑投入生产。我们使用 Azure devops 工件将项目笔记本目录注册为 Azure devops 工件提要中的通用包。我们用发布版本标记主分支，这触发了包括工件注册的管道运行。在注册 notebook 工件之前，发布版本在 notebook input 小部件中用 *sed* 设置为默认的“package_version ”,见下文(下面的示例为 1.0.0 版)。注意，附带的 Python 包也注册为具有相同名称和版本的工件，但是在不同的工件 devops 工件提要中。这确保了在默认情况下，笔记本将使用测试时使用的 Python 包版本运行。因此，我们的笔记本产品是可复制的，并允许一个受控的发布过程。

```
# Databricks notebook source
dbutils.widgets.text("package_version", defaultValue='1.0.0')
package_version = dbutils.widgets.get("package_version")
```

如何生成发布版本由您决定。最初，您可以向主分支添加一个 git 标签来触发一个包含工件注册的构建，如下所示。对于完整 CICD，您可以在合并拉取请求时即时生成一个版本。

```
# azure-pipelines.yml excerpt
 variables:
  packageName: 'my_model'
  ${{ if startsWith(variables['Build.SourceBranch'], 'refs/tags/') }}:
    packageVersion: $(Build.SourceBranchName)
  ${{ if not(startsWith(variables['Build.SourceBranch'], 'refs/tags/')) }}:
    packageVersion: 0.0.0-dev.$(Build.BuildId)- job: publish_notebook_artifact
  pool:
    vmImage: "ubuntu-latest"
  dependsOn: [integration_test]
  condition: and(succeeded(), or(eq(variables['Build.Reason'], 'Manual'),  startsWith(variables['Build.SourceBranch'], 'refs/tags/')))
  steps:
  - bash: |
      set -e
      if [ -z "$PACKAGEVERSION" ]
      then
        echo "Require PACKAGEVERSION parameter"
        exit 1
      fi
      sed -i "s/defaultValue=.*/defaultValue='$PACKAGEVERSION')/" \
        notebooks/run_model.py
    displayName: Update default value for version
  - task: UniversalPackages@0
    displayName: Publish notebook artifact $(packageVersion)
    inputs:
      command: publish
      publishDirectory: "notebooks/"
      vstsFeedPublish: "DNAKAA/databricks-notebooks"
      vstsFeedPackagePublish: "my_model"
      packagePublishDescription: "notebooks of my_model"
      versionOption: custom
      versionPublish: "$(packageVersion)"
```

# 结论

我们已经展示了如何通过 Python 包单元测试在瞬态数据块集群上运行笔记本集成测试。这导致可再现的笔记本工件，允许笔记本的受控发布过程。Databricks 笔记本电脑是一等公民，要求工程师将笔记本电脑解放到他们的测试和发布流程中。我们期待着了解更多关于将数据科学家的现实与数据工程师的现实相结合，以提高生产力，定期发布。我们的目标是简化从探索、概念验证到生产的过程。在我们的下一篇博客中，我们将深入探讨如何在生产管道中使用笔记本工件，重点是 Azure DataFactory 管道。

*最初发布于*[*https://codebeez . nl*](https://codebeez.nl/blogs/elegant-cicd-databricks-notebooks/)*。*