# SQL basics

## SELECT

Basic form:

```
SELECT * FROM users;
```

Better practice:

```
SELECT id, username FROM users;
```

### WHERE

```
SELECT * FROM users
WHERE is_active = TRUE;
```

### ORDER BY

```
SELECT * FROM users
ORDER BY created_at DESC;
```

### LIMIT / OFFSET

```
SELECT * FROM users
LIMIT 10 OFFSET 20;
```

## INSERT (CREATE)

```
INSERT INTO users (username, email)
VALUES ('john', 'john@example.com');
```

## UPDATE

```
UPDATE users
SET email = 'new@example.com'
WHERE id = 1;
```

Without `WHERE`, you updates everything!!!

## DELETE

```
DELETE FROM users
WHERE id = 1;
```

Without `WHERE`, you deletes everything!!!

## Aggregating functions

### COUNT

```
SELECT COUNT(*) FROM users;
```

### SUM

```
SELECT SUM(price) FROM orders;
```

### AVG

```
SELECT AVG(price) FROM orders;
```

### MIN/MAX

```
SELECT MAX(price) FROM orders;
```

## GROUP BY (introduction)

```
SELECT author_id, COUNT(*)
FROM books
GROUP BY author_id;
```

It:

- groups data
- allows to us aggregation per group
