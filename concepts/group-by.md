# Concept — GROUP BY

## Core Idea

`GROUP BY` divides rows into groups according to one or more expressions.

The most important question when writing a grouped query is:

> **What should one group represent?**

Examples:

```sql
GROUP BY active
```

means:

> one group for each active state.

```sql
GROUP BY customer_id
```

means:

> one group per customer.

## Puzzle 001 Example

For the rental problem:

```sql
GROUP BY c.customer_id
```

was correct because the requirement was to calculate the number of rentals **for each customer**.

Conceptually:

```text
customer 1 → rental rows belonging to customer 1
customer 2 → rental rows belonging to customer 2
customer 3 → rental rows belonging to customer 3
```

## Important Distinction

`GROUP BY` is not the same as sorting.

```sql
GROUP BY active
```

collapses rows into groups.

```sql
ORDER BY active DESC
```

keeps the individual rows and only changes their presentation order.

## Common Mistake

Do not select arbitrary non-aggregated columns that are not determined by the grouping columns.

For example:

```sql
SELECT customer_id, first_name
FROM customer
GROUP BY active;
```

is ambiguous because one active group contains many customers.

## DBA Connection

Grouping can become expensive on large datasets. Later, we will use `EXPLAIN` and `EXPLAIN ANALYZE` to understand how MySQL executes grouped queries.
