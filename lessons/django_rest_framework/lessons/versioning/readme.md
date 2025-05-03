## Versioning in Django REST Framework

### Purpose

API versioning allows clients to interact with different versions of an API without breaking backward compatibility. As your application evolves, the API may undergo changes, such as new features, removed endpoints, or modified data structures. Versioning ensures that older clients can continue to interact with the API without issues, while newer clients can take advantage of the updated version.

Django REST Framework (DRF) supports various strategies for versioning the API. The most common methods include URL path versioning, query parameter versioning, header versioning, and accept header versioning.

---

### Versioning Strategies in DRF

#### 1. **URL Path Versioning**

URL path versioning involves including the version number directly in the URL path. This is the most common and straightforward approach to versioning.

**Example**:

```bash
GET /api/v1/items/
GET /api/v2/items/
```

To implement URL path versioning in DRF, you can define the version in the URL pattern.

**Implementation**:

```python
from rest_framework.routers import DefaultRouter
from django.urls import path, include
from .views import ItemViewSet

router = DefaultRouter()
router.register(r'items', ItemViewSet)

urlpatterns = [
    path('v1/', include((router.urls, 'v1'))),
    path('v2/', include((router.urls, 'v2'))),
]
```

**Request example**:

```bash
GET /api/v1/items/
```

#### 2. **Query Parameter Versioning**

Query parameter versioning involves including the version number as a query parameter in the request URL.

**Example**:

```bash
GET /api/items/?version=1
GET /api/items/?version=2
```

To implement query parameter versioning, you can customize the `versioning` scheme in DRF.

**Implementation**:

```python
from rest_framework.versioning import QueryParameterVersioning
from rest_framework.routers import DefaultRouter
from django.urls import path, include
from .views import ItemViewSet

# Use query parameter versioning
class CustomQueryVersioning(QueryParameterVersioning):
    version_param = 'version'

router = DefaultRouter()
router.register(r'items', ItemViewSet)

urlpatterns = [
    path('api/', include(router.urls)),
]
```

**Request example**:

```bash
GET /api/items/?version=1
```

#### 3. **Header Versioning**

Header versioning involves including the version number in the HTTP request header. This approach is cleaner and separates the versioning information from the URL and query parameters.

**Example**:

```bash
GET /api/items/  # No version specified
GET /api/items/  # With version header
```

To implement header versioning, you can subclass `BaseVersioning` and define a custom versioning scheme.

**Implementation**:

```python
from rest_framework.versioning import BaseVersioning
from rest_framework.response import Response
from .models import Item
from .serializers import ItemSerializer

class HeaderVersioning(BaseVersioning):
    def determine_version(self, request, view):
        version = request.META.get('HTTP_X_API_VERSION')
        if version:
            return version
        return 'v1'  # Default version

# Add the custom versioning scheme in settings.py
REST_FRAMEWORK = {
    'DEFAULT_VERSIONING_CLASS': 'path.to.HeaderVersioning'
}
```

**Request example**:

```bash
GET /api/items/ -H "X-API-Version: 2"
```

#### 4. **Accept Header Versioning**

Accept header versioning involves including the version number in the `Accept` header of the request. This is a more RESTful approach and allows clients to specify the version they want through the media type.

**Example**:

```bash
GET /api/items/  # Default version
GET /api/items/  # With Accept header version
```

To implement accept header versioning, you can use `AcceptHeaderVersioning` provided by DRF.

**Implementation**:

```python
from rest_framework.versioning import AcceptHeaderVersioning
from rest_framework.response import Response
from .models import Item
from .serializers import ItemSerializer

# Add the versioning class to settings.py
REST_FRAMEWORK = {
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.AcceptHeaderVersioning'
}
```

**Request example**:

```bash
GET /api/items/ -H "Accept: application/vnd.myapp.v2+json"
```

---

### How Versioning Works in DRF

1. **Defining the Versioning Scheme**:
   The versioning scheme can be set in the `settings.py` file, which tells DRF how to interpret and handle versions in incoming requests.

   ```python
   REST_FRAMEWORK = {
       'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.AcceptHeaderVersioning'
   }
   ```

2. **Versioning in Views**:
   Each view or viewset can be versioned by associating it with a version in the URL or setting a versioning method for the viewset.

   **Example**:

   ```python
   from rest_framework.views import APIView
   from rest_framework.response import Response

   class ItemView(APIView):
       versioning_class = 'rest_framework.versioning.AcceptHeaderVersioning'
       
       def get(self, request):
           version = request.version
           if version == 'v1':
               return Response({'message': 'Version 1 of items'})
           elif version == 'v2':
               return Response({'message': 'Version 2 of items'})
   ```

3. **Compatibility Between Versions**:
   Ensure backward compatibility by not removing older endpoints until a migration strategy is defined for the client. You can also version the serializers and views separately to maintain different versions of the data or features exposed by the API.

4. **Default Versioning**:
   If a version is not provided in the request, you can set a default version to be used.

   **Example**:

   ```python
   REST_FRAMEWORK = {
       'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.URLPathVersioning',
       'DEFAULT_API_VERSION': 'v1',
   }
   ```

---

### Summary of Key Concepts

* **URL Path Versioning**: Version is included in the URL path (e.g., `/api/v1/`).
* **Query Parameter Versioning**: Version is specified as a query parameter (e.g., `/api/items/?version=1`).
* **Header Versioning**: Version is included in the HTTP headers (e.g., `X-API-Version: 1`).
* **Accept Header Versioning**: Version is included in the `Accept` header, using media types (e.g., `Accept: application/vnd.myapp.v2+json`).
* **Versioning Class**: DRF provides versioning classes that can be used in the `settings.py` file to define the versioning strategy.

---
