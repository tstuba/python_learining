# Indexes

Index is a data structure (usually a B-tree) that allows faster searching of records in a table.

Without index database performs a full table scan.

With index databse searches the structured data.

Without:

```
SELECT * FROM users WHERE email = 'john@example.com';
```

the database checks each reacord - O(n)

With index:

```
CREATE INDEX idx_users_email ON users(email);
```

Now:

- databse uses B-tree
- searches logarithmically

O(log n)

Index helps with:

- `WHERE`
- `JOIN`
- `ORDER BY`
- `GROUP BY`

Example:

```
WHERE email = ...
WHERE created_at > ...
JOIN orders ON users.id = orders.user_id
ORDER BY created_at
```

Index:

- speeds up `SELECT`
- slows down `INSERT` / `UPDATE` / `DELETE`

Why?

With every change we need to update index.

## Composite index

```
CREATE INDEX idx_users_email_created
ON users(email, created_at);
```

Works well for:

```
WHERE email = ... AND created_at > ...
```

But not necessarily for `created_at` itself.

Order matters.

## How to check if an index is being used?

```
EXPLAIN SELECT ...
```

- Index Scan → good
- Seq Scan → full table scan

## How it looks in Django?

```
class User(models.Model):
    email = models.EmailField(db_index=True)
```

Or:

```
class Meta:
    indexes = [
        models.Index(fields=["email", "created_at"]),
    ]
```

## Summary

Indexes improve read performance by reducing full table scans, but they increase write overhead because the index must be updated on every insert or update.
