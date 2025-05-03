## **Django Views**

A **View** in Django is a Python function or class that takes a web request and returns a web response. Views contain the logic that connects **models and templates**, forming the core of the **MTV architecture**.

---

### **1. Purpose of Views**

* Handle business logic.
* Interact with models.
* Return responses: HTML, JSON, files, redirects, etc.

---

### **2. Function-Based Views (FBV)**

#### **Basic Syntax**

```python
from django.http import HttpResponse

def my_view(request):
    return HttpResponse("Hello, world!")
```

#### **Returning Template**

```python
from django.shortcuts import render

def home(request):
    return render(request, 'home.html', {'title': 'Home Page'})
```

---

### **3. Class-Based Views (CBV)**

Built using OOP principles. Useful for code reuse and built-in behavior.

#### **Import**

```python
from django.views import View
from django.http import HttpResponse
```

#### **Basic View**

```python
class MyView(View):
    def get(self, request):
        return HttpResponse('GET request response')

    def post(self, request):
        return HttpResponse('POST request response')
```

---

### **4. Built-in Generic Views**

Django provides powerful built-in CBVs for common use cases.

#### **Examples:**

| View Class     | Purpose                 |
| -------------- | ----------------------- |
| `TemplateView` | Render static templates |
| `ListView`     | Display list of objects |
| `DetailView`   | Display a single object |
| `CreateView`   | Create an object        |
| `UpdateView`   | Update an object        |
| `DeleteView`   | Delete an object        |
| `RedirectView` | Redirect to another URL |

---

### **5. Common Response Types**

| Function                 | Description                        |
| ------------------------ | ---------------------------------- |
| `HttpResponse()`         | Raw response with optional content |
| `render()`               | Combines template + context → HTML |
| `redirect()`             | Redirect to a URL                  |
| `JsonResponse()`         | Returns JSON data                  |
| `HttpResponseNotFound()` | 404 response                       |
| `HttpResponseRedirect()` | HTTP 302 response                  |

---

### **6. `render()` Function**

```python
render(request, template_name, context=None, content_type=None, status=None, using=None)
```

* Renders a template with context data.
* Shortcut for loading templates + returning `HttpResponse`.

---

### **7. `get_object_or_404()`**

Used in detail views to fetch a model instance or raise 404 if not found.

```python
from django.shortcuts import get_object_or_404

obj = get_object_or_404(MyModel, pk=5)
```

---

### **8. View Decorators**

| Decorator                                | Purpose                              |
| ---------------------------------------- | ------------------------------------ |
| `@login_required`                        | Restrict view to authenticated users |
| `@require_http_methods(['GET', 'POST'])` | Restrict to allowed HTTP methods     |
| `@csrf_exempt`                           | Disable CSRF protection              |
| `@permission_required('app.permission')` | Check specific permissions           |

---

### **9. Passing Context to Templates**

```python
def profile(request):
    context = {
        'username': 'JohnDoe',
        'age': 25
    }
    return render(request, 'profile.html', context)
```

---

### **10. Class-Based Generic View Example (ListView)**

```python
from django.views.generic import ListView
from .models import Article

class ArticleListView(ListView):
    model = Article
    template_name = 'article_list.html'
    context_object_name = 'articles'
```

---

### **11. URLs Linking to Views**

```python
from . import views
from django.urls import path

urlpatterns = [
    path('home/', views.home, name='home'),  # FBV
    path('articles/', ArticleListView.as_view(), name='article_list'),  # CBV
]
```

---
