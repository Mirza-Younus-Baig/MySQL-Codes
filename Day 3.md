# 📘 MySQL Learning Summary – Day 3 (May 22, 2025)

---

## ✅ Topics Covered

### 🔹 Square and Square Root

```sql
SELECT POWER(x, 2);      -- SquareSELECT SQRT(x);          -- Square Root
```

---

### 🔹 UNION and UNION ALL

- Combines results from two or more SELECTs.
- `UNION` removes duplicates.
- `UNION ALL` keeps all results.

```sql
SELECT name FROM employees
UNIONSELECT name FROM managers;
SELECT name, 'Employee' AS role FROM employees
UNIONSELECT name, 'Manager' FROM managers;
```

---

### 🔹 JOINs Overview

### 1. INNER JOIN

```sql
SELECT e.id, e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
```

### 2. LEFT JOIN

```sql
SELECT e.id, e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
```

### 3. RIGHT JOIN

```sql
SELECT e.id, e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
```

### 4. FULL OUTER JOIN (simulated)

```sql
SELECT e.id, e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.idUNIONSELECT e.id, e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
```

### 5. CROSS JOIN

```sql
SELECT e.name, d.dept_name
FROM employees e
CROSS JOIN departments d;
```

---

## 🧠 Key Takeaways

- `JOIN`s combine rows across tables based on relationships.
- `UNION` combines datasets vertically.
- Use `POWER()` and `SQRT()` for basic math operations.
- Column counts and types must match in `UNION` queries.

---

✅ End of Day 3. Ready for Day 4!