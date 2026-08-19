# SQL Puzzle 001 — Customers with More Than 30 Rentals

**Date:** 2026-08-19  
**Database:** Sakila  
**Difficulty:** Beginner → Intermediate  
**Topics:** JOIN, GROUP BY, HAVING, COUNT, ORDER BY, functional dependency

---

## 1. Problem

Find all customers who have made more than 30 rentals.

Return:

- `customer_id`
- `first_name`
- `last_name`
- rental count

Sort by rental count from highest to lowest.

### Constraints

- No subquery
- No CTE
- No window function

---

## 2. Initial Reasoning

The first attempts focused on the `customer.active` column rather than the `rental` table.

This led to an important discovery:

- `customer.active` describes the customer's current active state.
- `rental` contains the individual rental records.
- To count rentals, the query must count rental rows associated with each customer.

The key reasoning step was:

> **What should one GROUP represent?**

The answer is:

> One group should represent one customer.

Therefore, `GROUP BY customer_id` is appropriate.

---

## 3. Failed Attempts and Lessons

### Attempt 1 — Trying to group by an aggregate

```sql
SELECT
    customer_id,
    first_name,
    last_name,
    COUNT(active) AS count
FROM sakila.customer
GROUP BY COUNT(active)
HAVING active = 1
ORDER BY customer_id DESC;
```

### Error

```text
Error Code: 1056. Can't group on 'count'
```

### Why it failed

`GROUP BY` defines the groups on which aggregation operates. `COUNT(active)` is an aggregate calculated after groups have been formed.

The conceptual order is approximately:

```text
FROM
WHERE
GROUP BY
aggregate functions
HAVING
SELECT
ORDER BY
```

Therefore, using `COUNT(active)` as the grouping expression in this situation reverses the normal dependency between grouping and aggregation.

---

### Attempt 2 — Grouping by active state

```sql
SELECT
    customer_id,
    first_name,
    last_name,
    COUNT(active) AS count
FROM sakila.customer
GROUP BY active
HAVING active = 1
ORDER BY customer_id DESC;
```

### Error

```text
Error Code: 1055. Expression #1 of SELECT list is not in GROUP BY clause
and contains nonaggregated column ...
This is incompatible with sql_mode=only_full_group_by
```

### Why it failed

`GROUP BY active` is actually valid if the objective is to create two groups:

```text
active = 0 → inactive customers
active = 1 → active customers
```

The problem was selecting individual customer attributes from those groups.

For example, the `active = 1` group contains many customers. SQL cannot infer which `customer_id`, `first_name`, or `last_name` should represent the whole group.

This is why `ONLY_FULL_GROUP_BY` protects against ambiguous queries.

---

### Attempts 3–5 — Variations of GROUP BY active

The query was modified by changing `ORDER BY` and adding `active` to the SELECT list, but the fundamental issue remained:

```sql
GROUP BY active
```

creates groups by active state, not groups by customer.

Adding `active` to the SELECT list does not resolve the ambiguity of:

```text
customer_id
first_name
last_name
```

inside a group containing many customers.

---

### Attempt 6 — Subquery with IN

```sql
SELECT
    first_name,
    last_name,
    customer_id
FROM sakila.customer
WHERE acitve IN (
    SELECT
        active,
        COUNT(*)
    FROM sakila.customer
    GROUP BY active
);
```

### Error

```text
Error Code: 1054. Unknown column 'acitve' ...
```

### First problem

There was a spelling mistake:

```sql
acitve
```

instead of:

```sql
active
```

### Deeper problem

The subquery returns two columns:

```text
active | COUNT(*)
```

A normal `IN` predicate such as:

```sql
WHERE active IN (...)
```

expects a compatible single-column result.

This led to an important debugging habit:

> Run a complex subquery independently and inspect exactly what it returns before embedding it into the outer query.

---

## 4. Related Experiment — Active vs Inactive Customers

The following query correctly groups customers by active state:

```sql
SELECT
    active,
    COUNT(*) AS customer_count
FROM sakila.customer
GROUP BY active;
```

This produces conceptually:

```text
active | customer_count
-------+---------------
0      | ...
1      | ...
```

That confirmed an important point:

> `GROUP BY active` was not wrong. It was simply solving a different problem.

If the objective is to display individual customers ordered by active state, use:

```sql
SELECT
    customer_id,
    first_name,
    last_name,
    active
FROM sakila.customer
ORDER BY active DESC;
```

### Key distinction

```text
GROUP BY active
    → collapse rows into groups

ORDER BY active DESC
    → keep individual rows and arrange them
```

---

## 5. Final Query

```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    COUNT(r.rental_id) AS rental_count
FROM sakila.customer AS c
JOIN sakila.rental AS r
    ON r.customer_id = c.customer_id
GROUP BY c.customer_id
HAVING COUNT(r.rental_id) > 30
ORDER BY rental_count DESC;
```

### Verdict

**Correct.**

The only missing requirement in the submitted version was the requested ordering by rental count.

---

## 6. Why the Final Query Works

### Step 1 — Join customers to rentals

```sql
JOIN sakila.rental AS r
    ON r.customer_id = c.customer_id
```

A customer can have many rental records.

Conceptually:

```text
customer
customer_id = 21
      |
      +---- rental
      +---- rental
      +---- rental
      +---- ...
```

### Step 2 — Create one group per customer

```sql
GROUP BY c.customer_id
```

This creates one group containing all rental rows belonging to each customer.

### Step 3 — Count rentals

```sql
COUNT(r.rental_id)
```

Counts the rental records in each customer's group.

### Step 4 — Keep only customers with more than 30

```sql
HAVING COUNT(r.rental_id) > 30
```

`HAVING` filters groups after aggregation.

### Step 5 — Sort the resulting customers

```sql
ORDER BY rental_count DESC
```

Highest rental count first.

---

## 7. Functional Dependency Observation

MySQL accepted:

```sql
GROUP BY c.customer_id
```

while also selecting:

```sql
c.first_name,
c.last_name
```

This is because `customer_id` is the primary key of `customer`.

Conceptually:

```text
customer_id → first_name
customer_id → last_name
```

For one customer ID, there is one corresponding name.

This is different from:

```sql
GROUP BY active
```

because one active value corresponds to many customers.

This is an important `ONLY_FULL_GROUP_BY` and functional-dependency concept to revisit later.

---

## 8. COUNT(rental_id) vs COUNT(*)

For this particular INNER JOIN, these are equivalent:

```sql
COUNT(r.rental_id)
```

and:

```sql
COUNT(*)
```

because every row surviving the INNER JOIN has a non-NULL `rental_id`.

Using:

```sql
COUNT(r.rental_id)
```

also makes the intention explicit:

> Count the rental records.

The distinction becomes important with NULL values and especially with OUTER JOINs.

---

## 9. DBA / Performance Notes

Performance was not the primary focus of Puzzle 001.

However, the query introduces the exact type of workload that will later be used for optimizer and indexing practice:

```text
customer
   |
   | customer_id
   v
rental
   |
   +-- many rows per customer
   |
   v
GROUP BY customer_id
COUNT(...)
HAVING ...
ORDER BY ...
```

Later we should investigate:

- Index on `rental.customer_id`
- Existing Sakila indexes
- Join access path
- Grouping strategy
- Sorting
- `EXPLAIN`
- `EXPLAIN ANALYZE`
- What changes when the rental table becomes very large

Do not optimize this query prematurely. First establish correctness and relational reasoning.

---

## 10. Mistakes to Remember

1. Do not use an aggregate such as `COUNT()` as the grouping concept when the groups have not been defined.
2. `GROUP BY active` is valid when the desired groups are active and inactive customers.
3. `GROUP BY active` cannot directly return arbitrary individual customer attributes from each group.
4. `WHERE` filters rows; `HAVING` filters groups.
5. `GROUP BY` changes the shape of the result; `ORDER BY` changes its ordering.
6. Before using a subquery, inspect what columns and rows the subquery actually returns.
7. `IN` normally compares against a compatible single-column subquery.
8. When counting related records, identify the table that contains the repeated records.
9. `COUNT(column)` ignores NULL values; `COUNT(*)` counts rows.
10. `ONLY_FULL_GROUP_BY` is useful protection against ambiguous grouped queries.

---

## 11. New Questions for Future Puzzles

- When should `WHERE` be preferred over `HAVING`?
- What is the practical difference between `COUNT(*)` and `COUNT(column)`?
- When does `COUNT(*)` differ from `COUNT(DISTINCT column)`?
- Why does an INNER JOIN naturally eliminate customers with no rentals?
- How can a LEFT JOIN be used to find customers with zero rentals?
- Does SQL guarantee row order without `ORDER BY`?
- How does MySQL determine functional dependency?
- Which indexes does Sakila already have on the tables used here?
