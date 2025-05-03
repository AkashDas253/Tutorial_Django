## **Django Project Setup**

### **1. Prerequisites**

* Python (recommended: ≥ 3.8)
* pip (Python package manager)
* Virtual environment (recommended: `venv`, `virtualenv`, or `pipenv`)

---

### **2. Install Django**

```bash
pip install django
```

Check installation:

```bash
django-admin --version
```

---

### **3. Create a Django Project**

```bash
django-admin startproject projectname
```

Creates the following structure:

```
projectname/
├── manage.py
└── projectname/
    ├── __init__.py
    ├── asgi.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

---

### **4. Start Development Server**

```bash
cd projectname
python manage.py runserver
```

Access: `http://127.0.0.1:8000/`

---

### **5. Create a Django App**

```bash
python manage.py startapp appname
```

Creates:

```
appname/
├── admin.py
├── apps.py
├── models.py
├── views.py
├── tests.py
├── migrations/
│   └── __init__.py
└── __init__.py
```

---

### **6. Register App in Project**

In `projectname/settings.py`, add to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    'appname',
]
```

---

### **7. Define Models**

In `appname/models.py`:

```python
from django.db import models

class Item(models.Model):
    name = models.CharField(max_length=100)
    price = models.FloatField()
```

---

### **8. Create and Apply Migrations**

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **9. Create a Superuser**

```bash
python manage.py createsuperuser
```

Follow prompts: username, email, password.

---

### **10. Register Models in Admin**

In `appname/admin.py`:

```python
from django.contrib import admin
from .models import Item

admin.site.register(Item)
```

Access: `http://127.0.0.1:8000/admin/`

---

### **11. Create Views**

In `appname/views.py`:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Welcome to Django")
```

---

### **12. Configure URLs**

#### Project-level `urls.py` (`projectname/urls.py`):

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('appname.urls')),
]
```

#### App-level `urls.py` (`appname/urls.py`):

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

---

### **13. Templates Configuration**

In `settings.py` under `TEMPLATES['DIRS']`:

```python
'DIRS': [BASE_DIR / 'templates'],
```

Create `templates/` folder and add HTML files.

---

### **14. Static Files Configuration**

In `settings.py`:

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```

Create a `static/` directory.

---

### **15. Development vs Production**

* Use `DEBUG = True` for development.
* Use `ALLOWED_HOSTS = ['*']` in development; specify domains in production.
* Set up WSGI/ASGI for deployment.

---

### **16. Environment Variables (Recommended)**

Use `python-decouple` or `.env` files to manage secrets:

```bash
pip install python-decouple
```

Then in `settings.py`:

```python
from decouple import config
SECRET_KEY = config('SECRET_KEY')
```

---

### **17. Virtual Environment Setup (Optional but Best Practice)**

```bash
python -m venv env
source env/bin/activate  # On Linux/macOS
env\Scripts\activate     # On Windows
```

---
