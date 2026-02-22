# FastAPI

- async-first
- automatic validation
- automatic documentation (OpenAPI)
- dependency injection

```
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}
```

```
uvicorn main:app --reload
```

## Path parameters

```
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

FastAPI:

- automatically validates the type
- if not int → 422

## Query parameters

```
@app.get("/users")
def list_users(limit: int = 10):
    return {"limit": limit}
```

## Request body + Pydantic

```
from pydantic import BaseModel

class UserCreate(BaseModel):
    username: str
    email: str

@app.post("/users")
def create_user(user: UserCreate):
    return user
```

## Response model

```
@app.post("/users", response_model=UserCreate)
def create_user(user: UserCreate):
    return user
```

FastAPI:

- serializes according to the model
- filters unwanted fields

## Sync vs Async endpoints

```
def sync_endpoint():
    return {"type": "sync"}

@app.get("/async")
async def async_endpoint():
    return {"type": "async"}
```

When does async make sense?

- IO-bound operations
- external APIs
- async DB driver

## Request lifecycle in FastAPI

1. Request goes to ASGI server (Uvicorn)
2. Routing selects endpoint
3. Dependency Injection resolves dependencies
4. Pydantic validation
5. Endpoint executes
6. Response is serialized

## Summary

FastAPI is an ASGI-based framework that uses Pydantic for validation and automatic OpenAPI documentation. It supports dependency injection and both sync and async endpoints.
