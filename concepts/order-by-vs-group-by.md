# Concept — ORDER BY vs GROUP BY

## GROUP BY

`GROUP BY` changes the result shape by combining rows into groups.

Example:

```sql
SELECT
    active,
    COUNT(*) AS customer_count
FROM sakila.customer
GROUP BY active;
```

This produces one result row per active state.

## ORDER BY

`ORDER BY` does not combine rows.

Example:

```sql
SELECT
    customer_id,
    first_name,
    last_name,
    active
FROM sakila.customer
ORDER BY active DESC;
```

Every customer remains an individual result row, but active customers are displayed first.

## Key Mental Model

```text
GROUP BY
    → "How should rows be grouped?"

ORDER BY
    → "In what order should the result rows appear?"
```

## Practical Example

If the requirement is:

> Give me the number of active and inactive customers.

Use:

```sql
GROUP BY active
```

If the requirement is:

> Give me every customer, with active customers first.

Use:

```sql
ORDER BY active DESC
```

Do not confuse grouping with presentation order.
