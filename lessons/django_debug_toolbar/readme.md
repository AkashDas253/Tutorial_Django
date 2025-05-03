## **Django Debug Toolbar**

The **Django Debug Toolbar** is a powerful development tool that provides in-browser panels displaying debug information about requests, database queries, cache, signals, templates, settings, and more.

---

### **1. Installation**

```bash
pip install django-debug-toolbar
```

Add to `INSTALLED_APPS` in `settings.py`:

```python
INSTALLED_APPS = [
    ...
    "debug_toolbar",
]
```

Add to `MIDDLEWARE` **after** `django.middleware.common.CommonMiddleware`:

```python
MIDDLEWARE = [
    ...
    "debug_toolbar.middleware.DebugToolbarMiddleware",
]
```

Include the toolbar URLs in `urls.py` (only for development):

```python
from django.conf import settings
from django.conf.urls import include
from django.urls import path

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        path("__debug__/", include(debug_toolbar.urls)),
    ] + urlpatterns
```

---

### **2. Internal IPs**

Set allowed IPs in `settings.py`:

```python
INTERNAL_IPS = ["127.0.0.1"]
```

If using Docker:

```python
import socket

hostname, _, ips = socket.gethostbyname_ex(socket.gethostname())
INTERNAL_IPS += [ip[: ip.rfind(".")] + ".1" for ip in ips]
```

---

### **3. Available Panels**

| Panel        | Information Provided                 |
| ------------ | ------------------------------------ |
| Versions     | Django and installed packages        |
| Timer        | Time to process request              |
| Settings     | Current Django settings              |
| Headers      | Request and response headers         |
| Request      | GET, POST, COOKIES, and META data    |
| SQL          | All DB queries with time, stacktrace |
| Static files | Static file paths and config         |
| Templates    | Templates used, context variables    |
| Cache        | Cache operations and performance     |
| Signals      | All signals sent during request      |
| Logging      | Logging output during the request    |

---

### **4. Usage**

* Appears as a side panel in the browser when `DEBUG=True`
* Click each panel to explore in-depth info
* You can collapse or drag the toolbar

---

### **5. Customization**

Disable specific panels:

```python
DEBUG_TOOLBAR_PANELS = [
    'debug_toolbar.panels.timer.TimerPanel',
    'debug_toolbar.panels.sql.SQLPanel',
    # remove or comment out others
]
```

Enable stack traces for SQL queries:

```python
DEBUG_TOOLBAR_CONFIG = {
    "SHOW_TOOLBAR_CALLBACK": lambda request: True,
    "ENABLE_STACKTRACES": True,
}
```

---

### **6. Security Note**

**Never enable the debug toolbar in production.** It can expose sensitive data.

Ensure `DEBUG=False` in production, or use conditional imports as shown.

---
