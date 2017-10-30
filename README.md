
# Heroes of Pymoli analysis

1. Male players are considerably more than Female players (4:1) ratio.
2. Players tend to make more purchases when they are in their late teens to twenties. Especially the early twenties have high chance of buying items.
3. The number of purchases made for Popular items are not so different from number of purchases made for Profitable items. That might imply that Increasing the price of popular items, would help the profit.


```python
#Dependencies
import os
import pandas as pd

#inputfiles 
input_files = ['purchase_data.json','purchase_data2.json']
df_1 = pd.read_json('purchase_data.json')
df_2 = pd.read_json('purchase_data2.json')

#reading into pandas dataframe
df = pd.read_json('purchase_data.json')

#adding a purchase serial number to dataframe
df = df.reset_index()
df = df.rename(columns={"index":"Serial number"})
df.head()
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
      <th>Serial number</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



# Player count


```python
#Total number of players

number_of_players = len(df["SN"].unique())
pd.DataFrame({"Total players":[number_of_players]})
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
      <th>Total players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing analysis - Total


```python
#Purchasing analysis (Total) - Calculations

unique_items_count = len(df["Item ID"].unique())

item_price_df = df.loc[:,["Item ID","Item Name","Price"]]

item_price_df = item_price_df.drop_duplicates()

item_price_df.reset_index(drop=True)

average_purchasing_price = item_price_df["Price"].mean()

total_purchases=len(df)

total_purchases

total_revenue = df["Price"].sum()

#Purchasing analysis (Total) - Result Data Frame

purchasing_analysis_total = pd.DataFrame({"Number of Unique Items":[unique_items_count],
                                          "Average Purchase Price":[average_purchasing_price],
                                          "Total Number of Purchases":[total_purchases],
                                          "Total Revenue":[total_revenue]})

purchasing_analysis_total["Average Purchase Price"] = purchasing_analysis_total["Average Purchase Price"].map('${:,.2f}'.format)
purchasing_analysis_total["Total Revenue"] = purchasing_analysis_total["Total Revenue"].map('${:,.2f}'.format)

purchasing_analysis_total


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
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.95</td>
      <td>183</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



# Gender Demographics


```python
#Separating Players into a view

unique_users_df = df.loc[:,["Age","Gender","SN"]]
unique_users_df = unique_users_df.drop_duplicates()

#Gender Demographics

players_gender_df = pd.DataFrame(unique_users_df["Gender"].value_counts())
players_gender_df =players_gender_df.reset_index()

players_gender_df = players_gender_df.rename(columns={"index":"Gender","Gender":"Number of players"})

total_players = players_gender_df["Number of players"].sum()

players_gender_df["Percentage of players"] = (players_gender_df["Number of players"]/total_players)*100
players_gender_df["Percentage of players"] = players_gender_df["Percentage of players"].map('{:,.2f}%'.format)

players_gender_df
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
      <th>Gender</th>
      <th>Number of players</th>
      <th>Percentage of players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>465</td>
      <td>81.15%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>100</td>
      <td>17.45%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>8</td>
      <td>1.40%</td>
    </tr>
  </tbody>
</table>
</div>




```python
gender_group = df.groupby("Gender")

purchase_count_by_gender = pd.DataFrame(gender_group["Serial number"].count())

avg_purchase_price_by_gender = pd.DataFrame(gender_group["Price"].mean().round(2))

total_purchase_by_gender = pd.DataFrame(gender_group["Price"].sum())

# Normalized total - with median as norm
normalized_total_by_gender = pd.DataFrame(total_purchase_by_gender/df["Price"].median())

purchase_count_by_gender.reset_index(level='Gender',inplace=True)
avg_purchase_price_by_gender.reset_index(level='Gender',inplace=True)
total_purchase_by_gender.reset_index(level='Gender',inplace=True)
normalized_total_by_gender.reset_index(level='Gender',inplace=True)
```


```python
#Purchasing analysis (Gender) - Result Data Frame

purchase_analysis_gender = purchase_count_by_gender

result_items = [avg_purchase_price_by_gender,total_purchase_by_gender,normalized_total_by_gender]

for res_df in result_items:
    purchase_analysis_gender = purchase_analysis_gender.merge(res_df,on="Gender")

purchase_analysis_gender = purchase_analysis_gender.rename(columns={"Serial number":"Purchase count",
                                                                    "Price_x":"Average Purchase price",
                                                                    "Price_y":"Total purchase",
                                                                    "Price":"Normalized total"})
```

# Purchasing analysis - Gender


```python
# Formatting data

purchase_analysis_gender["Average Purchase price"] = purchase_analysis_gender["Average Purchase price"].map('${:,.2f}'.format)
purchase_analysis_gender["Total purchase"] = purchase_analysis_gender["Total purchase"].map('${:,.2f}'.format)
purchase_analysis_gender["Normalized total"] = purchase_analysis_gender["Normalized total"].map('${:,.2f}'.format)

purchase_analysis_gender
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
      <th>Gender</th>
      <th>Purchase count</th>
      <th>Average Purchase price</th>
      <th>Total purchase</th>
      <th>Normalized total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$132.95</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$648.50</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$12.41</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing analysis - Age


```python
#Purchasing analysis (Age)

age_bins = [0,10,15,20,25,30,35,40,45,50]

age_categories = ["under 10","10 - 14","15 - 19","20 - 24","25 - 29","30 - 34","35 - 39","40 - 44","45 and over"]

age_df = df

age_df["Age category"] = pd.cut(age_df['Age'], age_bins, labels=age_categories,right=False)
```


```python
age_category_group = age_df.groupby("Age category")

purchase_count_by_age = pd.DataFrame(age_category_group["Serial number"].count())

avg_purchase_price_by_age = pd.DataFrame(age_category_group["Price"].mean().round(2))

total_purchase_by_age = pd.DataFrame(age_category_group["Price"].sum())

# Normalized total - with median as norm
normalized_total_by_age = pd.DataFrame(total_purchase_by_age/df["Price"].median())

purchase_count_by_age.reset_index(level='Age category',inplace=True)
avg_purchase_price_by_age.reset_index(level='Age category',inplace=True)
total_purchase_by_age.reset_index(level='Age category',inplace=True)
normalized_total_by_age.reset_index(level='Age category',inplace=True)

```


```python
#Purchasing analysis (Age) - Result Data Frame

purchase_analysis_age = purchase_count_by_age

result_items = [avg_purchase_price_by_age,total_purchase_by_age,normalized_total_by_age]

for res_df in result_items:
    purchase_analysis_age = purchase_analysis_age.merge(res_df,on="Age category")

purchase_analysis_age = purchase_analysis_age.rename(columns={"Serial number":"Purchase count",
                                                                    "Price_x":"Average Purchase price",
                                                                    "Price_y":"Total purchase",
                                                                    "Price":"Normalized total"})
purchase_analysis_age = purchase_analysis_age.round(2)
```


```python
# Formatting data
purchase_analysis_age["Average Purchase price"] = purchase_analysis_age["Average Purchase price"].map('${:,.2f}'.format)
purchase_analysis_age["Total purchase"] = purchase_analysis_age["Total purchase"].map('${:,.2f}'.format)
purchase_analysis_age["Normalized total"] = purchase_analysis_age["Normalized total"].map('${:,.2f}'.format)

purchase_analysis_age
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
      <th>Age category</th>
      <th>Purchase count</th>
      <th>Average Purchase price</th>
      <th>Total purchase</th>
      <th>Normalized total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>under 10</td>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$28.98</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10 - 14</td>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$33.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15 - 19</td>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$134.17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20 - 24</td>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$339.85</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25 - 29</td>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$128.59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30 - 34</td>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$68.49</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35 - 39</td>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$41.46</td>
    </tr>
    <tr>
      <th>7</th>
      <td>40 - 44</td>
      <td>16</td>
      <td>$3.19</td>
      <td>$51.03</td>
      <td>$17.72</td>
    </tr>
    <tr>
      <th>8</th>
      <td>45 and over</td>
      <td>1</td>
      <td>$2.72</td>
      <td>$2.72</td>
      <td>$0.94</td>
    </tr>
  </tbody>
</table>
</div>



# Top Spenders


```python
# Top Spenders

groupby_player = df.groupby("SN")

spenders_df = pd.DataFrame(groupby_player["Price"].sum())
spenders_df["Purchase count"] = groupby_player["Price"].count()
spenders_df["Average purchase price"] = (groupby_player["Price"].sum()/groupby_player["Price"].count()).round(2)

spenders_df.reset_index(level='SN',inplace=True)

top_spenders = spenders_df.nlargest(5,"Price").reset_index(drop=True)

top_spenders = top_spenders.rename(columns={"SN":"Player name","Price":"Total purchase value"})

top_spenders["Average purchase price"] = top_spenders["Average purchase price"].map("${:.2f}".format)
top_spenders["Total purchase value"] = top_spenders["Total purchase value"].map("${:.2f}".format)

top_spenders
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
      <th>Player name</th>
      <th>Total purchase value</th>
      <th>Purchase count</th>
      <th>Average purchase price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>$17.06</td>
      <td>5</td>
      <td>$3.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saedue76</td>
      <td>$13.56</td>
      <td>4</td>
      <td>$3.39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mindimnya67</td>
      <td>$12.74</td>
      <td>4</td>
      <td>$3.18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Haellysu29</td>
      <td>$12.73</td>
      <td>3</td>
      <td>$4.24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eoda93</td>
      <td>$11.58</td>
      <td>3</td>
      <td>$3.86</td>
    </tr>
  </tbody>
</table>
</div>



# Popular Items


```python
# Popular Items 
groupby_item = df.groupby("Item ID")

items_purchase_count_df = pd.DataFrame(groupby_item["Serial number"].count())

items_purchase_count_df.reset_index(level='Item ID',inplace=True)
items_purchase_count_df = items_purchase_count_df.rename(columns={"Serial number":"Number of purchases"})


items_purchase_count_df = items_purchase_count_df.merge(item_price_df,on="Item ID")
items_purchase_count_df["Total purchase value"] = items_purchase_count_df["Price"]*items_purchase_count_df["Number of purchases"]

```


```python
popular_items = items_purchase_count_df.nlargest(5,"Number of purchases").reset_index(drop=True)

column_order = ['Item ID', 'Item Name', 'Price', 'Number of purchases',
       'Total purchase value']

popular_items = popular_items[column_order]
popular_items["Price"] = popular_items["Price"].map("${:.2f}".format)
popular_items["Total purchase value"] = popular_items["Total purchase value"].map("${:.2f}".format)
popular_items
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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>Number of purchases</th>
      <th>Total purchase value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>2</th>
      <td>13</td>
      <td>Serenity</td>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>31</td>
      <td>Trickster</td>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>4</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>



# Profitable Items


```python
profitable_items = items_purchase_count_df.nlargest(5,"Total purchase value").reset_index(drop=True)

column_order = ['Item ID', 'Item Name', 'Price', 'Number of purchases',
       'Total purchase value']

profitable_items = profitable_items[column_order]

profitable_items["Price"] = profitable_items["Price"].map("${:.2f}".format)
profitable_items["Total purchase value"] = profitable_items["Total purchase value"].map("${:.2f}".format)

profitable_items
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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>Number of purchases</th>
      <th>Total purchase value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


