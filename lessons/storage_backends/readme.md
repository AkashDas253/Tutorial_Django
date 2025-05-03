## **Storage Backends**

Django's **storage backends** define how and where files (media/static) are stored and accessed. The default backend stores files locally using `FileSystemStorage`, but you can integrate cloud storage (e.g., AWS S3, Google Cloud Storage, Azure) or use custom ones.

---

### **1. Built-in Storage Classes**

| Storage Class                | Description                                             |
| ---------------------------- | ------------------------------------------------------- |
| `FileSystemStorage`          | Default; saves files to the local filesystem            |
| `StaticFilesStorage`         | Used for collecting and serving static files            |
| `ManifestStaticFilesStorage` | Adds hashed filenames to static files for cache busting |

---

### **2. Key Settings**

| Setting                | Purpose                                            |
| ---------------------- | -------------------------------------------------- |
| `MEDIA_URL`            | Base URL for media files (`/media/`)               |
| `MEDIA_ROOT`           | Absolute path to store media files                 |
| `STATIC_URL`           | Base URL for static files (`/static/`)             |
| `STATIC_ROOT`          | Directory to store static files on `collectstatic` |
| `DEFAULT_FILE_STORAGE` | Path to the default storage class                  |
| `STATICFILES_STORAGE`  | Custom static files storage backend                |

---

### **3. Local Storage Example**

```python
MEDIA_URL = "/media/"
MEDIA_ROOT = BASE_DIR / "media"

STATIC_URL = "/static/"
STATIC_ROOT = BASE_DIR / "staticfiles"
```

Use in models:

```python
from django.db import models

class MyModel(models.Model):
    image = models.ImageField(upload_to="images/")
```

---

### **4. Cloud Storage Backends**

Install required packages and configure the following depending on the backend:

#### **a. AWS S3 (using `django-storages`)**

```bash
pip install django-storages boto3
```

```python
INSTALLED_APPS += ["storages"]

DEFAULT_FILE_STORAGE = "storages.backends.s3boto3.S3Boto3Storage"
AWS_ACCESS_KEY_ID = "your-key"
AWS_SECRET_ACCESS_KEY = "your-secret"
AWS_STORAGE_BUCKET_NAME = "your-bucket"
```

#### **b. Google Cloud Storage**

```bash
pip install django-storages google-cloud-storage
```

```python
DEFAULT_FILE_STORAGE = "storages.backends.gcloud.GoogleCloudStorage"
GS_BUCKET_NAME = "your-bucket"
```

#### **c. Azure Blob Storage**

```bash
pip install django-storages azure-storage-blob
```

```python
DEFAULT_FILE_STORAGE = "storages.backends.azure_storage.AzureStorage"
AZURE_ACCOUNT_NAME = "your-account"
AZURE_ACCOUNT_KEY = "your-key"
AZURE_CONTAINER = "your-container"
```

---

### **5. Custom Storage Backend**

Create a custom storage by subclassing `Storage`:

```python
from django.core.files.storage import Storage

class CustomStorage(Storage):
    def _open(self, name, mode='rb'):
        # custom open logic
        pass

    def _save(self, name, content):
        # custom save logic
        pass

    def exists(self, name):
        # return True/False
        pass
```

Set in settings:

```python
DEFAULT_FILE_STORAGE = "myapp.storage_backends.CustomStorage"
```

---

### **6. Useful Methods in Storage Classes**

| Method                 | Purpose               |
| ---------------------- | --------------------- |
| `_save(name, content)` | Saves file            |
| `_open(name, mode)`    | Opens file            |
| `exists(name)`         | Checks if file exists |
| `url(name)`            | Returns public URL    |
| `size(name)`           | Returns size of file  |
| `delete(name)`         | Deletes file          |

---

### **7. Deployment Tips**

* For cloud backends, ensure files are publicly accessible if needed.
* Use environment variables to keep credentials safe.
* Use separate storage backends for static and media files.

---
