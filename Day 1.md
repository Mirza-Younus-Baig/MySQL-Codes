
# 📘 MySQL Learning Summary – 2025-05-14

## ✅ Concepts & Learnings

### 🔹 Pattern Printing (Using Loops in Stored Procedures)
Use `WHILE` loops inside `CREATE PROCEDURE`.
Use `CONCAT()` to build strings.
`SELECT` prints each line on a new row.
`"*"` is 1 character; `"* "` is 2 characters.

### 🔹 Stored Procedures
```sql
DELIMITER //
CREATE PROCEDURE proc_name()
BEGIN
  -- logic
END //
DELIMITER ;
```

### 🔹 CASE Statement
Conditional output in `SELECT` or `ORDER BY`.

Example:
```sql
SELECT 
  CASE 
    WHEN occupation = 'Doctor' THEN name 
    ELSE NULL 
  END AS Doctor
FROM occupations;
```

### 🔹 Pivoting Data
Use multiple `CASE WHEN` in `SELECT`.
No built-in `PIVOT` in MySQL.

### 🔹 Counting
`COUNT(*)`: count all rows.
`COUNT(CASE WHEN ...)`: count conditionally.

### 🔹 Rounding
`ROUND(num)` – Nearest
`CEIL(num)` – Round up
`FLOOR(num)` – Round down

### 🔹 Type Conversion
`CAST(expr AS type)` – SQL standard
`CONVERT(expr, type)` – MySQL-specific

### 🔹 Temporary Tables
```sql
CREATE TEMPORARY TABLE temp_name (...);
```

### 🔹 CTE (Common Table Expression) – MySQL 8+
```sql
WITH cte_name AS (
  SELECT ...
)
SELECT * FROM cte_name;
```

### 🔹 SQL Execution Order
1. `FROM`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `ORDER BY`
7. `LIMIT`

> 🚫 Can't use SELECT aliases in WHERE clause.

## 💻 SQL Examples & Commands

### Pattern Printing Procedure
```sql
DELIMITER //
CREATE PROCEDURE star()
BEGIN
  DECLARE i INT DEFAULT 1;
  DECLARE j INT;
  DECLARE line VARCHAR(100);
  WHILE i <= 5 DO
    SET line = '';
    SET j = 1;
    WHILE j <= i DO
      SET line = CONCAT(line, '* ');
      SET j = j + 1;
    END WHILE;
    SELECT line;
    SET i = i + 1;
  END WHILE;
END //
DELIMITER ;
CALL star();
```

### Pivot Table Using CASE
```sql
SELECT
  CASE WHEN occupation = 'Doctor' THEN name ELSE NULL END AS Doctor,
  CASE WHEN occupation = 'Professor' THEN name ELSE NULL END AS Professor,
  CASE WHEN occupation = 'Singer' THEN name ELSE NULL END AS Singer,
  CASE WHEN occupation = 'Actor' THEN name ELSE NULL END AS Actor
FROM occupations;
```

### Salary Error Calculation (Zero-key error)
```sql
SELECT
  CEIL(AVG(salary) - AVG(CAST(REPLACE(salary, '0', '') AS UNSIGNED))
FROM employees;
```

### Temporary Table
```sql
CREATE TEMPORARY TABLE temp_emp AS
SELECT id, name FROM employees WHERE department = 'HR';
```

### CTE Example (MySQL 8+)
```sql
WITH high_paid AS (
  SELECT name, salary FROM employees WHERE salary > 100000
)
SELECT * FROM high_paid;
```

### Recursive CTE Example (1 to 10)
```sql
WITH RECURSIVE nums AS (
  SELECT 1 AS n
  UNION ALL
  SELECT n + 1 FROM nums WHERE n < 10
)
SELECT * FROM nums;
```

### Max Annual Salary & Count (No CTE)
```sql
SELECT
  MAX(annualSalary) AS maxSal,
  COUNT(*) AS count_of_employees_with_max_salary
FROM (
  SELECT salary * months AS annualSalary
  FROM Employee
) AS temp
WHERE annualSalary = (
  SELECT MAX(salary * months) FROM Employee
);
```

