# Week 4: Aggregate Functions & Grouping

## 1\. 📊 Aggregate Functions

Aggregate functions operate on a set of rows and return a single summary value. They are typically used with the `SELECT` statement and often with the `GROUP BY` clause.

  * **`COUNT()`**: Returns the number of items in a group.
      * `COUNT(*)`: Counts all rows, including NULL values.
      * `COUNT(column_name)`: Counts non-NULL values in the specified column.
  * **`SUM()`**: Calculates the **sum** of all values in the specified numeric column. It ignores NULL values.
  * **`AVG()`**: Calculates the **average (mean)** of all values in the specified numeric column. It ignores NULL values.
  * **`MAX()`**: Returns the **largest** value in the specified column. It works with numeric, string, and date/time data types.
  * **`MIN()`**: Returns the **smallest** value in the specified column. It works with numeric, string, and date/time data types.
  * **`DISTINCT` with Aggregates**: Used inside an aggregate function to only consider unique (non-duplicate) values.

### Example Usage:

```sql
SELECT
    COUNT(*) AS TotalRecords,
    SUM(OrderTotal) AS GrandTotalSales,
    AVG(OrderTotal) AS AverageOrderValue,
    MAX(OrderDate) AS MostRecentOrder,
    MIN(OrderDate) AS EarliestOrder,
    COUNT(DISTINCT CustomerID) AS UniqueCustomers
FROM
    Orders;
```

-----

## 2\. 📝 GROUP BY and HAVING

These clauses are used to organize data into groups and then apply filtering to those groups.

### Grouping Data (`GROUP BY`)

The **`GROUP BY`** clause divides the rows returned by the `SELECT` statement into sets of summary rows by the values in one or more columns. It is essential when you use an aggregate function and want that function to calculate a value **for each unique group** rather than for the entire dataset.

  * **Rule:** Any column selected in the `SELECT` list that is **not** inside an aggregate function *must* be included in the `GROUP BY` clause.

**Example: Total sales per employee**

```sql
SELECT
    EmployeeID,
    SUM(OrderTotal) AS EmployeeTotalSales
FROM
    Orders
GROUP BY
    EmployeeID;
```

### Filtering Groups (`HAVING`)

The **`HAVING`** clause is used to filter groups based on the result of an aggregate function. The standard `WHERE` clause cannot filter on aggregate results (since `WHERE` executes *before* the grouping happens). `HAVING` executes *after* `GROUP BY`.

  * **Scenario:** You want to see the employees whose total sales are greater than $10,000.

**Example: Employees with sales \> 10000**

```sql
SELECT
    EmployeeID,
    SUM(OrderTotal) AS EmployeeTotalSales
FROM
    Orders
GROUP BY
    EmployeeID
HAVING
    SUM(OrderTotal) > 10000; -- Filtering the result of the SUM aggregate
```

### Complex Grouping Scenarios (Order of Execution)

When a query contains all these clauses, they execute in a specific order:

1.  **`FROM`** and **`JOIN`**: Determine the data source.
2.  **`WHERE`**: Filters individual rows *before* grouping.
3.  **`GROUP BY`**: Groups the remaining rows.
4.  **Aggregate Functions**: Calculate summary values for each group.
5.  **`HAVING`**: Filters the resulting groups.
6.  **`SELECT`**: Outputs the final columns/aggregates.
7.  **`ORDER BY`**: Sorts the final result set.

### Multiple Column Grouping



**Example: Total sales per *country and city***

```sql
SELECT
    ShippingCountry,
    ShippingCity,
    SUM(OrderTotal) AS LocationTotalSales
FROM
    Orders
GROUP BY
    ShippingCountry,
    ShippingCity
ORDER BY
    LocationTotalSales DESC;
```

Would you like to try constructing a practice query using these concepts?

## Examples:
```sql
-- Basic aggregation
SELECT 
    category, 
    COUNT(*) as total_products,
    AVG(price) as avg_price,
    MIN(price) as min_price,
    MAX(price) as max_price
FROM products
GROUP BY category;

-- HAVING clause with multiple conditions
SELECT 
    department, 
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary,
    SUM(salary) as total_salary
FROM employees
GROUP BY department
HAVING COUNT(*) > 5 AND AVG(salary) > 50000;

-- Multiple column grouping
SELECT 
    department,
    job_title,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department, job_title
ORDER BY department, avg_salary DESC;

-- Using DISTINCT with aggregates
SELECT 
    COUNT(DISTINCT customer_id) as unique_customers,
    COUNT(*) as total_orders
FROM orders
WHERE order_date >= '2024-01-01';
```

## Practice Exercises:

# 1. Calculate Sales Summaries

### **Description**

Learn how to use aggregate functions to calculate totals, counts, averages, minimums, and maximums.
These functions help summarize large datasets into meaningful insights.

### **Example Queries**

**Total sales**

```sql
SELECT SUM(total_amount) AS total_sales
FROM orders;
```

**Total number of orders**

```sql
SELECT COUNT(*) AS total_orders
FROM orders;
```

**Highest and lowest order value**

```sql
SELECT 
    MAX(total_amount) AS highest_order,
    MIN(total_amount) AS lowest_order
FROM orders;
```

### **Challenges**

* Calculate the total revenue for 2024.
* Find the highest order for each customer.

---

# 2. Find Average Prices by Category

### **Description**

Use `AVG()` with `GROUP BY` to analyze average values per group, such as categories, suppliers, or customers.

### **Example Query**

```sql
SELECT 
    category,
    AVG(price) AS average_price
FROM products
GROUP BY category;
```

### **Challenges**

* Find the average price per supplier.
* Find the average order amount per customer.

---

# 3. Count Distinct Customers

### **Description**

Use `COUNT(DISTINCT column)` to find unique customers, products, or regions.

### **Example Queries**

**Unique customers overall**

```sql
SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM orders;
```

**Unique customers per region**

```sql
SELECT 
    region,
    COUNT(DISTINCT customer_id) AS unique_buyers
FROM customers
GROUP BY region;
```

### **Challenges**

* Count distinct products sold.
* Count unique customers per month.

---

# 4. Group Data by Multiple Columns

### **Description**

Learn to group data by more than one column to create deeper analytical insights.

### **Example Queries**

**Group by category and supplier**

```sql
SELECT 
    category,
    supplier,
    COUNT(*) AS number_of_products
FROM products
GROUP BY category, supplier;
```

**Group by customer and purchase date**

```sql
SELECT 
    customer_id,
    DATE(order_date) AS purchase_day,
    SUM(total_amount) AS daily_spending
FROM orders
GROUP BY customer_id, DATE(order_date);
```

### **Challenges**

* Group by region and month.
* Group products by category and stock status.

---

# 5. Use HAVING for Filtered Aggregates

### **Description**

`HAVING` filters grouped results after aggregation.
It works like `WHERE`, but for aggregated data.

### **Example Queries**

**Categories with sales > 10,000**

```sql
SELECT 
    category,
    SUM(total_amount) AS total_sales
FROM orders
JOIN products USING(product_id)
GROUP BY category
HAVING SUM(total_amount) > 10000;
```

**Customers with more than 5 orders**

```sql
SELECT 
    customer_id,
    COUNT(*) AS num_orders
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 5;
```

### **Challenges**

* Find suppliers with average price > $30.
* Find months where revenue > $20,000.

---

# 6. Combine Aggregates With Joins

### **Description**

This section teaches combining aggregates with joins — one of the most important real-world SQL skills.

### **Example Queries**

**Total spending per customer**

```sql
SELECT 
    c.customer_name,
    SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name;
```

**Sales per product category**

```sql
SELECT 
    p.category,
    SUM(o.total_amount) AS total_sales
FROM orders o
JOIN products p ON o.product_id = p.product_id
GROUP BY p.category;
```

### **Challenges**

* Join all tables to find the top-spending customer per region.
* Calculate monthly category sales by joining products + orders.

---

# 📂 Sample Datasets

These are example tables for practicing the queries above.

---

## **products table**

| product_id | name       | category    | price | supplier |
| ---------- | ---------- | ----------- | ----- | -------- |
| 1          | Keyboard   | Electronics | 25.00 | HP       |
| 2          | Headphones | Electronics | 40.00 | Sony     |
| 3          | Notebook   | Stationery  | 3.00  | LocalCo  |

---

## **customers table**

| customer_id | customer_name | region |
| ----------- | ------------- | ------ |
| 1           | Ahmed         | East   |
| 2           | Fatima        | West   |
| 3           | Mohamed       | North  |

---

## **orders table**

| order_id | customer_id | product_id | order_date | total_amount |
| -------- | ----------- | ---------- | ---------- | ------------ |
| 101      | 1           | 1          | 2024-01-15 | 25.00        |
| 102      | 2           | 3          | 2024-01-17 | 3.00         |
| 103      | 1           | 2          | 2024-02-02 | 40.00        |

---

## Key Concepts to Master:
- When to use each aggregate function
- GROUP BY vs WHERE
- HAVING clause usage
- NULL handling in aggregates
- Performance considerations

## Common Use Cases:
1. Sales reporting
2. Customer analysis
3. Inventory statistics
4. Employee performance metrics
5. Product category analysis

## Additional Resources:
- SQL Aggregate Functions Documentation
- Business Intelligence Reporting
- Data Analysis with SQL

- Performance Optimization for Aggregates 

