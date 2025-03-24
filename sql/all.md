**SQL Queries and Syntax Guide**

### Data Definition Language (DDL)

**1. CREATE TABLE**  
*Creates a new table in the database.*  
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100),
    Age INT,
    Department VARCHAR(50)
    FOREIGN KEY fk_ProjectWorker_Project (ProjectId)
      REFERENCES Project(ProjectId),

);
```

**2. ALTER TABLE**  
*Modifies an existing table by adding, deleting, or modifying columns.*  
```sql
ALTER TABLE Employees ADD COLUMN Salary DECIMAL(10,2);
```

**3. DROP TABLE**  
*Deletes a table and its data permanently.*  
```sql
DROP TABLE Employees;
```

**4. TRUNCATE TABLE**  
*Removes all records from a table without deleting its structure.*  
```sql
TRUNCATE TABLE Employees;
```

### Data Manipulation Language (DML)

**5. INSERT INTO**  
*Inserts new records into a table.*  
```sql
INSERT INTO Employees (EmployeeID, Name, Age, Department, Salary)
VALUES (1, 'John Doe', 30, 'IT', 60000);
```

**6. UPDATE**  
*Modifies existing records in a table.*  
```sql
UPDATE Employees SET Salary = 65000 WHERE EmployeeID = 1;
```

**7. DELETE FROM**  
*Removes records from a table.*  
```sql
DELETE FROM Employees WHERE EmployeeID = 1;
```

### Data Query Language (DQL)

**8. SELECT**  
*Retrieves data from a table.*  
```sql
SELECT Name, Department FROM Employees WHERE Age > 25;
```

**9. GROUP BY & HAVING**  
*Groups data and applies conditions on aggregated results.*  
```sql
SELECT Department, COUNT(*) AS EmployeeCount
FROM Employees
GROUP BY Department
HAVING COUNT(*) > 5;
```

### Joins

**10. INNER JOIN**  
*Returns only matching records from both tables.*  
```sql
SELECT e.Name, d.DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.Department = d.DepartmentID;
```

**11. LEFT JOIN**  
*Returns all records from the left table and matching records from the right table.*  
```sql
SELECT e.Name, d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.Department = d.DepartmentID;
```

**12. RIGHT JOIN**  
*Returns all records from the right table and matching records from the left table.*  
```sql
SELECT e.Name, d.DepartmentName
FROM Employees e
RIGHT JOIN Departments d ON e.Department = d.DepartmentID;
```

**13. FULL OUTER JOIN**  
*Returns all records when there is a match in either table.*  
```sql
SELECT e.Name, d.DepartmentName
FROM Employees e
FULL OUTER JOIN Departments d ON e.Department = d.DepartmentID;
```

### Set Operators

**14. UNION**  
*Combines results of two queries without duplicates.*  
```sql
SELECT Name FROM Employees
UNION
SELECT Name FROM Managers;
```

**15. UNION ALL**  
*Combines results of two queries, including duplicates.*  
```sql
SELECT Name FROM Employees
UNION ALL
SELECT Name FROM Managers;
```

### Sorting & Filtering

**16. ORDER BY**  
*Sorts the result set in ascending or descending order.*  
```sql
SELECT Name, Age FROM Employees ORDER BY Age DESC;
```

### Aggregate Functions

**17. COUNT**  
*Returns the number of rows.*  
```sql
SELECT COUNT(*) FROM Employees;
```

**18. MIN & MAX**  
*Finds the smallest and largest value in a column.*  
```sql
SELECT MIN(Salary) AS LowestSalary, MAX(Salary) AS HighestSalary FROM Employees;
```

**19. AVG & SUM**  
*Calculates the average and sum of a column.*  
```sql
SELECT AVG(Salary) AS AvgSalary, SUM(Salary) AS TotalSalary FROM Employees;
```

### Ranking Functions

**20. RANK()**  
*Assigns a unique rank to rows within a partition.*  
```sql
SELECT Name, Salary, RANK() OVER (ORDER BY Salary DESC) AS Rank
FROM Employees;
```

**21. DENSE_RANK()**  
*Similar to RANK() but without gaps in ranking.*  
```sql
SELECT Name, Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS DenseRank
FROM Employees;
```

**22. ROW_NUMBER()**  
*Assigns a unique sequential number to each row.*  
```sql
SELECT Name, Salary, ROW_NUMBER() OVER (ORDER BY Salary DESC) AS RowNumber
FROM Employees;
```

### Indexing

**23. CREATE INDEX**  
*Creates an index to improve query performance.*  
```sql
CREATE INDEX idx_employee_name ON Employees(Name);
```

**24. CREATE UNIQUE INDEX**  
*Ensures all values in the indexed column are unique.*  
```sql
CREATE UNIQUE INDEX idx_unique_email ON Employees(Email);
```

### Transactions

**25. BEGIN, COMMIT, ROLLBACK**  
*Manages transactions.*  
```sql
BEGIN;
UPDATE Employees SET Salary = 70000 WHERE EmployeeID = 1;
COMMIT;
```

```sql
BEGIN;
UPDATE Employees SET Salary = 70000 WHERE EmployeeID = 1;
ROLLBACK;
```

This guide covers essential SQL queries and their usage with examples.

