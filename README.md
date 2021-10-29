# pandas-challenge
Like many others in its genre, _Heroes of Pymoli_ is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. This project generates a report that breaks down the game's purchasing data into meaningful insights.
## Player Count
Display the overall player numbers.
```python
# import data
purchase_df = pd.read_csv(file)

# count the number of distinct "SN" number
Total = len(purchase_df["SN"].unique())
total_df = pd.DataFrame({"Total Players" : [Total]})
total_df
```
| Total Players |
| :---: |
| 576  | 

## Purchasing Analysis (Total)
  * Number of Unique Items
  * Average Purchase Price
  * Total Number of Purchases
  * Total Revenue
  
```python
# count number of "Item ID"
item_count = len(purchase_df["Item ID"].unique())

# Average Purchase Price
avg_price = sum(purchase_df["Price"])/Total

# Total Revenue
total_revenue = sum(purchase_df["Price"])

# Total Number of Purchases
total_purchase = purchase_df["Purchase ID"].count()

# create summary data frame
purhchase_analysis = pd.DataFrame({ "Number of Unique Items" : [item_count],
                                   "Average Purchase Price" : [avg_price],
                                   "Total Number of Purchases" : [total_purchase],
                                   "Total Revenue" : [total_revenue] })

# formatting the column value
purhchase_analysis["Average Purchase Price"] = purhchase_analysis["Average Purchase Price"].map("${:.2f}".format)
purhchase_analysis["Total Revenue"] = purhchase_analysis["Total Revenue"].map("${:.2f}".format)
purhchase_analysis
```

| Number of Unique Items  |Average Purchase Price |Total Number of Purchases|Total Revenue|
| :---:|:---: | :---: | :---:|
| 179  |  	$4.13 |  780 | $2379.77 |

## Gender Demographics
  * Percentage and Count of Male Players
  * Percentage and Count of Female Players
  * Percentage and Count of Other / Non-Disclosed

```python
# new data frame grouped by gender identity
gender = purchase_df.groupby('Gender')

# percentage of male/female players
gender_count  = purchase_df.groupby('Gender').size()
gender_percent = gender_count / total_purchase *100

# create summary data frame
gender_df = pd.DataFrame({"Total Counts" : gender_count,
                          "Percentage of Players" : gender_percent
                         })

# formatting data
gender_df["Percentage of Players"]=gender_df["Percentage of Players"].map("%{:.2f}".format)
gender_df
```
|Gender |Total Counts  |Percentage of Players |
| :---:|:---: | :---: | 
| **Female** |  	113|  %14.49 |
| **Male** |  	652 | %83.59 | 
| **Other / Non-Disclosed** | 15| %1.92|  

## Purchase Analysis (Gender)
  * The below each broken by gender
    * Purchase Count
    * Average Purchase Price
    * Total Purchase Value
    * Average Purchase Total per Person by Gender

```python
# Average Purchase Price
mean = gender["Price"].mean()

# Total Purchase Value
price_sum = gender["Price"].sum()

# Average Purchase Total per Person by Gender
gender_sum = purchase_df.groupby('Gender')['SN'].nunique()
totalAvg = price_sum /gender_sum

# create summary data frame
genderPurchae_df = pd.DataFrame({"Purchase Count" : gender_count,
                                 "Average Purchase Price" : mean,
                                 "Total Purchase Value" : price_sum,
                                 "Avg Total Purchase per Person" : totalAvg
                         })

# formatting data
genderPurchae_df["Average Purchase Price"] = genderPurchae_df["Average Purchase Price"].map("${:.2f}".format)
genderPurchae_df["Total Purchase Value"] = genderPurchae_df["Total Purchase Value"] .map("${:.2f}".format)
genderPurchae_df["Avg Total Purchase per Person"] = genderPurchae_df["Avg Total Purchase per Person"] .map("${:.2f}".format)

genderPurchae_df
```
|  Gender                   |Purchase Count |Average Purchase Price |Total Purchase Value|Avg Total Purchase per Person|
| :---:                     |:------------: | :-------------------: |  :----------------:| :--------------------------:|
| **Female**                |  	113         |  $3.20                |          $361.94   |        $4.47                |
| **Male**                  |  	652         |  $3.02                |           $1967.64 |        $4.07                |
| **Other / Non-Disclosed** |  	15          |  $3.35                |       $50.19       |        $4.56                |

## Age Demographics
  * Categorize the existing players using the age bins
  * Calculate the numbers and percentages by age group
  * Display summary table

```python
# create different age range and name for each range
bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 1000]
category = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

purchase_df["Age Range"] = pd.cut(purchase_df["Age"], bins, labels = category)

# Calculate the numbers by age group
count = purchase_df.groupby('Age Range').size()
# Calculate the  percentages by age group
age_percent = (count/Total)*100

# create summary data frame
age_summary = pd.DataFrame({"Total Count": count,
                            "Percentage of Players" : age_percent})

# formatting data
age_summary["Percentage of Players"]=age_summary["Percentage of Players"].map("%{:.2f}".format)
age_summary
```
|Age Range      |     Total Count       |Percentage of Players|
|:------------: | :-------------------: |  :----------------:|
|<10    |23|%3.99|
|10-14 	|28|%4.86|
|15-19 	|136|%23.61|
|20-24 	|365|%63.37|
|25-29  |101|%17.53|
|30-34 	|73|%12.67|
|35-39 	|41|%7.12|
|40+ 	  |13|%2.26|

## Purchasing Analysis (age)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age

```python
age = purchase_df.groupby('Age Range')
# Average Purchase Price
age_Avg = age["Price"].mean()
# Total Purchase Value
age_Sum = age["Price"].sum()
# Average Purchase Total per Person by Age
ageRange = purchase_df.groupby('Age Range')['SN'].nunique()
age_TotalAvg = (age_Sum)/(ageRange)

# create summary data frame
AgePurchae_df = pd.DataFrame({"Purchase Count" : count,
                              "Average Purchase Price" : age_Avg,
                              "Total Purchase Value" : age_Sum,
                              "Avg Total Purchase per Person" : age_TotalAvg
                         })

# formatting data
AgePurchae_df["Average Purchase Price"] = AgePurchae_df["Average Purchase Price"].map("${:.2f}".format)
AgePurchae_df["Total Purchase Value"] = AgePurchae_df["Total Purchase Value"] .map("${:.2f}".format)
AgePurchae_df["Avg Total Purchase per Person"] = AgePurchae_df["Avg Total Purchase per Person"] .map("${:.2f}".format)

AgePurchae_df
```

|Age Range      |   Purchase Count  |Average Purchase Price|Total Purchase Value |Avg Total Purchase per Person|
|:------------: | :-------------------: |  :----------------:| :----------------:| :----------------:          |
|<10            |23                     |$3.35 	             |$77.13             | 	$4.54  |
|10-14 	        |28                     |$2.96 	             |$82.78             | 	$3.76  |
|15-19 	        |136                    |$3.04 	             |$412.89            |  $3.86|
|20-24 	        |365                    |$3.05               |$1114.06           |	$4.32|
|25-29          |101                    |$2.90 	             |$293.00            |	$3.81|
|30-34 	        |73                     |$2.93             	 |$214.00            |	$4.12|
|35-39         	|41                     |$3.60 	             |$147.67            |	$4.76|
|40+ 	          |13                     |$2.94 	             |$38.24             |	$3.19|

## Top Spenders
  * Group the data by "SN" column
  * Sort the data by "Total Purchase Value"
  * Print out buyers ID 
  
```python
sn_df = purchase_df.groupby('SN')
# Purchase Count
sn_count  = purchase_df.groupby('SN').size()
# Average Purchase Price
sn_priceAvg = sn_df["Price"].mean()
# Total Purchase Value
sn_PurchaseSum = sn_df["Price"].sum()
sn_PurchaseSum = sn_PurchaseSum.sort_values(ascending=False)

# create summary data frame
spender = pd.DataFrame({"Average Purchase Price" : sn_priceAvg,
                        "Purchase Count" : sn_count,
                        "Total Purchase Value" : sn_PurchaseSum
                         })

# sort table by "Total Purchase Value", decending order and show the first 5 rows
spender = spender.sort_values("Total Purchase Value", ascending=False)

spender["Average Purchase Price"] = spender["Average Purchase Price"].map("${:.2f}".format)
spender["Total Purchase Value"] = spender["Total Purchase Value"].map("${:.2f}".format)


spender.head()
```
|SN             | Average Purchase Price|Purchase Count|Total Purchase Value|
|:------------: |:------------:         |:------------:|:------------:      |
|Lisosia93 	 |$3.79 | 5 |	$18.96|
|Idastidru52 |$3.86 | 4 |	$15.45|
|Chamjask73  |$4.61 |	3 |	$13.83|
|Iral74 	   |$3.40 | 4 |	$13.62|
|Iskadarya95 |$4.37 |	3 |	$13.10|

## Most Popular Items¶
  * Group the data by "Item ID"
  * Sort data by "Purchase Count"
  * Find the top 5 item with the most "Purchase Count"

```python
item = purchase_df[["Item ID","Item Name","Price"]]
item_pop = item.groupby(['Item ID','Item Name'])
item_count = item_pop["Price"].count()
item_PurchaseSum = item_pop["Price"].sum()
item_price = item_PurchaseSum/item_count

# create summary data frame
most_popular = pd.DataFrame({"Purchase Count" : item_count,
                             "Item Price" : item_price,
                             "Total Purchase Value" : item_PurchaseSum
                         })

# formatting data
most_popular["Item Price"] = most_popular["Item Price"].map("${:.2f}".format)
most_popular["Total Purchase Value"] = most_popular["Total Purchase Value"].map("${:.2f}".format)

# sort table by "Purchase Count", decending order and show the first 5 rows
most_popular = most_popular.sort_values("Purchase Count" , ascending=False)
most_popular.head()
```

|Item ID 	      |   Item Name 	| 	Purchase Count|     Item Price|Total Purchase Value|
|:------------: |:------------: |:------------:   |:------------: |     :------------: |
|92 	|Final Critic 	                              |13 	|$4.61 |	$59.99|
|178 	|Oathbreaker, Last Hope of the Breaking Storm |12 	|$4.23 |	$50.76|
|145 	|Fiery Glass Crusader 	                      |9  	|$4.58 |	$41.22|
|132 	|Persuasion 	                                |9 	  |$3.22 |	$28.99|
|108 	|Extraction, Quickblade Of Trembling Hands 	  |9	  |$3.53 |	$31.77|


# Conclusion
* The majority numbers of players are _male_
* Despite the significant difference between the numbers of male and female players,  the average money spent doesn't significantly differ between male and female players. 
* The majority numbers of players' ages fall between 20 and 24. Those players spent more money than players in other age range on average.
