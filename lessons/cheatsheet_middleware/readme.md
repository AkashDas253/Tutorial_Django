### **Django Middleware Cheatsheet**  

#### **What is Middleware?**  
- Middleware processes requests **before** reaching the view and responses **before** returning to the client.  
- Used for authentication, security, logging, etc.  

---

### **Default Middleware in `settings.py`**  
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',  # Security enhancements
    'django.contrib.sessions.middleware.SessionMiddleware',  # Session management
    'django.middleware.common.CommonMiddleware',  # Handles redirects, URL rewriting
    'django.middleware.csrf.CsrfViewMiddleware',  # CSRF protection
    'django.middleware.authentication.AuthenticationMiddleware',  # User authentication
    'django.middleware.clickjacking.XFrameOptionsMiddleware',  # Prevents clickjacking
]
```

---

### **Custom Middleware**  

#### **Creating a Middleware (`middleware.py`)**  
```python
from django.utils.timezone import now

class RequestTimeLoggerMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        start_time = now()
        response = self.get_response(request)
        duration = now() - start_time
        print(f"Request took {duration.total_seconds()} seconds")
        return response
```

#### **Adding Custom Middleware to `settings.py`**  
```python
MIDDLEWARE.append('my_app.middleware.RequestTimeLoggerMiddleware')
```

---

### **Types of Middleware Methods**  

| Method | Description |
|--------|-------------|
| `__init__(get_response)` | Called once when the server starts. |
| `__call__(request)` | Processes requests before and after reaching the view. |
| `process_view(request, view_func, view_args, view_kwargs)` | Runs **before** the view function executes. |
| `process_exception(request, exception)` | Handles exceptions raised in views. |
| `process_template_response(request, response)` | Modifies template responses **before** rendering. |

---

### **Example: Blocking Requests Based on IP**  
```python
from django.http import HttpResponseForbidden

class BlockIPMiddleware:
    BLOCKED_IPS = ['123.456.789.000']

    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        if request.META['REMOTE_ADDR'] in self.BLOCKED_IPS:
            return HttpResponseForbidden("Access Denied")
        return self.get_response(request)
```

---
