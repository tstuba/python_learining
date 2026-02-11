# yield vs. return

## return

Classic end of function

```
def get_numbers():
    return [1, 2, 3]
```

What is happening here?

- the function is performed in its entirety
- creates a list in memory
- returns the finished object
- the function ends
- everything happens at once

## yeild - generator

If a function contains yield, it becomes a generator function and returns a generator object instead of executing immediately.

```
def get_numbers():
    yield 1
    yield 2
    yield 3
```

What is happening here?

- the function does not return a list
- returns the object generator
- the code is executed step by step
- the state of the function is stored

### What does “maintains its condition” mean?

```
def counter():
    print("start")
    yield 1
    print("middle")
    yield 2
    print("end")

gen = counter()
next(gen)
```

Shell will print `start`, next call of next(gen) will print `middle`.
The function “remembers” where it was.

### Why is yield memory-efficient?

Examples:

```
def read_lines():
    for i in range(1_000_000):
        yield i
```

```
def read_numbers(limit):
    print("Generator started")
    for i in range(limit):
        yield i
    print("Generator finished")

gen = read_numbers(3)

for number in gen:
    print(number)

# Output:
# Generator started
# 0
# 1
# 2
# Generator finished
```

- You don't create a list of a million items
- You generate them on demand

This is a huge difference when it comes to:

- files
- CSV export
- batch processing

### When to use yield?

- large data sets
- streaming
- batch processing
- pipelines

### When NOT to use yield?

When you need:

- random access
- multiple iterations
- the entire collection
- when the code should be as simple as possible

## Key differences

| Feature         | return | yield     |
| --------------- | ------ | --------- |
| Ends function   | ✅     | ❌        |
| Returns         | value  | generator |
| Lazy evaluation | ❌     | ✅        |
| Memory usage    | bigger | lesser    |
| Keeps the state | ❌     | ✅        |

## Interview questions

1️⃣ What does a function with `yield` return?

<details>
<summary>Answer</summary>
A generator object.
</details>

2️⃣ When does the code inside a generator execute?

<details>
<summary>Answer</summary>
When iterated over, not when defined.
</details>

3️⃣ Why are generators memory efficient?

<details>
<summary>Answer</summary>
They produce values on demand instead of storing everything in memory.</details>
