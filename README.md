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
{Image4} (








