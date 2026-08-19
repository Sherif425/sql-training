# SQL Training Progress

| # | Date | Database | Main Topic | Result | Key Lesson |
|---|---|---|---|---|---|
| 001 | 2026-08-19 | Sakila | JOIN + GROUP BY + HAVING + COUNT | ✅ Correct with ordering improvement | One group should represent one customer; `HAVING` filters groups; `ORDER BY` sorts rows |

## Topics Encountered

### SQL Fundamentals
- [x] SELECT
- [x] WHERE
- [x] ORDER BY
- [ ] LIMIT — encountered through MySQL Workbench output, not a puzzle focus

### Aggregation
- [x] COUNT
- [x] GROUP BY
- [x] HAVING
- [ ] SUM
- [ ] AVG
- [ ] MIN / MAX
- [ ] COUNT(DISTINCT ...)

### JOINs
- [x] INNER JOIN
- [ ] LEFT JOIN
- [ ] Multiple JOINs
- [ ] Self JOIN

### Subqueries
- [x] Basic `IN` subquery concept
- [ ] Correlated subqueries
- [ ] EXISTS / NOT EXISTS

### Advanced SQL
- [ ] CTEs
- [ ] Window functions
- [ ] Recursive CTEs

### MySQL Performance / DBA
- [ ] Indexes
- [ ] Composite indexes
- [ ] Sargability
- [ ] EXPLAIN
- [ ] EXPLAIN ANALYZE
- [ ] Optimizer behavior

## Questions to Revisit

- Why does MySQL allow non-grouped columns when they are functionally dependent on a grouped primary key?
- When should `WHERE` be used instead of `HAVING`?
- What is the practical difference between `COUNT(*)` and `COUNT(column)`?
- When is `INNER JOIN` preferable to `LEFT JOIN`?
- What does SQL guarantee about result ordering when `ORDER BY` is absent?
