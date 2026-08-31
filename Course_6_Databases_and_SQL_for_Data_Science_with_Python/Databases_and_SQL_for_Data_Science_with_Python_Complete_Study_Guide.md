# Course 6: Databases and SQL for Data Science with Python - Complete Study Guide

## Executive Summary
This document provides a comprehensive study guide and key concepts for **Course 6: Databases and SQL for Data Science with Python**, part of the IBM Data Science Professional Certificate. All assessments have been completed with an overall score of **96.5%** (all scores between 90% and 100%).

---

## Final Official Grade Breakdown

| Assessment Item | Category / Type | Weight | Status | Grade |
| :--- | :--- | :---: | :---: | :---: |
| **Graded Quiz: Basic SQL** | Module 1 Graded Quiz | 10% | **Success: Passed** | **100%** |
| **Relational DB Concepts and Tables** | Module 2 Graded Quiz | 10% | **Success: Passed** | **100%** |
| **Graded Quiz: Refining Your Results** | Module 3 Graded Quiz | 10% | **Success: Passed** | **95%** |
| **Graded Quiz: Functions, Multiple Tables, and Sub-queries** | Module 3 Graded Quiz | 10% | **Success: Passed** | **100%** |
| **Graded Quiz: Accessing databases using Python** | Module 4 Graded Quiz | 10% | **Success: Passed** | **100%** |
| **Graded Quiz on Assignment** | Module 5 Final Assignment | 20% | **Success: Passed** | **90%** |
| **Final Exam** | Module 5 Final Exam | 30% | **Success: Passed** | **100%** |
| **OVERALL COURSE GRADE** | **Comprehensive Score** | **100%** | **PASSED** | **96.5%** |

---

## Key Technical Concepts & Knowledge Review

### 1. Introduction to Databases and Basic SQL
- **Database Basics**: A database is an organized collection of data. Relational Databases organize data into tables consisting of rows (records) and columns (attributes/fields).
- **Primary Key**: A column (or set of columns) that uniquely identifies each row in a table.
- **Foreign Key**: A column in one table that references the primary key in another table, creating a relational link.
- **SQL Categories**:
  - **DDL (Data Definition Language)**: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`.
  - **DML (Data Manipulation Language)**: `SELECT`, `INSERT`, `UPDATE`, `DELETE`.

### 2. Relational Database Concepts & Table Manipulation
- **CREATE TABLE**:
  ```sql
  CREATE TABLE EMPLOYEES (
      EMP_ID CHAR(9) NOT NULL PRIMARY KEY,
      F_NAME VARCHAR(15) NOT NULL,
      L_NAME VARCHAR(15) NOT NULL,
      DEP_ID CHAR(5)
  );
  ```
- **INSERT**: `INSERT INTO EMPLOYEES (EMP_ID, F_NAME, L_NAME) VALUES ('E1001', 'John', 'Doe');`
- **UPDATE**: `UPDATE EMPLOYEES SET DEP_ID = 'D01' WHERE EMP_ID = 'E1001';`
- **DELETE**: `DELETE FROM EMPLOYEES WHERE DEP_ID IN ('D01', 'D02');`

### 3. Refining Query Results (Filtering, Sorting, Grouping)
- **String Patterns (`LIKE`)**:
  - `%` represents zero or more characters (e.g., `WHERE L_NAME LIKE 'J%'`).
  - `_` represents a single character (e.g., `WHERE EMP_ID LIKE 'E100_'`).
- **Sorting (`ORDER BY`)**:
  - Sorts results ascending (`ASC`) or descending (`DESC`).
  - Example: `SELECT * FROM EMPLOYEES ORDER BY SALARY DESC LIMIT 5;`
- **Grouping (`GROUP BY` & `HAVING`)**:
  - `GROUP BY` aggregates rows sharing identical values in specified columns.
  - `HAVING` filters aggregated group results (unlike `WHERE`, which filters individual rows before aggregation).
  - Example:
    ```sql
    SELECT Country, COUNT(CustomerID) 
    FROM Customers 
    GROUP BY Country 
    HAVING COUNT(CustomerID) > 5;
    ```

### 4. Advanced SQL (Functions, Multiple Tables, Sub-queries)
- **Aggregate & Built-in Functions**: `SUM()`, `AVG()`, `COUNT()`, `MAX()`, `MIN()`, `DATEDIFF()`, `DATE_ADD()`.
- **Sub-queries (Nested Selects)**:
  - Sub-query evaluated first; outer query uses the evaluated sub-query result.
  - Example:
    ```sql
    SELECT F_NAME, L_NAME 
    FROM EMPLOYEES 
    WHERE SALARY = (SELECT MAX(SALARY) FROM EMPLOYEES);
    ```
- **Joining Multiple Tables**:
  - Implicit Join / WHERE Join: `SELECT E.F_NAME, D.DEP_NAME FROM EMPLOYEES E, DEPARTMENTS D WHERE E.DEP_ID = D.DEPT_ID_DEP;`
  - Explicit Joins: `INNER JOIN`, `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`, `FULL OUTER JOIN`.

### 5. Accessing Databases using Python & Pandas
- **Jupyter SQL Magic**:
  - Load SQL extension: `%load_ext sql`
  - Connect to SQLite DB: `%sql sqlite:///EMP.db`
- **Python DB-API (sqlite3)**:
  ```python
  import sqlite3
  import pandas as pd

  conn = sqlite3.connect('HR.db')
  # Read SQL query directly into a DataFrame
  df = pd.read_sql('SELECT * FROM Employees', conn)

  # Write DataFrame into an SQL table
  df.to_sql('SampleTable', conn, if_exists='replace', index=False)

  # Using cursor
  cursor = conn.cursor()
  cursor.execute('SELECT * FROM Employees')
  rows = cursor.fetchall()
  ```
- **DataFrame Summary Statistics**: `df.describe()` returns count, mean, std, min, 25%, 50%, 75%, max for numerical columns.

---

## Final Chicago Socioeconomic & Crime Data Assignment Summary
- **Crime Table Total Count**: 533 recorded crimes.
- **Crimes Involving Minors**: 2 rows retrieved (`PRIMARY_TYPE = 'KIDNAPPING' AND DESCRIPTION LIKE '%CHILD%'`).
- **Highest Hardship Index Subquery**:
  ```sql
  SELECT COMMUNITY_AREA_NAME 
  FROM CENSUS_DATA 
  WHERE HARDSHIP_INDEX IN (SELECT MAX(HARDSHIP_INDEX) FROM CENSUS_DATA);
  ```
- **Most Crime-Prone Community Area**: Community Area 25.0 (**Austin**).

---
*Study Guide Generated & Verified for Coursera IBM Data Science Professional Certificate.*
