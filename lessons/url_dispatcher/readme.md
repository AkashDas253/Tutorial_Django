## **Django URL Dispatcher**

The URL dispatcher in Django maps **requested URLs to views** using patterns defined in `urls.py`. It is a critical part of Django’s **MTV (Model-Template-View)** architecture.

---

### **1. Purpose**

* Connects **incoming requests** (URLs) to **views**.
* Enables **modular URL configurations** using `include()`.
* Allows **named URLs**, **dynamic parameters**, and **path converters**.

---

### **2. Project-Level `urls.py`**

Located at `projectname/urls.py`. Example:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('appname.urls')),  # Delegates to app-level URLs
]
```

---

### **3. App-Level `urls.py`**

Manages URLs specific to an app. Create this file manually in each app.

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('item/<int:id>/', views.item_detail, name='item_detail'),
]
```

---

### **4. Path Syntax and Parameters**

```python
path('path/', view, name='name')
```

#### **Path Converters:**

| Converter | Matches                    | Passed As |
| --------- | -------------------------- | --------- |
| `str`     | Non-empty string (default) | `str`     |
| `int`     | Integers                   | `int`     |
| `slug`    | Letters, numbers, hyphens  | `str`     |
| `uuid`    | UUID strings               | `UUID`    |
| `path`    | Like str but includes `/`  | `str`     |

---

### **5. Regex URLs (Deprecated in Django 2.0+)**

```python
from django.urls import re_path

urlpatterns = [
    re_path(r'^article/(?P<year>[0-9]{4})/$', views.article_by_year),
]
```

Prefer `path()` over `re_path()` unless complex patterns are needed.

---

### **6. Named URLs**

Helps reverse-lookup URL paths:

```python
path('home/', views.home, name='home')
```

Usage in templates:

```html
<a href="{% url 'home' %}">Home</a>
```

In views:

```python
from django.urls import reverse
reverse('home')  # Returns '/home/'
```

---

### **7. `include()` Function**

Used to split URL configurations by app:

```python
path('blog/', include('blog.urls'))
```

* Keeps project maintainable.
* Allows app reuse.

---

### **8. Serving Static and Media Files (Dev Only)**

In `project/urls.py`:

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

---

### **9. 404 and 500 Handlers**

Custom error views in `urls.py`:

```python
handler404 = 'appname.views.custom_404'
handler500 = 'appname.views.custom_500'
```

---

### **10. `url()` Function (Obsolete)**

* Removed in Django 4.x
* Previously used with regex, now replaced by `path()` and `re_path()`.

---
