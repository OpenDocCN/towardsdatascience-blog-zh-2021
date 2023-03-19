# 如何处理增强树的曝光偏移

> 原文：<https://towardsdatascience.com/how-to-handle-the-exposure-offset-with-boosted-trees-fd09cc946837?source=collection_archive---------16----------------------->

## 保险业从 GLM/GAM 模式转向 GBM 模式的三种方式。

![](img/2ed708608c1372b4d507197103dcf127.png)

[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/insurance?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

在保险环境中，索赔频率建模的一个常见任务是处理不同的风险等级。暴露是一个宽泛的术语，[保险媒体](https://www.insuranceopedia.com/definition/72/exposure)将其定义为“*对损失或风险*的敏感性”，但在我们的例子中，它将与时间类似。

我们预计，如果一个实体遭受某项风险达一年之久，其索赔频率会高于仅遭受该风险一周的情况。如果我们有历史索赔信息，但不是我们观察到的所有政策都持续了一年，那么在建模时必须考虑到这一点。传统上，GLM 或 GAM 模型用于预测索赔频率，这些包中的大多数都有一个所谓的偏移参数，让您自动添加暴露的影响。

> 本质上，我们要求模型根据可用数据进行预测，但我们对暴露部分进行微观管理。

如果您碰巧切换到梯度增强树模型，您将很快意识到在这些包中没有显式的偏移参数。在各种论坛上可以找到很多关于这个话题的讨论，有不同的解决方案，但我还没有真正找到一个值得信赖的综合来源。我决定我将得到这个结束，并彻底调查哪种方法工作，为什么。

# 摘要

我们将模拟一个数据集，并根据泊松分布(保险业中一个非常常见的假设)定义索赔计数。测试是在 R 中用 LightGBM 包完成的，但是应该很容易将结果转换成 Python 或其他包，如 XGBoost。

然后，我们将研究 3 种方法来处理不同程度的暴露。我们将看看预测有多准确，然后深入研究提升树算法，找出它们是如何工作的。

正如我提到的，最初的动机是一个保险问题，但我们所谈论的将完全适用于任何其他你有时间因素的原则。这不是 LightGBM 的一般教程。然而，我在文章的最后列出了一些通用的技巧。

完整代码在一个 R 脚本中，**可以从** [**my GitHub**](https://github.com/MatePocs/quick_projects/blob/main/lightgbm_exposure.R) 中获取。

> **重要提示**:就像你在网上读到的任何东西一样，仔细阅读这些信息。这些软件包很可能会改变现状。总是自己尝试和测试事情。

# 我们的测试环境

## r 包

我们需要以下两个 R 包:

*   [数据表](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html)；
*   [lightgbm](https://lightgbm.readthedocs.io/en/latest/index.html) 。

我们用`data.table`代替默认的`data.frame`，这只是我个人的喜好。如果你看到类似`table[,.(column_name)]`的东西，那就是`data.table`语法。

## 模拟数据

他们有两个分类变量，`var1`和`var2`。在曝光为 1 的观察中，不同的组合在泊松分布中将具有以下λ:

```
 var1 var2 lambda_base
1:    A    C         0.3
2:    B    C         0.7
3:    A    D         1.3
4:    B    D         1.9
```

然后，我们将产生不同的曝光水平，并调整 lambdas。

*例如，var1=A，var2=C，exposure = 0.4 的观测，索赔计数将从λ为 0.3 * 0.4 = 0.12 的泊松分布中随机生成。*

我们创建了数据表的两个变体，一个简单的，一个适中的。这两个版本的区别在于我们产生风险的方式。对于`data_easy`，它们被明确定义为 0.1、0.2、…1.0 的集合，而对于`data_mod`，曝光在 0 和 1 之间随机生成。

我们还生成调整后的索赔计数、定义的 claim_counts / expos(我们稍后会用到)。这是数据表的外观，列名缩写以适应屏幕:

```
 var1 var2  expos lambda_base  lambda ccn  ccn_adj
    1:    A    C 0.3078         0.3 0.09234   0 0.000000
    2:    A    C 0.2577         0.3 0.07731   0 0.000000
    3:    A    C 0.5523         0.3 0.16569   0 0.000000
    4:    A    C 0.0564         0.3 0.01692   0 0.000000
    5:    A    C 0.4685         0.3 0.14055   0 0.000000
   ---                                                  
 9996:    B    D 0.5999         1.9 1.13981   1 1.666944
 9997:    B    D 0.9259         1.9 1.75921   0 0.000000
 9998:    B    D 0.6517         1.9 1.23823   2 3.068897
 9999:    B    D 0.6904         1.9 1.31176   4 5.793743
10000:    B    D 0.6317         1.9 1.20023   3 4.749090
```

## 目标

在进入建模部分之前，重要的是考虑我们期望模型预测什么。

有些人可能认为我们期望预测`claim_counts`。这是不正确的。`claim_counts`是一个随机变量，期望模型能够预测是“不公平的”。

> 我们关心的是模型对λ参数的预测有多好。

# 解决方案 1 —初始分数

在第一个解决方案中，一个经常在论坛上建议的方案，我们将`lgb.Dataset`的`init_score`参数设置为曝光的对数。

## 密码

```
solution_1_predict <- function(data_curr){ data_curr_recoded <- lgb.convert_with_rules(
    data = data_curr[,.(var1, var2)])$data

  dtrain <- lgb.Dataset(
    data = as.matrix(data_curr_recoded),
    label = as.matrix(data_curr[,.(claim_count)]),
    init_score = as.matrix(data_curr[,.(log(expos))]),
    categorical_feature = c(1,2))

  param <- list(
    objective = "poisson",
    num_iterations = 100, 
    learning_rate = 0.5)

  lgb_model <- lgb.train(
    params = param, 
    data = dtrain, 
    verbose = -1)

  return(predict(lgb_model, as.matrix(data_curr_recoded)))
}
```

请注意，LightGBM 预测没有受到`init_score`的影响！所以在我们的例子中，当我们做一个预测时，我们必须手动乘以暴露量。(我认为它在 XGBoost 中的工作方式不同，除非另有说明，这里包含了`base_margin`。)

进行预测的正确方法是:

```
temp_predict <- solution_1_predict(data_easy)
data_easy[,sol_1_predict_raw := temp_predict]
data_easy[,sol_1_predict := sol_1_predict_raw * expos]
```

当然和`data_mod`的过程相同。

## 结果

原始预测非常接近基本的 lambdas，初看起来模型似乎是成功的。对于`data_mod`，结果:

```
data_mod[,.N, keyby = .(lambda_base, sol_1_predict_raw)]
```

返回

```
 lambda_base sol_1_predict_raw    N
1:         0.3         0.2941616 2500
2:         0.7         0.6677160 2500
3:         1.3         1.2907763 2500
4:         1.9         1.9207416 2500
```

索赔总数也与预测相符。

```
 claim_count pred_claim_count theoretical_claim_count
1:        5208             5208                5241.001
```

观察它和理论上的期望值，也就是 lambdas 的和，是不一样的。

## 预测的可能性

让我们分析一下，如果预测值与理论值不完全匹配，我们是否会感到满意。我们将解决方案 1)的性能与简单预测λ的模型的性能进行比较。

首先，我们计算对数似然性:

```
data_mod[,sol_1_ll := dpois(x = claim_count, 
                            lambda = sol_1_predict, log = TRUE)]
data_mod[,base_ll := dpois(x = claim_count, 
                           lambda = lambda, log = TRUE)]
data_mod[,saturated_ll := dpois(x = claim_count, 
                            lambda = claim_count, log = TRUE)]
data_mod[,null_ll := dpois(x = claim_count, 
                            lambda = mean(claim_count), log = TRUE)]
data_mod[,.(sol_1_ll = sum(sol_1_ll), 
            base_ll = sum(base_ll), 
            saturated_ll = sum(saturated_ll), 
            null_ll = sum(null_ll))]
```

退货:

```
 sol_1_ll   base_ll saturated_ll   null_ll
1: -8125.188 -8126.383     -3918.93 -10101.24
```

正如我们所看到的，我们的解决方案的对数似然比我们简单地使用 lambdas 进行预测的结果要高一点点。这意味着我们可以对我们的结果感到满意！

顺便说一下，从对数似然性，我们也可以计算出解释偏差的百分比。我不想跑题文章，代码请参考我的 [GitHub](https://github.com/MatePocs/quick_projects/blob/main/lightgbm_exposure.R) 。

> 然而，我认为重要的是要强调，对于一个几乎完美描述了所有非随机效应的模型，解释的偏差百分比仅为 32%。

## 它是如何工作的？

我最初对这种方法非常怀疑，我的想法是这样的:*“设置 init_score 可以加快这个过程，但最终，模型会远离这些数字。获取日志更没有意义，对于非负目标，它将是一个负数。”*这是不正确的。

对数是必要的，因为`“poisson”`物镜将使用对数链接，即使文档中没有提到这一点。这意味着计算出的原始分数的指数将是模型返回的最终预测值。

*注:其实挺有意思的，无论是* [*XGBoost 的*](https://xgboost.readthedocs.io/en/latest/parameter.html) *还是* [*LightGBM 的*](https://lightgbm.readthedocs.io/en/latest/Parameters.html) *文档都会特别提到 Tweedie 和 Gamma 使用了 log-link，但是没有指定用 Poisson。我知道 log-link 是针对阿松的* [*规范链接函数*](https://en.wikipedia.org/wiki/Poisson_regression) *，但看起来确实有人抄袭了他们的答案！*

当我们放入`init_score`时，我们实际上设置了观测值之间的固定差异。如果曝光是两次观察之间的唯一差异，预测的比率将与曝光相同。

例如，在我们的例子中，如果我们有以下两个观察结果:

```
 var1 var2  expos
    1:    A    C    0.5
    2:    A    C    0.3
```

第一个将得到一个修正对数(0.5)，第二个在其预测总和中得到一个修正对数(0.3)。一旦设置完毕，模型就只能使用`var1`和`var2`。由于这两个观察值相等，模型无法以任何其他方式区分它们，第一个预测值将始终等于第二个预测值的 0.5/0.3 倍。

这就是我们想要的。拟合模型，同时考虑这种假设的线性暴露影响。

# 解决方案 2——经调整的索赔

这是我经常看到的另一个解决方案。

*   首先，计算调整后的索赔次数=索赔次数/风险。*例如，如果您只有 0.5 的风险敞口和 3 项索赔，则调整后的索赔数为 3 / 0.5 = 6。*
*   您根据调整后的索赔数量运行模型。
*   你还需要用曝光率作为权重。(现在没有关于曝光的日志材料！)

让我们看看这个解决方案的表现如何！

## 密码

代码与解决方案 1)几乎相同，但是

*   我们的目标不同；
*   我们没有使用`log(expos)`作为`init_score`，而是将`expos`作为`weight`放入。

```
solution_2_predict <- function(data_curr){

  ... (same as solution 1) ...

  dtrain <- lgb.Dataset(
    data = as.matrix(data_curr_recoded),
    label = as.matrix(data_curr[,.(claim_count_adjusted)]),
    weight = as.matrix(data_curr[,.(expos)]),
    categorical_feature = c(1,2))

  ... (same as solution 1) ...
}
```

## 结果

不再赘述，结果将与解决方案 1)完全匹配。

> 这使得关于应该使用哪种解决方案来处理偏移的争论变得毫无意义。

## 它是如何工作的？

那么，为什么我们从两个看似不同的模型中得到了相同的预测呢？

我们必须考虑提升树(以及一堆其他使用[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)的机器学习模型)如何处理最大似然估计。在引擎盖下，他们不会使用原始形式的似然函数。相反，他们将使用对数似然函数相对于预测值的一阶和二阶导数。这些被称为*梯度*和*黑森*。

在这个方向上走得更远真的会让我们偏离主题，XGBoost 教程有一个很好的总结。(这里的 *g(i)* 和 *h(i)* 函数在文本中没有明确命名，但是它们是 gradient 和 hessian。)底线是，如果你知道目标的梯度和 hessian(比如“泊松”)，你就知道目标。

让我们看看 [LightGBM 源代码](https://github.com/microsoft/LightGBM/blob/4b1b412452218c5be5ac0f238454ec9309036798/src/objective/regression_objective.hpp)中的相关位。在`RegressionPoissonLoss`类下，我们有这个位:

```
gradients[i] = static_cast<score_t>((std::exp(score[i]) - label_[i])    * weights_[i]);        
hessians[i] = static_cast<score_t>(std::exp(score[i] + max_delta_step_) * weights_[i]);
```

注意事项:

*   在这个术语中，分数代表预测。
*   你可以通过计算导数来重现公式的基础。
*   权重参数没有任何更深的数学意义，这是它们将如何被用来衡量*梯度*和*黑森*。当然，如果不定义权重，就不使用`* weights_[i]`位。
*   `max_delta_step_`在黑森只是一个确保收敛的值。如果我理解正确的话，XGBoost 和 LightGBM 的默认值都是 0.7。

现在，让我们看看给定的`claim_count` — `prediction`对会发生什么。

在解决方案 1)中，转折是`log(exposure)`被自动添加到预测中，因此梯度将是:

```
exp(prediction + log(exposure)) - claim_count
```

这等于:

```
exp(prediction) * exposure - claim_count
```

而对于解决方案 2)，我们使用权重，因此梯度将简单地乘以曝光度，另外请注意，我们使用调整后的索赔计数作为标签。溶液 2 的梯度):

```
(exp(prediction) - claim_count_adjusted) * exposure
```

考虑到

```
claim_count_adjusted = claim_count / exposure
```

解 2)的梯度也将等于

```
exp(prediction) * exposure - claim_count
```

同样的思考过程可以重复用于 *hessian* 计算。

对于*(预测、曝光、索赔 _ 计数)*的任何可能值，两个解决方案具有相同的*梯度*和 *hessian* ，它们的所有学习参数都匹配，因此它们将返回相同的预测。

> 我会留下一点时间来处理这个，我认为这是一个很酷的结果。

# 解决方案 3 —自定义目标函数

既然我们对“泊松”目标有了如此透彻的理解，我想我们还不如编写我们自己的自定义目标函数。

我不打算在这里包括一个通用教程，我认为 [R 演示](https://github.com/microsoft/LightGBM/blob/master/R-package/demo/multiclass_custom_objective.R)脚本是相当有帮助的。要点是定制目标函数需要返回两个向量:`gradient`和`hessian`。

## 密码

对于自定义目标函数，我们基本上使用了我们到目前为止讨论过的所有内容。无论我们预测什么，都会随着曝光率自动调整，下文称之为`dtrain_expos`。黑森里的 0.7，简直就是我们前面说的`max_delta_step_`。

```
my_poisson_w_exposure <- function(preds, dtrain){

  labels <- getinfo(dtrain, "label")
  preds <- matrix(preds, nrow = length(labels))
  preds_expos_adj <- preds + log(dtrain_expos)
  grad <- exp(preds_expos_adj) - labels
  hess <- exp(preds_expos_adj + 0.7)

  return(list(grad = grad, hess = hess))
}
```

然后，我们可以使用这个新的目标函数，而不是内置的“泊松”函数。

```
solution_3_predict <- function(data_curr){

  data_curr_recoded <- lgb.convert_with_rules(
    data = data_curr[,.(var1, var2)])$data

  dtrain <- lgb.Dataset(
    data = as.matrix(data_curr_recoded),
    label = as.matrix(data_curr[,.(claim_count)]),
    categorical_feature = c(1,2))

  param <- list(
    max_depth = 2,
    objective = my_poisson_w_exposure,
    metric = "mae",
    num_iterations = 100, 
    learning_rate = 0.5)

  lgb_model <- lgb.train(
    params = param, 
    data = dtrain, 
    verbose = -1)

  return(predict(lgb_model, as.matrix(data_curr_recoded)))
}
```

瞧，基本上就是这样。请注意，我随机输入了“mae”作为度量。每个内置目标都有一个默认的度量对。当我们使用自定义目标时，需要对其进行定义。如果我们想要谨慎，我们还需要定义一个定制的度量函数。但是我们现在没有使用测试集，所以它不会在任何地方使用。

这种方法有两个缺点:

*   我无法找到在自定义目标函数中包含另一个参数的方法。因此，`data_expos`只是被假设为在全球环境中可用，这对我来说并不意味着稳定的结构。也许有一种方法可以输入除`preds`和`dtrain`之外的参数，**一定要让我知道**。
*   除了将从模型中得出的原始预测与暴露量相乘之外，我们还需要计算它们的指数。在内置的“泊松”目标中，有一个额外的步骤来计算原始分数的指数。换句话说，它使用了一个日志链接。这是我在自定义目标中无法做到的。

考虑到这两个问题，使用这种方法进行预测的正确方法是:

```
dtrain_expos <- as.matrix(data_mod[,.(expos)])
temp_predict <- solution_3_predict(data_mod)
data_mod[,sol_3_predict_raw := temp_predict]
data_mod[,sol_3_predict := exp(sol_3_predict_raw) * expos]
```

## 结果

是的，与前两个解决方案相同。

# 哪个解决方案是最好的？

这是一个个人偏好的问题，从计算的角度来看，它们是相同的模型，并将计算相同的预测。

*   我认为带有`init_score`的解决方案 1)是最优雅和简单的。我怀疑那也是最快的。
*   解决方案 3)，自定义目标函数是最健壮的，一旦你理解了它的工作原理，你就可以用它做任何事情。
*   我个人最不喜欢的是解决方案 2)，使用调整索赔计数的解决方案，我认为它有点绕弯，没有任何附加值。

# 一般照明 GBM 提示

我不想在上面的每个细节上花费时间，这里有一个通用提示列表，在使用 LightGBM 或某些情况下其他梯度增强树包时可能会派上用场。

## 分类特征

在 LightGBM 中，使用分类变量是一个两步过程:

*   首先，你要用`convert_with_rules`把它们转换成整数——这一步是必须的；
*   然后，当您创建`lgb.DataSet`时，您可能还想用`categorical_feature`指定数据中的哪些列是分类的——从技术上讲，模型将在没有此步骤的情况下运行，并将分类特征作为序数特征处理。

详情请看这个[演示脚本](https://github.com/microsoft/LightGBM/blob/master/R-package/demo/categorical_features_rules.R)。

## 证明文件

在我看来，软件包文档不是无缝的，你必须测试所有的东西。许多不同的版本(命令行接口，C / Python / R API)通常只是一起记录，这并没有什么帮助。

例如，我不太清楚如何定义`init_score`。根据相关的[演示脚本](https://github.com/microsoft/LightGBM/blob/master/R-package/demo/boost_from_prediction.R)，您必须使用以下语法:

```
setinfo(dtrain, "init_score", ptrain)
```

然而，这样做的时候用下面的定义`lgb.DataSet`:

```
...
init_score = ptrain,
...
```

似乎对我来说挺好的。`set_init_score`另一方面，看起来像是 Python 特有的方法。

## 链接功能

您应该知道您所选择的目标所使用的链接功能。例如，它使用泊松的日志链接。如果你想放入一个`init_score`，这很重要，你必须根据链接函数转换值。

当您运行`predict`方法时，您可以传递一个`rawscore = TRUE`参数来在转换之前获取结果并检查它们。

## 泊松对特威迪

如果您有非整数值，使用基本的“泊松”目标可能看起来很奇怪。毕竟泊松是离散分布。我们如何用泊松来拟合调整后的索赔数？

首先，你可以[将阶乘函数](https://en.wikipedia.org/wiki/Factorial)扩展到非整数，所以从技术上来说，你可以在非整数处计算分布函数的值。gradient 和 hessian 也不会关心标签是否为非整数。

但是你可能仍然会倾向于使用分散参数设置为 1 的 [Tweedie 分布](https://en.wikipedia.org/wiki/Tweedie_distribution)。这样做将导致完全相同的结果。

换句话说，在模型设置中替换这一行:

```
...
objective = "poisson",
...
```

有了这个:

```
...
objective = "tweedie",
tweedie_variance_power = 1,
...
```

不会改变你的结果。

如果你看一下[源代码](https://github.com/microsoft/LightGBM/blob/4b1b412452218c5be5ac0f238454ec9309036798/src/objective/regression_objective.hpp)中的泊松和特威迪损失函数，你可以看到当`rho`(色散参数，又名`tweedie_variance_power`)等于 1 时，它们是如何匹配的。

## 不同的术语

在 LightGBM 和 XGBoost 中，Metric、objective 和 eval 具有不同的含义。在 LightGBM 中，您需要一个目标函数来进行优化，只有在使用[验证集](https://github.com/microsoft/LightGBM/blob/master/R-package/demo/early_stopping.R)时，才会显示指标。

# 好消息来源

  <https://xgboost.readthedocs.io/en/latest/tutorials/model.html>  <https://github.com/microsoft/LightGBM/tree/master/R-package/demo> 