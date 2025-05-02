## **Django Internal Modules and Submodules**

### **1. `django.conf`**

* Handles project configurations.
* **Submodules:**

  * `settings` – Configuration settings manager.
  * `global_settings` – Default settings.
  * `urls` – URL configuration handling.

---

### **2. `django.core`**

* Core framework utilities and bootstrapping logic.
* **Submodules:**

  * `handlers` – WSGI and ASGI request handling.
  * `management` – Command-line tools (`manage.py`).
  * `mail` – Email backend.
  * `serializers` – Data serializers (e.g., XML).
  * `validators` – Built-in validation logic.
  * `exceptions` – Core exceptions.
  * `cache` – Backend cache interfaces.

---

### **3. `django.db`**

* ORM and database abstraction.
* **Submodules:**

  * `models` – Field types, model base, managers.
  * `backends` – Database-specific backends (SQLite, PostgreSQL, etc.).
  * `migrations` – Schema and data migrations.
  * `transaction` – Database transaction management.
  * `utils` – ORM helpers.
  * `connection` – Database connection handler.

---

### **4. `django.http`**

* Manages HTTP requests and responses.
* **Submodules:**

  * `request` – HttpRequest class.
  * `response` – HttpResponse, JsonResponse, etc.
  * `multipartparser` – File upload parsing.
  * `cookie` – Cookie parsing/management.

---

### **5. `django.urls`**

* Routing system for mapping URLs to views.
* **Submodules:**

  * `conf` – `include()` and `path()` functions.
  * `resolvers` – URL pattern resolution.
  * `exceptions` – URL resolver exceptions.

---

### **6. `django.template`**

* Template rendering engine.
* **Submodules:**

  * `loader` – Template loaders.
  * `context` – Context processing.
  * `defaultfilters` – Built-in filters.
  * `defaulttags` – Built-in template tags.
  * `engine` – Template engines interface.

---

### **7. `django.views`**

* Built-in view utilities.
* **Submodules:**

  * `generic` – Class-based views (CBVs).
  * `decorators` – View decorators (e.g., `@login_required`).
  * `static` – Serving static files.
  * `debug` – Debugging views and error reporting.

---

### **8. `django.middleware`**

* Standard middleware components.
* **Submodules:**

  * `security` – Security headers middleware.
  * `csrf` – CSRF protection.
  * `clickjacking` – X-Frame-Options header.
  * `common` – General HTTP middleware.
  * `locale` – Internationalization.
  * `sessions` – Session middleware.

---

### **9. `django.contrib`**

* Built-in pluggable apps.
* **Submodules (apps):**

  * `admin` – Django Admin interface.
  * `auth` – Authentication and permissions.
  * `sessions` – Session framework.
  * `messages` – Flash messaging system.
  * `staticfiles` – Static file management.
  * `contenttypes` – Generic model types.
  * `sites`, `sitemaps`, `redirects`, `flatpages` – Additional features.

---

### **10. `django.forms`**

* Form and field classes.
* **Submodules:**

  * `models` – `ModelForm`.
  * `fields`, `widgets`, `forms` – Form logic and rendering.

---

### **11. `django.dispatch`**

* Django’s internal signal framework.
* Used for loosely-coupled callbacks (e.g., `post_save`, `request_started`).

---

### **12. `django.test`**

* Testing tools built on Python `unittest`.
* **Submodules:**

  * `client` – Test client for HTTP requests.
  * `runner` – Test runner and setup tools.
  * `utils` – Assertions and testing helpers.

---

### **13. `django.utils`**

* Common internal utility functions.
* **Submodules:**

  * `timezone` – Date/time helpers.
  * `translation` – i18n support.
  * `text`, `dateparse`, `html`, `deprecation`, etc.

---

### **14. `django.apps`**

* App registry and configuration system.
* `AppConfig` class to configure apps.

---

### **15. `django.contrib.staticfiles`**

* Collects and serves static files.
* Uses a finder and storage mechanism.

---

### **16. `django.contrib.messages`**

* Framework for temporary messages (info, success, error).
* Middleware-based storage (cookie or session).

---
