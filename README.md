# SQL case study on fiction Textile industry data

## Contents 📖
- [Introduction](#introduction)
- [Problem Statement](#problem-statement)
- [Database Schema](#database-schema)
- [Case study: Questions and Answers](#case-study-questions-and-answers)
- [Additional Queries and Answers](#additional-queries-and-answers)
- [Feature Highlights](#feature-highlights)
- [Key Insights](#key-insights)
- [Use cases](#use-cases)

## Introduction 
This project presents a fictional case study analyzing the operations of a textile company, FabricFlow, using SQL. The dataset includes information about products, sales, inventory, suppliers, and purchases. The goal is to derive actionable insights to improve operational efficiency, inventory management, and supplier performance.

**🛠️ Tool Used** : MySQL

## Problem Statement
A fictional textile company, FabricFlow, manufactures and distributes textile products such as fabrics, clothing, and accessories. The company wants to analyze its operations, sales, inventory, and supplier performance to improve efficiency and profitability.

The case study revolves around five key datasets:
- Products
- Sales
- Inventory
- Suppliers
- Purchases

## Database Schema
**1. Table : Products**

| **Column Name** | **Data Type** | **Description**                          |
|-----------------|---------------|------------------------------------------|
| ProductID     | INT           | Unique identifier for each product     |
| ProductName   | VARCHAR       | Name of the product                    |
| Category        | VARCHAR       | Product category (e.g. Fabrics, Clothing)      |
| Price | DECIMAL | Price of the product |

**2. Table : Sales**

| **Column Name** | **Data Type** | **Description**                          |
|-----------------|---------------|------------------------------------------|
| SaleID | INT | Unique identifier for each sale |
| ProductID     | INT           | ID of the product sale     |
| SaleDate   | DATE       | Date of the sale                    |
| Quantity       | INT       | Quantity sold      |
| TotalAmount | DECIMAL | Total amount of the sale(Quantity*Price) |

**3. Table : Inventory**

| **Column Name** | **Data Type** | **Description**                          |
|-----------------|---------------|------------------------------------------|
| InventoryID     | INT           | Unique identifier for inventory record     |
| ProductID   | INT       | ID of the product                    |
| StockQuantity        | INT       | Quantity available in stock      |
| LastUpdated | DATE | Last updated date of stock |

**4. Table : Suppliers**

| **Column Name** | **Data Type** | **Description**                          |
|-----------------|---------------|------------------------------------------|
| SupplierID     | INT           | Unique identifier for each supplier     |
| SupplierName   | VARCHAR       | Name of the supplier                    |
| Rating        | INT       | Supply rating(1 to 5)      |

**4. Table : Purchases**

| **Column Name** | **Data Type** | **Description**                          |
|-----------------|---------------|------------------------------------------|
| PurchaseID     | INT           | Unique identifier for each purchase     |
| SupplierID  | INT       | ID of the supplier                    |
| ProductID        | INT       | ID of the product purchase      |
| PurchasesDate | DATE | Date of the purchase |
| Quantity | INT | Quantity Purchased |
| TotalCost | DECIMAL | Total of the purchase |

## Case study: Questions and Answers

**1. Total Sales by Product**
- Objective: What are the total sales and quantities sold for each product?
```sql
SELECT P.ProductID, P.ProductName,
    SUM(S.Quantity) AS TotalQuantitySold,
    SUM(S.TotalAmount) AS TotalSales
FROM Products P
JOIN Sales S ON P.ProductID = S.ProductID
GROUP BY P.ProductID, P.ProductName
ORDER BY TotalSales DESC;
```
- **Joins**: 
  - `Products` is joined with `Sales` using `ProductID`.
- **Aggregations**:
  - `SUM(S.Quantity)` calculates the **total quantity sold** for each product.
  - `SUM(S.TotalAmount)` calculates the **total sales revenue** for each product.
- **Grouping**: Results are grouped by `ProductID` and `ProductName`.
- **Ordering**: Data is sorted in descending order of `TotalSales` (highest sales first).

**2. Monthly Sales Trend**
- Objective: What is the monthly sales trend for the company?
```sql 
SELECT DATE_FORMAT(S.SaleDate, '%Y-%m') AS Month, SUM(S.TotalAmount) AS TotalSales
FROM Sales S
GROUP BY Month
ORDER BY Month;
```
- **Date Formatting**: 
  - `DATE_FORMAT(S.SaleDate, '%Y-%m')` extracts the year and month from the sale date in `YYYY-MM` format.
- **Aggregation**:
  - `SUM(S.TotalAmount)` computes the total sales revenue for each month.
- **Grouping**: Results are grouped by `Month`.
- **Ordering**: Data is sorted in chronological order by month.

**3. Top-Selling Categories**
- Objective: Which product categories generate the most revenue?
```sql
SELECT P.Category, SUM(S.TotalAmount) AS TotalSales
FROM Products P
JOIN Sales S ON P.ProductID = S.ProductID
GROUP BY P.Category
ORDER BY TotalSales DESC;
```
- **Joins**: 
  - `Products` is joined with `Sales` using `ProductID`.
- **Aggregation**:
  - `SUM(S.TotalAmount)` calculates the total sales revenue for each category.
- **Grouping**: Results are grouped by `P.Category`.
- **Ordering**: Data is sorted in descending order of `TotalSales` (highest-grossing categories first).

**4. Average Stock by Category**
- Objective: What is the average stock available for each product category?
```sql
SELECT P.Category, AVG(I.StockQuantity) AS AvgStock
FROM Products P
JOIN Inventory I ON P.ProductID = I.ProductID
GROUP BY P.Category
ORDER BY AvgStock DESC;
```
- **Joins**: 
  - `Products` is joined with `Inventory` using `ProductID`.
- **Aggregation**:
  - `AVG(I.StockQuantity)` computes the average stock quantity for each category.
- **Grouping**: Results are grouped by `P.Category`.
- **Ordering**: Data is sorted in descending order of `AvgStock` (categories with the highest average stock first).

**5. High-Performing Suppliers**
- Objective: Which suppliers have the highest total purchase volume?
```sql
SELECT SU.SupplierID, SU.SupplierName, SUM(PU.Quantity) AS TotalQuantityPurchased, SUM(PU.TotalCost) AS TotalCost
FROM Suppliers SU
JOIN Purchases PU ON SU.SupplierID = PU.SupplierID
GROUP BY SU.SupplierID, SU.SupplierName
ORDER BY TotalQuantityPurchased DESC;
```

**6. Cumulative Sales**
- Objective: What are the cumulative sales for the company over time?
```sql
SELECT S.SaleDate, SUM(S.TotalAmount) OVER (ORDER BY S.SaleDate) AS CumulativeSales
FROM Sales S
ORDER BY S.SaleDate;
```
- **Window Function**:
  - `SUM(S.TotalAmount) OVER (ORDER BY S.SaleDate)` computes the running total of sales, ordered by sale date.
- **Ordering**: Results are sorted by `S.SaleDate` to show the cumulative sales in chronological order.

**7. Top 3 Products by Revenue**
- Objective: Which are the top 3 products by revenue?
```sql
SELECT P.ProductName, SUM(S.TotalAmount) AS TotalRevenue
FROM Products P
JOIN Sales S ON P.ProductID = S.ProductID
GROUP BY P.ProductName
ORDER BY TotalRevenue DESC
LIMIT 3;
```
- **Joins**: 
  - `Products` is joined with `Sales` using `ProductID`.
- **Aggregation**:
  - `SUM(S.TotalAmount)` calculates the total revenue for each product.
- **Grouping**: Results are grouped by `P.ProductName`.
- **Ordering**: Data is sorted in descending order of `TotalRevenue` (highest revenue first).
- **Limiting**: The `LIMIT 3` clause restricts the output to the top 3 products.

**8. Sales vs. Stock Analysis**
- Objective: What is the sales-to-stock ratio for each product?
```sql
SELECT P.ProductName, SUM(S.Quantity) AS TotalSold, AVG(I.StockQuantity) AS AvgStock, SUM(S.Quantity) * 1.0 / AVG(I.StockQuantity) AS SalesToStockRatio
FROM Products P
JOIN Sales S ON P.ProductID = S.ProductID
JOIN Inventory I ON P.ProductID = I.ProductID
GROUP BY P.ProductName
ORDER BY SalesToStockRatio DESC;
```

**9. Supplier Rankings**
- Objective: Rank suppliers by the total value of purchases.
```sql
SELECT SupplierID, SupplierName, SUM(TotalCost) AS TotalCost,
    RANK() OVER (ORDER BY SUM(TotalCost) DESC) AS SupplierRank
FROM Suppliers SU
JOIN Purchases PU ON SU.SupplierID = PU.SupplierID
GROUP BY SupplierID, SupplierName;
```

## Additional Queries and Answers

**1. Products with No Sales**
- Objective: Which products have not been sold yet?
```sql
SELECT P.ProductID, P.ProductName
FROM Products P
LEFT JOIN Sales S ON P.ProductID = S.ProductID
WHERE S.SaleID IS NULL;
```

**2. Average Price by Product Category**
- Objective: What is the average price for each product category?
```sql
SELECT Category, AVG(Price) AS AveragePrice
FROM Products
GROUP BY Category
ORDER BY AveragePrice DESC;
```

**3. Inventory Restocking Alert**
- Objective: Which products have stock below a threshold of 100 units?
```sql
SELECT P.ProductID, P.ProductName, I.StockQuantity
FROM Products P
JOIN Inventory I ON P.ProductID = I.ProductID
WHERE I.StockQuantity < 100;
```

**4. Supplier Contribution to Each Product**
- Objective: What is the total quantity purchased from each supplier for each product?
```sql
SELECT PU.ProductID, P.ProductName, PU.SupplierID, SU.SupplierName, SUM(PU.Quantity) AS TotalQuantitySupplied
FROM Purchases PU
JOIN Products P ON PU.ProductID = P.ProductID
JOIN Suppliers SU ON PU.SupplierID = SU.SupplierID
GROUP BY PU.ProductID, P.ProductName, PU.SupplierID, SU.SupplierName
ORDER BY P.ProductName, TotalQuantitySupplied DESC;
```

**5. Most Profitable Products**
- Objective: Which products generate the highest profit based on sales and cost data?
```sql
WITH ProductCost AS (
    SELECT P.ProductID, P.ProductName, AVG(PU.TotalCost / PU.Quantity) AS AverageCost
    FROM Products P
    JOIN Purchases PU ON P.ProductID = PU.ProductID
    GROUP BY P.ProductID, P.ProductName)
SELECT P.ProductName, SUM(S.TotalAmount) AS TotalRevenue, SUM(S.Quantity * PC.AverageCost) AS TotalCost, SUM(S.TotalAmount) - SUM(S.Quantity * PC.AverageCost) AS Profit
FROM Sales S
JOIN Products P ON S.ProductID = P.ProductID
JOIN ProductCost PC ON P.ProductID = PC.ProductID
GROUP BY P.ProductName
ORDER BY Profit DESC;
```

**6. Yearly Sales Growth**
- Objective: How has the total revenue changed year over year?
```sql
WITH YearlySales AS (
    SELECT YEAR(S.SaleDate) AS SaleYear, SUM(S.TotalAmount) AS TotalSales
    FROM Sales S
    GROUP BY YEAR(S.SaleDate))
SELECT SaleYear, TotalSales, LAG(TotalSales) OVER (ORDER BY SaleYear) AS PreviousYearSales,
    (TotalSales - LAG(TotalSales) OVER (ORDER BY SaleYear)) AS SalesGrowth,
    ROUND(((TotalSales - LAG(TotalSales) OVER (ORDER BY SaleYear)) * 100.0 / LAG(TotalSales) OVER (ORDER BY SaleYear)), 2) AS GrowthPercentage
FROM YearlySales;
```

**7. Products with Highest Stock Turnover**
- Objective: Which products have the highest stock turnover ratio?
```sql
SELECT P.ProductID, P.ProductName, SUM(S.Quantity) AS TotalSold, AVG(I.StockQuantity) AS AvgStock,
    SUM(S.Quantity) * 1.0 / AVG(I.StockQuantity) AS StockTurnoverRatio
FROM Products P
JOIN Sales S ON P.ProductID = S.ProductID
JOIN Inventory I ON P.ProductID = I.ProductID
GROUP BY P.ProductID, P.ProductName
ORDER BY StockTurnoverRatio DESC;
```

**8. Quarterly Supplier Ranking**
- Objective: Rank suppliers by the total value of purchases on a quarterly basis.
```sql
SELECT QUARTER(PU.PurchaseDate) AS Quarter, SU.SupplierID, SU.SupplierName, SUM(PU.TotalCost) AS TotalCost,
    RANK() OVER (PARTITION BY QUARTER(PU.PurchaseDate) ORDER BY SUM(PU.TotalCost) DESC) AS SupplierRank
FROM Purchases PU
JOIN Suppliers SU ON PU.SupplierID = SU.SupplierID
GROUP BY Quarter, SU.SupplierID, SU.SupplierName;
```

## Feature Highlights
1. Basic SQL: Aggregations, filtering, and joins.
2. Window Functions: RANK, AVG, cumulative totals, lead & lag function and percentage calculations.
3. CTEs: Breaking down complex queries for readability and reuse.

## Key Insights
**1. Sales Performance**
  - Top Products: Cotton Fabric and Denim Jacket are the top-selling products by both revenue and quantity.
  - Category Revenue: Fabrics outperform other categories in revenue generation.
  - Seasonal Trends: Sales tend to peak in specific months, revealing opportunities for targeted marketing.
    
**2. Inventory Management**
  - Restocking Needs: Certain products, like Denim Jacket and Wool Fabric, require immediate restocking due to low stock levels.
  - Stock Turnover: High-demand products exhibit faster stock turnover, indicating efficient sales performance.
  - Average Stock Levels: Fabrics maintain higher average stock levels compared to Clothing.
    
**3. Supplier Analysis**
  - Top Suppliers: Textile Mills contributes the most to product supply, followed by Fabric Supplies.
  - Supplier Ratings: High-rated suppliers tend to deliver more consistent and cost-effective purchases.
  - Supplier Performance: Ranking suppliers by quarterly purchases helps identify consistent performers.
    
**4. Profitability**
  - Product Margins: Cotton Fabric has the highest profitability due to its low cost and high sales revenue.
  - Cost Management: Effective supplier selection and purchase planning reduce overall costs.
    
**5. Business Growth**
  - Yearly Growth: The company exhibits steady sales growth year-over-year, with a 20% increase in revenue in the last year.
  - Cumulative Sales: A clear upward trend in cumulative sales reflects strong operational performance.
    
**6. Operational Efficiency**
  - Sales-to-Stock Ratio: Products like Cotton Fabric demonstrate an optimal balance between inventory and sales.
  - Inventory Turnover: Efficient inventory turnover rates suggest strong product demand and sales alignment.

## Use cases
- Sales Performance Tracking: Identify top-performing products, analyze seasonal trends, and monitor revenue growth.
- Inventory Optimization: Detect low-stock items, calculate stock turnover rates, and ensure efficient inventory management.
- Supplier Evaluation: Rank suppliers based on cost, quantity supplied, and performance over time.
- Profitability Analysis: Determine product margins by comparing sales revenue and purchase costs.
- Operational Insights: Evaluate sales-to-stock ratios, identify restocking needs, and improve stock efficiency.
- Yearly Growth Monitoring: Assess revenue growth trends and plan for future sales targets.
- Cost Management: Optimize purchase decisions by analyzing supplier contributions and purchase trends.

