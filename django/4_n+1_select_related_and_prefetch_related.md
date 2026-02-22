# N+1 Problem

## What is N+1 problem

It is a situation when:

- you are executing one query
- and then you are executing antoher query for each result you get

So:

- 1 query + N additional queries = N+1

## Classic Example

```
class Author(models.Model):
    name = models.CharField(...)

class Book(models.Model):
    title = models.CharField(...)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)

books = Book.objects.all()

for book in books:
    print(book.author.name)
```

### What is happening here?

Query: `SELECT * FROM book;`

And then for each book: `SELECT * FROM author WHERE id = ...`

So if you have 100 books:

- 1 + 100 queries
- terible performance

## How to fix that?

### `select_related()`

It uses `JOIN` while building query.

```
class Author(models.Model):
    name = models.CharField(...)

class Book(models.Model):
    title = models.CharField(...)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)

books = Book.objects.select_related("author")

for book in books:
    print(book.author.name)

```

### What is happening here?

One query:

```
SELECT book.*, author.*
FROM book
JOIN author ON book.author_id = author.id;
```

So for 100 books instead of 101 queries we have 1 query.

Huge performance gains!

### When to use `select_related()`?

For:

- ForeignKey
- OneToOne

### `prefetch_related()`

This is for ManyToMany relations.

```
books = Book.objects.prefetch_related("tags")
```

It:

- executes 2 queries
- joins data in python

Why not `JOIN`?

Because, with ManyToMany `JOIN` can explode with count of lines.

### `select_related()` vs `prefetch_related()`

|               | `select_related()`   | `prefetch_related()`           |
| ------------- | -------------------- | ------------------------------ |
| Relation type | ForeignKey, OneToOne | ManyToMany, reverse ForeignKey |
| Queries count | 1                    | 2                              |
| `JOIN`        | ✅                   | ❌                             |
| data joins    | `SQL`                | `Python`                       |

## Summary

The N+1 problem occurs when one query retrieves a set of objects, and additional queries are executed for each related object. It can be solved using `select_related()` for foreign keys and `prefetch_related()` for many-to-many or reverse relationships.
