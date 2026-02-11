# \*args vs. \*\*kwargs

### Basic information

\*args collects positional arguments into a tuple, and \*\*kwargs collects keyword arguments into a dict.

Example function definition:

```
def f(a, b, *args, c=1, **kwargs):
    pass
```

Right order is:

1. Positional arguments,
2. `*args`,
3. Named arguments with default value,
4. `**kwargs`,

Different order would end up with `SyntaxError`

Function declaration:

```
f(1, 2, 3, 4, c=10, x=99)
# a = 1
# b = 2
# args = (3, 4)
# c = 10
# kwargs = {"x": 99}
```

### Unpacking

Tuple/List:

```
values = [1, 2, 3]
f(*values)

# it is the same as:

f(1, 2, 3)
```

Dict:

```
data = {"a": 1, "b": 2}
f(**data)

# keys must match parameters names
```

### When to use \*args \*\*kwargs

- wrappers
- decorators
- argument forwarding
- APIs with a variable number of parameters

### When not to use \*args \*\*kwargs

- when we have function contract defined
- business code
- as a way to avoid proper API design

\*args and \*\*kwargs are great for flexibility, but explicit parameters make APIs safer.

## Interview questions

1️⃣ What is the difference between \*args and \*\*kwargs, and what data types do they become inside the function?

(follow-up: can \*args ever be a list? why?)

<details>
<summary>Answer</summary>
TBD
</details>

2️⃣ What is the correct order of parameters in a function definition that uses \*args and \*\*kwargs, and why does Python enforce this order?

(follow-up: what error do you get if the order is wrong?)

<details>
<summary>Answer</summary>
TBD
</details>

3️⃣ How does argument unpacking work with \* and \*\*, and what happens if the provided arguments do not match the function signature?

(follow-up: what happens if \*\*kwargs contains an unexpected key?)

<details>
<summary>Answer</summary>
TBD
</details>

4️⃣ When is it a good idea to use \*args and \*\*kwargs, and when should you avoid them?

(follow-up: why are they common in decorators and wrappers?)

<details>
<summary>Answer</summary>
TBD
</details>

5️⃣ What are the risks of defining a function as def f(\*args, \*\*kwargs): and why is this often considered an anti-pattern?

(follow-up: how does this affect readability and API contracts?)

<details>
<summary>Answer</summary>
TBD
</details>
