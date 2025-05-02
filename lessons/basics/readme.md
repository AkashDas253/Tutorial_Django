## **Django Basics – Comprehensive Note**

### **Introduction**

* Django is a **high-level Python web framework** that promotes rapid development and clean, pragmatic design.
* Follows the **Model-View-Template (MVT)** architectural pattern.
* Built to be **secure, scalable, and maintainable**.

---

### **Key Features**

* **Object-Relational Mapper (ORM)**: Maps database models to Python classes.
* **Automatic Admin Interface**: Provides a web-based admin UI for managing application data.
* **Built-in Authentication**: User, group, and permission management.
* **URL Routing**: Clean URL design using `urls.py`.
* **Templating Engine**: Django Template Language (DTL) for rendering HTML dynamically.
* **Form Handling**: Simplifies form creation, validation, and processing.
* **Security**: CSRF, XSS, SQL injection protection by default.
* **Internationalization**: i18n/l10n support.

---

### **Project Structure**

When a Django project is created, it has the following structure:

```
myproject/
├── manage.py
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
└── myapp/
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    ├── views.py
    ├── urls.py
    └── migrations/
```

---

### **Core Concepts**

#### **1. Models**

* Define **data schema** using Python classes.
* Use Django ORM to create, query, update, and delete data.

```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=50)
```

---

#### **2. Views**

* Define **logic to process requests and return responses**.

```python
from django.http import HttpResponse

def hello(request):
    return HttpResponse("Hello, world!")
```

---

#### **3. Templates**

* Define the **presentation layer** using `.html` files and template tags.

```html
<h1>Hello, {{ user.username }}</h1>
```

---

#### **4. URLs**

* Define URL patterns and route them to views.

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.hello, name='hello'),
]
```

---

#### **5. Forms**

* Create and validate input forms easily.

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField()
    email = forms.EmailField()
```

---

#### **6. Admin Interface**

* Register models to manage them via a web UI.

```python
from django.contrib import admin
from .models import Book

admin.site.register(Book)
```

---

### **Common Commands**

| Command                               | Purpose                     |
| ------------------------------------- | --------------------------- |
| `django-admin startproject myproject` | Create a new Django project |
| `python manage.py startapp myapp`     | Create a new app            |
| `python manage.py runserver`          | Run development server      |
| `python manage.py makemigrations`     | Create new migrations       |
| `python manage.py migrate`            | Apply migrations            |
| `python manage.py createsuperuser`    | Create admin user           |
| `python manage.py shell`              | Open Django shell           |

---

### **MVT Pattern**

| Component | Description                         |
| --------- | ----------------------------------- |
| Model     | Handles the database schema and ORM |
| View      | Contains the business logic         |
| Template  | Renders HTML with data              |

---

### **Django Settings (`settings.py`)**

* Configures:

  * Installed apps (`INSTALLED_APPS`)
  * Middleware (`MIDDLEWARE`)
  * Templates (`TEMPLATES`)
  * Databases (`DATABASES`)
  * Static and media files
  * Security options

---

### **App Lifecycle**

1. **Request** hits Django through WSGI/ASGI.
2. Django processes it via **middleware**.
3. **URL resolver** maps it to a view.
4. View processes data using **models**.
5. View passes context to **template**.
6. **Response** returned to the browser.

---
