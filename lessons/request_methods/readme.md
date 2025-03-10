## **Handling Request Methods in Django Views**  

### **Definition**  
Django views handle different HTTP request methods to process user interactions. Function-Based Views (FBVs) use conditionals to check request types, while Class-Based Views (CBVs) define dedicated methods for each request type.

---

### **Handling Request Methods in FBVs**  
FBVs check request methods explicitly using `request.method`.  

```python
from django.http import JsonResponse

def my_view(request):
    if request.method == "GET":
        return JsonResponse({"message": "GET request received"})
    elif request.method == "POST":
        return JsonResponse({"message": "POST request received"})
    elif request.method == "PUT":
        return JsonResponse({"message": "PUT request received"})
    elif request.method == "DELETE":
        return JsonResponse({"message": "DELETE request received"})
    else:
        return JsonResponse({"error": "Method not allowed"}, status=405)
```

---

### **Handling Request Methods in CBVs**  
CBVs define separate methods for each HTTP request type.  

```python
from django.views import View
from django.http import JsonResponse

class MyView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request received"})

    def put(self, request, *args, **kwargs):
        return JsonResponse({"message": "PUT request received"})

    def delete(self, request, *args, **kwargs):
        return JsonResponse({"message": "DELETE request received"})
```

---

### **Handling Request Methods with Django Decorators**  
Django provides decorators to restrict views to specific HTTP methods.

#### **FBV with @require_http_methods()**
```python
from django.http import JsonResponse
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    return JsonResponse({"message": f"{request.method} request received"})
```

#### **CBV with @method_decorator()**
```python
from django.utils.decorators import method_decorator
from django.views.decorators.http import require_http_methods
from django.views import View

@method_decorator(require_http_methods(["GET", "POST"]), name="dispatch")
class MyView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request received"})
```

---

### **Handling Request Methods in Django REST Framework (DRF)**  
Django REST Framework (DRF) provides `APIView` to define API request handlers.

```python
from rest_framework.views import APIView
from rest_framework.response import Response

class MyAPIView(APIView):
    def get(self, request, *args, **kwargs):
        return Response({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return Response({"message": "POST request received"})
```

---

### **HTTP Request Methods and Their Uses**  

| Method  | Description |
|---------|------------|
| `GET`   | Retrieve data from the server. |
| `POST`  | Submit data to the server. |
| `PUT`   | Update an existing resource. |
| `PATCH` | Partially update an existing resource. |
| `DELETE` | Remove a resource. |

---
