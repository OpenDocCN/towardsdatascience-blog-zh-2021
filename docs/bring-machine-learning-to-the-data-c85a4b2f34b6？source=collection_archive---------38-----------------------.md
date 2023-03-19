# 将机器学习引入数据

> 原文：<https://towardsdatascience.com/bring-machine-learning-to-the-data-c85a4b2f34b6?source=collection_archive---------38----------------------->

## Google BigQuery 展示了一个新的范例

![](img/602f4894159252c383313ce7b871e695.png)

图片来自[穆罕默德·哈桑](https://pixabay.com/de/vectors/machine-learning-informationen-5720531/)在 [Pixabay](https://pixabay.com/de/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5720531)

从数据仓库或数据湖中提取数据的传统方法——然后对其进行清理、转换并构建模型——正慢慢被一种更新的方法所取代

> 将计算带到数据或
> 
> 将机器学习/算法引入数据

特别是因为像谷歌、亚马逊和微软这样的大提供商将数据库和数据仓库等服务与机器学习服务相链接，所以不再需要将数据集成到其他平台或系统中。在像 Google 的 BigQuery 这样的服务中，传统的数据库系统甚至用 ML 工具进行了内部扩展[2]。

## 新范式的优势

新方法的优势包括简化的基础架构。如果我们看一下下面的简化架构，就可以清楚地看到，如果服务已经可以在云环境中相互通信，或者集成在一个服务中，那么就不需要第三方系统的进一步接口。**这大大缩短了这些环境的设置和维护时间。**

![](img/9491b0166dfd4252ba785342b65bea24.png)

*经典分析流程架构—作者图片*

另一个重要因素是**数据科学流程可以显著简化**。每个数据科学家和工程师都知道这一过程有多耗时，因此将您需要的一切都放在云环境甚至服务中的方法大大简化了这一过程。

![](img/8c1dbe756db79bb045d3b320e5732c68.png)

集成的数据湖和分析平台

第三点是**可编程性的简化**——比如分析师只用 SQL 就能轻松完成机器学习任务。下面我想用 Google BigQuery 中的一个简单例子来说明这一点。如果您想更深入地了解数据平台现代化这一主题，您可能会对这篇文章感兴趣。

## BigQuery ML 示例

为了展示解决方案如何简化数据分析过程，我将使用 BigQuery 及其 ML 功能。这里，我使用了公共数据集，其中包含了爱荷华州所有批发购买的酒。

在这个例子中，我只是想做一些聚类。这个例子非常简单，但是很好地展示了可能性和您必须采取的步骤。

**步骤 1 —创建模型**

```
CREATE OR REPLACE MODEL DATA.iowa_bottle_clusters OPTIONS (model_type=’kmeans’, num_clusters=3, distance_type = ‘euclidean’) ASSELECT item_description, AVG(state_bottle_cost) AS state_bottle_cost,FROM `bigquery-public-data.iowa_liquor_sales.sales`WHERE EXTRACT(YEAR FROM date) = 2018GROUP BY item_description;
```

上面，您可以看到我使用了 k-Means 算法，并将聚类数的参数设置为 3。我的想法是，如我所说，高度简化的例子只是使用变量*state _ bottle _ payed*(酒精饮料部门为每瓶订购的酒支付的金额)将其分为三个价格类别。

**第二步——评估模型**

创建模型后，BigQuery 会自动为您提供一些指标。这些允许对聚类算法进行评估。对于*戴维斯-波尔丁指数*，目标将是最低的可能值【3】。

![](img/d2061bb9056fd88db4c35e9e9b9776a3.png)

指标—按作者分类的图像

另一个很棒的特性是我们得到的损失图表。

![](img/adab81fa50d30c235e443021de6e097a.png)

随着

```
SELECT * FROM ML.TRAINING_INFO(MODEL Data.iowa_bottle_clusters);
```

和

```
SELECT davies_bouldin_index FROM ML.EVALUATE(MODEL Data.iowa_bottle_clusters);
```

如果需要，您可以稍后查询结果。

**第三步——预测**

Via *ML。预测*我们将看到某个特定品牌属于哪个集群。

```
SELECT
 centroid_id,
 item_description,
 state_bottle_cost
FROM
 ML.PREDICT(MODEL Data.iowa_bottle_clusters,(SELECT
  item_description,
  AVG(state_bottle_cost) AS state_bottle_cost
 FROM
  bigquery-public-data.iowa_liquor_sales.sales
 WHERE
  date <= '2018-02-02'
  AND date >='2018-01-01'
 GROUP BY
  item_description) )
 ORDER BY
  centroid_id;
```

**步骤 4 —检查结果**

现在让我们检查结果是否有意义(当然这是一个非常简单的例子),但是让我们看看。以下是三个集群的示例:

第一组中只有一个项目似乎是高级产品:

*1 —雷米·马丁路易十三干邑— 1599.19*

在第二组中，我们有更多的瓶子，这似乎被认为是中产阶级，例如:

*2 —达尔莫尔雪茄麦芽苏格兰威士忌— 93.33*

*2 —卡瓦兰雪利酒橡木单一麦芽— 73.33*

*2 —吉姆梁酒厂的杰作— 104.91*

还有一组你可能会和可乐或其他混合物一起喝的产品:

*3—斯米尔诺夫葡萄— 8.25*

*3 —斯米尔诺夫酸青苹果— 8.25*

*3 —伯内特水果潘趣酒— 4.48*

所以最终的结果并没有那么糟糕——但当然可以优化。这里您需要的只是 BigQuery 和一些基本的 SQL。

## 结论

在这篇短文中，我想提供一些关于将机器学习或算法转移到数据的范式的理论基础知识。此外，相对于传统方法的优势是显而易见的。尤其是在设置和维护方面，以及在节省时间方面的实际数据分析过程。最后，我展示了现在只使用 SQL 和 Google 的 BigQuery 开发一个机器学习模型是多么容易。要了解更多信息，请点击下面的链接。

## 资料来源和进一步阅读

[1]唐斯，B. N .，奥菲姆，D. M .，黑尔，w .，Xi，l .，多纳霍，L. A .，&卡拉，D. (2014)。将计算引入数据的实际例子。*生物分子技术杂志:JBT* ， *25* (增刊)，S5。

【2】Google，[什么是 BigQuery ML？](https://cloud.google.com/bigquery-ml/docs/introduction?hl=en) (2020)

[3]戴维斯博士，波尔丁博士(1979 年)。“一个集群分离措施”。IEEE 模式分析与机器智能汇刊。PAMI-1 (2)，第 224 至 227 节。