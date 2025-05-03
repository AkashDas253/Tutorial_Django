## Routers in Django REST Framework

### Purpose

Routers in DRF automatically generate URL patterns for views based on the actions defined in a `ViewSet`. They simplify URL configuration by handling the URL routing automatically for common patterns such as listing, creating, updating, and deleting resources.

---

### Key Types of Routers

#### SimpleRouter

* Automatically creates URL patterns for the `list` and `detail` actions of a `ViewSet`.
* No additional features beyond basic route generation.
* Suitable for simple applications.

```python
from rest_framework.routers import SimpleRouter
from .views import ExampleViewSet

router = SimpleRouter()
router.register(r'examples', ExampleViewSet)
urlpatterns = router.urls
```

* Generated URLs:

  * `/examples/` for the `list` action (GET)
  * `/examples/{id}/` for the `retrieve`, `update`, and `destroy` actions (GET, PUT/PATCH, DELETE)

#### DefaultRouter

* Extends `SimpleRouter` and includes a root view (`api-root`) to list all registered routes.
* The root view is useful for discovering available API endpoints.
* More comprehensive than `SimpleRouter`, ideal for production-grade APIs.

```python
from rest_framework.routers import DefaultRouter
from .views import ExampleViewSet

router = DefaultRouter()
router.register(r'examples', ExampleViewSet)
urlpatterns = router.urls
```

* Generated URLs (similar to `SimpleRouter`) plus:

  * `/` for the root view (`api-root`)

#### Custom Routers

* DRF allows custom routers for advanced use cases, such as adding custom URL patterns or routes with more specific logic.
* Custom routers can override default behaviors for URL pattern generation and route handling.

```python
from rest_framework.routers import SimpleRouter

class CustomRouter(SimpleRouter):
    def get_urls(self):
        urls = super().get_urls()
        # Custom URL logic can go here
        return urls

router = CustomRouter()
router.register(r'examples', ExampleViewSet)
urlpatterns = router.urls
```

---

### ViewSet and Router Integration

* **ViewSet** actions like `list()`, `create()`, `retrieve()`, `update()`, `destroy()` are automatically mapped to corresponding URLs by the router.
* When using routers, the URL patterns are dynamically created without manually defining each endpoint.

Example with `ModelViewSet`:

```python
from rest_framework.viewsets import ModelViewSet
from .models import Example
from .serializers import ExampleSerializer

class ExampleViewSet(ModelViewSet):
    queryset = Example.objects.all()
    serializer_class = ExampleSerializer
```

By registering this `ViewSet` with a router, DRF automatically generates:

* `/examples/` (GET for list, POST for create)
* `/examples/{id}/` (GET for retrieve, PUT/PATCH for update, DELETE for destroy)

---

### Custom Actions with Routers

Custom actions can be added to a `ViewSet` using the `@action` decorator. The router will automatically create routes for these custom actions.

```python
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.viewsets import ViewSet

class ExampleViewSet(ViewSet):
    @action(detail=True, methods=['get'])
    def custom_action(self, request, pk=None):
        return Response({"message": f"Custom action for {pk}"})
```

* This creates a new URL pattern for the custom action, such as `/examples/{id}/custom_action/`.

---

### URL Parameters and Customization

Routers automatically map standard URL parameters like `{id}` to view methods, which is used for `retrieve`, `update`, and `destroy` actions. You can customize these patterns as needed:

```python
# Customizing URL patterns using a custom router
from rest_framework.routers import DefaultRouter
from .views import ExampleViewSet

router = DefaultRouter()
router.register(r'examples', ExampleViewSet, basename='example')
urlpatterns = router.urls
```

* The `basename` argument is used to explicitly specify a base name for URL patterns.

---

### Performance Considerations

* **Avoid Over-Serialization**: Use `select_related` and `prefetch_related` in `ViewSet` queries to optimize database lookups, especially for related objects.
* **Custom Pagination**: For large datasets, customize pagination settings to avoid loading too many objects at once, which can be achieved through pagination classes.

---

### Summary of Key Router Benefits

* **Automatic URL generation**: DRF automatically handles routing for standard actions (list, retrieve, create, etc.).
* **Easier API scaling**: As your application grows, routers simplify the addition of new endpoints and maintainable URL patterns.
* **Customizable routing**: DRF’s support for custom actions and URL patterns gives flexibility to define complex routes when needed.

---
