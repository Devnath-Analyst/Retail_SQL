# **Retail Sales Analysis SQL Project**

## **Project Overview**

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `retail_sales_p1`

This project showcases SQL skills and techniques commonly used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and addressing specific business questions using SQL queries. This project is perfect for those beginning their journey in data analysis and aiming to build a solid foundation in SQL.

## **Objectives**

1. **Database Setup**: Create and populate a retail sales database with the provided sales data.  
2. **Data Cleaning**: Identify and remove any records containing missing or null values.  
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to gain insights into the dataset.  
4. **Business Analysis**: Utilize SQL to address key business questions and extract meaningful insights from the sales data.

## **Project Structure**

### **1\. Database Setup**

* **Database Creation**: The project starts by creating a database named `retail_sales_p1`.  
* **Table Creation**: A table named `retail_sales` is created to store the sales data. The table includes columns such as transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

CREATE DATABASE retail\_sales\_p1;

DROP TABLE IF EXISTS retail\_sales;

CREATE TABLE retail\_sales (  
    transactions\_id INT PRIMARY KEY,  
    sale\_date DATE,  
    sale\_time TIME,  
    customer\_id INT,  
    gender VARCHAR(10),  
    age INT,  
    category VARCHAR(15),  
    quantiy FLOAT,  
    price\_per\_unit FLOAT,  
    cogs FLOAT,  
    total\_sale FLOAT  
);

SELECT \* FROM retail\_sales;

### **2\. Data Exploration & Cleaning**

* **Record Count**: Count the total number of records in the dataset.  
* **Unique Customer Count**: Determine how many unique customers are present.  
* **Category Identification**: Identify all unique product categories.  
* **Null Value Check**: Identify and remove records containing null values.

SELECT COUNT(\*) FROM retail\_sales;  
SELECT COUNT(DISTINCT customer\_id) FROM retail\_sales;  
SELECT DISTINCT category FROM retail\_sales;

SELECT \* FROM retail\_sales  
WHERE transactions\_id IS NULL  
   OR sale\_date IS NULL  
   OR sale\_time IS NULL  
   OR customer\_id IS NULL  
   OR gender IS NULL  
   OR age IS NULL  
   OR category IS NULL  
   OR quantiy IS NULL  
   OR price\_per\_unit IS NULL  
   OR cogs IS NULL  
   OR total\_sale IS NULL;

DELETE FROM retail\_sales  
WHERE transactions\_id IS NULL  
   OR sale\_date IS NULL  
   OR sale\_time IS NULL  
   OR customer\_id IS NULL  
   OR gender IS NULL  
   OR age IS NULL  
   OR category IS NULL  
   OR quantiy IS NULL  
   OR price\_per\_unit IS NULL  
   OR cogs IS NULL  
   OR total\_sale IS NULL;

### **3\. Data Analysis & Findings**

Key business questions were answered through SQL queries:

1. **Retrieve all sales made on '2022-11-05':**

SELECT \* FROM retail\_sales WHERE sale\_date \= '2022-11-05';

2. **Retrieve all transactions for 'Clothing' where quantity sold \> 4 in Nov-2022:**

SELECT \* FROM retail\_sales  
WHERE category \= 'Clothing'  
  AND quantiy \>= 4  
  AND TO\_CHAR(sale\_date, 'YYYY-MM') \= '2022-11';

3. **Calculate total sales for each category:**

SELECT category, SUM(total\_sale) FROM retail\_sales GROUP BY 1;

4. **Find the average age of customers who purchased 'Beauty' category items:**

SELECT ROUND(AVG(age), 2\) FROM retail\_sales WHERE category \= 'Beauty';

5. **Find all transactions with total sales greater than 1000:**

SELECT \* FROM retail\_sales WHERE total\_sale \> 1000;

6. **Count total transactions by gender and category:**

SELECT category, gender, COUNT(\*) AS total\_transactions  
FROM retail\_sales  
GROUP BY 1, 2  
ORDER BY 1;

7. **Calculate average monthly sales and find the best selling month for each year:**

SELECT year, month, avg\_sale  
FROM (      
    SELECT  
        EXTRACT(YEAR FROM sale\_date) AS year,  
        EXTRACT(MONTH FROM sale\_date) AS month,  
        AVG(total\_sale) AS avg\_sale,  
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale\_date) ORDER BY AVG(total\_sale) DESC) AS rank  
    FROM retail\_sales  
    GROUP BY 1, 2  
) AS t1  
WHERE rank \= 1;

8. **Identify the top 5 customers based on total sales:**

SELECT customer\_id, SUM(total\_sale) AS total\_sales  
FROM retail\_sales  
GROUP BY 1  
ORDER BY 2 DESC  
LIMIT 5;

9. **Find the number of unique customers per category:**

SELECT category, COUNT(DISTINCT customer\_id) AS uniq\_count FROM retail\_sales GROUP BY 1;

10. **Classify orders based on time of sale:**

WITH hourly\_sale AS (  
    SELECT \*,  
        CASE  
            WHEN EXTRACT(HOUR FROM sale\_time) \< 12 THEN 'Morning'  
            WHEN EXTRACT(HOUR FROM sale\_time) BETWEEN 12 AND 17 THEN 'Afternoon'  
            ELSE 'Evening'  
        END AS shift  
    FROM retail\_sales  
)  
SELECT shift, COUNT(\*) AS total\_orders FROM hourly\_sale GROUP BY shift;

## **Findings**

* **Customer Demographics**: The dataset contains a wide age range of customers, with significant purchases in categories like Clothing and Beauty.  
* **High-Value Transactions**: Transactions exceeding 1000 in sales indicate premium sales.  
* **Sales Trends**: Monthly analysis identifies seasonal trends and peak months.  
* **Customer Insights**: Identification of top-spending customers and popular product categories.

## **Reports**

* **Sales Summary**: Total sales, customer demographics, and category performance.  
* **Trend Analysis**: Monthly and shift-based trends.  
* **Customer Insights**: Key customer metrics and purchase behaviors.

## **Conclusion**

This project provides a thorough introduction to SQL, covering database setup, data cleaning, EDA, and business-focused queries. The insights gained can aid in better business decision-making by understanding sales trends and customer preferences.

## **How to Use**

1. **Clone the Repository**: Clone this project repository from GitHub.  
2. **Set Up the Database**: Execute the SQL scripts in `database_setup.sql` to create and populate the database.  
3. **Run the Queries**: Use the provided SQL queries to analyze the dataset.  
4. **Customize & Explore**: Modify queries to uncover additional insights.

\--- END OF PROJECT \---

