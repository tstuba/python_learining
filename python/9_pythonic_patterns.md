# Pythonic Patterns (must-know)

## List / Dict Comprehensions

Instead of:

```
result = []
for x in data:
    if x > 0:
        result.append(x * 2)
```

Do:

```
result = [x * 2 for x in data if x > 0]
```

- shorter
- clearer

## Truthy / Falsy

Instead of:

```
if len(items) > 0:
```

Do:

```
if items:
```

All of thoes means `False`:

- None
- 0
- ""
- []
- {}

## is vs. ==

- == → compares values
- is → compares identity (the same object in memory)

Instead of:

```
if x == None:
```

Do:

```
if x is None:
```

## Unpacking

Example:

```
a, b = (1, 2)
# a = 1, b = 2

first, *rest = [1, 2, 3, 4]
# first = 1, rest = [2, 3, 4]
```

## Python prefers EAFP

(Easier to Ask Forgiveness than Permission)

## Interview questions

1️⃣ What is the difference between `is` and `==`?

<details>
<summary>Answer</summary>

`==` compares values, while `is` compares object identity (whether two variables point to the same object in memory). In Python, `is` is typically used when comparing to `None`.

</details>

2️⃣ What does EAFP mean in Python?

<details>
<summary>Answer</summary>

EAFP stands for “Easier to Ask Forgiveness than Permission.” It means you try an operation and handle exceptions if they occur, instead of checking conditions beforehand. This is considered a more Pythonic style.

</details>

3️⃣ Why are list comprehensions preferred over loops in Python?

<details>
<summary>Answer</summary>

List comprehensions are more concise and expressive. They often improve readability when transforming or filtering data, while still being clear and efficient.

</details>
