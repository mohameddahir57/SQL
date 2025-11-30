-- Mohamed Dahir Osman
-- SQL Course Beginner to Advance

# SQL Course Notes

## 1. Creating and Managing Databases

### Creating a Database
```sql
-- Basic syntax
CREATE DATABASE database_name;

-- Example
CREATE DATABASE school_management;

-- Create database if it doesn't exist
CREATE DATABASE IF NOT EXISTS school_management;
```

### Using a Database
```sql
-- Switch to a database
USE school_management;

-- Show all databases
SHOW DATABASES;

-- Delete a database (be careful!)
DROP DATABASE database_name;
```

## 2. Creating Tables

### Basic Table Creation Syntax
```sql
CREATE TABLE table_name (
    column1 datatype1 constraints,
    column2 datatype2 constraints,
    column3 datatype3 constraints,
    ...
);
```

### Example 1: Creating a Simple Table
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    date_of_birth DATE
);
```

### Common Data Types
- INT / INTEGER: Whole numbers
- VARCHAR(n): Variable-length text with maximum length n
- TEXT: Variable unlimited-length text
- DATE: Date values
- DECIMAL(p,s): Decimal numbers with p total digits and s decimal places
- BOOLEAN: True/False values

### Common Constraints
- PRIMARY KEY: Unique identifier for the row
- NOT NULL: Column cannot contain NULL value
- UNIQUE: All values in column must be unique
- DEFAULT value: Sets a default value for the column
- FOREIGN KEY: Creates a link between tables

### Practice Exercises

1. Create a Database and Table:
```sql
-- First, create a bookstore database
CREATE DATABASE bookstore;

-- Switch to the bookstore database
USE bookstore;

-- Then create a books table
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author VARCHAR(100) NOT NULL,
    publication_date DATE,
    isbn VARCHAR(13) UNIQUE,
    price DECIMAL(10,2)
);
```

2. Check Your Work:
```sql
-- Show all tables in the current database
SHOW TABLES;

-- Show the structure of your table
DESCRIBE books;
```

## 3. SQL SHOW Commands

### Basic SHOW Commands
```sql
-- List all databases on the server
SHOW DATABASES;

-- List all tables in the current database
SHOW TABLES;

-- List all tables in a specific database
SHOW TABLES FROM database_name;

-- Show all columns in a table
SHOW COLUMNS FROM table_name;
-- OR use this alternative
DESCRIBE table_name;

-- Show create statement for a table
SHOW CREATE TABLE table_name;

-- Show table status (includes info about table size, rows, etc.)
SHOW TABLE STATUS;

-- Show all users and their hosts
SHOW USERS;

-- Show all active processes
SHOW PROCESSLIST;

-- Show all available character sets
SHOW CHARACTER SET;

-- Show all available collations
SHOW COLLATION;

-- Show server status variables
SHOW STATUS;

-- Show server system variables
SHOW VARIABLES;

-- Show grants for current user
SHOW GRANTS;
```

### Examples of Using SHOW Commands

```sql
-- Example 1: Create a database and inspect it
CREATE DATABASE bookstore;
SHOW DATABASES;

-- Example 2: Create a table and inspect its structure
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(200)
);
SHOW TABLES;
DESCRIBE books;

-- Example 3: Show detailed table information
SHOW CREATE TABLE books;

-- Example 4: Show tables with a specific pattern
SHOW TABLES LIKE 'book%';

-- Example 5: Show column information with specific pattern
SHOW COLUMNS FROM books LIKE '%id';
```

### Tips for Using SHOW Commands
- Use SHOW commands for database administration and debugging
- Combine with LIKE for pattern matching
- DESCRIBE is a shorthand for SHOW COLUMNS FROM
- Most SHOW commands can be combined with WHERE clauses
- Use these commands to verify your database structure

## 4. SQL DESCRIBE Commands

### Basic DESCRIBE Syntax
```sql
-- Basic syntax
DESCRIBE table_name;
-- OR
DESC table_name;
-- OR (full form)
SHOW COLUMNS FROM table_name;
```

### Understanding DESCRIBE Output
When you run DESCRIBE, you get these columns:
- Field: Column name
- Type: Data type of the column
- Null: Whether NULL values are allowed (YES or NO)
- Key: Type of key (PRI for primary, UNI for unique, MUL for multiple occurrences)
- Default: Default value for the column
- Extra: Additional information (auto_increment, etc.)

### Examples of DESCRIBE Usage

```sql
-- Example 1: Create and describe a simple table
CREATE TABLE employees (
    emp_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    hire_date DATE DEFAULT CURRENT_DATE,
    salary DECIMAL(10,2)
);

DESCRIBE employees;
/* Output will look like:
Field      | Type         | Null | Key | Default | Extra
-----------+--------------+------+-----+---------+----------------
emp_id     | INT         | NO   | PRI | NULL    | auto_increment
first_name | VARCHAR(50) | NO   |     | NULL    |
last_name  | VARCHAR(50) | NO   |     | NULL    |
hire_date  | DATE        | YES  |     | CURRENT_DATE |
salary     | DECIMAL(10,2)| YES |     | NULL    |
*/

-- Example 2: Describe specific column
DESCRIBE employees emp_id;

-- Example 3: Using DESCRIBE with wildcards
DESCRIBE employees '%name';  -- Shows all columns ending with 'name'

-- Example 4: Describe table from specific database
DESCRIBE database_name.table_name;
```

### Variations and Related Commands
```sql
-- Get detailed table creation statement
SHOW CREATE TABLE employees;

-- Get full table information
SHOW FULL COLUMNS FROM employees;

-- Get basic table status
SHOW TABLE STATUS LIKE 'employees';

-- Get index information
SHOW INDEX FROM employees;
```

### Common Use Cases
1. **Schema Verification**:
```sql
-- Check if table exists and its structure
DESCRIBE employees;
```

2. **Column Information**:
```sql
-- Find specific column details
DESCRIBE employees salary;
```

3. **Key Detection**:
```sql
-- Check which columns are keys
DESCRIBE employees;
-- Look for 'PRI', 'UNI', or 'MUL' in Key column
```

### Tips for Using DESCRIBE
- Use DESCRIBE before modifying tables to verify structure
- Combine with SHOW CREATE TABLE for complete table definition
- Use wildcards to filter specific columns
- Remember DESC is a shorthand for DESCRIBE
- Use SHOW FULL COLUMNS when you need additional column information like comments and collation

Sure! Here's a clear breakdown of **each `INSERT` command** you've listed — **with distinct, complete, and simple examples** for every concept. Each example uses either the `products` or `product_details` table.

---








## 1. **Basic INSERT Syntax**

### Example: Insert one product with all columns specified

```sql
INSERT INTO products (name, price, stock)
VALUES ('Smartphone', 499.99, 80);
```

---

##  2. **Multiple Rows INSERT**

### Example: Insert several products in one command

```sql
INSERT INTO products (name, price, stock)
VALUES 
    ('Tablet', 299.99, 60),
    ('Charger', 19.99, 200),
    ('Case', 9.99, 500);
```

---

##  3. **INSERT with SELECT**

### Example: Copy from `temp_products` into `products`

```sql
-- First: Create and insert into a temp table
CREATE TABLE temp_products (
    name VARCHAR(100),
    price DECIMAL(10,2)
);
INSERT INTO temp_products VALUES ('Speaker', 89.99), ('Camera', 399.99);

-- Then: Insert from temp_products to products
INSERT INTO products (name, price)
SELECT name, price FROM temp_products;
```

---

##  4. **INSERT IGNORE**

### Example: Avoid error if duplicate primary key

```sql
-- Assume product_id 1 already exists
INSERT IGNORE INTO products (product_id, name, price)
VALUES (1, 'Duplicate Phone', 399.99);  -- This row will be ignored
```

---

##  5. **INSERT ... ON DUPLICATE KEY UPDATE**

### Example: Insert or update product if `product_id` already exists

```sql
INSERT INTO products (product_id, name, price, stock)
VALUES (1, 'Smartphone Pro', 599.99, 70)
ON DUPLICATE KEY UPDATE
    price = VALUES(price),
    stock = VALUES(stock);
```

---

##  6. **INSERT with Calculated Expressions**

### Example: Use an average price for a bundle product

```sql
INSERT INTO products (name, price, stock)
VALUES ('Accessory Bundle', (SELECT AVG(price) FROM products) * 0.9, 25);
```

---

##  7. **INSERT with SET Syntax**

### Example: Use SET to insert values instead of VALUES keyword

```sql
INSERT INTO products
SET name = '3D Printer',
    price = 499.99,
    stock = 12;
```

---

##  8. **INSERT with JSON**

### Example: Insert a product with JSON details

```sql
CREATE TABLE product_details (
    id INT PRIMARY KEY,
    details JSON
);

INSERT INTO product_details (id, details)
VALUES (1, '{"color": "black", "weight": "2.5kg", "features": ["wireless", "bluetooth"]}');
```

---

##  9. **Best Practice: Always Specify Columns**

### Good:

```sql
INSERT INTO products (name, price, stock)
VALUES ('Microphone', 89.99, 40);
```

### Bad (don’t do this):

```sql
INSERT INTO products
VALUES (NULL, 'Microphone', 89.99, 40, NULL);
```

---

##  10. **Best Practice: Use Batch Inserts**

### Good (batch insert):

```sql
INSERT INTO products (name, price)
VALUES 
    ('Item A', 10.00),
    ('Item B', 20.00),
    ('Item C', 30.00);
```

### Bad (individual insert):

```sql
INSERT INTO products (name, price) VALUES ('Item A', 10.00);
INSERT INTO products (name, price) VALUES ('Item B', 20.00);
INSERT INTO products (name, price) VALUES ('Item C', 30.00);
```

---

##  11. **Best Practice: Use Transactions**

### Example: Multiple inserts in a transaction block

```sql
START TRANSACTION;

INSERT INTO products (name, price)
VALUES ('Webcam Pro', 149.99);

INSERT INTO products (name, price)
VALUES ('Tripod', 39.99);

COMMIT;
```

---

##  12. **Error Handling: Duplicate Key**

### Solution 1: IGNORE

```sql
INSERT IGNORE INTO products (product_id, name)
VALUES (2, 'Tablet 2024');
```

### Solution 2: ON DUPLICATE KEY UPDATE

```sql
INSERT INTO products (product_id, name)
VALUES (2, 'Tablet 2024')
ON DUPLICATE KEY UPDATE name = VALUES(name);
```

---

##  13. **Error Handling: Data Type Mismatch**

### Example: Cast a string to the correct data type

```sql
INSERT INTO products (name, price)
VALUES ('USB Cable', CAST('15.99' AS DECIMAL(10,2)));
```

---
That's great to hear! Since you've learned `INSERT` commands — which are about **adding data** — the natural next step is to learn how to **retrieve and view data**.

---

##  **Next Recommended Topic: SQL `SELECT` Statements**

### Why?

`SELECT` is the **most important** SQL command. It lets you **query data** from tables in flexible, powerful ways.

---

##  What You’ll Learn in the `SELECT` Topic:

### 1. **Basic SELECT Syntax**

```sql
SELECT * FROM island;
SELECT name, country FROM island;
```

### 2. **Filtering with WHERE**

```sql
SELECT * FROM island WHERE country = 'Indonesia';
```

### 3. **Ordering with ORDER BY**

```sql
SELECT * FROM island ORDER BY area_km2 DESC;
```

### 4. **Limiting Rows**

```sql
SELECT * FROM island LIMIT 5;
```

### 5. **Using Aliases**

```sql
SELECT name AS island_name FROM island;
```

### 6. **Aggregate Functions**

```sql
SELECT COUNT(*), AVG(area_km2) FROM island;
```

### 7. **GROUP BY and HAVING**

```sql
SELECT country, COUNT(*) FROM island GROUP BY country;
```

### 8. **Joins (Later sub-topic)**

```sql
SELECT i.name, d.metadata
FROM island i
JOIN island_info d ON i.id = d.island_id;
```

---
Great! Let's dive into the **`UPDATE` statement**, which is used to **modify existing data** in a SQL table.

---







## 🔧 What Is `UPDATE`?

The `UPDATE` statement allows you to **change values** in one or more columns **for existing rows**.

---

##  Basic Syntax

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

  **Always use a `WHERE` clause**, or else **all rows will be updated**.

---

##  Let's Use Your `island` Table

Assume your table looks like this:

```sql
SELECT * FROM island;

-- id | name       | country      | area_km2 | population
--  1 | Bora Bora  | France       | 30.55    | 10500
--  2 | Bali       | Indonesia    | 5780.00  | 4230050
--  3 | Maui       | USA          | 1883.50  | 144444
```

---

##  1. **Update One Column**

###   Example: Change population of Bora Bora

```sql
UPDATE island
SET population = 12000
WHERE name = 'Bora Bora';
```

---

##  2. **Update Multiple Columns**

###   Example: Update both area and population

```sql
UPDATE island
SET area_km2 = 32.00, population = 13000
WHERE name = 'Bora Bora';
```

---

##  3. **Update All Rows (Use with caution!)**

###   Example: Increase all island populations by 5%

```sql
UPDATE island
SET population = population * 1.05;
```

  This affects every row. Only do this intentionally!

---

##  4. **Update with a Condition**

###   Example: Set country to 'USA' where area is above 2000

```sql
UPDATE island
SET country = 'USA'
WHERE area_km2 > 2000;
```

---

##   5. **Update Using Subquery**

###   Example: Set population of smallest island to 999

```sql
UPDATE island
SET population = 999
WHERE area_km2 = (SELECT MIN(area_km2) FROM island);
```

---

##   6. **Using LIMIT (in MySQL)**

```sql
UPDATE island
SET population = 8888
ORDER BY population ASC
LIMIT 1;
```

  This updates just 1 row — the one with the **smallest population**.

---

##  Best Practices

| Tip                         | Why                                        |
| --------------------------- | ------------------------------------------ |
| Always use `WHERE`          | Prevents accidental updates to all rows    |
| Backup before major updates | To recover if something goes wrong         |
| Test with `SELECT` first    | Run a `SELECT` with your `WHERE` to verify |

---

##  Common Mistakes

###  Forgets WHERE:

```sql
UPDATE island SET population = 0;  -- updates **all rows**!
```

###  Wrong column name:

```sql
UPDATE island SET populations = 1000;  -- error: wrong column
```
Awesome — let’s move on to **`DELETE`**, the SQL command used to **remove rows** from a table.

---








##  What is `DELETE` in SQL?

`DELETE` permanently removes **one or more rows** from a table.

---

##  Basic Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

  **No `WHERE` = deletes all rows!**

---

##  Using Your `island` Table

Let’s say your table still looks like this:

```sql
-- id | name       | country      | area_km2 | population
-- 1  | Bora Bora  | France       | 30.55    | 10500
-- 2  | Bali       | Indonesia    | 5980.00  | 4230050
-- 3  | Maui       | Unknown      | 1883.50  | 144444
-- 4  | Jeju       | South Korea  | 1848.00  | 700000
-- 5  | Sicily     | Italy        | 25711.00 | 4500000
```

---

##  1. **Delete a Specific Row**

###  Delete Maui

```sql
DELETE FROM island
WHERE name = 'Maui';
```

---

##  2. **Delete Multiple Rows**

###   Delete all islands in `France` or `Italy`

```sql
DELETE FROM island
WHERE country IN ('France', 'Italy');
```

---

##   3. **Delete Using Numeric Condition**

###   Delete islands smaller than 1000 km²

```sql
DELETE FROM island
WHERE area_km2 < 1000;
```

---

##   4. **Delete All Rows** (Be Careful!)

```sql
DELETE FROM island;
```

 Removes **everything** but keeps the table structure.

---

##   5. **Delete with LIMIT** (MySQL only)

```sql
DELETE FROM island
ORDER BY population ASC
LIMIT 1;
```

  Deletes the **least populated** island.

---

##  6. **Delete with Subquery**

###   Delete island with **max area**

```sql
DELETE FROM island
WHERE area_km2 = (SELECT MAX(area_km2) FROM island);
```

---

##   DELETE vs DROP vs TRUNCATE

| Command    | What it does              | Keeps table? | Rollbackable?            |
| ---------- | ------------------------- | ------------ | ------------------------ |
| `DELETE`   | Removes rows (with WHERE) |   Yes        |  Yes (with transaction) |
| `DROP`     | Deletes entire table      |   No         |  No                     |
| `TRUNCATE` | Deletes all rows fast     |   Yes        |  No (in most systems)   |

---

##  Best Practices

1. **ALWAYS use `WHERE` unless you mean to delete all rows**.
2. Use `SELECT` first to preview what you're deleting.
3. Wrap in a **transaction** if needed:

```sql
START TRANSACTION;
DELETE FROM island WHERE name = 'Bali';
ROLLBACK;  -- or COMMIT;
```







##  What is a `JOIN`?

A **JOIN** lets you **combine rows from two or more tables** based on a related column between them — usually a **foreign key**.

---

##  Example Tables

Let’s say you have these two tables:

### 1. `island`

```sql
-- id | name       | country      | area_km2
-- 1  | Bali       | Indonesia    | 5780.00
-- 2  | Maui       | USA          | 1883.50
-- 3  | Sicily     | Italy        | 25711.00
```

### 2. `island_info`

```sql
-- id | island_id | population | famous_for
-- 1  | 1         | 4230050    | Tourism
-- 2  | 2         | 144444     | Volcanoes
-- 3  | 3         | 5000000    | Culture
```

>  `island_info.island_id` is a **foreign key** linking to `island.id`.

---

##  1. **INNER JOIN**

Only returns rows with matching values in **both tables**.

```sql
SELECT island.name, island_info.population, island_info.famous_for
FROM island
INNER JOIN island_info
  ON island.id = island_info.island_id;
```

** Result:**

| name   | population | famous\_for |
| ------ | ---------- | ----------- |
| Bali   | 4230050    | Tourism     |
| Maui   | 144444     | Volcanoes   |
| Sicily | 5000000    | Culture     |

---

##  2. **LEFT JOIN**

Returns **all rows from the left table** (`island`), and matched rows from the right (`island_info`), or `NULL` if no match.

```sql
SELECT island.name, island_info.population
FROM island
LEFT JOIN island_info
  ON island.id = island_info.island_id;
```

> Useful to **keep all islands**, even if info is missing.

---

##  3. **RIGHT JOIN**

Returns **all rows from the right table**, and matched rows from the left.

```sql
SELECT island_info.famous_for, island.name
FROM island_info
RIGHT JOIN island
  ON island.id = island_info.island_id;
```

> Not supported in all databases (like SQLite), but common in MySQL/PostgreSQL.

---

##  4. **FULL OUTER JOIN**

Returns all records when there's a match in **either table**.

```sql
SELECT island.name, island_info.population
FROM island
FULL OUTER JOIN island_info
  ON island.id = island_info.island_id;
```

>  Requires a DB that supports full outer joins (like PostgreSQL).

---

##  5. **CROSS JOIN**

Returns **every combination** of rows from both tables.

```sql
SELECT island.name, island_info.famous_for
FROM island
CROSS JOIN island_info;
```

>  Be careful — the result can grow **very large**!

---

##  Real-Life Use Case

Imagine you have:

* A `customers` table
* An `orders` table with a `customer_id`

You can join them:

```sql
SELECT customers.name, orders.order_date
FROM customers
JOIN orders ON customers.id = orders.customer_id;
```

---

##  JOIN Types Summary

| Type    | Returns...                            |
| ------- | ------------------------------------- |
| `INNER` | Only matched rows in both tables      |
| `LEFT`  | All from left + matched from right    |
| `RIGHT` | All from right + matched from left    |
| `FULL`  | All rows from both (if supported)     |
| `CROSS` | Every combination (Cartesian product) |

---





Awesome! Let’s learn about **Aggregate Functions** and **`GROUP BY`**, which let you **summarize data** in powerful ways.

---

## What Are Aggregate Functions?

Aggregate functions perform **calculations** on a **group of rows**, returning **a single value**.

###  Common Aggregate Functions:

| Function  | What it does            |
| --------- | ----------------------- |
| `COUNT()` | Counts rows             |
| `SUM()`   | Adds up numeric values  |
| `AVG()`   | Calculates average      |
| `MAX()`   | Gets the largest value  |
| `MIN()`   | Gets the smallest value |

---

##  What is `GROUP BY`?

`GROUP BY` groups rows **with the same value** in specified columns so you can apply aggregate functions **per group**.

---

## Example: island Table

Let’s say we have this data:

| id | name   | country   | area\_km2 | population |
| -- | ------ | --------- | --------- | ---------- |
| 1  | Bali   | Indonesia | 5780.00   | 4230050    |
| 2  | Lombok | Indonesia | 4514.00   | 3160000    |
| 3  | Maui   | USA       | 1883.50   | 144444     |
| 4  | Oahu   | USA       | 1545.00   | 953207     |
| 5  | Sicily | Italy     | 25711.00  | 5000000    |

---

##  1. `COUNT()` — How many islands per country?

```sql
SELECT country, COUNT(*) AS island_count
FROM island
GROUP BY country;
```

**Result:**

| country   | island\_count |
| --------- | ------------- |
| Indonesia | 2             |
| USA       | 2             |
| Italy     | 1             |

---

##  2. `SUM()` — Total population per country

```sql
SELECT country, SUM(population) AS total_population
FROM island
GROUP BY country;
```

---

##  3. `AVG()` — Average island size per country

```sql
SELECT country, AVG(area_km2) AS avg_area
FROM island
GROUP BY country;
```

---

##  4. `MAX()` and `MIN()` — Largest and smallest islands per country

```sql
SELECT country, MAX(area_km2) AS largest_island, MIN(area_km2) AS smallest_island
FROM island
GROUP BY country;
```

---

##  5. `GROUP BY` with `ORDER BY`

```sql
SELECT country, COUNT(*) AS island_count
FROM island
GROUP BY country
ORDER BY island_count DESC;
```

---

##  6. Filtering with `HAVING` (After `GROUP BY`)

```sql
SELECT country, COUNT(*) AS island_count
FROM island
GROUP BY country
HAVING island_count > 1;
```

> `HAVING` is like `WHERE`, but **used with aggregates**.

---

##  7. Without GROUP BY — Aggregate all rows

```sql
SELECT COUNT(*) AS total_islands, SUM(population) AS total_population
FROM island;
```

---

## Summary:

| Clause     | Used For                  |
| ---------- | ------------------------- |
| `GROUP BY` | Grouping rows by a column |
| `HAVING`   | Filtering groups          |
| `ORDER BY` | Sorting result            |
| `WHERE`    | Filtering individual rows |

---



Great! Let’s dive into ** Subqueries** — one of the most powerful features in SQL.

---

##  What is a Subquery?

A **subquery** (or nested query) is a `SELECT` statement **inside another SQL statement**.

Subqueries can be used in:

* `SELECT`
* `FROM`
* `WHERE`
* `HAVING`

---

##  Example Table: `island`

| id | name   | country   | area\_km2 | population |
| -- | ------ | --------- | --------- | ---------- |
| 1  | Bali   | Indonesia | 5780      | 4230050    |
| 2  | Lombok | Indonesia | 4514      | 3160000    |
| 3  | Maui   | USA       | 1883.5    | 144444     |
| 4  | Oahu   | USA       | 1545      | 953207     |
| 5  | Sicily | Italy     | 25711     | 5000000    |

---

##  1. Subquery in `WHERE` Clause

Get islands with **above-average population**:

```sql
SELECT name, population
FROM island
WHERE population > (
  SELECT AVG(population) FROM island
);
```

---

##  2. Subquery in `SELECT` Clause

Show island + country + average area of **all islands**:

```sql
SELECT name, country,
  (SELECT AVG(area_km2) FROM island) AS avg_area_all
FROM island;
```

---

##  3. Subquery in `FROM` Clause

Use subquery as a temporary table:

```sql
SELECT country, island_count
FROM (
  SELECT country, COUNT(*) AS island_count
  FROM island
  GROUP BY country
) AS sub;
```

---

##  4. Subquery with `IN`

Get islands in the **same countries** as those with more than 2 islands:

```sql
SELECT name
FROM island
WHERE country IN (
  SELECT country
  FROM island
  GROUP BY country
  HAVING COUNT(*) > 1
);
```

---

##  5. Subquery with `EXISTS`

Check if a country has **any island with area > 10,000**:

```sql
SELECT DISTINCT country
FROM island i
WHERE EXISTS (
  SELECT 1
  FROM island sub
  WHERE sub.country = i.country AND sub.area_km2 > 10000
);
```

---

##  6. Correlated Subquery

Compare each island to the **average population of its own country**:

```sql
SELECT name, population, country
FROM island i1
WHERE population > (
  SELECT AVG(population)
  FROM island i2
  WHERE i2.country = i1.country
);
```

>   Correlated subqueries refer to the **outer query’s row**, so they are **re-evaluated for each row**.

---

##  Subquery Tips

| Use Case            | Best Option         |
| ------------------- | ------------------- |
| Return single value | Scalar subquery     |
| Use in WHERE/IN     | Subquery in WHERE   |
| Build virtual table | Subquery in FROM    |
| Compare each row    | Correlated subquery |

---





Awesome!  Let's level up with **Advanced `SELECT` Filtering** — techniques to filter rows precisely using SQL.

---

##  1. `BETWEEN` – Filter a Range of Values

```sql
SELECT name, area_km2
FROM island
WHERE area_km2 BETWEEN 1000 AND 6000;
```

>  Includes both 1000 **and** 6000

---

##  2. `IN` – Match Against Multiple Values

```sql
SELECT name, country
FROM island
WHERE country IN ('Indonesia', 'Italy');
```

>  Same as using multiple `OR` conditions

---

##  3. `LIKE` – Pattern Matching

```sql
SELECT name
FROM island
WHERE name LIKE 'M%';  -- Starts with M
```

### Patterns:

| Pattern   | Matches                        |
| --------- | ------------------------------ |
| `'A%'`    | Starts with A                  |
| `'%land'` | Ends with 'land'               |
| `'%i%'`   | Contains 'i'                   |
| `'M___'`  | Starts with 'M' + 3 characters |

---

##  4. `IS NULL` / `IS NOT NULL`

```sql
SELECT name
FROM island
WHERE population IS NULL;
```

> Use this to find **missing** or **empty values**

---

##  5. Logical Operators: `AND`, `OR`, `NOT`

```sql
SELECT name
FROM island
WHERE area_km2 > 1000 AND country = 'USA';
```

```sql
SELECT name
FROM island
WHERE NOT country = 'Italy';
```

---

##  6. `CASE` for Conditional Filtering in Results

```sql
SELECT name, country,
  CASE
    WHEN population > 1000000 THEN 'Large'
    WHEN population > 500000 THEN 'Medium'
    ELSE 'Small'
  END AS population_group
FROM island;
```

---

##  7. Combine with `ORDER BY`, `LIMIT`, `DISTINCT`

```sql
-- Top 3 largest islands
SELECT name, area_km2
FROM island
ORDER BY area_km2 DESC
LIMIT 3;
```

```sql
-- Unique countries
SELECT DISTINCT country
FROM island;
```

---

##  Bonus: Filter with Subquery

```sql
SELECT name
FROM island
WHERE area_km2 > (
  SELECT AVG(area_km2) FROM island
);
```

---

##  Summary Table

| Clause       | Use Case                     |
| ------------ | ---------------------------- |
| `BETWEEN`    | Filter in range              |
| `IN`         | Match any from a list        |
| `LIKE`       | Match patterns in text       |
| `IS NULL`    | Filter missing values        |
| `AND/OR/NOT` | Combine conditions logically |
| `CASE`       | Conditional expressions      |
| `LIMIT`      | Limit number of results      |

---








Perfect!  Let’s learn about **Table Design & Relationships** — the foundation of a good database structure.

---

##  1. Why Table Design Matters

Designing tables well ensures:

*  Organized, reliable data
*  Efficient queries
*  Easy maintenance
*  Enforced data rules (integrity)

---

##  2. Table Components

Each SQL table has:

| Part            | Description                                     |
| --------------- | ----------------------------------------------- |
| **Columns**     | The data fields (name, age, etc.)               |
| **Data Types**  | Type for each column (INT, VARCHAR, DATE, etc.) |
| **Constraints** | Rules like NOT NULL, UNIQUE, etc.               |
| **Keys**        | Primary and foreign keys                        |

---

##  3. Common Constraints

| Constraint    | Meaning                              |
| ------------- | ------------------------------------ |
| `PRIMARY KEY` | Uniquely identifies a row            |
| `FOREIGN KEY` | Links to another table’s primary key |
| `NOT NULL`    | Field must have a value              |
| `UNIQUE`      | No duplicate values allowed          |
| `DEFAULT`     | Value to use if none provided        |
| `CHECK`       | Must meet a specific condition       |

---

##  4. Primary Keys

* Each table **must have a primary key**
* Can be one column or multiple (composite key)

```sql
CREATE TABLE country (
    code CHAR(2) PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

---

##  5. Foreign Keys – Table Relationships

* A foreign key in one table **references a primary key in another**
* Used to **connect data** between tables

### One-to-Many Relationship

```sql
CREATE TABLE island (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    country_code CHAR(2),
    FOREIGN KEY (country_code) REFERENCES country(code)
);
```

Here, many islands can belong to one country.

---

##  6. Types of Relationships

| Type         | Description                                |
| ------------ | ------------------------------------------ |
| One-to-One   | One record per side                        |
| One-to-Many  | One row in Table A maps to many in Table B |
| Many-to-Many | Needs a third (join) table                 |

### Many-to-Many Example

```sql
-- Students and Courses
CREATE TABLE student_course (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student(id),
    FOREIGN KEY (course_id) REFERENCES course(id)
);
```

---

##  7. Naming Conventions (Best Practice)

| Element    | Example          |
| ---------- | ---------------- |
| Table name | `students`       |
| PK column  | `id`             |
| FK column  | `student_id`     |
| Join table | `student_course` |

---

##  Example: Full Setup

```sql
CREATE TABLE country (
    code CHAR(2) PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE island (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    area_km2 DECIMAL(10,2),
    population INT,
    country_code CHAR(2),
    FOREIGN KEY (country_code) REFERENCES country(code)
);
```

---

##  Summary

* **Primary Key** = Unique row ID
* **Foreign Key** = Connects tables
* **Constraints** = Enforce rules
* **Relationships** = Link data across tables

---











Awesome!  Let’s explore **Transactions and Rollbacks** — critical for ensuring safe and reliable database operations.

---

##  What is a Transaction?

A **transaction** is a group of one or more SQL operations that are **executed as a single unit**.
If **any part fails**, the **whole transaction can be rolled back**.

Think of it like:

>  “Transfer money from Account A to Account B — but only if both operations succeed.”

---

## Transaction Properties (ACID)

| Property            | Meaning                                      |
| ------------------- | -------------------------------------------- |
| **A** – Atomicity   | All operations succeed or none do            |
| **C** – Consistency | Keeps database in a valid state              |
| **I** – Isolation   | Transactions don’t interfere with each other |
| **D** – Durability  | Once committed, data changes are permanent   |

---

## 🔧 1. Basic Transaction Syntax

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

>  All changes are saved only when `COMMIT` is executed.

---

##  2. ROLLBACK – Undo a Transaction

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- Oops! Something’s wrong
ROLLBACK;
```

>  No changes are saved. Everything is undone.

---

##  3. COMMIT – Finalize the Transaction

```sql
COMMIT;
```

After this, the changes are **permanently saved** to the database.

---

##   4. Real-World Example

```sql
START TRANSACTION;

INSERT INTO orders (user_id, total) VALUES (5, 199.99);
UPDATE inventory SET stock = stock - 1 WHERE product_id = 10;

-- Something fails: stock is already 0
ROLLBACK;
```

> Without `ROLLBACK`, the order would be placed even if stock is unavailable!

---

##  5. Savepoints (Optional/Advanced)

You can mark points in a transaction to **partially roll back**.

```sql
START TRANSACTION;

SAVEPOINT step1;
INSERT INTO users (name) VALUES ('Alice');

SAVEPOINT step2;
INSERT INTO users (name) VALUES ('Bob');

ROLLBACK TO step1; -- Undoes Bob, keeps Alice

COMMIT;
```

---

##  Summary

| Keyword             | Description                                   |
| ------------------- | --------------------------------------------- |
| `START TRANSACTION` | Begin a new transaction                       |
| `COMMIT`            | Save all changes made in the transaction      |
| `ROLLBACK`          | Undo all changes since `START`                |
| `SAVEPOINT`         | Create a rollback marker inside a transaction |
| `ROLLBACK TO`       | Roll back to a specific savepoint             |

---







Great choice!  **Views** and **Stored Procedures** are powerful tools that help organize, simplify, and reuse SQL logic. Let’s explore both.

---

##  1. **Views** — Virtual Tables

A **View** is a saved SQL query. It acts like a table but **doesn’t store data**, only the logic.

###  Why Use Views?

* Simplify complex queries
* Improve security (hide sensitive columns)
* Reuse query logic

### 🛠 Create a View

```sql
CREATE VIEW large_islands AS
SELECT name, area_km2, country
FROM island
WHERE area_km2 > 10000;
```

###  Use the View Like a Table

```sql
SELECT * FROM large_islands;
```

###  Update or Drop the View

```sql
DROP VIEW large_islands;
```

>  Not all views are editable (depends on the query inside).

---

##  2. **Stored Procedures** — Reusable SQL Blocks

A **Stored Procedure** is a block of SQL statements saved in the database that you can **call by name**.

###  Why Use Stored Procedures?

* Reuse SQL logic
* Accept parameters
* Simplify business logic
* Improve performance (in some cases)

---

###  Create a Procedure

```sql
DELIMITER $$

CREATE PROCEDURE GetIslandsByCountry(IN country_name VARCHAR(100))
BEGIN
    SELECT name, area_km2
    FROM island
    WHERE country = country_name;
END$$

DELIMITER ;
```

---

###  Call the Procedure

```sql
CALL GetIslandsByCountry('Indonesia');
```

---

###  Drop the Procedure

```sql
DROP PROCEDURE GetIslandsByCountry;
```

---

##  Comparison Table

| Feature            | View                | Stored Procedure      |
| ------------------ | ------------------- | --------------------- |
| Acts Like a Table  | ✅                  | ❌                     |
| Stores Data        |   (just logic)      | ❌ (logic only)        |
| Accepts Parameters | ❌                   | ✅                     |
| Useful For         | Simplifying queries | Reusable operations   |
| Supports Logic     | No                  | Yes (e.g., IF, loops) |

---

##  Summary

* Use **Views** to simplify complex `SELECT` queries
* Use **Stored Procedures** for reusable, parameterized SQL logic

---









Great!  Let’s dive into **Indexes** — the secret behind fast SQL queries.

---

## 🚀 What Is an Index?

An **index** in SQL is like an index in a book — it helps you **find rows faster** without scanning the entire table.

### Without an Index:

> SQL scans the whole table (slow if millions of rows)

### With an Index:

> SQL jumps directly to matching rows (much faster!)

---

## 📚 Basic Index Syntax

```sql
CREATE INDEX index_name ON table_name(column_name);
```

### Example:

```sql
CREATE INDEX idx_country_code ON island(country_code);
```

Now, queries like this become faster:

```sql
SELECT * FROM island WHERE country_code = 'JP';
```

---

## 💡 When to Use Indexes

| Use Index If...                              |
| -------------------------------------------- |
| You **search/filter** on the column (WHERE)  |
| You **JOIN** on that column                  |
| You **ORDER BY** or **GROUP BY** that column |
| The table has **lots of rows**               |

---

## ⚠️ Indexes Are Not Free

| Downside                       | Why it matters                      |
| ------------------------------ | ----------------------------------- |
| 🐢 Slower INSERT/UPDATE/DELETE | Indexes must also be updated        |
| 💽 Uses extra disk space       | Especially with many or big indexes |
| ❌ Too many indexes = confusion | Can actually slow down performance  |

---

## 🧠 Types of Indexes

| Index Type        | Description                                   |
| ----------------- | --------------------------------------------- |
| `PRIMARY KEY`     | Automatically indexed                         |
| `UNIQUE`          | Prevents duplicates, also indexed             |
| `SINGLE COLUMN`   | Most common, fast for one-column searches     |
| `COMPOSITE INDEX` | Multi-column index (e.g. `INDEX(col1, col2)`) |
| `FULLTEXT`        | For text searching (MySQL specific)           |
| `SPATIAL`         | For geolocation queries (MySQL/PostGIS)       |

---

## 🔍 Composite Index Example

```sql
CREATE INDEX idx_island_country_area ON island(country_code, area_km2);
```

This helps queries like:

```sql
SELECT * FROM island
WHERE country_code = 'ID' AND area_km2 > 1000;
```

> Tip: SQL only uses left-most columns of a composite index efficiently.

---

## 🔧 Managing Indexes

| Task          | Command                                |
| ------------- | -------------------------------------- |
| View indexes  | `SHOW INDEX FROM table_name;`          |
| Drop an index | `DROP INDEX index_name ON table_name;` |
| Rename index  | (Not all databases support directly)   |

---

## 🧠 Example: Indexed vs Non-Indexed

```sql
-- No index
SELECT * FROM island WHERE name = 'Java';

-- Add index
CREATE INDEX idx_name ON island(name);

-- Now it's fast!
```

---

## 🧪 Summary

✅ Index = fast lookup
✅ Use on WHERE, JOIN, ORDER BY
⚠️ Don’t overuse (hurts performance + space)
🛠 Use `SHOW INDEX` to review what's active

---







Excellent choice! 🔒 **User Permissions & Security** are essential for protecting your data and controlling access in a multi-user environment.

---

## 🔐 Why It Matters

Without proper security:

* Anyone could delete or modify critical data 😱
* Sensitive info (e.g., emails, passwords) could be leaked
* One careless user = big data loss

---

## 👤 SQL Users and Privileges

In most SQL systems (like MySQL, PostgreSQL, etc.), **users** are separate from tables and must be given **permissions**.

---

### 🧱 1. Creating a New User

```sql
-- Syntax may vary slightly by DBMS
CREATE USER 'student'@'localhost' IDENTIFIED BY 'strong_password';
```

---

### ✅ 2. Granting Permissions

```sql
-- Grant read-only access
GRANT SELECT ON school.* TO 'student'@'localhost';

-- Grant full access to a specific table
GRANT SELECT, INSERT, UPDATE, DELETE ON school.students TO 'student'@'localhost';
```

---

### ⛔ 3. Revoking Permissions

```sql
REVOKE INSERT, UPDATE ON school.students FROM 'student'@'localhost';
```

---

### 🔍 4. Check Permissions

```sql
SHOW GRANTS FOR 'student'@'localhost';
```

---

## 🔐 Common Privileges

| Privilege      | What it allows                           |
| -------------- | ---------------------------------------- |
| `SELECT`       | Read data                                |
| `INSERT`       | Add data                                 |
| `UPDATE`       | Modify data                              |
| `DELETE`       | Remove data                              |
| `ALL`          | All privileges                           |
| `CREATE`       | Create tables, databases                 |
| `DROP`         | Drop tables, databases                   |
| `GRANT OPTION` | Let user give their privileges to others |

---

### 📦 Example Use Case

```sql
-- Create user for reporting only
CREATE USER 'reporter'@'%' IDENTIFIED BY 'r3port123';

-- Give only SELECT access to everything in the company database
GRANT SELECT ON company.* TO 'reporter'@'%';
```

This user **cannot** modify or delete any data — perfect for dashboards and analytics.

---

## 🚨 Security Tips

✅ Use **strong passwords**
✅ Limit users to **only what they need**
✅ Avoid using the `root` user for apps
✅ Regularly **audit permissions**

---

## 🛑 Danger: Don't Do This

```sql
GRANT ALL PRIVILEGES ON *.* TO 'webuser'@'%' WITH GRANT OPTION;
```

This gives full, unrestricted access to everything! ❌

---

## 🔐 Summary

| Task                  | SQL Command Example                                     |
| --------------------- | ------------------------------------------------------- |
| Create user           | `CREATE USER 'bob'@'localhost' IDENTIFIED BY 'secret';` |
| Grant access          | `GRANT SELECT ON db.table TO 'bob'@'localhost';`        |
| Remove access         | `REVOKE UPDATE ON db.table FROM 'bob'@'localhost';`     |
| Check user privileges | `SHOW GRANTS FOR 'bob'@'localhost';`                    |

---






Perfect! 🗃️ **Normalization & Schema Design** are fundamental for creating efficient, reliable, and scalable databases. Let’s break it down.

---

## 🧱 What is Normalization?

**Normalization** is the process of organizing your database tables and relationships to:

* Reduce **data redundancy** (duplicate data)
* Improve **data integrity** (consistency & accuracy)
* Make updates, inserts, and deletes easier and less error-prone

---

## 📊 Normal Forms (NF) — Levels of Normalization

### 1️⃣ First Normal Form (1NF)

* Each column contains **atomic (indivisible)** values
* Each row is **unique**
* No repeating groups or arrays

**Example problem:**
A table with a column holding multiple phone numbers in one cell violates 1NF.

---

### 2️⃣ Second Normal Form (2NF)

* Meets **1NF**
* All **non-key columns** depend on the **whole primary key** (not just part of it)
* Applies mainly when primary key is composite (multiple columns)

---

### 3️⃣ Third Normal Form (3NF)

* Meets **2NF**
* No transitive dependencies: Non-key columns depend **only** on the primary key, not other non-key columns

---

## 🤔 Why Normalize?

| Benefit                | Explanation                         |
| ---------------------- | ----------------------------------- |
| Avoid Data Duplication | Less storage, less inconsistency    |
| Easier Updates         | Change data in one place, not many  |
| Better Data Integrity  | Relationships enforced through keys |
| Simplifies Maintenance | Clearer, organized schema           |

---

## 🧩 Example: Normalize a Simple Table

### Unnormalized Table: `orders`

| order\_id | customer\_name | customer\_phone | product  | quantity |
| --------- | -------------- | --------------- | -------- | -------- |
| 1         | Alice          | 123-456-7890    | Keyboard | 2        |
| 2         | Alice          | 123-456-7890    | Mouse    | 1        |
| 3         | Bob            | 987-654-3210    | Monitor  | 1        |

**Issues:**

* Customer info repeated for each order
* Hard to update customer phone without changing multiple rows

---

### Normalize into 3 Tables:

**Customers**

| customer\_id | customer\_name | customer\_phone |
| ------------ | -------------- | --------------- |
| 1            | Alice          | 123-456-7890    |
| 2            | Bob            | 987-654-3210    |

**Products**

| product\_id | product  |
| ----------- | -------- |
| 1           | Keyboard |
| 2           | Mouse    |
| 3           | Monitor  |

**Orders**

| order\_id | customer\_id | product\_id | quantity |
| --------- | ------------ | ----------- | -------- |
| 1         | 1            | 1           | 2        |
| 2         | 1            | 2           | 1        |
| 3         | 2            | 3           | 1        |

---

## 🛠 Schema Design Tips

* Use **primary keys** for uniqueness (usually auto-increment IDs)
* Define **foreign keys** to link related tables
* Choose appropriate **data types** for each column
* Avoid **nulls** where possible by using defaults or proper constraints
* Document your design for clarity

---

## 🧠 Quick Recap

| Normal Form | What it Fixes                       | Key Idea                          |
| ----------- | ----------------------------------- | --------------------------------- |
| 1NF         | Atomic values, no repeating groups  | Make each cell indivisible        |
| 2NF         | No partial dependency on part of PK | Full PK dependency                |
| 3NF         | No transitive dependency            | Non-key columns depend only on PK |

---







Great choice! 📦 **Backup & Restore** are crucial skills for protecting your data from loss or corruption. Let's dive into the essentials.

---

## 💾 What is Backup & Restore?

* **Backup**: A copy of your database data, saved safely elsewhere
* **Restore**: Using a backup to recover your database to a previous state

---

## 🔑 Why Backups Matter

* Protect against accidental deletion or data corruption
* Recover from hardware failure or software bugs
* Keep data safe during upgrades or migrations

---

## 🛠 Backup Methods

### 1. Logical Backup (SQL Dump)

* Exports database schema + data as SQL commands
* Portable across different database versions and systems
* Example (MySQL):

```bash
mysqldump -u username -p database_name > backup.sql
```

---

### 2. Physical Backup

* Copies raw database files at the filesystem level
* Faster for large databases but less portable
* Requires the database to be offline or in a consistent state

---

## 🧰 Restore Methods

### 1. Restore from SQL Dump

```bash
mysql -u username -p database_name < backup.sql
```

* Recreates tables and inserts data from the dump file

---

### 2. Restore from Physical Backup

* Depends on your database system; usually involves copying files back and restarting the server

---

## 🗓 Backup Best Practices

* Schedule **regular automated backups** (daily, weekly)
* Store backups **offsite or in the cloud**
* Test your backups periodically by restoring to a test server
* Keep multiple backup versions (last week, month, year)
* Secure backups with encryption and access controls

---

## 🔄 Example Workflow (MySQL)

1. Backup database:

```bash
mysqldump -u root -p mydatabase > mydatabase_backup_2025_06_07.sql
```

2. Restore database:

```bash
mysql -u root -p mydatabase < mydatabase_backup_2025_06_07.sql
```

---

## ⚠️ Things to Remember

* Always backup **before** making major changes
* Verify your backups are complete and uncorrupted
* Backups don’t replace good database design or security

---







Awesome! 🚀 Let’s talk about **Real-World Projects** — the best way to solidify your SQL skills by building practical, meaningful databases and queries.

---

## 🏗️ Why Real-World Projects?

* Apply everything you’ve learned in context
* Understand how different concepts fit together
* Solve real business problems
* Build a portfolio to showcase your skills

---

## 🔑 Key Elements of a Real-World Project

1. **Requirements Gathering**

   * What is the project for?
   * What data do you need to store?
   * Who will use the database, and how?

2. **Database Schema Design**

   * Tables, columns, data types
   * Primary and foreign keys
   * Relationships (1:1, 1\:N, M\:N)
   * Normalization

3. **Data Population**

   * Insert sample or real data
   * Use bulk inserts or data import tools

4. **Queries and Reports**

   * SELECT with filters, JOINs, GROUP BY
   * Aggregate functions and subqueries
   * Stored procedures and views (optional)

5. **Security and Access Control**

   * User roles and permissions

6. **Backup & Maintenance**

   * Schedule backups
   * Optimize with indexes

---

## 🛠 Sample Project Ideas

| Project               | Description                                     |
| --------------------- | ----------------------------------------------- |
| **Inventory System**  | Track products, suppliers, stock levels, sales  |
| **Library Database**  | Manage books, members, loans, returns           |
| **E-Commerce Store**  | Products, customers, orders, payments, reviews  |
| **School Management** | Students, classes, teachers, grades, attendance |
| **Travel Agency**     | Customers, bookings, destinations, payments     |

---

## 🧩 Example: Mini Inventory System Schema

```sql
CREATE TABLE products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    stock INT DEFAULT 0
);

CREATE TABLE suppliers (
    supplier_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    contact_email VARCHAR(100)
);

CREATE TABLE product_suppliers (
    product_id INT,
    supplier_id INT,
    PRIMARY KEY (product_id, supplier_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id),
    FOREIGN KEY (supplier_id) REFERENCES suppliers(supplier_id)
);
```

---

## 🚀 How to Start Your Project

1. Define the scope and requirements
2. Design tables and relationships
3. Create tables and constraints
4. Insert test data
5. Write queries to retrieve useful information
6. Add indexes and optimize
7. Add users and permissions
8. Backup your database regularly

---

---
*Note: This document will be updated with more SQL topics as we progress through the course.*

