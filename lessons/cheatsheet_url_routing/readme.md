## **Django URL Routing Cheatsheet (Including CBV)**  

### **Basic URL Configuration**  
| Syntax | Description |
|--------|------------|
| `path('route/', view_function, name='name')` | Maps a URL to a function-based view (FBV). |
| `path('route/', ClassView.as_view(), name='name')` | Maps a URL to a class-based view (CBV). |
| `re_path(r'regex/', view_function)` | Matches a regex-based URL pattern. |
| `include('app.urls')` | Includes another app’s URL configuration. |

**Example:**  
```python
from django.urls import path
from . import views

urlpatterns = [
    path('home/', views.home_view, name='home'),  # FBV
    path('dashboard/', views.DashboardView.as_view(), name='dashboard'),  # CBV
]
```

---

### **Path Converters**  
| Converter | Example | Matches |
|-----------|---------|---------|
| `str` | `<str:username>/` | Any non-empty string. |
| `int` | `<int:id>/` | Integer values. |
| `slug` | `<slug:slug_val>/` | Alphanumeric and hyphens. |
| `uuid` | `<uuid:uuid_val>/` | UUID values. |
| `path` | `<path:subpath>/` | Full path including `/`. |

**Example:**  
```python
path('user/<int:id>/', views.profile, name='profile')
```

---

### **Regular Expression-Based URLs**  
| Syntax | Example | Description |
|--------|---------|-------------|
| `(?P<name>pattern)` | `(?P<year>[0-9]{4})` | Captures named parameters. |
| `^text/` | `^article/` | URL must start with "article". |
| `/text$` | `/post$` | URL must end with "post". |

**Example:**  
```python
re_path(r'^article/(?P<year>[0-9]{4})/$', views.article)
```

---

### **Including Other URL Configurations**  
| Syntax | Description |
|--------|-------------|
| `path('app/', include('app.urls'))` | Routes requests to `app.urls`. |

**Example:**  
```python
from django.urls import include
path('blog/', include('blog.urls'))
```

---

### **URL Patterns for Class-Based Views (CBVs)**  
| CBV Type | Example | Description |
|----------|---------|-------------|
| `TemplateView` | `path('about/', TemplateView.as_view(template_name='about.html'))` | Renders a template without logic. |
| `ListView` | `path('posts/', PostListView.as_view(), name='post-list')` | Displays a list of objects. |
| `DetailView` | `path('post/<int:pk>/', PostDetailView.as_view(), name='post-detail')` | Shows details of a specific object. |
| `CreateView` | `path('post/new/', PostCreateView.as_view(), name='post-create')` | Handles form submission for object creation. |
| `UpdateView` | `path('post/<int:pk>/edit/', PostUpdateView.as_view(), name='post-update')` | Handles form submission for object updates. |
| `DeleteView` | `path('post/<int:pk>/delete/', PostDeleteView.as_view(), name='post-delete')` | Deletes an object and redirects. |

---

### **Named URLs and Reverse URL Resolution**  
| Function | Description |
|----------|-------------|
| `{% url 'name' %}` | Generates a URL in templates. |
| `reverse('name')` | Returns the URL for a named path. |

**Example:**  
```python
from django.urls import reverse
reverse('home')  # Returns '/'
```

---

### **Namespace for URL Naming**  
| Syntax | Description |
|--------|-------------|
| `app_name = 'namespace'` | Defines a namespace for app URLs. |
| `{% url 'namespace:name' %}` | Calls a URL within a namespace. |

**Example:**  
```python
app_name = 'blog'

urlpatterns = [
    path('', views.home, name='home'),
]
```

Referencing in templates:  
```html
<a href="{% url 'blog:home' %}">Blog Home</a>
```

---

### **Handling 404 Errors**  
| Syntax | Description |
|--------|-------------|
| `handler404 = 'app.views.custom_404'` | Defines a custom 404 error page. |

**Example:**  
```python
def custom_404(request, exception):
    return render(request, '404.html', status=404)
```

---

### **HTTP Methods Restrictions**  
| Decorator | Description |
|-----------|-------------|
| `@require_GET` | Allows only `GET` requests. |
| `@require_POST` | Allows only `POST` requests. |
| `@require_http_methods(["GET", "POST"])` | Restricts allowed HTTP methods. |

**Example:**  
```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    return HttpResponse("Hello!")
```

For CBVs, use `method_decorator`:  
```python
from django.utils.decorators import method_decorator
from django.views import View

class MyView(View):
    @method_decorator(require_http_methods(["GET", "POST"]))
    def dispatch(self, request, *args, **kwargs):
        return HttpResponse("Hello from CBV!")
```

---

### **Best Practices**  
| Tip | Reason |
|-----|--------|
| Use `name` attributes | Allows easier URL management. |
| Use `include()` for large projects | Keeps URLs modular and maintainable. |
| Avoid exposing sensitive data in URLs | Enhances security. |
| Use `reverse()` instead of hardcoded URLs | Improves flexibility. |
