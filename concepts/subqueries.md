# Concept — Subqueries

## Core Idea

A subquery is a query nested inside another SQL statement.

During Puzzle 001, a subquery was attempted with `IN`:

```sql
WHERE active IN (
    SELECT active, COUNT(*)
    FROM sakila.customer
    GROUP BY active
)
```

## Debugging Lesson

Before embedding a subquery, run it independently.

The inner query was:

```sql
SELECT
    active,
    COUNT(*)
FROM sakila.customer
GROUP BY active;
```

Its result contains two columns:

```text
active | COUNT(*)
```

Therefore it is not directly compatible with a scalar/single-column `IN` comparison such as:

```sql
WHERE active IN (...)
```

## General Habit

When debugging:

1. Run the outer query without the subquery if possible.
2. Run the subquery independently.
3. Inspect its columns.
4. Inspect its rows.
5. Determine what the outer predicate expects.
6. Recombine them.

## Future Topics

This concept will later expand to:

- scalar subqueries
- `IN`
- `EXISTS`
- `NOT EXISTS`
- correlated subqueries
- derived tables
- CTEs

The goal is not to prefer subqueries automatically, but to understand when they are appropriate and how MySQL executes them.
