# Week 3: Joins & Relationships

## 1\. Types of Joins 

**Joins** are used to combine rows from two or more tables based on a related column between them (often a Primary Key in one table and a Foreign Key in the other).

### **Visualizing the Joins**

The different types of joins determine which rows are included in the result set, particularly when there is no match in one of the tables.

[Image of SQL join types Venn diagrams]

| Join Type | Description | Key Result |
| :--- | :--- | :--- |
| **INNER JOIN** | Returns records that have **matching values** in *both* tables. | Only the **intersection** of the two tables. |
| **LEFT (OUTER) JOIN** | Returns all records from the **left** table, and the matched records from the right table. If there is no match, the right side is filled with `NULL`. | All of the left table + matching rows from the right. |
| **RIGHT (OUTER) JOIN** | Returns all records from the **right** table, and the matched records from the left table. If there is no match, the left side is filled with `NULL`. | All of the right table + matching rows from the left. |
| **FULL (OUTER) JOIN** | Returns records when there is a match in one of the tables. It combines the results of both Left and Right joins. If there are no matches, both sides are filled with `NULL`. | **All** records from both tables. |


### **Example Syntax**

Let's assume we have two tables: `Customers` (C) and `Orders` (O), joined on `CustomerID`.

```sql
-- 1. INNER JOIN (Finds customers who have placed orders)
SELECT C.Name, O.OrderID
FROM Customers C
INNER JOIN Orders O
  ON C.CustomerID = O.CustomerID;

-- 2. LEFT JOIN (Finds ALL customers, including those who have NOT placed orders)
SELECT C.Name, O.OrderID
FROM Customers C
LEFT JOIN Orders O
  ON C.CustomerID = O.CustomerID;
```

-----

## 2\. Multiple Table Joins and Best Practices 🔗

In complex applications, it's common to need data that spans three, four, or even more tables.

### Joining More Than Two Tables

To join multiple tables, you simply chain the `JOIN` clauses together, defining the join condition (`ON`) for each pair of tables.

**Example (Joining 3 Tables):**

Retrieve the customer name, order ID, and the name of the product in the order.

  * `Customers` (C) $\rightarrow$ `Orders` (O) $\rightarrow$ `OrderDetails` (D) $\rightarrow$ `Products` (P)

<!-- end list -->

```sql
SELECT
    C.CustomerName,
    O.OrderID,
    P.ProductName
FROM
    Customers C
INNER JOIN Orders O
    ON C.CustomerID = O.CustomerID -- 1st Join: Customers to Orders
INNER JOIN OrderDetails D
    ON O.OrderID = D.OrderID       -- 2nd Join: Orders to OrderDetails
INNER JOIN Products P
    ON D.ProductID = P.ProductID;   -- 3rd Join: OrderDetails to Products
```

### Using Aliases

**Aliases** (short, temporary names assigned using the `AS` keyword or just a space) are essential when working with joins for two main reasons:

1.  **Clarity and Brevity:** They shorten long table names in the `FROM` and `JOIN` clauses (e.g., `Customers C`).
2.  **Handling Ambiguity:** When columns with the same name exist in multiple tables (e.g., `ID` or `Name`), aliases allow you to specify exactly which table's column you mean (e.g., `C.Name` vs. `E.Name`).

### Join Conditions (`ON` Clause)

The `ON` clause is where you specify the criteria for joining the tables. This is almost always an **equality condition** (`=`) linking a **Primary Key** in one table to the corresponding **Foreign Key** in the other table.

### Understanding Relationships

Relational databases use different types of relationships to link data, and understanding these helps you design your joins correctly.

  * **One-to-One (1:1):** Each record in the first table relates to exactly one record in the second table (and vice versa). *Example: An employee table and a table for confidential employee details.*
  * **One-to-Many (1:N):** The most common type. One record in the first table can relate to many records in the second table, but each record in the second table relates to only one record in the first. *Example: One **Customer** can have many **Orders**.*
  * **Many-to-Many (N:N):** Many records in the first table can relate to many records in the second table. This relationship is typically resolved by a third, **junction** (or linking/bridge) table that holds the primary keys of the other two tables. *Example: Many **Students** can take many **Courses**, linked by a **Student\_Course** junction table.*

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


