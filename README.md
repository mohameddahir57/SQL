# SQL Learning Journey

This repository documents my week-by-week progress learning SQL, from fundamentals to advanced topics.  
Each week includes key topics, code examples, practice exercises, key concepts, and additional resources to guide self-paced learning.

---

## Week 1: SQL Fundamentals & Basic Queries

### 🧠 Overview
This week focuses on building a strong foundation in SQL by understanding databases, learning core SQL commands, and writing simple queries to retrieve, filter, and sort data.

### 📘 Topics Covered
1. **Database Basics**
   - What is a database?
   - Types of databases
   - RDBMS concepts

2. **Basic SQL Commands**
   - `SELECT` statement  
   - `WHERE` clause  
   - `ORDER BY`  
   - `LIMIT` / `TOP`

### 💻 Code Examples
```sql
-- Basic SELECT statement
SELECT first_name, last_name, email
FROM customers;

-- WHERE clause with conditions
SELECT product_name, price
FROM products
WHERE price > 100;

-- ORDER BY with multiple columns
SELECT first_name, last_name, age
FROM employees
ORDER BY age DESC, last_name ASC;
````

### 🧩 Practice Exercises

1. Create a simple database with a `customers` table
2. Write queries to select all customers
3. Filter customers by specific criteria
4. Sort results using different columns
5. Limit query results to a specific number of rows

### 🎯 Key Concepts to Master

* Understanding database structure
* Basic query syntax
* Column and table naming conventions
* Comparison operators
* Sorting and filtering basics

### 📚 Additional Resources

* [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)
* [PostgreSQL Documentation](https://www.postgresql.org/docs/)
* [MySQL Documentation](https://dev.mysql.com/doc/)
* [SQL Server Documentation](https://learn.microsoft.com/en-us/sql/sql-server/)

---

## Week 2: Filtering & Data Manipulation

### 🧠 Overview

This week deepens filtering logic using advanced operators and introduces data manipulation statements for inserting, updating, and deleting records safely.

### 📘 Topics Covered

1. **Advanced WHERE Clauses**

   * AND, OR, NOT operators
   * IN operator
   * BETWEEN operator
   * LIKE operator for pattern matching

2. **Data Manipulation**

   * `INSERT` statements
   * `UPDATE` statements
   * `DELETE` statements

### 💻 Code Examples

```sql
-- Complex WHERE clause
SELECT product_name, category, price
FROM products
WHERE category IN ('Electronics', 'Books')
AND price BETWEEN 50 AND 200;

-- Pattern matching with LIKE
SELECT first_name, email
FROM customers
WHERE email LIKE '%@gmail.com';

-- INSERT statement
INSERT INTO employees (first_name, last_name, department)
VALUES ('John', 'Doe', 'IT');

-- UPDATE with conditions
UPDATE products
SET price = price * 1.1
WHERE category = 'Electronics';

-- DELETE with conditions
DELETE FROM orders
WHERE order_date < '2023-01-01';
```

### 🧩 Practice Exercises

1. Write queries using multiple WHERE conditions
2. Practice pattern matching with different LIKE patterns
3. Insert multiple rows of data
4. Update data based on complex conditions
5. Safely delete data with proper conditions

### 🎯 Key Concepts to Master

* Complex filtering logic
* Pattern matching techniques
* Safe data manipulation practices
* Transaction safety
* Backup before updates/deletes

### 📚 Additional Resources

* [SQL Practice Exercises on HackerRank](https://www.hackerrank.com/domains/sql)
* Database backup and restore procedures
* SQL data manipulation best practices
* Pattern matching documentation

---

## Week 3: Joins & Relationships

### 🧠 Overview

Learn how to retrieve data from multiple tables using different types of joins and understand table relationships.

### 📘 Topics Covered

1. **Types of Joins**

   * INNER JOIN
   * LEFT (OUTER) JOIN
   * RIGHT (OUTER) JOIN
   * FULL (OUTER) JOIN

2. **Multiple Table Joins**

   * Joining more than two tables
   * Using aliases
   * Join conditions
   * Understanding relationships (one-to-one, one-to-many, many-to-many)

### 💻 Code Examples

```sql
-- Inner join between orders and customers
SELECT o.order_id, c.customer_name, o.order_date
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;

-- Left join with multiple conditions
SELECT p.product_name, c.category_name, s.supplier_name
FROM products p
LEFT JOIN categories c ON p.category_id = c.category_id
LEFT JOIN suppliers s ON p.supplier_id = s.supplier_id;

-- Multiple table join with conditions
SELECT 
    o.order_id,
    c.customer_name,
    p.product_name,
    oi.quantity,
    oi.unit_price
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
INNER JOIN products p ON oi.product_id = p.product_id
WHERE o.order_date >= '2024-01-01';
```

### 🧩 Practice Exercises

1. Create tables with relationships
2. Practice all types of joins
3. Write queries joining multiple tables
4. Use table aliases effectively
5. Combine joins with WHERE clauses
6. Handle NULL values in joins

### 🎯 Key Concepts to Master

* Understanding table relationships
* Join syntax and types
* NULL handling in joins
* Performance considerations
* Table alias best practices

### 📚 Additional Resources

* Visual Representation of SQL Joins
* Database normalization principles
* Join optimization techniques
* Real-world join examples

---

## Week 4: Aggregate Functions & Grouping

### 🧠 Overview

Learn to summarize and analyze data using aggregate functions and grouping techniques.

### 📘 Topics Covered

1. **Aggregate Functions**

   * COUNT(), SUM(), AVG(), MAX(), MIN()
   * DISTINCT with aggregates

2. **GROUP BY and HAVING**

   * Grouping data
   * Filtering groups
   * Multiple column grouping

### 💻 Code Examples

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

### 🧩 Practice Exercises

1. Calculate sales summaries
2. Find average prices by category
3. Count distinct customers
4. Group data by multiple columns
5. Use HAVING for filtered aggregates
6. Combine aggregates with joins

### 🎯 Key Concepts to Master

* Aggregate function usage
* GROUP BY vs WHERE
* HAVING clause usage
* NULL handling in aggregates
* Performance considerations

### 📚 Additional Resources

* SQL Aggregate Functions Documentation
* Business Intelligence Reporting
* Data Analysis with SQL
* Performance Optimization for Aggregates

---

## Week 5: Subqueries & CTEs (Common Table Expressions)

### 🧠 Overview

Learn to write subqueries and CTEs for complex data retrieval and analysis.

### 📘 Topics Covered

1. **Subqueries**

   * Single-row, multi-row, correlated
   * Subqueries in SELECT, FROM, WHERE

2. **Common Table Expressions (CTEs)**

   * Basic and recursive CTEs
   * Multiple CTEs
   * CTE best practices

### 💻 Code Examples

```sql
-- Subquery in WHERE clause
SELECT product_name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);

-- Subquery in FROM clause
SELECT dept_name, avg_salary
FROM (
    SELECT department as dept_name, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department
) dept_stats
WHERE avg_salary > 60000;

-- Correlated subquery
SELECT employee_name, salary,
(SELECT AVG(salary) FROM employees e2 
 WHERE e2.department = e1.department) as dept_avg
FROM employees e1;

-- Basic CTE
WITH HighValueCustomers AS (
    SELECT customer_id, SUM(order_amount) as total_spent
    FROM orders
    GROUP BY customer_id
    HAVING SUM(order_amount) > 10000
)
SELECT c.customer_name, h.total_spent
FROM HighValueCustomers h
JOIN customers c ON h.customer_id = c.customer_id;

-- Recursive CTE (organization hierarchy)
WITH RECURSIVE EmployeeHierarchy AS (
    SELECT employee_id, employee_name, manager_id, 1 as level
    FROM employees
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.employee_id, e.employee_name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN EmployeeHierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM EmployeeHierarchy;
```

### 🧩 Practice Exercises

1. Write subqueries in different clauses
2. Create CTEs for complex analysis
3. Build recursive queries for hierarchy
4. Optimize queries using CTEs
5. Compare subquery vs CTE approaches

### 🎯 Key Concepts to Master

* Subquery types and use cases
* CTE benefits and limitations
* Recursive query patterns
* Query optimization

### 📚 Additional Resources

* Advanced SQL documentation
* Query optimization guides
* Recursive CTE examples

---

## Week 6: Views & Stored Procedures

### 🧠 Overview

Learn to create views for reusable queries and stored procedures for encapsulating business logic.

### 📘 Topics Covered

1. **Views**

   * Creating and updating views
   * Materialized views
   * View limitations

2. **Stored Procedures**

   * Creating procedures
   * Input/output parameters
   * Error handling
   * Transaction management

### 💻 Code Examples

```sql
-- Creating a simple view
CREATE VIEW high_value_products AS
SELECT product_name, price, category
FROM products
WHERE price > 1000;

-- Basic stored procedure
CREATE PROCEDURE UpdateProductPrice
    @product_id INT,
    @new_price DECIMAL(10,2)
AS
BEGIN
    BEGIN TRY
        UPDATE products SET price = @new_price
        WHERE product_id = @product_id;
        IF @@ROWCOUNT = 0 THROW 50001, 'Product not found', 1;
    END TRY
    BEGIN CATCH
        THROW 50001, 'Error updating product price', 1;
    END CATCH
END;
```

### 🧩 Practice Exercises

1. Create views for common queries
2. Write procedures with error handling
3. Implement transaction management
4. Create parameterized procedures

### 🎯 Key Concepts to Master

* View types and limitations
* Procedure parameters
* Error handling patterns
* Transaction isolation levels

### 📚 Additional Resources

* SQL Server stored procedures documentation
* View optimization techniques
* Transaction management best practices

---

## Week 7: Indexes & Performance Optimization

### 🧠 Overview

Learn about indexing and query optimization to improve SQL performance.

### 📘 Topics Covered

1. **Indexes**

   * Clustered, non-clustered, covering, filtered
   * Index maintenance

2. **Query Optimization**

   * Execution plans, statistics, query tuning, cost analysis

### 💻 Code Examples

```sql
-- Creating a basic index
CREATE INDEX idx_last_name ON employees(last_name);

-- Analyzing query performance
EXPLAIN ANALYZE
SELECT p.product_name, c.category_name
FROM products p
JOIN categories c ON p.category_id = c.category_id
WHERE p.price > 100;
```

### 🧩 Practice Exercises

1. Create appropriate indexes
2. Analyze execution plans
3. Optimize queries
4. Maintain indexes

### 🎯 Key Concepts to Master

* Index types and use cases
* Query plan analysis
* Statistics management
* Performance monitoring

### 📚 Additional Resources

* SQL Server Index Design Guide
* Query Performance Tuning
* Execution Plan Analysis

---

## Week 8: Advanced Topics

### 🧠 Overview

Covers transactions, window functions, and triggers for advanced SQL operations.

### 📘 Topics Covered

1. **Transactions**

   * ACID properties, isolation levels, deadlock handling

2. **Window Functions**

   * ROW_NUMBER(), RANK(), DENSE_RANK(), LAG/LEAD

3. **Triggers**

   * AFTER, INSTEAD OF triggers
   * Error handling

### 💻 Code Examples

```sql
-- Transaction example
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
IF @@ERROR = 0 COMMIT TRANSACTION;
ELSE ROLLBACK TRANSACTION;

-- Window functions
SELECT employee_name, department, salary,
ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as salary_rank
FROM employees;

-- AFTER trigger
CREATE TRIGGER trg_update_audit
ON employees
AFTER UPDATE
AS
BEGIN
    INSERT INTO employee_audit(employee_id, changed_at, action)
    SELECT i.employee_id, GETDATE(), 'UPDATE'
    FROM inserted i;
END;
```

### 🧩 Practice Exercises

1. Implement transaction management
2. Write complex window functions
3. Create triggers
4. Handle deadlocks and concurrency

### 🎯 Key Concepts to Master

* Transaction isolation levels
* Window function types
* Trigger execution order
* Concurrency patterns

### 📚 Additional Resources

* Advanced Transaction Management
* Window Functions Documentation
* Trigger Design Patterns
* Concurrency Control Strategies

