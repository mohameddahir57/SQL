# Week 1: SQL Fundamentals & Basic Queries

## Topics:
1. Database basics
   - What is a database?
   - Types of databases
   - RDBMS concepts
   
2. Basic SQL Commands
   - SELECT statement
   - WHERE clause
   - ORDER BY
   - LIMIT/TOP

## Examples:
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
```

## Practice Exercises:
1. Create a simple database with a customers table
2. Write queries to select all customers
3. Filter customers by specific criteria
4. Sort results using different columns
5. Limit query results to specific number of rows

## Key Concepts to Master:
- Understanding database structure
- Basic query syntax
- Column and table naming conventions
- Comparison operators
- Sorting and filtering basics

## Additional Resources:
- W3Schools SQL Tutorial
- PostgreSQL Documentation
- MySQL Documentation
- SQL Server Documentation 