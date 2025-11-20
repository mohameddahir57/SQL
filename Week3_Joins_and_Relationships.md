# Week 3: Joins & Relationships

## Topics:
1. Types of Joins
   - INNER JOIN
   - LEFT (OUTER) JOIN
   - RIGHT (OUTER) JOIN
   - FULL (OUTER) JOIN
   
2. Multiple Table Joins
   - Joining more than two tables
   - Using aliases
   - Join conditions
   - Understanding relationships (one-to-one, one-to-many, many-to-many)

## Examples:
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

### Practice Exercises:

## **1. Create Tables with Relationships**

Learn how to define:

* Primary keys
* Foreign keys
* One-to-many and many-to-many relationships

Practice creating schemas that connect tables such as `customers`, `orders`, and `products`.

---

## **2. Practice All Types of Joins**

Work with every join type:

* `INNER JOIN`
* `LEFT JOIN`
* `RIGHT JOIN`
* `FULL OUTER JOIN`
* `CROSS JOIN`

Understand when each join is appropriate and how it affects the result.

---

## **3. Write Queries Joining Multiple Tables**

Improve your ability to connect two or more tables in the same query.
Examples include:

* Joining customers → orders → products
* Joining employees → departments → locations

---

## **4. Use Table Aliases Effectively**

Use short aliases to make queries cleaner and easier to read:

```sql
SELECT c.name, o.order_id
FROM customers AS c
JOIN orders AS o ON c.customer_id = o.customer_id;
```

---

## **5. Combine Joins with WHERE Clauses**

Practice filtering joined data using:

* Date filters
* Category filters
* Price or quantity ranges

Example:

```sql
SELECT c.name, o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total_amount > 100;
```

---

## **6. Handle NULL Values in Joins**

Learn how different joins treat missing data:

* Missing matches in left/right joins
* Using `COALESCE()` to replace NULLs
* Detecting unmatched rows

Example:

```sql
SELECT p.product_name, COALESCE(o.quantity, 0) AS quantity_sold
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id;
```
## Key Concepts to Master:
- Understanding table relationships
- Join syntax and types
- NULL handling in joins
- Performance considerations
- Table alias best practices

## Common Join Patterns:
1. Customer orders with details
2. Product categories and suppliers
3. Employee departments and managers
4. Sales reports with multiple dimensions

## Additional Resources:
- Visual Representation of SQL Joins
- Database normalization principles
- Join optimization techniques

- Real-world join examples 
