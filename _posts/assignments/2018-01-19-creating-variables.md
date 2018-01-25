---
layout: post
title:  EDA - Bivariate Analysis
date:   2017-12-04 13:09:10 +0100
categories: assignments
tag: Python
---
## 8. Creating Variables


```python
import pandas as pd
```

```python
df = pd.read_csv('train.csv')
```

### Dummy variables

So far we have addressed numerical variables. Suppose that we want to use a fast regression algorithm on this data, the data would need to be all in a numerical form the form, so how can we include categorical values?

If there are just two labels or of the values are ordinal (there is a natural order in the labels e.g. child, teen, adult), we have the option to replace the labels with numbers like we did for PavedDrive. 

However, if the values are on a nominal scale (i.e. there is no order in the labels) then simply converting them to numbers is not suitable for learning regression. For example ‘BldgType’ can be one of five possible values: ‘1Fam’, ‘2fmCon’, ‘Duplex’, ‘TwnhsE’, or  ‘Twnhs’. The solution is to create a column for each value, for example ‘BldgType_1Fam’, with values 0,1 depending on whether the data point has that value.

For this purpose, pandas has the function `get_dummies` that will replace nominal variables with dummy variables.


```python
df = pd.get_dummies(df)
```

For example, look at the first 20 entries for BldgType_1Fam


```python
df.BldgType_1Fam[:20]
```




    0     1
    1     1
    2     1
    3     1
    4     1
    5     1
    6     1
    7     1
    8     1
    9     0
    10    1
    11    1
    12    1
    13    1
    14    1
    15    1
    16    1
    17    0
    18    1
    19    1
    Name: BldgType_1Fam, dtype: uint8
