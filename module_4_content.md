# Module 4: Data Models and Relationships

## Introduction

In the previous modules, we explored basic computer skills, data fundamentals, and database concepts. Now, we'll delve deeper into how data is modeled and related within databases. Understanding data models and relationships is crucial for effective database design and forms the foundation for writing meaningful SQL queries. This module will introduce you to the concepts of data modeling, entity relationships, and database schema design.

## 4.1 Data Modeling Concepts

### Entity-Relationship Diagrams

Entity-Relationship (ER) diagrams are visual tools used to represent the structure of a database, showing the entities (objects), their attributes, and the relationships between them.

#### Key Components:

- **Entities**: Objects or concepts about which we want to store data (e.g., Customer, Product, Order)
- **Attributes**: Properties or characteristics of entities (e.g., CustomerName, ProductPrice)
- **Relationships**: Connections between entities (e.g., Customer places Order)
- **Cardinality**: The numerical relationship between entity instances (one-to-one, one-to-many, many-to-many)

#### Common Notation Styles:
- **Chen Notation**: Uses rectangles for entities, ovals for attributes, and diamonds for relationships
- **Crow's Foot Notation**: Uses rectangles for entities and different line endings to show cardinality
- **UML Class Diagrams**: Uses rectangles divided into sections for entity names, attributes, and methods

#### Example ER Diagram (Crow's Foot Notation):
```
[Customer] 1----< [Order] >----< [OrderItem] >----1 [Product]
```

In this simple diagram:
- A Customer can place many Orders (one-to-many)
- An Order can contain many OrderItems (one-to-many)
- A Product can appear in many OrderItems (one-to-many)

#### Why This Matters for SQL:
ER diagrams help you visualize the structure of a database before writing SQL. Understanding these diagrams helps you plan effective queries, especially when joining multiple tables.

### Conceptual, Logical, and Physical Data Models

Data modeling typically progresses through three levels of abstraction, from high-level concepts to implementation details.

#### Conceptual Data Model:
- **Purpose**: Represent high-level business concepts and their relationships
- **Audience**: Business stakeholders, non-technical users
- **Elements**: Major entities and relationships, minimal attributes
- **Example**: A simple diagram showing that Customers place Orders for Products

#### Logical Data Model:
- **Purpose**: Define the structure of data elements and their relationships
- **Audience**: Business analysts, database designers
- **Elements**: All entities, attributes, relationships, primary keys, foreign keys
- **Example**: Detailed ER diagram with all attributes and relationships defined

#### Physical Data Model:
- **Purpose**: Specify how the logical model will be implemented in a specific database system
- **Audience**: Database administrators, developers
- **Elements**: Tables, columns, data types, constraints, indexes, stored procedures
- **Example**: SQL scripts or database-specific diagrams showing implementation details

#### Progression Example:
- **Conceptual**: Customer places Order
- **Logical**: Customer (CustomerID, Name, Email) places Order (OrderID, OrderDate, CustomerID)
- **Physical**: CREATE TABLE Customers (CustomerID INT PRIMARY KEY, Name VARCHAR(100), Email VARCHAR(100)); CREATE TABLE Orders (OrderID INT PRIMARY KEY, OrderDate DATE, CustomerID INT FOREIGN KEY REFERENCES Customers(CustomerID));

#### Why This Matters for SQL:
SQL is used to implement the physical data model. Understanding the progression from conceptual to physical helps you write SQL that accurately reflects business requirements.

### Normalization Introduction

Normalization is a database design technique that reduces data redundancy and improves data integrity by organizing data into multiple related tables.

#### Key Concepts:

- **Data Redundancy**: Storing the same data in multiple places
- **Data Anomalies**: Problems that can occur with redundant data:
  - **Insertion Anomalies**: Difficulty adding new data
  - **Update Anomalies**: Inconsistencies when updating data
  - **Deletion Anomalies**: Unintended data loss when deleting records

#### Normal Forms:
- **First Normal Form (1NF)**: Eliminate repeating groups, create separate tables for related data
- **Second Normal Form (2NF)**: Meet 1NF requirements and remove partial dependencies
- **Third Normal Form (3NF)**: Meet 2NF requirements and remove transitive dependencies
- **Higher Normal Forms**: BCNF, 4NF, 5NF, and 6NF address more specific issues

#### Example of Normalization:

**Unnormalized Table**:
| OrderID | CustomerName | CustomerEmail | ProductName | ProductPrice | Quantity | OrderDate |
|---------|--------------|---------------|-------------|--------------|----------|-----------|
| 1       | John Smith   | john@example.com | Laptop     | 1200.00      | 1        | 2023-01-15 |
| 2       | John Smith   | john@example.com | Mouse      | 25.00        | 2        | 2023-01-15 |
| 3       | Jane Doe     | jane@example.com | Keyboard   | 50.00        | 1        | 2023-01-16 |

**After Normalization (3NF)**:

Customers Table:
| CustomerID | CustomerName | CustomerEmail    |
|------------|--------------|------------------|
| 1          | John Smith   | john@example.com |
| 2          | Jane Doe     | jane@example.com |

Products Table:
| ProductID | ProductName | ProductPrice |
|-----------|-------------|--------------|
| 101       | Laptop      | 1200.00      |
| 102       | Mouse       | 25.00        |
| 103       | Keyboard    | 50.00        |

Orders Table:
| OrderID | CustomerID | OrderDate   |
|---------|------------|-------------|
| 1       | 1          | 2023-01-15  |
| 2       | 1          | 2023-01-15  |
| 3       | 2          | 2023-01-16  |

OrderItems Table:
| OrderItemID | OrderID | ProductID | Quantity |
|-------------|---------|-----------|----------|
| 1           | 1       | 101       | 1        |
| 2           | 2       | 102       | 2        |
| 3           | 3       | 103       | 1        |

#### Benefits of Normalization:
- Reduces data redundancy
- Improves data integrity
- Makes the database more flexible for queries
- Simplifies data maintenance

#### Potential Drawbacks:
- Can make some queries more complex (requiring joins)
- May impact performance for certain read-heavy operations

#### Why This Matters for SQL:
Normalized database designs often require SQL joins to retrieve related data from multiple tables. Understanding normalization helps you write effective SQL queries for normalized databases.

## 4.2 Relationships Between Data

### One-to-One Relationships

A one-to-one (1:1) relationship exists when each record in Table A is related to exactly one record in Table B, and vice versa.

#### Characteristics:
- Least common type of relationship
- Often used to split a table with many columns
- Can be used to separate rarely used data
- Can represent a subtype or extension of an entity

#### Implementation:
- Primary key in one table serves as both primary key and foreign key in the related table
- Or, both tables have their own primary keys with a unique foreign key in one table

#### Example:
- A Person has one Passport, and a Passport belongs to one Person
- An Employee has one EmployeeDetail record with additional information

**Person Table:**
| PersonID | Name      | BirthDate  |
|----------|-----------|------------|
| 1        | John Doe  | 1980-05-15 |
| 2        | Jane Smith| 1992-11-30 |

**Passport Table:**
| PassportID | PersonID | PassportNumber | ExpiryDate |
|------------|----------|----------------|------------|
| 1          | 1        | AB123456       | 2028-04-22 |
| 2          | 2        | CD789012       | 2027-10-05 |

#### Why This Matters for SQL:
In SQL, you can join these tables using the shared key (PersonID in this example) to retrieve the complete set of information about a person and their passport.

### One-to-Many Relationships

A one-to-many (1:N) relationship exists when each record in Table A can be related to multiple records in Table B, but each record in Table B is related to only one record in Table A.

#### Characteristics:
- Most common type of relationship
- Represents parent-child or master-detail relationships
- Examples: Customer to Orders, Department to Employees

#### Implementation:
- Foreign key in the "many" table references the primary key in the "one" table
- The foreign key is not unique in the "many" table

#### Example:
- A Customer can place many Orders, but each Order is placed by only one Customer

**Customers Table:**
| CustomerID | CustomerName | Email            |
|------------|--------------|------------------|
| 1          | John Smith   | john@example.com |
| 2          | Jane Doe     | jane@example.com |

**Orders Table:**
| OrderID | CustomerID | OrderDate  | TotalAmount |
|---------|------------|------------|-------------|
| 101     | 1          | 2023-01-15 | 1225.00     |
| 102     | 1          | 2023-02-20 | 50.00       |
| 103     | 2          | 2023-01-16 | 75.00       |

#### Why This Matters for SQL:
One-to-many relationships are typically queried using SQL joins. Understanding this relationship helps you write queries that connect parent and child records correctly.

### Many-to-Many Relationships

A many-to-many (M:N) relationship exists when each record in Table A can be related to multiple records in Table B, and each record in Table B can be related to multiple records in Table A.

#### Characteristics:
- Requires a junction (or bridge) table to implement
- The junction table typically contains foreign keys to both related tables
- Often includes additional attributes about the relationship

#### Implementation:
- Create a third table (junction table)
- Include foreign keys to both related tables
- The combination of these foreign keys often serves as the primary key

#### Example:
- Students can enroll in multiple Courses, and each Course can have multiple Students

**Students Table:**
| StudentID | StudentName |
|-----------|-------------|
| 1         | Alice       |
| 2         | Bob         |
| 3         | Charlie     |

**Courses Table:**
| CourseID | CourseName        |
|----------|-------------------|
| 101      | Database Design   |
| 102      | Web Development   |
| 103      | Data Science      |

**Enrollments Table (Junction):**
| EnrollmentID | StudentID | CourseID | EnrollmentDate | Grade |
|--------------|-----------|----------|----------------|-------|
| 1            | 1         | 101      | 2023-01-10     | A     |
| 2            | 1         | 102      | 2023-01-10     | B+    |
| 3            | 2         | 101      | 2023-01-11     | A-    |
| 4            | 3         | 102      | 2023-01-12     | B     |
| 5            | 3         | 103      | 2023-01-12     | A     |

#### Why This Matters for SQL:
Many-to-many relationships require SQL joins through the junction table. Understanding this structure helps you write queries that correctly navigate these relationships.

### Foreign Keys and Referential Integrity

Foreign keys and referential integrity are mechanisms that enforce and maintain relationships between tables.

#### Foreign Keys:
- A column (or combination of columns) in one table that refers to the primary key in another table
- Creates a link between the two tables
- Enforces referential integrity

#### Referential Integrity:
- Ensures that relationships between tables remain consistent
- Prevents orphaned records (records with foreign key values that don't match any primary key)
- Defines what happens when related records are updated or deleted

#### Referential Actions:
- **CASCADE**: Automatically update or delete related records
- **SET NULL**: Set the foreign key to NULL when the referenced record is deleted
- **SET DEFAULT**: Set the foreign key to its default value
- **RESTRICT/NO ACTION**: Prevent the deletion or update if related records exist

#### Example:
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    Email VARCHAR(100)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
    ON DELETE RESTRICT
    ON UPDATE CASCADE
);
```

In this example:
- The `CustomerID` in the `Orders` table is a foreign key referencing the `CustomerID` in the `Customers` table
- If you try to delete a customer who has orders, the operation will be prevented (RESTRICT)
- If you update a customer's ID, the change will automatically be applied to all their orders (CASCADE)

#### Why This Matters for SQL:
SQL includes commands for defining foreign keys and referential integrity constraints. Understanding these concepts helps you design databases that maintain data consistency and write queries that respect these relationships.

## 4.3 Database Schema Design

### Tables and Their Structure

Tables are the fundamental structures for storing data in relational databases.

#### Key Components:

- **Table Name**: Identifies the table, typically a noun representing the entity
- **Columns**: Define the attributes of the entity
  - Column name
  - Data type
  - Constraints (NULL/NOT NULL, DEFAULT, etc.)
- **Primary Key**: Uniquely identifies each row
- **Foreign Keys**: Link to other tables
- **Indexes**: Improve query performance

#### Table Design Best Practices:
- Use clear, descriptive names for tables and columns
- Choose appropriate data types for each column
- Define a primary key for each table
- Normalize tables to reduce redundancy
- Create indexes for frequently queried columns
- Document table purpose and structure

#### Example Table Definition:
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    HireDate DATE NOT NULL,
    DepartmentID INT,
    Salary DECIMAL(10,2),
    IsActive BIT DEFAULT 1,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);
```

#### Why This Matters for SQL:
SQL is used to create, modify, and query tables. Understanding table structure helps you write effective SQL statements for these operations.

### Constraints and Rules

Constraints are rules enforced on the data in tables to maintain accuracy and reliability.

#### Types of Constraints:

- **PRIMARY KEY**: Uniquely identifies each record
- **FOREIGN KEY**: Ensures referential integrity between tables
- **UNIQUE**: Ensures all values in a column are different
- **NOT NULL**: Ensures a column cannot have NULL values
- **CHECK**: Ensures values meet specific conditions
- **DEFAULT**: Provides a default value for a column
- **INDEX**: Not technically a constraint, but improves query performance

#### Examples:

```sql
-- Primary Key constraint
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(100)
);

-- Foreign Key constraint
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- Unique constraint
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Email VARCHAR(100) UNIQUE
);

-- Not Null constraint
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL
);

-- Check constraint
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(100),
    Price DECIMAL(10,2) CHECK (Price > 0)
);

-- Default constraint
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE DEFAULT CURRENT_DATE
);
```

#### Why This Matters for SQL:
SQL includes commands for defining and modifying constraints. Understanding constraints helps you design databases that maintain data integrity and write queries that respect these rules.

### Best Practices for Schema Design

Effective schema design is crucial for database performance, maintainability, and scalability.

#### Naming Conventions:
- Use consistent, descriptive names
- Choose a naming convention (camelCase, snake_case, PascalCase) and stick to it
- Use singular nouns for table names (Customer, not Customers)
- Use prefixes or suffixes for clarity when needed

#### Data Type Selection:
- Choose the most appropriate data type for each column
- Use the smallest data type that can accommodate the expected data
- Consider future growth when selecting data types
- Be consistent with similar data across tables

#### Normalization Guidelines:
- Aim for Third Normal Form (3NF) in most cases
- Denormalize only when there's a clear performance benefit
- Document any denormalization decisions

#### Index Strategy:
- Index primary keys and foreign keys
- Index columns frequently used in WHERE clauses
- Avoid over-indexing (indexes speed up reads but slow down writes)
- Consider composite indexes for multi-column queries

#### Documentation:
- Document the purpose of each table and column
- Create and maintain an ER diagram
- Document any business rules implemented as constraints
- Keep documentation updated as the schema evolves

#### Performance Considerations:
- Avoid storing calculated values that can be derived
- Consider partitioning large tables
- Use appropriate data types for joins
- Be mindful of the impact of constraints on performance

#### Why This Matters for SQL:
Following best practices for schema design makes your SQL queries more efficient and easier to write. A well-designed schema is the foundation for effective SQL development.

## Practical Exercises

1. **ER Diagram Creation**:
   - Draw an ER diagram for a simple library system with the following entities:
     - Books (ISBN, Title, PublicationYear, AuthorID)
     - Authors (AuthorID, Name, Nationality)
     - Members (MemberID, Name, Email)
     - Loans (LoanID, BookID, MemberID, LoanDate, ReturnDate)
   - Identify the relationships between these entities
   - Specify the cardinality of each relationship

2. **Normalization Practice**:
   - Consider the following unnormalized table:
     - Orders (OrderID, CustomerName, CustomerEmail, CustomerPhone, ProductName, ProductPrice, Quantity, OrderDate)
   - Normalize this table to 3NF
   - Create the SQL statements to define the normalized tables

3. **Relationship Implementation**:
   - Write SQL statements to create tables for:
     - A one-to-one relationship between Employees and EmployeeDetails
     - A one-to-many relationship between Departments and Employees
     - A many-to-many relationship between Students and Courses
   - Include appropriate primary keys, foreign keys, and constraints

4. **Schema Design Critique**:
   - Review the following table design and identify potential issues:
     ```sql
     CREATE TABLE customer_order (
       id INT,
       customer_name VARCHAR(100),
       customer_email VARCHAR(100),
       product_id INT,
       product_name VARCHAR(100),
       product_price DECIMAL,
       order_quantity INT,
       order_date DATE
     );
     ```
   - Propose an improved design that addresses the issues you identified

## Summary

This module covered the essential concepts of data modeling and relationships that form the foundation for effective database design:

- **Data Modeling Concepts**: Entity-Relationship diagrams, conceptual/logical/physical models, and normalization
- **Relationships Between Data**: One-to-one, one-to-many, and many-to-many relationships, along with foreign keys and referential integrity
- **Database Schema Design**: Table structure, constraints and rules, and best practices for schema design

Understanding these concepts is crucial for designing effective databases and writing SQL queries that accurately reflect business requirements and relationships between data.

## Next Steps

Now that you understand how data is modeled and related within databases, you're ready to explore the specific data types and structures used to store information. The next module will cover common data types and their appropriate use cases.

## Additional Resources

- [Database Design for Mere Mortals](https://www.amazon.com/Database-Design-Mere-Mortals-Hands/dp/0321884493)
- [Entity-Relationship Diagram Tutorial](https://www.lucidchart.com/pages/er-diagrams)
- [Normalization in Database Design](https://www.guru99.com/database-normalization.html)
- [SQL Constraints Tutorial](https://www.w3schools.com/sql/sql_constraints.asp)
- [Database Schema Design Best Practices](https://www.sisense.com/blog/database-schema-design-best-practices/)
