---
layout: post
title:  EDA - Data Edges
date:   2017-12-04 13:09:10 +0100
categories: assignments
tag: Python
---

## 1. Checking data edges


```python
import pandas as pd
df = pd.read_csv('train.csv')
```

Pandas reads the file as a `Dataframe` object, which resembles a table in a database. The header (first line) provides the column labels, and every row in the file is inserted as a row in the Dataframe. We can view the first view lines with the `head()` method. You can also use `head(n)` to show the first `n` lines.

### the number of rows of a dataframe

If you know how many samples there should be (for instance from the source where you obtained the data) check it.

#### Assignment: Enter code to get the number of rows in the dataframe using the len() function, the correct answer should be 1460.


```python
len(df)
```




    1460



### columns in the dataframe

Similarly, if you known how many features there should be, check it. Also check for key features that you need for the problem you want to study. In this dataset, since the task is to predict the target variable SalePrice, we would require features that we expect to be most useful such as the size of the house.


```python
df.columns
```




    Index(['Id', 'MSSubClass', 'MSZoning', 'LotFrontage', 'LotArea', 'Street',
           'Alley', 'LotShape', 'LandContour', 'Utilities', 'LotConfig',
           'LandSlope', 'Neighborhood', 'Condition1', 'Condition2', 'BldgType',
           'HouseStyle', 'OverallQual', 'OverallCond', 'YearBuilt', 'YearRemodAdd',
           'RoofStyle', 'RoofMatl', 'Exterior1st', 'Exterior2nd', 'MasVnrType',
           'MasVnrArea', 'ExterQual', 'ExterCond', 'Foundation', 'BsmtQual',
           'BsmtCond', 'BsmtExposure', 'BsmtFinType1', 'BsmtFinSF1',
           'BsmtFinType2', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'Heating',
           'HeatingQC', 'CentralAir', 'Electrical', '1stFlrSF', '2ndFlrSF',
           'LowQualFinSF', 'GrLivArea', 'BsmtFullBath', 'BsmtHalfBath', 'FullBath',
           'HalfBath', 'BedroomAbvGr', 'KitchenAbvGr', 'KitchenQual',
           'TotRmsAbvGrd', 'Functional', 'Fireplaces', 'FireplaceQu', 'GarageType',
           'GarageYrBlt', 'GarageFinish', 'GarageCars', 'GarageArea', 'GarageQual',
           'GarageCond', 'PavedDrive', 'WoodDeckSF', 'OpenPorchSF',
           'EnclosedPorch', '3SsnPorch', 'ScreenPorch', 'PoolArea', 'PoolQC',
           'Fence', 'MiscFeature', 'MiscVal', 'MoSold', 'YrSold', 'SaleType',
           'SaleCondition', 'SalePrice'],
          dtype='object')



### the number of columns in a dataframe


```python
len(df.columns)
```

### Inspecting the first and last rows

One of the reasons you want to inpect the first and last rows is to make sure all the data was read ok. Sometimes, files are split, meaning you end up with half a record, or someone inserted some commentary on the first or last lines.


```python
df.head() # first rows
```

#### Assignment: Enter the code to only inspect the first row, hint you can pass the number of rows to head()


```python
df.head(1)
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
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>208500</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 81 columns</p>
</div>



We can view the datatypes and the number of non-null values, using the `info()` method. Null refers to 'no value' or 'unkown value'.


```python
df.tail() # last 5 rows
```

### subsetting rows


```python
# select the first two rows (the upper bound is always exclusive)
df[:2]
```


```python
# select the last 2 rows, negative numbers are an index that count back from the end of the dataframe
df[-2:]
```

#### Assignment: select rows 2 and 3 in one statement


```python
df[2:4]
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
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>140000</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 81 columns</p>
</div>



### Some additional inspections
### Subset rows by boolean indexing (logical statements)


```python
# creates a boolean index that indicates which rows have '04' in ExpMM.
df.MSZoning == 'RL'
```


```python
# create a subset dataframe of only rows that contain 'IR1' for the column LotShape
dfrl = df[df.LotShape == 'IR1']
dfrl.head(4)
```


```python
# create a subset dataframe of only rows that contain
# 'IR1' for the column LotShape AND '2008' for YrSold
dfrlreg = df[(df.LotShape == 'IR1') | (df.YrSold == 2008)]
dfrlreg.head(4)
```


```python
# create a subset dataframe of only rows that contain
# IR1' for the column LotShape OR '2008' for YrSold
dfrlreg = df[(df.LotShape == 'IR1') | (df.YrSold == 2008)]
dfrlreg.head(4)
```

#### Assignment: Show the sizes of the poolareas of the 7 houses that have a pool area (tip: PoolArea > 0)


```python
dfpoolareas = df[(df.PoolArea > 0)]
dfpoolareas
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
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>197</th>
      <td>198</td>
      <td>75</td>
      <td>RL</td>
      <td>174.0</td>
      <td>25419</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>512</td>
      <td>Ex</td>
      <td>GdPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>3</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>235000</td>
    </tr>
    <tr>
      <th>810</th>
      <td>811</td>
      <td>20</td>
      <td>RL</td>
      <td>78.0</td>
      <td>10140</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>648</td>
      <td>Fa</td>
      <td>GdPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>2006</td>
      <td>WD</td>
      <td>Normal</td>
      <td>181000</td>
    </tr>
    <tr>
      <th>1170</th>
      <td>1171</td>
      <td>80</td>
      <td>RL</td>
      <td>76.0</td>
      <td>9880</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>576</td>
      <td>Gd</td>
      <td>GdPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>7</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>171000</td>
    </tr>
    <tr>
      <th>1182</th>
      <td>1183</td>
      <td>60</td>
      <td>RL</td>
      <td>160.0</td>
      <td>15623</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>555</td>
      <td>Ex</td>
      <td>MnPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>7</td>
      <td>2007</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>745000</td>
    </tr>
    <tr>
      <th>1298</th>
      <td>1299</td>
      <td>60</td>
      <td>RL</td>
      <td>313.0</td>
      <td>63887</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR3</td>
      <td>Bnk</td>
      <td>AllPub</td>
      <td>...</td>
      <td>480</td>
      <td>Gd</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>2008</td>
      <td>New</td>
      <td>Partial</td>
      <td>160000</td>
    </tr>
    <tr>
      <th>1386</th>
      <td>1387</td>
      <td>60</td>
      <td>RL</td>
      <td>80.0</td>
      <td>16692</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>519</td>
      <td>Fa</td>
      <td>MnPrv</td>
      <td>TenC</td>
      <td>2000</td>
      <td>7</td>
      <td>2006</td>
      <td>WD</td>
      <td>Normal</td>
      <td>250000</td>
    </tr>
    <tr>
      <th>1423</th>
      <td>1424</td>
      <td>80</td>
      <td>RL</td>
      <td>NaN</td>
      <td>19690</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>738</td>
      <td>Gd</td>
      <td>GdPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>8</td>
      <td>2006</td>
      <td>WD</td>
      <td>Alloca</td>
      <td>274970</td>
    </tr>
  </tbody>
</table>
<p>7 rows × 81 columns</p>
</div>



### Subsetting columns
More advanced subsetting can be done with df.ix. The first range selects the rows (: selects all). The second range selects columns, either by index number of label name. Somewhat confusing, the upperbounds are inclusive here.


```python
# More advanced subsetting can be done with df.ix. The first range selects the rows. 
df.ix[:5, 'LotArea':'LotShape']
```


```python
# alternatively, give a list of columns.
df[['1stFlrSF', '2ndFlrSF']]
```