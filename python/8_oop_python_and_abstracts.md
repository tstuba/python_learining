# OOP in Python and abstracts

## OOP Essentials (Python backend version)

### Class vs Instance

Class:

- Blueprint/definition of behavior.

Instance:

- A specific object created from a class.

```
class User:
    def __init__(self, name):
        self.name = name

u1 = User("John")
u2 = User("Alice")
```

User → class

u1, u2 → instances

### `__init__` – what does it really do?

This is NOT a constructor in the sense of creating an object.

The object is created in `__new__`.

`__init__`:

- initializes the instance
- sets its state

This is a detail, but it shows a deeper understanding.

### Class attributes vs Instance attributes

```
class User:
    role = "user"  # class attribute

    def __init__(self, name):
        self.name = name  # instance attribute
```

Difference:

- Class attribute → shared
- Instance attribute → unique to the instance

Trap:

```
class A:
    items = []

a1 = A()
a2 = A()

a1.items.append(1)

print(a2.items)  # [1]
```

This is shared between instances.

The correct way:

```
class A:
    def __init__(self):
        self.items = []

a1 = A()
a2 = A()

a1.items.append(1)

print(a2.items)  # []
```

Each instance has its own list.

### Inheritance vs Composition

### Inheritance

```
class Admin(User):
    pass
```

### Composition

```
class Order:
    def __init__(self, user):
        self.user = user
```

“I prefer composition over inheritance when possible, as it leads to more flexible and loosely coupled code.”

### Method Resolution Order (high-level)

MRO specifies the order in which Python searches for methods and attributes in the inheritance hierarchy.

Python needs to know:

- which class to search this method in first
- and if it's not there — where to look next

You can check:

```
ClassName.__mro__
```

You don't need to know the algorithm, but you should know that:

- Python has a specific method search order
- super() uses MRO

Example:

```
class A:
    def hello(self):
        print("A")

class B(A):
    pass

b = B()
b.hello()
```

Python searches:

- in B
- then in A
- then in object

You can see this:

```
B.__mro__

# Output: (B, A, object)
```

### `@property`

Allows you to treat the method as an attribute.

```
class User:
    def __init__(self, first, last):
        self.first = first
        self.last = last

    @property
    def full_name(self):
        return f"{self.first} {self.last}"

# Usage
u.full_name
# instead of
u.full_name()
```

### Encapsulation

Python has no real privacy.

Conventions:

- `_private` → internal
- `__name` → name mangling

But that's not security.

Example encapsulation in python:

```
class User:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if not value:
            raise ValueError("Name cannot be empty")
        self._name = value
```

### Why is OOP important in Django?

Django is an OOP-heavy framework:

- models = classes
- views = classes
- serializers = classes
- mixins
- inheritance (CBV)

You need to understand:

- method overrides
- super()
- MRO

## Abstractions in Python

Abstract base classes define a contract for subclasses. They prevent instantiation of incomplete implementations and make architecture more explicit.

### What is `abc`?

`abc` (Abstract Base Classes) allows you to define classes that:

- cannot be instantiated
- force the implementation of specific methods in inheriting classes

Example:

```
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):

    @abstractmethod
    def process(self, amount):
        pass

class StripeProcessor(PaymentProcessor):
    def process(self, amount):
        print(f"Processing {amount}")

# If you do not implement process():

class BrokenProcessor(PaymentProcessor):
    pass

TypeError: Can't instantiate abstract class...
```

### What does this achieve?

- Enforces contracts
- Prevents partial implementation
- Facilitates maintenance of larger systems

## Interview questions

1️⃣ What is the difference between class and instance attributes?

<details>
<summary>Answer</summary>

Class attributes are shared across all instances of a class, while instance attributes belong to a specific object. Class attributes are defined at the class level, whereas instance attributes are usually defined inside `__init__`. Modifying a mutable class attribute affects all instances.

</details>

2️⃣ When would you use inheritance over composition?

<details>
<summary>Answer</summary>

I use inheritance when there is a clear “is-a” relationship and subclasses extend shared behavior from a base class. It’s useful for polymorphism and structured hierarchies. However, I generally prefer composition for flexibility and looser coupling.

</details>

3️⃣ How does `super()` work?

<details>
<summary>Answer</summary>

`super()` delegates method calls according to the class’s method resolution order.

</details>

4️⃣ Why use @property?

<details>
<summary>Answer</summary>

To expose computed values as attributes while keeping encapsulation.

</details>
