# Module 6: Logical Thinking and Problem-Solving

## Introduction

Logical thinking and problem-solving skills are essential for working with databases and SQL. These skills help you formulate effective queries, design efficient database structures, and troubleshoot issues. This module introduces fundamental concepts of logic, algorithmic thinking, set theory, and pattern recognition that will enhance your ability to work with data and SQL.

## 6.1 Logical Operators and Expressions

### AND, OR, NOT Concepts

Logical operators allow you to combine or modify conditions in SQL queries, particularly in WHERE clauses.

#### Basic Logical Operators:

- **AND**: Returns true when both conditions are true
- **OR**: Returns true when at least one condition is true
- **NOT**: Reverses the result of a condition

#### Truth Tables:

**AND Truth Table:**
| Condition 1 | Condition 2 | Result |
|-------------|-------------|--------|
| True        | True        | True   |
| True        | False       | False  |
| False       | True        | False  |
| False       | False       | False  |

**OR Truth Table:**
| Condition 1 | Condition 2 | Result |
|-------------|-------------|--------|
| True        | True        | True   |
| True        | False       | True   |
| False       | True        | True   |
| False       | False       | False  |

**NOT Truth Table:**
| Condition | Result |
|-----------|--------|
| True      | False  |
| False     | True   |

#### SQL Examples:

```sql
-- AND example: Find customers from New York who have spent over $1000
SELECT * FROM Customers
WHERE State = 'NY' AND TotalSpent > 1000;

-- OR example: Find customers from either New York or California
SELECT * FROM Customers
WHERE State = 'NY' OR State = 'CA';

-- NOT example: Find customers who are not from New York
SELECT * FROM Customers
WHERE NOT State = 'NY';
-- Alternatively: WHERE State <> 'NY';
```

#### Why This Matters for SQL:
Logical operators are fundamental to filtering data in SQL queries. Understanding how they work helps you write precise conditions to retrieve exactly the data you need.

### Comparison Operators

Comparison operators allow you to compare values in SQL conditions.

#### Common Comparison Operators:

- **=** (Equal to)
- **<>** or **!=** (Not equal to)
- **<** (Less than)
- **>** (Greater than)
- **<=** (Less than or equal to)
- **>=** (Greater than or equal to)
- **BETWEEN** (Within a range)
- **IN** (Matches any value in a list)
- **LIKE** (Pattern matching for strings)
- **IS NULL** / **IS NOT NULL** (Checking for NULL values)

#### SQL Examples:

```sql
-- Equal to
SELECT * FROM Products WHERE Category = 'Electronics';

-- Greater than
SELECT * FROM Orders WHERE TotalAmount > 500;

-- Between a range
SELECT * FROM Products WHERE Price BETWEEN 10 AND 50;

-- In a list
SELECT * FROM Customers WHERE State IN ('NY', 'CA', 'TX');

-- Pattern matching
SELECT * FROM Products WHERE ProductName LIKE 'Apple%';

-- NULL checking
SELECT * FROM Customers WHERE Phone IS NULL;
```

#### Special Considerations:

- **NULL Values**: NULL represents unknown or missing data and requires special handling
  - NULL is not equal to anything, including itself
  - Use IS NULL or IS NOT NULL, not = NULL or <> NULL
  - NULL in logical operations typically yields NULL, not true or false

- **String Comparisons**: May be case-sensitive or case-insensitive depending on database settings
  - Use functions like UPPER() or LOWER() for case-insensitive comparisons when needed

#### Why This Matters for SQL:
Comparison operators are essential for filtering data in SQL queries. Understanding their behavior, especially with NULL values, helps you write accurate conditions.

### Order of Operations

SQL, like mathematics, follows a specific order of operations when evaluating expressions.

#### Standard Order of Precedence:

1. Parentheses ()
2. Multiplication *, Division /
3. Addition +, Subtraction -
4. Comparison operators =, <>, <, >, <=, >=
5. NOT
6. AND
7. OR

#### Example:

```sql
-- Without parentheses (AND has higher precedence than OR)
SELECT * FROM Products
WHERE Category = 'Electronics' OR Category = 'Computers' AND Price < 500;

-- This is interpreted as:
SELECT * FROM Products
WHERE Category = 'Electronics' OR (Category = 'Computers' AND Price < 500);

-- With parentheses to change the order
SELECT * FROM Products
WHERE (Category = 'Electronics' OR Category = 'Computers') AND Price < 500;
```

#### Best Practices:

- Use parentheses to make your intent explicit, even when not strictly necessary
- Break complex conditions into smaller, clearer parts
- Test complex conditions with sample data to verify they work as expected

#### Why This Matters for SQL:
Understanding the order of operations helps you write complex conditions that are evaluated correctly. Parentheses give you control over how expressions are grouped and evaluated.

### Truth Tables and Boolean Logic

Truth tables provide a systematic way to understand how logical operators behave with different inputs.

#### Complex Truth Tables:

**Combining AND and OR:**
| A | B | C | A AND B | A OR B | (A AND B) OR C | A AND (B OR C) |
|---|---|---|---------|--------|----------------|----------------|
| T | T | T | T       | T      | T              | T              |
| T | T | F | T       | T      | T              | T              |
| T | F | T | F       | T      | T              | T              |
| T | F | F | F       | T      | F              | F              |
| F | T | T | F       | T      | T              | F              |
| F | T | F | F       | T      | F              | F              |
| F | F | T | F       | F      | T              | F              |
| F | F | F | F       | F      | F              | F              |

#### Boolean Expressions in SQL:

- Conditions evaluate to TRUE, FALSE, or UNKNOWN (when NULL values are involved)
- Rows are included in results when the WHERE condition evaluates to TRUE
- Rows are excluded when the condition evaluates to FALSE or UNKNOWN

#### Three-Valued Logic with NULL:

| A     | B     | A AND B | A OR B  | NOT A   |
|-------|-------|---------|---------|---------|
| TRUE  | TRUE  | TRUE    | TRUE    | FALSE   |
| TRUE  | FALSE | FALSE   | TRUE    | FALSE   |
| TRUE  | NULL  | NULL    | TRUE    | FALSE   |
| FALSE | TRUE  | FALSE   | TRUE    | TRUE    |
| FALSE | FALSE | FALSE   | FALSE   | TRUE    |
| FALSE | NULL  | FALSE   | NULL    | TRUE    |
| NULL  | TRUE  | NULL    | TRUE    | NULL    |
| NULL  | FALSE | FALSE   | NULL    | NULL    |
| NULL  | NULL  | NULL    | NULL    | NULL    |

#### Why This Matters for SQL:
Understanding boolean logic and truth tables helps you predict how complex conditions will behave, especially when NULL values are involved. This knowledge is crucial for writing accurate SQL queries.

## 6.2 Algorithmic Thinking

### Step-by-Step Problem Solving

Algorithmic thinking involves breaking down problems into clear, sequential steps.

#### Key Principles:

- **Problem Definition**: Clearly understand what problem you're trying to solve
- **Input/Output Identification**: Define what data you have and what results you need
- **Step Decomposition**: Break the solution into smaller, manageable steps
- **Logical Sequence**: Arrange steps in the correct order
- **Edge Cases**: Consider special situations or boundary conditions
- **Testing**: Verify the solution with sample data

#### Example Problem: Finding High-Value Customers

**Problem Definition**: Identify customers who have spent more than $1000 in the last year and have made at least 3 purchases.

**Step-by-Step Approach**:
1. Determine the date range (one year from today)
2. Find all orders within that date range
3. Group orders by customer
4. Calculate total spending per customer
5. Count the number of orders per customer
6. Filter for customers with total spending > $1000 AND order count >= 3
7. Return the filtered customer list

**SQL Implementation**:
```sql
SELECT 
    c.CustomerID,
    c.CustomerName,
    COUNT(o.OrderID) AS OrderCount,
    SUM(o.TotalAmount) AS TotalSpent
FROM 
    Customers c
JOIN 
    Orders o ON c.CustomerID = o.CustomerID
WHERE 
    o.OrderDate >= DATE_SUB(CURRENT_DATE, INTERVAL 1 YEAR)
GROUP BY 
    c.CustomerID, c.CustomerName
HAVING 
    COUNT(o.OrderID) >= 3 AND SUM(o.TotalAmount) > 1000
ORDER BY 
    TotalSpent DESC;
```

#### Why This Matters for SQL:
Complex SQL queries often require algorithmic thinking to break down business requirements into logical steps. This approach helps you write queries that correctly solve the problem at hand.

### Flowcharts and Pseudocode

Flowcharts and pseudocode are tools for planning and communicating algorithms before writing actual code or SQL.

#### Flowcharts:
- Visual representations of algorithms using standardized symbols
- Show the flow of control and decision points
- Help visualize branching logic and loops

#### Common Flowchart Symbols:
- **Oval**: Start/End
- **Rectangle**: Process/Action
- **Diamond**: Decision (branching)
- **Parallelogram**: Input/Output
- **Arrow**: Flow direction

#### Pseudocode:
- Informal, human-readable description of an algorithm
- Uses structured language but isn't tied to any specific programming syntax
- Focuses on logic rather than implementation details

#### Example: Customer Classification Algorithm

**Flowchart Description**:
1. Start
2. Retrieve customer data
3. For each customer:
   4. Calculate total spending
   5. If total spending > 1000, classify as "Premium"
   6. Else if total spending > 500, classify as "Regular"
   7. Else classify as "Basic"
8. End

**Pseudocode**:
```
BEGIN
  RETRIEVE customers FROM database
  FOR EACH customer IN customers DO
    SET totalSpending = SUM of all orders for customer
    IF totalSpending > 1000 THEN
      SET customerType = "Premium"
    ELSE IF totalSpending > 500 THEN
      SET customerType = "Regular"
    ELSE
      SET customerType = "Basic"
    END IF
    UPDATE customer SET Type = customerType
  END FOR
END
```

**SQL Implementation**:
```sql
UPDATE Customers
SET CustomerType = 
    CASE 
        WHEN (SELECT SUM(TotalAmount) FROM Orders WHERE CustomerID = Customers.CustomerID) > 1000 THEN 'Premium'
        WHEN (SELECT SUM(TotalAmount) FROM Orders WHERE CustomerID = Customers.CustomerID) > 500 THEN 'Regular'
        ELSE 'Basic'
    END;
```

#### Why This Matters for SQL:
Flowcharts and pseudocode help you plan complex SQL operations before writing the actual queries. They provide a clear roadmap for implementing business logic in SQL.

### Breaking Down Complex Problems

Complex data problems often require breaking them down into smaller, more manageable parts.

#### Strategies for Problem Decomposition:

- **Divide and Conquer**: Split the problem into independent sub-problems
- **Stepwise Refinement**: Start with a high-level solution and progressively add details
- **Bottom-Up Approach**: Solve small parts first, then combine them
- **Abstraction**: Focus on essential aspects while ignoring details initially

#### Example: Sales Analysis Problem

**Complex Problem**: Generate a report showing quarterly sales by product category, comparing current year to previous year, with percent change.

**Decomposition**:
1. **Sub-problem 1**: Identify the time periods (current year quarters and previous year quarters)
2. **Sub-problem 2**: Calculate sales by product category for each quarter of the current year
3. **Sub-problem 3**: Calculate sales by product category for each quarter of the previous year
4. **Sub-problem 4**: Join the results and calculate the percent change
5. **Sub-problem 5**: Format the final report

**Implementation Approach**:
- Create temporary tables or CTEs (Common Table Expressions) for each sub-problem
- Build the solution incrementally, testing each part
- Combine the parts into the final query

```sql
-- Example using Common Table Expressions (CTEs)
WITH CurrentYearSales AS (
    -- Sub-problem 2: Current year sales by quarter and category
    SELECT 
        QUARTER(OrderDate) AS Quarter,
        p.Category,
        SUM(oi.Quantity * oi.UnitPrice) AS Sales
    FROM 
        OrderItems oi
    JOIN 
        Orders o ON oi.OrderID = o.OrderID
    JOIN 
        Products p ON oi.ProductID = p.ProductID
    WHERE 
        YEAR(OrderDate) = YEAR(CURRENT_DATE)
    GROUP BY 
        QUARTER(OrderDate), p.Category
),
PreviousYearSales AS (
    -- Sub-problem 3: Previous year sales by quarter and category
    SELECT 
        QUARTER(OrderDate) AS Quarter,
        p.Category,
        SUM(oi.Quantity * oi.UnitPrice) AS Sales
    FROM 
        OrderItems oi
    JOIN 
        Orders o ON oi.OrderID = o.OrderID
    JOIN 
        Products p ON oi.ProductID = p.ProductID
    WHERE 
        YEAR(OrderDate) = YEAR(CURRENT_DATE) - 1
    GROUP BY 
        QUARTER(OrderDate), p.Category
)
-- Sub-problem 4 & 5: Join and calculate percent change
SELECT 
    cy.Quarter,
    cy.Category,
    cy.Sales AS CurrentYearSales,
    py.Sales AS PreviousYearSales,
    CASE 
        WHEN py.Sales = 0 THEN 'N/A' 
        ELSE CONCAT(ROUND((cy.Sales - py.Sales) / py.Sales * 100, 2), '%')
    END AS PercentChange
FROM 
    CurrentYearSales cy
LEFT JOIN 
    PreviousYearSales py ON cy.Quarter = py.Quarter AND cy.Category = py.Category
ORDER BY 
    cy.Quarter, cy.Category;
```

#### Why This Matters for SQL:
Breaking down complex problems is essential for writing sophisticated SQL queries. This approach makes complex analyses manageable and helps ensure accuracy in your results.

## 6.3 Set Theory Basics

### Sets and Members

Set theory provides a mathematical foundation for understanding database operations, particularly for SQL queries involving multiple tables.

#### Key Concepts:

- **Set**: A collection of distinct elements (members)
- **Member/Element**: An individual item in a set
- **Empty Set**: A set containing no elements
- **Subset**: A set whose elements are all contained in another set
- **Universal Set**: A set containing all possible elements in a given context

#### Database Relation to Sets:

- Tables can be viewed as sets of rows
- Query results are sets of rows
- Duplicate rows in results depend on whether DISTINCT is used

#### SQL Examples:

```sql
-- Creating a set of distinct values
SELECT DISTINCT Category FROM Products;

-- Checking if a value is a member of a set
SELECT * FROM Products WHERE Category IN ('Electronics', 'Computers');

-- Finding the empty set (no results)
SELECT * FROM Orders WHERE OrderDate > CURRENT_DATE;
```

#### Why This Matters for SQL:
Understanding sets helps you conceptualize how SQL operations work, especially when dealing with multiple tables and result sets.

### Union, Intersection, Difference

Set operations in SQL allow you to combine or compare results from multiple queries.

#### Primary Set Operations:

- **UNION**: Combines results from two queries, removing duplicates
- **UNION ALL**: Combines results from two queries, keeping duplicates
- **INTERSECT**: Returns only rows that appear in both query results
- **EXCEPT/MINUS**: Returns rows from the first query that don't appear in the second

#### Visual Representation:

- **UNION**: A ∪ B (all elements in either A or B or both)
- **INTERSECTION**: A ∩ B (only elements in both A and B)
- **DIFFERENCE**: A - B (elements in A but not in B)

#### SQL Examples:

```sql
-- UNION: Customers from both New York and California
SELECT * FROM Customers WHERE State = 'NY'
UNION
SELECT * FROM Customers WHERE State = 'CA';

-- INTERSECT: Products that are both electronic and on sale
SELECT ProductID FROM Products WHERE Category = 'Electronics'
INTERSECT
SELECT ProductID FROM Products WHERE OnSale = 1;

-- EXCEPT: Customers who have placed orders but not returned anything
SELECT CustomerID FROM Customers
EXCEPT
SELECT CustomerID FROM Returns;
```

#### Requirements for Set Operations:

- The number of columns must be the same in both queries
- Corresponding columns must have compatible data types
- Column names from the first query are used for the result

#### Why This Matters for SQL:
Set operations provide powerful ways to combine and compare data from different queries. Understanding these operations helps you solve complex data problems efficiently.

### How Set Theory Relates to Databases

Set theory concepts directly map to many database operations and SQL features.

#### Key Relationships:

- **Tables as Sets**: Each table represents a set of records
- **Rows as Elements**: Each row is an element in the set
- **Primary Keys**: Ensure elements in the set are unique
- **Foreign Keys**: Establish relationships between sets
- **Joins**: Combine sets based on related elements
- **WHERE Clause**: Filters elements based on conditions
- **GROUP BY**: Creates subsets based on common attributes

#### Common Database Operations as Set Operations:

- **SELECT**: Creates a subset of rows and columns
- **DISTINCT**: Removes duplicate elements from a set
- **JOIN**: Creates a new set from the Cartesian product of two sets, filtered by a condition
- **UNION/INTERSECT/EXCEPT**: Direct implementations of set operations

#### Example: Set Theory in Database Design

Consider a database with Students and Courses:
- Each Student can enroll in multiple Courses
- Each Course can have multiple Students enrolled
- This many-to-many relationship creates a set of Enrollments

The database might have these tables:
- Students (set of all students)
- Courses (set of all courses)
- Enrollments (set of student-course pairs)

#### Why This Matters for SQL:
Understanding the set theory foundations of databases helps you conceptualize how data is organized and manipulated. This knowledge leads to more effective database design and querying.

## 6.4 Pattern Recognition in Data

### Identifying Trends and Patterns

Recognizing patterns in data is a crucial skill for data analysis and database querying.

#### Common Data Patterns:

- **Trends**: Directional movements over time (increasing, decreasing, cyclical)
- **Clusters**: Groups of similar data points
- **Outliers**: Data points that significantly differ from others
- **Correlations**: Relationships between different data attributes
- **Seasonality**: Regular patterns that repeat at predictable intervals
- **Distributions**: How values are spread across a range

#### SQL Tools for Pattern Recognition:

- **Aggregate Functions**: COUNT, SUM, AVG, MIN, MAX
- **Window Functions**: Running totals, moving averages
- **GROUP BY**: Identifying patterns within categories
- **ORDER BY**: Arranging data to make patterns visible
- **HAVING**: Filtering groups based on aggregate properties
- **Date Functions**: Extracting year, month, day for temporal patterns

#### Example: Sales Trend Analysis

```sql
-- Monthly sales trend
SELECT 
    YEAR(OrderDate) AS Year,
    MONTH(OrderDate) AS Month,
    SUM(TotalAmount) AS MonthlySales,
    COUNT(OrderID) AS OrderCount,
    AVG(TotalAmount) AS AverageOrderValue
FROM 
    Orders
GROUP BY 
    YEAR(OrderDate), MONTH(OrderDate)
ORDER BY 
    Year, Month;

-- Identifying seasonal patterns
SELECT 
    MONTH(OrderDate) AS Month,
    AVG(SUM(TotalAmount)) OVER (PARTITION BY MONTH(OrderDate)) AS AvgMonthlySales
FROM 
    Orders
GROUP BY 
    YEAR(OrderDate), MONTH(OrderDate)
ORDER BY 
    Month;
```

#### Why This Matters for SQL:
SQL provides powerful tools for identifying patterns in data. Understanding how to use these tools helps you extract valuable insights from your database.

### Grouping and Categorizing Data

Grouping data is essential for summarizing information and identifying patterns within categories.

#### Grouping Concepts:

- **Categories**: Dividing data into meaningful groups
- **Hierarchies**: Nested groupings (e.g., Region > Country > City)
- **Binning**: Grouping continuous values into discrete ranges
- **Segmentation**: Dividing data based on multiple attributes

#### SQL Grouping Techniques:

- **GROUP BY**: Basic grouping by one or more columns
- **ROLLUP**: Hierarchical grouping with subtotals
- **CUBE**: Multi-dimensional grouping with all possible combinations
- **GROUPING SETS**: Custom combinations of group-by columns
- **CASE**: Creating custom categories

#### Example: Customer Segmentation

```sql
-- Basic grouping by location
SELECT 
    Country,
    State,
    COUNT(CustomerID) AS CustomerCount,
    AVG(TotalSpent) AS AverageSpent
FROM 
    Customers
GROUP BY 
    Country, State
ORDER BY 
    Country, State;

-- Custom segmentation by spending
SELECT 
    CASE 
        WHEN TotalSpent >= 5000 THEN 'High Value'
        WHEN TotalSpent >= 1000 THEN 'Medium Value'
        ELSE 'Low Value'
    END AS CustomerSegment,
    COUNT(CustomerID) AS CustomerCount,
    AVG(TotalSpent) AS AverageSpent
FROM 
    Customers
GROUP BY 
    CASE 
        WHEN TotalSpent >= 5000 THEN 'High Value'
        WHEN TotalSpent >= 1000 THEN 'Medium Value'
        ELSE 'Low Value'
    END
ORDER BY 
    AverageSpent DESC;
```

#### Why This Matters for SQL:
Effective grouping and categorization help you summarize large datasets and identify patterns within specific segments. These skills are essential for data analysis using SQL.

### Filtering and Sorting Concepts

Filtering and sorting are fundamental operations for focusing on relevant data and arranging it in meaningful ways.

#### Filtering Concepts:

- **Inclusion/Exclusion**: Determining which records to include or exclude
- **Simple Filters**: Based on single conditions
- **Complex Filters**: Combining multiple conditions with logical operators
- **Subquery Filters**: Using results from one query to filter another
- **Existence Filters**: Including or excluding based on related records

#### Sorting Concepts:

- **Ordering**: Arranging data in a specific sequence
- **Sort Keys**: Columns used for ordering
- **Sort Direction**: Ascending or descending
- **Multiple Sort Levels**: Primary, secondary, tertiary sorting
- **Null Handling**: Determining where NULL values appear in sorted results

#### SQL Implementation:

```sql
-- Basic filtering
SELECT * FROM Products WHERE Category = 'Electronics' AND Price < 500;

-- Complex filtering with subquery
SELECT * FROM Customers
WHERE CustomerID IN (
    SELECT CustomerID FROM Orders
    WHERE OrderDate >= DATE_SUB(CURRENT_DATE, INTERVAL 90 DAY)
    GROUP BY CustomerID
    HAVING COUNT(OrderID) >= 3
);

-- Existence filtering
SELECT * FROM Products p
WHERE EXISTS (
    SELECT 1 FROM OrderItems oi
    WHERE oi.ProductID = p.ProductID
);

-- Basic sorting
SELECT * FROM Products
ORDER BY Price DESC, ProductName ASC;

-- Custom sorting
SELECT * FROM Orders
ORDER BY 
    CASE 
        WHEN Status = 'Pending' THEN 1
        WHEN Status = 'Processing' THEN 2
        WHEN Status = 'Shipped' THEN 3
        ELSE 4
    END,
    OrderDate DESC;
```

#### Why This Matters for SQL:
Filtering and sorting are essential for focusing on relevant data and presenting it in a meaningful order. These operations are fundamental to almost all SQL queries.

## Practical Exercises

1. **Logical Operators Exercise**:
   - Write SQL conditions for the following scenarios:
     - Customers who live in New York or California and have spent over $1000
     - Products that are either electronics with a price under $500 or books with a price under $50
     - Orders placed in the last 30 days that have not been shipped
     - Employees who are either in the Sales department with less than 2 years of experience or in the Marketing department with more than 5 years of experience

2. **Problem Decomposition Practice**:
   - Break down the following problem into steps:
     - Find the top 3 selling products in each category for the current year, including their sales quantity, revenue, and percentage of category total
   - Write pseudocode for the solution
   - Implement the solution in SQL if possible

3. **Set Operations Challenge**:
   - Given two tables, ActiveCustomers and InactiveCustomers, write SQL queries to find:
     - All customers (active and inactive)
     - Customers who appear in both tables (data inconsistency)
     - Active customers who are not marked as inactive
   - Explain how set theory relates to each query

4. **Pattern Recognition Exercise**:
   - Given a sales data table with columns for Date, Product, Category, and Amount:
     - Write a query to identify monthly sales trends
     - Create a query to find the top-selling category for each quarter
     - Develop a query to identify products with unusual sales patterns (significantly higher or lower than their average)

## Summary

This module covered the essential concepts of logical thinking and problem-solving that form the foundation for effective database querying and design:

- **Logical Operators and Expressions**: AND, OR, NOT concepts, comparison operators, order of operations, and boolean logic
- **Algorithmic Thinking**: Step-by-step problem solving, flowcharts and pseudocode, and breaking down complex problems
- **Set Theory Basics**: Sets and members, union/intersection/difference, and how set theory relates to databases
- **Pattern Recognition in Data**: Identifying trends and patterns, grouping and categorizing data, and filtering and sorting concepts

These skills are crucial for formulating effective SQL queries, designing efficient database structures, and solving data-related problems.

## Next Steps

Now that you understand the logical thinking and problem-solving skills needed for SQL, you're ready to explore basic programming concepts. The next module will cover variables, control structures, functions, and error handling—concepts that will help you understand how SQL works as a programming language.

## Additional Resources

- [Introduction to Boolean Logic](https://www.khanacademy.org/computing/computer-science/algorithms/intro-to-algorithms/a/a-guessing-game)
- [Algorithmic Thinking and Problem Solving](https://www.coursera.org/learn/algorithmic-thinking)
- [Set Theory Fundamentals](https://www.mathsisfun.com/sets/sets-introduction.html)
- [Data Analysis Techniques](https://www.datacamp.com/courses/introduction-to-data-science-in-python)
- [SQL for Data Analysis](https://www.udacity.com/course/sql-for-data-analysis--ud198)
