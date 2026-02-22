# Django ORM - Fundamentals

## What is a `QuerySet`?

```
users = User.objects.filter(is_active=True)
```

This is not a list.
This is a `QuerySet` object. It is representing query.

It builds SQL, BUT DON'T EXECUTE IT.

When query is executed?

When it is evaluated.

Examples:

```
users = User.objects.filter(is_active=True)

list(users)
for u in users:
len(users)
bool(users)
users[0]
users.exists()
```

It allows you to build queries in phases.

For example:

```
qs = User.objects.all()
if something:
    qs = qs.filter(is_active=True)
qs = qs.order_by("username")
```

It allows you to:

- chain
- optimaize
- don't execute useless queries

## TRAP

```
users = User.objects.filter(is_active=True)

if users:
    print("We have users")
```

It executes query with `SELECT *`

Instead of `if users:` do `if users.exists()`, it uses `SELECT 1`.

## `QuerySet` caching

After first evaluation:

```
for u in users:
    ...

for u in users:
    ...
```

Secound iteration don't execute query.

BUT:

```
users = User.objects.filter(...)
users = users.filter(...)
```

Creates new `QuerySet`.

## How to get actual `SQL`?

```
qs = User.objects.filter(is_active=True)
print(qs.query)
```

It does not execute query, it only shows SQL.

```
qs = User.objects.filter(is_active=True)
print(qs.explain())
```

It executes query, shows plan of execute.

Allows you to analyze indexes and performance.

```
from django.db import connection

list(qs)  # wymuszenie query
print(connection.queries)
```

WORKS ONLY IN `DEBUG=True`.

It shows all executed queries and their execution time.

## Summary

Django `QuerySets` are lazily evaluated. Database queries are executed only when the `QuerySet` is evaluated, such as during iteration or when calling functions like `list()`, `len()`, or `exists()`.

## Interview questions

1️⃣ When does a `QuerySet` hit the database?

<details>
<summary>Answer</summary>

A `QuerySet` hits the database only when it is evaluated. This happens during iteration, when calling `list()`, `len()`, `bool()`, `exists()`, or accessing elements by index. Until then, it only builds the SQL query lazily.

</details>

2️⃣ Why is lazy evaluation useful?

<details>
<summary>Answer</summary>

Lazy evaluation allows queries to be constructed step by step without immediately hitting the database. This enables query chaining, better performance optimization, and avoids unnecessary database calls.

</details>

3️⃣ What is the difference between `exists()` and `count()`?

<details>
<summary>Answer</summary>

`exists()` checks whether at least one record matches the query and generates a lightweight query. `count()` returns the total number of matching records using `COUNT(*)`, which can be more expensive. If you only need to check existence, `exists()` is more efficient.

</details>
