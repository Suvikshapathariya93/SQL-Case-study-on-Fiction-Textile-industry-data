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

**3. Top-Selling Categories**
- Objective: Which product categories generate the most revenue?
```sql
SELECT P.Category, SUM(S.TotalAmount) AS TotalSales
FROM Products P
JOIN Sales S ON P.ProductID = S.ProductID
GROUP BY P.Category
ORDER BY TotalSales DESC;
```


