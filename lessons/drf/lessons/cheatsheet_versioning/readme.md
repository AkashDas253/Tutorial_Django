## **Django Rest Framework (DRF) - Versioning**  

### **Overview**  
Versioning in DRF allows maintaining multiple API versions to ensure backward compatibility and manage API changes effectively. It enables clients to request a specific API version while supporting smooth transitions between updates.

---

### **Types of Versioning in DRF**  

| Versioning Type | Description | Example |
|----------------|-------------|---------|
| **URL Path Versioning** | Version is specified in the URL. | `/v1/products/`, `/v2/products/` |
| **Query Parameter Versioning** | Version is passed as a query parameter. | `/products/?version=v1` |
| **Host Name Versioning** | Different versions are accessed via subdomains. | `v1.api.example.com` |
| **Accept Header Versioning** | Version is specified in the request header. | `Accept: application/vnd.myapp.v1+json` |
| **Custom Versioning** | Defines a custom logic for version handling. | Based on user roles, request metadata, etc. |

---

### **Enabling Versioning in DRF**  
Define the default versioning method in `settings.py`:  
```python
REST_FRAMEWORK = {
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.URLPathVersioning',
    'ALLOWED_VERSIONS': ['v1', 'v2'],
    'DEFAULT_VERSION': 'v1',
}
```

---

### **Implementing Different Versioning Strategies**  

#### **1. URL Path Versioning**  
- API version is included in the URL structure.  
- Example: `/api/v1/products/` and `/api/v2/products/`  

```python
from rest_framework.versioning import URLPathVersioning
from rest_framework.response import Response
from rest_framework.views import APIView

class ProductView(APIView):
    versioning_class = URLPathVersioning

    def get(self, request, *args, **kwargs):
        return Response({"version": request.version})
```

- **URL Pattern:**  
```python
urlpatterns = [
    path('api/<str:version>/products/', ProductView.as_view()),
]
```

---

#### **2. Query Parameter Versioning**  
- API version is passed as a query parameter (`?version=v1`).  

```python
from rest_framework.versioning import QueryParameterVersioning

class ProductView(APIView):
    versioning_class = QueryParameterVersioning

    def get(self, request, *args, **kwargs):
        return Response({"version": request.version})
```

- **Request Example:**  
  ```
  GET /api/products/?version=v2
  ```

---

#### **3. Accept Header Versioning**  
- API version is sent in the `Accept` header.  

```python
from rest_framework.versioning import AcceptHeaderVersioning

class ProductView(APIView):
    versioning_class = AcceptHeaderVersioning

    def get(self, request, *args, **kwargs):
        return Response({"version": request.version})
```

- **Request Header Example:**  
  ```
  Accept: application/vnd.myapp.v2+json
  ```

---

#### **4. Host Name Versioning**  
- Different API versions are served through subdomains (`v1.api.example.com`).  

```python
from rest_framework.versioning import HostNameVersioning

class ProductView(APIView):
    versioning_class = HostNameVersioning

    def get(self, request, *args, **kwargs):
        return Response({"version": request.version})
```

- **DNS Configuration Required:**  
  - `v1.api.example.com` → API v1  
  - `v2.api.example.com` → API v2  

---

#### **5. Custom Versioning**  
- Custom logic can be defined based on user roles, request headers, or metadata.  

```python
from rest_framework.versioning import BaseVersioning

class CustomVersioning(BaseVersioning):
    def determine_version(self, request, *args, **kwargs):
        return request.headers.get('X-Custom-Version', 'v1')

class ProductView(APIView):
    versioning_class = CustomVersioning

    def get(self, request, *args, **kwargs):
        return Response({"version": request.version})
```

- **Request Example:**  
  ```
  GET /api/products/
  X-Custom-Version: v2
  ```

---

### **Best Practices for API Versioning**  
- Use **URL Path Versioning** for clear versioning structure.  
- Ensure **backward compatibility** when introducing new versions.  
- **Deprecate old versions** gracefully by notifying clients.  
- Document version changes to **help API consumers** transition.  
- Consider **Custom Versioning** for flexible version control.  

---

### **Conclusion**  
API versioning in DRF is essential for maintaining compatibility and managing API evolution efficiently. By choosing an appropriate versioning strategy, developers can ensure seamless upgrades and long-term API stability.