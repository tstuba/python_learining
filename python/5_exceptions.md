# Exceptions

## What is an exception?

An exception is an object that:

- interrupts the normal flow of the program
- propagates up the call stack
- can be caught

In Python:

- everything is an object
- exceptions inherit from BaseException
- most often from Exception

Example structure:

```
try:
    risky_operation()
except ValueError:
    handle_error()
else:
    do_if_no_exception()
finally:
    always_execute()
```

What does each part do?

- try → code that may throw an exception
- except → handling of a specific exception
- else → will only be executed if there was NO exception
- finally → will always be executed

## Exception propagation

If the exception is not caught:

```
def a():
    b()

def b():
    raise ValueError("Boom")

a()
```

- exception “goes up”
- until it is caught
- or the program ends

This is called stack unwinding.

## Throwing an exception

```
raise ValueError("Invalid input")
```

or:

```
# This re-throws the current exception (in except).
try:
    risky_operation()
except ValueError:
    raise
```

## Custom exceptions

```
class InvalidUserError(Exception):
    pass

# usage

if not user:
    raise InvalidUserError("User not found")
```

## When to catch exceptions, and when NOT to?

Don't do this:

```
try:
    do_something()
except Exception:
    pass
```

It:

- masks errors
- makes debugging difficult

Better:

- catch specific exceptions
- log them
- re-raise if necessary

Example:

```
try:
    process_payment()
except PaymentError as e:
    logger.error("Payment failed", exc_info=True)
    raise
```

It:

- re-throws the same exception
- does not hide the error
- preserves the ORIGINAL stack trace
- provides context
- this is the correct way to re-raise an exception.

```
try:
    do_something()
except Exception as e:
    raise e
```

What does it do?

- throws the exception again
- BUT resets the stack trace
- the traceback starts from this line
- this can make debugging difficult.

## Exceptions in Django

In Django:

- exceptions are often converted to HTTP responses,
- for example, Http404.
- DRF has ValidationError.

You need to understand:

- exception ≠ application crash.
- It is often a flow control mechanism.

## Interview questions

1️⃣ What is the difference between `except Exception` and `except BaseException`?

<details>
<summary>Answer</summary>

- Exception → normal błędy
- BaseException → also includes KeyboardInterrupt, SystemExit

You usually catch Exception, not BaseException.

</details>

2️⃣ What happens if an exception is raised inside `finally`?

<details>
<summary>Answer</summary>

- overwrites the previous exception
- may make debugging difficult
</details>

3️⃣ When would you use `else`?

<details>
<summary>Answer</summary>
When you want to separate the “success path” code from the try
</details>
