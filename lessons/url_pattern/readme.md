### **Django URL Pattern – Comprehensive Note**  

#### **Overview**  
Django uses URL patterns to define routing rules that map user requests to specific views. URL patterns are declared in `urls.py` using `path()` and `re_path()` functions.

---

## **1. Basic URL Pattern Structure**  
Each URL pattern follows this structure:  
```python
path('route/', view, name='name')
```
- **`route`**: Defines the URL path (e.g., `'home/'`).
- **`view`**: Calls the corresponding view function or class-based view.
- **`name`**: Assigns a unique name for referencing the URL.

### **Example:**  
```python
from django.urls import path
from . import views

urlpatterns = [
    path('home/', views.home, name='home'),
    path('about/', views.about, name='about'),
]
```

---

## **2. URL Path Converters**  
Django provides built-in path converters to capture values from URLs.

| Converter | Description | Example URL | Captured Value |
|-----------|------------|-------------|----------------|
| `<str:name>` | Matches a string (excluding `/`) | `/user/john/` | `"john"` |
| `<int:id>` | Matches an integer | `/post/5/` | `5` |
| `<slug:slug>` | Matches a slug (letters, numbers, hyphens, underscores) | `/blog/my-post/` | `"my-post"` |
| `<uuid:uid>` | Matches a UUID | `/user/550e8400-e29b-41d4-a716-446655440000/` | `UUID` object |
| `<path:path>` | Matches a full path including `/` | `/media/uploads/photo.jpg` | `"uploads/photo.jpg"` |

### **Example:**  
```python
urlpatterns = [
    path('user/<str:name>/', views.profile, name='profile'),
    path('post/<int:id>/', views.post_detail, name='post_detail'),
    path('blog/<slug:slug>/', views.blog_detail, name='blog_detail'),
]
```

---

## **3. Named URL Patterns**  
Using `name` in `path()` allows dynamic URL referencing.

### **Defining in `urls.py`:**  
```python
path('dashboard/', views.dashboard, name='dashboard')
```

### **Using in Templates:**  
```html
<a href="{% url 'dashboard' %}">Go to Dashboard</a>
```

### **Using in Views:**  
```python
from django.shortcuts import redirect, reverse

def go_to_dashboard(request):
    return redirect(reverse('dashboard'))
```

---

## **4. Regular Expression-Based URL Patterns (`re_path`)**  
Django supports regex-based URLs using `re_path()`.

| Regex | Example | Description |
|--------|---------|-------------|
| `^text/` | `^article/` | Must start with "article" |
| `/text$` | `/post$` | Must end with "post" |
| `(?P<name>pattern)` | `(?P<year>[0-9]{4})` | Captures named parameters |

### **Example:**  
```python
from django.urls import re_path

urlpatterns = [
    re_path(r'^article/(?P<year>[0-9]{4})/$', views.article),
]
```

---

## **5. Class-Based View (CBV) URL Patterns**  
CBVs use `.as_view()` to handle requests.

| CBV Type | Example | Description |
|----------|---------|-------------|
| `TemplateView` | `path('about/', TemplateView.as_view(template_name='about.html'))` | Static template rendering |
| `ListView` | `path('posts/', PostListView.as_view(), name='post-list')` | List objects |
| `DetailView` | `path('post/<int:pk>/', PostDetailView.as_view(), name='post-detail')` | Display object details |

### **Example:**  
```python
from django.views.generic import TemplateView, ListView
from .models import Post

urlpatterns = [
    path('about/', TemplateView.as_view(template_name='about.html'), name='about'),
    path('posts/', ListView.as_view(model=Post, template_name='post_list.html'), name='post-list'),
]
```

---

## **6. Including Namespaces in URL Routing**  
Namespaces help manage multiple apps.

### **In `app/urls.py`:**  
```python
app_name = 'blog'

urlpatterns = [
    path('', views.home, name='home'),
]
```

### **Using in Templates:**  
```html
<a href="{% url 'blog:home' %}">Blog Home</a>
```

---

## **7. Custom Error Handling in URL Patterns**  
Custom views for handling errors like 404.

### **Define Custom 404 View:**  
```python
from django.shortcuts import render

def custom_404(request, exception):
    return render(request, '404.html', status=404)
```

### **Register in `urls.py`:**  
```python
handler404 = 'my_app.views.custom_404'
```

---

## **8. Restricting HTTP Methods in URL Patterns**  
Limit allowed HTTP methods for a view.

| Decorator | Description |
|-----------|-------------|
| `@require_GET` | Allows only `GET` requests |
| `@require_POST` | Allows only `POST` requests |
| `@require_http_methods(["GET", "POST"])` | Allows specific methods |

### **Example (FBV)**  
```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    return HttpResponse("Hello!")
```

### **Example (CBV with `method_decorator`)**  
```python
from django.utils.decorators import method_decorator
from django.views import View

class MyView(View):
    @method_decorator(require_http_methods(["GET", "POST"]))
    def dispatch(self, request, *args, **kwargs):
        return HttpResponse("Hello from CBV!")
```

---

## **9. Best Practices for URL Patterns**  
| Best Practice | Benefit |
|--------------|---------|
| Use `name` attributes in URLs | Avoids hardcoding paths |
| Organize URLs with `include()` | Keeps projects modular |
| Use `reverse()` instead of hardcoded links | Enhances maintainability |
| Avoid exposing sensitive data in URLs | Improves security |
| Use namespaces in multi-app projects | Prevents URL conflicts |

---
