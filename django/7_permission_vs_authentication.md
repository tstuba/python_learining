# Permissions vs. Authentication

## Authentication

Who are you?

- `SessionAuthentication`
- `TokenAuthentication`
- `JWT`
- `BasicAuthentication`

If authentacation is successful:

- `request.user` is set
- user is authenticated

If not:

- `request.user = AnonymousUser`

## Permission

Can you do that?

- `IsAuthenticated`
- `IsAdminUser`
- `AllowAny`
- custom permissions

## Order

1. Authentication
2. Permission
3. View logic

## Example

```
from rest_framework.permissions import IsAuthenticated
from rest_framework.authentication import TokenAuthentication

class UserViewSet(ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]
```

- user my be logged-in (token)
- otherwise 401

## Custom Permission

```
from rest_framework.permissions import BasePermission

class IsOwner(BasePermission):

    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user
```

```
permission_classes = [IsOwner]
```

## Global vs. per-view

In `settings.py`:

```
REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticated",
    ]
}
```

Or overwrite in view.

## Summary

Authentication determines who the user is, while permissions determine whether the authenticated user is allowed to perform a specific action.
