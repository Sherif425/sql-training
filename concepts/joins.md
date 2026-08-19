# Concept — JOINs

## Core Idea

A JOIN combines rows from related tables according to a join condition.

Puzzle 001 used:

```sql
FROM sakila.customer AS c
JOIN sakila.rental AS r
    ON r.customer_id = c.customer_id
```

This is an INNER JOIN.

## Why the JOIN Was Necessary

`customer` tells us:

- customer ID
- first name
- last name

`rental` contains one row per rental transaction.

Therefore, to calculate the number of rentals per customer, we need both tables.

## Relationship

Conceptually:

```text
customer
customer_id
    |
    | one customer
    | many rentals
    v
rental
customer_id
rental_id
```

## INNER JOIN

An INNER JOIN keeps rows that have matching records on both sides.

Therefore, a customer with no rental has no matching rental row and does not appear in Puzzle 001.

This observation will become the basis of Puzzle 002:

> Find customers who have never made a rental.

That puzzle will introduce the need to reason about JOIN type.

## DBA Connection

Join performance depends on factors including:

- indexes
- cardinality
- join order
- access paths
- optimizer decisions

These will become explicit topics later.
