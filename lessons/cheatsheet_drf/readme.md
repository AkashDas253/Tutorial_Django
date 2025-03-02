### **Django REST Framework (DRF) Cheatsheet**  

Django REST Framework (DRF) is used to build **RESTful APIs** in Django.  

---

## **1. Installation & Setup**  

### **Install DRF**  
```sh
pip install djangorestframework
```

### **Add to `INSTALLED_APPS` (`settings.py`)**  
```python
INSTALLED_APPS = [
    'rest_framework',
]
```

### **Basic DRF Settings (`settings.py`)**  
```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}
```

---

## **2. Serializers (Converting Models to JSON)**  

### **Define a Serializer (`serializers.py`)**  
```python
from rest_framework import serializers
from .models import Book

class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = '__all__'
```

| **Type** | **Description** |
|----------|----------------|
| `ModelSerializer` | Auto-generates fields from a model. |
| `Serializer` | Custom field definitions. |

---

## **3. Views & Endpoints**  

### **Basic API View (`views.py`)**  
```python
from rest_framework.response import Response
from rest_framework.decorators import api_view

@api_view(['GET'])
def api_home(request):
    return Response({"message": "Welcome to DRF!"})
```

### **Class-Based View (`views.py`)**  
```python
from rest_framework import generics
from .models import Book
from .serializers import BookSerializer

class BookListCreate(generics.ListCreateAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

| **Class** | **Purpose** |
|-----------|------------|
| `ListCreateAPIView` | List and create objects. |
| `RetrieveUpdateDestroyAPIView` | Get, update, delete objects. |
| `RetrieveAPIView` | Retrieve a single object. |

---

## **4. URL Configuration (`urls.py`)**  
```python
from django.urls import path
from .views import BookListCreate

urlpatterns = [
    path('books/', BookListCreate.as_view(), name='book-list'),
]
```

---

## **5. Authentication & Permissions**  

### **Permissions (`permissions.py`)**  
```python
from rest_framework import permissions

class IsAdminOrReadOnly(permissions.BasePermission):
    def has_permission(self, request, view):
        return request.method in permissions.SAFE_METHODS or request.user.is_staff
```

### **Apply Permission to View (`views.py`)**  
```python
class BookListCreate(generics.ListCreateAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    permission_classes = [IsAdminOrReadOnly]
```

| **Permission** | **Description** |
|---------------|----------------|
| `AllowAny` | No authentication required. |
| `IsAuthenticated` | Authenticated users only. |
| `IsAdminUser` | Admins only. |
| `IsAuthenticatedOrReadOnly` | Read for all, write for authenticated users. |

---

## **6. ViewSets & Routers**  

### **Define a ViewSet (`views.py`)**  
```python
from rest_framework import viewsets
from .models import Book
from .serializers import BookSerializer

class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

### **Register ViewSet with Router (`urls.py`)**  
```python
from rest_framework.routers import DefaultRouter
from .views import BookViewSet

router = DefaultRouter()
router.register(r'books', BookViewSet)

urlpatterns = router.urls
```

---

## **7. Token Authentication**  

### **Install DRF Token Auth**  
```sh
pip install djangorestframework authtoken
```

### **Add to `INSTALLED_APPS` (`settings.py`)**  
```python
INSTALLED_APPS = [
    'rest_framework.authtoken',
]
```

### **Run Migrations**  
```sh
python manage.py migrate
```

### **Generate Token for a User**  
```python
from rest_framework.authtoken.models import Token
from django.contrib.auth.models import User

user = User.objects.get(username='john')
token, created = Token.objects.get_or_create(user=user)
print(token.key)
```

### **Use Token in API Requests**  
- Include in **Authorization Header**  
```
Authorization: Token your_token_here
```

---

## **8. Pagination**  

### **Enable Pagination (`settings.py`)**  
```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
}
```

---

## **9. Filtering & Searching**  

### **Enable Filtering (`views.py`)**  
```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import filters

class BookListCreate(generics.ListCreateAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ['author', 'published_date']
    search_fields = ['title']
    ordering_fields = ['price']
```

| **Feature** | **Query Parameter** | **Example** |
|------------|------------------|------------|
| Filtering | `?author=John` | Get books by author. |
| Searching | `?search=Django` | Search by title. |
| Ordering | `?ordering=-price` | Sort by descending price. |

---
