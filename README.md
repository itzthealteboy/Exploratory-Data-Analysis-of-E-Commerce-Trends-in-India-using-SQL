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
