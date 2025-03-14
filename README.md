# Exploratory Data Analysis of E-Commerce Trends in India using SQL
![Ecommerce_logo](https://github.com/itzthealteboy/Exploratory-Data-Analysis-of-E-Commerce-Trends-in-India-using-SQL/blob/main/Olist.png)

## Objective
This project involves conducting an exploratory data analysis (EDA) on the dataset of an e-commerce platform operating in India. The primary goal is to extract meaningful insights to better understand customer behaviors, product trends, sales patterns, and delivery performance. By utilizing MySQL, the analysis covers various data points, including customer demographics, order details, product popularity, and delivery timelines.

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
