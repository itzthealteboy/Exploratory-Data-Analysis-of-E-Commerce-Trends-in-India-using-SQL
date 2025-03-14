# Exploratory Data Analysis of E-Commerce Trends in India using SQL
![Ecommerce_logo](https://github.com/itzthealteboy/Exploratory-Data-Analysis-of-E-Commerce-Trends-in-India-using-SQL/blob/main/Olist.png)

## Objective
This project involves conducting an exploratory data analysis (EDA) on the dataset of an e-commerce platform operating in India. The primary goal is to extract meaningful insights to better understand customer behaviors, product trends, sales patterns, and delivery performance. By utilizing MySQL, the analysis covers various data points, including customer demographics, order details, product popularity, and delivery timelines.

## Overview
The goal of this data analysis project is to leverage SQL to gain insights from an e-commerce platform's data in order to improve decision-making, optimize business operations, and enhance customer experience

## Dataset
The dataset for this project is sourced from the Kaggle dataset:
- **Dataset Link:** [E-commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

- ## Business Problems and Solution
-
-  ### 1. Describe the dataset
 ```sql
  SELECT *
  FROM olist_customers_dataset;
```

### 2. What is the customer distribution across different cities and states?
```sql
SELECT
customer_city,
customer_state,
COUNT(*) as num_cust
FROM olist_customers_dataset
GROUP BY customer_city, customer_state
ORDER BY COUNT(*) DESC;
```

### 3.  What is the total number of orders received?
```sql
SELECT 
COUNT(*)
FROM olist_orders_dataset
;
```

### 4. What are the days with the highest order volume?
```sql
SELECT 
DATE(order_purchase_timestamp) AS order_date,
COUNT(*) AS total_order
FROM olist_orders_dataset
GROUP BY order_date
ORDER BY total_order DESC
;
```

### 5. Which products are the most frequently purchased?
```sql
SELECT 
t.product_category_name_english,
COUNT(*) AS count_orders
FROM olist_products_dataset AS p
INNER JOIN olist_order_items_dataset as oi ON p.product_id = oi.product_id
INNER JOIN product_category_name_translation AS t on p.product_category_name = t.ï»¿product_category_name
GROUP BY t.product_category_name_english
ORDER BY COUNT(*) DESC
```

### 6. What is the estimated delivery day for each month based on historical order data?
```sql
SELECT
MONTH(order_purchase_timestamp) AS order_month,
year(order_purchase_timestamp) AS order_year,
AVG(DATEDIFF(order_estimated_delivery_date, order_delivered_customer_date)) as esti_diff
FROM olist_orders_dataset
WHERE order_status= "delivered"
GROUP BY order_month, order_year;
```

### 7. On which days were orders delayed beyond the expected delivery date?
```sql
SELECT
order_id,
order_year,
order_month,
actual_delivery_date,
estimated_delivery_date
date_diff,
avg_month_diff
FROM
(SELECT
order_id,
order_year,
order_month,
actual_delivery_date,
estimated_delivery_date,
DATEDIFF(actual_delivery_date, estimated_delivery_date) AS date_diff,
AVG(DATEDIFF(actual_delivery_date, estimated_delivery_date))
	OVER(PARTITION BY order_year, order_month) AS avg_month_diff
FROM
(
SELECT 
order_id,
YEAR(order_purchase_timestamp) AS order_year,
MONTH(order_purchase_timestamp) AS order_month,
order_delivered_customer_date AS actual_delivery_date,
order_estimated_delivery_date AS estimated_delivery_date
FROM olist_orders_dataset
WHERE order_status = 'delivered'
AND order_delivered_customer_date IS NOT NULL
)s
)t
WHERE date_diff < avg_month_diff;
```

## Conclusion
1. As a result of the exploratory analysis, we discovered that São Paulo has the highest number of customers, totaling 8,660. Following São Paulo, Rio de Janeiro ranks second in customer count.
2. The total number of orders on Olist is 99,441.
3. During our analysis, we observed an unusual sharp increase in the number of orders on Olist E-commerce on November 24, 2017, with a total of 1,176 orders. It was determined that this spike occurred due to Black Friday, a major sales event.
4. The "Bed Bath Table" is the most ordered product on Olist E-commerce
5. The estimated expected day of delivery varies across different months and years, with no discernible trend or pattern in how the delivery dates fluctuate.

This analysis provides a comprehensive view of the E-commerce and can help inform content stratergy and decision making.

## Stay Updated 
For more projects on SQL, data analysis and other related topics, make sure to follow me on social media
**Dataset Link:** [E-commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
