# Concept — WHERE vs HAVING

## WHERE

`WHERE` filters individual rows before grouping.

Conceptually:

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
```

Example:

```sql
SELECT *
FROM sakila.customer
WHERE active = 1;
```

This asks for individual active customer rows.

## HAVING

`HAVING` filters groups after aggregation.

Example:

```sql
SELECT
    customer_id,
    COUNT(*) AS rental_count
FROM sakila.rental
GROUP BY customer_id
HAVING COUNT(*) > 30;
```

This asks for customer groups whose aggregated count exceeds 30.

## Important Rule

Use:

```text
WHERE → row-level filtering
HAVING → group-level filtering
```

## A Subtle Observation

This is legal:

```sql
GROUP BY active
HAVING active = 1
```

It means:

1. Form groups for active = 0 and active = 1.
2. Keep only the active = 1 group.

However, if the same condition can filter rows before grouping, `WHERE` is often preferable because it can reduce the amount of data that must be processed.

We will investigate this later with execution plans rather than assuming a performance benefit in every case.
