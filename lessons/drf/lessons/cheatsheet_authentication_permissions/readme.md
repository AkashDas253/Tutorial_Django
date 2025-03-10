## **Django Rest Framework (DRF) - Authentication & Permissions**  

### **Overview**  
Authentication and permissions in DRF control access to API endpoints. Authentication verifies user identity, while permissions define what authenticated users can do.  

---

### **Authentication in DRF**  
Authentication determines **who** is making the request. DRF supports multiple authentication classes.  

| Authentication Type  | Description |
|---------------------|-------------|
| **Session Authentication**  | Uses Django’s session framework (for web apps). |
| **Basic Authentication**  | Uses username-password for authentication (not recommended for production). |
| **Token Authentication**  | Uses tokens for stateless authentication. |
| **JWT Authentication**  | Uses JSON Web Tokens (JWT) for secure authentication. |
| **OAuth2 Authentication**  | Uses third-party OAuth2 providers (Google, GitHub, etc.). |
| **Custom Authentication**  | Custom logic for user authentication. |

---

### **Setting Up Authentication**  
Define authentication classes in `settings.py`:  
```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ]
}
```

---

### **Permissions in DRF**  
Permissions control **what** an authenticated user can access.  

| Permission Type      | Description |
|---------------------|-------------|
| `AllowAny`          | No restrictions (public access). |
| `IsAuthenticated`   | Allows only authenticated users. |
| `IsAdminUser`       | Allows only admin users. |
| `IsAuthenticatedOrReadOnly` | Allows authenticated users full access, others read-only. |
| `DjangoModelPermissions` | Uses Django’s model-level permissions. |
| `Custom Permissions` | Define custom access control rules. |

---

### **Applying Permissions**  
#### **Class-Based Views**  
```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.views import APIView
from rest_framework.response import Response

class SecureView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": "You are authenticated!"})
```

#### **ViewSets**  
```python
from rest_framework.permissions import IsAdminUser
from rest_framework import viewsets
from myapp.models import User
from myapp.serializers import UserSerializer

class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsAdminUser]
```

---

### **Custom Permissions**  
Custom permissions allow fine-grained control over access.  

#### **Example: Custom Permission for Object Ownership**  
```python
from rest_framework import permissions

class IsOwnerOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True
        return obj.owner == request.user
```
Apply it to a view:  
```python
class UserDetailView(generics.RetrieveUpdateDestroyAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsOwnerOrReadOnly]
```

---

### **Authentication vs. Authorization**  

| Aspect         | Authentication | Permissions (Authorization) |
|--------------|----------------|-----------------------------|
| Purpose      | Identifies users | Determines user access levels |
| Examples     | Token authentication, JWT | `IsAuthenticated`, `IsAdminUser` |
| Defined in   | `DEFAULT_AUTHENTICATION_CLASSES` | `permission_classes` in views |
| Applied to   | API requests (headers, sessions) | API actions (GET, POST, DELETE) |

---

### **Best Practices**  
- Use **Token/JWT Authentication** for APIs instead of session-based authentication.  
- Combine authentication with **permissions** for fine-grained control.  
- Use **custom permissions** for complex access rules.  
- Secure sensitive endpoints with **IsAuthenticated** or **IsAdminUser**.  

---

### **Conclusion**  
Authentication ensures that users are verified, while permissions define access control. DRF provides built-in options like `TokenAuthentication` and `IsAuthenticated`, along with custom authentication and permission classes for advanced security.