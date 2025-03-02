### **Django Views Cheatsheet**  

#### **Types of Views**  
- **Function-Based Views (FBV)** – Defined using functions.  
- **Class-Based Views (CBV)** – Defined using Python classes.  
- **Generic Views** – Pre-built views for common use cases.  

#### **Function-Based Views (FBV)**  
- Defined as a Python function.  
- Uses `HttpRequest` as a parameter.  
- Returns `HttpResponse` or similar response.  

```python
from django.http import HttpResponse

def my_view(request):
    return HttpResponse("Hello, World!")
```

#### **Class-Based Views (CBV)**  
- Defined as Python classes inheriting from `View`.  
- Uses methods like `get()`, `post()`, `put()`, `delete()`.  

```python
from django.views import View
from django.http import HttpResponse

class MyView(View):
    def get(self, request):
        return HttpResponse("Hello, World!")
```

#### **Generic Class-Based Views (GCBV)**  
- Pre-built views for common patterns.  
- Reduces boilerplate code.  

| Generic View  | Purpose |
|--------------|---------|
| `TemplateView` | Renders a template. |
| `ListView` | Displays a list of objects. |
| `DetailView` | Shows details of a single object. |
| `CreateView` | Handles object creation. |
| `UpdateView` | Handles object updates. |
| `DeleteView` | Handles object deletion. |

#### **URL Mapping in Views**  
- Connects a view to a URL pattern.  

```python
from django.urls import path
from .views import my_view

urlpatterns = [
    path('my-url/', my_view, name='my-view'),
]
```

#### **Rendering Templates**  
- Uses `render()` to return an HTML template.  

```python
from django.shortcuts import render

def my_view(request):
    return render(request, 'template.html', {'key': 'value'})
```

#### **Handling Forms in Views**  
- Uses `request.POST` to handle form submissions.  

```python
def form_view(request):
    if request.method == "POST":
        data = request.POST.get("field_name")
```

#### **Redirecting in Views**  
- Uses `redirect()` to send users to another view or URL.  

```python
from django.shortcuts import redirect

def redirect_view(request):
    return redirect('target-view-name')
```

#### **Returning JSON Responses**  
- Uses `JsonResponse` for JSON data.  

```python
from django.http import JsonResponse

def json_view(request):
    return JsonResponse({"key": "value"})
```

#### **View Decorators**  
- `@login_required` – Restricts view to logged-in users.  
- `@permission_required('app.permission')` – Restricts access by permission.  
- `@csrf_exempt` – Disables CSRF protection (use cautiously).  

```python
from django.contrib.auth.decorators import login_required

@login_required
def my_protected_view(request):
    return HttpResponse("Protected content")
```
