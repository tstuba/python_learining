# Default arguments

### Key rule

Default arguments are evaluated once — at function definition time, not at call time.

### Why mutable types are bad for default arguments?

- list
- dict
- set
- custom mutable object

```
def add_item(item, items=[]):
    items.append(item)
    return items

print(add_item(1))  # [1]
print(add_item(2))  # [1, 2]
print(add_item(3))  # [1, 2, 3]
```

Why does it happening?
List `[]` was created once with function definition, and it is shared bettwen function declarations.

Python is working this way because:

- function is an obejct
- deafult values are stored in `add_item.__defaults__`, this is tuple with dafault values, they are not created dynamically with every declaration because it would be slower and less predictable.

### Correct pattern

```
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

Why?
Because:

- None is immutable
- new list is created within function
- every function gets fresh object

Why we are using `None`?
Because:

- it clearly means “no argument was provided”
- it is not coliding with true value
- this is a python convention

“I use None as a sentinel value to avoid mutable default arguments.”

### Is mutable default always wrong?

Theoretically not. We can use it for `cache` or `sharing state` but during the interview it is better to say:

- this is bad pattern
- pattern with None is better

## Interview questions

1️⃣ Why are mutable default arguments dangerous?

<details>
<summary>Answer</summary>
Because default arguments are evaluated once at function definition time, so mutable defaults are shared between function calls.
</details>

2️⃣ When are default arguments evaluated?

<details>
<summary>Answer</summary>
At function definition time.
</details>

3️⃣ How do you fix mutable default argument issues?

<details>
<summary>Answer</summary>
Use None as a sentinel and initialize inside the function.
</details>
