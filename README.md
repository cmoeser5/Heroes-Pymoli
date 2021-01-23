# Heroes of Pymoli
## Overview

Hereos of Pymoli is a free, online fantasy game where players are encouraged to buy additional items to enhance their gaming experience. In order to better understand how players are utilizing in-game purchase options, recent purchasing data has been broken down into a concise report.

## Observable Trends and Insights

* Players between the ages of 20-24 make up almost half of the total player count, at 44.5%.
* This same age group accounts for close to half of the total purchase count with a total purchase value of $1,114.06. 
* Gender demographics show that of the unique players a large majority of the players are male, at 84% of the population. Meaning 484 of the 576 players, identify as male.

#### Using the dataset in the "Resources" folder we have calulated the following information about the players of Heroes of Pymoli:

#### Player Count
* Total players was caluculated using .drop_duplicates on the SN(screen name) column to account for the unique players who have
made purchases. This was then passed into a data frame for display output.

```python
# calulated the total number of unique players
df_unique_player = df.drop_duplicates("SN")

# created a new data frame with the total players
total_players = pd.DataFrame({"Total Players": [len(df_unique_player)]})
```


#### Purchase Analysis (Total)
* The number of unique items was counted by using .unique() on the Item ID column.
* The average price was calulcated by using .mean() on the Price column and rounding to 2 decimal places.
* Total purchase was caluculated by using .count() on the Purchase ID column.
* Total revenue was done by using .sum() on the Price column.
* Each calulcaion was assinged to a variable, then passed into a data frame and formatting with f-strings for display
output.

```python
# calculated each aspect using different methods like unique, mean and sum
unique_item = len(df["Item ID"].unique())
avg_price = df["Price"].mean()
total_item = df["Item ID"].count()
total_rev = df["Price"].sum()

# create data frame with calculated data
purchase_analysis_total = pd.DataFrame(
    {
        "Number of Unique Items": [unique_item],
        "Average Price": [avg_price],
        "Number of Purchases": [total_item],
        "Total Revenue": [total_rev],
    }
)

purchase_analysis_total.round(2)
```

#### Gender Demographics
* In order to caluculate gender demographics, a new data frame was made consistig of the SN and Gender columns.
* The gender count was achievied by grouping and counting by the Gender row.
* Each gender percentage was accounted for by dividing the gender count by the sum of the gender count, and rounding to
two decimal places.
* The calulcations were assinged to variables and passed to a data frame. 

```python
# count players by gender
total_gender = df_unique_player["Gender"].value_counts()

gender_counts = (total_gender[0], total_gender[1], total_gender[2])

# calculate gender percentages
percents = [
    (total_gender[0] / len(df_unique_player)) * 100,
    (total_gender[1] / len(df_unique_player)) * 100,
    (total_gender[2] / len(df_unique_player)) * 100,
]

# create data frame with calculated data
gender_demo = pd.DataFrame(
    {"Total Count": gender_counts, "Percentage of Players": percents}
)

# set gender index
gender_demo.index = ["Male", "Female", "Other/Non-Disclosed"]


gender_demo.round(2)
```

#### Purchasing Analysis (Gender)
* Total purchases by gender were counted by grouping by the Gender column and using .count().
* The total purchase value was calulcated by grouping by the Gender column and using .sum() on the Price column.
* Each caluclation was then assinged to a variable.
* To get the average purchase price by gender, the total purchase value was divided by the total count of gender
purchases and rounded to 2 decimal places.
* The average total purchase per person in each gender group was calculated by diving the total purchase value by the
unique gender count that was created for the Gender Demographics table, then rounded to 2 decimal places.
* The calculations for averages were both assigned to variables.
* All variables were then passed to a data frame for display output.

```python 
# group by gender
gender = df.groupby(["Gender"])

# use the groups to calculate the purchase count and averages
purchase_cnt = gender["SN"].count()
avg_gen_price = gender["Price"].mean()
price_sum = gender["Price"].sum()
avg_price_person = price_sum / total_gender

# create data frame with calculated data
purchase_analysis_gender = pd.DataFrame(
    {
        "Purchase Count": purchase_cnt,
        "Average Purchase Price": avg_gen_price,
        "Total Purchase Value": price_sum,
        "Avg Total Purchase per Person": avg_price_person,
    }
)

purchase_analysis_gender.round(2)
```

#### Age Demographics
* Bins were used in increments of 4 years, from 0 to 45 (the max age in the dataset). Labels were created to accompany the respective
bins.
* A new dataframe was created out of columns SN, Age Range, and Age from the original data frame.
* Age count was calculated by grouping by the Age Range column and using .value_counts() on the Age column.
* The percent of players in each age range was calculated by diving the age count by the sum of the age count,
multiplying by 100 and rounding to 2 decimal places.
* Each calculation was assined to a variable.
* The variables were then passed to a data frame for display output.

```python
# establish bins for age
age_bins = [0, 9.9, 14.9, 19.9, 24.9, 29.9, 34.9, 39.9, 999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

df["Age Ranges"] = pd.cut(df_unique_player["Age"], bins=age_bins, labels=group_names)

age_demo_cnt = df["Age Ranges"].value_counts()

# calculate percent using the age count and the unique player count
age_demo_percent = (age_demo_cnt / len(df_unique_player)) * 100

# create data frame with calculation
age_demo = pd.DataFrame(
    {"Total Count": age_demo_cnt, "Percentage of Players": age_demo_percent}
)


age_demo.sort_index().round(2)
```

#### Top Spenders 
* The purchase count for each player was calculated by grouping by the SN column and using .count() on the Purchase ID
column.
* The total purchase per person was calculated by grouping by the SN column and using .sum() on the Price column.
* Both calculations were then passed to variables.
* The average purchase price per player was calculated by diving the total purchase per person by the total purchase
count per person, then rounding to 2 decimal points.
* The variables were then passed to a data frame for display output and sorted using .sort_values(), in descending order,
by Total Purchase Value. Then, .head(5) was used to dispaly the top 5 spenders.

```python
# calculated the purchase count, average and total prices by grouping by "SN"
purchase_sum_sn = df.groupby(["SN"])["Item ID"].count()
purchase_avg_sn = df.groupby(["SN"])["Price"].mean()
purchase_total_sn = df.groupby(["SN"])["Price"].sum()

# created data frame with the variables above
top_spenders = pd.DataFrame(
    {
        "Purchase Count": purchase_sum_sn,
        "Average Purchase Price": purchase_avg_sn,
        "Total Purchase Value": purchase_total_sn,
    }
)

# sorted top spenders by total purchase value
top_spenders.sort_values(by="Total Purchase Value", ascending=False).head(5).round(2)
```

#### Most Popular Items
* To analyze the most popular items, a data frame was created out of the columns Item ID, Item Name, and Price from the
original data frame.
* The purchase count was calulated by grouping by the Item ID and Item Name columns and using .count() on the Item ID
column.
* The item price was assigned to a variable by grouping by the Item ID and Item Name columns and using .mean() on the
Price column.
* The total purchase value was calculated by grouping by the Item ID and Item Name columns and using .sum() on the Price
column.
* The calculations were then assinged to variables.
* The variables were then passed to a data frame for display output and sorted using .sort_values(), in descending order,
by Purchase Count. To get the top 5 most popular items, .head(5) was used.

```python
# calcutated item count, average and total value by grouping the "Item ID" and "Item Name"
item_purchase_cnt = df.groupby(["Item ID", "Item Name"])["Item Name"].count()
item_purchase_avg = df.groupby(["Item ID", "Item Name"])["Price"].mean()
item_purchase_sum = df.groupby(["Item ID", "Item Name"])["Price"].sum()

# created data frame with variables above
popular_items = pd.DataFrame(
    {
        "Purchase Count": item_purchase_cnt,
        "Item Price": item_purchase_avg,
        "Total Purchase Value": item_purchase_sum,
    }
)

# sorted popular items by purchase count
popular_items.sort_values(by="Purchase Count", ascending=False).head(5)
```

#### Most Profitable Items
* The most profitable items table was created by reusing the Popular Items data frame and sorting by Total Purchase Value
in descending order with .head(5).

```python
# using the data frame from the problem above but sorting by
# "Total Purchase Value" instead to find the most profitable items

popular_items.sort_values(by="Total Purchase Value", ascending=False).head(5)
```