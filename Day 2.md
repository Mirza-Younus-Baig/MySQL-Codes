# 📘 MySQL Learning Summary – [Date: 2025-05-21]

# ✅ SQL Learning – Day 2 Summary

## 1. CASE Statement (Conditional Logic in SQL)

Used to categorize or label data based on conditions.

```sql
SELECT  CASE    WHEN condition THEN 'Result'    ELSE 'Default'  ENDFROM table;
```

Example: Classifying triangles (Equilateral, Isosceles, etc.).

---

## 2. Triangle Inequality Logic

Check if three sides form a valid triangle:

```sql
A + B > C AND B + C > A AND A + C > B
```

If this fails → “Not A Triangle”.

---

## 3. ROUND vs. TRUNCATE

| Function | Purpose | Example (5.6789, 2) |
| --- | --- | --- |
| `ROUND` | Rounds to nearest value | 5.68 |
| `TRUNCATE` | Cuts off, no rounding | 5.67 |
- `ROUND(number, decimal_places)`
- `TRUNCATE(number, decimal_places)`

---

## 4. CTEs (Common Table Expressions)

Simplify complex queries by creating a temporary named result set.

```sql
WITH cte_name AS (
  SELECT ...
)
SELECT ... FROM cte_name;
```

Example: Finding `MIN(LAT_N)` > threshold and related values.

---

## 5. Aggregation & Filtering

- Use `MIN()`, `MAX()` with `WHERE` to conditionally retrieve values.
- Example:
    
    ```sql
    SELECT TRUNCATE(MAX(LAT_N), 4)
    FROM STATION
    WHERE LAT_N < 137.2345;
    ```
    

---

## 🧠 Practice Suggestions

- Write your own `CASE` queries.
- Experiment with `ROUND` vs `TRUNCATE`.
- Use CTEs instead of nested `SELECT`s.

---

🎉 **Great progress on Day 2!** You’re building a solid foundation in SQL.

**Ready for Day 3 when you are!**