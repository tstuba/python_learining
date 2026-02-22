# DRF (Django Rest Framework) - core

## Serializer

Serializer - changes data to `JSON` and back

Works as:

- validator
- parser
- mapper bettwen modesl and `JSON`

### Serializer vs. ModelSerializer

Serializer:

```
class UserSerializer(serializers.Serializer):
    name = serializers.CharField()
    email = serializers.EmailField()
```

We need to write by hand `create()` and `update()`

ModelSerializer:

```
class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ["id", "name", "email"]
```

DRF:

- generates fields automatically
- by default implements `create()` and `update()`

## Validation flow

When we get POST request:

1. `serializer = Serializer(data=request.data)`
2. `serializer.is_valid()`
3. validating fields
4. validation `valdiate_<field>()`
5. global validation `validate()`
6. `serializer.save()` -> calls `create()` or `update()`

### Field-level validation

```
def validate_email(self, value):
    if "spam" in value:
        raise serializers.ValidationError("Invalid email")
    return value
```

### Object-level validation

```
def validate(self, data):
    if data["password"] != data["confirm"]:
        raise serializers.ValidationError("Passwords do not match")
    return data
```

## APIView vs. ViewSet vs. ModelViewSet

### APIView

```
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class UserAPIView(APIView):

    def get(self, request):
        users = User.objects.all()
        serializer = UserSerializer(users, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = UserSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=400)
```

- full control
- writing methods: `get()`, `post()` itd.

### ViewSet

```
from rest_framework.viewsets import ViewSet

class UserViewSet(ViewSet):

    def list(self, request):
        users = User.objects.all()
        serializer = UserSerializer(users, many=True)
        return Response(serializer.data)

    def create(self, request):
        serializer = UserSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        return Response(serializer.data)

    def retrieve(self, request, pk=None):
        user = User.objects.get(pk=pk)
        serializer = UserSerializer(user)
        return Response(serializer.data)
```

`urls.py`

```
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(r'users', UserViewSet)

urlpatterns = router.urls
```

- joins `CRUD` operations
- works with router
- less of a boilerplate

### ModelViewSet

```
from rest_framework.viewsets import ModelViewSet

class UserViewSet(ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

- ready to work crud
- be default handels:
  - list
  - retrieve
  - create
  - update
  - destroy

### Methods to HTTP mapping

| HTTP Method  | Action Method      |
| ------------ | ------------------ |
| GET (list)   | `list()`           |
| GET (detail) | `retrieve()`       |
| POST         | `create()`         |
| PUT          | `update()`         |
| PATCH        | `partial_update()` |
| DELETE       | `destroy()`        |

Example:

```
GET    /users/        → list()
GET    /users/1/      → retrieve()
POST   /users/        → create()
PUT    /users/1/      → update()
PATCH  /users/1/      → partial_update()
DELETE /users/1/      → destroy()
```

## Authentication vs. Permissions

### Authentication

Who are you?

- Token
- Session
- JWT

### Permission

Can you do that?

- `IsAuthenticated`
- `IsAdminUser`
- custom permissions

You can be authenticated without permissions.

## Pagination

DRF allows you:

- limit/offset
- page number
- custom pagination classes

## Summary

DRF serializers handle both validation and transformation between model instances and JSON, while ViewSets and routers reduce boilerplate for standard CRUD APIs.
