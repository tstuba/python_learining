# Context Managers

## Why do they exist?

Context managers are used to manage resources in a secure manner - automatic cleanup

Resources:

- files
- DB connections
- locks
- transactions
- sessions

## Problem without context manager

```
file = open("data.txt")
data = file.read()
file.close()
```

- what if `read()` throws an exception?
- `close()` will not be executed
- resource leak

### Classic pattern with try/finally

```
file = open("data.txt")
try:
    data = file.read()
finally:
    file.close()
```

It works. But a context manager can do it for you.

```
with open("data.txt") as file:
    data = file.read()
```

Under the hood, Python does something like this:

```
manager = open("data.txt")
value = manager.__enter__()
try:
    data = value.read()
finally:
    manager.__exit__()
```

## What must a context manager have?

Two methods:

- `__enter__()`
- `__exit__()`

## Custom context manager (class)

```
class MyContext:
    def __enter__(self):
        print("Enter")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Exit")
```

Usage:

```
with MyContext():
    print("Inside")
```

Output:

```
Enter
Inside
Exit
```

### What does `__exit__` get?

Parameters:

- exc_type
- exc_value
- traceback

If there was an exception inside the block:

- exc_type ≠ None

You can:

- handle the exception
- return True to “swallow” it

## Context manager by contextlib

```
from contextlib import contextmanager

@contextmanager
def my_context():
    print("Enter")
    try:
        yield
    finally:
        print("Exit")
```

- code before yield = `__enter__`
- code after yield = `__exit__`

This connects the topic with `yield`.

## Link to Django topic

Transactions:

```
from django.db import transaction

with transaction.atomic():
    create_user()
    create_profile()
```

If exception:

- rollback
- no commit

This is context manager in practice.

## Locks / connections

In systems:

- Redis lock
- DB connection pool
- file streaming

## Interview questions

1️⃣ What problem do context managers solve?

<details>
<summary>Answer</summary>

They guarantee resource cleanup even if an exception occurs.

</details>

2️⃣ What methods define a context manager?

<details>
<summary>Answer</summary>

`__enter__` and `__exit__`.

</details>

3️⃣ How is `with` related to `try/finally`?

<details>
<summary>Answer</summary>

The with statement ensures that setup and cleanup code are executed in a controlled way. Internally, it guarantees that the cleanup logic runs regardless of whether an exception occurs, similarly to how a `try/finally` block works.

</details>

4️⃣ How are context managers used in Django?

<details>
<summary>Answer</summary>
For example, `transaction.atomic()` ensures rollback on exceptions.
</details>
