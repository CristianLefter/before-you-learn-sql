# SQL Cheat Sheet for Beginners

## Basic SQL Commands

### SELECT - Retrieve data from a database
```sql
-- Basic SELECT
SELECT column1, column2 FROM table_name;

-- Select all columns
SELECT * FROM table_name;

-- Select with condition
SELECT column1, column2 FROM table_name WHERE condition;

-- Select with sorting
SELECT column1, column2 FROM table_name ORDER BY column1 [ASC|DESC];

-- Select with limit
SELECT column1, column2 FROM table_name LIMIT number;
```

### INSERT - Add new records to a table
```sql
-- Insert a single row
INSERT INTO table_name (column1, column2) VALUES (value1, value2);

-- Insert multiple rows
INSERT INTO table_name (column1, column2) 
VALUES (value1, value2), (value3, value4);
```

### UPDATE - Modify existing records
```sql
-- Update records
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```

### DELETE - Remove records from a table
```sql
-- Delete records
DELETE FROM table_name WHERE condition;
```

## Filtering Data

### WHERE Clause Operators
```sql
-- Comparison operators
SELECT * FROM table_name WHERE column_name = value;
SELECT * FROM table_name WHERE column_name > value;
SELECT * FROM table_name WHERE column_name < value;
SELECT * FROM table_name WHERE column_name >= value;
SELECT * FROM table_name WHERE column_name <= value;
SELECT * FROM table_name WHERE column_name <> value; -- Not equal

-- Logical operators
SELECT * FROM table_name WHERE condition1 AND condition2;
SELECT * FROM table_name WHERE condition1 OR condition2;
SELECT * FROM table_name WHERE NOT condition;

-- BETWEEN operator
SELECT * FROM table_name WHERE column_name BETWEEN value1 AND value2;

-- IN operator
SELECT * FROM table_name WHERE column_name IN (value1, value2, value3);

-- LIKE operator (pattern matching)
SELECT * FROM table_name WHERE column_name LIKE 'pattern';
-- % represents zero, one, or multiple characters
-- _ represents a single character
```

### NULL Values
```sql
-- Check for NULL values
SELECT * FROM table_name WHERE column_name IS NULL;
SELECT * FROM table_name WHERE column_name IS NOT NULL;
```

## Joining Tables

### Types of Joins
```sql
-- INNER JOIN (returns records with matching values in both tables)
SELECT columns FROM table1 
INNER JOIN table2 ON table1.column = table2.column;

-- LEFT JOIN (returns all records from left table and matched records from right table)
SELECT columns FROM table1 
LEFT JOIN table2 ON table1.column = table2.column;

-- RIGHT JOIN (returns all records from right table and matched records from left table)
SELECT columns FROM table1 
RIGHT JOIN table2 ON table1.column = table2.column;

-- FULL JOIN (returns all records when there is a match in either table)
SELECT columns FROM table1 
FULL JOIN table2 ON table1.column = table2.column;
```

## Aggregate Functions

### Common Aggregate Functions
```sql
-- COUNT (counts the number of rows)
SELECT COUNT(column_name) FROM table_name;
SELECT COUNT(*) FROM table_name;

-- SUM (calculates the sum of numeric column values)
SELECT SUM(column_name) FROM table_name;

-- AVG (calculates the average of numeric column values)
SELECT AVG(column_name) FROM table_name;

-- MIN (finds the minimum value)
SELECT MIN(column_name) FROM table_name;

-- MAX (finds the maximum value)
SELECT MAX(column_name) FROM table_name;
```

### GROUP BY and HAVING
```sql
-- GROUP BY (groups rows with the same values)
SELECT column1, COUNT(column2) FROM table_name GROUP BY column1;

-- HAVING (filters groups based on a condition)
SELECT column1, COUNT(column2) FROM table_name 
GROUP BY column1 HAVING COUNT(column2) > value;
```

## Creating and Modifying Database Objects

### CREATE - Create new database objects
```sql
-- Create a database
CREATE DATABASE database_name;

-- Create a table
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);

-- Create an index
CREATE INDEX index_name ON table_name (column_name);
```

### ALTER - Modify existing database objects
```sql
-- Add a column
ALTER TABLE table_name ADD column_name datatype;

-- Modify a column
ALTER TABLE table_name ALTER COLUMN column_name datatype;

-- Drop a column
ALTER TABLE table_name DROP COLUMN column_name;
```

### DROP - Remove database objects
```sql
-- Drop a table
DROP TABLE table_name;

-- Drop a database
DROP DATABASE database_name;

-- Drop an index
DROP INDEX index_name ON table_name;
```

## Common Data Types

### Numeric Types
- **INT** - Whole numbers
- **DECIMAL(p,s)** - Fixed precision and scale numbers
- **FLOAT** - Floating point numbers
- **MONEY** - Currency values

### String Types
- **CHAR(n)** - Fixed-length character string
- **VARCHAR(n)** - Variable-length character string
- **TEXT** - Variable unlimited length text

### Date and Time Types
- **DATE** - Date (YYYY-MM-DD)
- **TIME** - Time (HH:MM:SS)
- **DATETIME** - Date and time
- **TIMESTAMP** - Date and time with time zone

### Other Types
- **BOOLEAN** - True/False values
- **BINARY** - Binary data
- **XML** - XML data
- **JSON** - JSON data (in some databases)

## Constraints

### Common Constraints
```sql
-- PRIMARY KEY (uniquely identifies each record)
CREATE TABLE table_name (
    column_name datatype PRIMARY KEY,
    ...
);

-- FOREIGN KEY (links to a primary key in another table)
CREATE TABLE table_name (
    column_name datatype,
    FOREIGN KEY (column_name) REFERENCES other_table(column_name),
    ...
);

-- NOT NULL (ensures a column cannot have NULL value)
CREATE TABLE table_name (
    column_name datatype NOT NULL,
    ...
);

-- UNIQUE (ensures all values in a column are different)
CREATE TABLE table_name (
    column_name datatype UNIQUE,
    ...
);

-- DEFAULT (sets a default value for a column)
CREATE TABLE table_name (
    column_name datatype DEFAULT default_value,
    ...
);

-- CHECK (ensures values in a column satisfy a condition)
CREATE TABLE table_name (
    column_name datatype CHECK (condition),
    ...
);
```

## Transactions

### Transaction Control
```sql
-- Begin a transaction
BEGIN TRANSACTION;

-- Commit a transaction (save changes)
COMMIT;

-- Rollback a transaction (undo changes)
ROLLBACK;

-- Create a savepoint
SAVEPOINT savepoint_name;

-- Rollback to a savepoint
ROLLBACK TO savepoint_name;
```

## Views

### Creating and Using Views
```sql
-- Create a view
CREATE VIEW view_name AS
SELECT column1, column2
FROM table_name
WHERE condition;

-- Query a view
SELECT * FROM view_name;

-- Drop a view
DROP VIEW view_name;
```

## Subqueries

### Types of Subqueries
```sql
-- Subquery in SELECT
SELECT column1, (SELECT AVG(column2) FROM table2) AS avg_value
FROM table1;

-- Subquery in FROM
SELECT a.column1, b.column2
FROM table1 a, (SELECT * FROM table2 WHERE condition) b
WHERE a.column = b.column;

-- Subquery in WHERE
SELECT column1, column2
FROM table1
WHERE column1 IN (SELECT column1 FROM table2 WHERE condition);
```

## Common SQL Functions

### String Functions
```sql
-- CONCAT (combines strings)
SELECT CONCAT(column1, ' ', column2) FROM table_name;

-- UPPER (converts to uppercase)
SELECT UPPER(column_name) FROM table_name;

-- LOWER (converts to lowercase)
SELECT LOWER(column_name) FROM table_name;

-- LENGTH/LEN (returns the length of a string)
SELECT LENGTH(column_name) FROM table_name;

-- SUBSTRING (extracts a substring)
SELECT SUBSTRING(column_name, start, length) FROM table_name;
```

### Date Functions
```sql
-- CURRENT_DATE (returns the current date)
SELECT CURRENT_DATE;

-- CURRENT_TIME (returns the current time)
SELECT CURRENT_TIME;

-- CURRENT_TIMESTAMP (returns the current date and time)
SELECT CURRENT_TIMESTAMP;

-- EXTRACT (extracts parts from a date)
SELECT EXTRACT(YEAR FROM date_column) FROM table_name;

-- DATE_ADD/DATEADD (adds a time interval to a date)
SELECT DATE_ADD(date_column, INTERVAL value unit) FROM table_name;
```

### Numeric Functions
```sql
-- ROUND (rounds a number to a specified number of decimal places)
SELECT ROUND(column_name, decimals) FROM table_name;

-- CEILING (returns the smallest integer greater than or equal to a number)
SELECT CEILING(column_name) FROM table_name;

-- FLOOR (returns the largest integer less than or equal to a number)
SELECT FLOOR(column_name) FROM table_name;

-- ABS (returns the absolute value)
SELECT ABS(column_name) FROM table_name;
```

## Best Practices

1. **Use meaningful table and column names**
2. **Comment your SQL code**
3. **Use appropriate data types**
4. **Create indexes for frequently queried columns**
5. **Use parameterized queries to prevent SQL injection**
6. **Avoid SELECT * in production code**
7. **Use table aliases for clarity in joins**
8. **Test queries with small datasets first**
9. **Use transactions for multiple related operations**
10. **Regularly backup your database**
