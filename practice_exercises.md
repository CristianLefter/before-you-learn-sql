# Practice Exercises for "Before you learn SQL" Course

## Module 1: Basic Computer Skills

### Exercise 1: File System Navigation
1. Create a folder structure for a fictional company with departments: HR, Finance, Marketing, and IT
2. Create 3 text files in each department folder
3. Move a file from HR to Finance
4. Copy a file from Marketing to IT
5. Delete a file from one of the folders
6. Create a zip archive of the entire folder structure

### Exercise 2: Keyboard Shortcuts
1. Open a text editor and create a new document
2. Type a paragraph about why you want to learn SQL
3. Use keyboard shortcuts to:
   - Select all text
   - Copy the text
   - Create a new document
   - Paste the text
   - Save the document
   - Close the document
4. List all the shortcuts you used

### Exercise 3: Basic Troubleshooting
1. Intentionally disconnect your internet connection
2. Document the steps you would take to diagnose and fix the issue
3. Reconnect and verify the connection is working
4. Create a simple troubleshooting flowchart for common computer issues

## Module 2: Understanding Data Fundamentals

### Exercise 1: Data Types Identification
1. Identify the appropriate data type for each of the following:
   - Person's name
   - Date of birth
   - Phone number
   - Email address
   - Credit score
   - Product description
   - Yes/No question response
   - Geographic coordinates
   - Transaction amount
   - User password

### Exercise 2: Data Organization
1. Create a spreadsheet with the following columns:
   - ProductID
   - ProductName
   - Category
   - Price
   - InStock
2. Add at least 10 products with appropriate data
3. Sort the data by price (highest to lowest)
4. Filter to show only products that are in stock
5. Calculate the average price by category

### Exercise 3: Data Relationships
1. Draw a diagram showing relationships between the following entities:
   - Students
   - Courses
   - Instructors
   - Departments
2. Identify the type of relationship between each pair (one-to-one, one-to-many, many-to-many)
3. List what attributes (data fields) each entity might have

## Module 3: Database Fundamentals

### Exercise 1: Database Concept Mapping
1. Create a concept map that illustrates the relationships between:
   - Databases
   - Tables
   - Rows
   - Columns
   - Primary Keys
   - Foreign Keys
   - Indexes
   - Queries

### Exercise 2: Database Type Comparison
1. Research and create a comparison table of different database types:
   - Relational databases
   - NoSQL databases
   - Object-oriented databases
   - Graph databases
2. For each type, list:
   - Key characteristics
   - Typical use cases
   - Examples of database products
   - Advantages and disadvantages

### Exercise 3: Database Design Scenario
1. Design a simple database for a library system that needs to track:
   - Books (title, author, ISBN, publication date, genre)
   - Members (name, address, phone, email, join date)
   - Loans (which book, which member, loan date, due date, return date)
2. Create an entity-relationship diagram
3. List the tables and their columns
4. Identify primary and foreign keys

## Module 4: Data Models and Relationships

### Exercise 1: Normalization Practice
1. Given the following denormalized table:
   | OrderID | CustomerName | CustomerEmail | ProductName | ProductPrice | Quantity | OrderDate |
   |---------|--------------|---------------|-------------|--------------|----------|-----------|
   | 1001    | John Smith   | john@example.com | Laptop     | 1200.00      | 1        | 2023-01-15 |
   | 1002    | Mary Jones   | mary@example.com | Printer    | 299.99       | 1        | 2023-01-16 |
   | 1003    | John Smith   | john@example.com | Mouse      | 25.50        | 2        | 2023-01-20 |

2. Normalize this table to 3NF, creating appropriate tables and relationships

### Exercise 2: Relationship Modeling
1. For each of the following scenarios, identify the appropriate relationship type and draw a simple diagram:
   - A person can have multiple email addresses
   - A student can enroll in multiple courses, and each course can have multiple students
   - Each department has exactly one manager
   - A book can have multiple authors, and an author can write multiple books
   - Each country has one capital city

### Exercise 3: Schema Design Challenge
1. Design a database schema for a social media platform that needs to track:
   - Users (profile information)
   - Posts (content, timestamp)
   - Comments (on posts)
   - Friendships (between users)
   - Likes (on posts and comments)
2. Create an entity-relationship diagram
3. Identify potential performance issues and how you might address them

## Module 5: Data Types and Structures

### Exercise 1: Data Type Selection
1. For each of the following data items, select the most appropriate SQL data type and explain your reasoning:
   - Person's age
   - Email address
   - Phone number
   - Product price
   - Order status (New, Processing, Shipped, Delivered)
   - User's profile picture
   - Blog post content
   - Geographic coordinates
   - Transaction timestamp
   - Credit card number

### Exercise 2: Storage Calculation
1. Design a customer table with the following columns:
   - CustomerID (integer)
   - FirstName (up to 50 characters)
   - LastName (up to 50 characters)
   - Email (up to 100 characters)
   - Phone (up to 20 characters)
   - Address (up to 200 characters)
   - RegistrationDate (date and time)
   - LoyaltyPoints (integer)
2. Estimate the storage required per row
3. Calculate the total storage needed for 1 million customers
4. Suggest optimizations to reduce storage requirements

### Exercise 3: Special Data Structures
1. For each of the following scenarios, determine the most appropriate data structure or type:
   - Storing a customer's purchase history
   - Recording GPS coordinates for delivery tracking
   - Maintaining a list of product categories for an item
   - Storing hierarchical data like an organizational chart
   - Keeping track of available time slots for appointments

## Module 6: Logical Thinking and Problem-Solving

### Exercise 1: Logical Operators
1. Translate the following statements into logical expressions using AND, OR, and NOT:
   - Customers who live in New York or California and have spent over $1000
   - Products that are either electronics with a price under $500 or books with a price under $50
   - Orders placed in the last 30 days that have not been shipped
   - Employees who are either in the Sales department with less than 2 years of experience or in the Marketing department with more than 5 years of experience

### Exercise 2: Algorithmic Thinking
1. Write step-by-step instructions (pseudocode) for the following tasks:
   - Finding the highest-spending customer in a database
   - Identifying products that are frequently purchased together
   - Calculating the average processing time for orders
   - Determining which sales representatives exceeded their quarterly targets

### Exercise 3: Set Operations
1. Given two sets:
   - Set A: Customers who made a purchase in January
   - Set B: Customers who made a purchase in February
   
   Describe and illustrate the results of the following operations:
   - A UNION B
   - A INTERSECT B
   - A EXCEPT B
   - B EXCEPT A

### Exercise 4: Pattern Recognition
1. Identify patterns in the following data and describe what they might indicate:
   - Monthly sales figures: [12500, 13200, 12800, 18500, 19200, 18900, 25600, 26100, 25800, 14200, 15100, 29800]
   - Customer acquisition costs: [45, 42, 40, 38, 35, 33, 34, 36, 39, 43, 48, 52]
   - Website traffic by hour: [120, 80, 40, 20, 10, 15, 60, 210, 350, 410, 380, 320, 390, 410, 380, 350, 320, 380, 420, 380, 290, 210, 180, 150]

## Module 7: Basic Programming Concepts

### Exercise 1: Variable Usage
1. Write pseudocode that:
   - Declares variables for customer name, order count, and total amount
   - Assigns values to these variables
   - Calculates the average order value
   - Outputs a formatted message using these variables

### Exercise 2: Control Structures
1. Write pseudocode using if-else statements to categorize products based on their price:
   - Under $10: "Budget"
   - $10-$50: "Standard"
   - $51-$100: "Premium"
   - Over $100: "Luxury"

2. Write pseudocode using a loop to calculate the total value of all orders for a customer

### Exercise 3: Function Development
1. Design a function that calculates a loyalty discount based on:
   - Customer's total spending (higher spending = bigger discount)
   - Number of orders in the past year
   - Membership level (Bronze, Silver, Gold)
2. Write pseudocode for this function
3. Test the function with various customer scenarios

### Exercise 4: Error Handling
1. Write pseudocode for a procedure that transfers funds between two accounts, including error handling for:
   - Insufficient funds
   - Invalid account numbers
   - Connection issues
   - Constraint violations

## Module 8: SQL Development Environment

### Exercise 1: Environment Setup
1. Install a database system of your choice (SQLite, MySQL, PostgreSQL, or SQL Server Express)
2. Install an appropriate SQL client
3. Create a connection to your database
4. Create a new database called "Practice"
5. Document the steps you took and any challenges encountered

### Exercise 2: Client Exploration
1. Using your installed SQL client:
   - Create a new table with at least 5 columns of different data types
   - Insert at least 10 rows of sample data
   - Write a query to retrieve specific columns with a WHERE condition
   - Format the query for readability
   - Save the query for future use
2. Explore the features of your client and list 5 features you find most useful

### Exercise 3: Version Control Practice
1. Create a Git repository for your SQL scripts
2. Add your database creation script and commit it
3. Make changes to the script (add a new table or modify an existing one)
4. Commit the changes with a descriptive message
5. Create a branch for a new feature
6. Make changes in the branch and merge them back to the main branch

## Module 9: Introduction to SQL Concepts

### Exercise 1: SQL Categories
1. Categorize the following SQL statements as DDL, DML, DCL, or TCL:
   - `CREATE TABLE Employees (...)`
   - `SELECT * FROM Customers`
   - `GRANT SELECT ON Orders TO UserJohn`
   - `BEGIN TRANSACTION`
   - `INSERT INTO Products VALUES (...)`
   - `ALTER TABLE Customers ADD Email VARCHAR(100)`
   - `COMMIT`
   - `REVOKE UPDATE ON Inventory FROM UserSarah`
2. Write one example of each category not listed above

### Exercise 2: SQL Dialect Comparison
1. Research and list 5 differences between Transact-SQL and standard SQL
2. Find examples of how the same operation is performed in different SQL dialects
3. Create a reference table showing syntax differences for common operations

### Exercise 3: Real-World Application
1. Choose an industry or application type (e.g., e-commerce, healthcare, banking)
2. Identify at least 5 ways SQL would be used in that context
3. For each use case, describe:
   - What data would be involved
   - What SQL operations would be performed
   - Who would use the results
   - Why the operation is important

## Module 10: Preparing for Your SQL Journey

### Exercise 1: Learning Plan Development
1. Assess your current SQL knowledge and skills
2. Define 3-5 SMART goals for your SQL learning
3. Create a 30-day learning schedule with specific topics and activities
4. Identify resources you'll use for each topic
5. Design a method to track and measure your progress

### Exercise 2: Resource Evaluation
1. Find and evaluate three different SQL learning resources (book, online course, tutorial)
2. For each resource, identify:
   - Target audience and prerequisite knowledge
   - Topics covered and depth of coverage
   - Learning approach (theory vs. practice balance)
   - Quality of examples and exercises
   - User reviews or recommendations
3. Determine which resource best matches your learning style and needs

### Exercise 3: Community Engagement
1. Join at least one SQL-focused online community
2. Find and read through 5 interesting SQL-related discussions
3. Identify a question you can answer based on your current knowledge
4. Post a thoughtful response to help another learner
5. Ask a question about something you're currently learning
