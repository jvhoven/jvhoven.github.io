---
layout: post
title:  Recreational Datascience
date:   2017-12-04 13:09:10 +0100
categories: assignments
tag: Python
visible: True
---

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
from datetime import time, tzinfo, timedelta
warnings.filterwarnings('ignore') # to suppress a numpy warning
%matplotlib inline
```
```python
purchases = pd.read_excel("campuscard.xlsx")
climate = pd.read_excel("climate.xlsx")
```

Hypothesis:
* The warmer the day, the more beverages consumed
* The colder the day, the less beverages consumed

Prediction model:
* Given a temperature, would mister J.D consume more hot beverages or cold beverages?

Interesting things to figure out:
* Total expenditure per day, total quantity per day
* Correlation between weather and purchases (hot/cold beverages?)
* Train a linear regression model to predict expenditure for a day given an average temperature
* Distinction between hot and cold beverages

First we will need to clean the data.
- [x] Remove EURO sign.
- [x] Split timestamp into date and time for the purchases dataset.
- [x] Clean timestamp to YYYY-MM-DD in climate dataset.
* ~~Possibly filter out printouts, as we're only interested in consumptions.~~
- [x] Categorize consumptions.

```python
# Split timestamp into date and time for the purchases dataset.
purchases['Date'] = [d.date() for d in purchases['Timestamp']]
purchases['Time'] = [d.time() for d in purchases['Timestamp']]

# Filter out EURO sign to be able to calculate with the prices.
purchases['Gift'] = purchases['Gift'].str.replace('€', '')
purchases['TotalAmount'] = purchases['TotalAmount'].str.replace('€', '')
purchases['Amount'] = purchases['Amount'].str.replace('€', '')

# Set the datatype to floats.
purchases['TotalAmount'] = purchases['TotalAmount'].astype(float)
purchases['Gift'] = purchases['Gift'].astype(float)
purchases['Amount'] = purchases['Amount'].astype(float)

# Set the correct datatypes for Date and Time
purchases['Date'] = pd.to_datetime(purchases['Date'])

# We can drop timestamp
#purchases.drop(['Timestamp'], axis = 1, inplace = True)
purchases['Timestamp'] = pd.to_datetime(purchases['Timestamp'])  
```
```python
# Clean timestamp to YYYY-MM-DD in climate dataset.
climate['Date'] = climate['Date'].apply(lambda x: pd.to_datetime(str(x), format='%Y%m%d'))

# Drop the station number, we don't need it for analysis.
climate.drop(['Station'], axis = 1, inplace = True)
```
```python
# We want to convert our values to categorical types in order to easily check relations for our hypothesis.
types = {
    "Copy": "Copy",
    "Print": "Printout",
    "Cacao": "Hot Beverage",
    "Cappuccino": "Hot Beverage",
    "Chocolatemilk": "Hot Beverage",
    "Coffee": "Hot Beverage",
    "Cold Water": "Cold Beverage",
    "Decaf": "Hot Beverage",
    "Espreschoc": "Hot Beverage",
    "Espresso": "Hot Beverage",
    "Hot Water": "Hot Beverage",
    "Tea": "Hot Beverage"
}

def aggregate_types(product):
    for key, value in types.items():
        if product.find(key) != -1:
            return value
        
purchases['ProductType'] = purchases.Product.map(aggregate_types)
```
```python
# Extract consumptions per day
# TODO: Remove outliers
consumptions_per_day = purchases.groupby(purchases['Date'], as_index = False)['Gift', 'Quantity'].sum()
consumptions_per_day.plot.scatter(x='Gift', y='Quantity');
```
<img src="/public/images/recreational/output_6_0.png" class="diagram" />
```python
# Aggregate total consumptions per product
consumptions_per_product = purchases.groupby(by = ['Product', 'ProductType'], as_index = False)['Quantity', 'Gift'].sum()
consumptions_per_product.plot.barh(x='Product', y='Quantity', figsize=(25,20))
```
<img src="//public/images/recreational/output_7_1.png" class="diagram" />
```python
# Percentage of expenditure per product type
consumptions_per_product['GiftPercentage'] = consumptions_per_product['Gift'] / consumptions_per_product['Gift'].sum()
pie_data = consumptions_per_product.groupby(by = ['ProductType'])['GiftPercentage'].sum()
pie_data.plot.pie(x='ProductType', y='GiftPercentage', figsize=(10,10))

fig, axes = plt.subplots(0, figsize=(10,3))

for ax, idx in zip(axes, pie_data.index):
    ax.pie(df.loc[idx], labels=df.columns, autopct='%.2f')
    ax.set(ylabel='', title=idx, aspect='equal')
```
<img src="/public/images/recreational/output_8_0.png" class="diagram" />

It seems that we can conclude the hypothesis is nonsense as there is close to no consumation of cold beverages. Some other things we can look at are:

* Stressful periods, is this related to the amount of prints (exam weeks?) or the intake of coffee?
* What was the period where the teacher consumed decaff coffee?
* In which part of the day does the teacher consume the most?
* Can we recognize a pattern?

```python
# In order to be able to view a pattern we will be categorizing consumption per morning, afternoon and evening.
def aggregate_part_of_day(_time):
    if(_time < time(12, 00) and _time > time(00, 00)):
        return "Morning"
    elif(_time > time(12, 00) and _time < time(18, 00)):
        return "Afternoon"
    else:
        return "Evening"
            
purchases['DayPart'] = purchases.Time.map(aggregate_part_of_day)
```
```python
purchases.groupby(by = ['DayPart', 'ProductType']).sum()
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Quantity</th>
      <th>Amount</th>
      <th>Gift</th>
      <th>TotalAmount</th>
      <th>Unknown</th>
    </tr>
    <tr>
      <th>DayPart</th>
      <th>ProductType</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Afternoon</th>
      <th>Cold Beverage</th>
      <td>48</td>
      <td>-10.08</td>
      <td>10.08</td>
      <td>0.0</td>
      <td>26342585760</td>
    </tr>
    <tr>
      <th>Copy</th>
      <td>1</td>
      <td>-0.80</td>
      <td>0.80</td>
      <td>0.0</td>
      <td>548803870</td>
    </tr>
    <tr>
      <th>Hot Beverage</th>
      <td>207</td>
      <td>-78.84</td>
      <td>78.84</td>
      <td>0.0</td>
      <td>113602401090</td>
    </tr>
    <tr>
      <th>Printout</th>
      <td>902</td>
      <td>-304.92</td>
      <td>304.92</td>
      <td>0.0</td>
      <td>377028258690</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Evening</th>
      <th>Cold Beverage</th>
      <td>1</td>
      <td>-0.21</td>
      <td>0.21</td>
      <td>0.0</td>
      <td>548803870</td>
    </tr>
    <tr>
      <th>Copy</th>
      <td>54</td>
      <td>-2.16</td>
      <td>2.16</td>
      <td>0.0</td>
      <td>26342585760</td>
    </tr>
    <tr>
      <th>Hot Beverage</th>
      <td>34</td>
      <td>-18.58</td>
      <td>18.58</td>
      <td>0.0</td>
      <td>18659331580</td>
    </tr>
    <tr>
      <th>Printout</th>
      <td>53</td>
      <td>-21.20</td>
      <td>21.20</td>
      <td>0.0</td>
      <td>20854547060</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Morning</th>
      <th>Cold Beverage</th>
      <td>13</td>
      <td>-2.73</td>
      <td>2.73</td>
      <td>0.0</td>
      <td>7134450310</td>
    </tr>
    <tr>
      <th>Copy</th>
      <td>24</td>
      <td>-5.52</td>
      <td>5.52</td>
      <td>0.0</td>
      <td>7134450310</td>
    </tr>
    <tr>
      <th>Hot Beverage</th>
      <td>556</td>
      <td>-305.79</td>
      <td>305.79</td>
      <td>0.0</td>
      <td>305134951720</td>
    </tr>
    <tr>
      <th>Printout</th>
      <td>394</td>
      <td>-84.52</td>
      <td>84.52</td>
      <td>0.0</td>
      <td>144884221680</td>
    </tr>
  </tbody>
</table>
</div>
