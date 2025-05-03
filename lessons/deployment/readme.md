## **Deployment in Django**

Deployment is the process of making a Django application publicly accessible via a web server in a production environment. This involves setting up the web server, application server, database, static/media file handling, and security configurations.

---

### **1. Key Components in Deployment**

| Component          | Purpose                                               |
| ------------------ | ----------------------------------------------------- |
| Web Server         | Handles HTTP requests (e.g., Nginx)                   |
| WSGI Server        | Bridges web server and Django (e.g., Gunicorn, uWSGI) |
| Application Code   | Django project and apps                               |
| Database           | Production-grade DB (e.g., PostgreSQL)                |
| Static/Media Files | Handled via Nginx or cloud storage                    |
| Environment Vars   | Store secrets/config outside the codebase             |

---

### **2. Common Hosting Options**

| Platform       | Type           |
| -------------- | -------------- |
| Heroku         | PaaS           |
| PythonAnywhere | Shared Hosting |
| AWS EC2        | IaaS           |
| Railway.app    | Cloud PaaS     |
| DigitalOcean   | VPS            |

---

### **3. Production Settings**

In `settings.py`:

```python
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']

# Use environment variables for secrets
import os
SECRET_KEY = os.environ['DJANGO_SECRET_KEY']
```

---

### **4. Static and Media Files**

```python
# settings.py
STATIC_ROOT = BASE_DIR / 'staticfiles'
MEDIA_ROOT = BASE_DIR / 'media'
```

Commands:

```bash
python manage.py collectstatic
```

Nginx serves static/media from these directories in production.

---

### **5. WSGI Server Setup (Gunicorn Example)**

Install:

```bash
pip install gunicorn
```

Run:

```bash
gunicorn yourproject.wsgi:application
```

Can be run via systemd or supervisor for background execution.

---

### **6. Web Server Setup (Nginx Example)**

Sample Nginx config:

```nginx
server {
    server_name yourdomain.com;

    location /static/ {
        alias /path/to/staticfiles/;
    }

    location /media/ {
        alias /path/to/media/;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

---

### **7. Database Configuration**

Use PostgreSQL for production:

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ['DB_NAME'],
        ...
    }
}
```

---

### **8. HTTPS & Security**

* Use `SECURE_SSL_REDIRECT = True`
* Set `SESSION_COOKIE_SECURE = True` and `CSRF_COOKIE_SECURE = True`
* Use HTTPS via Let's Encrypt and Certbot

---

### **9. Logging and Error Reporting**

Configure logging:

```python
LOGGING = {
    'version': 1,
    ...
}
```

Use monitoring tools (e.g., Sentry) for error tracking.

---

### **10. Common Tools and Practices**

| Tool         | Purpose               |
| ------------ | --------------------- |
| `gunicorn`   | WSGI server           |
| `nginx`      | Web server            |
| `supervisor` | Process control       |
| `env files`  | Manage secrets        |
| `docker`     | Containerization      |
| `certbot`    | Free SSL certificates |

---
