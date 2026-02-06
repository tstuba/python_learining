## Data types

### Mutable vs Immutable

A type is mutable if its state can be changed without creating a new object (i.e. without changing its identity in memory).

Immutable example:

```
a = (1, 2, 3)
b = a

id(a) == id(b) # True
# now both b and a are pointing the same object

b += (4,) # new object is created
# a  = (1, 2, 3)
# b  = (1, 2, 3, 4)

id(a) != id(b) # True
# now they are pointing two different objects
```

In case of immutable types, operations like `+=` create a new object
because the original object cannot be modified in place.

Mutable example:

```
a = [1, 2, 3]
b = a

id(a) == id(b) # True
# now both b and a are pointing the same object

b += [4] # the original object is modified
# a  = [1, 2, 3, 4]
# b  = [1, 2, 3, 4]

id(a) != id(b) # False
# they are still pointing at the same object
```

Mutable types: `list`, `set`, `dict`

Immutable types: `tuple`, `str`, `int`, `float`, `bool`

### List vs. Tuple vs. Set vs. Dict

| Type  | Ordered | Mutable | Indexed | Hash-based |
| ----- | ------- | ------- | ------- | ---------- |
| List  | ✅      | ✅      | ✅      | ❌         |
| Tuple | ✅      | ❌      | ✅      | ❌         |
| Set   | ❌\*    | ✅      | ❌      | ✅         |
| Dict  | ✅\*\*  | ✅      | ❌      | ✅         |

\* order is not guaranteed

\*\* it is ordered from Python 3.7 and above

**Ordered** means that, when iterated, items are in the same order as they were inserted.

**Indexed** means that we can retrieve item by integer index in O(1) time and that it supports `[]` operator.

**Hash-based** means that the collection uses a hash table internally. Elements (or keys) are mapped to buckets using their hash value, which allows average O(1) lookup time, at the cost of additional memory usage. Hash-based collections require elements (or keys) to be hashable.

In a list, membership checks require a linear scan — Python compares the searched value with each element until it finds a match, which is O(n).

In a set, the hash of the element is calculated and used to locate the appropriate bucket in the hash table, so Python only checks a small subset of elements, giving average O(1) lookup time.

Only hashable objects can be used as set elements or dict keys. An object is hashable if its hash value does not change during its lifetime. This is why mutable types like `list` cannot be used as dict keys.

| Operation               | list   | tuple | set    | dict   |
| ----------------------- | ------ | ----- | ------ | ------ |
| Membership check (`in`) | O(n)   | O(n)  | O(1)   | O(1)   |
| Access by index         | O(1)   | O(1)  | ❌     | ❌     |
| Access by key           | ❌     | ❌    | ❌     | O(1)   |
| Append / add            | O(1)\* | ❌    | O(1)\* | O(1)\* |
| Remove element          | O(n)   | ❌    | O(1)   | O(1)   |
| Iterate all             | O(n)   | O(n)  | O(n)   | O(n)   |

\* amortized (resize may occur)

Summary:

- list / tuple → fast index access, slow membership
- set / dict → fast membership / lookup, higher memory usage
- hash-based structures trade memory for speed
