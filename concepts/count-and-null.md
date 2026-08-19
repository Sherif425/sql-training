# Concept — COUNT(*) vs COUNT(column)

## COUNT(*)

Counts rows.

```sql
COUNT(*)
```

does not require a particular column to be non-NULL.

## COUNT(column)

Counts non-NULL values in the specified column.

```sql
COUNT(r.rental_id)
```

counts rows where `rental_id` is not NULL.

## Puzzle 001

Because Puzzle 001 used an INNER JOIN and `rental_id` is present for every matching rental row:

```sql
COUNT(r.rental_id)
```

and:

```sql
COUNT(*)
```

produce the same result.

## Why This Will Matter Later

The distinction becomes especially important with OUTER JOINs.

For example, when looking for customers with zero rentals, a LEFT JOIN can produce a NULL rental side for customers without matches.

That makes the choice between:

```sql
COUNT(*)
```

and:

```sql
COUNT(r.rental_id)
```

important.

This will be explored in a future puzzle.
