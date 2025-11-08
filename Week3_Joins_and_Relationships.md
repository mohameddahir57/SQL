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

## Practice Exercises:
1. Create tables with relationships
2. Practice all types of joins
3. Write queries joining multiple tables
4. Use table aliases effectively
5. Combine joins with WHERE clauses
6. Handle NULL values in joins

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