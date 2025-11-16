# Week 4: Aggregate Functions & Grouping

## Topics:
1. Aggregate Functions
   - COUNT()
   - SUM()
   - AVG()
   - MAX()
   - MIN()
   - DISTINCT with aggregates
   
2. GROUP BY and HAVING
   - Grouping data
   - Filtering groups
   - Complex grouping scenarios
   - Multiple column grouping

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
1. Calculate sales summaries
2. Find average prices by category
3. Count distinct customers
4. Group data by multiple columns
5. Use HAVING for filtered aggregates
6. Combine aggregates with joins

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