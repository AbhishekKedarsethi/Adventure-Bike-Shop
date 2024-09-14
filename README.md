# Adventure Works -Dashboard

#### Dashboard Link : https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection

## Problem Statement

This dashboard provides invaluable insights into customer behavior, enabling the owner to monitor orders, revenue, returns, and profit effectively. By leveraging this tool, the owner can access detailed customer information and identify the geographical areas generating the most orders. For instance, if the majority of orders are coming from urban areas, targeted marketing campaigns can be developed to further penetrate these markets.

Additionally, the dashboard categorizes products into various categories and sub-categories, highlighting areas for improvement. For example, if a particular sub-category like “electronics accessories” shows higher return rates, the owner can investigate potential quality issues or mismatches in customer expectations.

Ultimately, this dashboard empowers the owner to refine their services and ensure a superior customer experience. By continuously analyzing data, the owner can stay ahead of trends, anticipate customer needs, and make informed decisions that drive business growth.



#### We have divided the total project into 4 parts 
- Loading and Shaping data
- Creating the data modeling
- Adding DAX Measures
- Building Reports

#### Loading and Shaping data (Steps followed)


In this initial phase, we focused on importing and preparing our datasets to ensure they are clean and ready for analysis.

- Step 1: We began by loading the data into Power BI Desktop, utilizing CSV files as our primary data source.
- Step 2: Upon uploading, Power BI automatically promoted headers and adjusted data types to match the dataset’s content.
- Step 3: We meticulously reviewed all headers and data types for accuracy, making manual adjustments where necessary via the Home ribbon’s data type options.
- Step 4: We identified and addressed errors and empty values within the columns, either removing or replacing them using the ‘Replace Value’ feature in the Transform ribbon.
- Step 5: Post-cleaning, we merged the “First Name” and “Last Name” columns to create a “Full Name” column. Additionally, we introduced a “Discount Price” column by applying a 10% discount to the “Product Price” column.
- Step 6: We loaded a new “Calendar” table, creating columns for:
Start of Week (starting Sunday)
Name of Day
Start of Month
Name of Month
Quarter of Year
Year

All necessary CSV files were loaded and transformed into appropriately named tables, completing the data cleaning and shaping phase.


#### Creating the Data Modeling
In this phase, we established robust relationships between our datasets to enable comprehensive and accurate analysis.

- Step 1: We navigated to the data model view in Power BI and connected tables using primary and foreign keys.
- Step 2: Specifically, we linked the transactions table with the customer, calendar, products, and stores tables, ensuring all necessary relationships were established.
- Step 3: We verified that all connections were one-to-many relationships, with primary keys (1) on the lookup side and foreign keys (*) on the data side, ensuring data integrity and accurate filtering.
- Step 4: To streamline the report creation process, we hid all foreign keys in the tables, reducing the risk of selecting incorrect columns during report generation.
- Step 5: We updated the format of various fields, such as currency, postal codes, country, city, and address, to facilitate easier report creation and to leverage advanced visualization options like maps and date-based analyses.

Here is the quick preview of the data model :

<img width="709" alt="AD_model" src="https://github.com/user-attachments/assets/8f2a5475-e4f2-4159-b538-adf01b2fb392">


By meticulously creating and validating these data models, we laid a solid foundation for insightful and reliable reporting, enabling the owner to make data-driven decisions with confidence

#### Adding DAX Measures

- Step 1 : In the data view we have created total orders, total custommers, total profit, total returns by using basic sum and count DAX functions.

Count dax function :

		Total Returns = 
		COUNT(
   		 'Returns Data'[ReturnQuantity]
		)

Distinctcount dax function :


		Total Orders = 
		DISTINCTCOUNT(
		    'Sales Data'[OrderNumber]
		)

- Step 2 : We have created previous month orders, profits, returns by using calculate dax function.

		Previus Month Orders = 
		CALCULATE(
		    [Total Orders],
		    DATEADD(
		        'Calendar Lookup'[Date],
		        -1,
 		       MONTH
		    )
		)

- Step 3 : We have created average retail price and average revenue per customer by using some basic common mathematical dax functions.

divide dax function :

        Average revenue per customer = 
		DIVIDE(
 		   [Total Revenue],
 		   [Total Customers]
		)

multiplication and addition dax functions :

		Adjustment Price = [Average retail price] * (1 + 'Price Adjustment (%)'[Price Adjustment (%) Value])

subtraction dax function :

		Total Profit = 
		[Total Revenue] - [Total Cost]

multiplication dax function :

		Target for revenue = 
		[Previous Month] * 1.1

- Step 3 : We can also use some conditional functions in dax such as bike returns, bike sales, bulk quantity, etc by using calculate dax function.

		Bike Sales = 
		CALCULATE(
		    [Total Orders],
		    'Product Categories Lookup'[CategoryName] = "Bikes"
		)

- Step 4 : We can use filters in calculate function to get more refined calculated values.

		High Ticket orders = 
		CALCULATE(
		    [Total Orders],
 		   FILTER(
 		       'Product Lookup',
 		       'Product Lookup'[ProductPrice] > [Over all average price]
 		   )
		)
by using all filter : 

		Over all average price = 
		CALCULATE(
 		   [Average retail price],
 		   ALL(
 		       'Product Lookup'
 		   )
		)

- Step 5 : We can get rolling revenue by using time dax functons.

for 10 days rolling revenue :

		10 days rolling revenue = 
		CALCULATE(
 		   [Total Revenue],
 		   DATESINPERIOD(
 		       'Calendar Lookup'[Date],
 		       MAX(
 		           'Calendar Lookup'[Date]
 		       ),
 		       -10,
  		      DAY
  		  )
		)

for 90 days rolling profit :

		90 days rolling profit = 
		CALCULATE(
 		   [Total Profit],
 		   DATESINPERIOD(
  		      'Calendar Lookup'[Date],
   		     MAX(
   		         'Calendar Lookup'[Date]
   		     ),
   		     -90,
    		    DAY)
		)

for Year to Date revenue :

		YTD Revenue = 
		CALCULATE(
 		   [Total Revenue],
 		   DATESYTD(
 		       'Calendar Lookup'[Date]
  		  )
		)


- Step 6 : We can also give conditional value for one value and for multiple values.

		Full Name (Customer details) = 
		IF(
 		   HASONEVALUE(
 		       'Customer Lookup'[Full Name]),
 		       MAX(
 		           'Customer Lookup'[Full Name]
 		       ),
  		      "Multiple Customers"
		)

#### Building Reports

- Step 1 : We have added 4 Card's that repesents "Revenue", "Profit", "Orders", "Returns" and grouped them.

<img width="490" alt="4 KPI's" src="https://github.com/user-attachments/assets/ef07dd29-6b3d-408e-84fb-41d00c7d80d8">


- Step 2 : We have added a clustered bar chart for the product category and total orders.

<img width="244" alt="category" src="https://github.com/user-attachments/assets/1497240d-0671-49a5-9015-4e7a6b923ccb">

- Step 3 : We have added a Line chart for the revenue generated over time which we can drill up and down so that we can sort it by time (ie year, Quarter, Month, Week) 

<img width="330" alt="Line chart" src="https://github.com/user-attachments/assets/273d5704-ffc5-427c-93dc-882557c85630">


- Step 4 : We also added 3 KPI's for Revenue, Orders, Returns for the monthly comparision with the previous month as target.

<img width="332" alt="Monthly Kpi" src="https://github.com/user-attachments/assets/0ce550bc-d195-42ce-8183-71d70db17a15">

- Step 5 : We have added a matrix for the Top 10 products with the values of Orders, Revenue and Returns and a row for Total values.


- Step 6 : We have added 2 Cards for "The Most Ordered Product" and "The Most Returned Product".

<img width="239" alt="top10" src="https://github.com/user-attachments/assets/fe7866dd-a8d6-407e-8dc3-9f7a76131245">

- Step 7 : We have created a Map in a new page with a slicer as tile with options as continents.

<img width="578" alt="Map" src="https://github.com/user-attachments/assets/fdda2dfc-1186-4db9-ac7c-96a6434004a7">

- Step 8 : We have also created 3 gauge's in a new page named as " Product Details" with orders, profit and revenue and a card with product details.

<img width="566" alt="gauge" src="https://github.com/user-attachments/assets/e7a76bec-e6bc-4c10-9515-1d016aaf69a0">

- Step 9 : We have also added a line graph with profit and date hierarchy with a slicer and also a return trending area chart. We have also added a Report Summary.

<img width="578" alt="product_chart" src="https://github.com/user-attachments/assets/f1ff85de-339c-4579-a2a8-e5d61587a36c">

- Step 10 : We have added a line chart for total customers and revenue per customer with date hierarchy in a new page named as "Customer details" 

<img width="455" alt="cus_line" src="https://github.com/user-attachments/assets/6c2918bb-64fd-4195-96c2-71e062b05182">

- Step 11 : We have also created matrix for 100 customers and a slicer with years and 3 cards for Top customer, Orders, Revenue

<img width="422" alt="top 100 cus" src="https://github.com/user-attachments/assets/451e3ea1-8646-4a77-bdf4-bc1c42dcfc48">

- Step 12 : We have created 2 cards for "Unique Customers", "Revenue per Customers" and 2 Donut charts for "Orders by Income", "Orders by Occupation".

<img width="141" alt="cards_pie" src="https://github.com/user-attachments/assets/1169949c-e10a-45e2-826b-165954b3a02e">

- Step 13 : We have crated buttons for each page and for also for main page by using bookmarks.

Here is the preview of the Dashboard :

<img width="600" alt="first_page" src="https://github.com/user-attachments/assets/45274177-26d0-4df0-a799-13279777d8fe">

[Adventure Works Bike Shop.pdf](https://github.com/user-attachments/files/17003153/Adventure.Works.Bike.Shop.pdf)
