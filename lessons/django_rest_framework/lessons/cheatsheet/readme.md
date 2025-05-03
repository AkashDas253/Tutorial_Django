## **Django REST Framework (DRF) cheatsheet**

---

### 🛠 Setup

```bash
pip install djangorestframework
```

```python
# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

---

### 📦 Basic Structure

```python
# urls.py
from django.urls import path, include
urlpatterns = [
    path('api/', include('myapp.api_urls')),
]
```

```python
# api_urls.py
from rest_framework.routers import DefaultRouter
router = DefaultRouter()
router.register('model', MyModelViewSet)
urlpatterns = router.urls
```

---

### 🧱 Serializers

```python
# serializers.py
from rest_framework import serializers
from .models import MyModel

class MyModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = MyModel
        fields = '__all__'
```

---

### 🧩 Views

```python
# views.py
from rest_framework import viewsets
from .models import MyModel
from .serializers import MyModelSerializer

class MyModelViewSet(viewsets.ModelViewSet):
    queryset = MyModel.objects.all()
    serializer_class = MyModelSerializer
```

---

### 🚀 ViewSet Actions

| Action          | Method | Detail | URL Pattern  |
| --------------- | ------ | ------ | ------------ |
| list            | GET    | False  | /model/      |
| retrieve        | GET    | True   | /model/{pk}/ |
| create          | POST   | False  | /model/      |
| update          | PUT    | True   | /model/{pk}/ |
| partial\_update | PATCH  | True   | /model/{pk}/ |
| destroy         | DELETE | True   | /model/{pk}/ |

---

### 🔧 Routers

```python
from rest_framework.routers import DefaultRouter
router = DefaultRouter()
router.register(r'items', ItemViewSet)
```

---

### 🔐 Authentication

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ]
}
```

---

### 🔐 Permissions

```python
from rest_framework.permissions import IsAuthenticated, IsAdminUser

class MyView(APIView):
    permission_classes = [IsAuthenticated]
```

Built-in:

* `AllowAny`
* `IsAuthenticated`
* `IsAdminUser`
* `IsAuthenticatedOrReadOnly`

---

### 🧵 Throttling

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.UserRateThrottle',
    ],
    'DEFAULT_THROTTLE_RATES': {
        'user': '1000/day',
    }
}
```

---

### 📃 Pagination

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
}
```

Types:

* `PageNumberPagination`
* `LimitOffsetPagination`
* `CursorPagination`

---

### 🔍 Filtering

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': [
        'django_filters.rest_framework.DjangoFilterBackend'
    ]
}
```

```python
# views.py
filterset_fields = ['name', 'status']
```

---

### ⚙ Versioning

```python
REST_FRAMEWORK = {
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.URLPathVersioning',
    'DEFAULT_VERSION': 'v1',
}
```

Types:

* `URLPathVersioning`
* `QueryParameterVersioning`
* `AcceptHeaderVersioning`
* `HostNameVersioning`

---

### 📤 Parsers

```python
REST_FRAMEWORK = {
    'DEFAULT_PARSER_CLASSES': [
        'rest_framework.parsers.JSONParser',
        'rest_framework.parsers.MultiPartParser',
    ]
}
```

---

### 📦 Renderers

```python
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': [
        'rest_framework.renderers.JSONRenderer',
        'rest_framework.renderers.BrowsableAPIRenderer',
    ]
}
```

---

### 🔀 Content Negotiation

```python
REST_FRAMEWORK = {
    'DEFAULT_CONTENT_NEGOTIATION_CLASS': 'rest_framework.negotiation.DefaultContentNegotiation'
}
```

---

### ❗ Exception Handling

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'myapp.utils.custom_exception_handler'
}
```

Common Exceptions:

* `ValidationError`
* `NotFound`
* `PermissionDenied`
* `AuthenticationFailed`

---

### 📄 Schema & Documentation

**Swagger / Redoc via drf-yasg**

```bash
pip install drf-yasg
```

```python
from drf_yasg.views import get_schema_view
from drf_yasg import openapi
```

**Spectacular**

```bash
pip install drf-spectacular
```

```python
REST_FRAMEWORK = {
    'DEFAULT_SCHEMA_CLASS': 'drf_spectacular.openapi.AutoSchema',
}
```

---

### 🧪 Testing

```python
from rest_framework.test import APITestCase

class MyModelTests(APITestCase):
    def test_create(self):
        response = self.client.post('/api/model/', data)
        self.assertEqual(response.status_code, 201)
```

---

### 📎 File Uploads

```python
class FileSerializer(serializers.ModelSerializer):
    file = serializers.FileField()
```

```python
class FileUploadView(APIView):
    parser_classes = [MultiPartParser]

    def post(self, request):
        ...
```

---

### 🔁 Caching

```python
from django.views.decorators.cache import cache_page

@method_decorator(cache_page(60*15), name='dispatch')
class CachedView(APIView):
    ...
```

---

### 🌳 Signals Integration

```python
from django.db.models.signals import post_save
@receiver(post_save, sender=MyModel)
def notify(sender, instance, created, **kwargs):
    ...
```

---

### 🔗 Nested Serializers

```python
class AuthorSerializer(serializers.ModelSerializer): ...
class BookSerializer(serializers.ModelSerializer):
    author = AuthorSerializer()
```

---
