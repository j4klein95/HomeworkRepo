

```python
#import dependencies/modules
import os
import csv
import pandas as pd
import numpy as np
import json
import statistics
from sklearn import preprocessing
```


```python
#find the total number of players
##set up your path to the players_complete csv
playerList = "./HeroesOfPymoli/players_complete.csv"
##set up your reader with pandas
playerList_df = pd.read_csv(playerList)
playerList_df.columns
playerCount = len(playerList_df.index)
playerCount_df = pd.DataFrame(
{"Total Players" : [playerCount]    
}
)
playerCount_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1163</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Time for purchasing analysis (Total); we need number of unique items, average price, number of purchases, total revenue
itemsList_location = "./HeroesOfPymoli/items_complete.csv"
purchaseData_location = "./HeroesOfPymoli/purchase_data_3.json"
itemList_df = pd.read_csv(itemsList_location)
purchaseData_df = pd.read_json(purchaseData_location)


numItems = len(itemList_df["Item Name"])
avePrice = itemList_df["Price"].mean()
numOfPurchases = len(purchaseData_df)
totalRevenue = purchaseData_df["Price"].sum()


purchasingAnalysis = pd.DataFrame(
    {"Number of Unique Items": [numItems],
    "Average Price": [avePrice],
    "Number of Purchases": [numOfPurchases],
    "Total Revenue": [totalRevenue]}
)


purchasingAnalysis["Average Price"] = purchasingAnalysis["Average Price"].map("${0:,.2f}".format)
purchasingAnalysis["Total Revenue"] = purchasingAnalysis["Total Revenue"].map("${0:,.2f}".format)
purchasingAnalysis

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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.98</td>
      <td>780</td>
      <td>190</td>
      <td>$2,365.17</td>
    </tr>
  </tbody>
</table>
</div>




```python

playerList_df['Gender'].value_counts()
player_counts = playerList_df['Gender'].value_counts()
percent_players = ((player_counts)/(playerCount))*100

genderPlayerList = pd.DataFrame(
    {"Percentage of Players": percent_players,
     "Number of Players": player_counts}
)
genderPlayerList["Percentage of Players"] = genderPlayerList["Percentage of Players"].map("{0:,.2f}%".format)
genderPlayerList
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
      <th>Number of Players</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>954</td>
      <td>82.03%</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>186</td>
      <td>15.99%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>23</td>
      <td>1.98%</td>
    </tr>
  </tbody>
</table>
</div>




```python
genderPurchaseData_df = purchaseData_df[['Gender', 'Price']]
genderPurchase_Counts = genderPurchaseData_df['Gender'].value_counts()
genderPurchase_Average = genderPurchaseData_df.groupby('Gender').Price.mean()
genderPurchase_Value = genderPurchaseData_df.groupby('Gender').Price.sum()

normalized_values = (genderPurchase_Value)/(genderPurchase_Counts)


#x = genderPurchaseData_df['Price'].values.astype(float)
#min_max_scaler = preprocessing.MinMaxScaler()
#x_scaled = min_max_scaler.fit_transform(x)
#normalized_df = pd.DataFrame(x_scaled)
#normalized_values = (genderPurchase_Average)/(normalized_df.mean())


#purchase analysis by gender: Purchase Count, Average Purchase Price, Total Purchase Value, Normalized Totals
genderPurchaseList_df = pd.DataFrame(
    {"Purchase Count": genderPurchase_Counts,
     "Average Purchase Price": genderPurchase_Average,
     "Total Purchase Value": genderPurchase_Value,
     "Normalized Totals": normalized_values,      
    }
)

genderPurchaseList_df["Average Purchase Price"] = genderPurchaseList_df["Average Purchase Price"].map("${0:,.2f}".format)
genderPurchaseList_df["Normalized Totals"] = genderPurchaseList_df["Normalized Totals"].map("${0:,.2f}".format)
genderPurchaseList_df["Total Purchase Value"] = genderPurchaseList_df["Total Purchase Value"].map("${0:,.2f}".format)
genderPurchaseList_df
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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$3.04</td>
      <td>$3.04</td>
      <td>130</td>
      <td>$395.80</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$3.03</td>
      <td>$3.03</td>
      <td>642</td>
      <td>$1,945.51</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$2.98</td>
      <td>$2.98</td>
      <td>8</td>
      <td>$23.86</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics: Percentage of Players, Total Count----use bins 

bins = [0, 10, 14, 19, 24, 29, 34, 39, 40]
age_bin= ['<10','11-14','15-19','20-24','25-29','30-34','35-39','40+']
pd.cut(playerList_df["Age"], bins, labels=age_bin)
playerList_df['Age Group'] = pd.cut(playerList_df['Age'], bins, labels=age_bin)
ageCount = playerList_df['Age Group'].value_counts()
percentAge = ((ageCount)/(playerCount)) * 100
ageDemo_df = pd.DataFrame(
{
    "Total Count": ageCount,
    "Percentage Of Players":percentAge
}
)

ageDemo_df["Percentage Of Players"] = ageDemo_df["Percentage Of Players"].map("{0:,.2f}%".format)

ageDemo_df

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
      <th>Percentage Of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20-24</th>
      <td>40.93%</td>
      <td>476</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>18.57%</td>
      <td>216</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>17.11%</td>
      <td>199</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.57%</td>
      <td>88</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.93%</td>
      <td>69</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>5.16%</td>
      <td>60</td>
    </tr>
    <tr>
      <th>11-14</th>
      <td>3.35%</td>
      <td>39</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.69%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase Count, Average Purchase Price, Total Purchase Value, Normalized Totals all based on age
bins = [0, 10, 14, 19, 24, 29, 34, 39, 40]
age_bin= ['<10','11-14','15-19','20-24','25-29','30-34','35-39','40+']
purchaseData_df['Age Group'] = pd.cut(purchaseData_df['Age'], bins, labels=age_bin)
ageGroupPurchase_df = purchaseData_df.groupby('Age Group')
purchase_Count = purchaseData_df['Age Group'].value_counts()
purchase_App = ageGroupPurchase_df['Price'].mean() 
purchase_Total = ageGroupPurchase_df['Price'].sum()
normalized_totals = (purchase_Total)/(purchase_Count)

financialAgeAnalysis_df = pd.DataFrame(
{
    "Purchase Count": purchase_Count,
    'Average Purchase Price': purchase_App,
    'Total Purchase Value': purchase_Total,
    'Normalized Totals': normalized_totals
}
)
financialAgeAnalysis_df["Average Purchase Price"] = financialAgeAnalysis_df["Average Purchase Price"].map("${0:,.2f}".format)
financialAgeAnalysis_df["Normalized Totals"] = financialAgeAnalysis_df["Normalized Totals"].map("${0:,.2f}".format)
financialAgeAnalysis_df["Total Purchase Value"] = financialAgeAnalysis_df["Total Purchase Value"].map("${0:,.2f}".format)
financialAgeAnalysis_df

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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11-14</th>
      <td>$2.98</td>
      <td>$2.98</td>
      <td>29</td>
      <td>$86.41</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$3.08</td>
      <td>$3.08</td>
      <td>132</td>
      <td>$406.62</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>$3.01</td>
      <td>$3.01</td>
      <td>316</td>
      <td>$950.59</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>$3.05</td>
      <td>$3.05</td>
      <td>137</td>
      <td>$417.21</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>$2.93</td>
      <td>$2.93</td>
      <td>62</td>
      <td>$181.40</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$3.18</td>
      <td>$3.18</td>
      <td>54</td>
      <td>$171.50</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>$2.79</td>
      <td>$2.79</td>
      <td>5</td>
      <td>$13.96</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>$2.92</td>
      <td>$2.92</td>
      <td>39</td>
      <td>$113.97</td>
    </tr>
  </tbody>
</table>
</div>




```python
#find top spenders, their average purchase price, and their total value
spenders_df = purchaseData_df.groupby('SN')
spenderCount = purchaseData_df["SN"].value_counts()
aPP = spenders_df['Price'].sum()/spenderCount
tPV = spenders_df['Price'].sum()

topSpender_df = pd.DataFrame(
{
    "Purchase Count": spenderCount,
    "Average Purchase Price": aPP,
    "Total Purchase Value": tPV,
}
)
topSpender_df

sorted_df = topSpender_df.sort_values('Total Purchase Value', ascending=False)
sorted_df["Average Purchase Price"] = sorted_df["Average Purchase Price"].map("${0:,.2f}".format)
sorted_df["Total Purchase Value"] = sorted_df["Total Purchase Value"].map("${0:,.2f}".format)
sorted_df.head(6)
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Chamalo71</th>
      <td>$3.36</td>
      <td>4</td>
      <td>$13.45</td>
    </tr>
    <tr>
      <th>Strithenu87</th>
      <td>$3.21</td>
      <td>4</td>
      <td>$12.83</td>
    </tr>
    <tr>
      <th>Mindosia50</th>
      <td>$4.00</td>
      <td>3</td>
      <td>$12.01</td>
    </tr>
    <tr>
      <th>Aeralria27</th>
      <td>$3.79</td>
      <td>3</td>
      <td>$11.38</td>
    </tr>
    <tr>
      <th>Eudai71</th>
      <td>$3.79</td>
      <td>3</td>
      <td>$11.37</td>
    </tr>
    <tr>
      <th>Rinallorap73</th>
      <td>$3.73</td>
      <td>3</td>
      <td>$11.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Find the most popular items, group by item id, item name; need Purchase Count, Item Price, Total Purchase Value
reduced_purchase_df = purchaseData_df.loc[:,['Item ID', 'Price', 'Item Name',]]

item_df = reduced_purchase_df.groupby(['Item Name', 'Item ID'])

item_df['Price'].max()

iC = item_df['Item Name'].count()

iP = item_df["Price"].max()

tpV = item_df["Price"].sum()


itemAsys_df = pd.DataFrame(
{
    "Purchase Count": iC,
    "Item Price": iP,
    "Total Purchase Value": tpV
}
)
itemAsys_df["Item Price"] = itemAsys_df["Item Price"].map("${0:,.2f}".format)
itemAsys_df["Total Purchase Value"] = itemAsys_df["Total Purchase Value"].map("${0:,.2f}".format)
itemAsys_df.sort_values('Purchase Count', ascending=False).head()
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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Hatred</th>
      <th>52</th>
      <td>$4.59</td>
      <td>11</td>
      <td>$50.49</td>
    </tr>
    <tr>
      <th>Misery's End</th>
      <th>111</th>
      <td>$4.90</td>
      <td>9</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>Primitive Blade</th>
      <th>174</th>
      <td>$1.39</td>
      <td>9</td>
      <td>$12.51</td>
    </tr>
    <tr>
      <th>Deluge, Edge of the West</th>
      <th>87</th>
      <td>$1.72</td>
      <td>8</td>
      <td>$13.76</td>
    </tr>
    <tr>
      <th>Brutality Ivory Warmace</th>
      <th>75</th>
      <td>$4.12</td>
      <td>8</td>
      <td>$32.96</td>
    </tr>
  </tbody>
</table>
</div>




```python
itemAsys_df.sort_values('Total Purchase Value', ascending=False).head()

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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Serenity</th>
      <th>13</th>
      <td>$4.98</td>
      <td>2</td>
      <td>$9.96</td>
    </tr>
    <tr>
      <th>Heartseeker, Reaver of Souls</th>
      <th>127</th>
      <td>$4.94</td>
      <td>2</td>
      <td>$9.88</td>
    </tr>
    <tr>
      <th>Shadowsteel</th>
      <th>170</th>
      <td>$1.93</td>
      <td>5</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>Deathraze</th>
      <th>150</th>
      <td>$4.82</td>
      <td>2</td>
      <td>$9.64</td>
    </tr>
    <tr>
      <th>Warped Fetish</th>
      <th>24</th>
      <td>$1.92</td>
      <td>5</td>
      <td>$9.60</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Observable Trends:

#Generally, gamers are young (~58% below the age of 24).
#This same age group spends the third most on average and are responsible for the highest total revenue received from in game purchases.
#On average, males and females are willing to spend about the same amount of money on in game purchases(Males willing to spend $3.03 on average while Females about $3.04). Men only account for higher total revenue due to their population size within the game.
```


```python

```
