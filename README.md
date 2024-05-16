# Pizza Sales Analysis

This project contains SQL queries for analyzing pizza sales data. The analysis is divided into three sections: Basic, Intermediate, and Advanced, each addressing different aspects of the sales data.

## Data Files

The following CSV files are used in this project:

1. `order_details.csv` - Contains details about each order, including the order ID, pizza ID, and quantity.
2. `orders.csv` - Contains information about each order, including the order ID, date, and time.
3. `pizza_types.csv` - Contains information about different pizza types, including the pizza type ID, name, category, and ingredients.
4. `pizzas.csv` - Contains information about pizzas, including the pizza ID, pizza type ID, size, and price.

## SQL Queries

### Basic

1. **Total number of orders placed:**
    ```sql
    SELECT COUNT(*) AS total_orders
    FROM orders;
    ```

2. **Total revenue generated from pizza sales:**
    ```sql
    SELECT SUM(od.quantity * p.price) AS total_revenue
    FROM order_details od
    JOIN pizzas p ON od.pizza_id = p.pizza_id;
    ```

3. **Highest-priced pizza:**
    ```sql
    SELECT pizza_id, MAX(price) AS highest_price
    FROM pizzas;
    ```

4. **Most common pizza size ordered:**
    ```sql
    SELECT p.size, COUNT(*) AS size_count
    FROM order_details od
    JOIN pizzas p ON od.pizza_id = p.pizza_id
    GROUP BY p.size
    ORDER BY size_count DESC
    LIMIT 1;
    ```

5. **Top 5 most ordered pizza types along with their quantities:**
    ```sql
    SELECT od.pizza_id, SUM(od.quantity) AS total_quantity
    FROM order_details od
    GROUP BY od.pizza_id
    ORDER BY total_quantity DESC
    LIMIT 5;
    ```

### Intermediate

1. **Total quantity of each pizza category ordered:**
    ```sql
    SELECT pt.category, SUM(od.quantity) AS total_quantity
    FROM order_details od
    JOIN pizzas p ON od.pizza_id = p.pizza_id
    JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
    GROUP BY pt.category;
    ```

2. **Distribution of orders by hour of the day:**
    ```sql
    SELECT EXTRACT(HOUR FROM time) AS order_hour, COUNT(*) AS order_count
    FROM orders
    GROUP BY order_hour
    ORDER BY order_hour;
    ```

3. **Category-wise distribution of pizzas:**
    ```sql
    SELECT pt.category, COUNT(*) AS category_count
    FROM order_details od
    JOIN pizzas p ON od.pizza_id = p.pizza_id
    JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
    GROUP BY pt.category;
    ```

4. **Average number of pizzas ordered per day:**
    ```sql
    SELECT date, AVG(order_count) AS average_pizzas_per_day
    FROM (
        SELECT o.date, COUNT(*) AS order_count
        FROM orders o
        JOIN order_details od ON o.order_id = od.order_id
        GROUP BY o.date
    ) subquery
    GROUP BY date;
    ```

5. **Top 3 most ordered pizza types based on revenue:**
    ```sql
    SELECT od.pizza_id, SUM(od.quantity * p.price) AS total_revenue
    FROM order_details od
    JOIN pizzas p ON od.pizza_id = p.pizza_id
    GROUP BY od.pizza_id
    ORDER BY total_revenue DESC
    LIMIT 3;
    ```

### Advanced

1. **Percentage contribution of each pizza type to total revenue:**
    ```sql
    WITH total_revenue AS (
        SELECT SUM(od.quantity * p.price) AS total
        FROM order_details od
        JOIN pizzas p ON od.pizza_id = p.pizza_id
    )
    SELECT od.pizza_id, (SUM(od.quantity * p
