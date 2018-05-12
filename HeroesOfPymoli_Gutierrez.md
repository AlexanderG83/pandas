
# House of Pymoli Data Analysis

Of the 573 active players analyzed, the majority are male (81%), with females and other/undisclosed consisting of roughly 17.5% and 1.5% of the population, respectively.

The average age of Pymoli players is between 20 and 24 years old, with the second and third closest age demographics falling in the 15 to 19 and 25 to 29 brackets, respectively.

Although the most revenue falls within the largest population brackets, older players tend to spend more per player, with 40-44 year olds spending the most per purchase at $3.19, and 30-34 year olds spending the second most at $3.08.



```python
import pandas as pd
import numpy as np
```


```python
purchase_data = pd.read_json("purchase_data.json", orient = 'columns')
purchase_df = purchase_data
purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
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
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
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



### Player Count


```python
###Obtains list of unique players###

total_players = len(purchase_df['SN'].unique())
players = pd.DataFrame(
    {"Total Players": [total_players]})

players
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Total)


```python
###Purchasing Analysis (Total)###

#Number of Unique Items
#Average Purchase Price
#Total Number of Purchases
#Total Revenue

numUniques = len(purchase_df['Item ID'].unique())
avgPurchase = round(purchase_df['Price'].mean(),2)
numPurchase = purchase_df['Price'].count()
totRevenue = purchase_df['Price'].sum()

Purchasing_Analysis = pd.DataFrame(
    {
        "Number of Uniques": [numUniques],
        "Average Price": [avgPurchase],
        "Total Purchases": [numPurchase],
        "Total Revenue": [totRevenue]
    })

Purchasing_Analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Price</th>
      <th>Number of Uniques</th>
      <th>Total Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.93</td>
      <td>183</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>



### Gender Demographics


```python
###Gender Demographics###

#Percentage and Count of Male Players
#Percentage and Count of Female Players
#Percentage and Count of Other / Non-Disclosed

males = purchase_df.loc[purchase_df['Gender'] == 'Male', ['SN']]
malesUnique = len(males.groupby(['SN']))

females = purchase_df.loc[purchase_df['Gender'] == 'Female', ['SN']]
femalesUnique = len(females.groupby(['SN']))

other = purchase_df.loc[purchase_df['Gender'] == 'Other / Non-Disclosed',['SN']]
otherUnique = len(other.groupby(['SN']))

Genders = pd.DataFrame(
    {"Percentage of Players": [round((malesUnique/total_players)*100,2),
                   round((femalesUnique/total_players)*100,2),
                   round((otherUnique/total_players)*100,2)],
     "Gender": ['Male','Female','Other / Non-Disclosed'],
    "Count of Gender": [malesUnique, femalesUnique, otherUnique]
    }
)

Genders_Reordered = Genders.reindex (columns=['Gender','Percentage of Players','Count of Gender'])

Genders_Reordered
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Percentage of Players</th>
      <th>Count of Gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Gender)


```python
###Purchasing Analysis (Gender)###

#Broken out by gender:
#Purchase Count
#Average Purchase Price
#Total Purchase Value
#Normalized Totals

malePurchases = purchase_df.loc[purchase_df['Gender'] == 'Male', ['Price']]
malePurchaseCount = malePurchases.count()
malePurchaseSum = round(malePurchases.sum(),2)
malePurchaseAvg = round(malePurchases.mean(),2)
maleNormalized = round((malePurchaseSum/malesUnique),2)

malePurchaseCount_VALUES = malePurchaseCount.values
malePurchaseSum_VALUES = malePurchaseSum.values
malePurchaseAvg_VALUES = malePurchaseAvg.values
maleNormalized_VALUES = maleNormalized.values

femalePurchases = purchase_df.loc[purchase_df['Gender'] == 'Female', ['Price']]
femalePurchaseCount = femalePurchases.count()
femalePurchaseSum = round(femalePurchases.sum(),2)
femalePurchaseAvg = round(femalePurchases.mean(),2)
femaleNormalized = round((femalePurchaseSum/femalesUnique),2)

femalePurchaseCount_VALUES = femalePurchaseCount.values
femalePurchaseSum_VALUES = femalePurchaseSum.values
femalePurchaseAvg_VALUES = femalePurchaseAvg.values
femaleNormalized_VALUES = femaleNormalized.values

otherPurchases = purchase_df.loc[purchase_df['Gender'] == 'Other / Non-Disclosed', ['Price']]
otherPurchaseCount = otherPurchases.count()
otherPurchaseSum = round(otherPurchases.sum(),2)
otherPurchaseAvg = round(otherPurchases.mean(),2)
otherNormalized = round((otherPurchaseSum/otherUnique),2)

otherPurchaseCount_VALUES = otherPurchaseCount.values
otherPurchaseSum_VALUES = otherPurchaseSum.values
otherPurchaseAvg_VALUES = otherPurchaseAvg.values
otherNormalized_VALUES = otherNormalized.values

Gender_Purchasing = pd.DataFrame(
    {"Gender": ['Male','Female','Other / Non-Disclosed'],
    "Purchase Count": [malePurchaseCount_VALUES, femalePurchaseCount_VALUES, otherPurchaseCount_VALUES],
    "Average Purchase Price": [malePurchaseAvg_VALUES, femalePurchaseAvg_VALUES, otherPurchaseAvg_VALUES],
    "Total Purchase Value": [malePurchaseSum_VALUES, femalePurchaseSum_VALUES, otherPurchaseSum_VALUES],
     "Normalized Totals": [maleNormalized_VALUES, femaleNormalized_VALUES, otherNormalized_VALUES]
    }
)

Gender_PurchasingReordered = Gender_Purchasing.reindex (columns=['Gender','Purchase Count',
                                                                'Average Purchase Price',
                                                                'Total Purchase Value',
                                                                'Normalized Totals'])

Gender_PurchasingReordered
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>[633]</td>
      <td>[2.95]</td>
      <td>[1867.68]</td>
      <td>[4.02]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>[136]</td>
      <td>[2.82]</td>
      <td>[382.91]</td>
      <td>[3.83]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>[11]</td>
      <td>[3.25]</td>
      <td>[35.74]</td>
      <td>[4.47]</td>
    </tr>
  </tbody>
</table>
</div>



### Age Demographics


```python
###Age Demographics###

#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
#Purchase Count
#Average Purchase Price
#Total Purchase Value
#Normalized Totals


bins = [0, 9, 14, 19, 24, 29, 34, 39, 44, 49, 54]
group_labels = [">10",
                "10 to 14",
                "15 to 19",
                "20 to 24",
                "25 to 29",
                "30 to 34",
                "35 to 39",
                "40 to 44",
                "45 to 49",
                "50 to 54"]

age_series = pd.cut(purchase_df['Age'], bins, labels = group_labels)

ageDataFrame = pd.DataFrame(purchase_df)


ageDataFrame['Age Group'] = age_series


age_Sum = purchase_df.groupby('Age Group')['Price'].sum()
age_Count = purchase_df.groupby('Age Group')['Price'].count()
age_Avg = round(purchase_df.groupby('Age Group')['Price'].mean(),2)
age_Normalized = round((age_Sum/age_Count),2)



age_groups = ageDataFrame.groupby('Age Group')

age_analysis = pd.DataFrame(
    {
        "Purchase Count": age_Count,
        "Average Cost": age_Avg,
        "Total Value": age_Sum,
        "Normalized Value": age_Normalized
    })

age_analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Cost</th>
      <th>Normalized Value</th>
      <th>Purchase Count</th>
      <th>Total Value</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&gt;10</th>
      <td>2.98</td>
      <td>2.98</td>
      <td>28</td>
      <td>83.46</td>
    </tr>
    <tr>
      <th>10 to 14</th>
      <td>2.77</td>
      <td>2.77</td>
      <td>35</td>
      <td>96.95</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>2.91</td>
      <td>2.91</td>
      <td>133</td>
      <td>386.42</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>2.91</td>
      <td>2.91</td>
      <td>336</td>
      <td>978.77</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>2.96</td>
      <td>2.96</td>
      <td>125</td>
      <td>370.33</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>3.08</td>
      <td>3.08</td>
      <td>64</td>
      <td>197.25</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>2.84</td>
      <td>2.84</td>
      <td>42</td>
      <td>119.40</td>
    </tr>
    <tr>
      <th>40 to 44</th>
      <td>3.19</td>
      <td>3.19</td>
      <td>16</td>
      <td>51.03</td>
    </tr>
    <tr>
      <th>45 to 49</th>
      <td>2.72</td>
      <td>2.72</td>
      <td>1</td>
      <td>2.72</td>
    </tr>
    <tr>
      <th>50 to 54</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>



### Top Spenders


```python
#Top Spenders

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#SN
#Purchase Count
#Average Purchase Price
#Total Purchase Value

players_Sum = purchase_df.groupby('SN')['Price'].sum()
players_Count = purchase_df.groupby('SN')['Price'].count()
players_Avg = round(purchase_df.groupby('SN')['Price'].mean(),2)

player_analysis = pd.DataFrame(
    {
        "Total Count": players_Count,
        "Average Spend": players_Avg,
        "Total Spend": players_Sum
    })


player_analysis_sorted = player_analysis.sort_values("Total Spend", ascending = False)

player_analysis_sorted_top5 = player_analysis_sorted.iloc[0:5]

player_analysis_sorted_top5

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Spend</th>
      <th>Total Count</th>
      <th>Total Spend</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>3.41</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.39</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.18</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4.24</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3.86</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



### Most Popular Items


```python
#Most Popular Items

#Identify the 5 most popular items by total purchase count, then list (in a table):
#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

popular_Sum = purchase_df.groupby('Item ID')['Price'].sum()
popular_Count = purchase_df.groupby('Item ID')['Price'].count()
popular_Avg = purchase_df.groupby('Item ID')['Price'].mean()
popular_Name = purchase_df.groupby('Item ID')['Item Name']

#popular_Sum = purchase_df['Price'].sum()
#popular_Count = purchase_df['Price'].count()
#popular_Avg = purchase_df['Price'].mean()
#popular_Name = purchase_df['Item Name']

popular_analysis = pd.DataFrame(
    {
        "Purchase Count": popular_Count,
        "Average Cost": popular_Avg,
        "Total Value": popular_Sum,
        "Item Name": popular_Name
    })


popular_analysis_sorted = popular_analysis.sort_values("Purchase Count", ascending = False)

popular_analysis_sorted_top5 = popular_analysis_sorted.iloc[0:5]

popular_analysis_sorted_top5_reordered = popular_analysis_sorted_top5.reindex(columns = ['Item Name','Purchase Count','Average Cost','Total Value'])

popular_analysis_sorted_top5_reordered

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Cost</th>
      <th>Total Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>(39, [Betrayal, Whisper of Grieving Widows, Be...</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>(84, [Arcane Gem, Arcane Gem, Arcane Gem, Arca...</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <td>(31, [Trickster, Trickster, Trickster, Trickst...</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <td>(175, [Woeful Adamantite Claymore, Woeful Adam...</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>(13, [Serenity, Serenity, Serenity, Serenity, ...</td>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



### Most Profitable Items


```python
#Most Profitable Items

#Identify the 5 most profitable items by total value, then list (in a table):
#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

item_Sum = purchase_df.groupby('Item ID')['Price'].sum()
item_Count = purchase_df.groupby('Item ID')['Price'].count()
item_Avg = purchase_df.groupby('Item ID')['Price'].mean()
item_Name = purchase_df.groupby('Item ID')['Item Name']

item_analysis = pd.DataFrame(
    {
        "Purchase Count": item_Count,
        "Average Cost": item_Avg,
        "Total Value": item_Sum,
        "Item Name": item_Name
    })


item_analysis_sorted = item_analysis.sort_values("Total Value", ascending = False)

item_analysis_sorted_top5 = item_analysis_sorted.iloc[0:5]

item_analysis_sorted_top5_reordered = item_analysis_sorted_top5.reindex(columns = ['Item Name','Purchase Count','Average Cost','Total Value'])

item_analysis_sorted_top5_reordered


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Cost</th>
      <th>Total Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>(34, [Retribution Axe, Retribution Axe, Retrib...</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>(115, [Spectral Diamond Doomblade, Spectral Di...</td>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>(32, [Orenmir, Orenmir, Orenmir, Orenmir, Oren...</td>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>(103, [Singed Scalpel, Singed Scalpel, Singed ...</td>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>(107, [Splitter, Foe Of Subtlety, Splitter, Fo...</td>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


