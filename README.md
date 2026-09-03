# Retail-Sales-Analysis-Using-SQL
Retail sales data analysis using SQL (PostgreSQL)
## About the Project

This is a practice project created to strengthen my SQL and data
analysis skills. The project focuses on exploring and analyzing
retail sales data using PostgreSQL.

This project was developed as a learning exercise by following
an online YouTube tutorial and applying the SQL concepts covered
in the tutorial.
## Tools
- PostgreSQL
- SQL

## Skills Applied
- Database & Table Creation
- Data Cleaning
- Data Exploration
- Aggregate Functions
- GROUP BY & ORDER BY
- Date & Time Functions
- CASE WHEN
- Window Functions
- Business Data Analysis
## Analysis Performed

The project answers several business questions, including:

- Sales on a specific date
- Clothing sales during November 2022
- Total sales by category
- Average customer age for the Beauty category
- Transactions with sales above 1,000
- Transactions by gender and category
- Best-selling month in each year
- Top 5 customers by total sales
- Unique customers by category
- Number of orders by sales shift
## Learning Reference

This project was created for learning purposes based on the
following YouTube tutorial:

https://youtu.be/ChIQjGBI3AM?si=s4Y32CRKB29ZuSjm

## Learning Outcomes

Through this project, I strengthened my understanding of SQL
querying, data cleaning, data exploration, and solving
business-related questions using data.
CREATE DATABASE sql_project01
--CREATE TABLE--
CREATE TABLE retail_sales
	(
		transactions_id INT PRIMARY KEY,
		sale_date DATE,
		sale_time TIME,
		customer_id INT,
		gender VARCHAR(15),
		age INT,
		category VARCHAR(15),
		quantiy INT,
		price_per_unit FLOAT,
		cogs FLOAT,
		total_sale FLOAT	

	);
SELECT * FROM retail_sales
LIMIT 10
SELECT
	COUNT(*) 
FROM retail_sales
--Data Cleaning--
SELECT * FROM retail_sales
WHERE  transactions_id is NULL

SELECT * FROM retail_sales
WHERE  sale_date is NULL

SELECT * FROM retail_sales
WHERE  sale_time is NULL

SELECT * FROM retail_sales
WHERE  
		transactions_id is NULL
		OR
		sale_date is NULL
		OR
		sale_time is NULL
		OR
		customer_id is NULL
		OR
		gender is NULL
		OR
		age is NULL
		OR
		category is NULL
		OR
		quantiy is NULL
		OR
		price_per_unit is NULL
		OR
		cogs is NULL
		OR
		total_sale is NULL;
DELETE FROM retail_sales
WHERE  
		transactions_id is NULL
		OR
		sale_date is NULL
		OR
		sale_time is NULL
		OR
		customer_id is NULL
		OR
		gender is NULL
		OR
		age is NULL
		OR
		category is NULL
		OR
		quantiy is NULL
		OR
		price_per_unit is NULL
		OR
		cogs is NULL
		OR
		total_sale is NULL;
--Data Exploration--
SELECT * FROM retail_sales

-- How many sales we have?
SELECT COUNT(*) as total_sale FROM retail_sales

-- How many unique customers we have?
SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales

-- How many uniques category we have?
SELECT COUNT(DISTINCT category) as total_sale FROM retail_sales
SELECT DISTINCT category FROM retail_sales

-- Data Analysis & Business Key Problems & Answers
SELECT * FROM retail_sales

-- My Analysis & Findings
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05'

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022
SELECT * FROM retail_sales 
WHERE category = 'Clothing' 
AND quantiy >= 4
AND TO_CHAR(sale_date, 'MM-YYYY') = '11-2022'

AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'


-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
SELECT 
	category,
	SUM(total_sale) as net_sale,
	COUNT (*) as total_orders
FROM retail_sales
GROUP BY 1

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
SELECT 
		ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category='Beauty'

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
SELECT *
FROM retail_sales
WHERE total_sale >= 1000


-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
SELECT 
		COUNT(*) as total_transaction,
		gender,
		category
FROM retail_sales
GROUP BY
		gender,
		category
ORDER BY 1
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
SELECT 
		year,
		month,
		avg_sale
FROM
(
SELECT
EXTRACT(YEAR FROM sale_date) as year,
EXTRACT(MONTH FROM sale_date) as month,
AVG(total_sale) as avg_sale,
RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)ORDER BY AVG(total_sale)DESC) as rank
FROM retail_sales
GROUP BY 1,2
) as t1
WHERE rank=1

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
SELECT 
	customer_id,
	SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
SELECT 
	category,
	COUNT(DISTINCT customer_id) as unique_customers
FROM retail_sales
GROUP BY category

-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
WITH hourly_sale
AS
(SELECT*,
	CASE
		WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time)BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END as shift
FROM retail_sales
)
SELECT
	shift,
	COUNT (*) as total_orders
FROM hourly_sale
GROUP BY shift

--End of project--

## My Additional Analysis

To extend my learning beyond the tutorial, I developed additional
business analysis questions focusing on profitability, customer
behavior, and demographic analysis.

1. Revenue and Profit by Category
2. Profit Margin by Category
3. Customers with the Highest Number of Transactions
4. Average Transaction Value by Gender
5. Sales Performance by Age Group
