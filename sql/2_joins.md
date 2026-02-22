# JON (INNER vs. LEFT vs. RIGHT)

Let's assume tables:

Users:

| id  | username |
| --- | -------- |
| 1   | John     |
| 2   | Alice    |
| 3   | Bob      |

Orders:

| id  | user_id | amount |
| --- | ------- | ------ |
| 1   | 1       | 100    |
| 2   | 1       | 200    |
| 3   | 2       | 300    |

## INNER JOIN

```
SELECT u.username, o.amount
FROM users u
INNER JOIN orders o
ON u.id = o.user_id;
```

Returns only records that match on both sides.

| username | amount |
| -------- | ------ |
| John     | 100    |
| John     | 200    |
| Alice    | 300    |

## LEFT JOIN

```
SELECT u.username, o.amount
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id;
```

All records from the left table (users),
and if there is no match → NULL on the right.

| username | amount |
| -------- | ------ |
| John     | 100    |
| John     | 200    |
| Alice    | 300    |
| Bob      | NULL   |

## RIGHT JOIN

```
SELECT u.username, o.amount
FROM users u
RIGHT JOIN orders o
ON u.id = o.user_id;
```

Returns all records from the right table (orders).

| username | amount |
| -------- | ------ |
| John     | 100    |
| John     | 200    |
| Alice    | 300    |

## Differences

| JOIN  | What returns?                                 |
| ----- | --------------------------------------------- |
| INNER | only matching                                 |
| LEFT  | everything on the left + matched on the right |
| RIGHT | all on the right + aligned on the left        |

## How it releates to Django

`select_related()` uses `INNER JOIN` be default.

reverse relations / optional FK depending on configuration can use `LEFT JOIN`

## JOIN + WHERE

```
SELECT *
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.amount > 100;
```

This removes NULLs and works like an `INNER JOIN`.

Why?

Because `WHERE` is filtering records that are returned by `JOIN`.

Right way:

```
SELECT *
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id AND o.amount > 100;
```

## Summary

`INNER JOIN` returns only matching records, while `LEFT JOIN` returns all rows from the left table and NULLs where no match exists. `RIGHT JOIN` behaves symmetrically but is less commonly used.
