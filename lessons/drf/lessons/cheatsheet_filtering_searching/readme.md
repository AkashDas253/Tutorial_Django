## **Django Rest Framework (DRF) - Filtering & Searching**  

### **Overview**  
Filtering and searching allow users to refine API responses based on query parameters, improving efficiency and usability. DRF provides built-in filters and search mechanisms to handle structured queries.

---

### **Filtering in DRF**  
Filtering enables API consumers to retrieve specific subsets of data.  

| Filtering Type | Description |
|---------------|-------------|
| **Basic Filtering** | Manually extracts query parameters from requests (`?category=books`). |
| **DjangoFilterBackend** | Uses `django-filter` to filter querysets based on model fields. |
| **SearchFilter** | Allows text-based searches across multiple fields. |
| **OrderingFilter** | Enables sorting based on query parameters (`?ordering=name`). |
| **Custom Filtering** | Implements filtering logic using custom methods. |

**Example: DjangoFilterBackend**  
```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework.viewsets import ReadOnlyModelViewSet
from myapp.models import Product
from myapp.serializers import ProductSerializer

class ProductViewSet(ReadOnlyModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [DjangoFilterBackend]
    filterset_fields = ['category', 'price']
```
- Allows filtering by `?category=electronics&price=100`.  

---

### **Searching in DRF**  
Searching enables keyword-based retrieval of data.  

| Search Method | Description |
|--------------|-------------|
| **Simple Search** | Uses `?search=query` to match fields. |
| **Case-Insensitive Search** | Matches queries regardless of case sensitivity. |
| **Multi-Field Search** | Searches across multiple fields using `search_fields`. |
| **Custom Search** | Implements advanced search logic. |

**Example: SearchFilter**  
```python
from rest_framework.filters import SearchFilter

class ProductViewSet(ReadOnlyModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [SearchFilter]
    search_fields = ['name', 'description']
```
- Enables searching by `?search=laptop`.  

---

### **Ordering in DRF**  
Ordering allows sorting API results based on fields.  

**Example: OrderingFilter**  
```python
from rest_framework.filters import OrderingFilter

class ProductViewSet(ReadOnlyModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [OrderingFilter]
    ordering_fields = ['price', 'name']
```
- Allows sorting with `?ordering=price` or `?ordering=-name`.  

---

### **Global Configuration in `settings.py`**  
```python
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': [
        'django_filters.rest_framework.DjangoFilterBackend',
        'rest_framework.filters.SearchFilter',
        'rest_framework.filters.OrderingFilter',
    ],
}
```
- Enables **filtering, searching, and ordering** for all views.  

---

### **Best Practices**  
- Use **DjangoFilterBackend** for model-based filtering.  
- Use **SearchFilter** for keyword-based queries.  
- Use **OrderingFilter** to enable sorting in API responses.  
- Implement **custom filtering** when complex queries are needed.  

---

### **Conclusion**  
Filtering and searching in DRF improve API efficiency by enabling precise data retrieval, ensuring flexible and scalable query handling.