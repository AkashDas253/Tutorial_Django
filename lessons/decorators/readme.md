## **Django Decorators**  

### **Definition**  
Django decorators are functions that wrap another function or class-based view to modify its behavior. They are commonly used for authentication, caching, permissions, and request modifications.

---

### **Built-in Django View Decorators**  

| Decorator | Usage | Description |
|-----------|-------|-------------|
| `@login_required` | `@login_required` | Restricts access to authenticated users. |
| `@permission_required('app.permission')` | `@permission_required('app.view_model')` | Ensures the user has the specified permission. |
| `@user_passes_test(test_func)` | `@user_passes_test(lambda u: u.is_staff)` | Restricts access based on a custom test function. |
| `@staff_member_required` | `@staff_member_required` | Grants access only to staff users. |
| `@require_GET` | `@require_GET` | Ensures the view only handles GET requests. |
| `@require_POST` | `@require_POST` | Ensures the view only handles POST requests. |
| `@require_http_methods(["GET", "POST"])` | `@require_http_methods(["GET", "POST"])` | Restricts a view to specific HTTP methods. |
| `@cache_page(timeout)` | `@cache_page(60 * 15)` | Caches a view’s response for a specified duration. |
| `@csrf_exempt` | `@csrf_exempt` | Disables CSRF protection for a specific view. |
| `@xframe_options_exempt` | `@xframe_options_exempt` | Allows a view to be embedded in an iframe. |
| `@xframe_options_deny` | `@xframe_options_deny` | Prevents the view from being embedded in an iframe. |
| `@vary_on_headers("User-Agent")` | `@vary_on_headers("User-Agent")` | Modifies cache behavior based on headers. |
| `@vary_on_cookie` | `@vary_on_cookie` | Modifies cache behavior based on cookies. |

---

### **Using Decorators in Function-Based Views (FBVs)**  

```python
from django.http import HttpResponse
from django.contrib.auth.decorators import login_required, permission_required, user_passes_test
from django.views.decorators.http import require_http_methods, require_GET, require_POST
from django.views.decorators.csrf import csrf_exempt

@require_http_methods(["GET", "POST"])
@login_required
@permission_required('app.view_model')
@user_passes_test(lambda user: user.is_superuser)
@csrf_exempt
def my_view(request):
    return HttpResponse("Hello, World!")
```

---

### **Using Decorators in Class-Based Views (CBVs)**  

Since CBVs use classes instead of functions, decorators must be applied using `method_decorator`.  

```python
from django.views import View
from django.utils.decorators import method_decorator
from django.contrib.auth.decorators import login_required, permission_required
from django.views.decorators.csrf import csrf_exempt

class MyView(View):
    @method_decorator(login_required)
    @method_decorator(permission_required('app.view_model'))
    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return HttpResponse("Hello from CBV!")
```

---

### **Applying Decorators to All Methods in CBVs**  

Instead of applying decorators to each method, `dispatch()` applies them to all request methods.

```python
from django.views import View
from django.utils.decorators import method_decorator
from django.contrib.auth.decorators import login_required, permission_required

@method_decorator(login_required, name='dispatch')
@method_decorator(permission_required('app.view_model'), name='dispatch')
class MyView(View):
    def get(self, request):
        return HttpResponse("GET request received.")

    def post(self, request):
        return HttpResponse("POST request received.")
```

---

### **Custom Django Decorators**  

Custom decorators allow modifying request handling before or after calling a view function.

#### **Creating a Simple Decorator**  
Uses `functools.wraps` to preserve function metadata.

```python
from functools import wraps
from django.http import HttpResponseForbidden

def custom_decorator(view_func):
    @wraps(view_func)
    def wrapper(request, *args, **kwargs):
        if not request.user.is_authenticated:
            return HttpResponseForbidden("Access denied")
        return view_func(request, *args, **kwargs)
    return wrapper
```

#### **Applying a Custom Decorator**  

```python
@custom_decorator
def my_view(request):
    return HttpResponse("Welcome!")
```

---

### **Class-Based View Decorators**  

Django provides `method_decorator` to apply function-based decorators to class-based views.

| Method | Usage | Description |
|--------|-------|-------------|
| `method_decorator(decorator_name)` | `@method_decorator(login_required, name='dispatch')` | Applies a decorator to a class-based view. |
| `method_decorator(decorator_name, name='method')` | `@method_decorator(csrf_exempt, name='post')` | Applies a decorator to a specific method. |

#### **Applying to a Class-Based View**  

```python
from django.utils.decorators import method_decorator
from django.views import View

@method_decorator(login_required, name='dispatch')
class ProtectedView(View):
    def get(self, request):
        return HttpResponse("This is a protected view")
```

---
