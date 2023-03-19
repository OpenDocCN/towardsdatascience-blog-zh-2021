# 如何在 Python 中减小熊猫数据帧的大小

> 原文：<https://towardsdatascience.com/how-to-reduce-the-size-of-a-pandas-dataframe-in-python-7ed6e4269f88?source=collection_archive---------9----------------------->

## 减小数据的大小有时会很棘手。在这个快速教程中，我们将演示如何通过向下转换将数据帧的大小减少一半，让您事半功倍。

![](img/389122f1c331f43751498cc1129918cd.png)

Guillaume de Germain 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 背景

无论您是为业余爱好项目还是为工作目的构建模型，您的第一次尝试都可能包括打开 jupyter 笔记本并读取一些数据。最终，你的笔记本肯定会遇到内存问题。在开始删除行或尝试复杂的采样技术来减小数据大小之前，应该检查数据的结构。

# 了解我们数据的规模

为了探索如何减少数据集的大小，我们需要一些样本数据。对于本教程，我使用的是我为 Kaggle 上的未来销售预测竞赛创建的旧数据集([这里是我关于预处理数据](https://nankarstad.medium.com/how-to-use-lightgbm-and-boosted-decision-trees-forecast-sales-cf65ce8ab645)的文章)。完全连接和功能工程化后，数据集有 58 列和 11，128，050 条记录。对于一台小型笔记本电脑来说，这是很大的数据量。我们需要一个减少数据量的解决方案。

在我们开始之前，我们应该检查了解更多的数据。熊猫库中的 df.info()是一个非常有用的函数。

```
df.info(memory_usage = "deep")
```

这段代码 snippit 返回以下输出:

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 11128050 entries, 0 to 11128049
Data columns (total 58 columns):
 #   Column                                            Dtype         
---  ------                                            -----         
 0   date_block_num                                    int64         
 1   item_id                                           int64         
 2   shop_id                                           int64         
 3   target                                            float64       
 4   item_name                                         object        
 5   item_category_id                                  int64         
 6   Category_type                                     object        
 7   City                                              object        
 8   Month_End_Date                                    datetime64[ns]
 9   Number_of_Mondays                                 int64         
 10  Number_of_Tuesdays                                int64         
 11  Number_of_Wednesdays                              int64         
 12  Number_of_Thursdays                               int64         
 13  Number_of_Fridays                                 int64         
 14  Number_of_Saturdays                               int64         
 15  Number_of_Sundays                                 int64         
 16  Year                                              int64         
 17  Month                                             int64         
 18  Days_in_Month                                     int64         
 19  min_item_sale_date_block_num                      int64         
 20  Months_Since_Item_First_Sold                      int64         
 21  avg_first_months_sales_by_item_category_id        float64       
 22  avg_first_months_sales_by_item_category_and_shop  float64       
 23  target_lag_1                                      float64       
 24  target_lag_2                                      float64       
 25  target_lag_3                                      float64       
 26  target_lag_4                                      float64       
 27  target_lag_5                                      float64       
 28  target_lag_6                                      float64       
 29  target_lag_12                                     float64       
 30  avg_monthly_by_item_lag_1                         float64       
 31  avg_monthly_by_item_lag_2                         float64       
 32  avg_monthly_by_item_lag_3                         float64       
 33  avg_monthly_by_item_lag_6                         float64       
 34  avg_monthly_by_item_lag_12                        float64       
 35  avg_monthly_by_shop_lag_1                         float64       
 36  avg_monthly_by_shop_lag_2                         float64       
 37  avg_monthly_by_shop_lag_3                         float64       
 38  avg_monthly_by_shop_lag_6                         float64       
 39  avg_monthly_by_shop_lag_12                        float64       
 40  avg_monthly_by_category_lag_1                     float64       
 41  avg_monthly_by_category_lag_2                     float64       
 42  avg_monthly_by_category_lag_3                     float64       
 43  avg_monthly_by_category_lag_6                     float64       
 44  avg_monthly_by_category_lag_12                    float64       
 45  avg_monthly_by_city_lag_1                         float64       
 46  avg_monthly_by_city_lag_2                         float64       
 47  avg_monthly_by_city_lag_3                         float64       
 48  avg_monthly_by_city_lag_6                         float64       
 49  avg_monthly_by_city_lag_12                        float64       
 50  item_price_lag_1                                  float64       
 51  item_price_lag_2                                  float64       
 52  item_price_lag_3                                  float64       
 53  item_price_lag_4                                  float64       
 54  item_price_lag_5                                  float64       
 55  item_price_lag_6                                  float64       
 56  target_3_month_avg                                float64       
 57  target_6_month_avg                                float64       
dtypes: datetime64[ns](1), float64(38), int64(16), object(3)
memory usage: 7.6 GB
```

使用 df.info，我们可以获得以下信息:

*   行数或条目数
*   列数
*   每列的索引和名称
*   每列的数据类型
*   数据帧的总内存使用量
*   每种数据类型有多少列

仔细观察这个表，我们可以看到每个数据类型后面的数字说明了它们使用的位数。

由于其中许多被列为 int64 或 float64，我们可能会将它们减少到更小的空间数据类型，如 int16 或 float8。向下转换意味着我们将每个特性的数据类型减少到最低可能的类型。

```
## downcasting loop
for column in df:
 if df[column].dtype == ‘float64’:
 df[column]=pd.to_numeric(df[column], downcast=’float’)
 if df[column].dtype == ‘int64’:
 df[column]=pd.to_numeric(all_data[column], downcast=’integer’)## dropping an unused column
df = df.drop('item_name',axis =1)
```

我们可以再次检查数据帧的大小。

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 11128050 entries, 0 to 11128049
Data columns (total 57 columns):
 #   Column                                            Dtype         
---  ------                                            -----         
 0   date_block_num                                    int8          
 1   item_id                                           int16         
 2   shop_id                                           int8          
 3   target                                            float32       
 4   item_category_id                                  int8          
 5   Category_type                                     object        
 6   City                                              object        
 7   Month_End_Date                                    datetime64[ns]
 8   Number_of_Mondays                                 int8          
 9   Number_of_Tuesdays                                int8          
 10  Number_of_Wednesdays                              int8          
 11  Number_of_Thursdays                               int8          
 12  Number_of_Fridays                                 int8          
 13  Number_of_Saturdays                               int8          
 14  Number_of_Sundays                                 int8          
 15  Year                                              int16         
 16  Month                                             int8          
 17  Days_in_Month                                     int8          
 18  min_item_sale_date_block_num                      int8          
 19  Months_Since_Item_First_Sold                      int8          
 20  avg_first_months_sales_by_item_category_id        float32       
 21  avg_first_months_sales_by_item_category_and_shop  float32       
 22  target_lag_1                                      float32       
 23  target_lag_2                                      float32       
 24  target_lag_3                                      float32       
 25  target_lag_4                                      float32       
 26  target_lag_5                                      float32       
 27  target_lag_6                                      float32       
 28  target_lag_12                                     float32       
 29  avg_monthly_by_item_lag_1                         float32       
 30  avg_monthly_by_item_lag_2                         float32       
 31  avg_monthly_by_item_lag_3                         float32       
 32  avg_monthly_by_item_lag_6                         float32       
 33  avg_monthly_by_item_lag_12                        float32       
 34  avg_monthly_by_shop_lag_1                         float32       
 35  avg_monthly_by_shop_lag_2                         float32       
 36  avg_monthly_by_shop_lag_3                         float32       
 37  avg_monthly_by_shop_lag_6                         float32       
 38  avg_monthly_by_shop_lag_12                        float32       
 39  avg_monthly_by_category_lag_1                     float32       
 40  avg_monthly_by_category_lag_2                     float32       
 41  avg_monthly_by_category_lag_3                     float32       
 42  avg_monthly_by_category_lag_6                     float32       
 43  avg_monthly_by_category_lag_12                    float32       
 44  avg_monthly_by_city_lag_1                         float32       
 45  avg_monthly_by_city_lag_2                         float32       
 46  avg_monthly_by_city_lag_3                         float32       
 47  avg_monthly_by_city_lag_6                         float32       
 48  avg_monthly_by_city_lag_12                        float32       
 49  item_price_lag_1                                  float32       
 50  item_price_lag_2                                  float32       
 51  item_price_lag_3                                  float32       
 52  item_price_lag_4                                  float32       
 53  item_price_lag_5                                  float32       
 54  item_price_lag_6                                  float32       
 55  target_3_month_avg                                float32       
 56  target_6_month_avg                                float32       
dtypes: datetime64[ns](1), float32(38), int16(2), int8(14), object(2)
memory usage: 3.2 GB
```

我们看到现在有几个 8、16 和 32 位的列，通过简单地更改数据类型，我们已经有效地将数据帧的大小减半。

更有趣的是，dataframe 的内存使用量下降到了 3.2gb。这大约是我们刚开始时 7.6gb 的一半。通过这种类型的内存减少，我们可以将更多的数据加载到更小的机器上，最终降低成本。

# 结论

在本教程中，我们将 1100 万条记录数据集加载到熊猫数据帧中。我们学习了如何通过使用？pandas 中的 info()函数。这为我们提供了有用的信息，如行数和列数、数据帧的大小和内存使用情况以及每列的数据类型。

我们学习了向下转换，我们用它将数据帧的大小减半，这样我们可以使用更少的内存，对数据做更多的事情。