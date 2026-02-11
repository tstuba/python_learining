# Django – General Structure and Fundamentals

## Project vs. Application

### Project

- configuration
- settings
- URL root
- WSGI/ASGI entry point

Command:

```
django-admin startproject config
```

### Application

- specific functionality
- e.g., users, payments, blog

Command:

```
python manage.py startapp users
```

The project may have many applications.

## Typical project structure

```
project/
│
├── manage.py
├── config/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── users/
│   ├── models.py
│   ├── views.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations/
│   └── tests.py
```

### What does `manage.py` do?

- CLI for Django
- starting the server
- migrations
- shell
- tests

### `settings.py` — what's important and what it does

- `INSTALLED_APPS` - it registers Django apps so their models, migrations, and configurations are recognized by the framework.
- `MIDDLEWARE` - Middleware processes requests and responses globally, before and after the view logic.

- `DATABASES` - It defines database connections used by Django ORM.
- `DEBUG` - True → developer mode, False → production

  When DEBUG=True:
  - detailed errors
  - stack trace in the browser
  - fewer restrictions

  In production, it MUST be False.

- `ALLOWED_HOSTS` - List of domains that the application accepts. If a request comes from a host not on the list → Django will reject it (SecurityError).
- `TEMPLATES` - Template system configuration.

  Defines:
  - where Django looks for templates
  - whether in apps (APP_DIRS=True)
  - which context processors are used

- `STATIC_URL` - Defines the URL for static files.

## URL routing

In `urls.py`:

```
urlpatterns = [
    path("users/", include("users.urls")),
]
```

Flow:

- Request → URL resolver → view

## `models.py`

You define:

- DB structure
- relationships
- model methods

Each model = table in DB.

## `migrations/`

Django generates migrations

- migrations update the DB schema
- model change ≠ DB change without migration

## `admin.py`

- model registration
- auto admin panel

## Interview questions

1️⃣ What is the difference between a Django project and an app?

<details>
<summary>Answer</summary>

A Django project is the overall configuration container that manages settings, URLs, and global configuration. An app is a self-contained module that implements specific functionality, such as users or payments. A project can contain multiple apps.

</details>

2️⃣ What does `INSTALLED_APPS` do?

<details>
<summary>Answer</summary>

`INSTALLED_APPS` registers Django applications so their models, migrations, and configurations are recognized by the framework. If an app is not listed there, Django will not load its models or apply its migrations.

</details>

3️⃣ What is middleware?

<details>
<summary>Answer</summary>

Middleware is a layer that processes requests before they reach the view and responses before they are returned to the client. It is commonly used for authentication, security, logging, and session handling.

</details>

4️⃣ How does a request reach a view?

<details>
<summary>Answer</summary>

A request first goes through the configured middleware, then the URL resolver matches it to a view based on `urls.py`. The matched view processes the request and returns a response, which then passes back through middleware before being sent to the client.

</details>
