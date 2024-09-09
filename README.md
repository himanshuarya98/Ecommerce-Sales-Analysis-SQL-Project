# Ecommerce Sales Analysis SQL Project

## Project ##

This project showcases my ability to analyze and derive insights from an online eCommerce dataset using SQL. The project involves managing an eCommerce database to explore sales trends, customer behavior, product performance, and inventory management. Through a series of SQL queries, I addressed key business questions, such as identifying top-selling products, analyzing customer orders, assessing sales performance across different categories and brands, and evaluating the contribution of different regions and supervisors to overall sales. This project is a practical demonstration of essential SQL skills, including data cleaning, exploratory data analysis. 

## OBJECTIVE ##

- **Database:** I have used `PracticeDB` database to analyze the dataset.
- **Data Preparation:** Renamed the table from **`online-ecommerce`** to **`online_ecommerce`**. Converted the **`Order_Date`** column from text to a **`DATE`** data type, renaming it to **`Orders_Date`**.
- **Exploratory Data Analysis (EDA):** Conducted basic exploratory data analysis to gain an understanding of the dataset.
- **Data Analysis:** Executed SQL queries to answer specific business questions and extract insights from the eCommerce sales data.

## Table Info ##
1. Order_Number int,
2. State_Code Varchar(10),
3. Customer_Name Varchar(30),
4. Status Varchar(20),
5. Product Varchar(30),
6. Category Varchar(30),
7. Brand Varchar(20),
8. Cost int,
9. Sales int,
10. Quantity int,
11. Total_Cost int,
12. Total_Sales int,
13. Assigned_Supervisor Varchar(40),
14. Orders_Date Date

## Data Prepation ##

``` sql

#Loaded CSV File Into MySQL

#Using Praticedb Dataset to Analyse the online_ecommerce dataset. 

use practicedb;

SELECT * FROM PracticeDB.`online-ecommerce`;

#Renaming Table 'online-ecommerce' to 'online_ecommerce'

RENAME TABLE PracticeDB.`online-ecommerce` to online_ecommerce;

#Renaming Column 'Assigned Supervisor' to 'Assigned_Supervisor'

Alter Table online_ecommerce Change Column `Assigned Supervisor` Assigned_Supervisor varchar(40);

#Current Order_Date Column is in "dd/mm/yyyy". Changing the Date Format to "yyyy-mm-dd"

SELECT DATE_FORMAT(STR_TO_DATE(Order_Date, '%d/%m/%Y'), '%Y-%m-%d') AS Orders_Date FROM online_ecommerce;

Alter Table online_ecommerce Add Column Orders_Date DATE;

UPDATE online_ecommerce SET Orders_date = STR_TO_DATE(Order_Date, '%d/%m/%Y')
WHERE STR_TO_DATE(Order_Date, '%d/%m/%Y') IS NOT NULL;

#Now Dropping Old Column Order_Date 

Alter Table online_ecommerce 

Drop column Order_Date;

```

## Data Cleaning & Exploration ##

- **Record Count**: Determined the total number of records in the dataset.
- **Unique Category Count**: Identified the unique categories within the dataset.
- **State Code Count**: Counted the unique state codes present in the dataset.
- **Null Values**: Checked for any null values across all columns in the dataset.

``` sql 
SELECT * From online_ecommerce;

#Checked Total Records in our Dataset
SELECT Count(*) From online_ecommerce;

#Checked Unique Category in our Dataset
SELECT Distinct Category From online_ecommerce;

#Checked Unique State_Code in our Dataset
SELECT Distinct State_Code From online_ecommerce;

#Checked For Null Values in Our Dataset
SELECT * From online_ecommerce 
Where Order_Number is null 
OR State_Code is null
OR Customer_Name is null
OR Order_Date is null
OR Status is null
OR Product is null
OR Category is null
OR Brand is null
OR Cost is null
OR Sales is null
OR Quantity is null
OR Total_Cost is null
OR Total_Sales is null
OR Assigned_Supervisor is null;
```
## Data Analysis Using SQL Queries ##

**1. Which state has generated the highest total sales this year?**
```sql
SELECT State_code, Sum(Total_sales) as Total_Sales From online_ecommerce 
Group By State_code order by Highest_Sales Desc;
```

**2. What are the top 5 products in terms of sales revenue?**
```sql
SELECT Product, Sum(Total_Sales) as Sales_Revenue From online_ecommerce
Group By Product Order By Sales_Revenue Desc Limit 5;
```

**3. How many orders has each customer placed?**
```sql
SELECT Customer_Name, Count(Order_Number) as Orders_Placed From online_ecommerce
Group By Customer_Name Order By Orders_Placed Desc;
```

**4. What percentage of orders are still pending or canceled?**
```sql
SELECT (COUNT(Case When Status IN('Processing','Shipped') Then 1 End)/Count(*)) * 100 as Orders_Percentage
From online_ecommerce;
```

**5. Which product category generates the most revenue?**
```sql
SELECT Category, SUM(Total_Sales) as Total_Revenue From online_ecommerce 
GROUP BY Category Order By Highest_Revenue DESC;
```

**6. How does the sales performance of different brands compare?**
```sql
SELECT Brand, SUM(Total_Sales) as Total_Sales From online_ecommerce
Group By Brand Order By Total_Sales DESC;
```

**7. What is the average order value for each customer?**
```sql
SELECT Customer_Name, ROUND(AVG(Total_Sales),2) as Avg_Order_Value From online_ecommerce 
Group By Customer_Name Order by Avg_Order_Value DESC;
```

**8. What is the total sales contribution of orders assigned to each supervisor?**
```sql
SELECT Assigned_Supervisor, SUM(Total_Sales) as Total_Sales_Contribution From online_ecommerce 
Group By Assigned_Supervisor Order By Total_Sales_Contribution Desc;
```

**9. What is the profit margin (Sales - Cost) for each product?**
```sql
SELECT Product, (SUM(Sales)-SUM(Cost)) as Profit_Margin From online_ecommerce 
Group By Product Order By Profit_Margin Desc;
```

**10. Which Month has contributed Highest in terms of sales?**
```sql
SELECT Date_Format(Orders_Date, '%Y-%m') as Month, Sum(total_sales) as Total_sales From online_ecommerce
Where Orders_Date Between '2020-01-01' AND '2020-05-31'
Group By Month Order By Month Desc;
```
## Findings & Insights ##

- **State-wise Sales Contribution**: Analyzed the dataset to identify the states contributing the most to overall sales.
- **Top Product Analysis**: Evaluated the dataset to determine the top 5 products based on sales revenue.
- **Product Category Insights**: Conducted a product category analysis to understand user purchasing behavior and preferences.
- **Brand Sales Comparison**: Compared sales performance across different brands to identify top-performing brands.
- **Average Order Value (AOV)**: Calculated the average order value to assess customer spending behavior.
- **Monthly Sales Trends**: Conducted a month-wise analysis to identify sales patterns and trends throughout the months.
