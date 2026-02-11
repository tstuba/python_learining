# Deep copy vs. Shallow copy

## Problem: references, not values

In Python, variables store references to objects, not the objects themselves.

```
a = [1, 2, 3]
b = a
```

These are NOT two lists.
These are two references to the same object.

## Shallow Copy

Creates:

- a new external object
- BUT references to the same internal objects

Example:

```
import copy

original = [[1, 2], [3, 4]]
shallow = copy.copy(original)

original is shallow          # False
original[0] is shallow[0]    # True
```

That is:

- new external list
- internal lists remain the same

Side effect:

```
original[0].append(99)

print(original)
# Output: [[1, 2, 99], [3, 4]]

print(shallow)
# Output: [[1, 2, 99], [3, 4]]
```

The change is “leaking.”

## Deep Copy

Creates:

- a new object
- and copies all nested objects

```
import copy

original = [[1, 2], [3, 4]]

deep = copy.deepcopy(original)

original is deep          # False
original[0] is deep[0]    # False
```

## Summary

- Shallow copy copies the outer object, but keeps references to nested objects.
- Deep copy recursively copies all nested objects.

## Why does this matter in Django?

Examples:

- copying `request.data`
- manipulating configs
- copying JSON structures
- operations on nested dictionaries

Errors:

- you modify the structure
- the state changes elsewhere in the system

These are real production bugs.

## What is shallow by default?

- `list.copy()`
- `dict.copy()`
- slicing `[:]`
- `copy.copy()`

## Mental model (most important)

Imagine a tree:

- shallow → you copy the root
- deep → you copy the entire tree

“Shallow copy duplicates the container, deep copy duplicates the entire object graph.”

## Interview questions

1️⃣ What is the difference between shallow and deep copy?

<details>
<summary>Answer</summary>

Shallow copy copies the outer object, but keeps references to nested objects. Deep copy recursively copies all nested objects.

</details>

2️⃣ When would shallow copy be enough?

<details>
<summary>Answer</summary>

- when the object has no nested mutables
- when nested objects are immutable

</details>

3️⃣ Is deep copy always safe?

<details>
<summary>Answer</summary>

Not always:

- it can be expensive
- it may not work correctly with custom objects
- it may copy something you don't want

</details>
