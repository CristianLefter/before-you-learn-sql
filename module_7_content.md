# Module 7: Basic Programming Concepts

## Introduction

While SQL is primarily a query language, it incorporates many programming concepts that are essential for effective database interaction. Understanding these basic programming principles will help you write more powerful SQL queries and better comprehend how SQL works. This module introduces fundamental programming concepts including variables, control structures, functions, and error handling that form the foundation for SQL programming.

## 7.1 Variables and Data Storage

### What are Variables?

Variables are named storage locations that hold data values which can be changed during program execution.

#### Key Concepts:

- **Variable**: A named container for storing data
- **Value**: The actual data stored in a variable
- **Data Type**: The kind of data a variable can store (number, text, date, etc.)
- **Scope**: Where in a program a variable can be accessed
- **Lifetime**: How long a variable exists during program execution

#### Variables in SQL:

In SQL, variables are primarily used in:
- Stored procedures
- Functions
- Triggers
- Batch scripts

Different database systems have different syntax for variables:

**SQL Server:**
```sql
DECLARE @CustomerName VARCHAR(100);
SET @CustomerName = 'John Smith';
-- Or
DECLARE @OrderTotal DECIMAL(10,2) = 125.50;
```

**MySQL:**
```sql
SET @CustomerName = 'John Smith';
SET @OrderTotal = 125.50;
```

**PostgreSQL:**
```sql
DO $$
DECLARE
  customer_name VARCHAR(100) := 'John Smith';
  order_total DECIMAL(10,2) := 125.50;
BEGIN
  -- Use variables here
END $$;
```

#### Why This Matters for SQL:
Understanding variables helps you work with SQL procedural extensions like stored procedures and functions. Even in basic SQL, the concept of temporary storage is important for understanding how queries process data.

### Declaring and Assigning Values

Variables must be declared with a specific data type before they can be used, and then values can be assigned to them.

#### Declaration Process:

1. Specify that you're creating a variable
2. Provide a name (usually with a prefix like @ in SQL Server)
3. Define the data type
4. Optionally provide an initial value

#### Assignment Methods:

- Direct assignment using SET or :=
- Assignment as part of declaration
- Assignment from query results
- Assignment through parameters

#### Examples:

**SQL Server:**
```sql
-- Declaration with separate assignment
DECLARE @ProductName VARCHAR(100);
SET @ProductName = 'Laptop';

-- Declaration with initial value
DECLARE @Quantity INT = 5;

-- Assignment from query
DECLARE @CustomerCount INT;
SELECT @CustomerCount = COUNT(*) FROM Customers;

-- Multiple variables
DECLARE @FirstName VARCHAR(50), @LastName VARCHAR(50);
SELECT @FirstName = 'John', @LastName = 'Smith';
```

**MySQL:**
```sql
-- Assignment
SET @product_name = 'Laptop';
SET @quantity = 5;

-- Assignment from query
SELECT COUNT(*) INTO @customer_count FROM Customers;

-- Multiple assignments
SET @first_name = 'John', @last_name = 'Smith';
```

#### Why This Matters for SQL:
Proper variable declaration and assignment are fundamental skills for writing SQL procedures and functions. Understanding these concepts helps you manipulate and store intermediate results in complex SQL operations.

### Scope and Lifetime of Variables

The scope and lifetime of variables determine where and when they can be accessed.

#### Variable Scope:

- **Local Variables**: Accessible only within the block where they are declared
- **Session Variables**: Accessible throughout a user's connection session
- **Global Variables**: Accessible to all sessions (usually system-defined)

#### Variable Lifetime:

- **Local Variables**: Exist only during the execution of their containing block
- **Session Variables**: Exist until the session ends or they are explicitly dropped
- **Global Variables**: Exist until the server is restarted or they are explicitly changed

#### Examples:

**SQL Server:**
```sql
-- Local variable (procedure scope)
CREATE PROCEDURE CalculateTotal
AS
BEGIN
    DECLARE @Total DECIMAL(10,2);
    SELECT @Total = SUM(Amount) FROM Orders;
    RETURN @Total;
END;

-- Session variable (connection scope)
SET @UserID = 1001;
```

**MySQL:**
```sql
-- Local variable
DELIMITER //
CREATE PROCEDURE CalculateTotal()
BEGIN
    DECLARE total DECIMAL(10,2);
    SELECT SUM(amount) INTO total FROM orders;
    SELECT total;
END //
DELIMITER ;

-- Session variable
SET @user_id = 1001;

-- Global variable
SET GLOBAL max_connections = 100;
```

#### Why This Matters for SQL:
Understanding scope and lifetime helps you manage variables effectively in SQL procedures and avoid common pitfalls like unintended variable reuse or premature disposal.

## 7.2 Control Structures

### Conditional Statements (if-then-else)

Conditional statements allow code to make decisions based on certain conditions.

#### Basic Structure:

- **IF**: Evaluates a condition
- **THEN**: Code to execute if the condition is true
- **ELSE**: Code to execute if the condition is false
- **ELSEIF/ELSIF**: Additional conditions to check

#### Implementation in Different SQL Dialects:

**SQL Server:**
```sql
DECLARE @Age INT = 25;
DECLARE @Category VARCHAR(20);

IF @Age < 18
    SET @Category = 'Minor';
ELSE IF @Age < 65
    SET @Category = 'Adult';
ELSE
    SET @Category = 'Senior';

SELECT @Category AS AgeCategory;
```

**MySQL:**
```sql
DELIMITER //
CREATE PROCEDURE CategorizeCustomer(IN customerAge INT, OUT category VARCHAR(20))
BEGIN
    IF customerAge < 18 THEN
        SET category = 'Minor';
    ELSEIF customerAge < 65 THEN
        SET category = 'Adult';
    ELSE
        SET category = 'Senior';
    END IF;
END //
DELIMITER ;
```

**PostgreSQL:**
```sql
DO $$
DECLARE
    age INT := 25;
    category VARCHAR(20);
BEGIN
    IF age < 18 THEN
        category := 'Minor';
    ELSIF age < 65 THEN
        category := 'Adult';
    ELSE
        category := 'Senior';
    END IF;
    
    RAISE NOTICE 'Category: %', category;
END $$;
```

#### Conditional Logic in Standard SQL:

Even in standard SQL without procedural extensions, conditional logic can be implemented using:

- **CASE expressions**
- **Conditional functions (IF, IFNULL, COALESCE, etc.)**

```sql
-- CASE expression
SELECT 
    CustomerName,
    CASE 
        WHEN Age < 18 THEN 'Minor'
        WHEN Age < 65 THEN 'Adult'
        ELSE 'Senior'
    END AS AgeCategory
FROM Customers;
```

#### Why This Matters for SQL:
Conditional statements are essential for implementing business logic in SQL procedures and functions. Understanding these structures helps you write SQL code that can make decisions based on data values.

### Loops and Iteration

Loops allow code to repeat a block of statements multiple times.

#### Common Loop Types:

- **WHILE**: Repeats while a condition is true
- **FOR/FOREACH**: Iterates through a range or collection
- **REPEAT/DO-WHILE**: Executes at least once, then repeats while a condition is true

#### Implementation in Different SQL Dialects:

**SQL Server:**
```sql
-- WHILE loop
DECLARE @Counter INT = 1;
WHILE @Counter <= 5
BEGIN
    PRINT 'Counter: ' + CAST(@Counter AS VARCHAR);
    SET @Counter = @Counter + 1;
END;

-- Cursor (for iterating through rows)
DECLARE @CustomerName VARCHAR(100);
DECLARE CustomerCursor CURSOR FOR
    SELECT CustomerName FROM Customers WHERE State = 'CA';
    
OPEN CustomerCursor;
FETCH NEXT FROM CustomerCursor INTO @CustomerName;

WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT 'Customer: ' + @CustomerName;
    FETCH NEXT FROM CustomerCursor INTO @CustomerName;
END;

CLOSE CustomerCursor;
DEALLOCATE CustomerCursor;
```

**MySQL:**
```sql
-- WHILE loop
DELIMITER //
CREATE PROCEDURE CountToFive()
BEGIN
    DECLARE counter INT DEFAULT 1;
    WHILE counter <= 5 DO
        SELECT counter;
        SET counter = counter + 1;
    END WHILE;
END //
DELIMITER ;

-- REPEAT loop
DELIMITER //
CREATE PROCEDURE CountToFiveRepeat()
BEGIN
    DECLARE counter INT DEFAULT 1;
    REPEAT
        SELECT counter;
        SET counter = counter + 1;
    UNTIL counter > 5 END REPEAT;
END //
DELIMITER ;
```

**PostgreSQL:**
```sql
DO $$
DECLARE
    counter INT := 1;
BEGIN
    -- Simple loop with EXIT
    LOOP
        RAISE NOTICE 'Counter: %', counter;
        counter := counter + 1;
        EXIT WHEN counter > 5;
    END LOOP;
    
    -- FOR loop
    FOR i IN 1..5 LOOP
        RAISE NOTICE 'i: %', i;
    END LOOP;
END $$;
```

#### Set-Based Alternatives:

SQL is primarily designed for set-based operations rather than iterative processing. When possible, use set-based approaches instead of loops:

```sql
-- Instead of looping through customers to update status
UPDATE Customers
SET Status = 'Active'
WHERE LastPurchaseDate >= DATE_SUB(CURRENT_DATE, INTERVAL 90 DAY);
```

#### Why This Matters for SQL:
While SQL is primarily set-based, understanding loops is important for procedural SQL extensions. However, it's equally important to recognize when to use set-based operations instead of loops for better performance.

### Case Statements

CASE statements provide a way to implement conditional logic in SQL expressions.

#### Types of CASE Expressions:

- **Simple CASE**: Compares an expression to multiple values
- **Searched CASE**: Evaluates multiple Boolean expressions

#### Syntax:

**Simple CASE:**
```sql
CASE expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ...
    [ELSE default_result]
END
```

**Searched CASE:**
```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    [ELSE default_result]
END
```

#### Examples:

```sql
-- Simple CASE in SELECT
SELECT 
    ProductName,
    UnitPrice,
    CASE Category
        WHEN 'Electronics' THEN 'Tech'
        WHEN 'Clothing' THEN 'Apparel'
        WHEN 'Food' THEN 'Grocery'
        ELSE 'Other'
    END AS CategoryGroup
FROM Products;

-- Searched CASE in SELECT
SELECT 
    OrderID,
    OrderDate,
    TotalAmount,
    CASE
        WHEN TotalAmount < 100 THEN 'Small Order'
        WHEN TotalAmount < 1000 THEN 'Medium Order'
        ELSE 'Large Order'
    END AS OrderSize
FROM Orders;

-- CASE in ORDER BY
SELECT CustomerName, State, TotalPurchases
FROM Customers
ORDER BY 
    CASE 
        WHEN State = 'CA' THEN 1
        WHEN State = 'NY' THEN 2
        ELSE 3
    END,
    TotalPurchases DESC;

-- CASE in UPDATE
UPDATE Employees
SET Bonus = 
    CASE
        WHEN YearsOfService < 5 THEN Salary * 0.05
        WHEN YearsOfService < 10 THEN Salary * 0.10
        ELSE Salary * 0.15
    END;
```

#### Why This Matters for SQL:
CASE expressions are versatile and can be used in SELECT, WHERE, ORDER BY, and other clauses. Understanding CASE statements helps you implement conditional logic directly in SQL queries without needing procedural code.

## 7.3 Functions and Procedures

### What are Functions?

Functions are named blocks of code that perform specific tasks and return values.

#### Key Characteristics:

- **Reusability**: Can be called multiple times
- **Modularity**: Encapsulate specific functionality
- **Parameters**: Accept input values
- **Return Value**: Produce an output value

#### Types of SQL Functions:

- **Built-in Functions**: Provided by the database system
- **User-Defined Functions**: Created by users
  - **Scalar Functions**: Return a single value
  - **Table-Valued Functions**: Return a table result
  - **Aggregate Functions**: Operate on multiple values and return a single value

#### Examples of Built-in Functions:

```sql
-- String functions
SELECT 
    UPPER(FirstName) AS UpperName,
    LOWER(LastName) AS LowerName,
    CONCAT(FirstName, ' ', LastName) AS FullName,
    LENGTH(Email) AS EmailLength
FROM Customers;

-- Numeric functions
SELECT 
    ROUND(Price, 2) AS RoundedPrice,
    CEILING(Price) AS CeilingPrice,
    FLOOR(Price) AS FloorPrice,
    ABS(Balance) AS AbsoluteBalance
FROM Products;

-- Date functions
SELECT 
    OrderID,
    OrderDate,
    YEAR(OrderDate) AS OrderYear,
    MONTH(OrderDate) AS OrderMonth,
    DATEDIFF(DAY, OrderDate, ShipDate) AS DaysToShip
FROM Orders;
```

#### Why This Matters for SQL:
Functions are essential components of SQL queries. Understanding how to use built-in functions and create user-defined functions helps you manipulate data effectively and write more concise queries.

### Input Parameters and Return Values

Functions and procedures can accept input parameters and return values.

#### Parameter Types:

- **Input Parameters**: Values passed into the function/procedure
- **Output Parameters**: Values returned by reference (procedures)
- **Input/Output Parameters**: Values that are both input and output

#### Return Value Mechanisms:

- **Function Return**: The primary value returned by a function
- **Output Parameters**: Additional values returned by procedures
- **Result Sets**: Tables returned by table-valued functions or procedures

#### Examples:

**SQL Server:**
```sql
-- Scalar function with parameters
CREATE FUNCTION CalculateDiscount(@Price DECIMAL(10,2), @DiscountRate DECIMAL(5,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN @Price * (1 - @DiscountRate);
END;

-- Using the function
SELECT 
    ProductName,
    Price,
    dbo.CalculateDiscount(Price, 0.1) AS DiscountedPrice
FROM Products;

-- Stored procedure with input and output parameters
CREATE PROCEDURE GetCustomerOrderStats
    @CustomerID INT,
    @OrderCount INT OUTPUT,
    @TotalSpent DECIMAL(12,2) OUTPUT
AS
BEGIN
    SELECT 
        @OrderCount = COUNT(OrderID),
        @TotalSpent = SUM(TotalAmount)
    FROM Orders
    WHERE CustomerID = @CustomerID;
END;

-- Using the procedure
DECLARE @NumOrders INT, @TotalAmount DECIMAL(12,2);
EXEC GetCustomerOrderStats 1001, @NumOrders OUTPUT, @TotalAmount OUTPUT;
SELECT @NumOrders AS OrderCount, @TotalAmount AS TotalSpent;
```

**MySQL:**
```sql
-- Function with parameters
DELIMITER //
CREATE FUNCTION CalculateDiscount(price DECIMAL(10,2), discount_rate DECIMAL(5,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN price * (1 - discount_rate);
END //
DELIMITER ;

-- Procedure with input and output parameters
DELIMITER //
CREATE PROCEDURE GetCustomerOrderStats(
    IN customer_id INT,
    OUT order_count INT,
    OUT total_spent DECIMAL(12,2)
)
BEGIN
    SELECT 
        COUNT(order_id),
        SUM(total_amount)
    INTO 
        order_count, 
        total_spent
    FROM orders
    WHERE customer_id = customer_id;
END //
DELIMITER ;
```

#### Why This Matters for SQL:
Understanding parameters and return values is essential for creating and using functions and procedures effectively. These mechanisms allow for flexible and reusable SQL code components.

### Built-in vs. Custom Functions

SQL provides many built-in functions, but you can also create custom functions for specific needs.

#### Built-in Functions:

- **String Functions**: CONCAT, SUBSTRING, UPPER, LOWER, TRIM, etc.
- **Numeric Functions**: ROUND, CEILING, FLOOR, ABS, etc.
- **Date Functions**: DATEADD, DATEDIFF, YEAR, MONTH, DAY, etc.
- **Conversion Functions**: CAST, CONVERT, etc.
- **Aggregate Functions**: SUM, AVG, COUNT, MIN, MAX, etc.
- **Analytical Functions**: ROW_NUMBER, RANK, DENSE_RANK, etc.

#### Custom Function Benefits:

- **Business Logic**: Implement specific business rules
- **Code Reuse**: Write logic once, use it many times
- **Abstraction**: Hide complex logic behind simple interfaces
- **Consistency**: Ensure calculations are performed the same way everywhere
- **Maintenance**: Update logic in one place

#### Examples:

**Built-in Functions:**
```sql
-- String manipulation
SELECT 
    SUBSTRING(ProductName, 1, 10) AS ShortName,
    UPPER(Category) AS CategoryUpper
FROM Products;

-- Date calculations
SELECT 
    OrderID,
    OrderDate,
    DATEADD(DAY, 7, OrderDate) AS ExpectedDelivery
FROM Orders;

-- Analytical functions
SELECT 
    ProductName,
    Category,
    Price,
    ROW_NUMBER() OVER (PARTITION BY Category ORDER BY Price DESC) AS PriceRank
FROM Products;
```

**Custom Functions:**
```sql
-- Custom business logic function
CREATE FUNCTION CalculateShipping(@Weight DECIMAL(8,2), @Distance INT, @ExpressDelivery BIT)
RETURNS DECIMAL(10,2)
AS
BEGIN
    DECLARE @BaseRate DECIMAL(10,2) = 5.00;
    DECLARE @WeightRate DECIMAL(10,2) = 0.5;
    DECLARE @DistanceRate DECIMAL(10,2) = 0.1;
    DECLARE @ExpressFactor DECIMAL(5,2) = 1.5;
    
    DECLARE @ShippingCost DECIMAL(10,2);
    
    SET @ShippingCost = @BaseRate + (@Weight * @WeightRate) + (@Distance * @DistanceRate);
    
    IF @ExpressDelivery = 1
        SET @ShippingCost = @ShippingCost * @ExpressFactor;
        
    RETURN @ShippingCost;
END;
```

#### Why This Matters for SQL:
Knowing when to use built-in functions versus creating custom functions helps you write efficient and maintainable SQL code. Built-in functions are optimized and portable, while custom functions allow for specialized business logic.

### Procedure Basics

Stored procedures are named collections of SQL statements that can be executed as a single unit.

#### Key Characteristics:

- **Named Code Blocks**: Stored in the database
- **Parameter Support**: Can accept input and output parameters
- **No Return Value**: Unlike functions, procedures don't have a return value (but can use output parameters)
- **Multiple Result Sets**: Can return multiple tables of data
- **Transaction Support**: Can include transaction control statements
- **Flow Control**: Can include conditional logic and loops

#### Common Uses:

- **Data Modification**: Insert, update, or delete operations
- **Complex Processing**: Multi-step operations
- **Security**: Control access to underlying tables
- **Performance**: Pre-compiled execution plans
- **Batch Processing**: Run multiple statements as a single unit

#### Examples:

**SQL Server:**
```sql
CREATE PROCEDURE CreateNewOrder
    @CustomerID INT,
    @OrderDate DATE,
    @ShippingAddress VARCHAR(200),
    @NewOrderID INT OUTPUT
AS
BEGIN
    -- Start a transaction
    BEGIN TRANSACTION;
    
    -- Insert the order header
    INSERT INTO Orders (CustomerID, OrderDate, ShippingAddress, Status)
    VALUES (@CustomerID, @OrderDate, @ShippingAddress, 'Pending');
    
    -- Get the new order ID
    SET @NewOrderID = SCOPE_IDENTITY();
    
    -- Commit the transaction
    COMMIT TRANSACTION;
    
    -- Return order details
    SELECT * FROM Orders WHERE OrderID = @NewOrderID;
END;

-- Execute the procedure
DECLARE @OrderID INT;
EXEC CreateNewOrder 1001, '2023-04-15', '123 Main St, Anytown, USA', @OrderID OUTPUT;
SELECT @OrderID AS NewOrderID;
```

**MySQL:**
```sql
DELIMITER //
CREATE PROCEDURE CreateNewOrder(
    IN p_customer_id INT,
    IN p_order_date DATE,
    IN p_shipping_address VARCHAR(200),
    OUT p_new_order_id INT
)
BEGIN
    -- Start a transaction
    START TRANSACTION;
    
    -- Insert the order header
    INSERT INTO Orders (CustomerID, OrderDate, ShippingAddress, Status)
    VALUES (p_customer_id, p_order_date, p_shipping_address, 'Pending');
    
    -- Get the new order ID
    SET p_new_order_id = LAST_INSERT_ID();
    
    -- Commit the transaction
    COMMIT;
    
    -- Return order details
    SELECT * FROM Orders WHERE OrderID = p_new_order_id;
END //
DELIMITER ;
```

#### Why This Matters for SQL:
Stored procedures are powerful tools for encapsulating business logic and database operations. Understanding procedures helps you create modular, secure, and efficient database applications.

## 7.4 Error Handling Concepts

### Types of Errors

Understanding different types of errors is essential for writing robust SQL code.

#### Common Error Categories:

- **Syntax Errors**: Incorrect SQL syntax or grammar
- **Semantic Errors**: Valid syntax but impossible operations
- **Execution Errors**: Runtime errors during query execution
- **Constraint Violations**: Attempts to violate database constraints
- **Connection Errors**: Problems with database connectivity
- **Timeout Errors**: Operations exceeding time limits
- **Permission Errors**: Insufficient privileges for operations

#### Examples:

**Syntax Error:**
```sql
-- Missing comma between columns
SELECT CustomerID CustomerName FROM Customers;
```

**Semantic Error:**
```sql
-- Referencing a non-existent column
SELECT NonExistentColumn FROM Customers;
```

**Execution Error:**
```sql
-- Division by zero
SELECT OrderID, Amount / 0 AS InvalidCalculation FROM Orders;
```

**Constraint Violation:**
```sql
-- Duplicate primary key
INSERT INTO Customers (CustomerID, CustomerName)
VALUES (1001, 'John Smith'); -- Assuming 1001 already exists
```

#### Why This Matters for SQL:
Recognizing different types of errors helps you debug SQL code more effectively and write more robust queries and procedures.

### Debugging Basics

Debugging is the process of identifying and fixing errors in code.

#### SQL Debugging Techniques:

- **Syntax Checking**: Verify SQL syntax before execution
- **Step-by-Step Execution**: Run complex queries in parts
- **Print/Select Statements**: Output intermediate values
- **Transaction Rollback**: Test modifications without committing
- **Error Messages**: Analyze error messages for clues
- **Execution Plans**: Review how the database executes queries

#### Debugging Tools:

- **Database Client Tools**: SQL Server Management Studio, MySQL Workbench, etc.
- **Query Analyzers**: Examine query performance and execution
- **Logging**: Record errors and execution information
- **Debuggers**: Step through stored procedures (in some environments)

#### Debugging Examples:

```sql
-- Using print/select for debugging
DECLARE @CustomerID INT = 1001;
PRINT 'Looking up customer: ' + CAST(@CustomerID AS VARCHAR);

SELECT * FROM Customers WHERE CustomerID = @CustomerID;

-- Testing with transactions
BEGIN TRANSACTION;

UPDATE Customers SET CreditLimit = CreditLimit * 1.1;
-- Check the results before committing
SELECT CustomerID, CustomerName, CreditLimit FROM Customers;

-- If something looks wrong
ROLLBACK TRANSACTION;
-- If everything looks good
-- COMMIT TRANSACTION;
```

#### Why This Matters for SQL:
Basic debugging skills are essential for developing and maintaining SQL code. These techniques help you identify and fix issues efficiently.

### Exception Handling Introduction

Exception handling allows you to gracefully manage errors that occur during SQL execution.

#### Exception Handling Concepts:

- **Try-Catch Blocks**: Attempt operations and catch errors
- **Error Information**: Access details about the error
- **Error Logging**: Record error information for later analysis
- **Graceful Recovery**: Continue processing despite errors
- **Custom Error Messages**: Provide meaningful information

#### Implementation in Different SQL Dialects:

**SQL Server:**
```sql
BEGIN TRY
    -- Attempt operations
    INSERT INTO Orders (CustomerID, OrderDate, TotalAmount)
    VALUES (1001, GETDATE(), 500);
    
    UPDATE Inventory
    SET Quantity = Quantity - 1
    WHERE ProductID = 5001;
END TRY
BEGIN CATCH
    -- Handle errors
    DECLARE @ErrorMessage NVARCHAR(4000) = ERROR_MESSAGE();
    DECLARE @ErrorSeverity INT = ERROR_SEVERITY();
    DECLARE @ErrorState INT = ERROR_STATE();
    
    -- Log the error
    INSERT INTO ErrorLog (ErrorDate, ErrorMessage, Procedure)
    VALUES (GETDATE(), @ErrorMessage, 'ProcessOrder');
    
    -- Re-throw the error or return a custom message
    RAISERROR(@ErrorMessage, @ErrorSeverity, @ErrorState);
    -- Or
    -- RETURN -1;
END CATCH;
```

**MySQL:**
```sql
DELIMITER //
CREATE PROCEDURE ProcessOrder(IN p_customer_id INT, IN p_product_id INT)
BEGIN
    -- Declare handler for exceptions
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Get diagnostic information
        GET DIAGNOSTICS CONDITION 1
            @sqlstate = RETURNED_SQLSTATE,
            @errno = MYSQL_ERRNO,
            @text = MESSAGE_TEXT;
        
        -- Log the error
        INSERT INTO ErrorLog (ErrorDate, ErrorCode, ErrorMessage, Procedure)
        VALUES (NOW(), @errno, @text, 'ProcessOrder');
        
        -- Return error information
        SELECT 'Error occurred', @errno, @text;
        
        -- Rollback any changes
        ROLLBACK;
    END;
    
    -- Start transaction
    START TRANSACTION;
    
    -- Attempt operations
    INSERT INTO Orders (CustomerID, OrderDate)
    VALUES (p_customer_id, CURDATE());
    
    UPDATE Inventory
    SET Quantity = Quantity - 1
    WHERE ProductID = p_product_id;
    
    -- If we get here, commit the transaction
    COMMIT;
    
    SELECT 'Order processed successfully';
END //
DELIMITER ;
```

#### Why This Matters for SQL:
Exception handling is crucial for writing robust SQL procedures that can gracefully manage errors. Understanding these concepts helps you create more reliable database applications.

## Practical Exercises

1. **Variable Usage Exercise**:
   - Write a SQL procedure that:
     - Declares variables for customer name, order count, and total amount
     - Retrieves values from the database based on a customer ID
     - Formats and outputs a summary message
   - Test the procedure with different customer IDs

2. **Control Structure Practice**:
   - Create a SQL procedure that categorizes products based on their price:
     - Under $10: "Budget"
     - $10-$50: "Standard"
     - $51-$100: "Premium"
     - Over $100: "Luxury"
   - The procedure should update a Category column in the Products table
   - Include error handling for invalid prices

3. **Function Development**:
   - Create a custom function that calculates a loyalty discount based on:
     - Customer's total spending (higher spending = bigger discount)
     - Number of orders in the past year
     - Membership level (Bronze, Silver, Gold)
   - Test the function with various customer scenarios

4. **Error Handling Challenge**:
   - Write a procedure that transfers funds between two accounts
   - Include comprehensive error handling for scenarios like:
     - Insufficient funds
     - Invalid account numbers
     - Connection issues
     - Constraint violations
   - Ensure the procedure maintains data integrity (no partial transfers)

## Summary

This module covered the essential programming concepts that form the foundation for SQL programming:

- **Variables and Data Storage**: What variables are, how to declare and assign values, and variable scope and lifetime
- **Control Structures**: Conditional statements, loops and iteration, and case statements
- **Functions and Procedures**: What functions are, input parameters and return values, built-in vs. custom functions, and procedure basics
- **Error Handling Concepts**: Types of errors, debugging basics, and exception handling introduction

Understanding these programming concepts will help you write more powerful SQL queries and better comprehend how SQL works as a programming language.

## Next Steps

Now that you understand the basic programming concepts that underpin SQL, you're ready to explore the SQL development environment. The next module will cover SQL clients and tools, setting up a development environment, SQL editors and features, and version control basics.

## Additional Resources

- [SQL Server T-SQL Programming](https://docs.microsoft.com/en-us/sql/t-sql/language-reference)
- [MySQL Stored Procedures](https://dev.mysql.com/doc/refman/8.0/en/stored-programs-defining.html)
- [PostgreSQL PL/pgSQL](https://www.postgresql.org/docs/current/plpgsql.html)
- [Error Handling Best Practices](https://www.sqlshack.com/error-handling-in-sql-server-part-1-the-basics/)
- [SQL Programming Techniques](https://www.red-gate.com/simple-talk/sql/t-sql-programming/)
