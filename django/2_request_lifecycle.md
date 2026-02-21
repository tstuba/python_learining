# Request Lifecycle

## 1. Request gets to the server

Django gets object as `HttpRequest`.

## 2. Middleware

Each request goes through the middleware in the order from `settings.py`.

Middleware can:

- modify request
- block request
- return response without calling view

Examples:

- `AuthenticationMiddleware`
- `SessionMiddleware`
- `CSRF`

## 3. URL Resolver

Django checks `urls.py`

```
urlpatterns = [
    path("users/", users_view),
]
```

Resolver:

- matches URLs
- gets right view
- passes parameters

If there is no match, returns 404.

## 4. View

View:

- gets request
- handels logic
- usually uses ORM
- returns `HttpResponse`

In DRF:

- serialization
- validation
- returns JSON response

## 5. Response goes through middleware (response phase)

Middleware now works in revers order.

Middleware can:

- add headers
- change response
- set cookies

## 6. Response gets back to client

Django return response to WSGI/ASGI server, server returns response to client.

## Summary

A request enters Django through WSGI/ASGI, passes through middleware, is routed by the URL resolver to a view, which may interact with the ORM, and the response then travels back through middleware before being returned to the client.

## Interview questions

1️⃣ Where does middleware run?

<details>
<summary>Answer</summary>

Before and after the view.

</details>

2️⃣ Can middleware return a response without calling a view?

<details>
<summary>Answer</summary>

Yes.

</details>

3️⃣ Where would you handle transactions?

<details>
<summary>Answer</summary>

Typically in the view or service layer, often using `transaction.atomic()`.

</details>
