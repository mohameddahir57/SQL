# Week 2: Filtering & Data Manipulation

## 1\. Advanced WHERE Clauses 

The `WHERE` clause is used to filter records in a result set or to specify which records to update or delete. These operators allow you to define complex filtering conditions.

| Operator | Purpose | Example |
| :--- | :--- | :--- |
| **AND** | Combines two conditions; both must be $\mathbf{TRUE}$. | `SELECT * FROM Employees WHERE Department = 'Sales' **AND** Salary > 50000;` |
| **OR** | Combines two conditions; at least one must be $\mathbf{TRUE}$. | `SELECT * FROM Employees WHERE Department = 'Sales' **OR** Department = 'Marketing';` |
| **NOT** | Negates a condition (finds records where the condition is **not** $\mathbf{TRUE}$). | `SELECT * FROM Employees WHERE **NOT** Department = 'HR';` |
| **IN** | Specifies a list of possible values for a column. | `SELECT * FROM Products WHERE Category **IN** ('Electronics', 'Apparel', 'Books');` |
| **BETWEEN** | Selects values within a given range (inclusive). | `SELECT * FROM Orders WHERE OrderDate **BETWEEN** '2025-01-01' **AND** '2025-02-29';` |
| **LIKE** | Searches for a specified **pattern** in a column. | `SELECT * FROM Customers WHERE LastName **LIKE** 'S%';` |

### Pattern Matching with the `LIKE` Operator

The `LIKE` operator is powerful for flexible text searching, using **wildcard characters**:

  * **`%`** (Percent): Represents zero, one, or multiple characters.
      * Example: `'a%'` finds any value that starts with 'a'.
      * Example: `'%man%'` finds any value that has 'man' in any position.
  * **`_`** (Underscore): Represents a single, exact character.
      * Example: `'h_t'` finds 'hat', 'hit', 'hot', etc.

**Example Query using `LIKE`:**

```sql
SELECT
    ProductName
FROM
    Products
WHERE
    ProductName LIKE '%Keyboard'; -- Finds products that end with "Keyboard"
```

[Image of SQL WHERE clause flow chart showing AND OR and NOT logic]

-----

## 2\. Data Manipulation (DML) 

**Data Manipulation Language (DML)** commands are used to add, modify, and delete data in a database.

### INSERT Statements (Adding New Data)

The `INSERT INTO` statement is used to add new rows of data into a table.

There are two main ways to use it:

1.  **Specifying both column names and values:** (Recommended, as it's less prone to errors if the table structure changes.)

    ```sql
    INSERT INTO Customers (
        FirstName,
        LastName,
        City
    )
    VALUES (
        'Mohamed Dahir',
        'Osman',
        'Mogadishu'
    );
    ```

2.  **Only specifying values:** (Requires providing values for ALL columns in the exact order they appear in the table.)

    ```sql
    INSERT INTO Customers
    VALUES (
        101, -- CustomerID
        'Zakia',
        'Abdinur',
        'Mogadishu'
    );
    ```

### UPDATE Statements (Modifying Existing Data)

The `UPDATE` statement is used to modify the existing records in a table. **It is crucial to use the `WHERE` clause** with `UPDATE` to target specific rows; otherwise, **ALL** rows in the table will be updated.

```sql
UPDATE
    Employees
SET
    Salary = 60000, -- New value for the Salary column
    JobTitle = 'Senior Developer' -- New value for the JobTitle column
WHERE
    EmployeeID = 123; -- CRUCIAL: Specifies which employee(s) to update
```

### DELETE Statements (Removing Data)

The `DELETE FROM` statement is used to remove existing records from a table. Just like `UPDATE`, **you must use the `WHERE` clause** to specify which row(s) to remove. Without a `WHERE` clause, **ALL** rows in the table will be deleted\!

```sql
DELETE FROM
    Orders
WHERE
    OrderDate < '2024-01-01'; -- Deletes all orders placed before Jan 1, 2024
```

> ** Important Note on `DELETE`:**
> If you want to delete all rows from a table, `TRUNCATE TABLE TableName;` is often faster and uses fewer system resources than `DELETE FROM TableName;`, as `TRUNCATE` does not log individual row deletions.

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


