# Dials、Tune 和 Parsnip: Tidymodels 创建和调整模型参数的方式

> 原文：<https://towardsdatascience.com/dials-tune-and-parsnip-tidymodels-way-to-create-and-tune-model-parameters-c97ba31d6173?source=collection_archive---------11----------------------->

## 管理参数值的三种简洁方法及预测企鹅体重的示例代码

![](img/04f356c5c40349b79f9f39e8a798c1d5.png)

伊恩·帕克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

找到最佳值可以显著改善机器学习模型。我喜欢在调整过程中尝试各种值，并看到我的模型在预测方面越来越好。然而，必须弄清楚参数的名称、范围、值和其他方面可能会很麻烦。令人高兴的是，`dials`库是`tidymodels`的一部分，它的创建使得参数调整变得更加容易。

在这篇文章中，我介绍了用`tidymodels`调整参数的三种方法，并提供了示例代码。到目前为止，我看到的大多数`tidymodels`的例子都没有将`dials`包括在建模中。虽然`dials`不是必需的，但它有助于参数调整。

# 概观

我使用`tidymodels`进行演示，其中`rsample`用于拆分数据，`parsnip`用于建模，`workflow`用于绑定流程，`tune`用于调优，`dials`用于参数管理。如果你正在读这篇文章，我假设你对`tidymodels`有所了解。总的来说，有三种使用参数的方法(据我所知):

1.  在`parsnip`中应用默认值
2.  使用`tune`创建一个调整网格并交叉验证`parsnip`模型
3.  用`dials`创建一个调整网格，供`tune`交叉验证`parsnip`模型

我总是在我的帖子中强调这一点，但我将在这里再次强调:这绝不是一个详尽的指南。数据科学和`tidymodels`一样，在不断发展变化。我正在分享我到目前为止学到的东西，并希望帮助人们了解如何使用这些图书馆。如果您有任何问题或者您觉得这篇文章有帮助，请告诉我。

# 输入数据

我使用了 tidymodels 中包含的[企鹅](https://github.com/allisonhorst/palmerpenguins)数据集。删除具有 NA 值的行并排除“孤岛”功能后，它有 344 行和 7 列。数据集相对较小，但混合了名义变量和数值变量。目标变量是`body_mass_g`，它是数值型的，使得这成为一个回归任务。

```
**library**(tidymodels)
data("penguins")
penguins <- penguins %>%
  select(-island) %>%
  drop_na() 
penguins %>% glimpse() 
## Rows: 344
## Columns: 7
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie…
## $ island            <fct> Torgersen, Torgersen, Torgersen…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, 36.7, 39.3, 38.9, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, 19.3, 20.6, 17.8,…
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195,…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625…
## $ sex               <fct> male, female, female, NA, female, male…
```

因为目标是演示参数调整的各种方法，所以我不做探索性的数据分析、预处理、特征工程和其他通常在机器学习项目中执行的工作。在这里，我将数据分为训练和测试，并设置了 5 重交叉验证，重复 2 次。

```
set.seed(300)
split <- initial_split(penguins, prop = 0.75)
penguins_train <- training(split)
penguins_test <- testing(split)
folds_5 <- vfold_cv(penguins_train, v = 5, repeats = 2)
```

# 创建随机森林模型

在筛选模型选项之前，我通常需要考虑项目目标、数据类型和其他因素。在这里，我决定使用随机森林，因为它有几个参数，我喜欢它。

在 tidymodels 中，`parsnip`为模型提供了一个整洁的、**统一的**接口。R 中的一个挑战是函数自变量和参数的不同。例如，`randomForest`和`ranger`都是构建随机森林模型的函数。但是，`randomForest`有`mtry`和`ntree`，`ranger`有`mtry`和`num.trees`。尽管`ntree`和`num.trees`指的是同一个概念，但它们的名称不同。我发现自己在编码时经常使用`?randomForest`或`?ranger`来计算参数名。创建`parsnip`是为了提供问题的解决方案。

## 查找可用的引擎

首先，我从[参考页面](https://parsnip.tidymodels.org/reference/index.html)中找到随机森林模型的名称。令人惊讶的是，不是`rf`而是`rand_forest`。`parsnip`为每个型号提供了几个引擎，我可以调用`show_engines(“rand_forest”)`列出所有可用的引擎。

```
## # A tibble: 6 x 2
##   engine       mode          
##   <chr>        <chr>         
## 1 ranger       classification
## 2 ranger       regression    
## 3 randomForest classification
## 4 randomForest regression    
## 5 spark        classification
## 6 spark        regression
```

接下来，`show_model_info("rand_forest")`显示模式、自变量、拟合模块和预测模型。下面的代码块被裁剪成只显示参数，输出清楚地翻译了跨`rand_forest`和三个引擎的参数名。

```
##  arguments: 
##    ranger:       
##       mtry  --> mtry
##       trees --> num.trees
##       min_n --> min.node.size
##    randomForest: 
##       mtry  --> mtry
##       trees --> ntree
##       min_n --> nodesize
##    spark:        
##       mtry  --> feature_subset_strategy
##       trees --> num_trees
##       min_n --> min_instances_per_node
```

至此，我选择`rand_forest`作为模型，`randomForest`作为引擎，我知道模型有三个参数:`mtry`、`trees`和`min_n`。在接下来的部分中，我将讨论三种不同的使用参数的方法，如概述中所述。

# 1.在 parsnip 中使用默认参数

`rf_spec`是用`parsnip`创建的随机森林模型规范。我没有为任何参数指定值，因此使用了默认值。像往常一样，我然后在训练数据上拟合模型。打印默认参数。

```
# Create random forest specification
rf_spec <- 
  rand_forest(mode = "regression") %>%
  set_engine("randomForest")# Fit training data
model_default <-
  rf_spec %>%
  fit(body_mass_g~., data = penguins_train)

model_default 
## parsnip model object
## 
## Fit time:  133ms 
## 
## Call:
##  randomForest(x = maybe_data_frame(x), y = y) 
##                Type of random forest: regression
##                      Number of trees: 500
## No. of variables tried at each split: 1
## 
##           Mean of squared residuals: 89926.71
##                     % Var explained: 85.51
```

使用默认参数的模型没有经过交叉验证。因为只有一组参数，所以交叉验证不会改进模型，但会使训练数据的性能更接近测试。一旦训练完成，我就对测试数据进行预测，并用`yardstick`计算性能指标。

```
model_default %>% 
  predict(penguins_test) %>% 
  bind_cols(penguins_test) %>% 
  metrics(body_mass_g, .pred) 
## # A tibble: 3 x 3
##   .metric .estimator .estimate
##   <chr>   <chr>          <dbl>
## 1 rmse    standard     317\.   
## 2 rsq     standard       0.877
## 3 mae     standard     242.
```

# 2.使用 tune 来调整 parsnip 模型

之前，`rf_spec`使用默认参数值。为了调整参数，我需要添加参数。下面我提供了两种方法。

```
# Add parameters
rf_spec <-
  rf_spec %>%
  update(mtry = tune(), trees = tune())# Option 2: Start again
rf_spec_new <-
  rand_forest(
    mode = "regression",
    mtry = tune(),
    trees = tune()
  ) %>%
  set_engine("randomForest")
```

`tune()`是等待调整的值的占位符。所以现在，`rf_spec`需要尝试不同的参数，找到最好的。这就是交叉验证的切入点。对于如此小的数据集，交叉验证的模型性能可能更能代表新数据。

听起来很不错，对吧？`tidymodels`也让它变得非常简洁，因为`workflow`捆绑了预处理、建模和后处理请求。也常用于用`recipes`包含数据预处理，但我跳过了。相反，我指定了结果和预测，并添加了模型规格。

```
# Create a workflow
rf_workflow <-
  workflow() %>%
  add_variables(
    outcomes = body_mass_g, predictors = everything()
  ) %>%
  add_model(rf_spec)
```

## 手动提供值

现在，我们终于到了调音阶段！`[tune_grid()](https://tune.tidymodels.org/reference/tune_grid.html)`函数包括交叉验证和参数网格，应该是参数组合的数据框。`expand.grid()`是一种生成所有输入组合的简单方法。我为`mtry`和`trees`分别列出了三个值，产生了九种组合(三乘以三)。

我不知道这是否是一个警钟，我应该更换我五年前的 MacBook，或者建模只是需要时间，调整总是需要大量的时间。完成后，我打电话给`collect_metrics()`查看结果。

```
set.seed(300)
manual_tune <-
  rf_workflow %>%
  tune_grid(
    resamples = folds_5, 
    grid = expand.grid(
      mtry = c(1, 3, 5), 
      trees = c(500, 1000, 2000)
    )
  )collect_metrics(manual_tune) 
## # A tibble: 18 x 8
##     mtry trees .metric .estimator    mean     n std_err .config             
##    <dbl> <dbl> <chr>   <chr>        <dbl> <int>   <dbl> <chr>               
##  1     1   500 rmse    standard   306\.       10  9.84   Preprocessor1_Model1
##  2     1   500 rsq     standard     0.858    10  0.0146 Preprocessor1_Model1
##  3     3   500 rmse    standard   301\.       10 14.3    Preprocessor1_Model2
##  4     3   500 rsq     standard     0.854    10  0.0178 Preprocessor1_Model2
##  5     5   500 rmse    standard   303\.       10 14.5    Preprocessor1_Model3
##  6     5   500 rsq     standard     0.852    10  0.0180 Preprocessor1_Model3
##  7     1  1000 rmse    standard   305\.       10  9.82   Preprocessor1_Model4
##  8     1  1000 rsq     standard     0.859    10  0.0143 Preprocessor1_Model4
##  9     3  1000 rmse    standard   300\.       10 14.5    Preprocessor1_Model5
## 10     3  1000 rsq     standard     0.854    10  0.0180 Preprocessor1_Model5
## 11     5  1000 rmse    standard   304\.       10 14.5    Preprocessor1_Model6
## 12     5  1000 rsq     standard     0.851    10  0.0180 Preprocessor1_Model6
## 13     1  2000 rmse    standard   306\.       10 10.1    Preprocessor1_Model7
## 14     1  2000 rsq     standard     0.858    10  0.0144 Preprocessor1_Model7
## 15     3  2000 rmse    standard   300\.       10 14.5    Preprocessor1_Model8
## 16     3  2000 rsq     standard     0.854    10  0.0179 Preprocessor1_Model8
## 17     5  2000 rmse    standard   304\.       10 14.7    Preprocessor1_Model9
## 18     5  2000 rsq     standard     0.851    10  0.0181 Preprocessor1_Model9 
```

要读的东西太多？我同意。让我们来关注一下使用`show_best()`性能最好的一个。建议`mtry = 3`和`trees = 2000`为最佳参数。

```
show_best(manual_tune, n = 1)
## # A tibble: 1 x 8
##    mtry trees .metric .estimator  mean     n std_err .config             
##   <dbl> <dbl> <chr>   <chr>      <dbl> <int>   <dbl> <chr>               
## 1     3  2000 rmse    standard    300\.    10    14.5 Preprocessor1_Model8 
```

`manual_tune`不是一个模型，而是一个调整网格，所以我需要用最好的参数(`mtry = 3` & `trees = 2000`)来最终确定，并适合整个训练数据。测试的 RMSE 是 296，低于使用默认参数的模型的 317，表明调优产生了改进。

```
manual_final <-
  finalize_workflow(rf_workflow, select_best(manual_tune)) %>%
  fit(penguins_train)

manual_final %>% 
  predict(penguins_test) %>% 
  bind_cols(penguins_test) %>% 
  metrics(body_mass_g, .pred) 
## # A tibble: 3 x 3
##   .metric .estimator .estimate
##   <chr>   <chr>          <dbl>
## 1 rmse    standard     296\.   
## 2 rsq     standard       0.881
## 3 mae     standard     238.
```

## 指定自动生成的网格大小

或者不使用`expand.grid()`作为`grid`参数来手动输入值，我可以用一个整数指定要尝试的候选数。这里，我要求模型尝试五组参数。由于`collect_metrics()`同时返回 RMSE 和 R 平方，每组有两行输出，五组导致十行结果。

```
set.seed(300)
random_tune <-
  rf_workflow %>%
  tune_grid(
    resamples = folds_5, grid = 5
  )collect_metrics(random_tune)
## # A tibble: 10 x 8
##     mtry trees .metric .estimator    mean     n std_err .config             
##    <int> <int> <chr>   <chr>        <dbl> <int>   <dbl> <chr>               
##  1     5  1879 rmse    standard   304\.       10 14.5    Preprocessor1_Model1
##  2     5  1879 rsq     standard     0.851    10  0.0181 Preprocessor1_Model1
##  3     2   799 rmse    standard   298\.       10 13.6    Preprocessor1_Model2
##  4     2   799 rsq     standard     0.857    10  0.0171 Preprocessor1_Model2
##  5     3  1263 rmse    standard   300\.       10 14.5    Preprocessor1_Model3
##  6     3  1263 rsq     standard     0.854    10  0.0179 Preprocessor1_Model3
##  7     2   812 rmse    standard   297\.       10 13.7    Preprocessor1_Model4
##  8     2   812 rsq     standard     0.858    10  0.0171 Preprocessor1_Model4
##  9     4   193 rmse    standard   302\.       10 14.9    Preprocessor1_Model5
## 10     4   193 rsq     standard     0.852    10  0.0182 Preprocessor1_Model5
```

同样，我使用`show_best()`来关注最佳结果。提醒一下，手动调谐的最佳交叉验证 RMSE 是 300，带有`mtry = 3`和`trees = 2000`。

```
show_best(random_tune) 
## # A tibble: 1 x 8
##    mtry trees .metric .estimator  mean     n std_err .config             
##   <int> <int> <chr>   <chr>      <dbl> <int>   <dbl> <chr>               
## 1     2   812 rmse    standard    297\.    10    13.7 Preprocessor1_Model4
```

类似地，我完成工作流程，根据训练数据拟合模型，并测试它。不错！RMSE 从 296 降至 295。

```
random_final <-
  finalize_workflow(rf_workflow, select_best(random_tune)) %>%
  fit(penguins_train)

random_final %>% 
  predict(penguins_test) %>% 
  bind_cols(penguins_test) %>% 
  metrics(body_mass_g, .pred) 
## # A tibble: 3 x 3
##   .metric .estimator .estimate
##   <chr>   <chr>          <dbl>
## 1 rmse    standard     295\.   
## 2 rsq     standard       0.883
## 3 mae     standard     233.
```

# 3.使用刻度盘创建参数值

`dials`处理`parameter`对象。在我的随机森林模型中，有`mtry`和`trees`。每个对象包含关于范围、可能值、类型等的信息。我发现`dials`是一个非常强大和有用的工具，因为太多时候，我不得不通读文档，翻阅我的书籍，或者谷歌来找出给定参数的范围和值。

## 检查参数信息

先来关注一下`mtry`。我看到它是一个数量参数，指的是随机选择的预测因子的数量。范围是从 1 到…等等…？一个问号？我们再试试`range_get()`看看范围。现在还不得而知。

```
mtry()
*## # Randomly Selected Predictors (quantitative)*
*## Range: [1, ?]*

mtry() %>% range_get()
*## $lower*
*## [1] 1*
*##* 
*## $upper*
*## unknown()*
```

嗯，这是因为区间上限取决于数据中预测因子的个数，我需要手动指定值。我这样做是通过得到列数并减去 1(结果变量)。有两种方法可以做到这一点，如下所示。

```
# Option 1: Use range_set
mtry() %>% range_set(c(1, ncol(penguins_train) - 1))
*## # Randomly Selected Predictors (quantitative)*
*## Range: [1, 5]*# Options 2: Include in the argument
mtry(c(1, ncol(penguins_train) - 1))
*## # Randomly Selected Predictors (quantitative)*
*## Range: [1, 5]*
```

用`trees`试试吧。参数不依赖于数据，因此提供了范围。

```
trees()
*## # Trees (quantitative)*
*## Range: [1, 2000]*
```

## 为参数创建值

现在，我知道了参数，我如何创造价值？有两种方法。我可以使用`value_seq()`生成一个跨越整个范围的`n`数字序列。在这里，我试图得到 4、5 和 10 个数字。如您所见，包括了`trees`的最小值和最大值。

```
trees() %>% value_seq(n = 4)
*## [1]    1  667 1333 2000*

trees() %>% value_seq(n = 5)
*## [1]    1  500 1000 1500 2000*

trees() %>% value_seq(n = 10)
*##  [1]    1  223  445  667  889 1111 1333 1555 1777 2000*
```

或者我可以用`value_sample()`生成随机数。

```
set.seed(300)
trees() %>% value_sample(n = 4)
*## [1]  590  874 1602  985*

trees() %>% value_sample(n = 5)
*## [1] 1692  789  553 1980 1875*

trees() %>% value_sample(n = 10)
*##  [1] 1705  272  461  780 1383 1868 1107  812  460  901*
```

## 为参数创建网格

让我们回忆一下`tune_grid()`，调整参数的功能。它要求格网是一个数据框。返回向量的两种方法。那么我如何生成一个网格呢？当然，我可以简单地将向量转换成数据帧。但是，`dials`有更好的方法做到这一点。同样，有两种方法:创建一个数字序列和创建一组随机数。

使用`grid_regular()`创建带序列的网格。添加自变量`mtry()`和`trees()`中的参数，并指定每个参数的级别。我希望每个参数有三个级别，产生九个组合(三乘以三)。

```
set.seed(300)
dials_regular <- grid_regular(
  mtry(c(1, ncol(penguins_train) - 1)),
  trees(),
  levels = 3
)
dials_regular
*## # A tibble: 9 x 2*
*##    mtry trees*
*##   <int> <int>*
*## 1     1     1*
*## 2     3     1*
*## 3     5     1*
*## 4     1  1000*
*## 5     3  1000*
*## 6     5  1000*
*## 7     1  2000*
*## 8     3  2000*
*## 9     5  2000*
```

对于随机数，使用`grid_random()`并指定`size`。

```
set.seed(300)
dials_random <- grid_random(
  mtry(c(1, ncol(penguins_train) - 1)),
  trees(),
  size = 6
)
dials_random
*## # A tibble: 6 x 2*
*##    mtry trees*
*##   <int> <int>*
*## 1     2  1980*
*## 2     2  1875*
*## 3     1  1705*
*## 4     4   272*
*## 5     5   461*
*## 6     1   780*
```

## 使用带有 tune_grid()的刻度盘

这两种方法都创建了一个准备好与`tune_grid()`一起使用的数据帧。对于`grid_regular()`:

```
dials_regular_tune <-
  rf_workflow %>%
  tune_grid(
    resamples = folds_5, grid = dials_regular
  )show_best(dials_regular_tune, n = 1)
*## # A tibble: 1 x 8*
*##    mtry trees .metric .estimator  mean     n std_err .config* 
*##   <int> <int> <chr>   <chr>      <dbl> <int>   <dbl> <chr>* 
*## 1     3  1000 rmse    standard    300\.    10    14.4 Preprocessor1_Model5*dials_regular_final <-
  finalize_workflow(
    rf_workflow, select_best(dials_regular_tune)
  ) %>%
  fit(penguins_train)

dials_regular_final %>% 
  predict(penguins_test) %>% 
  bind_cols(penguins_test) %>% 
  metrics(body_mass_g, .pred)
*## # A tibble: 3 x 3*
*##   .metric .estimator .estimate*
*##   <chr>   <chr>          <dbl>*
*## 1 rmse    standard     296\.* 
*## 2 rsq     standard       0.881*
*## 3 mae     standard     237.*
```

对于`grid_random()`:

```
dials_random_tune <-
  rf_workflow %>%
  tune_grid(
    resamples = folds_5, grid = dials_random
  )show_best(dials_random_tune, n = 1)
*## # A tibble: 1 x 8*
*##    mtry trees .metric .estimator  mean     n std_err .config* 
*##   <int> <int> <chr>   <chr>      <dbl> <int>   <dbl> <chr>* 
*## 1     2  1875 rmse    standard    297\.    10    13.6 Preprocessor1_Model2*dials_random_final <-
  finalize_workflow(
    rf_workflow, select_best(dials_random_tune)
  ) %>%
  fit(penguins_train)

dials_random_final %>% 
  predict(penguins_test) %>% 
  bind_cols(penguins_test) %>% 
  metrics(body_mass_g, .pred)
*## # A tibble: 3 x 3*
*##   .metric .estimator .estimate*
*##   <chr>   <chr>          <dbl>*
*## 1 rmse    standard     296\.* 
*## 2 rsq     standard       0.882*
*## 3 mae     standard     234.*
```

# 结论

我在文章中解释了三种调整参数的方法。

1.  在`parsnip`中应用默认值:不指定参数值，`parsnip`将使用所选引擎的默认值。
2.  将`tune`与`parsnip`一起使用:`tune_grid()`功能交叉验证一组参数。它可以处理预定义的数据帧或生成一组随机数。
3.  用`dials`创建将在`tune`中使用的值，以交叉验证`parsnip`模型:`dials`提供关于参数的信息，并为它们生成值。这些值可以是一系列跨越一定范围的数字，也可以是一组随机数。

我非常喜欢简洁明了的代码。刚开始玩`dials`的时候感觉很多余，没必要，因为我本来可以直接用`tune`的。然而，随着我对`dials`越来越熟悉，我开始明白最初为什么要创建它。我再也不用回头看文档看参数是整数还是浮点数，有没有范围限制，是否需要变换。

我花了一段时间才把从`rsample`和`recipe`到`tune`和`dials`的所有东西完全拼在一起。我希望你喜欢这篇文章，并有一个美好的一天！以下是所有代码的要点:

## 参考

Gorman KB、Williams TD、Fraser WR (2014)南极企鹅(*Pygoscelis*属)群落中的生态两性异形和环境可变性。PLoS ONE 9(3): e90081。[https://doi.org/10.1371/journal.pone.0090081](https://doi.org/10.1371/journal.pone.0090081)

【https://github.com/allisonhorst/palmerpenguins】