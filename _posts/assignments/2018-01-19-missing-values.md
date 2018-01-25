---
layout: post
title:  EDA - Missing Values
date:   2017-12-04 13:09:10 +0100
categories: assignments
tag: Python
---
## 5. Missing Values

```python
import pandas as pd
import numpy as np
df = pd.read_csv('train.csv')
```

### Missing Data

Important questions when thinking about missing data:
- How prevalent is the missing data?
- Is missing data random or does it have a pattern?

The answer to these questions is important for practical reasons because missing data can imply a reduction of the sample size. This can prevent us from proceeding with the analysis. Moreover, from a substantive perspective, we need to ensure that the missing data process is not biased and hiding an inconvenient truth.


```python
df.Alley.unique()
```




    array([nan, 'Grvl', 'Pave'], dtype=object)




```python
#missing data
total = df.isnull().sum().sort_values(ascending=False)
percent = (df.isnull().sum() / df.count()).sort_values(ascending=False)
missing_data = pd.concat([total, percent], axis=1, keys=['Total', 'Percent'])
missing_data.head(20)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total</th>
      <th>Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>PoolQC</th>
      <td>1453</td>
      <td>207.571429</td>
    </tr>
    <tr>
      <th>MiscFeature</th>
      <td>1406</td>
      <td>26.037037</td>
    </tr>
    <tr>
      <th>Alley</th>
      <td>1369</td>
      <td>15.043956</td>
    </tr>
    <tr>
      <th>Fence</th>
      <td>1179</td>
      <td>4.195730</td>
    </tr>
    <tr>
      <th>FireplaceQu</th>
      <td>690</td>
      <td>0.896104</td>
    </tr>
    <tr>
      <th>LotFrontage</th>
      <td>259</td>
      <td>0.215654</td>
    </tr>
    <tr>
      <th>GarageCond</th>
      <td>81</td>
      <td>0.058738</td>
    </tr>
    <tr>
      <th>GarageType</th>
      <td>81</td>
      <td>0.058738</td>
    </tr>
    <tr>
      <th>GarageYrBlt</th>
      <td>81</td>
      <td>0.058738</td>
    </tr>
    <tr>
      <th>GarageFinish</th>
      <td>81</td>
      <td>0.058738</td>
    </tr>
    <tr>
      <th>GarageQual</th>
      <td>81</td>
      <td>0.058738</td>
    </tr>
    <tr>
      <th>BsmtExposure</th>
      <td>38</td>
      <td>0.026723</td>
    </tr>
    <tr>
      <th>BsmtFinType2</th>
      <td>38</td>
      <td>0.026723</td>
    </tr>
    <tr>
      <th>BsmtFinType1</th>
      <td>37</td>
      <td>0.026001</td>
    </tr>
    <tr>
      <th>BsmtCond</th>
      <td>37</td>
      <td>0.026001</td>
    </tr>
    <tr>
      <th>BsmtQual</th>
      <td>37</td>
      <td>0.026001</td>
    </tr>
    <tr>
      <th>MasVnrArea</th>
      <td>8</td>
      <td>0.005510</td>
    </tr>
    <tr>
      <th>MasVnrType</th>
      <td>8</td>
      <td>0.005510</td>
    </tr>
    <tr>
      <th>Electrical</th>
      <td>1</td>
      <td>0.000685</td>
    </tr>
    <tr>
      <th>Utilities</th>
      <td>0</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### Let's analyse this to understand how to handle the missing data. 

In some cases, missing data indicates that a value is simply not applicable. Notice for instance that the variables whose names start with 'Garage' all have the same number of missing values. This is likely because 81 of the houses do not have a garage. To use a variable like GarageType, we could replace the missing values with a new category 'No Garage'. The same logic applies to variables whose names start with 'Bsmt' which is short for Basement.

Alley has a high percentage of missing data. The code book tells us the values are Paved, Gravel or NA (not available). It appears NA was incorrectly interpreted by Python as a missing value, so we can safely replace the missing values with 'NA' to restore those. The same applies to PoolQC and Fence; for Fence NA means 'No Fence'.

The variable LotFrontage means the number of feet that the lot is connected to the main road. For LotFrontage there are 18% missing values, possibly because 18% of the houses does not have a lot. If we were to replace those values with 0, we might be able to use the variable in the prediction of SalePrice. On the other hand, LotFrontage may not matter that much, so we may be ok to just to just delete it. In fact, in this tutorial we will simply remove all variables with > 15% mising values. However, if one of these variables is interesting we should not dismiss them too easily. 

Regarding 'MasVnrArea' and 'MasVnrType', we can consider that these variables are not essential. Furthermore, they have a strong correlation with 'YearBuilt' and 'OverallQual' which are already considered. Thus, we are not likely to lose information if we delete 'MasVnrArea' and 'MasVnrType'.

Finally, we have one missing observation in 'Electrical'. Since it is just one observation, we'll delete this observation and keep the variable. Yo have to be careful though, not to remove too many samples or to bias your dataset by removing samples.

In summary, to handle missing data, we'll delete all the variables with missing data, except the variable 'Electrical'. In 'Electrical' we'll just delete the observation with missing data


```python
#dealing with missing data
#df = df.drop((missing_data[missing_data['Total'] > 1]).index,1)
#df = df.drop(df.loc[df['Electrical'].isnull()].index)
df.isnull().sum().max() #just checking that there's no missing data missing...

df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1459 entries, 0 to 1459
    Data columns (total 63 columns):
    Id               1459 non-null int64
    MSSubClass       1459 non-null int64
    MSZoning         1459 non-null object
    LotArea          1459 non-null int64
    Street           1459 non-null object
    LotShape         1459 non-null object
    LandContour      1459 non-null object
    Utilities        1459 non-null object
    LotConfig        1459 non-null object
    LandSlope        1459 non-null object
    Neighborhood     1459 non-null object
    Condition1       1459 non-null object
    Condition2       1459 non-null object
    BldgType         1459 non-null object
    HouseStyle       1459 non-null object
    OverallQual      1459 non-null int64
    OverallCond      1459 non-null int64
    YearBuilt        1459 non-null int64
    YearRemodAdd     1459 non-null int64
    RoofStyle        1459 non-null object
    RoofMatl         1459 non-null object
    Exterior1st      1459 non-null object
    Exterior2nd      1459 non-null object
    ExterQual        1459 non-null object
    ExterCond        1459 non-null object
    Foundation       1459 non-null object
    BsmtFinSF1       1459 non-null int64
    BsmtFinSF2       1459 non-null int64
    BsmtUnfSF        1459 non-null int64
    TotalBsmtSF      1459 non-null int64
    Heating          1459 non-null object
    HeatingQC        1459 non-null object
    CentralAir       1459 non-null object
    Electrical       1459 non-null object
    1stFlrSF         1459 non-null int64
    2ndFlrSF         1459 non-null int64
    LowQualFinSF     1459 non-null int64
    GrLivArea        1459 non-null int64
    BsmtFullBath     1459 non-null int64
    BsmtHalfBath     1459 non-null int64
    FullBath         1459 non-null int64
    HalfBath         1459 non-null int64
    BedroomAbvGr     1459 non-null int64
    KitchenAbvGr     1459 non-null int64
    KitchenQual      1459 non-null object
    TotRmsAbvGrd     1459 non-null int64
    Functional       1459 non-null object
    Fireplaces       1459 non-null int64
    GarageCars       1459 non-null int64
    GarageArea       1459 non-null int64
    PavedDrive       1459 non-null object
    WoodDeckSF       1459 non-null int64
    OpenPorchSF      1459 non-null int64
    EnclosedPorch    1459 non-null int64
    3SsnPorch        1459 non-null int64
    ScreenPorch      1459 non-null int64
    PoolArea         1459 non-null int64
    MiscVal          1459 non-null int64
    MoSold           1459 non-null int64
    YrSold           1459 non-null int64
    SaleType         1459 non-null object
    SaleCondition    1459 non-null object
    SalePrice        1459 non-null int64
    dtypes: int64(35), object(28)
    memory usage: 729.5+ KB


#### Assignment: 

inspect what df.drop(df.loc[df['Electrical'].isnull()].index) does by inspecting 
