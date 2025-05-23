# 📘 MySQL Learning Summary – Day 5 (May 23, 2025)

---

## ✅ Topics Covered

### 🔹 Ranking Functions

 **RANK()**: Assigns ranks with gaps for ties.
 **ROW_NUMBER()**: Assigns unique sequential numbers without gaps.
 **DENSE_RANK():** is a window function in SQL that assigns ranks to rows in an ordered partition similar to RANK(), but without gaps between the ranks.

### 🔹 Median Calculation

 Use `ROW_NUMBER()` to assign positions ordered by the target column.
 Use `ROW_NUMBER()` to assign positions ordered by the target column.
 Use a separate count to find total rows.
 Select middle row(s) and compute average for median.
 Use `ROUND(..., 4)` to round the result.

### 🔹 Understanding `ON TRUE`

 Used in joins without conditions, acts as a cross join.
 Allows joining a single-row CTE or subquery to all rows.

### 🔹 CTE Storage

 Temporary, in-memory or disk temp table depending on query planner.
 Not persistent beyond query execution.

### 🔹 Conditional Querying and Ordering

 Use `CASE` or `IF` for conditional columns or ordering.
 Sort conditionally by grade:
    - Show `NULL` for names if grade < 8.
    - Order names alphabetically if grade ≥ 8.
    - Order marks ascending if grade < 8.

---

## 💻 Code Examples

### Ranking with ROW_NUMBER()

```sql
SELECT
  name, department, salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num
FROM employees;
```

---

### **Median Calculation (with rounding)**

```
WITH ordered AS (
  SELECT LAT_N, ROW_NUMBER() OVER (ORDER BY LAT_N) AS rn FROM STATION
),
counted AS (
  SELECT COUNT(*) AS total FROM STATION
)
SELECT ROUND(AVG(LAT_N), 4) AS median
FROM ordered
JOIN counted ON TRUE
WHERE rn IN (FLOOR((total + 1)/2), CEIL((total + 1)/2));
```

---

### **Conditional Ordering with NULL names for low grades**

```
WITH graded AS (
  SELECT name, marks,
    CASE WHEN marks = 100 THEN 10 ELSE FLOOR(marks / 10) + 1 END AS grade
  FROM Students
)
SELECT
  CASE WHEN grade < 8 THEN NULL ELSE name END AS name,
  grade,
  marks
FROM graded
ORDER BY
  grade DESC,
  CASE WHEN grade >= 8 THEN name ELSE NULL END ASC,
  CASE WHEN grade < 8 THEN marks ELSE NULL END ASC;
```

---