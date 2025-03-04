### **HTMX & Django Cheatsheet**  

HTMX allows Django to handle AJAX-like requests without writing JavaScript, making dynamic UIs easier.

---

## **1. Installation**  

### **Include HTMX in Your Project**
#### **Using CDN (Recommended)**
```html
<script src="https://unpkg.com/htmx.org@1.9.6"></script>
```
#### **Using Django Static Files**
```sh
pip install django-htmx
```
Add `django_htmx` to `INSTALLED_APPS` in `settings.py`:
```python
INSTALLED_APPS = [
    ...,
    "django_htmx",
]
```

---

## **2. Using HTMX with Django Views**  

### **Django View Example (`views.py`)**
```python
from django.shortcuts import render
from django.http import HttpResponse

def update_text(request):
    return HttpResponse("<p>Updated Text!</p>")
```

### **Django URL Configuration (`urls.py`)**
```python
from django.urls import path
from .views import update_text

urlpatterns = [
    path("update-text/", update_text, name="update-text"),
]
```

### **HTMX in Template (`index.html`)**
```html
<button hx-get="{% url 'update-text' %}" hx-target="#output">
    Click Me
</button>
<div id="output"></div>
```
| **HTMX Attribute** | **Function** |
|-------------------|-------------|
| `hx-get` | Sends a `GET` request. |
| `hx-target` | Updates the specified element. |
| `hx-trigger` | Specifies the event (e.g., `click`). |

---

## **3. Handling Forms with HTMX**  

### **Django View (`views.py`)**
```python
from django.shortcuts import render
from django.http import JsonResponse

def submit_form(request):
    if request.method == "POST":
        name = request.POST.get("name")
        return JsonResponse({"message": f"Hello, {name}!"})
    return render(request, "form.html")
```

### **Template (`form.html`)**
```html
<form hx-post="{% url 'submit-form' %}" hx-target="#message">
    <input type="text" name="name" placeholder="Enter Name">
    <button type="submit">Submit</button>
</form>
<div id="message"></div>
```

---

## **4. Server-Side Pagination with HTMX**  

### **Django View (`views.py`)**
```python
from django.core.paginator import Paginator
from django.shortcuts import render
from myapp.models import Product

def product_list(request):
    page = request.GET.get("page", 1)
    products = Paginator(Product.objects.all(), 5).get_page(page)
    return render(request, "products.html", {"products": products})
```

### **Template (`products.html`)**
```html
<div id="product-list">
    {% for product in products %}
        <p>{{ product.name }}</p>
    {% endfor %}
</div>

<button hx-get="?page={{ products.next_page_number }}" hx-target="#product-list" 
        {% if not products.has_next %}disabled{% endif %}>
    Load More
</button>
```

---

## **5. HTMX with Django Authentication**  

### **Login View (`views.py`)**
```python
from django.contrib.auth import authenticate, login
from django.http import JsonResponse

def login_view(request):
    if request.method == "POST":
        username = request.POST["username"]
        password = request.POST["password"]
        user = authenticate(request, username=username, password=password)
        if user:
            login(request, user)
            return JsonResponse({"success": True})
        return JsonResponse({"success": False, "error": "Invalid credentials"})
```

### **Login Form (`login.html`)**
```html
<form hx-post="{% url 'login' %}" hx-target="#login-message">
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <button type="submit">Login</button>
</form>
<div id="login-message"></div>
```

---

## **6. HTMX Swap Strategies**
| **HTMX Attribute** | **Description** |
|-------------------|----------------|
| `hx-swap="outerHTML"` | Replaces the entire element. |
| `hx-swap="innerHTML"` | Updates only the inner content. |
| `hx-swap="beforebegin"` | Inserts before the element. |
| `hx-swap="afterbegin"` | Inserts at the start of the element. |
| `hx-swap="beforeend"` | Inserts at the end of the element. |
| `hx-swap="afterend"` | Inserts after the element. |

Example:
```html
<button hx-get="/update/" hx-swap="beforeend" hx-target="#container">
    Append Content
</button>
<div id="container"></div>
```

---

## **7. WebSockets with HTMX & Django Channels**
```html
<div hx-ext="ws" ws-connect="/ws/chat/">
    <p ws-send>Send Message</p>
    <div ws-receive></div>
</div>
```

---
