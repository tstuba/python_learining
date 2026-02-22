# Pagination and Filtering

## Pagination

You can set in `settings.py`

```
REST_FRAMEWORK = {
    "DEFAULT_PAGINATION_CLASS":
        "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 10,
}
```

Now endpoint have `?page` attribute.

```
GET /users/?page=2
```

### Pagination types

### PageNumberPagination

```
?page=2
```

### LimitOffsetPagination

```
?limit=10&offset=20
```

### CursorPagination

- good for infinite scroll

### Custom pagination

```
from rest_framework.pagination import PageNumberPagination

class CustomPagination(PageNumberPagination):
    page_size = 20
```

In view:

```
pagination_class = CustomPagination
```

## Filtering

### Simple manual filtering

```
def get_queryset(self):
    queryset = User.objects.all()
    is_active = self.request.query_params.get("is_active")
    if is_active:
        queryset = queryset.filter(is_active=is_active)
    return queryset
```

### DjangoFilterBackend

Install `django-filter`

In view:

```
from django_filters.rest_framework import DjangoFilterBackend

class UserViewSet(ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    filter_backends = [DjangoFilterBackend]
    filterset_fields = ["is_active", "role"]
```

Now you can:

```
GET /users/?is_active=true
```

### Search + Ordering

```
from rest_framework.viewsets import ModelViewSet
from rest_framework.filters import SearchFilter, OrderingFilter

class UserViewSet(ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer

    filter_backends = [SearchFilter, OrderingFilter]
    search_fields = ["username", "email"]
    ordering_fields = ["created_at"]
```

Examples:

```
?search=john
?ordering=created_at
?ordering=-created_at
```

### Global config

```
REST_FRAMEWORK = {
    "DEFAULT_FILTER_BACKENDS": [
        "rest_framework.filters.SearchFilter",
        "rest_framework.filters.OrderingFilter",
    ]
}
```

## Summary

Pagination limits the number of returned records to improve performance and scalability. Filtering can be implemented manually in `get_queryset()` or using DjangoFilterBackend for declarative filtering.
