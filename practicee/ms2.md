**2) Project Explanation**

My project was an event management platform where users could browse, book, and attend events organized by pubs and artists. The backend was built using NestJS and PostgreSQL, with TypeORM as the ORM. My role was as a backend developer responsible for creating APIs, managing database schemas, and integrating Firebase authentication.

**Challenges:** One major challenge was handling complex entity relationships and ensuring data consistency when deleting a pub or user. I overcame this by implementing proper cascading deletes and custom service logic.

---

**3) Linux Questions**

- **Commands I am familiar with:** ls, cd, mkdir, rm, grep, find, chmod, chown, top, ps, tail, head, cat, awk, sed, crontab.

- **Command to count repeated word in a file:**
  ```bash
  grep -o '\bword\b' filename.txt | wc -l
  ```

- **Command to print count of lines containing a specific word:**
  ```bash
  grep -c "word" filename.txt
  ```

- **What is grep?**
  `grep` is a command-line utility in Unix/Linux used to search for specific patterns in files.

- **Command to print first 10 lines of a file:**
  ```bash
  head -n 10 filename.txt
  ```

- **Command to print top 5 processes consuming most CPU:**
  ```bash
  ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -n 6
  ```

---

**4) SQL Questions**

- **Difference between DELETE, DROP, and TRUNCATE:**
  - DELETE: Deletes specific rows; can be rolled back.
  - DROP: Removes the entire table; cannot be rolled back.
  - TRUNCATE: Removes all rows; faster than DELETE; cannot be rolled back.

- **What are joins and types:**
  Joins combine rows from two or more tables based on a related column. Types include:
  - INNER JOIN
  - LEFT JOIN
  - RIGHT JOIN
  - FULL OUTER JOIN

- **SQL to print first 5 rows:**
  ```sql
  SELECT * FROM table_name LIMIT 5;
  ```

- **Query for employees aged between 30 and 40:**
  ```sql
  SELECT name FROM employees WHERE age BETWEEN 30 AND 40;
  ```

- **Query for branch name and student count in descending order:**
  ```sql
  SELECT branch_name, COUNT(*) FROM students GROUP BY branch_name ORDER BY COUNT(*) DESC;
  ```

---

**5) Python Program to Reverse a String**

```python
def reverse_string(s):
    return s[::-1]

# Example
print(reverse_string("hello"))  # Output: olleh
```

**Approach:** Use Python slicing to reverse the string in one line.

---

**6) About Morgan Stanley and Competitors**

Morgan Stanley is a leading global financial services firm providing investment banking, securities, wealth management, and investment management services. Competitors include:
- Goldman Sachs
- JPMorgan Chase
- Bank of America Merrill Lynch
- Citigroup

---

**7) Opinion on Taking Help from Others**

Taking help is essential for growth. It promotes collaboration, knowledge sharing, and reduces time spent solving a problem. It’s important to balance independence with teamwork.

---

**SQL**
- **Types of Joins:** Inner, Left, Right, Full Outer
- **Differences:**
  - Inner: Only matching rows
  - Left: All from left + matching from right
  - Right: All from right + matching from left
  - Full: All rows from both tables
- **Difference between TRUNCATE and DELETE:** TRUNCATE is faster, cannot be rolled back. DELETE is slower, can be rolled back.
- **NULL in a table:** Represents a missing or undefined value.

**Linux**
- **Pattern Searching:** grep, awk, sed
- **grep types:** -i (ignore case), -v (invert match), -c (count), -n (line number)
- **sed:** Stream editor for transforming text
- **awk:** Pattern scanning and processing language
- **Shell scripting:** Writing scripts using bash or sh to automate tasks

**ITSM**
- **Scenario-based:** Focus on handling incidents, change management, and service requests using ITIL practices.

**Finance Basics**
- **Assets:** Anything owned with value (e.g., stocks, property)
- **Commodities:** Raw materials like gold, oil, traded in markets

**AWS and Google Cloud Overview**
- **Capabilities:** Hosting, storage, databases, machine learning, CI/CD, security
- **Examples of Use:** Deploying apps, creating VMs, serverless functions, cloud databases

**Unix**
- **Basic Commands:** cd, ls, mv, cp, rm, chmod
- **Using grep:** `grep 'pattern' file.txt`
- **MySQL Commands:** SELECT, INSERT, UPDATE, DELETE, CREATE, DROP
- **Job Scheduling Tools:** cron, at, Jenkins (CI/CD scheduling)
- **cron in Linux:** Time-based job scheduler
- **How it works:** Uses crontab file with time expression and command to run periodically

