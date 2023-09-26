# Sales-Analysis
sales overview dashboard with one page with works as a dashboard and overview, with two other pages focused on combining tables for necessary details and visualizations to show sales over time, per customer and per product.
![dashboard-2](https://github.com/tamseel101/Sales-Analysis/assets/78289413/7c87772a-8ac1-48d4-829e-0d8a610e8e23)

### User Stories

The business requested an executive sales report for sales managers in this data analyst project. I defined user stories based on the business's proposition to ensure project delivery and adherence to acceptance criteria.

| # | As a (role) | I want (request / demand) | So that I (user value) | Acceptance Criteria |
|---|-------------|---------------------------|------------------------|---------------------|
| 1 | Sales Manager | To get a dashboard overview of internet sales | Can follow better which customers and products sell the best | A Power BI dashboard which updates data once a day |
| 2 | Sales Representative | A detailed overview of Internet Sales per Customers | Can follow up with my customers who buy the most and whom we can sell more to | A Power BI dashboard which allows me to filter data for each customer |
| 3 | Sales Representative | A detailed overview of Internet Sales per Product | Can follow up my Products that sell the most | A Power BI dashboard which allows me to filter data for each Product |
| 4 | Sales Manager | A dashboard overview of internet sales | Follow sales over time against budget | A Power BI dashboard with graphs comparing against budget |


```markdown
## Data Cleansing using SQL

Below are the SQL statements for cleansing and transforming given data from https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms
```

### Calendar:

```
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

```
-- Cleansed Customers Table --
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

```
-- Cleansed Products Table --
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
  [AdventureWorksDW].[dbo].[DimProduct] as p
  LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey
```
```
Data Model
Below is a screenshot of the data model after cleansed and prepared tables were read into Power BI.

This data model also shows how Budget has been connected to Sales and other necessary tables.
```
![datamodel-1](https://github.com/tamseel101/Sales-Analysis/assets/78289413/97badfca-0124-4998-a642-a2d4edc668a1)

