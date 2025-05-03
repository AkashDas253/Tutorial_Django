## **Middleware in Django**

Middleware is a lightweight, low-level plugin system that processes requests and responses globally. It sits between the web server and Django views, allowing modification of input and output.

---

### **1. Structure of Middleware**

A middleware is a Python class with at least one of the following methods:

```python
class MyMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
    
    def __call__(self, request):
        # Code before view
        response = self.get_response(request)
        # Code after view
        return response
```

---

### **2. Middleware Functions**

| Function                           | Purpose                                   |
| ---------------------------------- | ----------------------------------------- |
| `__init__(self, get_response)`     | Initialization; runs once at server start |
| `__call__(self, request)`          | Called on each request                    |
| `process_view()`                   | Called before view function               |
| `process_exception()`              | Called on exceptions                      |
| `process_template_response()`      | Called for `TemplateResponse`             |
| `process_request()` *(old style)*  | Pre-view processing                       |
| `process_response()` *(old style)* | Post-view processing                      |

> Only `__call__` and `__init__` are required for modern middleware.

---

### **3. Adding Middleware**

In `settings.py`:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    ...
]
```

Order matters: each middleware wraps the next like a stack.

---

### **4. Built-in Middleware Examples**

| Middleware                 | Role                                        |
| -------------------------- | ------------------------------------------- |
| `SecurityMiddleware`       | Enforces HTTPS, HSTS                        |
| `SessionMiddleware`        | Enables session support                     |
| `CommonMiddleware`         | Adds common HTTP headers, URL normalization |
| `CsrfViewMiddleware`       | CSRF protection                             |
| `AuthenticationMiddleware` | Sets `request.user`                         |
| `MessageMiddleware`        | Adds support for temporary messages         |
| `LocaleMiddleware`         | Supports language preferences               |

---

### **5. Custom Middleware Example**

```python
class LogIPMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        print(f"Request from IP: {request.META['REMOTE_ADDR']}")
        return self.get_response(request)
```

Add to `settings.py`:

```python
MIDDLEWARE += ['myapp.middleware.LogIPMiddleware']
```

---

### **6. Middleware Execution Order**

* **Request phase**: Top → Bottom
* **View execution**
* **Response phase**: Bottom → Top

Each middleware wraps the request/response lifecycle.

---

### **7. Exception and Template Handling**

```python
def process_exception(self, request, exception):
    return HttpResponse("Error occurred", status=500)

def process_template_response(self, request, response):
    response.context_data['extra'] = 'value'
    return response
```

---

### **8. Use Cases**

* Logging
* Performance timing
* Authentication
* Request validation
* Response formatting
* Content compression

---
