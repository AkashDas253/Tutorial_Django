## **Sessions and Cookies in Django**

Django provides session management to store and retrieve arbitrary data on a per-site-visitor basis, using **cookies** to track users across requests. Cookies store session keys; actual data is stored on the server (e.g., DB, cache).

---

### **1. Session Framework**

Enabled by default in `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'django.contrib.sessions',
]

MIDDLEWARE = [
    ...
    'django.contrib.sessions.middleware.SessionMiddleware',
]
```

---

### **2. Session Configuration**

| Setting                           | Description            | Default                                 |
| --------------------------------- | ---------------------- | --------------------------------------- |
| `SESSION_ENGINE`                  | Backend engine         | `'django.contrib.sessions.backends.db'` |
| `SESSION_COOKIE_NAME`             | Name of session cookie | `'sessionid'`                           |
| `SESSION_COOKIE_AGE`              | Expiry in seconds      | `1209600` (2 weeks)                     |
| `SESSION_EXPIRE_AT_BROWSER_CLOSE` | Clear on browser close | `False`                                 |
| `SESSION_SAVE_EVERY_REQUEST`      | Save every time        | `False`                                 |

---

### **3. Session Storage Backends**

| Backend   | Setting                                             |
| --------- | --------------------------------------------------- |
| Database  | `'django.contrib.sessions.backends.db'`             |
| Cached DB | `'django.contrib.sessions.backends.cached_db'`      |
| Cache     | `'django.contrib.sessions.backends.cache'`          |
| File      | `'django.contrib.sessions.backends.file'`           |
| In-memory | `'django.contrib.sessions.backends.signed_cookies'` |

---

### **4. Using Sessions in Views**

#### Set Session Data:

```python
request.session['user_id'] = 42
request.session['theme'] = 'dark'
```

#### Get Session Data:

```python
user_id = request.session.get('user_id', default=None)
```

#### Delete Session Key:

```python
del request.session['theme']
```

#### Clear Entire Session:

```python
request.session.flush()  # Deletes the session and cookie
```

---

### **5. Session Lifecycle**

| Action      | Method                             |
| ----------- | ---------------------------------- |
| New session | `request.session.create()`         |
| End session | `request.session.flush()`          |
| Modify data | Direct dictionary-like access      |
| Persist     | Session auto-saved on modification |

---

### **6. Cookie Basics**

Cookies are small data stored on the client side and sent with every request to the same domain.

---

### **7. Setting Cookies in Django**

```python
def set_cookie_view(response):
    response.set_cookie('name', 'John', max_age=3600)
    return response
```

| Parameter                                          | Description           |
| -------------------------------------------------- | --------------------- |
| `key`                                              | Cookie name           |
| `value`                                            | Cookie value          |
| `max_age`                                          | In seconds            |
| `expires`                                          | Absolute expiry       |
| `path`, `domain`, `secure`, `httponly`, `samesite` | Additional attributes |

---

### **8. Reading Cookies**

```python
name = request.COOKIES.get('name', 'Guest')
```

---

### **9. Deleting Cookies**

```python
response.delete_cookie('name')
```

---

### **10. Session vs Cookie**

| Feature         | Session           | Cookie            |
| --------------- | ----------------- | ----------------- |
| Stored          | Server            | Client (browser)  |
| Size Limit      | Large             | \~4KB             |
| Security        | More secure       | Less secure       |
| Access          | `request.session` | `request.COOKIES` |
| Auto expiration | Yes               | Manual setup      |

---

### **11. Security Considerations**

* Use `SESSION_COOKIE_SECURE = True` to allow cookies only over HTTPS.
* Use `SESSION_COOKIE_HTTPONLY = True` to prevent JavaScript access.
* Use `SESSION_COOKIE_SAMESITE = 'Lax'` or `'Strict'` to prevent CSRF.
* Always sign cookies to prevent tampering.

---
