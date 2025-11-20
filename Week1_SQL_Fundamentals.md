# Week 1: SQL Fundamentals & Basic Queries

# SQL Learning Notes

## 1. Database Basics

### **What is a Database?**

A **database** is an organized collection of data that allows easy access, management, and updating. Databases help store information efficiently so applications and users can retrieve it quickly.

### **Types of Databases**

* **Relational Databases (RDBMS)**: Store data in tables (rows and columns). Examples: MySQL, PostgreSQL, SQL Server.
* **NoSQL Databases**: Store data in non‑tabular formats such as documents, key-value pairs, graphs, or wide-columns. Examples: MongoDB, Redis.
* **Hierarchical Databases**: Data stored in tree‑like structures.
* **Network Databases**: More flexible connections than hierarchical databases using graph‑like structures.

### **RDBMS Concepts**

* **Table**: Structure that stores data in rows and columns.
* **Row (Record)**: A single entry inside a table.
* **Column (Field)**: Defines the type of data stored.
* **Primary Key**: Uniquely identifies each row.
* **Foreign Key**: Connects one table to another.
* **SQL**: Language used to interact with the database.

---

## 2. Basic SQL Commands

### **SELECT Statement**

Used to retrieve data from a table.

```sql
SELECT * FROM students;
SELECT name, age FROM users;
```

### **WHERE Clause**

Filters data based on conditions.

```sql
SELECT * FROM employees
WHERE salary > 3000;
```

### **ORDER BY**

Sorts data in ascending or descending order.

```sql
SELECT * FROM products
ORDER BY price ASC;

SELECT * FROM products
ORDER BY price DESC;
```

### **LIMIT / TOP**

Used to get a specific number of results.

**MySQL / PostgreSQL:**

```sql
SELECT * FROM logs
LIMIT 10;
```

**SQL Server:**

```sql
SELECT TOP 10 * FROM logs;
```


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

### **1. Create a Simple Database with a `customers` Table**

Learn how to create a database and define a basic table structure.

Example:

```sql
CREATE DATABASE shop;

USE shop;

CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(150),
    region VARCHAR(50),
    created_at DATE
);
```

---

### **2. Write Queries to Select All Customers**

Retrieve all records from the customers table.

Example:

```sql
SELECT *
FROM customers;
```

---

### **3. Filter Customers by Specific Criteria**

Use `WHERE` conditions to filter data.

Examples:

```sql
-- Customers in a specific region
SELECT *
FROM customers
WHERE region = 'East';

-- Customers created after a date
SELECT *
FROM customers
WHERE created_at > '2024-01-01';
```

---

### **4. Sort Results Using Different Columns**

Practice sorting with `ORDER BY`.

Examples:

```sql
-- Sort alphabetically by name
SELECT *
FROM customers
ORDER BY name ASC;

-- Sort newest customers first
SELECT *
FROM customers
ORDER BY created_at DESC;
```

---

### **5. Limit Query Results to a Specific Number of Rows**

Use `LIMIT` to control how many rows are returned.

Examples:

```sql
-- Return first 5 rows
SELECT *
FROM customers
LIMIT 5;

-- Return top 10 customers ordered by name
SELECT *
FROM customers
ORDER BY name
LIMIT 10;
```

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
