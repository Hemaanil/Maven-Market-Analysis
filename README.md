## Maven Market Analysis

## PART 1: Connecting & Shaping the Data 

1) Update your Power BI options and settings as follows: 
•	Deselect the "Autodetect new relationships after data is loaded" option in the Data Load tab
•	Make sure that Locale for import is set to "English (United States)" in the Regional Settings tab

2) Connect to the MavenMarket_Customers csv file
•	Name the table "Customers", and make sure that headers have been promoted
•	Confirm that data types are accurate (Note: "customer_id" should be whole numbers, and both "customer_acct_num" and "customer_postal_code" should be text)
•	Add a new column named "full_name" to merge  the "first_name" and "last_name" columns, separated by a space

first select both the column names, go to Add column -> merge columns->in a pop up box select SPACE in the separator and give the new  column name ->ok

•	Create a new column named "birth_year" to extract the year from the "birthdate" column, and format as text

First select  the specified column and  go to  Add column -> Extract (drop down)-> text after delimeter-> give the delimeter(-,hyphen) . In advanced options in the same pop up box, in the scan for the delimeter ( frm the st of the input),  no.of  delimeters to skip (enter 1)-> ok

Create a conditional column named "has_children" which equals "N" if "total_children" = 0, otherwise "Y"

Select  total children column->Add column-> conditional column-> value(0)->N-> else -> Y

3) Connect to the MavenMarket_Products csv file

•	Name the table "Products" and make sure that headers have been promoted
•	Confirm that data types are accurate (Note: "product_id" should be whole numbers, "product_sku" should be text), "product_retail_price" and "product_cost" should be decimal numbers)
•	Use the statistics tools to return the number of distinct product brands, followed by distinct product names
Spot check: You should see 111 brands and 1,560 product names

Select pdct brands ->Transform->statistics->count distinct values-> will get ans
And the same process for pdct names column too.
This qusn is just to check the values.

•	Add a calculated column named "discount_price", equal to 90% of the original retail price
Format as a fixed decimal number, and then use the rounding tool to round to 2 digits

Go to Add Column -> Custom Column -> give the column name-> give the formula like(select the column name *0.9) - > ok -> to round the fig to 2 digits -> Transform -> Rounding -> 2 digits.

•	Select "product_brand" and use the Group By option to calculate the average retail price by brand, and name the new column "Avg Retail Price"
Spot check: You should see an average retail price of $2.18 for Washington products, and $2.21 for Green Ribbon
Delete the last applied step to return the table to its pre-grouped state


•	Replace "null" values with zeros in both the "recyclable" and "low-fat" columns
Select the respective columns individually and go to transform column -> replace with  -> give value.

4) Connect to the MavenMarket_Stores csv file

•	Name the table "Stores" and make sure that headers have been promoted
•	Confirm that data types are accurate (Note: "store_id" and "region_id" should be whole numbers)
•	Add a calculated column named "full_address", by merging "store_city", "store_state", and "store_country", separated by a comma and space (hint: use a custom separator)
By Selecting the respective 3 columns go to Add column -> Merge Columns -> give column name and in separator tab select custom and give , (coma, space)
•	Add a calculated column named "area_code", by extracting the characters before the dash ("-") in the "store_phone" field
Select the column -> add column -> Extract -> Text b4 Delimeter ->in upper tab give  (-) and in lower tab mention 1 .

5) Connect to the MavenMarket_Regions csv file
•	Name the table "Regions" and make sure that headers have been promoted
•	Confirm that data types are accurate (Note: "region_id" should be whole numbers)

6) Connect to the MavenMarket_Calendar csv file
•	Name the table "Calendar" and make sure that headers have been promoted
•	Use the date tools in the query editor to add the following columns:
Go to AddColumn -> Date -> Start of week
•	Start of Week (starting Sunday)
To get Sunday Week name  we have to give the formula as
= Table.AddColumn(#"Changed Type", "Start of Week", each Date.StartOfWeek([date],Day.Sunday))
Select Date column in that calendar table go to Add Column -> Name of Day
•	Name of Day
•	Start of Month
•	Name of Month
•	Quarter of Year
•	Year
For all the above sections first we have to select Date Column and then we have to select particular type in date field.

7) Connect to the MavenMarket_Returns csv file
•	Name the table "Return_Data" and make sure that headers have been promoted
•	Confirm that data types are accurate (all ID columns and quantity should be whole numbers)

8) Add a new folder on your desktop (or in your documents) named "MavenMarket Transactions", containing both the MavenMarket_Transactions_1997 and MavenMarket_Transactions_1998 csv files
•	Connect to the folder path, and choose "Edit" (vs. Combine and Edit)
•	Click the "Content" column header (double arrow icon) to combine the files, then remove the "Source.Name" column
Delete first column(source.name) by right clicking and remove option.
•	Name the table "Transaction_Data", and confirm that headers have been promoted
•	Confirm that data types are accurate (all ID columns and quantity should be whole numbers)
•	Spot check: You should see data from 1/1/1997 through 12/30/1998 in the "transaction_date" column
Select that column and go to Transform->Statastics -> distinct values and give the above mentioned dates to spot check the data, for the next step delete this step in applied steps.

9) With the exception of the two data tables, disable "Include in Report Refresh", then Close & Apply
•	Confirm that all 7 tables are now accessible within both the RELATIONSHIPS view and the DATA view
For the remaining tables like Customers, products, calendar, stores and Regions
Right click and uncheck include in Report Refresh.

## PART 2: Creating the Data Model

1) In the MODEL view, arrange your tables with the lookup tables above the data tables
•	Connect Transaction_Data to Customers, Products, and Stores using valid primary/foreign keys 
•	Connect Transaction_Data to Calendar using both date fields, with an inactive "stock_date" relationship
•	Connect Return_Data to Products, Calendar, and Stores using valid primary/foreign keys
•	Connect Stores to Regions as a "snowflake" schema

2) Confirm the following:
•	All relationships follow one-to-many cardinality, with primary keys (1) on the lookup side and foreign keys (*) on the data side
•	Filters are all one-way (no two-way filters)
•	Filter context flows "downstream" from lookup tables to data tables
•	Data tables are connected via shared lookup tables (not directly to each other)

3) Hide all foreign keys in both data tables from Report View, as well as "region_id" from the Stores table

4) In the DATA view, complete the following:
•	Update all date fields (across all tables) to the "M/d/yyyy" format using the formatting tools in the Modeling tab
•	Update "product_retail_price", "product_cost", and "discount_price" to Currency ($ English) format
•	In the Customers table, categorize "customer_city" as City, "customer_postal_code" as Postal Code, and "customer_country" as Country/Region
•	In the Stores table, categorize "store_city" as City, "store_state" as State or Province, "store_country" as Country/Region, and "full_address" as Address

## PART 3: Adding DAX Measures

1) In the DATA view, add the following calculated columns:
•	In the Calendar table, add a column named "Weekend"
•	Equals "Y" for Saturdays or Sundays (otherwise "N")

*Weekend = IF('Calendar'[Day Name]="Saturday" || 'Calendar'[Day Name]="Sunday", "Y", "N")*

•	In the Calendar table, add a column named "End of Month"
•	Returns the last date of the current month for each row

•	*End of Month = ENDOFMONTH('Calendar'[date])*

•	In the Customers table, add a column named "Current Age"
•	Calculates current customer ages using the "birthdate" column and the TODAY() function

•	*Current Age = DATEDIFF(Customers[birthdate],TODAY(),YEAR)*

•	In the Customers table, add a column named "Priority"
•	Equals "High" for customers who own homes and have Golden membership cards (otherwise "Standard") 

•	*Priority = IF(Customers[homeowner] ="Y" && Customers[member_card] = "Golden", "High","Standard")*
 
•	In the Customers table, add a column named "Short_Country"
•	Returns the first three characters of the customer country, and converts to all uppercase 
•	

*Short_Country = UPPER(LEFT(Customers[Country/Region],3)*

•	In the Customers table, add a column named "House Number"
•	Extracts all characters/numbers before the first space in the "customer_address" column (hint: use SEARCH)

•	*House_Number = LEFT(Customers[customer_address], SEARCH(" ", Customers[customer_address])-1)*

•	In the Products table, add a column named "Price_Tier"
•	Equals "High" if the retail price is >$3, "Mid" if the retail price is >$1, and "Low" otherwise

•	*Price_Tier = IF(Products[product_retail_price]>3 , "High", IF(Products[product_retail_price]>1 ,"Mid", "Low"))*

•	In the Stores table, add a column named "Years_Since_Remodel"
•	Calculates the number of years between the current date (TODAY()) and the last remodel date
•	Years_Since_Remodel = DATEDIFF(Stores[last_remodel_date],TODAY(),YEAR)

2) In the REPORT view, add the following measures (Assign to tables as you see fit, and use a matrix to match the "spot check" values)
•	Create new measures named "Quantity Sold" and "Quantity Returned" to calculate the sum of quantity from each data table
•	Spot check: You should see total Quantity Sold = 833,489 and total Quantity Returned = 8,289

•	*Quantity Returned = SUM(Return_data[quantity])*

•	*Quantity Sold = SUM(Transaction_Data[quantity])*
•	

•	Create new measures named "Total Transactions" and "Total Returns" to calculate the count of rows from each data table
•	Spot check: You should see 269,720 transactions and 7,087 returns

•	*Total Transactions = COUNTROWS(Transaction_Data)*
![Screenshot 2024-06-13 005423](https://github.com/Hemaanil/Maven-Market-Analysis/assets/165702332/9dfb2b66-8b4c-4287-9344-7fa60300788d)


•	*Total Returns = COUNTROWS(Return_data)*
![Screenshot 2024-06-13 005447](https://github.com/Hemaanil/Maven-Market-Analysis/assets/165702332/59f17177-6c35-46d2-a4df-a4bc85eca41e)


•	Create a new measure named "Return Rate" to calculate the ratio of quantity returned to quantity sold (format as %)
•	Spot check: You should see an overall return rate of 0.99%

•	*Return Rate = [Quantity Returned]/[Quantity Sold]*
![Screenshot 2024-06-13 005501](https://github.com/Hemaanil/Maven-Market-Analysis/assets/165702332/1d311c80-cd13-4e39-873b-c340f0b2cfc2)


•	Create a new measure named "Weekend Transactions" to calculate transactions on weekends
•	Spot check: You should see 76,608 total weekend transactions

•	*Weekend Transactions = CALCULATE(Transaction_Data[Total Transactions], 'Calendar'[Weekend] = "Y")*

•	Create a new measure named "% Weekend Transactions" to calculate weekend transactions as a percentage of total transactions (format as %)
•	Spot check: You should see 28.4% weekend transactions

•	*%Weekend Transactions = DIVIDE([Weekend Transactions],Transaction_Data[Total Transactions])*

•	Create new measures named "All Transactions" and "All Returns" to calculate grand total transactions and returns (regardless of filter context)
•	Spot check: You should see 269,720 transactions and 7,087 returns across all rows (test with product_brand on rows)
•	All Transactions = COUNT(Transaction_Data[quantity])
•	All Returns = COUNT(Return_data[quantity])

•	Create a new measure to calculate "Total Revenue" based on transaction quantity and product retail price, and format as $ (hint: you'll need an iterator)
•	Spot check: You should see a total revenue of $1,764,546

•	*Total Revenue = SUMX(Transaction_Data, Transaction_Data[quantity] * RELATED(Products[product_retail_price]))*

•	Create a new measure to calculate "Total Cost" based on transaction quantity and product cost, and format as $ (hint: you'll need an iterator)
•	Spot check: You should see a total cost of $711,728
•	

*Total Cost = SUMX(Transaction_Data, Transaction_Data[quantity]*RELATED(Products[product_cost])*


•	Create a new measure named "Total Profit" to calculate total revenue minus total cost, and format as $
•	Spot check: You should see a total profit of $1,052,819
•	Total Profit = [Total Revenue] - [Total Cost]

•	Create a new measure to calculate "Profit Margin" by dividing total profit by total revenue calculate total revenue (format as %)
•	Spot check: You should see an overall profit margin of 59.67%
•	Proft Margin = [Total Profit]/[Total Revenue]

•	Create a new measure named "Unique Products" to calculate the number of unique product names in the Products table
•	Spot check: You should see 1,560 unique products
•	Unique Products = DISTINCTCOUNT(Products[product_name])

•	Create a new measure named "YTD Revenue" to calculate year-to-date total revenue, and format as $
•	Spot check: Create a matrix with "Start of Month" on rows; you should see $872,924 in YTD Revenue in September 1998

•	*YTD Revenue = CALCULATE([Total Revenue], DATESYTD('Calendar'[date]))*

•	Create a new measure named "60-Day Revenue" to calculate a running revenue total over a 60-day period, and format as $
•	Spot check: Create a matrix with "date" on rows; you should see $97,570 in 60-Day Revenue on 4/14/1997

•	*60-Day Revenue = CALCULATE([Total Revenue], DATESINPERIOD('Calendar'[date], MAX('Calendar'[date]),-60,DAY))* 

•	Create new measures named  "Last Month Transactions", "Last Month Revenue", "Last Month Profit", and "Last Month Returns"
•	Spot check: Create a matrix with "Start of Month" on rows to confirm accuracy

•	*Last Month Profit = CALCULATE([Total Profit], DATEADD('Calendar'[date],-1,MONTH))
•	Last Month Revenue = CALCULATE([Total Revenue], DATEADD('Calendar'[date],-1,MONTH))
•	Last Month Returns = CALCULATE([Total Returns], DATEADD('Calendar'[date],-1,MONTH))
•	Last Month Transactions = CALCULATE([Total Transactions], DATEADD('Calendar'[date],-1,MONTH))*

•	Create a new measure named "Revenue Target" based on a 5% lift over the previous month revenue, and format as $
•	Spot check: You should see a Revenue Target of $99,223 in March 1998

•	*Revenue Target = [Last Month Revenue] * 1.05*

Expressing a Percentage Increase as a Multiplier:
•	To express a percentage increase as a multiplier in a formula, we use 1 + (percentage as a decimal).
•	For a 5% increase, we express it as 1 + 0.05.

# PART 4: Building the Report

1)	Rename the tab "Topline Performance" and insert the Maven Market logo

2) Insert a Matrix visual to show Total Transactions, Total Profit, Profit Margin, and Return Rate by Product_Brand (on rows)

Add conditional formatting to show data bars on the Total Transactions column, and color scales on Profit Margin (White to Green) and Return Rate (White to Red)
Add a visual level Top N filter to only show the top 30 product brands, then sort descending by Total Transactions


3) Add a KPI Card to show Total Transactions, with Start of Month as the trend axis and Last Month Transactions as the target goal

Update the title to "Current Month Transactions", and format as you see fit
Create two more copies: one for Total Profit (vs. Last month Profit) and one for Total Returns (vs. Last Month Returns)
Make sure to update titles, and change the Returns chart to color coding to "Low is Good"


4) Add a Map visual to show Total Transactions by store city

Add a slicer for store country 
Under the "selection controls" menu in the formatting pane, activate the "Show Select All" option
Pro Tip: Change the orientation in the "General" formatting menu to horizontal and resize to create a vertical stack (rather than a list)


5) Next to the map, add a Treemap visual to break down Total Transactions by store country

Pull in store_state and store_city beneath store_country in the "Group" field to enable drill-up and drill-down functionality


6) Beneath the map, add a Column Chart to show Total Revenue by week, and format as you see fit

Add a report level filter to only show data for 1998
Update the title to "Weekly Revenue Trending"


7) In the lower right, add a Gauge Chart to show Total Revenue against Revenue Target (as either "target value" or "maximum value")

Add a visual level Top N filter to show the latest Start of Month
Remove data labels, and update the title to "Revenue vs. Target"


8) Select the Matrix and activate the  Edit interactions option to prevent the Treemap from filtering



9) Select "USA" in the country slicer, and drill down to select "Portland" in the Treemap

Add a new bookmark named "Portland 1000 Sales"
Add a new report page, named "Notes"
Insert a text box and write something along the lines of "Portland hits 1,000 sales in December"
Add a button (your choice) and use the "Action" properties to link it to the bookmark you created
Test the bookmark by CTRL-clicking the button
Find 2-3 additional insights from the Topline Performance tab and add new bookmarks and notes linking back
![Screenshot 2024-06-13 004958](https://github.com/Hemaanil/Maven-Market-Analysis/assets/165702332/666c4086-a9b8-4c6e-882b-5de21326d482)



10) Get creative! Practice creating new visuals, pages, or bookmarks to continue exploring the data!

Final Report is......
![Screenshot 2024-06-13 005702](https://github.com/Hemaanil/Maven-Market-Analysis/assets/165702332/a3b55315-5c4e-4abb-b31a-fdc7361d0acc)








 









