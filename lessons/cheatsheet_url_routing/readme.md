### **Django URL Routing Cheatsheet**  

#### **Purpose:**  
- Maps URLs to views.  
- Defined in `urls.py`.  

---

### **Project-Level URL Configuration (`my_project/urls.py`)**  
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),  # Admin panel
    path('', include('my_app.urls')),  # Include app-level URLs
]
```

---

### **App-Level URL Configuration (`my_app/urls.py`)**  
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

### **Path Converters**  

| Converter | Description | Example |
|-----------|------------|---------|
| `<str:name>` | String | `/profile/john/` |
| `<int:id>` | Integer | `/post/1/` |
| `<slug:slug>` | Slug (hyphenated text) | `/blog/my-first-post/` |
| `<uuid:uid>` | UUID | `/user/550e8400-e29b-41d4-a716-446655440000/` |
| `<path:path>` | Full path | `/files/images/photo.jpg` |

---

### **Named URLs (Reverse URL Mapping)**  
- Used to avoid hardcoded URLs.  

#### **In `urls.py`**  
```python
path('dashboard/', views.dashboard, name='dashboard')
```

#### **In Template (`href` with `url` tag)**  
```html
<a href="{% url 'dashboard' %}">Dashboard</a>
```

#### **In Views (`redirect` & `reverse`)**  
```python
from django.shortcuts import redirect, reverse

def go_to_dashboard(request):
    return redirect(reverse('dashboard'))
```

---

### **Including Multiple URL Configurations**  
- Helps organize large projects by splitting routes.  

#### **In `my_project/urls.py`**  
```python
urlpatterns = [
    path('blog/', include('blog.urls')),  # Includes URLs from the blog app
]
```

---

### **Handling 404 Pages**  
#### **Custom 404 View (`views.py`)**  
```python
from django.shortcuts import render

def custom_404(request, exception):
    return render(request, '404.html', status=404)
```

#### **In `settings.py`**  
```python
handler404 = 'my_app.views.custom_404'
```

---
