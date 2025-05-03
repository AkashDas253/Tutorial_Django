## Permissions in Django REST Framework

### Purpose

Permissions in DRF define who can access a particular resource or view based on the authentication details of the user. They determine whether a user has the right to perform an action (e.g., GET, POST, PUT, DELETE) on a specific resource. Permissions are a core aspect of controlling access to API endpoints, ensuring that users can only perform authorized actions.

---

### Types of Permissions in DRF

#### 1. **AllowAny**

* **Description**: This permission allows access to any user, regardless of authentication status.
* **Use Case**: Ideal for public endpoints where no user authentication is needed.

**Example**:

```python
from rest_framework.permissions import AllowAny
from rest_framework.views import APIView
from rest_framework.response import Response

class PublicView(APIView):
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"message": "Anyone can access this"})
```

#### 2. **IsAuthenticated**

* **Description**: Only authenticated users can access the view. If the user is not authenticated, they will receive an unauthorized error.
* **Use Case**: This permission is useful when you want to restrict access to logged-in users only.

**Example**:

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.views import APIView
from rest_framework.response import Response

class AuthenticatedView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": "You must be authenticated to access this"})
```

#### 3. **IsAdminUser**

* **Description**: Only admin users can access the view. Users with `is_staff` set to `True` in their user model will pass this permission check.
* **Use Case**: Useful when you want to provide access only to users with administrative privileges.

**Example**:

```python
from rest_framework.permissions import IsAdminUser
from rest_framework.views import APIView
from rest_framework.response import Response

class AdminOnlyView(APIView):
    permission_classes = [IsAdminUser]

    def get(self, request):
        return Response({"message": "Only admin users can access this"})
```

#### 4. **IsAuthenticatedOrReadOnly**

* **Description**: Allows any user to view (GET) the resource, but only authenticated users can modify (POST, PUT, DELETE) it.
* **Use Case**: Often used for resources that are publicly viewable, but require authentication for modifications (e.g., blog posts, public APIs).

**Example**:

```python
from rest_framework.permissions import IsAuthenticatedOrReadOnly
from rest_framework.views import APIView
from rest_framework.response import Response

class PublicEditableView(APIView):
    permission_classes = [IsAuthenticatedOrReadOnly]

    def get(self, request):
        return Response({"message": "Public content viewable"})
    
    def post(self, request):
        return Response({"message": "Only authenticated users can create"})
```

#### 5. **DjangoModelPermissions**

* **Description**: This permission class automatically grants access based on the permissions defined on the model's `Meta` class (i.e., `add`, `change`, `delete` permissions).
* **Use Case**: Useful when you want to use Django’s built-in model-level permissions to control access.

**Configuration**:
In your model’s `Meta` class, ensure that `permissions` are defined.

```python
class MyModel(models.Model):
    name = models.CharField(max_length=100)
    
    class Meta:
        permissions = [
            ("can_view_mymodel", "Can view MyModel"),
            ("can_edit_mymodel", "Can edit MyModel"),
        ]
```

**Example**:

```python
from rest_framework.permissions import DjangoModelPermissions
from rest_framework.views import APIView
from rest_framework.response import Response

class ModelPermissionView(APIView):
    permission_classes = [DjangoModelPermissions]

    def get(self, request):
        return Response({"message": "Model-level permissions applied"})
```

#### 6. **Custom Permissions**

* **Description**: You can define custom permissions to control access based on specific conditions, such as user roles or custom business logic.
* **Use Case**: When you need more fine-grained control over who can access certain resources.

**Example**:

```python
from rest_framework.permissions import BasePermission

class IsOwner(BasePermission):
    """
    Custom permission to only allow owners of an object to edit it.
    """
    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user
```

**Usage**:

```python
from rest_framework.views import APIView
from rest_framework.permissions import IsAuthenticated

class ExampleView(APIView):
    permission_classes = [IsAuthenticated, IsOwner]

    def get(self, request, pk):
        obj = get_object_or_404(Example, pk=pk)
        self.check_object_permissions(request, obj)
        return Response({"message": "You are the owner", "data": obj})
```

---

### Combining Permissions

Permissions can be combined in a view, and **AND** logic is applied. All conditions must be satisfied for the request to be allowed.

**Example**:

```python
from rest_framework.permissions import IsAuthenticated, IsAdminUser

class CombinedPermissionView(APIView):
    permission_classes = [IsAuthenticated, IsAdminUser]

    def get(self, request):
        return Response({"message": "Only authenticated admins can access this"})
```

---

### Permission Classes and Views

Permissions can be set on the **view level** or the **viewset level**.

#### 1. **View-level Permissions**

You can set permissions directly on individual views using the `permission_classes` attribute.

**Example**:

```python
class ExampleView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": "Only authenticated users can access"})
```

#### 2. **ViewSet-level Permissions**

For viewsets, you can set permissions at the class level using the `permission_classes` attribute. DRF will then apply the permissions to all actions within the viewset.

**Example**:

```python
from rest_framework import viewsets
from rest_framework.permissions import IsAuthenticated

class ExampleViewSet(viewsets.ModelViewSet):
    queryset = Example.objects.all()
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return Example.objects.filter(user=self.request.user)
```

---

### Summary of Key Concepts

* **Built-in Permissions**:

  * **AllowAny**: Allows access to any user.
  * **IsAuthenticated**: Only authenticated users can access.
  * **IsAdminUser**: Only admin users can access.
  * **IsAuthenticatedOrReadOnly**: Allows read-only access to unauthenticated users, but only authenticated users can modify.
  * **DjangoModelPermissions**: Uses Django's model-level permissions for access control.

* **Custom Permissions**: Custom permission classes can be created by inheriting `BasePermission` and implementing logic for object-level permissions.

* **Combining Permissions**: You can combine multiple permissions using the `permission_classes` list, applying **AND** logic across them.

---
