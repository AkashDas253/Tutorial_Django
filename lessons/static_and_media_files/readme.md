## **Static and Media Files in Django**

Django separates **static files** (CSS, JS, images) from **media files** (user uploads). Static files are part of the source code; media files are dynamic and uploaded at runtime.

---

### **1. Static Files**

**Static files** are non-dynamic assets used in templates.

#### Configuration

In `settings.py`:

```python
STATIC_URL = '/static/'                  # URL to access static files
STATICFILES_DIRS = [BASE_DIR / 'static']  # Additional static dirs (for development)
STATIC_ROOT = BASE_DIR / 'staticfiles'    # For collectstatic (in production)
```

#### File Structure (example)

```
project/
├── static/
│   ├── css/
│   └── js/
└── myapp/
    └── static/
        └── myapp/
            └── style.css
```

#### Template Usage

```django
{% load static %}
<link rel="stylesheet" href="{% static 'myapp/style.css' %}">
```

#### Collecting Static Files (for production)

```bash
python manage.py collectstatic
```

---

### **2. Media Files**

**Media files** refer to user-uploaded files (images, documents, etc.).

#### Configuration

In `settings.py`:

```python
MEDIA_URL = '/media/'                  # URL for media
MEDIA_ROOT = BASE_DIR / 'media'        # Directory to store uploaded files
```

#### Serve in Development

In `urls.py`:

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

### **3. File Upload in Models**

```python
from django.db import models

class Profile(models.Model):
    avatar = models.ImageField(upload_to='avatars/')
```

* Files will be stored under `media/avatars/`

---

### **4. Display Media in Templates**

```django
<img src="{{ user.profile.avatar.url }}">
```

Ensure `MEDIA_URL` is properly set in `settings.py`.

---

### **5. Notes on Deployment**

* Static files are usually served via a CDN or web server (e.g., Nginx).
* Media files are served via cloud storage (e.g., S3) or protected paths.
* Never use Django to serve static/media files in production.

---
