
# 40+ Essential SQL and SQL Server Questions with Practical Examples

This comprehensive guide covers fundamental SQL concepts with hands-on examples to strengthen your database knowledge.

## Table of Contents

### **Core SQL Operations (1-10)**

1. [DELETE, TRUNCATE, and DROP Operations](#1-delete-truncate-and-drop-operations)
2. [ACID Properties in Database Transactions](#2-acid-properties-in-database-transactions)
3. [Clustered vs Non-Clustered Indexes](#3-clustered-vs-non-clustered-indexes)
4. [SQL Join Types](#4-sql-join-types)
5. [SQL Injection Prevention](#5-sql-injection-prevention)
6. [Stored Procedures](#6-stored-procedures)
7. [Query Optimization Techniques](#7-query-optimization-techniques)
8. [WHERE vs HAVING Clauses](#8-where-vs-having-clauses)
9. [Normalization vs Denormalization](#9-normalization-vs-denormalization)
10. [Common Table Expressions (CTEs)](#10-common-table-expressions-ctes)

### **Advanced SQL Features (11-20)**

11. [Window Functions](#11-window-functions)
12. [Primary Keys](#12-primary-keys)
13. [VARCHAR vs NVARCHAR](#13-varchar-vs-nvarchar)
14. [UNION vs UNION ALL](#14-union-vs-union-all)
15. [Foreign Keys](#15-foreign-keys)
16. [Database Indexing Types](#16-database-indexing-types)
17. [Database Locking](#17-database-locking)
18. [Deadlock Management](#18-deadlock-management)
19. [COALESCE Function](#19-coalesce-function)
20. [Ranking Functions Comparison](#20-ranking-functions-comparison)

### **SQL Server Specific Features (21-30)**

21. [MERGE Statement](#21-merge-statement)
22. [Database Triggers](#22-database-triggers)
23. [SQL Server Profiler](#23-sql-server-profiler)
24. [Views](#24-views)
25. [Table Partitioning](#25-table-partitioning)
26. [SQL Server Replication Types](#26-sql-server-replication-types)
27. [Database Mirroring](#27-database-mirroring)
28. [NOLOCK Hint](#28-nolock-hint)
29. [CHAR vs VARCHAR](#29-char-vs-varchar)
30. [TempDB Database](#30-tempdb-database)

### **Advanced SQL Server Concepts (31-40)**

31. [PIVOT and UNPIVOT Operations](#31-pivot-and-unpivot-operations)
32. [Recursive CTEs](#32-recursive-ctes)
33. [Error Handling with TRY-CATCH](#33-error-handling-with-try-catch)
34. [Dynamic SQL](#34-dynamic-sql)
35. [Table-Valued Functions](#35-table-valued-functions)
36. [Cursors](#36-cursors)
37. [Transaction Isolation Levels](#37-transaction-isolation-levels)
38. [XML Data Handling](#38-xml-data-handling)
39. [Full-Text Search](#39-full-text-search)
40. [Database Backup and Recovery](#40-database-backup-and-recovery)

***

## 1. DELETE, TRUNCATE, and DROP Operations

### DELETE

**Definition:** Removes specific rows based on conditions while preserving table structure and indexes.

```sql
DELETE FROM Employees WHERE EmployeeID = 1;
```

- **Use case:** Selective row removal
- **Rollback:** Possible (logged operation)


### TRUNCATE

**Definition:** Removes all rows while keeping table structure, but faster than DELETE.

```sql
TRUNCATE TABLE Employees;
```

- **Use case:** Quick table cleanup
- **Rollback:** Limited (minimally logged)


### DROP

**Definition:** Completely removes the table and all its data from the database.

```sql
DROP TABLE Employees;
```

- **Use case:** Permanent table removal
- **Rollback:** Not possible after commit

[🔝 Back to Top](#table-of-contents)

***

## 2. ACID Properties in Database Transactions

**Definition:** ACID represents four fundamental properties that guarantee reliable database transactions.

### Atomicity

**Definition:** Ensures all operations in a transaction succeed or all fail together.

```sql
BEGIN TRANSACTION;
UPDATE Account SET Balance = Balance - 100 WHERE AccountID = 1;
UPDATE Account SET Balance = Balance + 100 WHERE AccountID = 2;
COMMIT;
```


### Consistency

**Definition:** Maintains database integrity by enforcing all constraints and rules.

### Isolation

**Definition:** Prevents concurrent transactions from interfering with each other.

### Durability

**Definition:** Guarantees committed transactions persist even after system failures.

[🔝 Back to Top](#table-of-contents)

***

## 3. Clustered vs Non-Clustered Indexes

### Clustered Index

**Definition:** Physically sorts table data based on the indexed column(s).

```sql
CREATE CLUSTERED INDEX IX_Employees_ID ON Employees(EmployeeID);
```

- **Limitation:** One per table
- **Performance:** Faster for range queries


### Non-Clustered Index

**Definition:** Creates a separate structure with pointers to actual data rows.

```sql
CREATE NONCLUSTERED INDEX IX_Employees_Name ON Employees(LastName);
```

- **Limitation:** Multiple allowed per table
- **Performance:** Faster for specific lookups

[🔝 Back to Top](#table-of-contents)

***

## 4. SQL Join Types

**Definition:** Joins combine rows from two or more tables based on related columns.

### INNER JOIN

Returns only matching records from both tables.

```sql
SELECT e.Name, d.Name
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```


### LEFT JOIN

Returns all records from the left table and matching records from the right.

```sql
SELECT e.Name, d.Name
FROM Employees e
LEFT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```


### RIGHT JOIN

Returns all records from the right table and matching records from the left.

```sql
SELECT e.Name, d.Name
FROM Employees e
RIGHT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```


### FULL OUTER JOIN

Returns all records when there's a match in either table.

```sql
SELECT e.Name, d.Name
FROM Employees e
FULL JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```


### CROSS JOIN

Returns the Cartesian product of both tables.

```sql
SELECT e.Name, d.Name
FROM Employees e
CROSS JOIN Departments d;
```

[🔝 Back to Top](#table-of-contents)

***

## 5. SQL Injection Prevention

**Definition:** SQL injection is a security vulnerability where malicious SQL code is inserted into application queries.

### Vulnerable Code

```sql
-- Dangerous: Direct string concatenation
SELECT * FROM Users WHERE Username = '' OR '1'='1' AND Password = '';
```


### Secure Code

```csharp
// Safe: Parameterized queries
SqlCommand cmd = new SqlCommand(
    "SELECT * FROM Users WHERE Username = @username AND Password = @password", 
    connection);
cmd.Parameters.AddWithValue("@username", userInput);
cmd.Parameters.AddWithValue("@password", userInput);
```

[🔝 Back to Top](#table-of-contents)

***

## 6. Stored Procedures

**Definition:** Pre-compiled SQL code stored in the database that can be executed repeatedly.

**Benefits:** Improved security, performance, and code reusability.

```sql
CREATE PROCEDURE GetEmployee
    @EmployeeID INT
AS
BEGIN
    SELECT * FROM Employees WHERE EmployeeID = @EmployeeID;
END;

-- Execution
EXEC GetEmployee @EmployeeID = 1;
```

[🔝 Back to Top](#table-of-contents)

***

## 7. Query Optimization Techniques

**Definition:** Methods to improve SQL query performance and reduce execution time.

### Use Indexes

```sql
CREATE INDEX IX_Employees_DepartmentID ON Employees(DepartmentID);
```


### Avoid SELECT *

```sql
-- Good: Specific columns
SELECT Name, Age FROM Employees WHERE DepartmentID = 1;
```


### Prefer JOINs over Subqueries

```sql
SELECT e.Name, d.Name
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

[🔝 Back to Top](#table-of-contents)

***

## 8. WHERE vs HAVING Clauses

### WHERE

**Definition:** Filters rows **before** grouping occurs.

```sql
SELECT DepartmentID, COUNT(*)
FROM Employees
WHERE Age > 30
GROUP BY DepartmentID;
```


### HAVING

**Definition:** Filters groups **after** grouping and aggregation.

```sql
SELECT DepartmentID, COUNT(*)
FROM Employees
GROUP BY DepartmentID
HAVING COUNT(*) > 5;
```

[🔝 Back to Top](#table-of-contents)

***

## 9. Normalization vs Denormalization

### Normalization

**Definition:** Process of organizing data to reduce redundancy and improve data integrity.

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name NVARCHAR(50),
    DepartmentID INT
);

CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName NVARCHAR(50)
);
```


### Denormalization

**Definition:** Deliberately adding redundancy to improve query performance.

```sql
CREATE TABLE EmployeeDetails (
    EmployeeID INT,
    Name NVARCHAR(50),
    DepartmentName NVARCHAR(50)
);
```

[🔝 Back to Top](#table-of-contents)

***

## 10. Common Table Expressions (CTEs)

**Definition:** Temporary named result sets that exist only for the duration of a single query.

```sql
WITH EmployeeCTE AS (
    SELECT Name, ROW_NUMBER() OVER (ORDER BY Name) AS RowNum
    FROM Employees
)
SELECT * FROM EmployeeCTE WHERE RowNum BETWEEN 1 AND 10;
```

[🔝 Back to Top](#table-of-contents)

***

## 11. Window Functions

**Definition:** Functions that perform calculations across related rows without grouping the result set.

```sql
SELECT Name, Salary,
       RANK() OVER (ORDER BY Salary DESC) AS SalaryRank
FROM Employees;
```

[🔝 Back to Top](#table-of-contents)

***

## 12. Primary Keys

**Definition:** A column or combination of columns that uniquely identifies each row in a table.

### Single Primary Key

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name NVARCHAR(50)
);
```


### Composite Primary Key

```sql
CREATE TABLE Orders (
    OrderID INT,
    ProductID INT,
    PRIMARY KEY (OrderID, ProductID)
);
```

**Note:** A table can have only **one** primary key (single or composite).

[🔝 Back to Top](#table-of-contents)

***

## 13. VARCHAR vs NVARCHAR

### VARCHAR

**Definition:** Variable-length character data type that stores **non-Unicode** data (1 byte per character).

```sql
CREATE TABLE Employees (Name VARCHAR(50));
```


### NVARCHAR

**Definition:** Variable-length character data type that stores **Unicode** data (2 bytes per character) for multilingual support.

```sql
CREATE TABLE Employees (Name NVARCHAR(50));
```

[🔝 Back to Top](#table-of-contents)

***

## 14. UNION vs UNION ALL

### UNION

**Definition:** Combines results from multiple SELECT statements and **removes duplicates**.

```sql
SELECT Name FROM Employees
UNION
SELECT Name FROM Managers;
```


### UNION ALL

**Definition:** Combines results from multiple SELECT statements and **keeps duplicates** (faster performance).

```sql
SELECT Name FROM Employees
UNION ALL
SELECT Name FROM Managers;
```

[🔝 Back to Top](#table-of-contents)

***

## 15. Foreign Keys

**Definition:** A column or combination of columns that creates a link between two tables and enforces referential integrity.

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    EmployeeID INT,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);
```

[🔝 Back to Top](#table-of-contents)

***

## 16. Database Indexing Types

**Definition:** Database structures that improve query performance by creating shortcuts to data.

### Basic Index Creation

```sql
CREATE INDEX IX_Employees_Name ON Employees(Name);
```


### Index Types

- **Clustered:** Physical data sorting
- **Non-Clustered:** Separate pointer structure
- **Unique:** Enforces uniqueness
- **Full-text:** Text search optimization

[🔝 Back to Top](#table-of-contents)

***

## 17. Database Locking

**Definition:** Mechanism that prevents data conflicts during concurrent transactions by restricting access to resources.

```sql
BEGIN TRANSACTION;
UPDATE Employees SET Salary = Salary + 500 WHERE EmployeeID = 1;
-- Lock held on EmployeeID = 1 until commit
COMMIT;
```

[🔝 Back to Top](#table-of-contents)

***

## 18. Deadlock Management

**Definition:** A situation where two or more transactions are waiting indefinitely for each other to release locks.

### Scenario

- Transaction A: Locks Table1, waits for Table2
- Transaction B: Locks Table2, waits for Table1


### Prevention Strategies

- Acquire locks in consistent order
- Use shorter transactions
- Implement timeout settings

[🔝 Back to Top](#table-of-contents)

***

## 19. COALESCE Function

**Definition:** Returns the first non-null value from a list of expressions.

```sql
SELECT Name, COALESCE(PhoneNumber, 'N/A') AS Phone
FROM Employees;
```

[🔝 Back to Top](#table-of-contents)

***

## 20. Ranking Functions Comparison

**Definition:** Window functions that assign ranking values to rows based on specified ordering.

### RANK()

Creates gaps in ranking for ties.

```sql
SELECT Name, Salary, RANK() OVER (ORDER BY Salary DESC) AS SalaryRank
FROM Employees;
```


### DENSE_RANK()

No gaps in ranking for ties.

```sql
SELECT Name, Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS SalaryDenseRank
FROM Employees;
```


### ROW_NUMBER()

Assigns unique sequential numbers.

```sql
SELECT Name, Salary, ROW_NUMBER() OVER (ORDER BY Salary DESC) AS RowNum
FROM Employees;
```

[🔝 Back to Top](#table-of-contents)

***

## 21. MERGE Statement

**Definition:** Performs INSERT, UPDATE, or DELETE operations based on matching conditions in a single statement.

```sql
MERGE INTO Employees AS Target
USING NewEmployees AS Source
ON Target.EmployeeID = Source.EmployeeID
WHEN MATCHED THEN
    UPDATE SET Target.Name = Source.Name
WHEN NOT MATCHED THEN
    INSERT (EmployeeID, Name) VALUES (Source.EmployeeID, Source.Name);
```

[🔝 Back to Top](#table-of-contents)

***

## 22. Database Triggers

**Definition:** Special stored procedures that automatically execute in response to specific database events.

```sql
CREATE TRIGGER trgAfterInsert
ON Employees
AFTER INSERT
AS
BEGIN
    PRINT 'New employee record inserted';
END;
```

[🔝 Back to Top](#table-of-contents)

***

## 23. SQL Server Profiler

**Definition:** A monitoring tool that captures and analyzes SQL Server database activity for performance tuning and debugging.

**Purpose:** Monitor SQL Server performance, debug queries, and analyze slow operations by capturing detailed database events.

[🔝 Back to Top](#table-of-contents)

***

## 24. Views

**Definition:** Virtual tables created from query results that don't store data physically but present data from underlying tables.

```sql
CREATE VIEW EmployeeView AS
SELECT Name, DepartmentName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID;

-- Usage
SELECT * FROM EmployeeView;
```

[🔝 Back to Top](#table-of-contents)

***

## 25. Table Partitioning

**Definition:** Technique that splits large tables across multiple storage groups to improve performance and manageability.

```sql
CREATE PARTITION FUNCTION MyRangePF1 (int)
AS RANGE LEFT FOR VALUES (1, 100, 1000);

CREATE PARTITION SCHEME MyRangePS1
AS PARTITION MyRangePF1
TO (Group1, Group2, Group3, Group4);
```

[🔝 Back to Top](#table-of-contents)

***

## 26. SQL Server Replication Types

**Definition:** Technologies for copying and distributing data from one database to another and synchronizing between databases.

### Snapshot Replication

Periodic full data synchronization.

### Transactional Replication

Real-time continuous data synchronization.

### Merge Replication

Bidirectional data synchronization with conflict resolution.

[🔝 Back to Top](#table-of-contents)

***

## 27. Database Mirroring

**Definition:** High availability solution that maintains two copies of a database on separate server instances.

Provides high availability through **principal** and **mirror** database setup with optional **witness** server for automatic failover.

[🔝 Back to Top](#table-of-contents)

***

## 28. NOLOCK Hint

**Definition:** Query hint that allows reading data without acquiring shared locks, potentially resulting in dirty reads.

```sql
SELECT * FROM Employees WITH (NOLOCK);
```

[🔝 Back to Top](#table-of-contents)

***

## 29. CHAR vs VARCHAR

### CHAR

**Definition:** Fixed-length character data type that always uses the specified number of bytes.

```sql
CREATE TABLE TestChar (Code CHAR(10)); -- Always uses 10 bytes
```


### VARCHAR

**Definition:** Variable-length character data type that uses only the bytes needed for actual data.

```sql
CREATE TABLE TestVarChar (Code VARCHAR(10)); -- Uses actual string length
```

[🔝 Back to Top](#table-of-contents)

***

## 30. TempDB Database

**Definition:** System database used for temporary storage of objects and intermediate results in SQL Server.

### Temporary Tables

```sql
CREATE TABLE #TempTable (ID INT); -- Session-scoped
```


### Table Variables

```sql
DECLARE @TempTable TABLE (ID INT); -- Batch-scoped
```


### Additional Uses

- Sort operations
- Hash joins
- Cursors
- Snapshot isolation versioning

[🔝 Back to Top](#table-of-contents)

***

## 31. PIVOT and UNPIVOT Operations

**Definition:** PIVOT rotates rows to columns, while UNPIVOT rotates columns to rows.

### PIVOT Example

```sql
SELECT *
FROM (
    SELECT Department, Salary, Name
    FROM Employees
) AS SourceTable
PIVOT (
    AVG(Salary)
    FOR Department IN ([IT], [HR], [Finance])
) AS PivotTable;
```


### UNPIVOT Example

```sql
SELECT Department, Salary
FROM (
    SELECT IT, HR, Finance
    FROM DepartmentSalaries
) AS SourceTable
UNPIVOT (
    Salary FOR Department IN (IT, HR, Finance)
) AS UnpivotTable;
```

[🔝 Back to Top](#table-of-contents)

***

## 32. Recursive CTEs

**Definition:** Common Table Expressions that reference themselves to handle hierarchical data.

```sql
WITH EmployeeHierarchy AS (
    -- Anchor member
    SELECT EmployeeID, Name, ManagerID, 1 AS Level
    FROM Employees
    WHERE ManagerID IS NULL
    
    UNION ALL
    
    -- Recursive member
    SELECT e.EmployeeID, e.Name, e.ManagerID, eh.Level + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
)
SELECT * FROM EmployeeHierarchy;
```

[🔝 Back to Top](#table-of-contents)

***

## 33. Error Handling with TRY-CATCH

**Definition:** Structured error handling mechanism in SQL Server for graceful error management.

```sql
BEGIN TRY
    BEGIN TRANSACTION;
    
    INSERT INTO Employees (Name, DepartmentID) 
    VALUES ('John Doe', 999); -- This might fail
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;
    
    SELECT 
        ERROR_NUMBER() AS ErrorNumber,
        ERROR_MESSAGE() AS ErrorMessage;
END CATCH;
```

[🔝 Back to Top](#table-of-contents)

***

## 34. Dynamic SQL

**Definition:** SQL statements constructed and executed at runtime using string concatenation or parameters.

```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(50) = 'Employees';
DECLARE @ColumnName NVARCHAR(50) = 'Name';

SET @SQL = 'SELECT ' + @ColumnName + ' FROM ' + @TableName;
EXEC sp_executesql @SQL;

-- Safer approach with parameters
SET @SQL = 'SELECT Name FROM Employees WHERE DepartmentID = @DeptID';
EXEC sp_executesql @SQL, N'@DeptID INT', @DeptID = 1;
```

[🔝 Back to Top](#table-of-contents)

***

## 35. Table-Valued Functions

**Definition:** User-defined functions that return a table data type.

### Inline Table-Valued Function

```sql
CREATE FUNCTION GetEmployeesByDepartment(@DeptID INT)
RETURNS TABLE
AS
RETURN (
    SELECT EmployeeID, Name, Salary
    FROM Employees
    WHERE DepartmentID = @DeptID
);

-- Usage
SELECT * FROM GetEmployeesByDepartment(1);
```


### Multi-Statement Table-Valued Function

```sql
CREATE FUNCTION GetEmployeeStats(@DeptID INT)
RETURNS @Result TABLE (
    TotalEmployees INT,
    AvgSalary DECIMAL(10,2),
    MaxSalary DECIMAL(10,2)
)
AS
BEGIN
    INSERT INTO @Result
    SELECT 
        COUNT(*),
        AVG(Salary),
        MAX(Salary)
    FROM Employees
    WHERE DepartmentID = @DeptID;
    
    RETURN;
END;
```

[🔝 Back to Top](#table-of-contents)

***

## 36. Cursors

**Definition:** Database objects that allow row-by-row processing of query results.

```sql
DECLARE @EmployeeID INT, @Name NVARCHAR(50);

DECLARE employee_cursor CURSOR FOR
SELECT EmployeeID, Name FROM Employees;

OPEN employee_cursor;

FETCH NEXT FROM employee_cursor INTO @EmployeeID, @Name;

WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT 'Employee: ' + @Name;
    FETCH NEXT FROM employee_cursor INTO @EmployeeID, @Name;
END;

CLOSE employee_cursor;
DEALLOCATE employee_cursor;
```

[🔝 Back to Top](#table-of-contents)

***

## 37. Transaction Isolation Levels

**Definition:** Settings that determine how transactions interact with each other and handle concurrency.

```sql
-- Read Uncommitted (allows dirty reads)
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Read Committed (default, prevents dirty reads)
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Repeatable Read (prevents dirty and non-repeatable reads)
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Serializable (highest isolation, prevents phantom reads)
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Snapshot (uses row versioning)
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;
```

[🔝 Back to Top](#table-of-contents)

***

## 38. XML Data Handling

**Definition:** SQL Server's built-in support for storing, querying, and manipulating XML data.

```sql
-- Create table with XML column
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductData XML
);

-- Insert XML data
INSERT INTO Products VALUES (1, 
'<Product>
    <Name>Laptop</Name>
    <Price>1000</Price>
    <Category>Electronics</Category>
</Product>');

-- Query XML data
SELECT 
    ProductData.value('(/Product/Name)[1]', 'NVARCHAR(50)') AS ProductName,
    ProductData.value('(/Product/Price)[1]', 'DECIMAL(10,2)') AS Price
FROM Products;
```

[🔝 Back to Top](#table-of-contents)

***

## 39. Full-Text Search

**Definition:** Advanced text search capabilities for finding words and phrases in character-based data.

```sql
-- Create full-text catalog
CREATE FULLTEXT CATALOG ProductCatalog;

-- Create full-text index
CREATE FULLTEXT INDEX ON Products(ProductName)
KEY INDEX PK_Products
ON ProductCatalog;

-- Search using CONTAINS
SELECT * FROM Products
WHERE CONTAINS(ProductName, 'laptop OR computer');

-- Search using FREETEXT
SELECT * FROM Products
WHERE FREETEXT(ProductName, 'gaming laptop');
```

[🔝 Back to Top](#table-of-contents)

***

## 40. Database Backup and Recovery

**Definition:** Essential operations for protecting data and ensuring business continuity.

### Full Backup

```sql
BACKUP DATABASE MyDatabase
TO DISK = 'C:\Backups\MyDatabase_Full.bak'
WITH FORMAT, COMPRESSION;
```


### Differential Backup

```sql
BACKUP DATABASE MyDatabase
TO DISK = 'C:\Backups\MyDatabase_Diff.bak'
WITH DIFFERENTIAL, COMPRESSION;
```


### Transaction Log Backup

```sql
BACKUP LOG MyDatabase
TO DISK = 'C:\Backups\MyDatabase_Log.trn';
```


### Restore Operations

```sql
-- Restore full backup
RESTORE DATABASE MyDatabase
FROM DISK = 'C:\Backups\MyDatabase_Full.bak'
WITH REPLACE, NORECOVERY;

-- Restore differential backup
RESTORE DATABASE MyDatabase
FROM DISK = 'C:\Backups\MyDatabase_Diff.bak'
WITH NORECOVERY;

-- Restore log backup
RESTORE LOG MyDatabase
FROM DISK = 'C:\Backups\MyDatabase_Log.trn'
WITH RECOVERY;
```

[🔝 Back to Top](#table-of-contents)

***

