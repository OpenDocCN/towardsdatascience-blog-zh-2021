# 数据科学的超能力:缺失数据

> 原文：<https://towardsdatascience.com/data-science-superpowers-missing-data-d814ede9d423?source=collection_archive---------48----------------------->

## 使用最大似然预测输入缺失值

![](img/47d70ab5ec51977844339e709761e37b.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

数据并不总是漂亮的…

…好吧，让我们面对现实:数据几乎*从来都不*漂亮。

丢失数据是非常常见的事情，有许多方法可以解决这个问题。有时删除缺少值的条目没问题，其他时候使用平均值、中值或众数进行输入是一种方便的策略。

**但是还有一个办法！**

# DS 超级大国:用 ML 预测填充缺失值！

因此，您正在进行一个项目，在这个项目中，您将要预测一些事情(假设`house_price`)，并且您在预测列中遇到了一些丢失的数据。

在我们预测`y`之前，先预测一下`x`！

![](img/ddc7ec1e397c9d6a61af3584c69da0b5.png)

史蒂文·卡梅纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 示例:设置

导入库。

```
# Import libraries
import pandas as pd
import numpy as np
from sklearn.datasets import make_regression
from sklearn.ensemble import RandomForestRegressor
```

创建玩具数据集。

```
# Set random_state variable.
R = 2020# Set up phony names for features.
x_vars = [
    'year_built',
    'bedrooms',
    'bathrooms',
    'sqft',
    'lot_size'
]
y_var = ['sale_price']# Make a toy dataset.
X, y = make_regression(
    n_samples=1_000,
    n_features=5,
    n_targets=1,
    random_state=R)# Convert to pandas.
X = pd.DataFrame(X, columns=x_vars)
y = pd.DataFrame(y, columns=y_var)# Set 30 random missing values in `year_built` column.
X.loc[X.sample(30, random_state=R).index, 'year_built'] = np.nan
```

我们现在有两只熊猫数据帧:`X`和`y`。这些列已经有了名称——尽管这些名称对于数据值来说毫无意义，但是我们将使用它们进行演示。

重要的是，我们在整个`year_built`列中遗漏了一些值。我们希望使用 ML 来填充这些值，而不是丢弃它们或使用平均值。

```
>>> X.shape, y.shape
((1000, 5), (1000, 1))>>> X.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 5 columns):
year_built    970 non-null float64
bedrooms      1000 non-null float64
bathrooms     1000 non-null float64
sqft          1000 non-null float64
lot_size      1000 non-null float64
dtypes: float64(5)
memory usage: 39.2 KB
```

根据我们的数据，事情看起来是有条不紊的。让我们开始有趣的部分吧！

# 示例:流程走查。

这很简单，只需从缺少值的列中分离出预测列，实例化一个模型，拟合它，并将其预测设置到数据框架中！先一步一步来，然后功能化。

1.  **确定哪个 X 列将用于预测，哪个 X 列是新的临时目标。**在我们的例子中，我们将使用`X_cols = ['bedrooms', 'bathrooms', 'sqft', 'lot_size']`和`y_col = 'year_built'`。

2.**实例化并拟合模型。我喜欢使用随机森林模型，因为它们不容易过度拟合，并且可以获取未缩放的数据。**

```
rf = RandomForestRegressor(random_state=R)# Filter data that does not include rows where `y_col` is missing.
rf.fit(
    df[~df[y_col].isna()][X_cols],
    df[~df[y_col].isna()][y_col]
)
```

3.**获得预测并设置到数据帧中。**

```
y_pred = rf.predict(df[df[y_col].isna()][X_cols])df.loc[df[y_col].isna(), y_col] = y_pred
```

# 就是这么回事！简单！

当然这个过程是喊着要功能化的，那就干吧！我们还应该给出一些详细信息，并检查森林的适合程度。

# 用 RF:函数填充缺失值

```
def fill_missing_values_with_rf(df: pd.DataFrame, 
                                X_cols: list,
                                y_col: str,
                                astype: type,
                                random_state: int,
                                verbose=True):
    """
    Replace missing values from `y_col` with predictions from a 
    RandomForestRegressor.
    """

    if not df[y_col].isna().sum():
        if verbose:
            print(f'No missing values found in `{y_col}`.')
        return df
    df = df.copy()

    # Instantiate and fit model.
    if verbose:
        print('Instantiating and fitting model...')
    rf = RandomForestRegressor(random_state=random_state)
    rf.fit(
        df[~df[y_col].isna()][X_cols],
        df[~df[y_col].isna()][y_col]
    )
    if verbose:
        print('\tModel fit.')
        r2score = rf.score(
            df[~df[y_col].isna()][X_cols],
            df[~df[y_col].isna()][y_col]
        )
        print(f'\tModel `r^2`: {round(r2score, 3)}')

    # Get predictions.
    if verbose:
        print('Predicting values...')
    y_pred = rf.predict(df[df[y_col].isna()][X_cols])

    # Set values in df.
    if verbose:
        print('Setting values...')
    df.loc[df[y_col].isna(), y_col] = y_pred

    # Set dtype.
    df[y_col] = df[y_col].astype(astype)

    if verbose:
        print('Complete!')
    return df
```

**我们现在可以看到运行中的函数:**

```
>>> new_df = fill_missing_values_with_rf(
        X, 
        ['bedrooms', 'bathrooms', 'sqft', 'lot_size'],
        'year_built',
        float,
        R)
Instantiating and fitting model...
	Model fit.
	Model `r^2`: 0.844
Predicting values...
Setting values...
Complete!>>> new_df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 5 columns):
year_built    1000 non-null float64
bedrooms      1000 non-null float64
bathrooms     1000 non-null float64
sqft          1000 non-null float64
lot_size      1000 non-null float64
dtypes: float64(5)
memory usage: 39.2 KB
```

# 看起来棒极了！

我们研究了一个成熟的过程，即获取带有缺失值的数据，并用我们最擅长的方法智能地填充它们:建模！

这种数据处理无疑是 DS 的一大优势。我们可以预测未来、过去和未知——我们应该利用这一点！

*用任何方法填充缺失数据都要用心。*