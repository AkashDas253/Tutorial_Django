## **Django Static & Media Files**  

### **Static Files**  
#### **Purpose:**  
- Store non-user-generated assets such as **CSS, JavaScript, fonts, and images**.  

#### **Configuration (In `settings.py`)**  
| Setting | Purpose | Example |  
|---------|---------|---------|  
| `STATIC_URL` | Base URL for serving static files | `STATIC_URL = '/static/'` |  
| `STATICFILES_DIRS` | List of directories for project-level static files | `STATICFILES_DIRS = [BASE_DIR / "static"]` |  
| `STATIC_ROOT` | Directory where `collectstatic` stores files for production | `STATIC_ROOT = BASE_DIR / "staticfiles"` |  

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

#### **Serving Static Files in Production**  
- **Using WhiteNoise** (Recommended when not using Nginx)  
  ```sh
  pip install whitenoise
  ```
  ```python
  MIDDLEWARE = [
      'django.middleware.security.SecurityMiddleware',
      'whitenoise.middleware.WhiteNoiseMiddleware',  # Enables serving static files
  ]

  STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
  ```

---

### **Media Files**  
#### **Purpose:**  
- Store **user-uploaded content**, such as profile pictures, documents, or videos.  

#### **Configuration (In `settings.py`)**  
| Setting | Purpose | Example |  
|---------|---------|---------|  
| `MEDIA_URL` | Base URL for serving media files | `MEDIA_URL = '/media/'` |  
| `MEDIA_ROOT` | Directory where uploaded files are stored | `MEDIA_ROOT = BASE_DIR / "media"` |  

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
    avatar = models.ImageField(upload_to='avatars/')  # Saves files in media/avatars/
```

#### **Displaying Media in Templates**  
```html
<img src="{{ profile.avatar.url }}" alt="Profile Image">
```

---

### **Static vs Media Files Comparison**  
| Feature        | Static Files                         | Media Files                        |  
|---------------|--------------------------------------|------------------------------------|  
| Purpose       | Stores assets (CSS, JS, images)     | Stores user-generated content     |  
| Location     | `static/` or `staticfiles/`         | `media/`                          |  
| Access via URL | `{% static 'path' %}` in templates  | `<img src="{{ object.field.url }}">` |  
| Required in Production | Needs `collectstatic` | Needs a media file handler (e.g., S3, Nginx) |  

---

### **Production Deployment Considerations**  
#### **Serving Static Files with Nginx**  
- **Nginx Configuration Snippet:**  
  ```nginx
  location /static/ {
      alias /path/to/staticfiles/;
  }
  ```

#### **Serving Media Files Using AWS S3**  
- **Use `django-storages` for cloud storage**  
  ```sh
  pip install django-storages[boto3]
  ```
  ```python
  DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
  ```

---

### **Debugging Static & Media Files Issues**  
| Issue | Possible Cause | Solution |  
|-------|---------------|----------|  
| Static files not loading | `{% load static %}` missing | Add `{% load static %}` in templates |  
| 404 for media files | Incorrect `MEDIA_URL` or `MEDIA_ROOT` | Ensure settings are configured correctly |  
| Static files not updated | Browser cache issue | Run `collectstatic` and clear cache |  
| Images not displayed in admin | `MEDIA_URL` missing in template | Use `<img src="{{ object.field.url }}">` |  
