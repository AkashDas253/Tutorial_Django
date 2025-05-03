## 🧠 **Django Cheatsheet**

---

### 🔧 Project Setup

```bash
django-admin startproject projectname
cd projectname
python manage.py startapp appname
```

**Structure**:

```
project/
├── manage.py
├── project/
│   └── settings.py, urls.py, asgi.py, wsgi.py
└── app/
    └── models.py, views.py, urls.py, admin.py, forms.py
```

---

### ⚙️ `settings.py` Highlights

| Setting                                         | Purpose                |
| ----------------------------------------------- | ---------------------- |
| `INSTALLED_APPS`                                | Registered Django apps |
| `DATABASES`                                     | DB config              |
| `TEMPLATES`                                     | Template settings      |
| `STATIC_URL`, `STATICFILES_DIRS`, `STATIC_ROOT` | Static files           |
| `MEDIA_URL`, `MEDIA_ROOT`                       | Media files            |
| `AUTH_USER_MODEL`                               | Custom user model      |
| `LOGIN_URL`, `LOGIN_REDIRECT_URL`               | Auth redirection       |

---

### 🗺️ URL Dispatcher

**Project-level**:

```python
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("app/", include("app.urls")),
]
```

**App-level**:

```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

---

### 🧠 Views

| Type           | Syntax                                        |
| -------------- | --------------------------------------------- |
| Function-Based | `def view(request): return render(...)`       |
| Class-Based    | `class MyView(View): def get(self, request):` |

---

### 📄 Templates

| Tag                         | Usage                |
| --------------------------- | -------------------- |
| `{{ variable }}`            | Output variable      |
| `{% if %}` / `{% for %}`    | Logic                |
| `{% extends "base.html" %}` | Template inheritance |
| `{% block content %}`       | Define block         |
| `{% include %}`             | Include template     |

---

### 🧱 Models

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)
    body = models.TextField()
```

**Model Commands**:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 🛠️ Admin

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

---

### 🧾 Forms

```python
from django import forms

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = "__all__"
```

---

### 🗃️ ORM Queries

| Query  | Example                           |
| ------ | --------------------------------- |
| All    | `Post.objects.all()`              |
| Filter | `Post.objects.filter(title="A")`  |
| Get    | `Post.objects.get(id=1)`          |
| Create | `Post.objects.create(...)`        |
| Update | `post.title = "New"; post.save()` |
| Delete | `post.delete()`                   |

---

### 🔐 Auth

| Function                          | Use           |
| --------------------------------- | ------------- |
| `authenticate`, `login`, `logout` | Auth process  |
| `@login_required`                 | Restrict view |
| `User.objects.create_user(...)`   | Create user   |

---

### 🍪 Sessions & Cookies

| Session | Example                          |
| ------- | -------------------------------- |
| Set     | `request.session["key"] = value` |
| Get     | `request.session.get("key")`     |
| Delete  | `del request.session["key"]`     |

---

### ⚙️ Middleware

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    ...
]
```

Custom middleware: define `__call__` or `process_request/response`.

---

### 📶 Signals

```python
from django.db.models.signals import post_save
@receiver(post_save, sender=User)
def create_profile(...): ...
```

---

### ✅ Testing

```bash
python manage.py test
```

```python
from django.test import TestCase

class TestModels(TestCase):
    def test_something(self): ...
```

---

### 🖼️ Static & Media

```python
STATIC_URL = "/static/"
MEDIA_URL = "/media/"
```

In `urls.py` (dev only):

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

### 🚀 Deployment (Gunicorn + Nginx + DB)

* Set `DEBUG = False`, `ALLOWED_HOSTS`
* Use `collectstatic`
* Configure WSGI server + static/media paths

---

### 🛡️ Security Best Practices

* `SECURE_SSL_REDIRECT = True`
* `CSRF_COOKIE_SECURE = True`
* `SESSION_COOKIE_SECURE = True`
* Use `.env` files for secrets

---

### 🌍 i18n / l10n

```bash
django-admin makemessages -l fr
django-admin compilemessages
```

Enable in `settings.py`:

```python
USE_I18N = True
USE_L10N = True
```

---

### 🚀 Performance & Caching

```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.locmem.LocMemCache",
    }
}
```

Use decorators: `@cache_page(60 * 15)`

---

### 🕒 Celery Setup (Asynchronous Tasks)

```bash
celery -A project worker -l info
```

```python
@app.task
def add(x, y): return x + y
```

---

### 🔌 Channels (WebSockets)

Install `channels`, configure `ASGI_APPLICATION`, use `Consumers`.

---

### 🔍 Debug Toolbar

```bash
pip install django-debug-toolbar
```

Add to `INSTALLED_APPS`, `MIDDLEWARE`, and `urlpatterns`.

---

### 🔑 Allauth + Social Auth

```bash
pip install django-allauth
```

Add `allauth`, `account`, `socialaccount` to `INSTALLED_APPS`.

Use `path("accounts/", include("allauth.urls"))`

---
