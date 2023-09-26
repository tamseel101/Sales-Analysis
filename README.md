# Sales-Analysis

##User Stories

The business request for this data analyst project was an executive sales report for sales managers. Based on the proposition that was made by the business, the following user stories were defined to fulfill delivery and ensure that acceptance criteria were maintained throughout the project.

| # | As a (role) | I want (request / demand) | So that I (user value) | Acceptance Criteria |
|---|-------------|---------------------------|------------------------|---------------------|
| 1 | Sales Manager | To get a dashboard overview of internet sales | Can follow better which customers and products sell the best | A Power BI dashboard which updates data once a day |
| 2 | Sales Representative | A detailed overview of Internet Sales per Customers | Can follow up my customers who buy the most and whom we can sell more to | A Power BI dashboard which allows me to filter data for each customer |
| 3 | Sales Representative | A detailed overview of Internet Sales per Product | Can follow up my Products that sell the most | A Power BI dashboard which allows me to filter data for each Product |
| 4 | Sales Manager | A dashboard overview of internet sales | Follow sales over time against budget | A Power BI dashboard with graphs and KPIs comparing against budget |


```markdown
## Data Cleansing & Transformation (SQL)

The following tables were extracted using SQL to create the necessary data model for doing analysis and fulfilling the business needs defined in the user stories.

One data source (sales budgets) was provided in Excel format and connected in the data model later in the process.

Below are the SQL statements for cleansing and transforming necessary data.

### Calendar:

```SQL
-- Cleansed DIM_Date Table --
SELECT 
  [DateKey], 
  [FullDateAlternateKey] AS Date, 
  [EnglishDayNameOfWeek] AS Day, 
  [EnglishMonthName] AS Month, 
  Left([EnglishMonthName], 3) AS MonthShort,   -- Useful for front-end date navigation and front-end graphs.
  [MonthNumberOfYear] AS MonthNo, 
  [CalendarQuarter] AS Quarter, 
  [CalendarYear] AS Year
FROM 
 [AdventureWorksDW].[dbo].[DimDate]
WHERE 
  CalendarYear >= 2019
```

### Customers:

```SQL
-- Cleansed DIM_Customers Table --
SELECT 
  c.customerkey AS CustomerKey, 
  c.firstname AS [First Name], 
  c.lastname AS [Last Name], 
  c.firstname + ' ' + lastname AS [Full Name], 
  CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
  c.datefirstpurchase AS DateFirstPurchase, 
  g.city AS [Customer City]
FROM 
  [AdventureWorksDW2019].[dbo].[DimCustomer] as c
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
ORDER BY 
  CustomerKey ASC
```

### Products:

```SQL
-- Cleansed DIM_Products Table --
SELECT 
  p.[ProductKey], 
  p.[ProductAlternateKey] AS ProductItemCode, 
  p.[EnglishProductName] AS [Product Name], 
  ps.EnglishProductSubcategoryName AS [Sub Category],
  pc.EnglishProductCategoryName AS [Product Category],
  p.[Color] AS [Product Color], 
  p.[Size] AS [Product Size], 
  p.[ProductLine] AS [Product Line], 
  p.[ModelName] AS [Product Model Name], 
  p.[EnglishDescription] AS [Product Description], 
  ISNULL (p.Status, 'Outdated') AS [Product Status]
FROM 
  [AdventureWorksDW2019].[dbo].[DimProduct] as p
  LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey
```
```
Data Model
Below is a screenshot of the data model after cleansed and prepared tables were read into Power BI.

This data model also shows how FACT_Budget has been connected to FACT_InternetSales and other necessary DIM tables.

![dashboard-2](https://github.com/tamseel101/Sales-Analysis/assets/78289413/7c87772a-8ac1-48d4-829e-0d8a610e8e23)
![datamodel-1](https://github.com/tamseel101/Sales-Analysis/assets/78289413/24c452c9-1b9f-4fad-9301-c903eb6223ca)
