# Module 5: Data Types and Structures

## Introduction

Understanding data types is fundamental to working with databases and SQL. Data types define what kind of information can be stored in a database column, how much space it occupies, and what operations can be performed on it. This module introduces common data types used in databases, special data structures, and considerations for choosing appropriate data types for your database design.

## 5.1 Common Data Types

### Numeric Types

Numeric data types store numbers that can be used in mathematical calculations.

#### Integer Types:
- **INTEGER/INT**: Whole numbers without decimal places
- **SMALLINT**: Smaller range integer, uses less storage
- **BIGINT**: Larger range integer, for very large numbers
- **TINYINT**: Very small range integer, minimal storage

#### Decimal Types:
- **DECIMAL/NUMERIC(p,s)**: Exact decimal numbers with precision (p) and scale (s)
- **FLOAT/REAL**: Approximate floating-point numbers
- **DOUBLE PRECISION**: Higher precision floating-point numbers

#### Usage Considerations:
- Use INTEGER for whole numbers (counts, IDs, quantities)
- Use DECIMAL for financial calculations where precision is critical
- Use FLOAT/REAL for scientific calculations where approximate values are acceptable
- Consider storage requirements and value ranges when selecting specific types

#### Example:
```sql
CREATE TABLE Products (
    ProductID INT,
    Quantity INT,
    Weight DECIMAL(5,2),  -- 5 digits total, 2 after decimal point
    Temperature FLOAT
);
```

#### Why This Matters for SQL:
SQL queries often involve numeric operations and comparisons. Understanding numeric types helps you write accurate calculations and avoid unexpected results due to type limitations.

### Text/String Types

Text data types store character data, from single characters to large documents.

#### Common String Types:
- **CHAR(n)**: Fixed-length character strings
- **VARCHAR(n)**: Variable-length character strings with maximum length n
- **TEXT**: Variable-length character strings for large text
- **NCHAR/NVARCHAR**: Unicode character versions (for international characters)

#### Specialized Text Types:
- **ENUM**: String object with a value chosen from a predefined list
- **SET**: String object that can have zero or more values from a predefined list

#### Usage Considerations:
- Use CHAR for fixed-length data (state codes, postal codes)
- Use VARCHAR for variable-length data (names, addresses)
- Use TEXT for long content (descriptions, comments)
- Consider storage space and performance implications

#### Example:
```sql
CREATE TABLE Customers (
    CustomerID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Address VARCHAR(100),
    StateCode CHAR(2),
    Notes TEXT
);
```

#### Why This Matters for SQL:
String operations in SQL (searching, concatenating, extracting substrings) depend on the underlying data type. Understanding string types helps you write effective string manipulation queries.

### Date and Time Types

Date and time data types store temporal information, from simple dates to precise timestamps.

#### Common Date/Time Types:
- **DATE**: Calendar date (year, month, day)
- **TIME**: Time of day (hour, minute, second)
- **DATETIME/TIMESTAMP**: Combined date and time
- **INTERVAL**: Period of time

#### Usage Considerations:
- Use DATE when only the calendar date matters
- Use TIME when only the time of day matters
- Use DATETIME/TIMESTAMP for complete temporal information
- Consider time zone implications with timestamps
- Be aware of the supported date ranges for your database system

#### Example:
```sql
CREATE TABLE Orders (
    OrderID INT,
    OrderDate DATE,
    OrderTime TIME,
    CreatedAt TIMESTAMP,
    DeliveryWindow INTERVAL
);
```

#### Why This Matters for SQL:
SQL includes many functions for working with dates and times (extracting parts, calculating differences, formatting). Understanding date/time types helps you perform temporal calculations and comparisons correctly.

### Boolean Types

Boolean data types store true/false values.

#### Implementation Across Databases:
- **BOOLEAN/BOOL**: True or false (some databases)
- **BIT**: 0 or 1 (SQL Server)
- **TINYINT**: 0 or 1 (MySQL)
- **CHAR(1)**: 'Y'/'N' or 'T'/'F' (some legacy systems)

#### Usage Considerations:
- Use for flags, status indicators, yes/no attributes
- Be aware of how NULL values interact with boolean logic
- Understand your specific database's implementation

#### Example:
```sql
CREATE TABLE UserAccounts (
    UserID INT,
    Username VARCHAR(50),
    IsActive BOOLEAN,
    IsAdmin BOOLEAN,
    MFA_Enabled BIT
);
```

#### Why This Matters for SQL:
Boolean conditions are fundamental to SQL WHERE clauses. Understanding boolean types helps you write logical conditions that correctly filter your data.

### Binary Data

Binary data types store non-textual data such as images, documents, or any binary file.

#### Common Binary Types:
- **BINARY(n)**: Fixed-length binary data
- **VARBINARY(n)**: Variable-length binary data
- **BLOB**: Binary Large Object for storing large binary data
- **BYTEA**: PostgreSQL's binary data type

#### Usage Considerations:
- Consider whether to store binary data in the database or file paths instead
- Be aware of size limitations and performance implications
- Understand how to retrieve and manipulate binary data in your application

#### Example:
```sql
CREATE TABLE Documents (
    DocumentID INT,
    FileName VARCHAR(100),
    FileData BLOB,
    FileType VARCHAR(50)
);
```

#### Why This Matters for SQL:
While SQL isn't optimized for binary data manipulation, understanding binary types helps you make appropriate design decisions for applications that need to store non-textual data.

## 5.2 Special Data Structures

### Arrays and Lists

Some database systems support array types that can store multiple values in a single column.

#### Implementation Across Databases:
- **ARRAY**: Native array type in PostgreSQL
- **JSON Arrays**: Stored as JSON in MySQL, SQL Server
- **Delimited Strings**: Comma-separated values in a VARCHAR (workaround)
- **Separate Tables**: Normalized approach using a one-to-many relationship

#### Usage Considerations:
- Arrays can simplify certain data models but may complicate queries
- Consider normalization principles before using arrays
- Be aware of the query syntax for your specific database

#### Example (PostgreSQL):
```sql
CREATE TABLE Products (
    ProductID INT,
    ProductName VARCHAR(100),
    Categories TEXT[],
    Tags VARCHAR(50)[]
);
```

#### Why This Matters for SQL:
Understanding array structures helps you decide when to use them versus when to normalize data into separate tables, affecting how you write SQL queries.

### JSON and XML Data

Modern databases often support semi-structured data formats like JSON and XML.

#### JSON Support:
- Store complete JSON documents or fragments
- Query and extract specific JSON properties
- Update JSON values
- Index JSON properties for performance

#### XML Support:
- Store XML documents
- Query using XPath or XQuery
- Validate against schemas
- Extract and transform XML data

#### Usage Considerations:
- Useful for flexible schema requirements
- Good for hierarchical or nested data
- Consider performance implications for complex queries
- Balance between flexibility and the benefits of structured data

#### Example:
```sql
CREATE TABLE CustomerProfiles (
    CustomerID INT,
    BasicInfo VARCHAR(100),
    Preferences JSON,
    ExtendedDetails XML
);
```

#### Why This Matters for SQL:
Modern applications often work with JSON or XML data. Understanding how these formats are supported in SQL helps you integrate structured and semi-structured data effectively.

### Spatial Data Types

Spatial data types store geographic information such as points, lines, and polygons.

#### Common Spatial Types:
- **POINT**: A single location (x,y coordinates)
- **LINESTRING**: A sequence of points forming a line
- **POLYGON**: A closed shape defined by a sequence of points
- **GEOMETRY/GEOGRAPHY**: General types that can store various spatial objects

#### Spatial Operations:
- Calculate distances between points
- Determine if a point is within a polygon
- Find intersections between spatial objects
- Calculate areas and lengths

#### Example (PostgreSQL with PostGIS):
```sql
CREATE TABLE Stores (
    StoreID INT,
    StoreName VARCHAR(100),
    Location GEOMETRY(Point)
);

CREATE TABLE DeliveryZones (
    ZoneID INT,
    ZoneName VARCHAR(100),
    Boundary GEOMETRY(Polygon)
);
```

#### Why This Matters for SQL:
Applications with geographic components require spatial data types and operations. Understanding these capabilities helps you implement location-based features using SQL.

### When to Use Different Data Types

Choosing the right data type is a critical decision in database design.

#### Selection Criteria:
- **Data Nature**: What kind of information are you storing?
- **Range Requirements**: What are the minimum and maximum values needed?
- **Precision Requirements**: How exact do numeric values need to be?
- **Storage Efficiency**: How much space is available?
- **Performance Needs**: Which operations will be performed most frequently?
- **Compatibility**: How will the data interact with application code?

#### Common Scenarios and Recommended Types:

| Data Category | Scenario | Recommended Type |
|---------------|----------|------------------|
| **Identifiers** | Primary keys | INT, BIGINT |
| **Names** | Person names | VARCHAR(50-100) |
| **Descriptions** | Product details | VARCHAR(255) or TEXT |
| **Money** | Prices, amounts | DECIMAL(precision,2) |
| **Dates** | Birth dates, event dates | DATE |
| **Timestamps** | Event logging | TIMESTAMP |
| **Status** | Active/inactive flags | BOOLEAN/BIT |
| **Codes** | Country codes, postal codes | CHAR(fixed length) |
| **Large Text** | Comments, articles | TEXT |
| **Files** | Document storage | BLOB or file path in VARCHAR |

#### Why This Matters for SQL:
The data types you choose affect not only storage but also how you can query and manipulate the data with SQL. Making informed choices leads to more efficient and effective database operations.

## 5.3 Data Type Considerations

### Storage Requirements

Different data types have different storage requirements, which can significantly impact database size and performance.

#### Storage Basics:
- **Fixed-Length Types**: Always use the same amount of storage (CHAR, INT)
- **Variable-Length Types**: Use storage based on actual content (VARCHAR, TEXT)
- **Numeric Storage**: Depends on precision and range
- **NULL Values**: May require additional storage for tracking

#### Approximate Storage Requirements:
- **INT**: 4 bytes
- **BIGINT**: 8 bytes
- **DECIMAL(p,s)**: Varies based on precision
- **CHAR(n)**: n bytes (plus character set overhead)
- **VARCHAR(n)**: Actual length + 1-2 bytes overhead
- **DATE**: 3-4 bytes
- **TIMESTAMP**: 4-8 bytes

#### Storage Optimization Strategies:
- Use the smallest data type that meets requirements
- Consider SMALLINT instead of INT for limited ranges
- Use VARCHAR instead of CHAR for variable-length text
- Evaluate whether NULL values are necessary

#### Why This Matters for SQL:
Efficient storage design affects database performance, backup time, and hardware requirements. Understanding storage implications helps you make cost-effective design decisions.

### Performance Implications

Data types can significantly impact query performance.

#### Performance Factors:
- **Indexing**: Some types are more efficiently indexed than others
- **Comparison Operations**: Fixed-length types compare faster than variable-length
- **Joins**: Matching data types perform better in joins
- **Functions**: Type conversions in queries can reduce performance
- **I/O Operations**: Smaller data types reduce disk I/O

#### Performance Optimization Strategies:
- Match data types across tables for join columns
- Use fixed-length types for frequently searched columns
- Avoid unnecessary type conversions in queries
- Consider indexed computed columns for frequent transformations
- Test performance with representative data volumes

#### Example Performance Impact:
```sql
-- Less efficient (type conversion required)
SELECT * FROM Orders WHERE OrderID = '1001';

-- More efficient (no conversion)
SELECT * FROM Orders WHERE OrderID = 1001;
```

#### Why This Matters for SQL:
SQL query performance depends partly on data type choices. Understanding performance implications helps you write efficient queries and design performant databases.

### Conversion Between Types

Data type conversion (casting) is often necessary in SQL operations.

#### Conversion Types:
- **Implicit Conversion**: Automatic conversion performed by the database
- **Explicit Conversion**: Conversion specified in the query using CAST or CONVERT

#### Common Conversion Scenarios:
- Converting strings to numbers for calculations
- Formatting dates as strings for display
- Converting between different numeric types
- Extracting parts of dates or times

#### Conversion Risks:
- Data loss (truncation)
- Precision loss
- Runtime errors if conversion is impossible
- Performance impact

#### Example Conversions:
```sql
-- Explicit conversion of string to integer
SELECT CAST('100' AS INT) AS NumberValue;

-- Explicit conversion of date to string
SELECT CAST(OrderDate AS VARCHAR) AS OrderDateString FROM Orders;

-- Implicit conversion (may work but not recommended)
SELECT * FROM Orders WHERE OrderID = '1001';
```

#### Why This Matters for SQL:
SQL often requires converting between data types. Understanding conversion principles helps you write correct queries and avoid unexpected results or errors.

### Data Type Compatibility

Data type compatibility affects how values can be compared, combined, and operated upon.

#### Compatibility Considerations:
- **Assignment Compatibility**: Can one type be assigned to another?
- **Comparison Compatibility**: Can two types be directly compared?
- **Operation Compatibility**: Can operations be performed between types?
- **Function Compatibility**: Which functions can be applied to which types?

#### Common Compatibility Issues:
- Comparing strings and numbers
- Joining tables on columns with different types
- Performing arithmetic on inappropriate types
- Using string functions on numeric data

#### Best Practices:
- Ensure join columns have matching types
- Explicitly convert types when necessary
- Be aware of implicit conversion rules in your database
- Test queries with edge cases to verify correct behavior

#### Example:
```sql
-- Potential compatibility issue (joining on different types)
SELECT c.CustomerName, o.OrderID
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID;
-- Works if both are INT, may fail or perform poorly if different types
```

#### Why This Matters for SQL:
Understanding data type compatibility helps you avoid subtle bugs and performance issues in SQL queries, especially when working with data from different sources.

## Practical Exercises

1. **Data Type Selection Exercise**:
   - For each of the following data items, identify the most appropriate SQL data type and explain your reasoning:
     - Person's age
     - Email address
     - Phone number
     - Product price
     - Order status (New, Processing, Shipped, Delivered)
     - User's profile picture
     - Blog post content
     - Geographic coordinates

2. **Storage Calculation**:
   - Design a simple customer table with at least 8 columns using appropriate data types
   - Calculate the approximate storage required per row
   - Estimate the total storage needed for 1 million customers
   - Suggest optimizations to reduce storage requirements

3. **Type Conversion Practice**:
   - Write SQL statements to perform the following conversions:
     - Convert a string '2023-04-15' to a date
     - Convert a number 1234.56 to a string with currency format
     - Extract the year from a date
     - Convert a comma-separated string to individual rows

4. **Performance Comparison**:
   - Create two simple table designs for storing product information:
     - One using optimal data types
     - One using suboptimal types (e.g., VARCHAR for numeric data)
   - Discuss how these designs might affect:
     - Storage requirements
     - Query performance
     - Data integrity
     - Application code complexity

## Summary

This module covered the essential concepts of data types and structures used in databases:

- **Common Data Types**: Numeric, text/string, date and time, boolean, and binary types
- **Special Data Structures**: Arrays, JSON/XML, and spatial data types
- **Data Type Considerations**: Storage requirements, performance implications, type conversion, and compatibility

Understanding data types is fundamental to effective database design and SQL development. The right data type choices lead to efficient storage, better performance, and more reliable data operations.

## Next Steps

Now that you understand the various data types used in databases, you're ready to explore logical thinking and problem-solving skills that are essential for effective database querying and design. The next module will cover logical operators, algorithmic thinking, set theory, and pattern recognition in data.

## Additional Resources

- [MySQL Data Types Reference](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)
- [PostgreSQL Data Types](https://www.postgresql.org/docs/current/datatype.html)
- [SQL Server Data Types](https://docs.microsoft.com/en-us/sql/t-sql/data-types/data-types-transact-sql)
- [Data Type Selection Best Practices](https://www.sisense.com/blog/better-sql-schema/)
- [Working with JSON in SQL](https://www.postgresql.org/docs/current/functions-json.html)
