**SQL/DBMS Theory**

**1. What is JOIN in SQL?**
A JOIN clause is used to combine rows from two or more tables based on a related column. It helps retrieve data stored across multiple tables.

**2. Explain joins.**

- **INNER JOIN**: Returns only matching rows from both tables.
- **LEFT JOIN (LEFT OUTER JOIN)**: Returns all rows from the left table and matching rows from the right table.
- **RIGHT JOIN (RIGHT OUTER JOIN)**: Returns all rows from the right table and matching rows from the left table.
- **FULL JOIN (FULL OUTER JOIN)**: Returns all rows from both tables, with NULLs where no match exists.
- **CROSS JOIN**: Returns a Cartesian product of both tables.

**3. What is the HAVING clause?**
The HAVING clause filters grouped records after the `GROUP BY` statement, allowing conditions to be applied to aggregate functions.

**4. What is normalization? Explain with examples.**
Normalization is the process of structuring a relational database to reduce redundancy and improve data integrity. It involves multiple normal forms:

- **1NF**: Eliminates duplicate columns.
- **2NF**: Ensures all non-key attributes are dependent on the primary key.
- **3NF**: Removes transitive dependencies.

**5. What is the difference between RDBMS and DBMS?**

- **DBMS**: Manages data without enforcing relationships (e.g., file-based systems).
- **RDBMS**: Uses tables with relationships (e.g., MySQL, PostgreSQL).

**6. Explain about indexes in a database.**
Indexes improve query performance by enabling faster data retrieval. Types:

- **Clustered Index**: Sorts and stores data rows in the table based on the key.
- **Non-Clustered Index**: Creates a separate structure for fast lookup.

**7. What is DDL, DML with examples?**

- **DDL (Data Definition Language)**: Defines database structure (e.g., `CREATE`, `ALTER`, `DROP`).
- **DML (Data Manipulation Language)**: Manipulates data (e.g., `INSERT`, `UPDATE`, `DELETE`).

**8. What do you know about SDLC?**
SDLC (Software Development Life Cycle) outlines phases in software development: Planning, Design, Development, Testing, Deployment, and Maintenance.

**9. What in SQL excites you?**
The ability to manipulate, retrieve, and analyze large amounts of structured data efficiently using queries, joins, and indexing strategies.

**10. What are primary and foreign keys? Why do we need them?**

- **Primary Key**: A unique identifier for a record.
- **Foreign Key**: A reference to a primary key in another table, ensuring referential integrity.

**11. What is the difference between DML and DDL?**

- **DML**: Modifies data (`INSERT`, `UPDATE`, `DELETE`).
- **DDL**: Modifies database schema (`CREATE`, `ALTER`, `DROP`).

**12. Have you heard about EXISTS and NOT EXISTS?**

- **EXISTS**: Returns true if a subquery returns any rows.
- **NOT EXISTS**: Returns true if a subquery returns no rows.

---

**SQL Queries**

**1. Find the count of employees who joined in April 2024.**

```sql
SELECT COUNT(*) 
FROM employees 
WHERE MONTH(joining_date) = 4 AND YEAR(joining_date) = 2024;
```

**2. How do you connect to a database using Python?**

```python
import mysql.connector
conn = mysql.connector.connect(host='localhost', user='root', password='password', database='company')
cursor = conn.cursor()
```

**3. Get the maximum salary of each department.**

```sql
SELECT department_id, MAX(salary) 
FROM employees
GROUP BY department_id;
```

**4. Self-join to calculate time taken to finish a job.**

```sql
SELECT a.jobname, (b.jobtimestamp - a.jobtimestamp) AS time_taken
FROM jobs a
JOIN jobs b ON a.jobname = b.jobname AND a.jobstatus = 'Start' AND b.jobstatus = 'End';
```

**5. Check for duplicate values in a table.**

```sql
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name HAVING COUNT(*) > 1;
```

**6. Retrieve the second-highest salary without LIMIT or TOP.**

```sql
SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees);
```

**7. Return records in Table A but not in Table B using NOT EXISTS.**

```sql
SELECT * FROM TableA WHERE NOT EXISTS (SELECT 1 FROM TableB WHERE TableA.id = TableB.id);
```

**8. Combine values from two tables.**

```sql
SELECT value FROM TableA UNION ALL SELECT value FROM TableB;
```

**9. Return unique values from two tables.**

```sql
SELECT value FROM TableA UNION SELECT value FROM TableB;
```

**10. Display only duplicate entries in a table.**

```sql
SELECT Aadhar, first_name, last_name, phone, email, COUNT(*)
FROM employees
GROUP BY Aadhar, first_name, last_name, phone, email
HAVING COUNT(*) > 1;
```

**11. Retrieve details of an employee named XYZ who works in more than one department.**

```sql
SELECT * FROM employees WHERE name = 'XYZ' GROUP BY department_id HAVING COUNT(*) > 1;
```

**12. Execute the above query for all employees.**

```sql
SELECT employee_id, name FROM employees GROUP BY employee_id, name HAVING COUNT(department_id) > 1;
```

**13. Compensation total of the top 5 employees.**

```sql
SELECT name, salary FROM employees ORDER BY salary DESC LIMIT 5;
```

