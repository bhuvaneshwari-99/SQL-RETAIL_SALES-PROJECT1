# SQL-RETAIL_SALES-PROJECT1
# Retail Sales Analysis (SQL Project)

## Project Overview

**Project Title:** Retail Sales Analysis
**Level:** Beginner
**Database:** p1_retail_db

This project demonstrates core SQL skills used in data analysis, including database creation, data cleaning, exploratory data analysis (EDA), and solving business problems using SQL queries. The objective is to transform raw retail transaction data into meaningful insights that support business decision-making.

---

## Objectives

* Design and create a structured retail sales database
* Perform data cleaning by identifying and removing null values
* Conduct exploratory data analysis to understand the dataset
* Answer business-driven questions using SQL
* Derive insights on sales performance and customer behavior

---

## Tools and Technologies

* SQL (PostgreSQL)
* pgAdmin
* Excel

---

## Database Setup

### Database Creation

```sql
CREATE DATABASE p1_retail_db;
```

### Table Creation

```sql
CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## Data Cleaning and Exploration

### Key Steps

* Count total records
* Identify unique customers
* List distinct categories
* Detect and remove null values

```sql
SELECT COUNT(*) FROM retail_sales;

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL 
   OR gender IS NULL OR age IS NULL OR category IS NULL 
   OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL 
   OR gender IS NULL OR age IS NULL OR category IS NULL 
   OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

## Data Analysis and Business Queries

### Sales on a Specific Date

```sql
SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### Clothing Sales in November 2022 (Quantity >= 4)

```sql
SELECT * 
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

### Total Sales by Category

```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

### Average Age for Beauty Category

```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

### High Value Transactions

```sql
SELECT * 
FROM retail_sales
WHERE total_sale > 1000;
```

### Transactions by Gender and Category

```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender;
```

### Best Selling Month in Each Year

```sql
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) t1
WHERE rank = 1;
```

### Top 5 Customers by Sales

```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### Unique Customers per Category

```sql
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

### Sales by Time Shift

```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## Key Findings

* Sales show variation across months, helping identify peak periods
* Certain product categories contribute more to total revenue
* High-value transactions indicate the presence of premium customers
* Customer distribution differs across categories and demographics

---

## Conclusion

This project demonstrates how SQL can be used to clean, explore, and analyze retail sales data effectively. It provides a strong foundation in data analysis by combining technical SQL skills with practical business insights.

---





