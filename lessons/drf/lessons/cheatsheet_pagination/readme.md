## **Django Rest Framework (DRF) - Pagination**  

### **Overview**  
Pagination controls how large datasets are split into manageable pages in API responses. It improves performance and enhances usability by limiting the number of records returned per request.  

---

### **Types of Pagination in DRF**  

| Pagination Class | Description |
|-----------------|-------------|
| `PageNumberPagination` | Divides results into pages, accessible via a `page` query parameter. |
| `LimitOffsetPagination` | Uses `limit` (number of items per request) and `offset` (starting point). |
| `CursorPagination` | Uses an encoded cursor for efficient, consistent ordering. |
| **Custom Pagination** | Allows defining custom pagination logic. |

---

### **Default Pagination Configuration**  
Set global pagination settings in `settings.py`:  

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```
- Sets **`PageNumberPagination`** as the default.  
- Limits responses to **10 items per page**.  

---

### **1. Page Number Pagination**  
Divides results into numbered pages, controlled via the `page` query parameter.  

**Example Request:**  
```
GET /users/?page=2
```

**Custom Page Number Pagination Class:**  
```python
from rest_framework.pagination import PageNumberPagination

class CustomPageNumberPagination(PageNumberPagination):
    page_size = 5
    page_size_query_param = 'size'
    max_page_size = 50
```
- `page_size`: Default number of items per page.  
- `page_size_query_param`: Allows clients to specify page size.  
- `max_page_size`: Prevents excessive data requests.  

---

### **2. Limit Offset Pagination**  
Allows clients to control the number of items per request using `limit` and `offset` parameters.  

**Example Request:**  
```
GET /users/?limit=5&offset=10
```

**Custom LimitOffsetPagination Class:**  
```python
from rest_framework.pagination import LimitOffsetPagination

class CustomLimitOffsetPagination(LimitOffsetPagination):
    default_limit = 5
    max_limit = 50
```
- `default_limit`: Default number of items per request.  
- `max_limit`: Prevents excessively large responses.  

---

### **3. Cursor Pagination**  
Uses an encoded cursor to fetch the next or previous set of results.  

**Example Request:**  
```
GET /users/?cursor=cD0yMDIzLTAzLTAx
```

**Custom CursorPagination Class:**  
```python
from rest_framework.pagination import CursorPagination

class CustomCursorPagination(CursorPagination):
    page_size = 5
    ordering = '-created_at'  # Sorts results by newest first
```
- `page_size`: Number of items per request.  
- `ordering`: Defines sorting order (e.g., newest first).  

---

### **Using Pagination in a ViewSet**  
Attach pagination to specific views instead of global settings.  

```python
from rest_framework.viewsets import ReadOnlyModelViewSet
from myapp.models import User
from myapp.serializers import UserSerializer
from myapp.pagination import CustomPageNumberPagination

class UserViewSet(ReadOnlyModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    pagination_class = CustomPageNumberPagination
```

---

### **Best Practices**  
- Use **PageNumberPagination** for simple navigation.  
- Use **LimitOffsetPagination** for more control over result size.  
- Use **CursorPagination** for performance in large datasets.  
- Implement **custom pagination** for unique business needs.  

---

### **Conclusion**  
Pagination in DRF optimizes API responses by limiting data size. Different pagination methods provide flexibility for different use cases, ensuring efficient data retrieval.