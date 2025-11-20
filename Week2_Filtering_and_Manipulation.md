# Week 2: Filtering & Data Manipulation

## Topics:
1. Advanced WHERE Clauses
   - AND, OR, NOT operators
   - IN operator
   - BETWEEN operator
   - LIKE operator for pattern matching
   
2. Data Manipulation
   - INSERT statements
   - UPDATE statements
   - DELETE statements

## Examples:
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

## Practice Exercises:

### **1. Write Queries Using Multiple WHERE Conditions**

Learn to filter data using:

* `AND`
* `OR`
* Comparison operators (`>`, `<`, `=`, `!=`)
* Date conditions
* Combined logic

Example:

```sql
SELECT *
FROM customers
WHERE region = 'North'
  AND status = 'Active'
  AND created_at > '2024-01-01';
```

---

### **2. Practice Pattern Matching with Different LIKE Patterns**

Work with:

* Prefix matching (`'abc%'`)
* Suffix matching (`'%xyz'`)
* Substring matching (`'%text%'`)
* Single-character wildcards (`'_a%'`)

Example:

```sql
SELECT name
FROM products
WHERE name LIKE '%book%';
```

---

### **3. Insert Multiple Rows of Data**

Practice inserting several records in one statement.

Example:

```sql
INSERT INTO products (name, category, price)
VALUES
  ('Keyboard', 'Electronics', 25),
  ('Notebook', 'Stationery', 3),
  ('Backpack', 'Accessories', 20);
```

---

### **4. Update Data Based on Complex Conditions**

Use multiple filters, comparisons, and logical operators in `UPDATE` queries.

Example:

```sql
UPDATE orders
SET status = 'Completed'
WHERE total_amount > 50
  AND order_date < '2024-03-01'
  AND customer_id IN (1, 3, 7);
```

---

### **5. Safely Delete Data with Proper Conditions**

Learn to delete only the records you intend to remove.
Always use conditions to prevent data loss.

Example:

```sql
DELETE FROM logs
WHERE created_at < '2023-01-01'
  AND event_type = 'INFO';
```

---
## Key Concepts to Master:
- Complex filtering logic
- Pattern matching techniques
- Safe data manipulation practices
- Transaction safety
- Backup before updates/deletes

## Additional Resources:
- SQL Practice exercises on HackerRank
- Database backup and restore procedures
- SQL data manipulation best practices

- Pattern matching documentation 
