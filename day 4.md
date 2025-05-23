# 📘 MySQL Learning Summary – Day 4 (May 23, 2025)

---

## ✅ Topics Covered

### 🔹 Column Value in Another Column

```sql
SELECT * FROM table1
WHERE column1 IN (SELECT column2 FROM table2);
```

- `IN` must be used with a subquery or fixed list.

---

### 🔹 Aggregating Hierarchical Roles

```sql
COUNT(DISTINCT role_code)
```

- Use `LEFT JOIN` to ensure all companies are included.
- Avoids missing data for companies with no certain role.

---

### 🔹 Invalid Nested Aggregation

```sql
-- ❌ Invalid:SUM(COUNT(column))
-- ✅ Correct:COUNT(column)
```

---

### 🔹 Optimized Aggregation with Subqueries

```sql
SELECT c.company_code, c.founder, COALESCE(r.count, 0)
FROM Company c
LEFT JOIN (
    SELECT company_code, COUNT(DISTINCT role_code) AS count    FROM Role    GROUP BY company_code
) r ON c.company_code = r.company_code;
```

- Each role table is aggregated first.
- Prevents join explosion.

---

### 🔹 Performance Comparison

### ❌ JOIN + GROUP BY (Slower)

```sql
SELECT
    c.company_code,
    c.founder,
    COUNT(DISTINCT l.lead_manager_code),
    COUNT(DISTINCT s.senior_manager_code),
    COUNT(DISTINCT m.manager_code),
    COUNT(DISTINCT e.employee_code)
FROM Company c
JOIN Lead_Manager l ON c.company_code = l.company_code
JOIN Senior_Manager s ON c.company_code = s.company_code
JOIN Manager m ON c.company_code = m.company_code
JOIN Employee e ON c.company_code = e.company_code
GROUP BY c.company_code, c.founder
ORDER BY c.company_code;
```

### ✅ Subquery + JOIN (Faster)

```sql
SELECT
    c.company_code,
    c.founder,
    COALESCE(l.count, 0) AS lead_manager_count,
    COALESCE(s.count, 0) AS senior_manager_count,
    COALESCE(m.count, 0) AS manager_count,
    COALESCE(e.count, 0) AS employee_count
FROM Company c
LEFT JOIN (
    SELECT company_code, COUNT(DISTINCT lead_manager_code) AS count    FROM Lead_Manager
    GROUP BY company_code
) l ON c.company_code = l.company_code
LEFT JOIN (
    SELECT company_code, COUNT(DISTINCT senior_manager_code) AS count    FROM Senior_Manager
    GROUP BY company_code
) s ON c.company_code = s.company_code
LEFT JOIN (
    SELECT company_code, COUNT(DISTINCT manager_code) AS count    FROM Manager
    GROUP BY company_code
) m ON c.company_code = m.company_code
LEFT JOIN (
    SELECT company_code, COUNT(DISTINCT employee_code) AS count    FROM Employee
    GROUP BY company_code
) e ON c.company_code = e.company_code
ORDER BY c.company_code;
```

---

### 🔹 Visual Flow

- Document created: **Subquery Vs Joins Flow**
- Describes the difference between the two methods visually.

---

### 🔹 COALESCE() Usage

```sql
COALESCE(value, 0)
```

- Returns the first non-NULL value.
- Replaces NULL in results with 0.

---

✅ End of Day 4. Ready for Day 5!