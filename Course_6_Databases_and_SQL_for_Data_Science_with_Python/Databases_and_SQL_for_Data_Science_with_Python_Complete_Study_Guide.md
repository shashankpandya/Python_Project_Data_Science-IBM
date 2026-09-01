# Course 6: Databases and SQL for Data Science with Python - Complete Study Guide
IBM Data Science Professional Certificate - Course 6 of 12
Learner: Pandya Shashank | Status: Completed - 100%

---

## Course Overview

Databases and SQL for Data Science with Python teaches the full SQL skill set required by professional data scientists, from basic SELECT queries through advanced JOINs, subqueries, stored procedures, and Python-SQL integration via the DB-API.

---

## Module 1: Relational Databases and SQL Fundamentals

### Relational Database Concepts
- A Relational Database organizes data into tables (relations) with rows (tuples) and columns (attributes/fields)
- Tables are linked via Primary Keys (unique row identifiers) and Foreign Keys (references to PKs in other tables)
- RDBMS software: IBM Db2, MySQL, PostgreSQL, Oracle, SQLite, Microsoft SQL Server

### SQL Sub-languages
| Sub-language | Commands | Purpose |
|:---|:---|:---|
| DDL - Data Definition Language | CREATE, ALTER, DROP, TRUNCATE | Define and modify database structure |
| DML - Data Manipulation Language | SELECT, INSERT, UPDATE, DELETE | Query and change data in tables |
| DCL - Data Control Language | GRANT, REVOKE | Control access permissions |
| TCL - Transaction Control Language | COMMIT, ROLLBACK, SAVEPOINT | Manage atomic transactions |

---

## Module 2: SELECT Queries - Complete Reference

### Full SELECT Syntax (Execution order in parentheses)
`sql
SELECT [DISTINCT] column1, column2, expression AS alias   -- (5) What to return
FROM table_name                                            -- (1) Source table
WHERE row_condition                                        -- (2) Filter rows
GROUP BY group_column                                      -- (3) Group rows
HAVING group_condition                                     -- (4) Filter groups
ORDER BY sort_column [ASC|DESC]                            -- (6) Sort output
LIMIT n OFFSET m;                                          -- (7) Paginate
`

### Comprehensive SELECT Examples
`sql
-- Select all columns
SELECT * FROM EMPLOYEES;

-- Specific columns with alias
SELECT EMP_ID, F_NAME, L_NAME, SALARY AS Annual_Salary
FROM EMPLOYEES;

-- DISTINCT: eliminate duplicate values
SELECT DISTINCT DEP_ID FROM EMPLOYEES;

-- WHERE with multiple conditions (AND, OR, NOT)
SELECT F_NAME, L_NAME, SALARY
FROM EMPLOYEES
WHERE SALARY >= 60000 AND DEP_ID = 5
ORDER BY SALARY DESC;

-- BETWEEN (inclusive on both ends)
SELECT * FROM EMPLOYEES WHERE SALARY BETWEEN 50000 AND 80000;

-- IN: match any value in a list
SELECT * FROM EMPLOYEES WHERE DEP_ID IN (2, 5, 7);

-- NOT IN: exclude values in a list
SELECT * FROM EMPLOYEES WHERE DEP_ID NOT IN (2, 5);

-- LIKE: pattern matching
-- % = any sequence of characters (zero or more)
-- _ = exactly one character
SELECT * FROM EMPLOYEES WHERE F_NAME LIKE 'J%';      -- Starts with J
SELECT * FROM EMPLOYEES WHERE L_NAME LIKE '%son';    -- Ends with son
SELECT * FROM EMPLOYEES WHERE L_NAME LIKE '_rown';   -- 5-char ending in rown
SELECT * FROM EMPLOYEES WHERE F_NAME LIKE '%an%';    -- Contains "an" anywhere

-- IS NULL / IS NOT NULL (never use = NULL)
SELECT * FROM EMPLOYEES WHERE MANAGER_ID IS NULL;
SELECT * FROM EMPLOYEES WHERE SALARY IS NOT NULL;

-- LIMIT and OFFSET (pagination)
SELECT * FROM EMPLOYEES ORDER BY SALARY DESC LIMIT 10;         -- Top 10 earners
SELECT * FROM EMPLOYEES ORDER BY SALARY DESC LIMIT 10 OFFSET 10; -- Page 2
`

---

## Module 3: Aggregate Functions and GROUP BY

`sql
-- Core aggregate functions (all ignore NULL by default except COUNT(*))
SELECT COUNT(*) AS total_rows FROM EMPLOYEES;           -- Counts ALL rows incl NULL
SELECT COUNT(SALARY) AS salary_count FROM EMPLOYEES;    -- Counts only non-NULL SALARY
SELECT SUM(SALARY) AS total_payroll FROM EMPLOYEES;
SELECT AVG(SALARY) AS avg_salary FROM EMPLOYEES;
SELECT MAX(SALARY) AS highest_salary FROM EMPLOYEES;
SELECT MIN(SALARY) AS lowest_salary FROM EMPLOYEES;
SELECT ROUND(AVG(SALARY), 2) AS rounded_avg FROM EMPLOYEES;

-- GROUP BY: calculate aggregate per group
SELECT DEP_ID,
       COUNT(*) AS dept_headcount,
       AVG(SALARY) AS avg_dept_salary,
       MIN(SALARY) AS min_salary,
       MAX(SALARY) AS max_salary
FROM EMPLOYEES
GROUP BY DEP_ID
ORDER BY avg_dept_salary DESC;

-- HAVING: filter GROUPS (applied after GROUP BY)
-- Cannot use aggregate functions in WHERE - must use HAVING
SELECT DEP_ID, AVG(SALARY) AS avg_salary
FROM EMPLOYEES
GROUP BY DEP_ID
HAVING AVG(SALARY) > 70000
ORDER BY avg_salary DESC;

-- WHERE vs HAVING: key distinction
SELECT DEP_ID, COUNT(*) AS emp_count
FROM EMPLOYEES
WHERE SALARY > 50000          -- WHERE filters individual ROWS before grouping
GROUP BY DEP_ID
HAVING COUNT(*) >= 3;         -- HAVING filters GROUPS after aggregation
`

---

## Module 4: Built-in Functions

### String Functions
`sql
-- IBM Db2 / Standard SQL
SELECT UCASE(F_NAME) AS upper_name FROM EMPLOYEES;    -- UPPER in PostgreSQL/MySQL
SELECT LCASE(L_NAME) AS lower_name FROM EMPLOYEES;    -- LOWER in PostgreSQL/MySQL
SELECT LENGTH(F_NAME) AS name_len FROM EMPLOYEES;     -- String length
SELECT TRIM('  hello  ') AS trimmed;                  -- Remove leading/trailing spaces
SELECT RTRIM(L_NAME) FROM EMPLOYEES;                  -- Remove right-side spaces
SELECT SUBSTR(F_NAME, 1, 3) FROM EMPLOYEES;           -- Substring: start pos 1, len 3
SELECT REPLACE(ADDRESS, 'Street', 'St') FROM EMPLOYEES;  -- Find and replace
SELECT CONCAT(F_NAME, ' ', L_NAME) AS full_name FROM EMPLOYEES;  -- Concatenate strings
`

### Date and Time Functions
`sql
-- IBM Db2
SELECT CURRENT_DATE;                               -- Returns current date
SELECT CURRENT_TIMESTAMP;                          -- Returns current datetime
SELECT YEAR(B_DATE)  AS birth_year  FROM EMPLOYEES;
SELECT MONTH(B_DATE) AS birth_month FROM EMPLOYEES;
SELECT DAY(B_DATE)   AS birth_day   FROM EMPLOYEES;

-- MySQL equivalents
SELECT CURDATE();                                  -- Current date
SELECT NOW();                                      -- Current datetime
SELECT YEAR(hire_date), MONTH(hire_date) FROM employees;
SELECT DATEDIFF(NOW(), birth_date) AS age_days FROM employees;
SELECT DATE_FORMAT(hire_date, '%Y-%m') AS ym FROM employees;

-- PostgreSQL equivalents
SELECT CURRENT_DATE;
SELECT EXTRACT(YEAR FROM hire_date) AS yr FROM employees;
SELECT AGE(hire_date) FROM employees;
`

### Conditional Expressions
`sql
-- CASE WHEN: if/elif/else equivalent in SQL
SELECT F_NAME, SALARY,
    CASE
        WHEN SALARY >= 100000 THEN 'Executive'
        WHEN SALARY >= 70000  THEN 'Senior'
        WHEN SALARY >= 50000  THEN 'Mid-level'
        ELSE 'Junior'
    END AS seniority_level
FROM EMPLOYEES;

-- COALESCE: return the first non-NULL value
SELECT F_NAME, COALESCE(PHONE, EMAIL, 'No Contact Info') AS contact
FROM EMPLOYEES;

-- NULLIF: returns NULL if the two expressions are equal (useful to avoid division by zero)
SELECT SALARY / NULLIF(HOURS_WORKED, 0) AS hourly_rate FROM EMPLOYEES;
`

---

## Module 5: Subqueries

Subqueries are nested SELECT statements embedded within another SQL query.

`sql
-- Subquery in WHERE: single-value (scalar) subquery
SELECT F_NAME, L_NAME, SALARY
FROM EMPLOYEES
WHERE SALARY = (SELECT MAX(SALARY) FROM EMPLOYEES);

-- Subquery with IN: multi-row subquery
SELECT F_NAME, L_NAME
FROM EMPLOYEES
WHERE DEP_ID IN (
    SELECT DEP_ID FROM DEPARTMENTS WHERE DEP_NAME = 'Engineering'
);

-- Correlated subquery: inner query references outer query table
-- Finds employees earning above their department's average
SELECT E.F_NAME, E.SALARY, E.DEP_ID
FROM EMPLOYEES E
WHERE E.SALARY > (
    SELECT AVG(E2.SALARY)
    FROM EMPLOYEES E2
    WHERE E2.DEP_ID = E.DEP_ID   -- References outer E.DEP_ID for each row
);

-- Subquery in FROM clause (derived table / inline view)
SELECT dept.DEP_ID, dept.avg_salary
FROM (
    SELECT DEP_ID, AVG(SALARY) AS avg_salary
    FROM EMPLOYEES
    GROUP BY DEP_ID
) AS dept
WHERE dept.avg_salary > 65000;

-- EXISTS: check if subquery returns any row
SELECT F_NAME, L_NAME
FROM EMPLOYEES E
WHERE EXISTS (
    SELECT 1 FROM DEPARTMENTS D
    WHERE D.DEP_ID = E.DEP_ID
    AND D.LOCATION_ID = 'L0001'
);
`

---

## Module 6: JOINs - Complete Reference

JOINs combine rows from two or more tables based on a related column.

`sql
-- INNER JOIN: only rows with matching values in BOTH tables
SELECT E.F_NAME, E.L_NAME, D.DEP_NAME
FROM EMPLOYEES E
INNER JOIN DEPARTMENTS D ON E.DEP_ID = D.DEPT_ID_DEP;

-- LEFT OUTER JOIN: all rows from LEFT + matching from RIGHT (NULL if no match)
SELECT E.EMP_ID, E.F_NAME, D.DEP_NAME
FROM EMPLOYEES E
LEFT OUTER JOIN DEPARTMENTS D ON E.DEP_ID = D.DEPT_ID_DEP;
-- Result: all employees; employees with no dept show NULL for DEP_NAME

-- RIGHT OUTER JOIN: all rows from RIGHT + matching from LEFT (NULL if no match)
SELECT E.F_NAME, D.DEP_NAME
FROM EMPLOYEES E
RIGHT OUTER JOIN DEPARTMENTS D ON E.DEP_ID = D.DEPT_ID_DEP;
-- Result: all departments; depts with no employees show NULL for F_NAME

-- FULL OUTER JOIN: all rows from BOTH tables (NULLs where no match)
SELECT E.F_NAME, D.DEP_NAME
FROM EMPLOYEES E
FULL OUTER JOIN DEPARTMENTS D ON E.DEP_ID = D.DEPT_ID_DEP;

-- CROSS JOIN: Cartesian product (every row of A x every row of B)
SELECT E.F_NAME, D.DEP_NAME
FROM EMPLOYEES E CROSS JOIN DEPARTMENTS D;
-- WARNING: Returns rows(E) * rows(D) combinations - use carefully!

-- SELF JOIN: join a table to itself (e.g., employee -> manager hierarchy)
SELECT E1.F_NAME AS Employee, E2.F_NAME AS Manager
FROM EMPLOYEES E1
INNER JOIN EMPLOYEES E2 ON E1.MANAGER_ID = E2.EMP_ID;

-- Implicit JOIN (old style, equivalent to INNER JOIN - avoid in modern SQL)
SELECT E.F_NAME, D.DEP_NAME
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEP_ID = D.DEPT_ID_DEP;
`

### JOIN Type Visual Summary
| JOIN Type | Left Rows | Right Rows | NULLs? |
|:---|:---|:---|:---|
| INNER | Matching only | Matching only | No |
| LEFT OUTER | All | Matching (NULL if missing) | Right side may be NULL |
| RIGHT OUTER | Matching (NULL if missing) | All | Left side may be NULL |
| FULL OUTER | All | All | Both sides may be NULL |
| CROSS | All | All | No (Cartesian product) |

---

## Module 7: DDL - Create and Manage Tables

`sql
-- CREATE TABLE with constraints
CREATE TABLE EMPLOYEES (
    EMP_ID     CHAR(9)       NOT NULL PRIMARY KEY,
    F_NAME     VARCHAR(15)   NOT NULL,
    L_NAME     VARCHAR(15)   NOT NULL,
    SSN        CHAR(9)       UNIQUE NOT NULL,
    B_DATE     DATE          CHECK(B_DATE > '1900-01-01'),
    SEX        CHAR(1)       CHECK(SEX IN ('M', 'F')),
    ADDRESS    VARCHAR(30),
    JOB_ID     CHAR(9)       REFERENCES JOB(JOB_IDENT),
    SALARY     DECIMAL(10,2) DEFAULT 30000.00,
    MANAGER_ID CHAR(9)
);

-- ALTER TABLE
ALTER TABLE EMPLOYEES ADD COLUMN EMAIL VARCHAR(100);      -- Add column
ALTER TABLE EMPLOYEES DROP COLUMN EMAIL;                   -- Remove column

-- INSERT rows
INSERT INTO EMPLOYEES (EMP_ID, F_NAME, L_NAME, SALARY)
VALUES ('E001', 'John', 'Smith', 75000.00);

-- UPDATE rows
UPDATE EMPLOYEES
SET SALARY = SALARY * 1.05
WHERE DEP_ID = 5;

-- DELETE rows
DELETE FROM EMPLOYEES WHERE EMP_ID = 'E001';

-- DROP TABLE (permanent!)
DROP TABLE EMPLOYEES;

-- TRUNCATE (remove all rows, keep structure)
TRUNCATE TABLE EMPLOYEES;
`

---

## Module 8: Python and SQL Integration

### sqlite3 - Built-in Python DB-API
`python
import sqlite3
import pandas as pd

# Connect (creates .db file if not exists)
conn = sqlite3.connect('HR_database.db')
cursor = conn.cursor()

# Create table
cursor.execute("""
    CREATE TABLE IF NOT EXISTS Employees (
        EMP_ID TEXT PRIMARY KEY,
        F_NAME TEXT NOT NULL,
        SALARY REAL
    )
""")

# Insert rows
cursor.execute("INSERT INTO Employees VALUES ('E001', 'John', 75000.00)")
conn.commit()   # Must call commit() for DML changes to persist

# Query with cursor
cursor.execute("SELECT * FROM Employees WHERE SALARY > 60000")
rows = cursor.fetchall()    # List of tuples
for row in rows:
    print(row)

# Query directly into Pandas DataFrame (most common data science pattern)
df = pd.read_sql("SELECT * FROM Employees ORDER BY SALARY DESC", conn)
print(df.head())

# Write DataFrame to SQL table
new_data = pd.DataFrame({"EMP_ID": ["E002"], "F_NAME": ["Jane"], "SALARY": [85000.0]})
new_data.to_sql("Employees", conn, if_exists="append", index=False)

conn.close()
`

### Jupyter SQL Magic Extension
`python
# Install: pip install ipython-sql sqlalchemy
%load_ext sql

# Connect to SQLite
%sql sqlite:///HR_database.db

# Connect to IBM Db2 cloud
# %sql ibm_db_sa://username:password@hostname:port/database

# Run SQL in a single line
%sql SELECT COUNT(*) AS total FROM EMPLOYEES;

# Run multi-line SQL block
%%sql
SELECT DEP_ID,
       COUNT(*) AS emp_count,
       AVG(SALARY) AS avg_salary
FROM EMPLOYEES
GROUP BY DEP_ID
ORDER BY avg_salary DESC;

# Capture result into Python variable for further analysis
result = %sql SELECT * FROM EMPLOYEES WHERE SALARY > 70000
df = result.DataFrame()   # Convert ResultSet to Pandas DataFrame
print(df.shape)
`

---

## Final Project: Chicago Socioeconomic and Crime Data Analysis

### Key SQL Queries from the Final Assignment
`sql
-- 1. Total crimes recorded
SELECT COUNT(*) AS total_crimes FROM CHICAGO_CRIME_DATA;

-- 2. Crimes involving children (KIDNAPPING)
SELECT * FROM CHICAGO_CRIME_DATA
WHERE PRIMARY_TYPE = 'KIDNAPPING'
AND DESCRIPTION LIKE '%CHILD%';

-- 3. Most crime-prone community area
SELECT COMMUNITY_AREA_NUMBER, COUNT(*) AS crime_count
FROM CHICAGO_CRIME_DATA
GROUP BY COMMUNITY_AREA_NUMBER
ORDER BY crime_count DESC
LIMIT 1;

-- 4. Community area with highest hardship index (via subquery)
SELECT COMMUNITY_AREA_NAME
FROM CENSUS_DATA
WHERE HARDSHIP_INDEX = (SELECT MAX(HARDSHIP_INDEX) FROM CENSUS_DATA);

-- 5. Top 5 most crime-prone community areas with names (JOIN)
SELECT C.COMMUNITY_AREA_NAME, COUNT(*) AS crime_count
FROM CHICAGO_CRIME_DATA CR
JOIN CENSUS_DATA C ON CR.COMMUNITY_AREA_NUMBER = C.COMMUNITY_AREA_NUMBER
GROUP BY C.COMMUNITY_AREA_NAME
ORDER BY crime_count DESC
LIMIT 5;
`

---

## Assessment and Grade Summary

| Assessment | Score |
|:---|:---:|
| Graded Quiz: Getting Started with SQL | 100% |
| Graded Quiz: Introduction to Relational Databases and SQL | 100% |
| Graded Quiz: Intermediate SQL | 100% |
| Graded Quiz: Accessing Databases using Python | 100% |
| Final Exam | 100% |
| **Overall Grade** | **100%** |

---

## SQL Quick Reference Cheat Sheet

| Command | Pattern | Purpose |
|:---|:---|:---|
| SELECT | SELECT col FROM tbl WHERE cond | Query rows from a table |
| COUNT/SUM/AVG/MAX/MIN | SELECT AVG(salary) FROM tbl | Aggregate functions |
| GROUP BY | GROUP BY col HAVING cond | Group rows and filter groups |
| INNER JOIN | FROM A INNER JOIN B ON A.id = B.id | Only matching rows from both |
| LEFT JOIN | FROM A LEFT JOIN B ON A.id = B.id | All rows from A + matches from B |
| Subquery | WHERE col = (SELECT MAX(col) FROM tbl) | Nested query |
| LIKE | WHERE name LIKE 'J%' | Pattern matching |
| BETWEEN | WHERE salary BETWEEN 50000 AND 80000 | Inclusive range |
| CASE WHEN | CASE WHEN cond THEN val ELSE val END | Conditional column |
| pd.read_sql() | pd.read_sql(sql, conn) | SQL query result to DataFrame |
| .to_sql() | df.to_sql(name, conn, if_exists='replace') | DataFrame to SQL table |

---
IBM Data Science Professional Certificate - Pandya Shashank
