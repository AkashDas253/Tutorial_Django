## Decorators for Function-Based Views (FBV) in Django

Django provides several decorators that can be used with **Function-Based Views (FBVs)** to add functionality like restricting access, requiring authentication, or handling different HTTP methods. Decorators in Django are a way to wrap your view functions and modify their behavior without changing their code.

#### Example:

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    if request.method == "GET":
        return render(request, "template.html")
    elif request.method == "POST":
        return HttpResponse("POST Response")
```

---

## **Decorators for Function-Based Views (FBVs) in Short**

| **Decorator**                            | **Purpose**                                                                                                                                                                                                                     | **Parameters, Defaults, and Descriptions**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `@login_required`                        | Ensures only authenticated users can access the view. Redirects unauthenticated users to a login page.                                                                                  | - **`login_url`**: Custom URL to redirect unauthenticated users. Defaults to `settings.LOGIN_URL`. Accepts a string (e.g., `'/login/'`).<br>- **`redirect_field_name`**: Name of the query parameter used to store the original URL. Defaults to `'next'`. Accepts a string.                                                                                                                                                                                                                                  |
| `@permission_required(perm, ...)`        | Checks if the user has a specific permission before allowing access to the view.                                                                                                        | - **`perm`**: The required permission in the format `'app_label.permission_code'`. No default.<br>- **`login_url`**: Custom URL to redirect unauthenticated users. Defaults to `settings.LOGIN_URL`. Accepts a string.<br>- **`raise_exception`**: If `True`, raises a `PermissionDenied` exception instead of redirecting. Defaults to `False`. Accepts `True` or `False`.                                                                                                                                      |
| `@require_http_methods(http_methods)`    | Restricts the view to handle only specific HTTP methods (e.g., `GET`, `POST`).                                                                                                           | - **`http_methods`**: List of allowed HTTP methods. No default.<br>- Example: `["GET", "POST"]`. Accepts a list of HTTP methods (strings like `'GET'`, `'POST'`, `'DELETE'`, etc.).                                                                                                                                                                                                                                                                                                                        |
| `@require_GET`                           | Restricts the view to handle only `GET` requests.                                                                                                                                       | - No additional parameters. Restricts the view to handle only `GET` requests. Returns `405 Method Not Allowed` for other HTTP methods.                                                                                                                                                                                                                                                                                                                                                                     |
| `@require_POST`                          | Restricts the view to handle only `POST` requests.                                                                                                                                      | - No additional parameters. Restricts the view to handle only `POST` requests. Returns `405 Method Not Allowed` for other HTTP methods.                                                                                                                                                                                                                                                                                                                                                                    |
| `@csrf_exempt`                           | Disables CSRF validation for the specific view. Useful for handling external API calls or services where CSRF protection is not required.                                              | - No additional parameters. Disables CSRF protection for the view. Be cautious when using this, as it may expose the application to CSRF attacks.                                                                                                                                                                                                                                                                                                                                                          |
| `@cache_page(timeout, ...)`              | Caches the output of a view for a specified time. Helps improve performance for views with static or infrequently changing content.                                                    | - **`timeout`**: Time in seconds for which the view's output should be cached. No default. Example: `60 * 15` (15 minutes).<br>- **`cache`**: Name of the cache backend to use. Defaults to `None`. Accepts a string (e.g., `'default'`).<br>- **`key_prefix`**: Prefix for cache keys. Defaults to `None`. Accepts a string.                                                                                                                                                                                |
| `@vary_on_headers(*headers)`             | Ensures different cached responses are served based on specific HTTP request headers.                                                                                                  | - **`headers`**: Headers to vary the cache on (e.g., `'Accept-Language'`, `'User-Agent'`). No default. Accepts one or more string arguments.<br>- Example: `@vary_on_headers('Accept-Language')`. Ensures the cache varies for different values of `'Accept-Language'`.                                                                                                                                                                                                                                  |
| `@method_decorator(decorator, name=None)`| Used to apply a function-based decorator to a method of a Class-Based View (CBV).                                                                                                      | - **`decorator`**: The function-based decorator to apply.<br>- **`name`**: Name of the method to which the decorator should be applied. Defaults to `None`. If `None`, applies the decorator directly to the method.<br>- Example: `@method_decorator(login_required, name='dispatch')`.                                                                                                                                                                                                                        |
| `@sensitive_post_parameters(*parameters)`| Prevents specified POST parameters from appearing in Django's debug logs. Useful for protecting sensitive information like passwords or credit card details.                                                                 | - **`parameters`**: Names of POST parameters to hide from logs (e.g., `'password'`, `'credit_card'`). No default. Accepts one or more string arguments.<br>- Example: `@sensitive_post_parameters('password', 'credit_card')`.                                                                                                                                                                                                                                                                            |

---

## Common Decorators 

### **`@login_required`**

The `@login_required` decorator ensures that a user must be logged in to access a particular view. If the user is not logged in, they are redirected to the login page.

#### Usage:

```python
from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    return render(request, 'my_template.html')
```

**Explanation**:
- `@login_required` is used to protect views from being accessed by users who are not authenticated.
- If the user is not authenticated, Django will redirect them to the login URL specified in the settings (`LOGIN_URL`).

---

### **`@permission_required`**

The `@permission_required` decorator restricts access to a view based on a specific permission. It checks whether the logged-in user has the required permission to access the view.

#### Usage:

```python
from django.contrib.auth.decorators import permission_required

@permission_required('myapp.can_edit')
def edit_view(request):
    return render(request, 'edit_template.html')
```

**Explanation**:
- `@permission_required('app_name.permission_code')` ensures that the user has the specified permission (`can_edit` in this case) before they can access the view.
- If the user does not have the permission, they will be redirected to the URL specified by `LOGIN_REDIRECT_URL` or shown a 403 error.

---

### **`@require_http_methods`**

The `@require_http_methods` decorator restricts the allowed HTTP methods for a particular view. This ensures that the view only responds to the specified methods.

#### Usage:

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    return render(request, 'my_template.html')
```

**Explanation**:
- The `@require_http_methods` decorator allows only the specified HTTP methods (`GET` and `POST` in this case) to be handled by the view.
- If a client sends a different HTTP method (e.g., `DELETE`), Django will return a 405 Method Not Allowed error.

---

### **`@require_GET` and `@require_POST`**

These are shorthand decorators that are used to restrict views to only accept `GET` or `POST` requests, respectively.

#### Usage:

- **For GET requests**:

```python
from django.views.decorators.http import require_GET

@require_GET
def my_get_view(request):
    return render(request, 'my_get_template.html')
```

- **For POST requests**:

```python
from django.views.decorators.http import require_POST

@require_POST
def my_post_view(request):
    return render(request, 'my_post_template.html')
```

**Explanation**:
- `@require_GET` ensures that the view can only handle `GET` requests.
- `@require_POST` ensures that the view can only handle `POST` requests.
- If any other HTTP method is used, a `405 Method Not Allowed` error is returned.

---

### **`@csrf_exempt`**

The `@csrf_exempt` decorator disables the Cross-Site Request Forgery (CSRF) protection for the specific view. This is useful for views that handle requests from external services or APIs where CSRF protection is not required.

#### Usage:

```python
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def my_view(request):
    return HttpResponse('CSRF protection is disabled for this view.')
```

**Explanation**:
- `@csrf_exempt` tells Django to skip CSRF validation for the view.
- Use this decorator with caution, as it can make your application vulnerable to CSRF attacks.

---

### **`@cache_page`**

The `@cache_page` decorator caches the output of the view for a specified time, which can significantly improve the performance of views that do not change frequently.

#### Usage:

```python
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)  # Cache for 15 minutes
def my_view(request):
    return render(request, 'my_template.html')
```

**Explanation**:
- The `@cache_page(seconds)` decorator caches the output of the view for the specified number of seconds (in this case, 15 minutes).
- When the view is requested, if the cached version is still valid, it is returned instead of regenerating the response.

---

### **`@vary_on_headers`**

The `@vary_on_headers` decorator is used to vary the cache based on specific request headers, typically the `Accept-Language` or `User-Agent` headers. This ensures that different versions of the view are cached for different headers.

#### Usage:

```python
from django.views.decorators.vary import vary_on_headers

@vary_on_headers('Accept-Language')
def my_view(request):
    return render(request, 'my_template.html')
```

**Explanation**:
- `@vary_on_headers` instructs the cache to vary based on the specified headers. In this case, the cache will differ for different values of the `Accept-Language` header (e.g., English vs. Spanish).

---

### **`@debug`**

This decorator is used during development to output detailed debugging information when the view is accessed. It should not be used in production.

#### Usage:

```python
from django.views.decorators.debug import sensitive_post_parameters

@sensitive_post_parameters('password')
def sensitive_view(request):
    # Handle the view logic
    pass
```

**Explanation**:
- `@debug` is typically used to prevent sensitive data (e.g., password) from appearing in logs.

---

### **`@sensitive_post_parameters`**

The `@sensitive_post_parameters` decorator helps prevent sensitive data (such as passwords) from being logged or printed in Django's debug logs.

#### Usage:

```python
from django.views.decorators.debug import sensitive_post_parameters

@sensitive_post_parameters('password', 'credit_card')
def post_data_view(request):
    # Process POST data
    pass
```

**Explanation**:
- `@sensitive_post_parameters` prevents certain POST parameters (e.g., `password`, `credit_card`) from being exposed in logs or debug information.
  
---

### **`@method_decorator`**

If you want to apply a decorator to a class-based view (CBV) method, you can use the `@method_decorator`. This is useful when combining function-based decorators with class-based views.

#### Usage:

```python
from django.utils.decorators import method_decorator
from django.views import View

class MyView(View):
    @method_decorator(login_required)
    def get(self, request):
        return render(request, 'my_template.html')
```

**Explanation**:
- `@method_decorator` applies the `@login_required` decorator to the `get` method of a class-based view (CBV).
- This is essential when you want to use function-based decorators with CBVs.

---

### Summary of Common FBV Decorators

| **Decorator**                 | **Purpose**                                            | **Usage Example**                                        |
|-------------------------------|--------------------------------------------------------|----------------------------------------------------------|
| `@login_required`              | Ensures the user is authenticated before accessing the view | `@login_required`                                         |
| `@permission_required`         | Ensures the user has a specific permission             | `@permission_required('myapp.can_edit')`                 |
| `@require_http_methods`        | Restricts the view to specific HTTP methods            | `@require_http_methods(["GET", "POST"])`                 |
| `@require_GET`                 | Restricts the view to only handle GET requests         | `@require_GET`                                           |
| `@require_POST`                | Restricts the view to only handle POST requests        | `@require_POST`                                          |
| `@csrf_exempt`                 | Disables CSRF protection for the view                  | `@csrf_exempt`                                           |
| `@cache_page`                  | Caches the view's output for a specified time          | `@cache_page(60 * 15)`                                    |
| `@vary_on_headers`             | Varies the cached view based on request headers        | `@vary_on_headers('Accept-Language')`                    |
| `@method_decorator`            | Applies a decorator to a CBV method                    | `@method_decorator(login_required)`                      |

---

### Conclusion

Decorators are a powerful feature in Django, allowing you to modify the behavior of views easily. They can be used to implement features such as authentication, permissions, caching, and method restrictions in a clean and reusable manner. When using function-based views (FBVs), decorators help keep your code DRY (Don't Repeat Yourself) and easy to manage.