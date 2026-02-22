# ORM optimization

## `values()` vs `values_list()`

Why?

When you don't need model objects, but only data for:

- API response
- report
- export
- simple processing

`values()` → list of dicts

```
qs = User.objects.filter(is_active=True).values("id", "email")
# [{'id': 1, 'email': 'a@x.com'}, ...]
```

`values_list()` → list of tuples / list of values

```
qs = User.objects.values_list("id", "email")
# [(1, 'a@x.com'), ...]

emails = User.objects.values_list("email", flat=True)
# ['a@x.com', ...]
```

✅ Advantage: less overhead than creating model instances.

⚠️ Disadvantage: you lose model methods, properties, etc.

## `only()` / `defer()` (partial loading)

Why?

When a model has many fields (e.g., large text/json) and you want to:

- download only “light” columns
- limit transfer from the DB
- limit memory

`only()` — retrieve only selected fields

```
users = User.objects.only("id", "email")
```

`defer()` — skip heavy fields

```
users = User.objects.defer("profile_blob")
```

## Bulk operations (`bulk_create`, `bulk_update`)

Why?

When you're doing mass operations — much faster than `save()` in a loop.

`bulk_create`

```
User.objects.bulk_create([
    User(email="a@x.com"),
    User(email="b@x.com"),
])
```

`bulk_update`

```
for u in users:
    u.is_active = False
User.objects.bulk_update(users, ["is_active"])
```

✅ Benefit: fewer queries → significant performance boost.
⚠️ Trade-off (important to mention in the interview):

- usually bypasses `save()`
- may not trigger signals
- does not run full per-object logic as in a loop
- validation (clean/full_clean) and side effects are your responsibility

## Summary

I use `values()` / `values_list()` when I only need raw data and want to avoid model instantiation overhead. I use `only()` / `defer()` to reduce transferred columns, but I’m careful because accessing deferred fields later can trigger extra queries. For bulk inserts/updates I use `bulk_create` and `bulk_update` to reduce query count, knowing they may bypass per-object hooks like `save()` or signals.
