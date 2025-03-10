## **Decorators in Django Views**  

### **Definition**  
Decorators in Django views are functions that modify the behavior of views without changing their code. They are often used for authentication, request validation, and method restrictions.

---

### **Common Decorators in Django Views**  

| Decorator | Purpose |
|-----------|---------|
| `@login_required` | Restricts access to authenticated users. |
| `@permission_required('app.permission')` | Ensures users have a specific permission. |
| `@user_passes_test(test_function)` | Restricts access based on a custom test function. |
| `@csrf_exempt` | Disables CSRF protection for a specific view. |
| `@require_http_methods(["GET", "POST"])` | Restricts allowed HTTP methods for the view. |
| `@require_GET` | Ensures only `GET` requests are allowed. |
| `@require_POST` | Ensures only `POST` requests are allowed. |
| `@require_safe` | Allows only `GET` and `HEAD` requests. |

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

Instead of applying decorators to each method, you can use `dispatch()` to apply them to all request methods.

```python
@method_decorator(login_required, name='dispatch')
@method_decorator(permission_required('app.view_model'), name='dispatch')
class MyView(View):
    def get(self, request):
        return HttpResponse("GET request received.")

    def post(self, request):
        return HttpResponse("POST request received.")
```

---
