### **Django Static & Media Files Cheatsheet**  

---

### **Static Files**  
#### **Purpose:**  
- Store **CSS, JavaScript, images**, etc.  

#### **Configuration (In `settings.py`)**  
```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]  # For project-level static files
STATIC_ROOT = BASE_DIR / "staticfiles"  # For production (collectstatic)
```

#### **Using Static Files in Templates**  
```html
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<img src="{% static 'images/logo.png' %}" alt="Logo">
```

#### **Collecting Static Files for Deployment**  
```sh
python manage.py collectstatic
```

---

### **Media Files**  
#### **Purpose:**  
- Store **user-uploaded files** (e.g., profile pictures, documents).  

#### **Configuration (In `settings.py`)**  
```python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "media"  # Folder where uploaded files are stored
```

#### **Serving Media Files in Development (`urls.py`)**  
```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # Other URLs...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

#### **Uploading Media in Models**  
```python
from django.db import models

class Profile(models.Model):
    avatar = models.ImageField(upload_to='avatars/')  # Uploads to media/avatars/
```

#### **Displaying Media in Templates**  
```html
<img src="{{ profile.avatar.url }}" alt="Profile Image">
```
