## Heroes of Pymoli
   
from the purchase data, show the various result summary using pandas dataframe

### Dependencies and Setup
import pandas as pd
import numpy as np

### File to Load  
file_to_load = "purchase_data.csv"

### Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)

### Player Count

* Display the total number of players


purchase_data.head()

player_demo = purchase_data.loc[:, ["Gender", "SN", "Age"]]
    

player_demo=player_demo.drop_duplicates()

#Total number of players
player_count=player_demo.count()[1]

#### Display total number of players as data frame
pd.DataFrame({"Total Players": [player_count]})

### Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


### Run basic calculations
unique_item_number=len(purchase_data["Item ID"].unique())
average_price=purchase_data["Price"].mean()
total_price=purchase_data["Price"].sum()
purchase_count=purchase_data["Price"].count()

### summary data frame 
summary_table=pd.DataFrame({"Number of Unique Items": unique_item_number, "Total Revenue": [total_price], "Number of Purchase": [purchase_count], "Average Price": [average_price]})

summary_table=summary_table.loc[:, ["Number of Unique Items", "Average Price", "Number of Purchase", "Total Revenue"]]

#### Data Munging
summary_table=summary_table.round(2)
summary_table["Average Price"]=summary_table["Average Price"].map("${:,.2f}".format)
summary_table["Number of Purchase"]=summary_table["Number of Purchase"].map("{:,}".format)
summary_table["Total Revenue"]=summary_table["Total Revenue"].map("${:,.2f}".format)

### Display the summary data frame
summary_table

### Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed




### Percentage and Count of Players
gender_demo_totals = player_demo["Gender"].value_counts()
gender_demo_percents = gender_demo_totals / player_count * 100
gender_demo = pd.DataFrame({"Total Count": gender_demo_totals, "Percentage of Players": gender_demo_percents})


### Data Munging
gender_demo=gender_demo.round(2)

### Display gender data frame
gender_demo


### Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame

### Run basic calculations
gender_purchase_total = purchase_data.groupby(["Gender"]).sum()["Price"].rename("Total Purchase Value")
gender_purchase_average = purchase_data.groupby(["Gender"]).mean()["Price"].rename("Average Purchase Price")
gender_purchase_count = purchase_data.groupby(["Gender"]).count()["Price"].rename("Purchase Count")

### Calculate Average Total Purchase per Person
average_total = gender_purchase_total / gender_demo["Total Count"]

### Convert to DataFrame
summary_data = pd.DataFrame({"Purchase Count": gender_purchase_count, "Average Purchase Price": gender_purchase_average, "Total Purchase Value": gender_purchase_total, "Average Purchase Total": average_total})

### Data Munging
summary_data["Average Purchase Price"] = summary_data["Average Purchase Price"].map("${:,.2f}".format)
summary_data["Total Purchase Value"] = summary_data["Total Purchase Value"].map("${:,.2f}".format)
summary_data ["Purchase Count"] = summary_data["Purchase Count"].map("{:,}".format)
summary_data["Average Purchase Total"] = summary_data["Average Purchase Total"].map("${:,.2f}".format)
summary_data = summary_data.loc[:, ["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Average Purchase Total"]]

### Display the Summary Table
summary_data

### Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table


### Establish the bins 
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 999]
age_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

### Categorize the existing players using the age bins
player_demo["Age Ranges"] = pd.cut(player_demo["Age"], age_bins, labels=age_names)

### Calculate the Numbers and Percentages by Age Group
age_demo_totals = player_demo["Age Ranges"].value_counts()
age_demo_percents = age_demo_totals / player_count * 100
age_demo = pd.DataFrame({"Total Count": age_demo_totals, "Percentage of Players": age_demo_percents})

### Data Munging
age_demo = age_demo.round(2)

### Display Age Demographics Table
age_demo.sort_index()

### Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame

### Bin the purchase_data data frame by age
purchase_data["Age Range"] = pd.cut(purchase_data["Age"], age_bins, labels=age_names)
### Run basic calculations
age_purchase_total = purchase_data.groupby(["Age Range"]).sum()["Price"].rename("Total Purchase Value")
age_purchase_average = purchase_data.groupby(["Age Range"]).mean()["Price"].rename("Average Purchase Price")
age_purchase_count = purchase_data.groupby(["Age Range"]).count()["Price"].rename("Purchase Count")

### Calculate Average Total Purchase per Person
age_average_total = age_purchase_total / age_demo["Total Count"]

### Convert to DataFrame
age_summary_data = pd.DataFrame({"Purchase Count": age_purchase_count, "Average Purchase Price": age_purchase_average, "Total Purchase Value": age_purchase_total, "Average Purchase Total Per Person": age_average_total})

### Data Munging
age_summary_data["Average Purchase Price"] = age_summary_data["Average Purchase Price"].map("${:,.2f}".format)
age_summary_data["Total Purchase Value"] = age_summary_data["Total Purchase Value"].map("${:,.2f}".format)
age_summary_data ["Purchase Count"] = age_summary_data["Purchase Count"].map("{:,}".format)
age_summary_data["Average Purchase Total Per Person"] = age_summary_data["Average Purchase Total Per Person"].map("${:,.2f}".format)
age_summary_data = age_summary_data.loc[:, ["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Average Purchase Total Per Person"]]

### Display the Summary Table
age_summary_data

## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame



### Run basic calculations
sn_total = purchase_data.groupby(["SN"]).sum()["Price"].rename("Total Purchase Value")
sn_average_price = purchase_data.groupby(["SN"]).mean()["Price"].rename("Average Purchase Price")
sn_purchase_count = purchase_data.groupby(["SN"]).count()["Price"].rename("Purchase Count")
sn_summary_data = pd.DataFrame({"Purchase Count": sn_purchase_count, "Average Purchase Price": sn_average_price, "Total Purchase Value": sn_total})
### sort the total purchase value in descending order
sn_sort = sn_summary_data.sort_values("Total Purchase Value", ascending=False)
### Data munging
sn_sort["Average Purchase Price"] = sn_sort["Average Purchase Price"].map("${:,.2f}".format)
sn_sort["Total Purchase Value"] = sn_sort["Total Purchase Value"].map("${:,.2f}".format)
sn_sort = sn_sort.loc[:,["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

### Display a preview of the summary data frame
sn_sort.head(5)

## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame



### Extract item Data
item_data = purchase_data.loc[:,["Item ID", "Item Name", "Price"]]

### Perform basic calculations
total_item_purchase = item_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")
average_item_purchase = item_data.groupby(["Item ID", "Item Name"]).mean()["Price"]
item_count = item_data.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Purchase Count")

### Create new DataFrame
item_data_pd = pd.DataFrame({"Total Purchase Value": total_item_purchase, "Item Price": average_item_purchase, "Purchase Count": item_count})

### Sort Values
item_data_count_sorted = item_data_pd.sort_values("Purchase Count", ascending=False)

### Data Munging
item_data_count_sorted["Item Price"] = item_data_count_sorted["Item Price"].map("${:,.2f}".format)
item_data_count_sorted["Purchase Count"] = item_data_count_sorted["Purchase Count"].map("{:,}".format)
item_data_count_sorted["Total Purchase Value"] = item_data_count_sorted["Total Purchase Value"].map("${:,.2f}".format)
item_popularity = item_data_count_sorted.loc[:,["Purchase Count", "Item Price", "Total Purchase Value"]]

item_popularity.head(5)

### Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame



### Sort Values
item_data_count_sorted = item_data_pd.sort_values("Total Purchase Value", ascending=False)

item_profitable = item_data_count_sorted.loc[:,["Purchase Count", "Item Price", "Total Purchase Value"]]

### Display data frame
item_profitable.head(5)



