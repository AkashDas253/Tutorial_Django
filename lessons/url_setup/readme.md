### **Django URL Setup**  

#### **Overview**  
Django's URL configuration maps web requests to specific views, allowing efficient routing and modular project organization. URLs are defined in `urls.py` files at both the project and app levels.

---

## **1. Project-Level URL Configuration**  
The main `urls.py` file (located in the project's root directory) defines the base URL patterns and includes app-level URLs.

### **Structure**  
- `urls.py` in the project folder (e.g., `my_project/urls.py`)  
- Defines routes for apps and the Django admin panel  

### **Example:**  
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),  # Admin panel
    path('', include('my_app.urls')),  # App-level URLs
]
```

- `path('admin/', admin.site.urls)`: Routes `/admin/` to Django's built-in admin panel.  
- `include('my_app.urls')`: Delegates further URL handling to `my_app/urls.py`.

---

## **2. App-Level URL Configuration**  
Each Django app has its own `urls.py` to define specific URL patterns for that app.

### **Structure**  
- `urls.py` inside each app folder (e.g., `my_app/urls.py`)  
- Maps paths to views in `views.py`  

### **Example:**  
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),  # Home page
    path('about/', views.about, name='about'),  # About page
    path('post/<int:id>/', views.post_detail, name='post_detail'),  # Dynamic URL
]
```

---

## **3. URL Path Converters**  
Django provides built-in path converters to extract parameters from URLs.

| Converter | Description | Example URL |
|-----------|------------|-------------|
| `<str:name>` | String | `/profile/john/` |
| `<int:id>` | Integer | `/post/1/` |
| `<slug:slug>` | Slug (hyphenated text) | `/blog/my-first-post/` |
| `<uuid:uid>` | UUID | `/user/550e8400-e29b-41d4-a716-446655440000/` |
| `<path:path>` | Full path | `/files/images/photo.jpg` |

### **Example:**  
```python
path('user/<int:id>/', views.profile, name='profile')
```

---

## **4. Named URLs (Reverse URL Mapping)**  
Named URLs help avoid hardcoding and allow easy changes.

### **In `urls.py`**  
```python
path('dashboard/', views.dashboard, name='dashboard')
```

### **In Templates**  
```html
<a href="{% url 'dashboard' %}">Go to Dashboard</a>
```

### **In Views (Redirect & Reverse)**  
```python
from django.shortcuts import redirect, reverse

def go_to_dashboard(request):
    return redirect(reverse('dashboard'))
```

---

## **5. Regular Expression-Based URLs (`re_path`)**  
For advanced matching, Django allows regex-based URL patterns.

| Syntax | Example | Description |
|--------|---------|-------------|
| `(?P<name>pattern)` | `(?P<year>[0-9]{4})` | Captures named parameters |
| `^text/` | `^article/` | URL must start with "article" |
| `/text$` | `/post$` | URL must end with "post" |

### **Example:**  
```python
from django.urls import re_path
re_path(r'^article/(?P<year>[0-9]{4})/$', views.article)
```

---

## **6. Class-Based View (CBV) URL Patterns**  
Instead of function-based views, Django supports Class-Based Views (CBVs).

| CBV Type | Example | Description |
|----------|---------|-------------|
| `TemplateView` | `path('about/', TemplateView.as_view(template_name='about.html'))` | Static template rendering |
| `ListView` | `path('posts/', PostListView.as_view(), name='post-list')` | List objects |
| `DetailView` | `path('post/<int:pk>/', PostDetailView.as_view(), name='post-detail')` | Display object details |
| `CreateView` | `path('post/new/', PostCreateView.as_view(), name='post-create')` | Handle form submission |
| `UpdateView` | `path('post/<int:pk>/edit/', PostUpdateView.as_view(), name='post-update')` | Handle updates |
| `DeleteView` | `path('post/<int:pk>/delete/', PostDeleteView.as_view(), name='post-delete')` | Handle deletions |

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

## **7. Including Namespaces in URL Routing**  
Namespaces help avoid conflicts in multi-app projects.

### **Defining a Namespace (`app/urls.py`)**  
```python
app_name = 'blog'

urlpatterns = [
    path('', views.home, name='home'),
]
```

### **Using the Namespace in Templates**  
```html
<a href="{% url 'blog:home' %}">Blog Home</a>
```

---

## **8. Handling 404 Errors (Custom Error Pages)**  
Django allows custom 404 error handling.

### **Custom 404 View (`views.py`)**  
```python
from django.shortcuts import render

def custom_404(request, exception):
    return render(request, '404.html', status=404)
```

### **Registering in `urls.py`**  
```python
handler404 = 'my_app.views.custom_404'
```

---

## **9. Restricting HTTP Methods in URLs**  
Django provides decorators to limit HTTP methods.

| Decorator | Description |
|-----------|-------------|
| `@require_GET` | Allows only `GET` requests |
| `@require_POST` | Allows only `POST` requests |
| `@require_http_methods(["GET", "POST"])` | Restricts to specific methods |

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

## **10. Best Practices for URL Routing**  
| Best Practice | Benefit |
|--------------|---------|
| Use `name` attributes in URLs | Avoids hardcoding paths |
| Organize URLs with `include()` | Keeps large projects manageable |
| Use `reverse()` instead of hardcoded links | Enhances maintainability |
| Avoid exposing sensitive data in URLs | Improves security |
| Use namespaces in multi-app projects | Prevents URL conflicts |

---
