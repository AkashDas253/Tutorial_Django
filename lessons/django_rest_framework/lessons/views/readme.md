## Views in Django REST Framework

### Purpose

Views in DRF handle HTTP requests and return HTTP responses. They define the logic for handling requests such as GET, POST, PUT, DELETE, and more. DRF provides class-based views (CBVs) that make it easier to write views for common API patterns.

---

### Key View Classes

#### APIView

* **Base class for all views in DRF.**
* Provides methods for HTTP methods like `get()`, `post()`, `put()`, `delete()`, etc.
* Allows full control over request handling.

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class ExampleView(APIView):
    def get(self, request):
        return Response({"message": "GET request"})
    
    def post(self, request):
        return Response({"message": "POST request"}, status=status.HTTP_201_CREATED)
```

#### GenericAPIView

* Inherits from `APIView` and provides additional helper methods.
* Often used as a base for views that need built-in behavior (e.g., list, create, retrieve).

```python
from rest_framework.generics import GenericAPIView
from rest_framework.mixins import ListModelMixin

class ExampleListView(GenericAPIView, ListModelMixin):
    queryset = Example.objects.all()
    serializer_class = ExampleSerializer
    
    def get(self, request):
        return self.list(request)
```

#### Mixins

* **Mixins** provide reusable methods for handling common actions like list, create, retrieve, etc.

  * `ListModelMixin`: Handles listing objects.
  * `CreateModelMixin`: Handles creating new objects.
  * `RetrieveModelMixin`: Handles retrieving a single object.
  * `UpdateModelMixin`: Handles updating an object.
  * `DestroyModelMixin`: Handles deleting an object.

#### ViewSet

* **Simplifies the creation of views** for models by combining multiple actions (list, create, retrieve, update, destroy).
* Automatically wired to handle actions like `list()`, `create()`, `retrieve()`, `update()`, `destroy()`.
* Can be used with a router to automatically generate URL patterns.

```python
from rest_framework.viewsets import ViewSet

class ExampleViewSet(ViewSet):
    queryset = Example.objects.all()
    serializer_class = ExampleSerializer

    def list(self, request):
        # Handle GET for list
        return Response({"message": "List of examples"})
    
    def create(self, request):
        # Handle POST to create
        return Response({"message": "Create an example"})
```

#### ModelViewSet

* Inherits from `ViewSet` and provides default implementations for CRUD operations (create, read, update, delete).
* Automatically uses the `queryset` and `serializer_class`.

```python
from rest_framework.viewsets import ModelViewSet

class ExampleModelViewSet(ModelViewSet):
    queryset = Example.objects.all()
    serializer_class = ExampleSerializer
```

#### ReadOnlyModelViewSet

* Similar to `ModelViewSet`, but only provides read-only operations (list and retrieve).
* Useful for APIs that only need to expose data without modification.

```python
from rest_framework.viewsets import ReadOnlyModelViewSet

class ExampleReadOnlyViewSet(ReadOnlyModelViewSet):
    queryset = Example.objects.all()
    serializer_class = ExampleSerializer
```

---

### ViewSet Actions

* **@action decorator** allows adding custom actions to a ViewSet.
* Useful for custom endpoints like filtering, custom reports, etc.

```python
from rest_framework.decorators import action
from rest_framework.response import Response

class ExampleViewSet(ViewSet):
    @action(detail=True, methods=['get'])
    def custom_action(self, request, pk=None):
        return Response({"message": f"Custom action for {pk}"})
```

* Parameters:

  * `detail=True/False`: Indicates whether the action operates on a single instance or a collection.
  * `methods`: The allowed HTTP methods (`get`, `post`, etc.)
  * `url_path`: Custom URL path for the action.
  * `url_name`: Custom URL name for the action.

---

### Request Handling Methods

* **get(self, request)**: Handle GET requests.
* **post(self, request)**: Handle POST requests.
* **put(self, request)**: Handle PUT requests.
* **patch(self, request)**: Handle PATCH requests (partial update).
* **delete(self, request)**: Handle DELETE requests.

Each method receives a `request` object and typically returns a `Response` object.

---

### Router Integration

* **Routers** automatically generate URL patterns for ViewSets.
* DRF provides two main types of routers:

  * `SimpleRouter`: Generates standard routes for `list` and `detail` actions.
  * `DefaultRouter`: Includes a root view that lists all registered viewsets.

```python
from rest_framework.routers import DefaultRouter
from .views import ExampleViewSet

router = DefaultRouter()
router.register(r'examples', ExampleViewSet)
urlpatterns = router.urls
```

* The router automatically generates URLs for:

  * `list` (GET) on the collection (e.g., `/examples/`)
  * `retrieve` (GET) on an individual item (e.g., `/examples/{id}/`)
  * `create` (POST) for creating items
  * `update` (PUT) for modifying items
  * `destroy` (DELETE) for deleting items

---

### Custom Views

Custom views can be used when more control is needed over the request handling process. This allows writing views with specific logic for one-off actions.

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class CustomView(APIView):
    def get(self, request, *args, **kwargs):
        # Custom logic
        return Response({"message": "Custom view"})
```

---

### Performance Considerations

* **Select Related and Prefetch Related**: Use `select_related` and `prefetch_related` in viewsets to minimize database queries when dealing with related models.
* **Pagination**: Use pagination classes (e.g., `PageNumberPagination`) in views to handle large datasets efficiently.

---
