
## **Django Static & Media Files Cheatsheet**  

### **Static Files**  
#### **Purpose:**  
- Store **CSS, JavaScript, images**, and other non-user-generated files.  

#### **Configuration (In `settings.py`)**  
```python
STATIC_URL = '/static/'  # URL to access static files
STATICFILES_DIRS = [BASE_DIR / "static"]  # Project-level static files
STATIC_ROOT = BASE_DIR / "staticfiles"  # Collects static files for production
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

#### **Serving Static Files in Production**  
- **WhiteNoise (For Serving Static Files Without a Web Server like Nginx)**  
  ```python
  # Install WhiteNoise
  pip install whitenoise
  
  # Add to middleware (in settings.py)
  MIDDLEWARE = [
      'django.middleware.security.SecurityMiddleware',
      'whitenoise.middleware.WhiteNoiseMiddleware',  # Add WhiteNoise
      # Other middleware...
  ]

  STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
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

---

### **Static vs Media Files Comparison**  

| Feature        | Static Files                         | Media Files                        |  
|---------------|--------------------------------------|------------------------------------|  
| Purpose       | Stores assets (CSS, JS, images)     | Stores user-generated content     |  
| Location     | `static/` or `staticfiles/`         | `media/`                          |  
| Access via URL | `{% static 'path' %}` in templates  | `<img src="{{ object.field.url }}">` |  
| Required in Production | Needs to be collected using `collectstatic` | Needs a media file handler (e.g., S3, Nginx) |  

---

### **Production Deployment Considerations**  
#### **Serving Static Files Using Nginx**  
- Configure Nginx to serve static files efficiently. Example snippet:  
  ```nginx
  location /static/ {
      alias /path/to/staticfiles/;
  }
  ```
  
#### **Serving Media Files Using AWS S3**  
- Use `django-storages` for cloud storage.  
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
| Static files not loading | Missing `{% load static %}` | Add `{% load static %}` at the top of your template |  
| 404 for media files | Media URL not properly configured | Ensure `MEDIA_URL` and `MEDIA_ROOT` are set correctly |  
| Static files not updated | Cached version served | Run `python manage.py collectstatic --noinput` |  
| Images not displayed in admin | `MEDIA_URL` not used properly | Use `<img src="{{ object.field.url }}">` in templates |  

---
