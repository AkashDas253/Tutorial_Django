## **Django Settings**

The `settings.py` file in Django contains **all configuration** for your project. It is auto-generated when you create a project using `startproject`.

---

### **1. Project Structure & Location**

File path:

```
projectname/projectname/settings.py
```

Accessible via:

```python
from django.conf import settings
```

---

### **2. Core Settings**

| Setting         | Description                                                                                |
| --------------- | ------------------------------------------------------------------------------------------ |
| `BASE_DIR`      | Root directory of the project, usually defined as `Path(__file__).resolve().parent.parent` |
| `SECRET_KEY`    | Secret used for cryptographic signing (keep it confidential)                               |
| `DEBUG`         | Boolean – `True` for development, `False` for production                                   |
| `ALLOWED_HOSTS` | List of domains/IPs allowed to serve the app                                               |

---

### **3. Application Configuration**

```python
INSTALLED_APPS = [
    'django.contrib.admin',         # Admin interface
    'django.contrib.auth',          # Authentication system
    'django.contrib.contenttypes',  # Content type framework
    'django.contrib.sessions',      # Session framework
    'django.contrib.messages',      # Messaging framework
    'django.contrib.staticfiles',   # Serving static files
    'myapp',                        # Your custom app(s)
]
```

---

### **4. Middleware**

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

Middleware processes requests/responses globally.

---

### **5. URL Configuration**

```python
ROOT_URLCONF = 'projectname.urls'
```

Points to the main URL dispatcher file.

---

### **6. Templates**

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],   # Custom template directory
        'APP_DIRS': True,                   # Auto-discover templates in apps
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

---

### **7. WSGI and ASGI**

```python
WSGI_APPLICATION = 'projectname.wsgi.application'
ASGI_APPLICATION = 'projectname.asgi.application'
```

Used for deployment; WSGI for synchronous, ASGI for async.

---

### **8. Database Configuration**

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',  # Default DB
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Change for PostgreSQL, MySQL, etc.

---

### **9. Authentication and Password Validators**

```python
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator'},
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator'},
    {'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator'},
    {'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator'},
]
```

Custom validators can be added as needed.

---

### **10. Localization**

```python
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'
USE_I18N = True     # Internationalization
USE_L10N = True     # Localization formatting
USE_TZ = True       # Time zone aware datetimes
```

---

### **11. Static Files**

```python
STATIC_URL = '/static/'                    # URL prefix
STATICFILES_DIRS = [BASE_DIR / "static"]   # Custom dirs
STATIC_ROOT = BASE_DIR / "staticfiles"     # Collected for production
```

---

### **12. Media Files**

```python
MEDIA_URL = '/media/'                   # URL prefix for media
MEDIA_ROOT = BASE_DIR / 'media'        # Directory for uploaded files
```

---

### **13. Email Configuration (Example)**

```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your_email@gmail.com'
EMAIL_HOST_PASSWORD = 'your_password'
```

---

### **14. Logging (Optional)**

```python
LOGGING = {
    'version': 1,
    'handlers': {
        'console': {'class': 'logging.StreamHandler'},
    },
    'root': {
        'handlers': ['console'],
        'level': 'WARNING',
    },
}
```

---

### **15. Custom User Model (if any)**

```python
AUTH_USER_MODEL = 'yourapp.CustomUser'
```

---

### **16. CSRF and Security Settings**

| Setting                       | Purpose                       |
| ----------------------------- | ----------------------------- |
| `CSRF_COOKIE_SECURE`          | Use HTTPS for CSRF cookie     |
| `SESSION_COOKIE_SECURE`       | Use HTTPS for session cookie  |
| `SECURE_BROWSER_XSS_FILTER`   | Enable XSS protection         |
| `SECURE_CONTENT_TYPE_NOSNIFF` | Prevent content type sniffing |
| `X_FRAME_OPTIONS = 'DENY'`    | Prevent clickjacking          |

---
