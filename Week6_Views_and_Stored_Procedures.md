# Week 6: Views & Stored Procedures

## Topics:
1. Views
   - Creating views
   - Updating views
   - Materialized views
   - View limitations and best practices
   
2. Stored Procedures
   - Creating procedures
   - Input/Output parameters
   - Error handling
   - Transaction management

## Examples:
```sql
-- Creating a simple view
CREATE VIEW high_value_products AS
SELECT 
    product_name, 
    price, 
    category
FROM products
WHERE price > 1000;

-- Creating a complex view with joins
CREATE VIEW order_summary AS
SELECT 
    o.order_id,
    c.customer_name,
    SUM(oi.quantity * oi.unit_price) as total_amount,
    COUNT(oi.product_id) as total_items
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id, c.customer_name;

-- Basic stored procedure
CREATE PROCEDURE UpdateProductPrice
    @product_id INT,
    @new_price DECIMAL(10,2)
AS
BEGIN
    BEGIN TRY
        UPDATE products
        SET price = @new_price
        WHERE product_id = @product_id;
        
        IF @@ROWCOUNT = 0
            THROW 50001, 'Product not found', 1;
    END TRY
    BEGIN CATCH
        THROW 50001, 'Error updating product price', 1;
    END CATCH
END;

-- Stored procedure with transaction
CREATE PROCEDURE TransferMoney
    @from_account INT,
    @to_account INT,
    @amount DECIMAL(10,2),
    @success BIT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    
    BEGIN TRY
        BEGIN TRANSACTION;
            -- Deduct from source account
            UPDATE accounts 
            SET balance = balance - @amount
            WHERE account_id = @from_account
            AND balance >= @amount;
            
            -- Add to destination account
            UPDATE accounts
            SET balance = balance + @amount
            WHERE account_id = @to_account;
            
            -- Check if both updates were successful
            IF @@ROWCOUNT > 0
            BEGIN
                COMMIT TRANSACTION;
                SET @success = 1;
            END
            ELSE
            BEGIN
                ROLLBACK TRANSACTION;
                SET @success = 0;
            END
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        SET @success = 0;
        THROW;
    END CATCH
END;
```

## Practice Exercises:

## **1. Create Views for Common Queries**

### ✔ Example: Sales summary view

```sql
CREATE VIEW vw_SalesSummary AS
SELECT 
    CustomerID,
    COUNT(OrderID) AS TotalOrders,
    SUM(TotalAmount) AS TotalSpent,
    AVG(TotalAmount) AS AvgOrderValue
FROM Orders
GROUP BY CustomerID;
```

---

## **2. Write Procedures with Error Handling**

### ✔ Example: Insert order with TRY…CATCH

```sql
CREATE PROCEDURE AddOrder
    @CustomerID INT,
    @Amount DECIMAL(10,2)
AS
BEGIN
    BEGIN TRY
        INSERT INTO Orders(CustomerID, TotalAmount)
        VALUES (@CustomerID, @Amount);

        SELECT 'Order added successfully' AS Message;
    END TRY
    BEGIN CATCH
        SELECT 
            ERROR_MESSAGE() AS ErrorMessage,
            ERROR_LINE() AS ErrorLine;
    END CATCH
END;
```

---

## **3. Implement Transaction Management**

### ✔ Example: Transaction with rollback on failure

```sql
BEGIN TRAN;

BEGIN TRY
    UPDATE Accounts
    SET Balance = Balance - 500
    WHERE AccountID = 1;

    UPDATE Accounts
    SET Balance = Balance + 500
    WHERE AccountID = 2;

    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
    SELECT ERROR_MESSAGE() AS ErrorMessage;
END CATCH;
```

---

## **4. Create Parameterized Procedures**

### ✔ Example: Get orders by date range

```sql
CREATE PROCEDURE GetOrdersByDate
    @StartDate DATE,
    @EndDate DATE
AS
BEGIN
    SELECT *
    FROM Orders
    WHERE OrderDate BETWEEN @StartDate AND @EndDate;
END;
```

---

## **5. Update Data Through Views**

### ✔ Step 1 — Create an updatable view

```sql
CREATE VIEW vw_Products AS
SELECT ProductID, ProductName, Price
FROM Products;
```

### ✔ Step 2 — Update through the view

```sql
UPDATE vw_Products
SET Price = 19.99
WHERE ProductID = 3;
```

> Works because the view is simple (no joins, no aggregates).

---

## **6. Handle Complex Business Logic**

### ✔ Example: Order processing logic

```sql
CREATE PROCEDURE ProcessOrder
    @OrderID INT
AS
BEGIN
    BEGIN TRAN;

    BEGIN TRY
        -- Step 1: Check stock
        IF EXISTS (
            SELECT 1 
            FROM OrderDetails od
            JOIN Products p ON od.ProductID = p.ProductID
            WHERE od.OrderID = @OrderID AND p.Stock < od.Quantity
        )
        BEGIN
            RAISERROR('Not enough stock.', 16, 1);
        END

        -- Step 2: Reduce stock
        UPDATE p
        SET p.Stock = p.Stock - od.Quantity
        FROM Products p
        JOIN OrderDetails od ON p.ProductID = od.ProductID
        WHERE od.OrderID = @OrderID;

        -- Step 3: Mark order as complete
        UPDATE Orders
        SET Status = 'Completed'
        WHERE OrderID = @OrderID;

        COMMIT;
    END TRY
    BEGIN CATCH
        ROLLBACK;
        SELECT ERROR_MESSAGE() AS ErrorMessage;
    END CATCH
END;
```

---


## Key Concepts to Master:
- View types and limitations
- Procedure parameters
- Error handling patterns
- Transaction isolation levels
- Security considerations

## Common Use Cases:
1. Data access abstraction
2. Business logic encapsulation
3. Data validation
4. Audit trailing
5. Complex data operations

## Additional Resources:
- SQL Server stored procedures documentation
- View optimization techniques
- Transaction management best practices
- Error handling patterns

- Security considerations 
