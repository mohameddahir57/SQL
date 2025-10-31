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
1. Write queries using multiple WHERE conditions
2. Practice pattern matching with different LIKE patterns
3. Insert multiple rows of data
4. Update data based on complex conditions
5. Safely delete data with proper conditions

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