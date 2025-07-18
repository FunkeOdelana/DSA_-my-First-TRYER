## SQL Queries & Outputs

`~~~sql

-- 1 Product Category with the Highest Scale

SELECT TOP 1 [Product_Category],
SUM(Sales) AS Total_Sales
FROM KMS_Case
GROUP BY [Product_Category]
ORDER BY Total_Sales DESC;
~~~

**Output/ Visuals**

 {Image} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/commit/3c93dbe38ae6b1182b716ee7454449058ec4e2ed)

~~~sql

---2 Top3_ And Bottom 3_ Regions _By_Sales

SELECT TOP 3 Region,
SUM(Sales) AS Total_Sales
FROM KMS_Case
GROUP BY Region
ORDER BY Total_Sales DESC;

---Bottom 3 Regions--
SELECT TOP 3 Region,
SUM(Sales) AS Total_Sales
FROM KMS_Case
GROUP BY Region
ORDER BY Total_Sales ASC;
~~~
**Output/ Visuals**

{Image2} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q2.PNG)

~~~sql

---3_ Total Appliances _Sale


SELECT Region, Product_Sub_Category, SUM (Sales) AS [Total_Sales]
FROM KMS_Case
WHERE Region = 'Ontario' AND Product_Sub_Category = 'Appliances'
GROUP BY Region, Product_Sub_Category
~~~
**Output/Visuala**

{Image3} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q3.PNG)

~~~sql

---Question 4: Advise the management of KMS on what to do to increase revenue from the bottom 10 customers?--

SELECT TOP 10 [Customer_Name],
SUM(sales) AS Total_Sales
FROM KMS_Case
GROUP BY [Customer_Name]
ORDER BY Total_Sales ASC;
~~~
**Output /Visuals
{Image4} ( https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q4.PNG)


~~~sql

---Question 5: KMS incurred the most shipping cost using which shipping method?--

SELECT Ship_Mode, SUM(Shipping_Cost) AS Total_Shipping_Cost
FROM KMS_Case
GROUP BY Ship_Mode
ORDER BY Total_Shipping_Cost DESC;
~~~
*Output/Visuals
{Image 5} ( https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q5.PNG)

~~~sql


--Question 6? Who are the most valuable customers, and what product or services do they typically purchase?---

WITH CustomerCategorySales AS (
SELECT Customer_Name,Product_Category,
SUM(Sales) AS Category_Sales
FROM KMS_Case
GROUP BY Customer_Name, Product_Category),
-- Step 2: Rank each category per customer (most sales first)
RankedCategories AS (
SELECT *, ROW_NUMBER() OVER (
PARTITION BY Customer_Name
ORDER BY Category_Sales DESC) AS rn
FROM CustomerCategorySales),
-- Step 3: Get total sales per customer (for top 5)
CustomerTotals AS (
SELECT Customer_Name, SUM(Category_Sales) AS Total_Sales
FROM CustomerCategorySales
GROUP BY Customer_Name),
-- Step 4: Top 5 customers by total sales
Top5Customers AS (
SELECT TOP 5 * FROM CustomerTotals
ORDER BY Total_Sales DESC)
-- Step 5: Join to get each top customer's top product category
SELECT 
rc.Customer_Name,
rc.Product_Category AS Top_Product_Category,
rc.Category_Sales,
t5.Total_Sales
FROM RankedCategories rc
JOIN Top5Customers t5 ON rc.Customer_Name = t5.Customer_Name
WHERE rc.rn = 1
ORDER BY t5.Total_Sales DESC;
~~~

**Output Visuals**
{Image 6} ( https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q6.PNG)


~~~sql
--Question 7: Which small business customer had the highest sales?--

SELECT TOP 1 Customer_Name, SUM(Sales) AS Total_Sales
FROM KMS_Case
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY Total_Sales DESC;
~~~

**Output Visuals**
{Image 7} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q7.PNG)


~~~sql

---Question 8: Which corporate customer placed the most number of orders in 2009-2012?-
SELECT TOP 1 Customer_Name, COUNT(DISTINCT Order_ID) AS Number_Of_Orders
FROM KMS_Case
WHERE Customer_Segment = 'Corporate'
GROUP BY Customer_Name
ORDER BY Number_Of_Orders DESC;
~~~
**Output Visuals**
{Image 8} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q8.PNG)

~~~sql

---Question 9:  Which consumer customer was the most profitable one?---
SELECT TOP 1 Customer_Name, SUM(Profit) AS Total_Profit
FROM KMS_Case
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Total_Profit DESC;
~~~
**Output Visuals**
{Image 9} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q9.PNG)

~~~sql

---Question 10: Which consumer returned items, and what segment do they belong to?----
SELECT DISTINCT 
    cs.[Customer_Name],
    cs.[Customer_Segment]
FROM KMS_Case cs
INNER JOIN 
Order_Status os ON cs.[Order_ID] = os.[Order_ID]
WHERE os.[Status] = 'Returned';
~~~

**Output/Visuals**
{Image10} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q1O.PNG)


~~~sql
--Question 11: If the delivery truck is the most economical but the slowest, shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately sent shipping costs based on the Order Priority? 
WITH EvaluatedOrders AS (
SELECT Order_ID, Customer_Name, Order_Priority, Ship_Mode, Shipping_Cost, 
Order_Date, Ship_Date,
DATEDIFF(DAY, Order_Date, Ship_Date) AS Shipping_Delay,
CASE 
WHEN Order_Priority IN ('Low', 'Medium') AND Ship_Mode = 'Express Air' 
THEN 'Unnecessary Express'
WHEN Order_Priority IN ('High', 'Critical') AND Ship_Mode = 'Delivery Truck' 
THEN 'Slow for Urgent Order'
ELSE 'Appropriate' END AS Shipping_Evaluation
FROM KMS_Case)

SELECT Shipping_Evaluation,
COUNT(DISTINCT Order_ID) AS Order_Count,
SUM(Shipping_Cost) AS Total_Shipping_Cost,
AVG(Shipping_Cost) AS Average_Shipping_Cost,
AVG(Shipping_Delay) AS Average_Shipping_Delay
FROM EvaluatedOrders
GROUP BY Shipping_Evaluation
ORDER BY Order_Count DESC;
~~~
**Output Visuals**
{Image 11} (https://github.com/FunkeOdelana/DSA_-my-First-TRYER/blob/main/Q11.PNG)
~~~
























