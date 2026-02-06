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
