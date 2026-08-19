# SQL Training — Daily Puzzle Lab

## Purpose

This repository documents a daily, hands-on SQL learning track using MySQL 8.4 and practical databases such as Sakila and Employees.

The objective is not simply to obtain correct SQL answers. Each puzzle is used to develop:

- SQL problem-solving and reasoning
- Understanding of relational operations
- Ability to diagnose SQL errors
- Comparison of multiple valid solutions
- Awareness of query cost and performance
- MySQL DBA-oriented thinking

The SQL track runs **in parallel with Project Phoenix**. It is not intended to replace or pause Phoenix.

## Daily Learning Loop

1. Receive one SQL puzzle.
2. Attempt the query independently.
3. Run it in MySQL Workbench.
4. Record successful and failed attempts.
5. Review the errors and the reasoning behind them.
6. Compare alternative valid solutions.
7. Record useful conceptual lessons.
8. Add performance/DBA observations when they become relevant.
9. Move to the next puzzle.

## Core Principle

> Document the path to the solution, not only the final solution.

Failed queries are valuable because they show how SQL reasoning developed and provide future reference material.

## Databases

Primary practice databases:

- `sakila`
- `employees`

Environment:

- MySQL 8.4
- Ubuntu 26.04
- MySQL Workbench

## Documentation Structure

```text
sql-training/
├── README.md
├── progress.md
├── daily-puzzles/
│   ├── 2026-08-19-puzzle-001-customer-rentals.md
│   └── ...
└── concepts/
    ├── group-by.md
    ├── where-vs-having.md
    ├── joins.md
    ├── subqueries.md
    ├── order-by-vs-group-by.md
    ├── count-and-null.md
    ├── functional-dependency.md
    └── ...
```

## Future Progression

The difficulty will gradually move through:

1. SELECT / WHERE / ORDER BY
2. Aggregation
3. GROUP BY / HAVING
4. JOINs
5. Subqueries
6. CTEs
7. Window functions
8. Advanced SQL problem-solving
9. Indexing
10. EXPLAIN
11. EXPLAIN ANALYZE
12. Query optimizer and performance analysis

The final stages will connect SQL problem solving directly to MySQL DBA and query-optimization skills.
