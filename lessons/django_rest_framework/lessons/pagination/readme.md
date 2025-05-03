## Pagination in Django REST Framework

### Purpose

Pagination is a technique used to split a large set of results into smaller chunks or pages. It helps improve performance by reducing the amount of data returned in a single response, making it more manageable and faster for both the server and client.

In Django REST Framework (DRF), pagination allows you to return data in a paginated format for API responses that contain large sets of data, improving client-side performance and reducing server load.

---

### Types of Pagination in DRF

DRF provides several built-in pagination styles, which can be applied globally or per view.

#### 1. **PageNumberPagination**

* **Description**: This is the most basic form of pagination. It splits results into pages and includes a page number in the response. Clients can request a specific page by using query parameters like `?page=2`.
* **Default settings**:

  * **Page size**: 10 items per page.

**Example**:

```python
from rest_framework.pagination import PageNumberPagination
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Example
from .serializers import ExampleSerializer

class ExamplePagination(PageNumberPagination):
    page_size = 5  # Items per page
    page_size_query_param = 'page_size'  # Allow client to specify page size
    max_page_size = 100  # Maximum allowed page size

class ExampleView(APIView):
    pagination_class = ExamplePagination

    def get(self, request):
        queryset = Example.objects.all()
        paginator = ExamplePagination()
        paginated_result = paginator.paginate_queryset(queryset, request)
        serializer = ExampleSerializer(paginated_result, many=True)
        return paginator.get_paginated_response(serializer.data)
```

* **Request example**: `/api/examples/?page=2`
* **Response**:

```json
{
    "count": 100,
    "next": "http://example.com/api/examples/?page=3",
    "previous": "http://example.com/api/examples/?page=1",
    "results": [
        {"id": 6, "name": "Example 6"},
        {"id": 7, "name": "Example 7"}
    ]
}
```

#### 2. **LimitOffsetPagination**

* **Description**: This pagination style allows the client to specify the number of items and the offset (starting point) through query parameters. It is more flexible and allows clients to retrieve data starting from any index and specify how many results to retrieve.
* **Default settings**:

  * **Limit**: `10`
  * **Offset**: `0`

**Example**:

```python
from rest_framework.pagination import LimitOffsetPagination
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Example
from .serializers import ExampleSerializer

class ExamplePagination(LimitOffsetPagination):
    default_limit = 5  # Items per page
    max_limit = 100  # Maximum limit to prevent abuse

class ExampleView(APIView):
    pagination_class = ExamplePagination

    def get(self, request):
        queryset = Example.objects.all()
        paginator = ExamplePagination()
        paginated_result = paginator.paginate_queryset(queryset, request)
        serializer = ExampleSerializer(paginated_result, many=True)
        return paginator.get_paginated_response(serializer.data)
```

* **Request example**: `/api/examples/?limit=10&offset=20`
* **Response**:

```json
{
    "count": 100,
    "next": "http://example.com/api/examples/?limit=10&offset=30",
    "previous": "http://example.com/api/examples/?limit=10&offset=10",
    "results": [
        {"id": 21, "name": "Example 21"},
        {"id": 22, "name": "Example 22"}
    ]
}
```

#### 3. **CursorPagination**

* **Description**: Cursor pagination provides a more efficient way of paginating large datasets by using a "cursor" that points to the last item in the previous page. Unlike the other pagination styles, it does not require counting the total number of records and thus performs better with very large datasets.
* **Default settings**:

  * **Cursor**: The cursor is encoded and passed as a query parameter to navigate through the pages.

**Example**:

```python
from rest_framework.pagination import CursorPagination
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Example
from .serializers import ExampleSerializer

class ExamplePagination(CursorPagination):
    page_size = 5  # Items per page
    ordering = 'id'  # The field used to order the dataset

class ExampleView(APIView):
    pagination_class = ExamplePagination

    def get(self, request):
        queryset = Example.objects.all()
        paginator = ExamplePagination()
        paginated_result = paginator.paginate_queryset(queryset, request)
        serializer = ExampleSerializer(paginated_result, many=True)
        return paginator.get_paginated_response(serializer.data)
```

* **Request example**: `/api/examples/?cursor=abc123`
* **Response**:

```json
{
    "next": "http://example.com/api/examples/?cursor=xyz456",
    "previous": "http://example.com/api/examples/?cursor=abc123",
    "results": [
        {"id": 21, "name": "Example 21"},
        {"id": 22, "name": "Example 22"}
    ]
}
```

---

### Configuring Pagination

Pagination can be applied globally or on a per-view basis.

#### 1. **Global Pagination**

To apply pagination globally, add the `DEFAULT_PAGINATION_CLASS` and `PAGE_SIZE` settings in the Django `settings.py` file.

**Example**:

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
}
```

#### 2. **Per-View Pagination**

To apply pagination to a specific view, set the `pagination_class` attribute in the view or viewset.

**Example**:

```python
from rest_framework.pagination import PageNumberPagination
from rest_framework.views import APIView
from rest_framework.response import Response

class ExampleView(APIView):
    pagination_class = PageNumberPagination

    def get(self, request):
        queryset = Example.objects.all()
        paginator = PageNumberPagination()
        paginated_result = paginator.paginate_queryset(queryset, request)
        return paginator.get_paginated_response(paginated_result)
```

---

### Custom Pagination

You can create custom pagination classes by subclassing one of the built-in pagination classes and modifying the behavior as required.

#### **Creating Custom Pagination Class**

**Example**:

```python
from rest_framework.pagination import PageNumberPagination

class CustomPagination(PageNumberPagination):
    page_size = 20  # Custom page size
    page_size_query_param = 'page_size'  # Allow the client to specify a custom page size
    max_page_size = 50  # Limit maximum page size

# In views
class ExampleView(APIView):
    pagination_class = CustomPagination

    def get(self, request):
        queryset = Example.objects.all()
        paginator = CustomPagination()
        paginated_result = paginator.paginate_queryset(queryset, request)
        return paginator.get_paginated_response(paginated_result)
```

---

### Summary of Key Concepts

* **PageNumberPagination**: Basic pagination with a page number query parameter.
* **LimitOffsetPagination**: Pagination with limit and offset query parameters.
* **CursorPagination**: Efficient pagination for large datasets using a cursor.
* **Global Pagination**: Apply pagination globally using settings in `settings.py`.
* **Per-View Pagination**: Apply pagination per specific view by setting the `pagination_class` attribute.
* **Custom Pagination**: Create custom pagination classes by subclassing built-in classes like `PageNumberPagination`, `LimitOffsetPagination`, or `CursorPagination`.

---
