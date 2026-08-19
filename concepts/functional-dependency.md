# Concept — Functional Dependency and ONLY_FULL_GROUP_BY

## Observation from Puzzle 001

MySQL accepted a query grouping by:

```sql
GROUP BY c.customer_id
```

while selecting:

```sql
c.first_name,
c.last_name
```

This can appear surprising because those columns were not explicitly listed in `GROUP BY`.

## Why?

`customer_id` is the primary key of `customer`.

For a given customer ID, there is one corresponding customer name.

Conceptually:

```text
customer_id → first_name
customer_id → last_name
```

This is a functional dependency.

## Contrast with GROUP BY active

When grouping by:

```sql
GROUP BY active
```

many customers can belong to the same group.

Therefore:

```text
active → customer_id
```

is not true.

There is no single customer ID that represents the active group.

## ONLY_FULL_GROUP_BY

MySQL's `ONLY_FULL_GROUP_BY` mode prevents ambiguous grouped queries.

Rather than silently choosing an arbitrary customer from a group, MySQL reports an error when the selected non-aggregated values cannot be justified by the grouping.

## DBA Relevance

Understanding functional dependency is useful when interpreting MySQL SQL-mode errors and when reasoning about schema design, keys, and grouped queries.
