# 🍜 Danny's Diner — SQL Data Analysis Project

<img width="1067" height="789" alt="image" src="https://github.com/user-attachments/assets/ec5f4418-10df-4cad-8cad-1ea4617f3392" />

## 📌 Project Overview
This project is based on the popular "Danny's Diner" SQL case study.  
The goal is to analyze a fictional Japanese restaurant’s sales, menu, and membership data to uncover customer behavior, sales trends, and the impact of the membership program.

# Introduction
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.
                                                      
                                                      
                                                      
# Problem Statement

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

## 🗂️ Dataset
The project uses three CSV tables:

- **sales** — Records of customer purchases (customer_id, order_date, product_id)
- **menu** — Menu details with product_id, product_name, and price
- **members** — Membership signup information (customer_id, join_date)
# Entity Relationship Diagram

<img width="849" height="449" alt="image" src="https://github.com/user-attachments/assets/e64939a9-4734-4cf3-8fa8-414ce168af80" />



## 🧠 SQL Case Study Questions

1.What is the total amount each customer spent at the restaurant?

2.How many days has each customer visited the restaurant?

3.What was the first item from the menu purchased by each customer?

4.What is the most purchased item on the menu and how many times was it purchased by all customers?

5.Which item was the most popular for each customer?

6.Which item was purchased first by the customer after they became a member?

7.Which item was purchased just before the customer became a member?

8.What is the total items and amount spent for each member before they became a member?

9.If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

10.In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

## 🛠️ Tools & Concepts Used
- SQL (PostgreSQL / MySQL / SQLite)
- Joins (INNER, LEFT)
- Aggregations (SUM, COUNT, AVG)
- Window Functions (RANK, DENSE_RANK, ROW_NUMBER)
- Common Table Expressions (CTEs)
- Date and String Functions


## 🚀 Solutions
### Q1. What is the total amount each customer spent at the restaurant?
```
SELECT 
  s.customer_id, 
  SUM(m.price) AS total_spent
FROM sales s
JOIN menu m 
  ON s.product_id = m.product_id
GROUP BY s.customer_id;

| customer_id  | total_spent  |
| ------------ | ------------ |
| A            | 76           |
| B            | 74           |
| C            | 36           |

```
### Q2. How many days has each customer visited the diner?

```sql
SELECT 
  customer_id,
  COUNT(DISTINCT order_date) AS visit_days
FROM sales
GROUP BY customer_id;

| customer_id  | visit_days  |
| ------------ | ----------- |
| A            | 4           |
| B            | 6           |
| C            | 2           |
```

### 3. What was the first item from the menu purchased by each customer?
```
WITH first_purchase AS (
  SELECT 
    s.customer_id,
    m.product_name,
    ROW_NUMBER() OVER (
        PARTITION BY s.customer_id 
        ORDER BY s.order_date
    ) AS row_num
  FROM sales s 
  JOIN menu m 
    ON s.product_id = m.product_id
)
SELECT 
  customer_id,
  product_name
FROM first_purchase
WHERE row_num = 1;
```

| customer _id | product _name |
| ------------ | ------------- |
| A            | curry         |
| B            | curry         |
| C            | ramen         |

### 4.What is the most purchased item on the menu and how many times was it purchased by all customers?


```sql
SELECT 
  m.product_name,
  COUNT(*) AS total_purchases
FROM sales s
JOIN menu m 
  ON s.product_id = m.product_id
GROUP BY m.product_name
ORDER BY total_purchases DESC
LIMIT 1;
```
| product _name | total _purchases |
| ------------- | ---------------- |
| ramen         | 8                |


###5. 

