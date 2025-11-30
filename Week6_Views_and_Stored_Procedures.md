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
1. Create views for common queries
2. Write procedures with error handling
3. Implement transaction management
4. Create parameterized procedures
5. Update data through views
6. Handle complex business logic

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