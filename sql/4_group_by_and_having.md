# `GROUP BY` + `HAVING`

## `GROUP BY`

```
SELECT COUNT(*) FROM orders;
```

→ total number of orders.

```
SELECT user_id, COUNT(*)
FROM orders
GROUP BY user_id;
```

→ number of orders per user.

### Example

Orders:

| id  | user_id | amount |
| --- | ------- | ------ |
| 1   | 1       | 100    |
| 2   | 1       | 200    |
| 3   | 2       | 300    |
| 4   | 2       | 400    |
| 5   | 3       | 150    |

```
SELECT user_id, COUNT(*)
FROM orders
GROUP BY user_id;
```

| user_id | count |
| ------- | ----- |
| 1       | 2     |
| 2       | 2     |
| 3       | 1     |

If you use `GROUP BY`:

- Each column in `SELECT` must be either in `GROUP BY` or aggregated.

## HAVING vs. WHERE

### WHERE

- filters before grouping

### HAVING

- filters after aggregation

### Example

```
SELECT user_id, COUNT(*)
FROM orders
GROUP BY user_id
HAVING COUNT(*) > 1;
```

| user_id | count |
| ------- | ----- |
| 1       | 2     |
| 2       | 2     |

### Trap: WHERE vs. HAVING

```
WHERE COUNT(*) > 1
```

WON'T WORK!!!

Because `COUNT` only works after `GROUP BY`.

## GROUP BY + JOIN

```
SELECT u.username, COUNT(o.id)
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.username;
```

This gives the number of orders per user.

## GROUP BY + SUM

```
SELECT user_id, SUM(amount)
FROM orders
GROUP BY user_id;
```

Total expenditure per user.

## How this works in Django?

```
from django.db.models import Count

User.objects.annotate(order_count=Count("order"))
```

This generates `GROUP BY` under the hood.

```
User.objects.annotate(
    order_count=Count("order")
).filter(order_count__gt=1)
```

To generuje `HAVING` pod maską.
