# Pizza Sales Analysis Project

## Project Overview

Welcome to the Pizza Sales Analysis Project! This project demonstrates the use of SQL for analyzing a comprehensive pizza sales dataset. The aim is to extract meaningful insights that can aid in strategic decision-making for a pizza business. This repository contains SQL queries that address various business-related questions, uncovering trends and patterns in pizza sales.

## Project Objectives

- Analyze the total number of orders placed.
- Calculate the total revenue generated from pizza sales.
- Identify the highest-priced pizza.
- Determine the most common pizza size ordered.
- List the top 5 most ordered pizza types along with their quantities.
- Analyze the distribution of orders by hour of the day.
- Calculate the average number of pizzas ordered per day.
- Determine the top 3 most ordered pizza types based on revenue.
- Analyze the cumulative revenue generated over time.
- Determine the percentage contribution of each pizza type to total revenue.
- Analyze the revenue for each pizza category.
  
## Project Schema

The project is based on a relational database schema involving multiple tables. The key tables used in this project include:

- **Orders:** Contains information about each order placed.
- **Pizzas:** Contains details about each type of pizza, including price and category.
- **OrderDetails:** A junction table that links orders and pizzas, capturing the quantity of each pizza ordered.
- **PizzaCategories:** Contains information about the different categories of pizzas.
- **Times:** Contains time-related data for analyzing orders by hour.

## SQL Queries and Analysis

### 1. Total Number of Orders Placed

This query retrieves the total number of orders placed:

```sql
USE pizza_hut;

SELECT COUNT(*) Total_Orders FROM orders;
```

### 2. Total Revenue Generated from Pizza Sales

This query calculates the total revenue generated:

```sql
SELECT
  ROUND(SUM(od.quantity * p.price), 2) AS Total_Revenue
FROM
  pizzas
    JOIN
  order_details od ON p.pizza_id = od.pizza_id
```

### 3. Highest-Priced Pizza

This query identifies the highest-priced pizza:

```sql
SELECT
    *
FROM
    pizzas
ORDER BY price DESC
LIMIT 1;
```

### 4. Most Common Pizza Size Ordered

This query determines the most common pizza size ordered:

```sql
SELECT
    p.size, COUNT(p.size) Order_Count
FROM
    pizzas p
        JOIN
    order_details od ON p.pizza_id = od.pizza_id
GROUP BY size
ORDER BY Order_Count DESC
```

### 5. Top 5 Most Ordered Pizza Types

This query lists the top 5 most ordered pizza types along with their quantities:

```sql
SELECT
    pt.name, SUM(od.quantity) Most_Ordered
FROM
    pizza_types pt
        JOIN
    pizzas p ON pt.pizza_type_id = p.pizza_type_id
        JOIN
    order_details od ON p.pizza_id = od.pizza_id
GROUP BY pt.name
ORDER BY Most_Ordered DESC
LIMIT 5
```

### 6. Distribution of Orders by Hour of the Day

This query determines the distribution of orders by hour:

```sql
SELECT
    HOUR(order_time) hour, COUNT (o.order_id) order_count
FROM
    orders o
GROUP BY hour
```

### 7. Average Number of Pizzas Ordered Per Day

This query calculates the average number of pizzas ordered per day:

```sql
SELECT
    ROUND(AVG(ordered_quantity)) Avg_pizza_order_per_day
FROM
    (SELECT
        order_date, SUM(od.quantity) ordered_quantity
    FROM
        orders o
    JOIN order_details od ON o.order_id = od.order_id
    GROUP BY order_date) AS Date_wise_order_quantity
```

### 8. Top 3 Most Ordered Pizza Types Based on Revenue

This query determines the top 3 most ordered pizza types based on revenue:

```sql
SELECT
    pt.name, SUM(od.quantity * p.price) Total_revenue
FROM
    pizzas p
        JOIN
    pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
        JOIN
    order_details od ON p.pizza_id = od.pizza_id
GROUP BY pt.name
ORDER BY Total_revenue DESC
LIMIT 3
```

### 9. Cumulative Revenue Generated Over Time

This query analyzes the cumulative revenue generated over time:

```sql
SELECT order_date,
SUM(Revenue) OVER (ORDER BY order_date) Cumulative_Revenue
FROM
(SELECT o.order_date, ROUND (SUM(od.quantity * p.price), 2) Revenue
FROM orders o
JOIN order_details od
ON o.order_id = od.order_id
JOIN pizzas p
ON od.pizza_id = p.pizza_id
GROUP BY o.order_date) sales
```

### 10. Percentage Contribution of Each Pizza Type to Total Revenue

This query calculates the percentage contribution of each pizza type to total revenue:

```sql
SELECT
    category,
    ROUND((ROUND(SUM(od.quantity* p.price), 2) / (SELECT SUM(od.quantity * p.price)
            FROM
                pizzas p
                    JOIN
                order_details od ON p.pizza_id = od.pizza_id) * 100),
          2) AS percentage_Contribution
FROM
    pizzas p
        JOIN
    pizza_types pt ON p.pizza_type_id= pt.pizza_type_id
        JOIN
    order_details od ON p.pizza_id = od.pizza_id
GROUP BY category
```

### 11. Revenue for Each Pizza Category

This query analyzes the revenue for each pizza category:

```sql
SELECT
    category, SUM(od.quantity) Category_Count
FROM
    pizza_types pt
        JOIN
    pizzas p ON pt.pizza_type_id = p.pizza_type_id
        JOIN
    order_details od ON p.pizza_id = od.pizza_id
GROUP BY category
ORDER BY Category_Count DESC;
```

## Conclusion

This project showcases the application of advanced SQL queries to derive actionable insights from pizza sales data. By analyzing various aspects such as order patterns, revenue distribution, and pizza preferences, we can better understand customer behavior and optimize business strategies.

Thank you for exploring this project. If you have any questions or suggestions, feel free to reach out!

---

## Author

**Somnath Pandit** - [GitHub](https://github.com/your-github-profile)
