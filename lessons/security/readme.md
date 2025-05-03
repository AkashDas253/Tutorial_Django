## **Security in Django**

Django provides built-in tools to guard against common security threats. Proper configuration and best practices are essential for securing production deployments.

---

### **1. Common Security Threats Django Handles**

| Threat                     | Protection Mechanism                |
| -------------------------- | ----------------------------------- |
| SQL Injection              | ORM automatically escapes queries   |
| Cross-Site Scripting (XSS) | Template auto-escaping              |
| Cross-Site Request Forgery | CSRF tokens in forms                |
| Clickjacking               | Middleware to set `X-Frame-Options` |
| Session Hijacking          | Secure cookies, HTTPS               |

---

### **2. Important Settings in `settings.py`**

| Setting                              | Description                                         |
| ------------------------------------ | --------------------------------------------------- |
| `DEBUG = False`                      | Never keep True in production                       |
| `ALLOWED_HOSTS = ['yourdomain.com']` | Whitelisted hostnames                               |
| `SECRET_KEY`                         | Must be kept secret; load via environment variables |
| `SECURE_SSL_REDIRECT = True`         | Redirect all HTTP requests to HTTPS                 |
| `SESSION_COOKIE_SECURE = True`       | Send cookies only over HTTPS                        |
| `CSRF_COOKIE_SECURE = True`          | Send CSRF cookie only over HTTPS                    |
| `SECURE_HSTS_SECONDS`                | Enables HTTP Strict Transport Security              |
| `SECURE_BROWSER_XSS_FILTER = True`   | Enables browser XSS protection                      |
| `X_FRAME_OPTIONS = 'DENY'`           | Prevent clickjacking by disallowing iframes         |

---

### **3. CSRF Protection**

* Enabled by default using `CsrfViewMiddleware`.
* Forms must include `{% csrf_token %}`.
* AJAX requires CSRF token in headers.

---

### **4. Session and Cookie Security**

* Set `SESSION_COOKIE_SECURE`, `CSRF_COOKIE_SECURE`, and `SESSION_EXPIRE_AT_BROWSER_CLOSE`.
* Use `django.contrib.sessions.middleware.SessionMiddleware`.

---

### **5. Authentication Security**

* Use `Argon2PasswordHasher` for strong password hashing:

  ```python
  PASSWORD_HASHERS = ['django.contrib.auth.hashers.Argon2PasswordHasher']
  ```
* Enforce strong password policies:

  ```python
  AUTH_PASSWORD_VALIDATORS = [...]
  ```

---

### **6. Preventing Clickjacking**

```python
# settings.py
X_FRAME_OPTIONS = 'DENY'
```

---

### **7. Content Security Policy (CSP)**

Use a third-party package like `django-csp` to control allowed sources:

```bash
pip install django-csp
```

```python
MIDDLEWARE += ['csp.middleware.CSPMiddleware']
CSP_DEFAULT_SRC = ("'self'",)
```

---

### **8. Limiting Request Size and Rate**

* Set `DATA_UPLOAD_MAX_MEMORY_SIZE`.
* Use Django REST Framework’s throttling for APIs.
* Use a reverse proxy like Nginx with rate limiting.

---

### **9. Security Packages and Tools**

| Tool          | Purpose                       |
| ------------- | ----------------------------- |
| `django-axes` | Prevent brute-force logins    |
| `django-csp`  | Implement CSP headers         |
| `whitenoise`  | Serve static files securely   |
| `sentry`      | Monitor errors and exceptions |

---

### **10. Best Practices**

* Store secrets in environment variables.
* Use HTTPS with valid SSL certificates.
* Run regular vulnerability scans.
* Keep Django and all dependencies updated.
* Limit admin access (`/admin`) to trusted IPs or add 2FA.

---
