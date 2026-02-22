# Django Transactions + `transaction.atomic`

## What is transaction?

Transaction is a group of operations on databse, that can be executed all or none.

That means:

- everything will be save
- or everything will be withdrawn (rollback)

## Problem without transactions

```
def create_order():
    order = Order.objects.create(...)
    Payment.objects.create(...)
```

If:

- `Order` will be saved correctly
- `Payment` will throm exception

You have save `Order` without `Payment`.
Data integrity is violated.

### Solution?

```
from django.db import transaction

def create_order():
    with transaction.atomic():
        order = Order.objects.create(...)
        Payment.objects.create(...)
```

Now:

- everything is ok -> commit
- something throws exception -> rollback

## How that works?

`transaction.atomic()` is a context manager.

Underneith:

- starts a transaction
- if exception -> rollback
- if no exception -> commit

### What if you catch exception?

```
with transaction.atomic():
    try:
        do_something()
    except Exception:
        pass
```

We won't have rollback here, because `Exception` was caught

Rollback works only if `Exception` reaches out of the block of code.

## Nested transactions

```
with transaction.atomic():
    ...
    with transaction.atomic():
        ...
```

They are workin as a savepoints.

- in side rollback reverse only to nearest `transaction.atomic()`
- external `transaction.atomic()` controlls commit

## Where to use `atomic`

In most cases:

- view
- service layer
- with operations involving multiple models
- with changes dependant on data

## Summary

I use `transaction.atomic()` when multiple database operations must succeed or fail together. If an exception occurs inside the block, Django rolls back the transaction automatically.

## Interview questions

1️⃣ What happens if an exception occurs inside `atomic`?

<details>
<summary>Answer</summary>

The transaction is rolled back.

</details>

2️⃣ What if you catch the exception?

<details>
<summary>Answer</summary>

No rollback unless you re-raise it.

</details>

3️⃣ Is every Django request wrapped in a transaction?

<details>
<summary>Answer</summary>

Not always.

You can turn it on: `ATOMIC_REQUESTS = True`

But by default: No.

</details>
