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
| Product ID     | INT           | Unique identifier for each product     |
| ProductName   | VARCHAR       | Name of the product                    |
| Category        | VARCHAR       | Product category (e.g. Fabrics, Clothing)      |
| Price | DECIMAL | Price of the product |

