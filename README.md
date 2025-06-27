# 🛒 UK E-Commerce SQL Analysis Project

This project analyzes a real-world e-commerce transactional dataset using SQL. It is ideal for beginners learning SQL for data analysis, with a focus on filtering, aggregation, joins, subqueries, views, and performance optimization using indexes.

---

## 📁 Dataset Overview

The dataset (`UK E-commerce`) contains records of customer purchases for a UK-based online retail store, with the following key columns:

- `InvoiceNo`: Transaction invoice number  
- `StockCode`, `Description`: Product identifiers and descriptions  
- `Quantity`: Number of items purchased  
- `InvoiceDate`: Date and time of purchase  
- `UnitPrice`: Price per item  
- `CustomerID`: Unique ID of the customer  
- `Country`: Customer's country  

---

## 🧹 Data Cleaning

To ensure analysis accuracy, invalid data entries were removed. This includes records with negative or zero quantities or prices, and null/missing descriptions or customer IDs.


SELECT * 
FROM [UK E-commerce]
WHERE 
  Quantity > 0
  AND UnitPrice > 0
  AND CustomerID IS NOT NULL
  AND Description IS NOT NULL

AND TRIM(Description) != '';

---

## 🔍 SQL Analysis
1. Top-Selling Products

SELECT Description, SUM(Quantity) AS TotalSold
FROM UK_Retail
GROUP BY Description
ORDER BY TotalSold DESC;

2. Sales After a Specific Date

SELECT * 
FROM UK_Retail
WHERE InvoiceDate >= '2011-09-01'
ORDER BY InvoiceDate;

##🔗 Joins (Customer Info)
A mock CustomerInfo table was created to practice JOIN operations.

CREATE TABLE CustomerInfo AS
SELECT DISTINCT CustomerID, Country
FROM UK_Retail;
3. INNER JOIN - Country by Invoice

SELECT r.InvoiceNo, r.CustomerID, c.Country, r.Quantity, r.UnitPrice
FROM UK_Retail r
INNER JOIN CustomerInfo c ON r.CustomerID = c.CustomerID;

4. LEFT JOIN - Invoices with/without Country Info

SELECT r.InvoiceNo, r.CustomerID, c.Country
FROM UK_Retail r
LEFT JOIN CustomerInfo c ON r.CustomerID = c.CustomerID;

##🧠 Subqueries & Aggregates
5. Customers Spending More Than Average


SELECT CustomerID, SUM(Quantity * UnitPrice) AS TotalSpent
FROM UK_Retail
GROUP BY CustomerID
HAVING TotalSpent > (
  SELECT AVG(Quantity * UnitPrice)
  FROM UK_Retail
);

6. Monthly Revenue Trend

SELECT strftime('%Y-%m', InvoiceDate) AS Month,
       SUM(Quantity * UnitPrice) AS Revenue
FROM UK_Retail
GROUP BY Month
ORDER BY Month;

7. Average Quantity per Invoice

SELECT AVG(Quantity) AS AvgQuantity
FROM UK_Retail;

##📊 Views & Optimization
8. View for Top-Selling Products

CREATE VIEW Top_Products AS
SELECT Description, SUM(Quantity) AS TotalSold
FROM UK_Retail
GROUP BY Description
ORDER BY TotalSold DESC
LIMIT 10;

SELECT * FROM Top_Products;

9. Index Creation for Performance

CREATE INDEX idx_customer_id ON [UK E-commerce](CustomerID);
CREATE INDEX idx_invoice_date ON [UK E-commerce](InvoiceDate);

##🌍 Country-Based Insights
10. Most Active Countries by Customer Count

SELECT Country, COUNT(DISTINCT CustomerID) AS TotalCustomers
FROM UK_Retail
GROUP BY Country
ORDER BY TotalCustomers DESC;

11. Revenue per Product per Country

SELECT Country, Description, SUM(Quantity * UnitPrice) AS Revenue
FROM UK_Retail
GROUP BY Country, Description
ORDER BY Revenue DESC;

---

##🧠 Skills Covered
✅ SELECT, WHERE, GROUP BY, ORDER BY

✅ Joins: INNER JOIN, LEFT JOIN

✅ Subqueries & Aggregates (SUM, AVG)

✅ Views for reusability

✅ Indexes for performance

---

##🛠 Tools Used

SQLiteStudio – to query the database
SQL (SQLite dialect)

Dataset :- (https://www.kaggle.com/datasets/carrie1/ecommerce-data/data)

