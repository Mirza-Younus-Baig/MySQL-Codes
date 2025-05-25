# Day 6 (May 25) - SQL Problem Summary

## 📝 Today's Problem Summary

Samantha conducts interviews using coding contests. For each contest, we must calculate and display:

- contest_id, hacker_id, name
- The **sum** of:
    - total_submissions
    - total_accepted_submissions
    - total_views
    - total_unique_views

Exclude contests where **all four metrics are 0 or NULL**. Sort the output by contest_id.

## 👨‍💻 Your Version of the Code (Nested Aggregation)

```sql
SELECT cn.contest_id, cn.hacker_id, cn.name, cov.sts, cov.stas, cov.stv, cov.stuv
FROM contests cn
JOIN (
    SELECT co.contest_id,
           SUM(chv.sts) AS sts,
           SUM(chv.stas) AS stas,
           SUM(chv.stv) AS stv,
           SUM(chv.stuv) AS stuv
    FROM colleges co
    JOIN (
        SELECT ch.challenge_id, ch.college_id,
               COALESCE(SUM(ss.total_submissions), 0) AS sts,
               COALESCE(SUM(ss.total_accepted_submissions), 0) AS stas,
               COALESCE(SUM(vs.total_views), 0) AS stv,
               COALESCE(SUM(vs.total_unique_views), 0) AS stuv
        FROM challenges ch
        LEFT JOIN view_stats vs ON ch.challenge_id = vs.challenge_id
        LEFT JOIN submission_stats ss ON ch.challenge_id = ss.challenge_id
        GROUP BY ch.challenge_id, ch.college_id
    ) AS chv ON co.college_id = chv.college_id
    GROUP BY co.contest_id
    HAVING NOT (sts = 0 AND stas = 0 AND stv = 0 AND stuv = 0)
) AS cov ON cn.contest_id = cov.contest_id
ORDER BY cn.contest_id;

```

## ✅ Recommended Simplified Version (Flat Join, Better Performance)

```sql
SELECT
    c.contest_id,
    c.hacker_id,
    c.name,
    SUM(COALESCE(ss.total_submissions, 0)) AS total_submissions,
    SUM(COALESCE(ss.total_accepted_submission, 0)) AS total_accepted_submissions,
    SUM(COALESCE(vs.total_views, 0)) AS total_views,
    SUM(COALESCE(vs.total_unique_views, 0)) AS total_unique_views
FROM contests c
JOIN colleges cl ON c.contest_id = cl.contest_id
JOIN challenges ch ON cl.college_id = ch.college_id
LEFT JOIN submission_stats ss ON ch.challenge_id = ss.challenge_id
LEFT JOIN view_stats vs ON ch.challenge_id = vs.challenge_id
GROUP BY c.contest_id, c.hacker_id, c.name
HAVING
    SUM(COALESCE(ss.total_submissions, 0)) > 0 OR
    SUM(COALESCE(ss.total_accepted_submission, 0)) > 0 OR
    SUM(COALESCE(vs.total_views, 0)) > 0 OR
    SUM(COALESCE(vs.total_unique_views, 0)) > 0
ORDER BY c.contest_id;

```

## 🆚 Comparison: Your Code vs Recommended Code

| Feature | Your Version | Recommended Version |
| --- | --- | --- |
| Structure | Deeply nested subqueries | Flat and linear joins |
| Aggregation Level | First at challenge → college → contest | Directly at contest level |
| Performance | Slightly heavier due to nested grouping | Better performance with fewer joins |
| Readability | Moderate (good aliases used) | High (clean join path) |
| Accuracy | Correct if data has no duplication at college level | More robust against data volume and duplication |

## 📘 Concept Recap

### 🔸 GROUP BY

Used to group rows that share a value so aggregate functions (like SUM, COUNT) can be applied to each group.

### 🔸 HAVING vs WHERE

- **WHERE** filters **rows before** aggregation.
- **HAVING** filters **groups after** aggregation.

### 🔸 COALESCE()

Returns the first non-NULL value in its list of arguments. Useful to avoid NULL in aggregates:

```sql
SUM(COALESCE(column, 0))

```