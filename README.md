## SuperMarket Sales Analysis

## Table of Content
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings](#findings)
- [Recommendations](#recommendations)


### Project Overview 

This data analysis project aims to provide insights into the sales performance of an online retail over the past years. By analyzing various aspects of the supermarket dataset, we seek to identify trends, make data-driven recommendation, and gain a deeper understanding of the company's performance.

### Data Source

Sales Data: The primary dataset used for this analysis is the "Online_Sales_Data.xlsx" file which contains detailed information about each sales transaction.

## Tools
- Excel (This is used for data cleaning)
- Microsoft SQL server (This is used for data analysis)

## Data Cleaning and Preparation
In the initial preparation phase, I performed the following tasks:

- Data loading and inspection
- Handling missing values.
- Data cleaning and formatting.
- Ensured Data Integrity.

## Exploratory Data Analysis
- EDA involves exploring the datasets to find out and answer key questions such as :
  
1. calculate the rolling average profit for each customer over the past 3 orders.
2. find the customers who have made purchases in at least 3 different states.
3. calculate the year-over-year growth in sales for each category.
4. find the customers who have placed orders on consecutive days.
5. find the products that have never been ordered.
6. customers who have placed orders on all weekdays.
13. calculate the average profit margin for each sub-category.
14. customers who have placed the highest number of orders in each region.
15. products that were ordered more than once on the same day.
16. calculate the total profit for each quarter.
17. customers who have never placed an order in the 'Technology' category.
18. find the average age of customers in each segment.
19. find the products that have had a decrease in sales compared to the previous year.
20. calculate the cumulative profit for each customer, ordered by their total profit in descending order.
21. identify the products with the highest and lowest profit margins within each category.
22. find the customers who have the highest total sales in each region.
23. calculate the median order value for each sub-category.
24. categorize customers based on their total purchase amount into three groups: 'High Spenders', 'Medium Spenders', and 'Low Spenders'. Consider the following criteria:
'High Spenders' have a total purchase amount greater than $1000.
'Medium Spenders' have a total purchase amount between $500 and $1000 (inclusive).
'Low Spenders' have a total purchase amount less than $500.
25. analyze customer purchasing behavior over time. Categorize customers into 'Steady Buyers', 'Occasional Buyers', and 'One-Time Buyers' based on the following criteria:
'Steady Buyers' are customers who have placed orders in at least three different months.
'Occasional Buyers' are customers who have placed orders in two different months.
'One-Time Buyers' are customers who have placed orders in only one month.
26. perform cohort analysis on customer retention. Determine the percentage of customers who made repeat purchases in the following months after their initial purchase month. The output should display the initial purchase month and the retention rate for each subsequent month.
27. calculate the average order processing time for each shipping mode. Define the order processing time as the difference in days between the order date and the ship date. Use a CASE statement to categorize orders into 'Fast Processing' (less than 2 days), 'Normal Processing' (2 to 5 days), and 'Slow Processing' (more than 5 days).


## Data Analysis
#### SELECT STATEMENT
#### AGGREGATE FUNCTION
#### CASE FUNCTION
#### SUBQUERY
#### CTE
#### Windows Functions
#### Date Functions

- Aggregate Function
```` sql
SELECT segment, COUNT(customer_id) AS Segment_Count
FROM dbo.customer
GROUP BY segment
ORDER BY 2 DESC;
````
- DATE FUNCTION
```` sql
SELECT DATEPART(year, order_date) as Order_Year, SUM(profit) as SumProfit
FROM dbo.sales
GROUP BY  DATEPART(year, order_date)
ORDER BY  2 DESC;
````

- CTE and WINDOWS FUNCTION
````  sql
WITH Sales1 as 
		( SELECT customer_id, order_date, profit, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date
		  DESC)as row_num
		  FROM dbo.sales)

SELECT  customer_id, order_date, profit,
AVG(profit) OVER (PARTITION BY customer_id ORDER BY order_date  ROWS BETWEEN 2 PRECEDING
 AND CURRENT ROW) as rn
FROM Sales1
WHERE row_num <=3;
````

## Findings 
1. Customer TC- 20980 has shown an increasing trend in profit over the past three orders, however, The rolling average profit across all customers has decreased over the period analyzed with just 7 customers crossing the 1000+ threshold.
2. There was no customer that made purchases in the at least 3 different states
3. 2016 has thehighest YoT Growth in across two categories , "Technology" and "Office Supplies"
![b2ebf6a8b3e46acf6c593aa45079511](https://github.com/CherryChristie/sales-transaction-analysis/assets/148567375/6f878ca2-d0c1-4e29-9eca-a6e1dcfeacf2)

4. We identified just 29 customers out of 700+ customers that have placed orders in consecutive days.
![8e3acccdd252abc38c66b1e5b874257](https://github.com/CherryChristie/sales-transaction-analysis/assets/148567375/54e97027-0b71-4f34-b8c7-47d37f6429dc)

5. We were not able to identify any product that has never been ordered, therefore it is safe to say to all product have been previously ordered by the customers.

6. The average profit marging using the profit divvided by sales multiplied by 100 formula resulted to 12.

7. Out of 793 customers in our database, 781 has ordered product during weekdays, therefore, weekdays are presumablely the best time for sales

8. There are four regions in our dataset, the highest order amongst them is from the east region with a total of 37, from the customer with an id:WB-21850, while the highest orders placed in other regions from the rest three customers are 34 orders.

9. Out of 1000+ products , 32 were ordered more than once on the same day.

10.  find the products that have had a decrease in sales compared to the previous year. Out of 1874 products over 1200 proucts were identified to having a decrease in sales compared to the previous year with this product OFF-BI-10003527	Fellowes PB500 Electric Punch Plastic Comb Binding Machine with Manual Bind having the highest sales different of -13472 between the analyzed periods.

11. Customer TC-20980 has the highest cumulative profit, indicating a strong purchasing history, we also identified that Certain customers have a surprisingly low or negative cumulative profit, which could indicate returns or other issues. We also identfied that a small number of customers are responsible for a large portion of profits, which could suggest a dependency on key accounts.

12. the analysis to find the product with the highest and lowest profit margin in each category helps to identify products are performing well in terms of profitability and which are not, within their respective categories. And the prodocuts with the result is as follows: Furniture category , the product with the highest profit margin is 	FUR-BO-10001567	while the lowest profit margin is FUR-FU-10002937, Office Supplies Category the product with the highest profit margin	OFF-AP-10001634 while the lowest profit margin	OFF-FA-10000490, in  the Technology category,	the product with the highest profit margin TEC-MA-10000418	while the lowest profit margin TEC-PH-10002807.
![49a961be889d57d66471a6d34ba3443](https://github.com/CherryChristie/sales-transaction-analysis/assets/148567375/d49cc56b-ae23-45df-bc67-61afee84e1ce)

13. To determine the High Spenders, Medium Spender and  Low Spenders, we used a case stayement to classify that andThe query results show that the majority of customers are categorized as 'High Spenders', indicating that they have spent $1000 or more in total purchases. This indicates a healthy customer base with significant spending. It may reflect successful sales strategies, effective marketing, or a strong product-market fit.
![0a3bfff6dfe278f8955c3c2d7f35372](https://github.com/CherryChristie/sales-transaction-analysis/assets/148567375/4f2f4f6f-ff80-4157-ba4d-465e73616998)


## Recommendations
Based  on the analysis we recommend the following:
- Customer Segmentation and Personalization:

Focus on customer TC-20980 and similar customers exhibiting increasing profit trends to understand what drives their purchases and replicate this success across other accounts.
Develop targeted marketing campaigns for the 7 customers who have crossed the 1000+ profit threshold to further enhance their buying experience and encourage repeat purchases.

- Growth and Trend Analysis:
  
Capitalize on the YoY growth observed in the "Technology" and "Office Supplies" categories in 2016 by allocating more resources to these areas and exploring what drove the growth.

- Order Frequency:
  
For the 29 customers placing orders on consecutive days, consider creating loyalty programs or rewards to encourage continued engagement and regular purchasing patterns.

- Sales Timing Optimization:
  
Leverage the finding that 781 customers order during weekdays to concentrate sales efforts and promotions during these times to maximize revenue.

- Regional Sales Strategy:

Investigate the reasons behind the East region's high order volume, particularly from customer WB-21850, to identify successful strategies that can be applied to other regions.
Address disparities in order volumes across regions by tailoring marketing strategies to regional preferences and demands.

- Product Popularity and Repurchase Rates:

Review the sales strategies for the 32 products ordered more than once on the same day to understand what drives repeat purchases within a short timeframe and apply these insights to other products.

- Customer Profitability Focus:

Investigate customers with low or negative cumulative profit to understand the causes and develop strategies to convert them into profitable accounts.
Develop a risk mitigation strategy to reduce dependency on key accounts that contribute a large portion of profits.

- Profit Margin Optimization:
  
Highlight products with the highest profit margins for potential upselling and cross-selling opportunities.
Assess the products with the lowest profit margins to determine if cost reductions, price increases, or discontinuation is appropriate.

- Spending Level Strategies:

Use the categorization of spenders to develop tiered marketing strategies, with incentives designed to move customers from 'Low' and 'Medium' spender categories to 'High Spenders'.
Analyze the purchasing behavior of 'High Spenders' to develop personalized services or products that cater to their preferences.





































