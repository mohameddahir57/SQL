# Week 5: Subqueries & CTEs (Common Table Expressions)

## Topics:
1. Subqueries
   - Single-row subqueries
   - Multi-row subqueries
   - Correlated subqueries
   - Subqueries in SELECT, FROM, WHERE
   
2. Common Table Expressions (CTEs)
   - Basic CTEs
   - Recursive CTEs
   - Multiple CTEs
   - CTE best practices

## Examples:
```sql
-- Subquery in WHERE clause
SELECT product_name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);

-- Subquery in FROM clause
SELECT dept_name, avg_salary
FROM (
    SELECT 
        department as dept_name,
        AVG(salary) as avg_salary
    FROM employees
    GROUP BY department
) dept_stats
WHERE avg_salary > 60000;

-- Correlated subquery
SELECT 
    employee_name,
    salary,
    (SELECT AVG(salary) 
     FROM employees e2 
     WHERE e2.department = e1.department) as dept_avg
FROM employees e1;

-- Basic CTE
WITH HighValueCustomers AS (
    SELECT 
        customer_id, 
        SUM(order_amount) as total_spent
    FROM orders
    GROUP BY customer_id
    HAVING SUM(order_amount) > 10000
)
SELECT 
    c.customer_name, 
    h.total_spent
FROM HighValueCustomers h
JOIN customers c ON h.customer_id = c.customer_id;

-- Multiple CTEs
WITH 
    CustomerStats AS (
        SELECT 
            customer_id,
            COUNT(*) as order_count,
            SUM(order_amount) as total_spent
        FROM orders
        GROUP BY customer_id
    ),
    TopCustomers AS (
        SELECT *
        FROM CustomerStats
        WHERE total_spent > 5000
    )
SELECT 
    c.customer_name,
    t.order_count,
    t.total_spent
FROM TopCustomers t
JOIN customers c ON t.customer_id = c.customer_id;

-- Recursive CTE (organization hierarchy)
WITH RECURSIVE EmployeeHierarchy AS (
    -- Base case: top level employees
    SELECT 
        employee_id,
        employee_name,
        manager_id,
        1 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case
    SELECT 
        e.employee_id,
        e.employee_name,
        e.manager_id,
        eh.level + 1
    FROM employees e
    JOIN EmployeeHierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM EmployeeHierarchy;
```

## Practice Exercises:

## **1. Write Subqueries in Different Clauses**

Use this sample dataset:

**Tables**
`employees(emp_id, name, dept_id, salary)`
`departments(dept_id, dept_name)`
`projects(project_id, emp_id, hours_worked)`

### **Exercises**

1. **Subquery in WHERE**

   * Find employees who earn more than the *average salary* in the company.

2. **Subquery in SELECT**

   * For each employee, show total hours worked (use a subquery inside SELECT).

3. **Subquery in FROM**

   * Create a derived table that lists total hours worked per employee, then find those with more than 40 hours.

4. **Subquery in HAVING**

   * Find departments where total salaries are above the company's *average department salary cost*.

---

## **2. Create CTEs for Complex Data Analysis**

Using the same dataset:

### **Exercises**

1. Write a CTE to calculate each department's average salary, then join it with employees.
2. Write a CTE that ranks employees by salary inside their department.
3. Write a CTE to calculate each project's total hours and find the top 5 most time-consuming projects.

---

## **3. Build Recursive Queries for Hierarchical Data**

Use this dataset:

**Table**
`categories(cat_id, cat_name, parent_id)`
Example:
Electronics → Phones → Smartphones → Android Phones

### **Exercises**

1. Write a recursive CTE to list **all subcategories** of "Electronics".
2. Create a hierarchy path like:
   `Electronics > Phones > Smartphones > Android Phones`
3. Count how many levels deep each branch goes.
4. Find the root parent for each category.

---

## **4. Optimize Queries Using CTEs**

### **Exercises**

1. Rewrite a subquery-heavy query using CTEs and compare readability.
2. Use multiple CTEs to break down a problem:
   * CTE 1: Total hours per project
   * CTE 2: Total hours per employee
   * CTE 3: Compare employee performance to company average

3. Optimize a query by removing repeated subqueries using CTEs only once.
4. Convert a complex join + filter query into layered CTE steps.

---

## **5. Compare Subquery vs CTE Approaches**

Use any of the above scenarios.

### **Exercises**

1. Solve the same problem using a **subquery** and then using a **CTE**:

   * Find employees with above-average hours.

2. Write both versions and compare:

   * Which is easier to read?
   * Which is easier to debug?

3. Test which version is faster on a large dataset (if possible).
4. Identify cases where subqueries are better than CTEs.

---

## Key Concepts to Master:
- Subquery types and use cases
- CTE benefits and limitations
- Query optimization
- Recursive query patterns
- When to use subqueries vs CTEs

## Common Use Cases:
1. Hierarchical data analysis
2. Complex data transformations
3. Report generation
4. Data filtering with dynamic criteria
5. Performance optimization

## Additional Resources:
- Advanced SQL documentation
- Query optimization guides
- Recursive CTE examples
- Real-world use cases
- Performance comparison studies 