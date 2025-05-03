## Filtering in Django REST Framework

### Purpose

Filtering allows clients to retrieve a subset of data based on certain criteria. It is an essential feature when dealing with large datasets, allowing users to query the API for specific information based on fields, ranges, and conditions.

Django REST Framework (DRF) provides several methods for filtering data in API views or viewsets. The most common approaches include using query parameters for filtering, implementing custom filters, and using third-party libraries like `django-filter`.

---

### Built-in Filtering in DRF

#### 1. **Basic Filtering**

In DRF, basic filtering can be applied using query parameters in the URL. DRF provides simple ways to filter data based on model fields, such as `?field=value`.

**Example**: Filtering based on a model field `name`:

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Example
from .serializers import ExampleSerializer

class ExampleView(APIView):
    def get(self, request):
        queryset = Example.objects.all()
        
        # Simple filtering by 'name' field using query parameter
        name_filter = request.query_params.get('name', None)
        if name_filter is not None:
            queryset = queryset.filter(name__icontains=name_filter)
        
        serializer = ExampleSerializer(queryset, many=True)
        return Response(serializer.data)
```

**Request example**:

```bash
GET /api/examples/?name=example
```

**Response**:

```json
[
    {"id": 1, "name": "Example 1"},
    {"id": 2, "name": "Example 2"}
]
```

#### 2. **Filtering with `filter()` Method**

You can filter data using the `filter()` method to apply conditions on fields, such as exact matches, partial matches, greater-than or less-than comparisons, etc.

* **Exact match**: `field=value`
* **Partial match**: `field__icontains=value`
* **Range filters**: `field__gte=value`, `field__lte=value`

**Example**: Filtering by multiple fields:

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Example
from .serializers import ExampleSerializer

class ExampleView(APIView):
    def get(self, request):
        queryset = Example.objects.all()
        
        # Filtering by name and id
        name_filter = request.query_params.get('name', None)
        id_filter = request.query_params.get('id', None)
        
        if name_filter:
            queryset = queryset.filter(name__icontains=name_filter)
        
        if id_filter:
            queryset = queryset.filter(id=id_filter)
        
        serializer = ExampleSerializer(queryset, many=True)
        return Response(serializer.data)
```

**Request example**:

```bash
GET /api/examples/?name=example&id=1
```

**Response**:

```json
[
    {"id": 1, "name": "Example 1"}
]
```

---

### Advanced Filtering with `django-filter`

`django-filter` is a third-party package that integrates with DRF to provide more advanced filtering options. It allows you to easily define filters for complex query conditions, including field lookups, date ranges, and more.

#### 1. **Installation**

To install `django-filter`, run the following command:

```bash
pip install django-filter
```

#### 2. **Integration with DRF**

To use `django-filter`, update the `DEFAULT_FILTER_BACKENDS` setting in `settings.py`:

```python
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': ['django_filters.rest_framework.DjangoFilterBackend'],
}
```

#### 3. **Creating Filters**

You can create a filter class by subclassing `django_filters.FilterSet`, where you define filters for the fields of your model.

**Example**:

```python
import django_filters
from .models import Example

class ExampleFilter(django_filters.FilterSet):
    name = django_filters.CharFilter(lookup_expr='icontains')
    id = django_filters.NumberFilter(lookup_expr='exact')
    
    class Meta:
        model = Example
        fields = ['name', 'id']
```

#### 4. **Using Filters in Views**

Apply the filter to a view or viewset by using `filterset_class`.

**Example**: Using `django-filter` with a viewset:

```python
from rest_framework import viewsets
from rest_framework.response import Response
from .models import Example
from .serializers import ExampleSerializer
from .filters import ExampleFilter

class ExampleViewSet(viewsets.ModelViewSet):
    queryset = Example.objects.all()
    serializer_class = ExampleSerializer
    filterset_class = ExampleFilter
```

**Request example**:

```bash
GET /api/examples/?name=example&id=1
```

**Response**:

```json
[
    {"id": 1, "name": "Example 1"}
]
```

---

### Filtering with `FilterBackend`

You can create custom filtering backends if needed. DRF’s `FilterBackend` allows you to implement custom logic to filter querysets based on the request.

**Example**:

```python
from rest_framework.filters import BaseFilterBackend

class CustomFilterBackend(BaseFilterBackend):
    def filter_queryset(self, request, queryset, view):
        # Implement custom filtering logic here
        name_filter = request.query_params.get('name', None)
        if name_filter:
            queryset = queryset.filter(name__icontains=name_filter)
        return queryset

# Apply the custom filter to a view
class ExampleView(APIView):
    filter_backends = [CustomFilterBackend]

    def get(self, request):
        queryset = Example.objects.all()
        # The filtering is applied automatically by the custom filter
        serializer = ExampleSerializer(queryset, many=True)
        return Response(serializer.data)
```

---

### Common Filter Lookups

* **Exact match**: `field=value`
* **Partial match**: `field__icontains=value`
* **Greater than**: `field__gt=value`
* **Less than**: `field__lt=value`
* **Greater than or equal to**: `field__gte=value`
* **Less than or equal to**: `field__lte=value`
* **In a list**: `field__in=[value1, value2]`
* **Null check**: `field__isnull=True`
* **Range**: `field__range=(start, end)`

---

### Summary of Key Concepts

* **Basic Filtering**: Use query parameters to filter based on model fields, with operators like `icontains`, `exact`, `gte`, `lte`, etc.
* **django-filter**: A powerful package that provides advanced filtering capabilities and integrates well with DRF.
* **Custom Filter Backends**: Allows the creation of custom filtering logic for complex use cases.
* **DRF Filters**: DRF provides built-in filters for common use cases (e.g., `FilterSet`, `CharFilter`, `NumberFilter`).

---
