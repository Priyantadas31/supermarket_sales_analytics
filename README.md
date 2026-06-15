# supermarket_sales_analytics
An end-to-end SQL and Business Intelligence project to study data quality data at the super market level and line of business revenue in Databricks. 

1.Project Overview
======================
This project analyzes supermarket sales data to identify revenue trends, customer behavior, product performance, profitability, and branch performance using SQL.

2.Dataset Exploration
========================
**view data**
SELECT * FROM catalog01.default.super_market_analysis;

3.Data Cleaning
 ====================

---Check Missing Values--
SELECT
SUM(CASE WHEN Rating IS NULL THEN 1 END) AS missing_rating,
SUM(CASE WHEN gender IS NULL THEN 1 END) AS missing_gender,
SUM(CASE WHEN  city IS NULL THEN 1 END) AS missing_city
FROM catalog01.default.super_market_analysis;

3. Handle Missing Values
===================================
UPDATE catalog01.default.super_market_analysis
SET Rating = 0
WHERE Rating is NULL;

UPDATE catalog01.default.super_market_analysis
SET Gender = 'Unknown'
WHERE Gender is NULL;
UPDATE catalog01.default.super_market_analysis
SET City = 'Unknown'
WHERE City is NULL;

4. Exploratory Data Analysis
==========================
---Revenue by Product Line----
SELECT `Product line`,MAX(Payment) FROM catalog01.default.super_market_analysis GROUP BY `Product line`;
SELECT MIN(Payment) FROM catalog01.default.super_market_analysis;
SELECT MAX(City) FROM catalog01.default.super_market_analysis;
SELECT MAX(`Product line`)  FROM catalog01.default.super_market_analysis WHERE Rating< 9;
SELECT  SUM(SALES) FROM catalog01.default.super_market_analysis GROUP BY Product line;

-----Revenue by Customer Type------
SELECT `Customer type`, ROUND(SUM(SALES),2) FROM catalog01.default.super_market_analysis GROUP BY `Customer type`;

----Profit by City Payment Method Usage-----
SELECT City, ROUND(SUM(`gross income`),2) AS total FROM catalog01.default.super_market_analysis
GROUP BY City
ORDER BY total DESC;

-----Average Sale Value------
SELECT AVG (Sales) FROM catalog01.default.super_market_analysis;

-----Payment Method Usage------
SELECT Payment, COUNT(*)
FROM catalog01.default.super_market_analysis
GROUP BY Payment;

5. Business Analysis
=============================
----Product Line Profitability-----
SELECT `Product line`,
  ROUND(SUM(Sales),2) AS Total_revenue,
  ROUND(SUM(`gross income`), 2) AS TOTAL_PROFIT,
  ROUND((SUM(`gross income`)/SUM(Sales))*100, 2) AS Profit_Margin
FROM catalog01.default.super_market_analysis
GROUP BY `Product line`
ORDER BY Profit_Margin DESC;


-----Branch Performance----
SELECT Branch,
ROUND(SUM(Sales),2) AS daily_sales
FROM catalog01.default.super_market_analysis
GROUP BY Branch
ORDER BY daily_sales DESC;

6.View Creation
===============================

CREATE OR REPLACE VIEW catalog01.default.vwsuper_market
AS
SELECT `Product line` AS Name,
SUM(COALESCE(`Unit price`* Quantity,0)) AS Totalsales,
COUNT(*) AS Ttaltransactions
FROM catalog01.default.super_market_analysis
GROUP BY `Product line`;
SELECT * FROM catalog01.default.vwsuper_market;
SELECT * FROM catalog01.default.super_market_analysis;

7. Key Findings
========================
--Product lines contribute differently to overall revenue and profit.
--Customer types generate different sales volumes.
--Certain cities contribute more profit than others.
-- Electronic payment methods are widely used.
--Branch performance varies significantly.
--Profit margins differ across product categories.
8. Conclusion
===============
The analysis identified the most profitable product lines, highest-performing branches, customer purchasing patterns, and city-level profitability. These insights can help management optimize inventory, marketing, and operational decisions.
